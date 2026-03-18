# Documento de Requerimientos del Sistema (Funcionales y No Funcionales)

## 1. Capacidades Funcionales Base (Requerimientos Funcionales)

Los requerimientos funcionales se organizan por módulo lógico del sistema. Cada requerimiento describe una capacidad que WhiteLightPass —como implementación concreta del sistema definido en el Documento de Definición del Sistema— debe proveer en al menos una de sus plataformas objetivo (escritorio, móvil Android, web). Cuando un requerimiento aplica solo a plataformas específicas, se indica explícitamente.

### RF-01. Módulo de Gestión de la Bóveda (Contenedor Criptográfico)

| ID | Requerimiento |
|---|---|
| RF-01.1 | El sistema debe permitir **crear una nueva bóveda** (Contenedor Criptográfico vacío) a partir de una frase de paso maestra provista por el usuario, generando automáticamente todos los parámetros criptográficos iniciales (Sales, Semillas KDF, Marcadores de Identidad y Versionado, un **Nonce estocástico de al menos 192 bits** para mitigar colisiones en entornos asíncronos multi-dispositivo, y el **Testigo de Verificación Temprana (Stream Start Bytes)**) sin intervención técnica del usuario. |
| RF-01.2 | El sistema debe permitir **abrir una bóveda existente** solicitando la frase de paso maestra (y/o archivo-llave si fue configurado), ejecutando la derivación KDF, la derivación de sub-llaves (HKDF), la validación de la Cabecera vía AEAD/AAD, y el descifrado secuencial de los bloques del Payload con verificación concurrente de HMAC por bloque. Los bloques deben procesarse de forma iterativa y liberarse individualmente de memoria conforme se consumen (descarga secuencial en vivo), evitando acumular el Payload completo descifrado en RAM simultáneamente. Si cualquier paso de verificación falla, el proceso debe abortar en estado Fail-Closed sin revelar en cuál etapa falló. |
| RF-01.3 | El sistema debe permitir **cerrar la bóveda** explícitamente. Si existen cambios no persistidos desde el último guardado, el sistema debe serializar automáticamente el estado actual del Log Mutacional (aplicando el mismo proceso de compresión, padding, cifrado y autenticación por bloques descrito en RF-01.6) antes de proceder al cierre. Tras la serialización (o si no hay cambios pendientes), el sistema destruye todo el material criptográfico y los datos en texto plano de la memoria volátil (zeroing), y regresa la aplicación a su estado de reposo (pantalla de desbloqueo). |
| RF-01.4 | El sistema debe permitir **cambiar la frase de paso maestra** de una bóveda abierta, re-derivando todos los parámetros criptográficos y re-cifrando el Contenedor Criptográfico completo con las nuevas llaves. |
| RF-01.5 | El sistema debe generar y almacenar en la Cabecera Estructural Abierta los **Marcadores de Identidad y Versionado (Magic Numbers)** que permitan identificar inequívocamente el formato del archivo y la versión del esquema criptográfico utilizado. |
| RF-01.6 | El sistema debe permitir **guardar explícitamente** el estado actual de una bóveda abierta, serializando el Log Mutacional vigente y re-aplicando compresión, padding, cifrado y autenticación por bloques para producir una nueva versión íntegra del Contenedor Criptográfico. |

### RF-02. Módulo de Gestión de Activos Primarios (CRUD de Credenciales)

| ID | Requerimiento |
|---|---|
| RF-02.1 | El sistema debe permitir **crear un nuevo activo** (entrada de credencial) con al menos los siguientes campos: título, nombre de usuario, contraseña, URL asociada y notas de texto libre. |
| RF-02.2 | El sistema debe permitir **visualizar los datos de un activo** existente, descifrando JIT (Just-in-Time) los campos sensibles (contraseña) únicamente en el momento de la consulta explícita del usuario, manteniéndolos ofuscados en RAM en todo otro momento. |
| RF-02.3 | El sistema debe permitir **editar cualquier campo** de un activo existente. Cada edición debe registrarse como un Delta individual en el Log Mutacional (Motor CRDT), no como una sobrescritura monolítica del estado anterior. |
| RF-02.4 | El sistema debe permitir **eliminar un activo** existente. La eliminación se registra como un Delta de revocación en el Log Mutacional, no como un borrado físico del historial. |
| RF-02.5 | El sistema debe permitir **buscar activos** por coincidencia parcial en los campos de título, nombre de usuario y URL. |
| RF-02.6 | El sistema debe permitir **organizar los activos** en una estructura jerárquica de grupos/carpetas con al menos un nivel de anidamiento, permitiendo crear, renombrar, mover y eliminar grupos. Todas las operaciones sobre grupos (creación, renombramiento, movimiento, eliminación) deben registrarse como **Deltas individuales en el Log Mutacional** (Motor CRDT), al igual que las operaciones sobre activos, para garantizar su resolución automática ante líneas temporales divergentes entre dispositivos. |
| RF-02.7 | El alcance de los activos se limita a **campos de texto estructurado** (título, usuario, contraseña, URL, notas). El sistema **no soportará adjuntos binarios** (archivos, imágenes, documentos) como parte de un activo en su ciclo inicial. Si se implementa en el futuro, deberá respetar la segregación lógica de Adjuntos Pesados definida en el Documento de Definición del Sistema (Sección 2.2) para evitar el colapso de RAM durante los bucles de consolidación CRDT. |

### RF-03. Módulo de Generación Estocástica (Generador de Contraseñas)

| ID | Requerimiento |
|---|---|
| RF-03.1 | El sistema debe proveer un **generador de contraseñas aleatorias** basado en CSPRNG del sistema operativo local, configurable en longitud y composición de caracteres (mayúsculas, minúsculas, dígitos, símbolos especiales). |
| RF-03.2 | El generador debe poder invocarse tanto de forma **independiente** (para generar una contraseña nueva sin asociarla a un activo) como de forma **contextual** (directamente desde el formulario de creación o edición de un activo, inyectando el resultado en el campo de contraseña). |
| RF-03.3 | El sistema debe mostrar al usuario un **indicador de fortaleza entrópica** de la contraseña generada o ingresada manualmente, con el fin de disuadir el uso de secretos de baja entropía. |

### RF-04. Módulo de Entrega Transitoria (Salida Útil al Suprasistema)

| ID | Requerimiento |
|---|---|
| RF-04.1 | El sistema debe permitir **copiar al portapapeles** del sistema operativo el valor de cualquier campo sensible de un activo (contraseña, nombre de usuario, notas) mediante una acción explícita del usuario. |
| RF-04.2 | El contenido copiado al portapapeles debe **auto-destruirse** (limpiarse del portapapeles) transcurrido un periodo de caducidad configurable por el usuario, cuyo valor por defecto debe ser breve (ej. 10–30 segundos). |
| RF-04.3 | *(Escritorio y Móvil)* El sistema debe proveer un mecanismo de **autocompletado o asistencia de llenado** que permita inyectar las credenciales de un activo en campos de autenticación externos del sistema operativo, sin requerir la copia manual al portapapeles. La implementación específica dependerá de las capacidades de cada plataforma (ej. servicio de accesibilidad en Android, integración con el SO en escritorio). |

### RF-05. Módulo de Sincronización vía Nube Personal (Canal Simétrico Ciego)

| ID | Requerimiento |
|---|---|
| RF-05.1 | El sistema debe permitir al usuario **vincular su cuenta de Google Drive** mediante el flujo de autenticación OAuth estándar del proveedor, sin almacenar ni transmitir las credenciales de Google a través de WhiteLightPass. |
| RF-05.1b | El sistema debe permitir al usuario **desvincular su cuenta de Google Drive**, revocando los tokens OAuth almacenados localmente y deteniendo toda sincronización automática. El usuario debe poder vincular una cuenta diferente después de desvincular la anterior. |
| RF-05.2 | Una vez vinculada la cuenta, el sistema debe **subir automáticamente** el Contenedor Criptográfico al Google Drive del usuario tras cada operación de guardado (cierre de bóveda o guardado explícito), sin intervención manual. |
| RF-05.3 | Al abrir la aplicación, el sistema debe **descargar automáticamente** la versión más reciente del Contenedor Criptográfico desde Google Drive, comparando marcas de tiempo o identificadores de versión para determinar si la copia local es la más actual. |
| RF-05.4 | Cuando el sistema detecte que la copia local y la copia remota han divergido (ambas contienen cambios respecto a un ancestro común), debe ejecutar automáticamente el **proceso de fusión CRDT** sobre los Logs Mutacionales de ambas versiones, produciendo un Contenedor unificado sin pérdida de datos y sin requerir intervención del usuario. |
| RF-05.5 | El sistema debe soportar el **Cover Traffic (Flujo de Cobertura)**: sincronizaciones periódicas automáticas con el Contenedor a una **frecuencia constante configurable** ($t = \text{constante}$), independientemente de si existen cambios reales del usuario. Los paquetes de cobertura deben ser **indistinguibles en tamaño** de las sincronizaciones reales (gracias al Padding volumétrico de RF-07), de modo que un observador en la red o en el proveedor de almacenamiento no pueda diferenciar una sincronización real de una de cobertura. |
| RF-05.6 | El sistema debe operar en modo **Offline-First**: todas las operaciones CRUD sobre la bóveda deben ejecutarse localmente sin requerir conectividad a internet. La sincronización con Google Drive debe ocurrir cuando la conectividad esté disponible, sin bloquear ninguna operación local. |
| RF-05.7 | El sistema debe **persistir localmente** el Contenedor Criptográfico cifrado en disco tras cada operación de guardado (explícito o al cierre de la bóveda), garantizando que la bóveda esté disponible para apertura local incluso sin conectividad a Google Drive. La persistencia local es siempre en estado cifrado (nunca en texto plano). |

### RF-06. Módulo de Resolución de Conflictos (Motor CRDT)

| ID | Requerimiento |
|---|---|
| RF-06.1 | El sistema debe implementar un **Log Mutacional basado en eventos** (Event Sourcing) donde cada operación del usuario (crear, editar, eliminar activos) se registre como un Delta inmutable con identificador único, estampa lógico-temporal y referencia al dispositivo de origen. |
| RF-06.2 | El motor de resolución debe fusionar **líneas temporales divergentes** de múltiples Agentes Cliente aplicando reglas heurísticas deterministas que garanticen el mismo resultado final sin importar el orden de llegada de los Deltas (propiedad de conmutatividad de los CRDTs). |
| RF-06.3 | El motor debe implementar una **estrategia de compactación local** (Garbage Collection del Log Mutacional) que consolide Deltas históricos ya unificados globalmente, reduciendo el tamaño del Log sin perder el estado final de los activos. |
| RF-06.4 | El motor debe ser capaz de resolver correctamente los siguientes escenarios de conflicto sin intervención del usuario: a) Un mismo activo editado en campos distintos desde dos dispositivos. b) Un mismo activo editado en el mismo campo desde dos dispositivos. c) Un activo editado en un dispositivo y eliminado en otro. d) Nuevos activos creados simultáneamente en dispositivos distintos. |

### RF-07. Módulo de Homeostasis Volumétrica (Padding y Opacidad)

| ID | Requerimiento |
|---|---|
| RF-07.1 | El sistema debe inyectar **Padding dinámico** (bytes aleatorios desechables) en el Contenedor Criptográfico antes de cifrar, manteniendo el tamaño final del archivo en escalones fijos predefinidos para dificultar el análisis de tráfico basado en variaciones de tamaño. |
| RF-07.2 | Cuando el Payload real crezca más allá del escalón de Padding actual, el sistema debe **escalar al siguiente peldaño volumétrico** automáticamente, rellenando el espacio restante con entropía desechable. |
| RF-07.3 | El sistema debe aplicar **compresión determinista** sobre el Payload antes del cifrado. El orden de operaciones debe ser estrictamente: compresión → padding → cifrado, para prevenir ataques de extensión de longitud (oráculos CRIME/BREACH). |

### RF-08. Módulo de Adaptación Criptográfica (Resiliencia Evolutiva)

| ID | Requerimiento |
|---|---|
| RF-08.1 | El sistema debe **detectar periódicamente** si el tiempo de derivación KDF en el hardware actual ha descendido por debajo del umbral seguro configurado (ej. < 0.5 segundos), y en ese caso notificar al usuario sugiriendo un incremento de los parámetros KDF (rondas/memoria). |
| RF-08.2 | El sistema debe permitir al usuario **actualizar los parámetros KDF** de una bóveda abierta (incrementar rondas, cambiar algoritmo si el formato lo permite), re-cifrando el Contenedor con los nuevos parámetros de forma transparente. |
| RF-08.3 | La **Cabecera Estructural Abierta** debe ser extensible: el sistema debe soportar la lectura de campos de cabecera desconocidos (de versiones futuras) sin abortar la apertura, permitiendo la evolución gradual del formato. |

### RF-09. Módulo de Autenticación Compuesta (Múltiples Vectores de Entropía)

| ID | Requerimiento |
|---|---|
| RF-09.1 | El sistema debe soportar la apertura de la bóveda mediante **frase de paso maestra sola** como modo de autenticación mínimo. |
| RF-09.2 | El sistema debe soportar opcionalmente la **combinación de frase de paso maestra + archivo-llave** (Key File) como segundo vector de entropía, hasheándolos individualmente antes de concatenarlos para construir la pre-llave bruta compuesta. Otros vectores de entropía mencionados en el Documento de Definición del Sistema (ej. tokens asimétricos, firmas criptográficas de hardware) **no se incluyen en el alcance inicial** del producto y se evaluarán en fases futuras. |
| RF-09.3 | Los vectores de entropía inyectados por el usuario deben **destruirse de la memoria** inmediatamente después de derivar la Llave Compuesta, sin persistir en RAM más allá de la fase de derivación. |

### RF-10. Módulo de Plataforma y Ciclo de Vida de la Aplicación

| ID | Requerimiento |
|---|---|
| RF-10.1 | *(Escritorio)* El sistema debe proveer una **aplicación de escritorio** funcional en Windows como plataforma primaria. |
| RF-10.2 | *(Móvil)* El sistema debe proveer una **aplicación móvil nativa para Android** funcional. |
| RF-10.3 | *(Web — Mediano Plazo)* El sistema debe proveer una **aplicación web** accesible desde cualquier navegador moderno, que permita abrir, consultar y editar la bóveda almacenada en Google Drive. |
| RF-10.4 | La bóveda debe poder **abrirse y editarse desde cualquiera de las tres plataformas** sin requerir conversión de formato, operando siempre sobre el mismo Contenedor Criptográfico almacenado en Google Drive. |
| RF-10.5 | *(Escritorio y Móvil)* El sistema debe implementar un **bloqueo automático** de la bóveda tras un periodo de inactividad configurable por el usuario, destruyendo el material criptográfico en memoria y requiriendo re-autenticación para continuar. |

## 2. Atributos de Calidad (Requerimientos No Funcionales)

### RNF-01. Suite Criptográfica y Primitivas

| ID | Requerimiento |
|---|---|
| RNF-01.1 | La **Función Derivadora de Claves (KDF)** debe ser **Argon2id** (RFC 9106). Los parámetros mínimos de seguridad son: memoria ≥ 64 MiB, iteraciones ≥ 3, paralelismo ≥ 4, longitud de salida = 32 bytes (256 bits). Estos valores constituyen el piso de seguridad; el sistema debe calibrar los parámetros al hardware local de manera que el tiempo de derivación se mantenga en el rango de **0.5 s – 1.5 s** por intento en el dispositivo del usuario (conforme a RF-08.1). |
| RNF-01.2 | El **esquema AEAD primario** (Authenticated Encryption with Associated Data) debe ser **XChaCha20-Poly1305** con nonce de 192 bits y clave de 256 bits. Se selecciona esta primitiva porque: (a) satisface nativamente el invariante de Nonce ≥ 192 bits sin construcciones de extensión adicionales; (b) ofrece rendimiento constante en software puro, inmune a ataques de sincronización (timing side-channels) en dispositivos sin instrucciones criptográficas de hardware; (c) dispone de implementaciones auditadas y uniformes vía libsodium en las tres plataformas objetivo. |
| RNF-01.3 | La **derivación de sub-llaves** debe utilizar **HKDF-SHA256** (RFC 5869) con contextos ortogonales de dominio aislado. De la Llave Compuesta (salida de Argon2id) se derivarán como mínimo tres sub-llaves: `Key_Header` (autenticación AEAD de la Cabecera), `Key_Payload` (cifrado de bloques del Payload) y `Key_MAC` (autenticación HMAC por bloque). Cada sub-llave debe usar una etiqueta de contexto (`info`) textualmente distinta e irrepetible. |
| RNF-01.4 | Cada **bloque individual del Payload** debe autenticarse mediante **HMAC-SHA256** usando `Key_MAC`, siguiendo una construcción **Encrypt-then-MAC**: primero se cifra el bloque con `Key_Payload`, luego se calcula el HMAC sobre el bloque cifrado. La verificación del HMAC debe ejecutarse **antes** de intentar el descifrado del bloque, permitiendo la descarga secuencial en vivo sin exponer datos no autenticados en RAM. |
| RNF-01.5 | Los **Nonces** del Contenedor Criptográfico deben ser generados por CSPRNG y tener una longitud mínima de **192 bits** (requisito heredado del invariante del Sistema y de la especificación de XChaCha20-Poly1305). Ningún nonce debe reutilizarse entre dos instancias diferentes del Contenedor. |
| RNF-01.6 | El **Cifrado de Flujo JIT en RAM** (protección de campos sensibles en memoria tras el descifrado general — RF-02.2) debe emplear **XChaCha20** (stream cipher sin tag de autenticación) con una Protected Stream Key derivada en la Cabecera y un **IV criptográficamente único por cada activo**, calculado determinísticamente a partir del hash de identidad del Delta asociado. Queda estrictamente prohibida la reutilización de un IV estático en RAM para evitar colisiones de Keystream (*XOR attacks*). |
| RNF-01.7 | La **generación de entropía estocástica** (RF-03.1) debe utilizar exclusivamente fuentes CSPRNG provistas por el sistema operativo local: `BCryptGenRandom` en Windows, `/dev/urandom` o `getrandom()` en Android/Linux, y `crypto.getRandomValues()` en Web. Queda prohibido el uso de generadores pseudo-aleatorios basados en reloj del sistema, seeds de red o cualquier fuente determinística predecible por un observador externo (Invariante 7 del Sistema). |
| RNF-01.8 | El sistema debe **advertir explícitamente** al usuario cuando la frase de paso maestra ingresada (durante la creación de la bóveda o el cambio de frase de paso — RF-01.1, RF-01.4) se evalúe por debajo de un umbral entrópico mínimo razonable. La advertencia debe ser clara, visible y no descartable sin una acción deliberada de confirmación, pero **no debe bloquear** la operación: el usuario conserva la decisión final sobre su propia frase de paso (coherente con la filosofía de soberanía individual). Este requisito complementa el indicador de fortaleza de RF-03.3, operando como una salvaguarda contra el riesgo de degradación de entropía originaria descrito en la Sección 4.2.1 del Documento de Definición del Sistema. |

### RNF-02. Aislamiento y Protección de Memoria

| ID | Requerimiento |
|---|---|
| RNF-02.1 | Los secretos descifrados, el material criptográfico derivado (Llave Compuesta, sub-llaves, Protected Stream Key) y la pre-llave bruta del usuario **deben residir exclusivamente en memoria volátil (RAM)**. Bajo ninguna circunstancia se escribirán en almacenamiento persistente en texto plano (Invariante 2 del Sistema). |
| RNF-02.2 | *(Escritorio y Móvil)* La aplicación debe solicitar **bloqueo de memoria del sistema operativo** (`VirtualLock` en Windows, `mlock` en Android/Linux) sobre las regiones de memoria que contengan material criptográfico o secretos descifrados, impidiendo que el sistema operativo pagine esas regiones al archivo de intercambio (swap) en disco. |
| RNF-02.3 | Todo material sensible (Llave Compuesta, sub-llaves, pre-llave, vectores de entropía del usuario, campos descifrados de activos) debe ser **borrado explícitamente de memoria (zeroing)** inmediatamente después de su uso, empleando funciones resistentes a la eliminación por optimización del compilador. Las APIs específicas por plataforma tecnológica son: `CryptographicOperations.ZeroMemory()` en .NET (escritorio), `Arrays.fill()` sobre buffers nativos vía JNI o `Destroyable.destroy()` en Kotlin/Android (móvil), y sobreescritura explícita de `TypedArray` en WASM memory en Angular (web). En ningún caso el zeroing puede depender de la recolección de basura (GC) del runtime. |
| RNF-02.4 | Los buffers criptográficos sensibles **no deben gestionarse mediante recolectores de basura (Garbage Collectors)** que no garanticen zeroing determinístico. Las tres plataformas tecnológicas elegidas (.NET en escritorio, JVM/Kotlin en móvil, JavaScript/Angular en web) poseen GC; por tanto, en **todas** ellas los secretos deben encapsularse en buffers fuera del heap administrado o en estructuras de vida controlada que aseguren el borrado explícito al finalizar su uso. Mitigaciones específicas: `Span<byte>` sobre memoria pinned (`GCHandle.Alloc` con `Pinned`) o `stackalloc` en .NET; `ByteBuffer.allocateDirect` o JNI nativo en Kotlin/Android; `WASM memory` (vía libsodium.js) en Angular. |
| RNF-02.5 | *(Web)* En el cliente web, donde el aislamiento de memoria es inherentemente limitado (JavaScript no ofrece `mlock` ni zeroing garantizado por hardware), la aplicación debe **minimizar la ventana temporal de exposición** de texto plano: descifrar JIT solo al momento de consulta, destruir referencias inmediatamente, y documentar explícitamente al usuario que el entorno web ofrece garantías de aislamiento inferiores a los clientes nativos. |
| RNF-02.6 | Los **tokens OAuth** obtenidos durante la vinculación con Google Drive (RF-05.1) deben almacenarse utilizando el **almacén seguro de credenciales del sistema operativo** de cada plataforma: Credential Manager / DPAPI en Windows, Android Keystore en móvil, y almacenamiento en memoria de sesión (no persistente) en Web. Los tokens **no deben almacenarse en texto plano** en disco ni en archivos de configuración sin cifrar. Al desvincular la cuenta (RF-05.1b), los tokens deben eliminarse del almacén seguro de forma explícita, no solo deferenciarse. |

### RNF-03. Rendimiento y Capacidad

| ID | Requerimiento |
|---|---|
| RNF-03.1 | El **tiempo de derivación KDF** (Argon2id) debe calibrarse al hardware local para mantenerse en el rango de **0.5 s – 1.5 s** por intento legítimo. Este rango equilibra la fricción exponencial contra ataques de fuerza bruta con la usabilidad percibida de apertura (Leverage Point #1 del Sistema, §4.3.1). |
| RNF-03.2 | La **apertura del Contenedor** (descifrado del Payload após KDF) debe completarse en **≤ 500 ms** para una bóveda con hasta 500 activos. Este umbral excluye el tiempo de derivación KDF (que ya tiene su propia restricción en RNF-03.1). |
| RNF-03.3 | La **búsqueda de activos** por coincidencia parcial (RF-02.5) debe retornar resultados en **≤ 100 ms** sobre un inventario de hasta 500 activos, medido desde la pulsación del usuario hasta la actualización visible de la lista de resultados. |
| RNF-03.4 | La **fusión CRDT** de dos Logs Mutacionales divergentes debe completarse en **≤ 2 s** para hasta 1,000 Deltas combinados (500 por rama). Este techo asegura que las sincronizaciones sean percibidas como instantáneas tras la descarga. |
| RNF-03.5 | La **copia al portapapeles** de un campo sensible (RF-04.1) debe ejecutarse en **≤ 50 ms** desde la acción del usuario hasta la escritura efectiva en el portapapeles del sistema operativo. |
| RNF-03.6 | El **techo de diseño** para el número de activos soportados es de **10,000 credenciales** por bóveda. Este límite no se espera alcanzar en uso real (el escenario típico es < 500), pero las estructuras de datos y algoritmos deben diseñarse para no degradarse catastróficamente hasta ese umbral. Más allá de 10,000, el sistema puede rechazar nuevas inserciones con un mensaje informativo. |
| RNF-03.7 | El **consumo de memoria RAM** de la aplicación con una bóveda abierta de 500 activos no debe superar **64 MiB en Android** ni **128 MiB en escritorio** (excluyendo la memoria consumida por el proceso de derivación KDF, que es transitoria). En el cliente web, se busca mantenerse por debajo de 128 MiB de heap JavaScript. |
| RNF-03.8 | El **timeout de bloqueo automático** por inactividad (RF-10.5) debe ser configurable por el usuario dentro de un rango de **1 minuto a 30 minutos**, con un **valor por defecto de 5 minutos**. No se permite la opción de desactivar completamente el auto-lock ("nunca bloquear"), ya que esto anularía la protección de zeroing por inactividad exigida por la Invariante 2 del Sistema. |

### RNF-04. Disponibilidad y Resiliencia

| ID | Requerimiento |
|---|---|
| RNF-04.1 | El sistema debe operar en modo **Offline-First** (RF-05.6): el 100% de las operaciones CRUD locales deben ser funcionales sin conectividad a internet. Ninguna operación de creación, lectura, edición, eliminación o búsqueda de activos puede bloquearse ni degradarse por la ausencia de conexión a Google Drive. |
| RNF-04.2 | El sistema debe aprovechar el **versionado nativo de Google Drive** (revision history) para conservar al menos las **5 últimas versiones** del Contenedor Criptográfico subido. Tras cada subida exitosa (RF-05.2), el sistema debe marcar la revisión como retenida (`keepForever` o equivalente en la API de GDrive) hasta que existan al menos 5 revisiones posteriores, permitiendo al usuario restaurar una versión anterior ante corrupción o sobreescritura accidental. Si la API de Google Drive no soporta este control o falla la retención, el sistema debe registrar la incidencia pero **no bloquear** la operación de subida. |
| RNF-04.3 | Todo fallo de verificación de integridad — sea en la autenticación AEAD de la Cabecera, en el Testigo de Verificación Temprana (Stream Start Bytes), o en el HMAC de cualquier bloque individual del Payload — debe resultar en un **abort en estado Fail-Closed**: destrucción inmediata de todo material criptográfico en memoria (zeroing), retorno a pantalla de desbloqueo, y mensaje genérico de error que **no revele** en cuál etapa específica falló la verificación (Invariante 5 y 6 del Sistema). |
| RNF-04.4 | La **persistencia local** del Contenedor cifrado (RF-05.7) debe completarse y verificarse **antes** de iniciar cualquier subida a Google Drive. Si la escritura local falla, la subida no debe ejecutarse y el usuario debe ser notificado. Esto garantiza que la copia local siempre refleje el último estado guardado, independientemente del resultado de la sincronización remota. |

### RNF-05. Compatibilidad de Plataforma

| ID | Requerimiento |
|---|---|
| RNF-05.1 | **Escritorio:** Windows 10 (build 1903 en adelante) y versiones posteriores. |
| RNF-05.2 | **Móvil:** Android API 26 (8.0 Oreo) y versiones posteriores. |
| RNF-05.3 | **Web:** Chrome, Edge y Firefox en sus **2 últimas versiones mayores** al momento de cada release. Safari no es requisito. |
| RNF-05.4 | El **formato del Contenedor Criptográfico** debe ser **binariamente idéntico** independientemente de la plataforma que lo genere. No debe existir ninguna variación de serialización, endianness o codificación entre Windows, Android y Web. Un Contenedor producido en cualquier plataforma debe ser legible sin conversión por cualquier otra. |
| RNF-05.5 | Todas las **operaciones criptográficas** (KDF, AEAD, HKDF, HMAC, CSPRNG) y la **compresión del Payload** (RF-07.3) deben producir resultados **determinísticamente idénticos** para los mismos inputs en las tres plataformas. Esto exige que tanto las implementaciones criptográficas como la librería de compresión compartan la misma base (libsodium recomendado para criptografía; para compresión, un algoritmo determinista con implementación única o validada con vectores de prueba cruzados inter-plataforma) o estén validadas con vectores de prueba cruzados que verifiquen output byte-a-byte idéntico. Si la compresión produce resultados diferentes por plataforma, el tamaño del Contenedor variará entre plataformas, violando RNF-05.4 y potencialmente el PVR de RNF-08.1. |

### RNF-06. Interfaz y Localización

| ID | Requerimiento |
|---|---|
| RNF-06.1 | La **interfaz de usuario** debe estar íntegramente en **español**. Todos los textos visibles por el usuario (menús, etiquetas, mensajes de error, confirmaciones, tooltips) deben estar en español. |
| RNF-06.2 | El **código fuente** (identificadores, nombres de variables, funciones, clases, comentarios, mensajes de commit, documentación técnica interna) debe estar íntegramente en **inglés**. |
| RNF-06.3 | Las **operaciones interactivas** (crear activo, editar, eliminar, navegar entre grupos, copiar al portapapeles) deben proveer retroalimentación visual al usuario en **≤ 200 ms** desde la acción del usuario. Operaciones que requieran procesamiento más largo (apertura de bóveda, sincronización, fusión CRDT) deben mostrar un indicador de progreso. |

### RNF-07. Mantenibilidad y Evolución del Formato

| ID | Requerimiento |
|---|---|
| RNF-07.1 | El formato del Contenedor debe soportar **compatibilidad hacia adelante en lectura**: una versión del Agente Cliente que encuentre campos de Cabecera desconocidos (de una versión futura del formato) debe poder leerlos sin abortar la apertura, siempre que los campos conocidos sean válidos. Los campos desconocidos se preservan en re-serializaciones para evitar pérdida de metadatos. |
| RNF-07.2 | La suite criptográfica (algoritmo AEAD, algoritmo KDF, función de hash para HMAC y HKDF) debe ser **configurable vía parámetros de la Cabecera Estructural Abierta**, no codificada de forma estática en la aplicación. Esto permite migrar a nuevas primitivas (ej. de XChaCha20-Poly1305 a un algoritmo post-cuántico) mediante una actualización de la aplicación que re-cifre el Contenedor con la nueva suite, sin rediseñar el formato binario (Sección 5.3.2 del Sistema). |
| RNF-07.3 | Los **Marcadores de Identidad y Versionado (Magic Numbers)** deben incluir al menos: un identificador de formato (magic bytes fijos que distinguen un archivo WhiteLightPass de cualquier otro binario) y un número de versión del esquema que permita a la aplicación determinar qué pipeline de procesamiento aplicar. |

### RNF-08. Opacidad Observacional (Mitigación de Análisis de Tráfico)

| ID | Requerimiento |
|---|---|
| RNF-08.1 | El **Padding Variance Ratio (PVR)** — definido como la variación porcentual del tamaño observable del Contenedor Criptográfico mes a mes en condiciones de uso normal — debe mantenerse por debajo del **1%** mientras el Payload real no supere el escalón de padding vigente (indicador §5.2 del Sistema). Cuando el escalón salta, la variación es estructuralmente inevitable pero debe ocurrir como un salto discreto único, no como un crecimiento gradual observable. |
| RNF-08.2 | Los **paquetes de Cover Traffic** (RF-05.5) deben ser **indistinguibles** de las sincronizaciones reales tanto en tamaño (garantizado por el Padding de RF-07) como en patrón temporal (frecuencia constante configurable). Un observador en la red o en Google Drive no debe poder diferenciar una sincronización que contiene cambios reales de una sincronización de cobertura, ni por tamaño del archivo transferido, ni por el momento en que ocurre respecto a la cadencia establecida. |
| RNF-08.3 | El **tamaño del escalón de Padding** (Padding Step Size) debe ser configurable internamente como parámetro del sistema. El valor por defecto debe establecer un equilibrio entre opacidad (escalones grandes reducen la frecuencia de saltos observables) y eficiencia de almacenamiento/transferencia (escalones excesivamente grandes desperdician cuota de Google Drive y ancho de banda). Un valor inicial razonable es un escalón base de **1 MiB** con crecimiento geométrico si el Payload supera los primeros peldaños (ej. 1 → 2 → 4 → 8 MiB). |
