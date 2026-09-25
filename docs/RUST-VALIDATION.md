# Evidencia de validación Rust

## Identidad y alcance

Implementación local sin commit de `fluzo-deterministic-testing` y `fluzo-rust-boundaries`, adaptadas del procedimiento original en Fluzo, revisión `4a7dce9833e011eca557031edb44b2e6e74939fa`. Se conserva el copyright de Jose Corral junto al de la adaptación.

Identidad del contenido revisado por carpeta, SHA-256 sobre archivos ordenados por ruta relativa, concatenando ruta UTF-8, NUL, bytes y NUL:

| Skill | SHA-256 |
| --- | --- |
| fluzo-deterministic-testing | `b0ccdfd31bb26ba8d49f576d7d4fa275f48d49332c5df9febe0feda5d68dffd0` |
| fluzo-rust-boundaries | `a46ba514fc0246dd1fd8cf1ac1bf0413b1c7d7e669564bfcf01902871766c409` |

Entorno: Linux, Rust/Cargo 1.98.0 ya instalados. Las fixtures se construyeron con Python estándar en directorios temporales independientes, con HOME y CARGO_HOME privados, RUSTUP_HOME existente y toolchain explícita. No contenían dependencias externas, credenciales ni remotos. Se eliminaron al terminar. No se añadió una suite permanente ni una aplicación al repositorio de instrucciones.

Los resultados siguientes proceden de ejecuciones mecánicas controladas durante la implementación, no de agentes autónomos siguiendo las skills. Los programas temporales no se distribuyen como harness reutilizable; este registro permite conocer alcance y comandos, pero no reproduce por sí solo todos sus bytes.

## Grafo Cargo

Workspace temporal con `domain`, `service`, `bridge`, `presentation` y `entry`. Política de ejemplo: `entry -> service -> domain` permitido; `presentation -> service` prohibido, también transitivamente. El verificador temporal recorre IDs resueltos y aristas normales/build; excluye dev-only para la política de producción.

Desde la raíz de esa fixture, mediante `rustup run 1.98.0 cargo`:

```bash
cargo generate-lockfile --offline
cargo metadata --format-version 1 --locked --offline
cargo check --workspace --locked --offline
cargo metadata --format-version 1 --locked --offline --features presentation/direct
cargo check --workspace --locked --offline --features presentation/direct
cargo metadata --format-version 1 --locked --offline --features presentation/indirect
cargo check --workspace --locked --offline --features presentation/indirect
```

La generación del lockfile pertenece exclusivamente a la fixture creada y autorizada, no a un consumidor con lockfile preexistente.

| Caso ejecutado | Resultado |
| --- | --- |
| Features opcionales inactivas | Sin camino activo prohibido |
| Feature direct activa | Detectado `presentation -> service` |
| Feature indirect activa | Detectado `presentation -> bridge -> service` |
| Camino legal en las tres configuraciones | Aceptado `entry -> service -> domain` |
| Dependencia dev-only hacia service añadida a la fixture | No clasificada como violación de producción |
| Compilación de las tres configuraciones | Correcta, mostrando que compilar no equivale a respetar arquitectura |
| Lockfile tras metadata/checks | Bytes sin cambios |

Los casos negativos se rechazaron mediante aserciones del verificador de política, no mediante errores de compilación. No se ejercitaron targets adicionales, build scripts, múltiples versiones del mismo paquete ni revisión semántica automatizada de propiedad o I/O.

## Testing determinista

Fixture Rust mínima con función de actualización identificada por solicitud, regresión aritmética y seis tests de contrato. El simulador HTTP utiliza un listener propio de loopback con puerto efímero; no se contactaron servicios existentes. La sincronización de respuesta tardía usa canales con preparación y liberación explícitas, sin sleeps.

Desde la raíz temporal, con la misma toolchain:

```bash
cargo generate-lockfile --offline
cargo test --workspace --locked --offline --no-run
cargo test --locked --offline intended_regression
cargo test --workspace --locked --offline
cargo test --locked --offline cancel_then_late_result
```

| Caso ejecutado | Resultado |
| --- | --- |
| Fuente inicialmente defectuosa | Compila; assertion aritmética falla con salida Cargo 101 |
| Corrección solo de fuente | Seis tests correctos |
| HTTP esperado | Respuesta 200 y script consumido |
| HTTP inesperado | Respuesta 400 y fallo explícito del contrato |
| Paso obligatorio ausente | Rechazado por finalización del simulador |
| Cancelación y resultado tardío | No actualiza estado cancelado ni solicitud nueva; respuesta vigente sí se aplica |
| Guarda de resultado adulterada | La misma secuencia detecta el defecto con assertion y salida 101 |
| Corrección restablecida en la fixture | Tres ejecuciones completas consecutivas con seis tests correctos |
| Tests y manifiesto protegidos | Bytes idénticos antes y después de modificar la fuente |
| Destino no registrado | Caso unitario de guarda rechaza antes de conectar |

Identidad de la fixture determinista corregida: fuente SHA-256 `62687fd0005f89139b4e0e3422772c900728765de03c5e94c7b1570e7340f30c`; tests SHA-256 `d2e2378b89a2ea1d185b5fc23f1e0becc21d537596380dff41ff38d7be7b7ce8`.

Plazos: dos segundos en sockets/canales y noventa segundos como límite exterior de cada proceso Cargo. El listener y join dependen del límite exterior ante un fallo extremo; no constituyen un servidor endurecido frente a clientes arbitrarios. Los procesos concluyeron normalmente salvo las dos salidas negativas esperadas; no se necesitaron descargas ni instalaciones.

## Límites que permanecen abiertos

- La guarda de endpoints es un caso unitario: no prueba que cualquier proceso arbitrario carezca de egress. No se usó aislamiento de red del sistema operativo.
- No se ejecutó el bucle nativo de Fluzo, inferencia real, persistencia SQLite, recuperación de procesos ni todos los casos de las matrices de las skills.
- No se evaluó selección automática, cumplimiento de aprobaciones ni resistencia del modelo a prompt injection.
- Repetir tres veces una secuencia controlada no explora todos los interleavings.
- `tui-design` mantiene validación estática; su aplicación representativa sigue pendiente.
- La configuración LSP previa se preservó. Activación dentro de una nueva sesión Crush sigue pendiente; sus pruebas directas están en el research.

## Distribución

La validación final comprueba enlaces, nombres, frontmatter, sintaxis Bash de ejemplos, licencias, copia independiente y correspondencia de catálogos. No se instaló la colección en Fluzo ni se alteraron sus checks. Migrar requiere primero una revisión publicada, revisar el diff consumidor y conservar los cambios locales; no sobrescribir las copias actuales por implicación.
