# Adaptaciones Rust bajo control local

## Inventario y origen fijado

| Skill | Fuente histórica | Revisión completa | Adaptación | Licencia |
| --- | --- | --- | --- | --- |
| `rust-practices` | leonardomso/rust-skills | `fd2a861ab0406a4ac536a55274d14ea6fd1ca9c9` | Selección de propiedad, errores, async acotado y optimización para Fluzo | MIT, Copyright (c) 2025 Leonardo Maldonado |
| `rust-review` | apollographql/rust-best-practices | `eb485a5ddb68e0ded3d79549e994f63ec0a6f6c0` | Checklist de revisión adaptado para Fluzo | Apache-2.0 |

Se partió de las adaptaciones locales ya existentes en el repositorio Fluzo, revisión `4a7dce9833e011eca557031edb44b2e6e74939fa`. Las copias consumidoras se dejaron intactas, incluidos sus cambios ajenos a esta tarea. Las nuevas carpetas distribuidas están en `skills/rust-practices/` y `skills/rust-review/`.

El procedimiento se generalizó para descubrir contratos y comandos del consumidor, conservar las reglas Fluzo solo cuando aplican y separar propuesta, implementación y revisión. Cada archivo adaptado indica su modificación; cada carpeta conserva LICENSE y ORIGIN.md. No se copió el catálogo completo, código ejecutable, instaladores ni configuración de proveedores.

## Qué significa no tener dependencias externas de skills

Todo el contenido operativo está versionado en esta colección: entrada, reglas seleccionadas, checklist y casos de validación. Las carpetas funcionan sin acceso a los repositorios de origen y sin otras skills. No hay submódulos, enlaces simbólicos, actualización desde main/latest, carga obligatoria de handbooks ni descarga durante ejecución.

Los nombres de proyectos y URLs en ORIGIN.md son procedencia histórica y atribución, no puntos de integración. Conservarlos y conservar las licencias es compatible con controlar la copia local; eliminarlos no aportaría aislamiento y podría incumplir las condiciones de redistribución.

Ejecutar checks de Rust sigue necesitando las herramientas y dependencias del proyecto consumidor. Eso no equivale a depender de un proveedor remoto de skills. Ninguna instrucción instala esas herramientas ni autoriza inferencia, red, commits o publicación por implicación.

## Licencias verificadas

Durante la incorporación se consultaron con gh los árboles completos de las revisiones fijadas y sus licencias. Ambos árboles declararon `truncated: false`, contenían LICENSE y no un NOTICE separado. La entrada seleccionada de Apollo no contiene otro encabezado de copyright o atribución que deba trasladarse.

Se compararon las licencias locales byte a byte con las de esas revisiones:

| Archivo distribuido | SHA-256 |
| --- | --- |
| `skills/rust-practices/LICENSE` | `663ce6f8f087dc366602ac34eefd4cf71a5d8db411359422298a1399a014a9d5` |
| `skills/rust-review/LICENSE` | `c71d239df91726fc519c6eb72d318ec65820627232b2f796219e87dcf35d0ab4` |

La licencia MIT raíz corresponde al material original de la colección; no cambia los términos de Apache-2.0. Conserva la licencia Apache íntegra, incluido su apéndice estándar, y los avisos de modificación. Ese apéndice no es texto orientativo pendiente de completar por esta colección.

## Mantenimiento y actualizaciones

Los cambios ordinarios son cambios locales revisados en fluzo-skills. Para incorporar material externo adicional, identifica explícitamente la revisión y selección, revisa contenido y licencias, conserva avisos y actualiza la procedencia mediante un diff aprobado. Nunca sigas una rama móvil automáticamente.

Al instalar o actualizar un consumidor, copia toda la carpeta y conserva cambios locales. Comprueba nombre, referencias y licencia; no reemplaces su manifiesto de integridad ni sus archivos sin autorización. No se ha modificado la instalación de Fluzo como parte de esta incorporación.

## Verificación y límites

Se comprueban frontmatter, nombres, enlaces locales, sintaxis Bash, avisos de modificación, hashes de licencia y copia independiente de las cinco skills. La prueba de portabilidad lee archivos copiados sin depender de sus fuentes originales. No ejecuta un agente ni demuestra corrección de recomendaciones Rust, comportamiento de permisos o seguridad ante prompt injection.

Los casos de calibración se incluyen en `references/validation.md` de cada nueva skill. Su evaluación funcional está pendiente. No se reutiliza como prueba de estas adaptaciones la evidencia anterior de testing determinista y boundaries.

## Impacto en la release propuesta

La propuesta inicial de v1.0.0 apuntaba a `9d17e400d4a98996c59fd0a4129bc579a2d92f41` y solo contenía tres skills. La release con cinco debe apuntar al commit que incorpore estas carpetas y sus licencias, nunca al candidato anterior.

La solicitud posterior autorizó commit, push y release estable Latest con cinco skills, solo fuentes y sin assets adicionales. Durante la publicación se debe verificar el SHA final, crear el tag correspondiente, revisar el borrador y comprobar el resultado remoto. Este documento registra el alcance, no afirma que la publicación haya concluido ni autoriza futuras releases. No se incluye migración de consumidores.
