# Mapa de Historias de Usuario (User Story Map)

## 1. Propósito y Trazabilidad

Este documento traduce las *Capacidades Funcionales* a un flujo cronológico de interacción. Garantiza la trazabilidad bidireccional: ninguna historia existe sin un requisito que la justifique, y ningún requisito queda sin un vector de implementación.

**Convenciones de trazabilidad:**

* Los **RF de plataforma** (RF-10.1 a RF-10.4) no generan historias individuales; son objetivos de entrega trazados en la Sección 5 (Releases).
* Los **RF de restricción de alcance** (RF-02.7: sin adjuntos binarios en el ciclo inicial) no generan historias porque definen lo que el sistema *no* hace. Se reconocen como fronteras de alcance en la Sección 3.
* Los **RNF transversales** que aplican a toda historia se declaran en la Sección 4 y no se repiten en la columna de RNF Específicos de la matriz.

## 2. El Viaje del Agente (Secuencia Base)

1. **Génesis del Contenedor (Configuración Inicial):** El Agente Operador interactúa por primera vez con el sistema para establecer los fundamentos de su bóveda. Esto incluye la creación de un nuevo Contenedor Criptográfico vacío a partir de una frase de paso maestra (RF-01.1), la configuración opcional de un archivo-llave como segundo vector de entropía (RF-09.2), y la vinculación de su cuenta de Google Drive como Canal Simétrico Ciego para la sincronización (RF-05.1). Esta fase ocurre una sola vez al inicio de la vida del sistema y establece todos los parámetros criptográficos fundacionales: Sales, Semillas KDF, Magic Numbers, Nonce ≥ 192 bits y Testigo de Verificación Temprana.

2. **Resolución Criptográfica (Autenticación y Desbloqueo):** Puerta de entrada obligatoria a toda sesión operativa. El Agente Operador inyecta su entropía originaria (frase de paso maestra y/o archivo-llave) a través de la Interfaz de Inyección de Entropía. El Agente Cliente ejecuta la cadena completa de derivación criptográfica: hasheo individual de inputs → pre-llave bruta compuesta → KDF (Argon2id) → HKDF (sub-llaves de dominio aislado) → validación AEAD/AAD de la Cabecera → verificación del Testigo de Verificación Temprana → descifrado secuencial del Payload con verificación HMAC por bloque. Si cualquier paso falla, abort Fail-Closed sin revelar la etapa. Los vectores de entropía del usuario se destruyen inmediatamente tras la derivación (RF-09.3). Al abrir la aplicación, el sistema opera sobre la copia local persistida del Contenedor (RF-05.7); si hay conectividad, descarga automáticamente la versión más reciente desde Google Drive (RF-05.3) y, al detectar líneas temporales divergentes, ejecuta la fusión CRDT automática mediante reglas heurísticas deterministas (RF-05.4, RF-06.2).

3. **Gestión del Inventario (Operación sobre Activos):** Núcleo operativo del sistema donde el Agente Operador realiza las acciones de valor sobre sus credenciales. Comprende: crear nuevos activos (RF-02.1), visualizar datos con descifrado JIT de campos sensibles (RF-02.2), editar campos con registro granular de Deltas en el Log Mutacional CRDT (RF-02.3), eliminar activos como Deltas de revocación (RF-02.4), buscar por coincidencia parcial (RF-02.5), organizar en grupos jerárquicos (RF-02.6), y generar contraseñas de alta entropía mediante el Agente Generador Estocástico con indicador de fortaleza (RF-03.1, RF-03.2, RF-03.3). Toda operación se registra como un Delta inmutable con estampa lógico-temporal e identificador de dispositivo en el Log Mutacional (RF-06.1), ejecutándose localmente en RAM en modo Offline-First (RF-05.6) sin requerir conectividad.

4. **Entrega Transitoria (Salida Útil al Suprasistema):** El sistema materializa su propósito entregando las credenciales al entorno externo a través de la Interfaz de Expulsión Controlada. El Agente Operador acciona la copia al portapapeles de cualquier campo sensible (RF-04.1), con auto-destrucción temporal del contenido copiado (RF-04.2), o invoca el mecanismo de autocompletado/asistencia de llenado para inyectar credenciales directamente en campos de autenticación externos del sistema operativo (RF-04.3). Los datos sensibles abandonan la ofuscación JIT en RAM únicamente durante el milisegundo exacto de entrega.

5. **Persistencia y Sincronización (Canal Simétrico Ciego):** El Agente Operador desencadena (explícita o implícitamente) la serialización del estado actual de la bóveda. El guardado explícito (RF-01.6) o el cierre de la bóveda (RF-01.3) producen una nueva versión íntegra del Contenedor Criptográfico, aplicando compresión → padding → cifrado → autenticación por bloques. El Contenedor se persiste localmente en disco en estado cifrado (RF-05.7) y se sube automáticamente a Google Drive (RF-05.2). El Cover Traffic — proceso de fondo activo durante toda la sesión abierta, no solo durante el guardado — mantiene sincronizaciones periódicas a frecuencia constante independientemente de la actividad real del usuario, para mitigar análisis de tráfico (RF-05.5). Esta fase también abarca la desvinculación de Google Drive (RF-05.1b). La compactación local del Log Mutacional (RF-06.3) se ejecuta como proceso interno durante la serialización para contener el crecimiento del Contenedor.

6. **Sellado y Protección (Cierre del Ciclo):** El ciclo operativo concluye mediante cierre explícito de la bóveda por el Agente Operador (RF-01.3) — que incluye auto-guardado de cambios pendientes — o mediante bloqueo automático por inactividad (RF-10.5). En ambos casos, el sistema destruye todo material criptográfico y datos en texto plano de la memoria volátil (zeroing) y regresa a la pantalla de desbloqueo (Fase 2).

**Nota — Operaciones de Mantenimiento del Contenedor (transversales a la sesión):** En cualquier momento de la sesión activa (no exclusivamente al cierre), el Agente Operador puede ejecutar operaciones de mantenimiento sobre el Contenedor que no constituyen gestión de activos: cambiar la frase de paso maestra re-derivando toda la cadena criptográfica (RF-01.4), actualizar los parámetros KDF cuando el sistema detecta degradación del tiempo de derivación (RF-08.1, RF-08.2), y configurar las preferencias del sistema (timeout de auto-lock por RF-10.5/RNF-03.8, caducidad del portapapeles por RF-04.2, frecuencia de Cover Traffic por RF-05.5). Las operaciones de RF-01.4 y RF-08.2 desencadenan un ciclo completo de re-derivación criptográfica y re-serialización del Contenedor (Fase 5).

## 3. Matriz del Mapa de Historias

*Por cada Fase del viaje definida en la sección anterior, se desglosan las historias consolidadas. Cada historia representa un flujo de valor completo y cohesivo. La columna **RNF Específicos** lista únicamente los requerimientos no funcionales que exigen verificación particularizada en el contexto de esa historia; los RNF transversales que aplican a toda historia se declaran en la Sección 4 y operan como criterios de aceptación implícitos globales.*

| Fase del Viaje (Secuencia) | ID Historia | Título Breve de la Historia (Acción y Valor) | RF Vinculados | RNF Específicos |
| :--- | :--- | :--- | :--- | :--- |
| **1. Génesis del Contenedor** | HU-01 | Crear y configurar una bóveda nueva: generar automáticamente todos los parámetros criptográficos fundacionales a partir de una frase de paso maestra (con advertencia entrópica clara y no descartable sin confirmación deliberada si está por debajo del umbral mínimo), calibrando los parámetros KDF al hardware local, y configurar opcionalmente un archivo-llave como segundo vector de entropía | RF-01.1, RF-01.5, RF-09.2 | RNF-01.1, RNF-01.2, RNF-01.3, RNF-01.4, RNF-01.5, RNF-01.8, RNF-03.1, RNF-07.3 |
| | HU-02 | Vincular y desvincular la cuenta de Google Drive mediante OAuth: almacenar tokens en el almacén seguro del SO al vincular, revocarlos explícitamente y detener toda sincronización al desvincular, permitiendo vincular una cuenta diferente posteriormente | RF-05.1, RF-05.1b | RNF-02.6 |
| **2. Resolución Criptográfica** | HU-03 | Abrir la bóveda con resolución completa de estado: ejecutar la cadena de derivación criptográfica (hasheo → pre-llave → KDF → HKDF → validación AEAD/AAD → Testigo → descifrado secuencial con HMAC por bloque), abortar en Fail-Closed sin revelar la etapa ante cualquier fallo de verificación, tolerar campos de Cabecera desconocidos de versiones futuras sin abortar la apertura, operar sobre la copia local en modo Offline-First cuando no hay conectividad, descargar automáticamente la versión remota al detectar conectividad, y fusionar líneas temporales divergentes mediante el motor CRDT sin intervención del usuario — resolviendo correctamente los 4 escenarios de conflicto (mismo activo/campos distintos, mismo campo, edición vs. eliminación, creaciones simultáneas) | RF-01.2, RF-05.3, RF-05.4, RF-05.6, RF-05.7, RF-06.2, RF-06.4, RF-08.3, RF-09.1, RF-09.2, RF-09.3 | RNF-01.1, RNF-01.2, RNF-01.3, RNF-01.4, RNF-03.1, RNF-03.2, RNF-03.4, RNF-04.1, RNF-04.3, RNF-07.1 |
| **3. Gestión del Inventario** | HU-04 | Gestionar el inventario completo de activos y grupos: crear activos (título, usuario, contraseña, URL, notas), visualizar con descifrado JIT de campos sensibles, editar registrando cada cambio como Delta individual en el Log Mutacional CRDT, eliminar como Delta de revocación sin borrado físico, buscar por coincidencia parcial en título/usuario/URL, organizar en estructura jerárquica de grupos (crear, renombrar, mover, eliminar), generar contraseñas de alta entropía configurable (longitud, composición) tanto de forma independiente como contextual, y visualizar indicador de fortaleza entrópica para cualquier contraseña generada o ingresada | RF-02.1, RF-02.2, RF-02.3, RF-02.4, RF-02.5, RF-02.6, RF-03.1, RF-03.2, RF-03.3, RF-06.1 | RNF-01.6, RNF-01.7, RNF-03.3 |
| **4. Entrega Transitoria** | HU-05 | Entregar credenciales al suprasistema: copiar al portapapeles cualquier campo sensible con auto-destrucción tras periodo de caducidad configurable (por defecto 10–30 s), y *(Escritorio y Móvil)* inyectar credenciales en campos de autenticación externos mediante autocompletado o asistencia de llenado del SO | RF-04.1, RF-04.2, RF-04.3 | RNF-03.5 |
| **5. Persistencia y Sincronización** | HU-06 | Persistir, sincronizar y proteger volumétricamente la bóveda: guardar explícitamente serializando el Log Mutacional con compresión → padding dinámico (escalones fijos con auto-escalamiento) → cifrado → autenticación HMAC por bloques, persistir localmente el Contenedor cifrado verificando la escritura antes de toda subida remota, subir automáticamente a Google Drive reteniendo al menos 5 últimas revisiones, compactar el Log Mutacional consolidando Deltas históricos unificados, y ejecutar Cover Traffic a frecuencia constante configurable indistinguible de sincronizaciones reales en tamaño y patrón temporal | RF-01.6, RF-05.2, RF-05.5, RF-05.7, RF-06.3, RF-07.1, RF-07.2, RF-07.3 | RNF-01.2, RNF-01.4, RNF-01.5, RNF-04.2, RNF-04.4, RNF-08.1, RNF-08.2, RNF-08.3 |
| **6. Sellado y Protección** | HU-07 | Cerrar y proteger la sesión: cierre explícito con auto-guardado de cambios pendientes, *(Escritorio y Móvil)* bloqueo automático por inactividad configurable (1–30 min, por defecto 5 min, sin opción "nunca"), zeroing completo de todo material criptográfico y datos en claro de RAM, y retorno a pantalla de desbloqueo exigiendo re-autenticación | RF-01.3, RF-10.5 | RNF-02.3, RNF-03.8 |
| **Mantenimiento del Contenedor (transversal)** | HU-08 | Mantener la resiliencia criptográfica del Contenedor: cambiar la frase de paso maestra re-derivando toda la cadena criptográfica y re-cifrando el Contenedor completo (con advertencia entrópica), detectar periódicamente si el tiempo de derivación KDF ha descendido bajo el umbral seguro y notificar al usuario, permitir actualizar los parámetros KDF (rondas/memoria) re-cifrando de forma transparente, y soportar forward-compatibility leyendo sin abortar campos de Cabecera desconocidos de versiones futuras preservándolos en re-serializaciones | RF-01.4, RF-08.1, RF-08.2, RF-08.3 | RNF-01.1, RNF-01.2, RNF-01.3, RNF-01.4, RNF-01.5, RNF-01.8, RNF-03.1, RNF-07.1, RNF-07.2 |

**Restricción de alcance (RF-02.7):** Los activos se limitan a campos de texto estructurado (título, usuario, contraseña, URL, notas). El sistema no soportará adjuntos binarios (archivos, imágenes, documentos) en su ciclo inicial.

## 4. Requerimientos No Funcionales Transversales (Implícitos)

* **RNF-02.1 — Aislamiento Volátil Estricto:** Los secretos descifrados y el material criptográfico derivado deben residir exclusivamente en RAM. Bajo ninguna circunstancia se escribirán en almacenamiento persistente en texto plano.
* **RNF-02.2 — Bloqueo de Paginación (Escritorio y Móvil):** Las regiones de memoria con material criptográfico o secretos descifrados deben protegerse contra paginación a swap (`VirtualLock` en Windows, `mlock` en Android/Linux).
* **RNF-02.3 — Zeroing Determinístico:** Todo material sensible debe borrarse explícitamente de memoria con funciones resistentes a eliminación por optimización del compilador (`CryptographicOperations.ZeroMemory()` en .NET, `Arrays.fill()` sobre buffers nativos en Kotlin/Android, sobreescritura de `TypedArray` en WASM en Angular). Nunca depender del GC.
* **RNF-02.4 — Buffers Fuera del Heap Administrado:** Los secretos deben encapsularse en buffers que no gestione el Garbage Collector: `Span<byte>` pinned / `stackalloc` en .NET, `ByteBuffer.allocateDirect` / JNI en Kotlin/Android, WASM memory vía libsodium.js en Angular.
* **RNF-02.5 — Ventana Mínima de Exposición Web:** En el cliente web, descifrar JIT solo al momento de consulta, destruir referencias inmediatamente, y documentar al usuario que el entorno web ofrece garantías de aislamiento inferiores a los nativos.
* **RNF-05.1 — Compatibilidad Windows:** Windows 10 build 1903 en adelante.
* **RNF-05.2 — Compatibilidad Android:** API 26 (Android 8.0 Oreo) en adelante.
* **RNF-05.3 — Compatibilidad Web:** Chrome, Edge y Firefox en sus 2 últimas versiones mayores por release. Safari no es requisito.
* **RNF-05.4 — Determinismo Binario Cross-Platform:** El formato del Contenedor Criptográfico debe ser binariamente idéntico independientemente de la plataforma que lo genere. Cero variación de serialización, endianness o codificación.
* **RNF-05.5 — Determinismo Criptográfico Cross-Platform:** Todas las operaciones criptográficas (KDF, AEAD, HKDF, HMAC, CSPRNG) y la compresión deben producir resultados byte-a-byte idénticos para los mismos inputs en las tres plataformas.
* **RNF-06.1 — UI en Español:** Toda la interfaz de usuario (menús, etiquetas, mensajes, tooltips) debe estar íntegramente en español.
* **RNF-06.2 — Código en Inglés:** Todo el código fuente (identificadores, comentarios, commits, documentación técnica interna) debe estar íntegramente en inglés.
* **RNF-06.3 — Retroalimentación Interactiva ≤ 200 ms:** Las operaciones interactivas deben proveer feedback visual al usuario en ≤ 200 ms. Operaciones de mayor duración deben mostrar indicador de progreso.
* **RNF-03.6 — Techo de Diseño 10,000 Activos:** Las estructuras de datos y algoritmos deben diseñarse para no degradarse catastróficamente hasta 10,000 credenciales por bóveda. Más allá de ese umbral, el sistema puede rechazar inserciones con mensaje informativo.
* **RNF-03.7 — Techo de RAM:** ≤ 64 MiB en Android, ≤ 128 MiB en escritorio, ≤ 128 MiB de heap JS en web (bóveda de 500 activos, excluyendo KDF transitorio).

## 5. Criterios de Liberación (Releases / Slices)

*Los Releases están ordenados por secuencia lógica de entrega, no por plazos calendario. El proyecto se desarrolla en tiempo disponible del autor; las fronteras de cada release se definen por completitud funcional, no por fechas.*

**Trazabilidad de Requerimientos de Plataforma:**

| RF de Plataforma | Release | Descripción |
| :--- | :--- | :--- |
| RF-10.1 | Release 1 | Aplicación de escritorio funcional en Windows |
| RF-10.2 | Release 2 | Aplicación móvil nativa funcional en Android |
| RF-10.3 | Release 3 | Aplicación web accesible desde navegador moderno |
| RF-10.4 | Transversal (RNF-05.4) | Formato binariamente idéntico cross-platform, sin conversión |

### 5.1. Release 1 — Escritorio Funcional con Sincronización

* **Objetivo de la Liberación:** Entregar la aplicación de escritorio (Windows — RF-10.1) como primer cliente funcional con el ciclo completo: crear bóveda → gestionar credenciales → sincronizar vía Google Drive. Establecer el formato del Contenedor Criptográfico y el motor CRDT como núcleo validado antes de extender a otras plataformas.
* **Historias incluidas:** HU-01, HU-02, HU-03, HU-04, HU-05 (parcial), HU-06 (parcial), HU-07, HU-08
* **Alcance parcial de historias en este Release:**
    * **HU-05 — Entrega Transitoria:** Se implementa portapapeles con auto-destrucción configurable (RF-04.1, RF-04.2). Se difiere el autocompletado del SO (RF-04.3) al Release 2 — requiere integración profunda con el SO que se implementa tras estabilizar el core.
    * **HU-06 — Persistencia y Sincronización:** Se implementa guardado + persistencia local + subida a GDrive + compactación (RF-01.6, RF-05.2, RF-05.7, RF-06.3, RF-07.1, RF-07.2, RF-07.3). Se difiere Cover Traffic (RF-05.5) al Release 2 — requiere el motor de sincronización estabilizado como prerequisito.

### 5.2. Release 2 — Extensión Móvil y Opacidad Operacional

* **Objetivo de la Liberación:** Extender la disponibilidad a Android (RF-10.2) como segundo cliente, validar la estabilidad del motor CRDT en escenarios reales de concurrencia escritorio ↔ móvil, y completar las capacidades diferidas de opacidad observacional y entrega avanzada.
* **Historias completadas en este Release:**
    * **HU-05 — porción diferida:** Autocompletado/asistencia de llenado del SO (RF-04.3) en Escritorio y Móvil.
    * **HU-06 — porción diferida:** Cover Traffic a frecuencia constante configurable (RF-05.5) con indistinguibilidad de sincronizaciones reales (RNF-08.2).
* **Nota:** Todas las historias de Release 1 se re-validan en la plataforma Android. No generan nuevos IDs de historia porque el comportamiento funcional es idéntico; la variación es de plataforma y se verifica contra los RNF transversales de compatibilidad (RNF-05.2, RNF-05.4, RNF-05.5).

### 5.3. Release 3 — Cliente Web y Consolidación

* **Objetivo de la Liberación:** Incorporar el cliente web (RF-10.3) como tercer punto de acceso para situaciones de emergencia o equipos ajenos, completando la disponibilidad multiplataforma. Someter el formato criptográfico y el motor CRDT a auditoría de seguridad. Evaluar la apertura al público.
* **Historias incluidas:** Re-validación de todas las HU aplicables en plataforma web, con las siguientes exclusiones:
    * **HU-05 (autocompletado del SO — RF-04.3):** El autocompletado/asistencia de llenado del SO no aplica en web por limitaciones inherentes de la plataforma. La copia al portapapeles con auto-destrucción (RF-04.1, RF-04.2) sí aplica en web.
    * **HU-07 (auto-lock por inactividad — RF-10.5):** El bloqueo automático por inactividad no aplica en web por limitaciones inherentes de la plataforma. El cierre explícito de la bóveda (RF-01.3) con zeroing y retorno a pantalla de desbloqueo sí aplica en web.
* **Nota:** El cliente web opera con las advertencias de RNF-02.5 (garantías de aislamiento inferiores documentadas al usuario).
