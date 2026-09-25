# Research y plan: testing determinista, límites Rust y LSP

## Alcance y estado

Solicitud: localizar las skills de testing determinista y boundaries, investigar su reutilización en esta colección, preparar un plan y añadir el LSP de Rust si el entorno lo permite.

La investigación, configuración local del LSP y adaptación de las dos skills están realizadas tras la solicitud explícita de implementar el plan. Se completaron P1-P4 en el alcance local: contratos, referencias, fixtures mecánicas y documentación. La revisión humana de los resultados queda pendiente. No se han modificado sus copias consumidoras, instalado dependencias, creado commits ni publicado archivos. Los cambios anteriores de `tui-design` se conservan.

## Fuentes identificadas

Las búsquedas iniciales en GitHub no identificaron una coincidencia concluyente. La inspección del proyecto Fluzo encontró las dos skills originales ya existentes:

| Skill | Ruta dentro del repositorio Fluzo | Alcance actual |
| --- | --- | --- |
| `fluzo-deterministic-testing` | `.agents/skills/fluzo-deterministic-testing/SKILL.md` | Aislamiento, simuladores estrictos, sincronización controlada y comprobación independiente de efectos |
| `fluzo-rust-boundaries` | `.agents/skills/fluzo-rust-boundaries/SKILL.md` | Grafo de crates, propiedad del estado, protocolo compartido y pruebas de dependencias prohibidas |

Fuente: repositorio `fluzo-labs/fluzo`, revisión `4a7dce9833e011eca557031edb44b2e6e74939fa`. Ambas skills coinciden con esa revisión sin modificaciones locales. Sus carpetas contienen únicamente `SKILL.md`: dependen de instrucciones y scripts del proyecto consumidor, y todavía no son unidades distribuibles completas.

Se revisaron también `SKILLS.md`, las secciones de arquitectura y testing de `AGENTS.md`, `scripts/check_dev_setup.py`, `scripts/check_lsp.py`, los manifiestos de Cargo y la toolchain. Algunos documentos y scripts consumidores tienen cambios locales previos; se usaron como contexto actual, no como evidencia atribuible íntegramente al commit citado, y se dejaron intactos.

La licencia de la fuente es MIT con copyright de Jose Corral. Si se trasladan o adaptan estos textos, cada carpeta distribuible debe conservar ese aviso. No basta con sustituirlo por la licencia general de la colección.

## Hallazgos: testing determinista

### Conservar

- Relacionar requisito, regresión y fallo esperado antes de implementar.
- Reglas puras en tests unitarios; archivos, SQLite y procesos reales pero desechables para sus límites.
- Simuladores HTTP que rechazan solicitudes inesperadas y pasos obligatorios ausentes.
- Aislar HOME, configuración, credenciales, proxies, puertos y repositorio. Localhost por sí solo no identifica un servicio seguro: el listener debe pertenecer a la prueba.
- Barreras y confirmaciones para carreras; reloj inyectable para tiempo de dominio y plazos reales acotados para operaciones del sistema.
- Verificación independiente: ni un texto de éxito ni un simulador que edita la fixture prueban la corrección del agente real.
- Sin inferencia real ni fallback a servicios externos. No confundir suites futuras con validación existente.

### Cambiar al distribuir

Los comandos Python y nombres de configuración actuales pertenecen a Fluzo y no pueden suponerse en cualquier consumidor. Descubrir comandos existentes; mantener los de Fluzo como perfil de ejemplo, no requisitos universales. El tratamiento de 429, reservas inciertas y reintentos debe conservar su contexto de política, no convertirse en una regla accidental para todo cliente HTTP.

Separar reproducibilidad de datos, aislamiento de recursos y control del scheduling. Una semilla o una prueba repetida no demuestra ausencia de carreras. Los tests paralelos necesitan fixtures y puertos propios; no modificar el entorno global del proceso de prueba sin una estrategia explícita. Añadir límites para tareas bloqueadas, limpieza tras fallo y conservación de evidencia de intentos fallidos.

## Hallazgos: boundaries Rust

### Conservar

- Grafo de producción explícito y análisis de caminos transitivos, no solo imports directos.
- Propiedad del estado y contratos de aplicación independientes del framework visual y de adaptadores concretos.
- Valores propios y versionados en el protocolo; distinguir acuse de recibo de finalización.
- Separar evidencia mecánica del grafo de revisión semántica: la biblioteca estándar también permite I/O sin añadir una dependencia externa.
- Casos negativos junto a un camino legal, para que el verificador no rechace todo indiscriminadamente.

### Cambiar al distribuir

El grafo `runtime -> core`, `tui -> core`, `cli -> core/runtime/tui` es una decisión de Fluzo, no una arquitectura obligatoria. Descubrir las reglas del consumidor y documentar excepciones aprobadas. No crear crates ni introducir una arquitectura nueva por activar la skill.

El checker actual rechaza todas las features y dependencias externas de producción deliberadamente durante el bootstrap. No copiar esa restricción como política general. Revisar dependencias normales, de build y de desarrollo por separado, targets y combinaciones de features soportadas. `cargo metadata` para una configuración no prueba todas las configuraciones; `--all-features` tampoco sustituye una matriz compatible.

`--offline` necesita dependencias disponibles y `--locked` necesita un lockfile válido. Un fallo por caché incompleta debe declararse bloqueado, no solucionarse descargando dependencias automáticamente ni cambiando el lockfile. La skill debe revisar configuración Cargo y código ejecutable antes de lanzar herramientas.

## LSP: realizado y límites

`rust-analyzer` existe como proxy en PATH, pero no funciona con la toolchain global `stable` porque allí falta el componente. La toolchain ya instalada `1.98.0` sí contiene rust-analyzer, rust-src, Clippy y rustfmt. No se descargó nada ni se cambió la toolchain por defecto.

Se añadió `crushrc` únicamente a esta colección. Registra rust-analyzer mediante `rustup run 1.98.0`, para archivos Rust y raíz Cargo. Fuerza Cargo offline y desactiva build scripts, macros procedurales y checks al guardar. Es una elección local basada en la instalación disponible, no un requisito de versión de las skills distribuibles.

Verificaciones ejecutadas:

- `rustup run 1.98.0 rust-analyzer --version`: servidor disponible.
- `bash -n crushrc`: sintaxis correcta.
- Evaluación con un sustituto local de `lsp`: argumentos y opciones preservados, sin lanzar el servidor desde la configuración.
- `rust-analyzer --print-config-schema`: las tres opciones de inicialización existen en el servidor instalado.
- `crush dirs --cwd` apuntando a esta colección: directorio del proyecto reconocido. No demuestra por sí solo registro o arranque del LSP en la sesión.
- Fixture Cargo temporal, sin dependencias externas, con HOME y CARGO_HOME aislados: initialize, definición, referencias, símbolos y shutdown LSP correctos usando las opciones configuradas.

No se probaron diagnósticos de compilador al guardar, que están desactivados, ni renombrado mediante herramientas de Crush. La sesión actual sigue abierta en otra colección: reabrir Crush en `fluzo-skills` y acceder a una fixture Rust revisada permite comprobar su activación real. No se añadió una aplicación Cargo artificial al repositorio de instrucciones.

Los límites del servidor están documentados en la [guía oficial de seguridad de rust-analyzer](https://rust-analyzer.github.io/book/security.html), consultada durante esta investigación. Desactivar macros, build scripts y checks reduce ejecución, pero no convierte el LSP en un sandbox: la configuración Cargo, toolchains y ejecutables del consumidor siguen siendo sensibles. No activar automáticamente estas capacidades para corregir análisis incompleto.

## Plan implementado localmente

| Fase | Trabajo | Resultado y aceptación |
| --- | --- | --- |
| P1: extraer contratos | Adaptar ambas skills a `skills/fluzo-deterministic-testing/` y `skills/fluzo-rust-boundaries/`, conservando nombres para evitar duplicados al migrar. Añadir licencia de origen y referencias locales; separar reglas generales del perfil Fluzo. | Cada carpeta funciona de forma independiente, sin rutas personales, imports de otras skills ni scripts obligatorios del consumidor original. |
| P2: especificar verificación | Para testing: aislamiento, simulador estricto, scheduling, errores y evidencia. Para boundaries: grafo resuelto, features/targets, contratos y revisión semántica. Definir modos propuesta, revisión e implementación con autorización diferenciada. | Casos positivos y negativos con entradas y resultados observables. Ninguna herramienta nueva ni matriz de targets inventada por defecto. |
| P3: calibrar en fixtures | Usar proyectos Rust temporales sin credenciales ni remotos. Ejercitar caminos de dependencia legales/prohibidos, una arista condicional y una fixture determinista con solicitud inesperada, cancelación y resultado tardío. | El caso negativo falla por la razón prevista y el positivo pasa; evidencia con versión, comandos, revisión y limitaciones. No declarar mejoras del agente sin evaluación específica. |
| P4: documentar y distribuir | Actualizar README bilingüe y guía de validación. Comprobar enlaces, frontmatter, licencia, copia independiente y ausencia de secretos. Proponer migración del consumidor solo después de disponer de una revisión publicada. | Tres skills catalogadas incluyendo `tui-design`; no duplicar instrucciones instaladas ni romper los checks actuales de Fluzo. Commit, push, instalación consumidora y release requieren sus autorizaciones. |

La solicitud posterior de implementar el plan autorizó estos cambios locales; no autorizó publicación ni migración del consumidor. El LSP es configuración de desarrollo local, no una tercera skill ni una dependencia para leer las otras dos.

| Fase | Implementación | Verificación | Revisión humana |
| --- | --- | --- | --- |
| P1 | Completada: dos carpetas autocontenidas y licencia original conservada | Frontmatter y enlaces locales correctos | Pendiente |
| P2 | Completada: aislamiento, simulación, grafo, protocolos y perfiles condicionales | Revisión de contratos y casos positivos/negativos | Pendiente |
| P3 | Completada: fixtures temporales sin dependencias externas | Grafo Cargo y seis tests Rust; fallos intencionados detectados | Pendiente |
| P4 | Completada: catálogo bilingüe, guía y evidencia | Comprobaciones documentales y portabilidad | Pendiente |

Los resultados exactos y límites constan en [RUST-VALIDATION.md](RUST-VALIDATION.md). No se declara aceptación global ni evaluación del agente. La distribución remota y actualización de Fluzo quedan fuera de esta implementación.

## Decisión recomendada

Promover las dos skills originales de Fluzo, no buscar sustitutos externos ni copiar un catálogo completo. Mantener nombres estables y estrechar el contrato de activación para que complementen a `tui-design`: boundaries se ocupa de acoplamiento y propiedad; deterministic-testing de reproducibilidad y evidencia; tui-design de interacción y terminal. Ninguna debe repetir el procedimiento entero de las otras.
