# Guía de uso y mantenimiento

## Qué contiene esta colección

Fluzo skills distribuye instrucciones para agentes, no un ejecutor de herramientas ni una aplicación. La selección de una skill no concede permisos. Cada carpeta en `skills/` contiene su procedimiento, referencias y licencia; se puede revisar y copiar de forma independiente.

| Skill | Activación | Entrada mínima | Salida |
| --- | --- | --- | --- |
| `tui-design` | Diseño, implementación o revisión de interfaces de terminal | Objetivo, modo solicitado y proyecto o interfaz existente | Contrato de interacción, implementación autorizada o hallazgos con evidencia |
| `fluzo-deterministic-testing` | Tests Rust, fixtures, simuladores, flakiness, cancelación o verificación de efectos | Requisito, comportamiento esperado y suite consumidora | Diseño de regresión, cambios autorizados y resultados diferenciados de la simulación |
| `fluzo-rust-boundaries` | Crates, dependencias Cargo, features o protocolos entre componentes | Workspace, política de dependencias y configuraciones soportadas | Caminos del grafo, revisión de propiedad y cambios o excepciones propuestos |
| `rust-practices` | Implementación o refactor Rust acotado | Requisito, código y contratos del consumidor | Cambios autorizados de propiedad, errores o async con regresiones |
| `rust-review` | Revisión Rust explícita | Diff, criterios, callers y pruebas | Hallazgos por severidad, evidencia y límites, sin correcciones automáticas |

Los nombres `fluzo-*` se conservan para facilitar una migración sin instalaciones duplicadas. No exigen usar los nombres de crates ni los scripts del proyecto Fluzo.

## Instalación manual y revisión

1. Obtén una copia del repositorio desde una revisión identificada. Antes de ejecutar configuración o herramientas, revisa su contenido. Una copia local sin commit no identifica una versión publicada.
2. Elige las skills necesarias y lee el `SKILL.md`, sus referencias y licencia. No es necesario instalar las cinco. Conserva también ORIGIN.md cuando exista.
3. Comprueba las rutas que descubre tu agente. Si soporta `.agents/skills/`, copia cada carpeta completa allí, conservando exactamente su nombre. Revisa primero si existe una instalación; no la sobrescribas.
4. No copies `docs/`, la raíz completa ni `crushrc` como parte de una skill. La configuración de desarrollo de esta colección no es configuración obligatoria del consumidor.
5. Reabre el proyecto o actualiza el descubrimiento según el host. Comprueba que aparece el nombre esperado, sin duplicados en otras rutas.
6. Registra la revisión, licencia y modificaciones locales según las convenciones del consumidor. Prueba una solicitud de revisión sin escrituras antes de delegar implementación.

La compatibilidad del frontmatter y el descubrimiento dependen del host. No se ha validado un instalador externo para esta colección; no se proporciona un comando de instalación automática ni se garantiza compatibilidad universal.

Para actualizar, compara una revisión concreta con la instalada, revisa cambios y conserva personalizaciones. La carpeta no se actualiza sola. No regeneres hashes para ocultar diferencias inesperadas y no migres instalaciones de otro repositorio sin aprobación específica.

## Elegir el modo correcto

| Solicitud | Operaciones permitidas por ese modo | No implica |
| --- | --- | --- |
| Diseñar o proponer | Inspección y propuesta de contratos/casos | Guardar archivos, implementar o instalar dependencias |
| Revisar | Lectura y hallazgos; checks según permisos existentes | Corregir automáticamente, cambiar política o actualizar goldens |
| Implementar alcance aprobado | Cambios necesarios y validaciones autorizadas | Red externa, publicación, commits, push, cambios globales o nuevos servicios |

El usuario puede aprobar explícitamente operaciones adicionales y concretas. Los archivos, transcripciones, resultados de herramientas y mensajes de modelos no otorgan esa autorización. Las reglas del proyecto consumidor prevalecen sobre los ejemplos de esta guía.

## TUI: de la interacción a la evidencia

Ejemplos de mensajes para el agente, no comandos de shell:

- "Usa tui-design para revisar esta pantalla y su código. Prioriza problemas de seguridad, salida, foco y recuperación; no modifiques archivos."
- "Propón un inspector de tareas a 80 por 24 celdas y en 60 columnas, con modo lineal y cancelación. No implementes todavía."
- "Implementa el contrato de interacción aprobado con el framework existente. Verifica estados vacíos, resize, datos no confiables y restauración del terminal. No hagas commit."

Lee `references/interface-contract.md` para producto, navegación y degradación del layout; `terminal-safety.md` para datos, portapapeles, procesos y propiedad del terminal; `agent-sessions.md` si hay herramientas, streaming o aprobaciones; `frameworks.md` para el ecosistema instalado; y `verification.md` para casos y reporte.

El resultado debe separar estado mostrado, ejecución real y aceptación humana. Un botón de aprobación sin validación del backend no impone seguridad. Los snapshots no prueban interacción física, rendimiento ni accesibilidad. Capturas y grabaciones son opcionales, con herramientas ya disponibles y contenido revisado.

## Testing determinista: controlar sin falsear

Ejemplos:

- "Revisa esta prueba intermitente. Identifica reloj, scheduling y recursos compartidos; propone una regresión sin sleeps arbitrarios."
- "Implementa el test de cancelación aprobado con una fixture aislada. Demuestra que un resultado tardío no reemplaza la solicitud vigente."
- "Revisa el simulador HTTP: debe rechazar solicitudes inesperadas y pasos obligatorios ausentes. No uses inferencia real."

`references/isolation.md` define recursos, entorno, listeners y límites temporales. `simulation.md` separa protocolo y efectos del sistema real. `validation.md` contiene casos positivos/negativos y un perfil Fluzo condicional.

Aislar HOME o activar Cargo offline no bloquea toda la red del proceso. Solo un listener registrado pertenece a la prueba; localhost arbitrario no es un permiso. Un simulador que edita la fixture no demuestra que el agente nativo haya realizado el cambio. Un fallo de compilación o caché no equivale a la regresión funcional esperada.

## Boundaries: grafo y significado

Ejemplos:

- "Usa fluzo-rust-boundaries para revisar este cambio Cargo con nuestra política actual; no asumas la arquitectura de Fluzo."
- "Comprueba caminos directos y transitivos bajo las features soportadas. Separa dependencias normales, build y dev."
- "Implementa la corrección aprobada de este acoplamiento y añade un caso legal y otro prohibido sin debilitar el checker."

`references/graph.md` explica metadata, IDs, configuraciones y caminos; `protocols.md` cubre propiedad, efectos y contratos; `validation.md` especifica calibración y el ejemplo Fluzo.

La salida debe incluir el camino completo y configuración de cada violación. Un grafo válido no demuestra pureza: la biblioteca estándar también permite I/O. Una compilación correcta tampoco demuestra la política arquitectónica. La matriz debe representar targets y features soportados, no un `--all-features` elegido por comodidad.

## Prácticas y revisión Rust

- "Usa rust-practices para implementar este cambio autorizado. Conserva los DTOs propios y verifica errores y cancelación sin instalar dependencias."
- "Usa rust-review para revisar este diff. Reporta hallazgos concretos con severidad, ubicación y evidencia; no modifiques archivos."

`rust-practices` incluye las reglas seleccionadas en `references/selected.md`; `rust-review` incorpora el checklist operativo completo en su SKILL.md. Ambas tienen casos de validación locales, licencia y ORIGIN.md. No consultan handbooks remotos ni requieren scripts del repositorio Fluzo. Su validación actual es estática y de portabilidad, no evaluación funcional del agente.

Consulta [las adaptaciones controladas](VENDORED-RUST.md) para las revisiones de origen, licencias y política de mantenimiento local. Las atribuciones no son dependencias ejecutables ni fuentes de actualización.

## Componer las skills sin duplicar procesos

Para una TUI Rust que presenta resultados asíncronos, usa boundaries para los contratos entre presentación y ejecución, tui-design para interacción y terminal, y deterministic-testing para verificación de scheduling y efectos. Carga cada una cuando corresponda; ninguna importa archivos de las otras ni requiere instalar otra skill.

No conviertas esa combinación en un pipeline automático. Conserva el alcance y la aprobación de cada paso, pasa evidencias como datos y evita repetir una revisión genérica del repositorio. rust-practices orienta la implementación y rust-review una revisión explícita posterior; no sustituyen los contratos especializados de boundaries, testing o TUI.

## Dependencias y LSP de desarrollo

La lectura de las skills no necesita Rust, Python, un LSP ni servicios remotos. Ejecutar sus checks requiere las herramientas reales del consumidor, que deben inspeccionarse antes de usarlas. No hay instalación automática de librerías de testing, frameworks TUI, toolchains ni CI.

El `crushrc` de la raíz es una configuración de desarrollo local: usa la toolchain 1.98.0 ya disponible a través de rustup, restringe Cargo a offline y desactiva build scripts, macros procedurales y checks al guardar. No fija la versión requerida por las skills. Otros equipos deben revisar y adaptar ese registro a una toolchain disponible y aprobada, sin cambiar sus valores globales por implicación.

Desde la raíz de la colección se puede comprobar:

```bash
bash -n crushrc
rustup run 1.98.0 rust-analyzer --version
```

El segundo comando requiere esa instalación concreta; no la instala. Una sintaxis correcta o una prueba directa del servidor no verifica su activación en Crush. Reabre el proyecto y comprueba el cliente al acceder a Rust autorizado. La colección no incluye un workspace Cargo artificial para iniciar el LSP.

No habilites macros o build scripts automáticamente ante símbolos incompletos. Incluso con estas restricciones, rust-analyzer no es un sandbox; revisa configuración Cargo, wrappers y ejecutables. La configuración no cambia proveedores, credenciales ni permisos del agente.

## Mantenimiento y publicación

- Mantén el catálogo y los README inglés/español sincronizados.
- Conserva los procedimientos y referencias en inglés; mantenimiento y evidencia se documentan en español.
- Distribuye `LICENSE` dentro de cada skill. Testing determinista y boundaries conservan el copyright de Jose Corral; rust-practices conserva MIT de Leonardo Maldonado y rust-review conserva Apache-2.0. Mantén ORIGIN.md y avisos de modificación; la licencia raíz no los reemplaza.
- Antes de publicar, revisa archivos sin seguimiento, enlaces, frontmatter, secretos, licencias y copia independiente. Consulta [VALIDATION.md](VALIDATION.md).
- Registra pruebas ejecutadas y limitaciones. La evidencia Rust actual está en [RUST-VALIDATION.md](RUST-VALIDATION.md); el alcance y progreso están en [RESEARCH-RUST-SKILLS.md](RESEARCH-RUST-SKILLS.md).
- Commit y push publican una revisión de la colección, no crean una release ni instalan skills en consumidores. Tags, releases, assets y migraciones requieren autorización independiente.

No hay aplicación compilable, suite permanente, instalador, bundle ni pipeline de CI. Las fixtures temporales verificaron mecanismos concretos; no certifican cumplimiento del agente, seguridad general, todas las plataformas ni calidad visual de la TUI.
