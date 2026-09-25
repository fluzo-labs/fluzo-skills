# Skills de Fluzo

[English](README.md) | Español

Instrucciones reutilizables mantenidas por Fluzo, con procedimientos originales y orientación Rust adaptada selectivamente. Las skills orientan a un agente; no son una aplicación, un entorno de ejecución aislado ni un flujo desatendido.

## Catálogo

| Skill | Para qué sirve |
| --- | --- |
| [tui-design](skills/tui-design/SKILL.md) | Diseñar, implementar y revisar interfaces de terminal, incluidas sesiones de agentes, aprobaciones seguras, resultados en streaming, accesibilidad y automatización |
| [fluzo-deterministic-testing](skills/fluzo-deterministic-testing/SKILL.md) | Fixtures Rust aisladas, simuladores estrictos, scheduling controlado y evidencia independiente de regresiones |
| [fluzo-rust-boundaries](skills/fluzo-rust-boundaries/SKILL.md) | Caminos del grafo Cargo resuelto, configuraciones de features, propiedad del estado y protocolos de aplicación |
| [rust-practices](skills/rust-practices/SKILL.md) | Implementación Rust acotada: propiedad, errores, async limitado y optimización con evidencia |
| [rust-review](skills/rust-review/SKILL.md) | Revisión Rust explícita con hallazgos concretos, severidad y checks del consumidor |

Las cinco skills se distribuyen como instrucciones; una release de fuentes no es una aplicación compilada ni una garantía de comportamiento. Los procedimientos de testing determinista y boundaries se comprobaron con fixtures Cargo temporales positivas y negativas; no es una evaluación del comportamiento del agente ni un E2E nativo de Fluzo. La validación de TUI, rust-practices y rust-review sigue siendo únicamente estática. No se proporciona instalador, bundle propio ni pipeline de CI.

## Usar una copia local

Copia la carpeta completa `skills/tui-design/` desde una revisión comprobada al directorio de skills admitido por tu agente, por ejemplo `.agents/skills/tui-design/` cuando ese host lo soporte. Conserva `LICENSE` y `references/`, comprueba que no exista una instalación antes de copiar y confirma que el agente descubre la skill. No copies la raíz del repositorio como si fuera una skill.

Para cualquier otra skill, utiliza el mismo procedimiento de carpeta completa con su nombre del catálogo. Conserva su licencia individual y su ORIGIN.md cuando exista. Testing determinista y boundaries conservan el aviso MIT de Jose Corral; rust-practices conserva la licencia MIT de Leonardo Maldonado; rust-review conserva Apache-2.0. No sobrescribas una instalación consumidora ni la migres hasta disponer de una revisión publicada y revisada.

El descubrimiento y los campos opcionales del frontmatter dependen del host. Las copias no se actualizan solas. Revisa explícitamente los cambios posteriores; esta colección no descarga instrucciones ni modifica permisos durante la ejecución.

Ejemplos de mensajes para tu agente:

- "Usa tui-design para revisar nuestra interfaz de terminal. Presenta hallazgos concretos sin modificar archivos."
- "Usa tui-design para proponer un inspector de tareas que funcione a 80 por 24 celdas y en un terminal estrecho. No implementes todavía."
- "Implementa la interacción de terminal aprobada con el framework existente. Comprueba cancelación, salida no confiable y modo no interactivo. No hagas commit ni publiques."

- "Usa fluzo-deterministic-testing para revisar los tests de cancelación y proponer una fixture offline estricta. No modifiques archivos."
- "Usa fluzo-rust-boundaries para inspeccionar caminos de dependencias permitidos y configuraciones de features soportadas. Separa evidencia del grafo y hallazgos de propiedad."

## Alcance y dependencias

La skill utiliza el framework y las herramientas existentes del consumidor. Incluye puntos de integración para Go, Rust, Python y TypeScript, sin exigir los cuatro ecosistemas ni descargar ejemplos específicos de una versión. Las afirmaciones sobre APIs deben comprobarse frente a la versión instalada.

Las referencias cubren contratos de interacción, seguridad del terminal, sesiones de agentes, integración con frameworks y verificación. Permanecen dentro de la carpeta de la skill; no se requiere otra skill. Las capturas y grabaciones son opcionales y requieren herramientas existentes autorizadas y contenido revisado.

Las skills Rust descubren la arquitectura y los comandos de prueba del consumidor. Sus perfiles Fluzo son ejemplos condicionales, no nombres de crates ni scripts Python obligatorios. No instalan dependencias ni acceden a servicios de inferencia reales. El `crushrc` local utiliza rust-analyzer 1.98.0 ya instalado con Cargo offline, build scripts y macros procedurales desactivados y sin checks de compilador al guardar; no es un requisito para usar las skills ni un sandbox. Falta comprobar su activación en Crush al reabrir este proyecto.

## Mantenimiento

Lee la [guía completa de uso](docs/USAGE.md) para instalación, entradas/salidas, ejemplos de las cinco skills, límites de aprobación, composición, LSP local y publicación.

Consulta [AGENTS.md](AGENTS.md) para las reglas de autoría y la [guía de validación](docs/VALIDATION.md) para las comprobaciones y sus límites. La documentación de mantenimiento está en español; las instrucciones y referencias de las skills están en inglés. Los resultados siguen el idioma del consumidor o, si no está definido, el del usuario.

Consulta el [research y plan de implementación Rust](docs/RESEARCH-RUST-SKILLS.md) y la [evidencia de fixtures](docs/RUST-VALIDATION.md) para el alcance, los resultados y las limitaciones pendientes.

## Licencia

El material original de la colección utiliza [MIT](LICENSE). El contenido adaptado conserva su licencia aplicable: `rust-practices` es MIT y `rust-review` es Apache-2.0. Conserva LICENSE y procedencia de cada carpeta; la licencia raíz no relicencia material de terceros. Consulta [las adaptaciones locales controladas](docs/VENDORED-RUST.md) para revisiones fijadas y política de actualización.
