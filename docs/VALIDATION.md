# Validación de la colección

## Comprobaciones de contenido

Desde la raíz, revisa `git diff --check` y `git status --short`. Los archivos sin seguimiento requieren lectura y comprobación explícitas, porque no aparecen en el diff ordinario.

Para cada skill:

1. Comprueba frontmatter, descripción de activación y coincidencia entre nombre y carpeta.
2. Verifica UTF-8, LF, salto de línea final y bloques Markdown equilibrados.
3. Resuelve los enlaces locales desde cada documento y confirma que las referencias permanecen dentro de la carpeta distribuible.
4. Revisa ausencia de secretos, instrucciones de descarga automática y textos orientativos sin sustituir.
5. Comprueba la inclusión de `LICENSE` y que su contenido coincida con la licencia aplicable.
6. Copia la carpeta completa a un directorio temporal ajeno al repositorio. Compara los bytes y vuelve a resolver sus enlaces sin depender de la colección original.
7. Comprueba la correspondencia de los README en inglés y español.

Estas son comprobaciones documentales, no una suite funcional. No se añaden dependencias de aplicación para ejecutarlas.

## Validación representativa pendiente

Usa el inspector de tareas con datos sintéticos descrito en `skills/tui-design/references/verification.md`, en un proyecto consumidor temporal autorizado. Registra framework y versión, revisión de la skill, implementación, comandos ejecutados, resultados observados y limitaciones.

Como mínimo, cubre revisión sin escrituras, actualización sin pérdida de foco, terminal estrecho, salida no confiable, cancelación, modo no interactivo y restauración del terminal. Para sesiones de agentes, usa un ejecutor simulado para comprobar aprobación obsoleta y eventos duplicados. No uses credenciales reales ni mutaciones de producción.

Una revisión de texto no sustituye esas ejecuciones. Hasta realizarlas, conserva la declaración de validación únicamente estática de `tui-design` en los README. No se ha probado la instalación mediante un gestor externo ni la compatibilidad funcional con todos los agentes.

## Skills Rust

`fluzo-deterministic-testing` y `fluzo-rust-boundaries` incluyen sus propias referencias de validación y perfiles Fluzo condicionales. Se ejercitaron fixtures Rust temporales positivas y negativas; consulta [la evidencia](RUST-VALIDATION.md) para comandos, identidad del contenido, resultados y límites. Son pruebas de mecanismos, no evaluación del comportamiento de agentes ni E2E del producto.

Al adaptar o copiar estas carpetas, conserva el copyright original de Jose Corral en sus licencias MIT. No exijas que todas las licencias de skills sean idénticas a la licencia raíz: deben corresponder al material distribuido.

Para futuros cambios, repite los casos afectados y actualiza la identidad de contenido solo tras revisar el diff. No conviertas fallos por caché offline, lockfile o configuración en permiso de instalación. Los escenarios no ejecutados siguen pendientes aunque las comprobaciones documentales sean correctas.

## LSP local

El `crushrc` configura el servidor 1.98.0 ya instalado, sin cambiar la toolchain global. Comprueba su sintaxis con `bash -n crushrc`. Esta colección no contiene un workspace Cargo ni requiere LSP para leer skills. Una prueba directa del servidor no equivale a confirmar su activación en la sesión actual de Crush.
