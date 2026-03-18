# Documento de Visión del Producto

## 1. El Problema y la Oportunidad

### 1.1. Declaración del Problema Central

La gestión de credenciales y secretos digitales enfrenta hoy una crisis bifurcada que obliga a los usuarios a elegir entre dos modelos igualmente deficientes. Un tercer modelo, aparentemente intermedio, también fracasa en la práctica:

**Modelo A — Autogestión Caótica (Status Quo del Usuario Promedio):**
Los operadores humanos, confrontados con decenas o cientos de identidades digitales dispersas en servicios independientes, recurren sistemáticamente a estrategias de baja entropía: reutilizar variantes predecibles de una misma contraseña, almacenar credenciales en archivos de texto plano, hojas de cálculo sin cifrar, notas adhesivas o hilos de mensajería instantánea. Este patrón genera una superficie de ataque masiva donde el compromiso de un solo servicio externo desencadena un efecto dominó sobre la totalidad de la vida digital del operador. El usuario carece de herramientas que le permitan generar, almacenar y recuperar secretos de alta entropía sin depender de su propia memoria biológica (inherentemente limitada y predecible).

**Modelo B — Delegación a Bóvedas Centralizadas SaaS (El Riesgo Sistémico Oculto):**
La alternativa predominante en el mercado consiste en delegar la custodia total de los secretos a un operador centralizado externo (servicios de gestión de contraseñas en la nube como 1Password, LastPass, Bitwarden Cloud, Dashlane). Este modelo introduce un riesgo catastrófico de punto único de fallo criptográfico: el compromiso del proveedor (por brecha de seguridad, presión gubernamental, insolvencia o adquisición hostil) expone simultáneamente la totalidad del inventario de credenciales de todos sus usuarios. Adicionalmente, esta delegación:

- **Subordina la privacidad del usuario** a las políticas internas, jurisdicciones legales y decisiones comerciales de una entidad externa sobre la cual el operador no tiene control ni auditoría.
- **Viola el principio de Conocimiento Cero (Zero-Knowledge)** cuando los metadatos de uso, la frecuencia de acceso, la cantidad de secretos o los patrones de sincronización son observables o inferibles por el proveedor o por un adversario que comprometa su infraestructura.
- **Introduce dependencia de disponibilidad externa:** el usuario pierde acceso a sus propios secretos si el servicio sufre una interrupción, bloqueo de cuenta o discontinuación comercial.

**Modelo C — Bóvedas Offline Locales sin Sincronización Nativa (La Falsa Soberanía):**
Herramientas como KeePass ofrecen soberanía criptográfica real: el archivo cifrado vive en el dispositivo del usuario y ningún servidor externo posee las llaves. Sin embargo, fracasan estrepitosamente en el escenario operativo real del usuario moderno que trabaja desde múltiples dispositivos (PC personal, PC de trabajo, teléfono móvil). KeePass2 y herramientas similares:

- **Carecen de sincronización nativa integrada con la nube personal del usuario.** Para compartir la bóveda entre dispositivos, el usuario debe recurrir a procesos manuales (copiar el archivo por USB, correo o carpetas compartidas) o configurar servidores propios, lo cual impone una barrera técnica inaceptable para el usuario promedio.
- **No resuelven la concurrencia.** Si el usuario edita la bóveda desde su teléfono y desde su PC de trabajo simultáneamente (o estando desconectados durante horas/días), al sincronizar se enfrenta a conflictos irreconciliables que resultan en pérdida de datos o requieren resolución manual. El paradigma de "Última Escritura Gana" destruye silenciosamente los cambios del dispositivo más lento.
- **No ofrecen acceso de emergencia multiplataforma.** Un usuario que solo tiene acceso a un navegador web (ej. en un equipo prestado, un cibercafé o una situación de emergencia) no puede consultar su bóveda si no tiene la aplicación de escritorio instalada.

Este Modelo C es precisamente el punto de origen de WhiteLightPass: la frustración concreta de un usuario técnico que necesita soberanía total sobre sus credenciales, pero también necesita que su bóveda cifrada viaje transparente y automáticamente entre su PC personal, su PC de trabajo, su teléfono Android y ocasionalmente un navegador web — todo conectado al mismo archivo cifrado alojado en su propia cuenta de almacenamiento en la nube (como Google Drive), sin intermediarios, sin servidores propietarios y sin pérdida de datos ante ediciones concurrentes.

**El Vacío Estructural:**
No existe actualmente en el mercado un sistema de gestión de secretos que satisfaga simultáneamente estas seis exigencias operativas:

1. **Soberanía Total del Dato:** Que el usuario sea el único custodio criptográfico de su inventario, sin que ningún servidor remoto, proveedor o intermediario posea la capacidad técnica de leer, inferir o reconstruir los secretos almacenados. El archivo cifrado reside en la cuenta de almacenamiento en la nube personal del propio usuario (ej. Google Drive), no en servidores del fabricante de la aplicación.
2. **Sincronización Asíncrona Descentralizada vía Nube Personal:** Que múltiples dispositivos del mismo operador (escritorio, móvil, web) puedan operar desconectados entre sí durante periodos prolongados y, al reconectarse a través del archivo compartido en la nube personal, fusionar automáticamente los cambios divergentes sin pérdida de datos y sin requerir un árbitro central conectado.
3. **Disponibilidad Multiplataforma Real:** Que el usuario pueda acceder a su bóveda desde una aplicación de escritorio (Windows como plataforma inicial), una aplicación móvil nativa (Android) y una aplicación web accesible desde cualquier navegador, cubriendo todos los escenarios operativos cotidianos incluyendo situaciones de emergencia donde no se dispone de los dispositivos habituales.
4. **Opacidad Estructural Reforzada:** Que un observador pasivo o activo en la red de tránsito o en el proveedor de almacenamiento en la nube no pueda extraer información prácticamente útil sobre el volumen, la frecuencia de cambio, la naturaleza o los patrones de uso del inventario. El sistema emplea padding dinámico y flujo de cobertura temporal para mitigar agresivamente el análisis de tráfico y metadatos, aunque se reconoce que en escenarios extremos de crecimiento volumétrico masivo, alteraciones mínimas de tamaño pueden ser teóricamente detectables por un observador persistente (ver restricciones en el Documento de Definición del Sistema, Sección 4.2.4).
5. **Resiliencia Criptográfica Evolutiva:** Que el sistema sea capaz de migrar sus primitivas de cifrado y parámetros de derivación de forma autónoma a medida que el hardware y los algoritmos criptoanalíticos evolucionen, sin requerir una reingeniería completa del formato ni romper la compatibilidad con instancias existentes.
6. **Generación Autónoma de Entropía Segura:** Que el sistema provea mecanismos internos para generar credenciales de alta entropía criptográficamente segura, supliendo la incapacidad biológica del usuario para producir secretos verdaderamente impredecibles.

Este vacío representa el punto de dolor central: **el usuario digital moderno está matemáticamente desprotegido, comercialmente cautivo o funcionalmente aislado entre sus propios dispositivos**, y ninguna solución existente le ofrece simultáneamente soberanía, sincronización automática vía nube personal, disponibilidad multiplataforma, opacidad y resiliencia evolutiva.

### 1.2. El Costo de No Actuar

Mantener el *status quo* no es una posición neutral: es una deuda de seguridad compuesta que se acumula silenciosamente hasta que se materializa en un evento catastrófico. Los costos de no resolver el problema descrito se manifiestan en tres dimensiones concretas:

**A. Costo por Compromiso de Credenciales (Impacto Directo del Modelo A):**
La reutilización de contraseñas y el almacenamiento en texto plano convierten cada brecha de un servicio externo en un vector de acceso a todos los demás. Según datos consistentes de la industria de ciberseguridad, más del 80% de las brechas relacionadas con *hacking* involucran credenciales débiles o reutilizadas. Para el usuario individual, un compromiso en cadena puede significar:

- Pérdida de acceso a cuentas bancarias, correo electrónico principal y servicios críticos de identidad digital.
- Suplantación de identidad con consecuencias legales, financieras y reputacionales difíciles de revertir.
- Horas o días invertidos en procesos de recuperación de cuentas, cambio masivo de contraseñas y verificación de daños colaterales.

El costo no es hipotético: es estadísticamente inevitable a medida que el número de servicios digitales por usuario crece año tras año.

**B. Costo por Cautividad Comercial y Exposición Centralizada (Impacto del Modelo B):**
Delegar el inventario completo a un proveedor SaaS centralizado es apostar la totalidad de la vida digital a la continuidad, integridad y buena fe de una sola empresa. El historial reciente demuestra que esta apuesta falla:

- Brechas masivas en proveedores líderes de gestión de contraseñas han expuesto bóvedas cifradas de millones de usuarios, sometiendo sus *master passwords* a ataques de fuerza bruta offline de duración indefinida.
- Cambios unilaterales en modelos de precios, restricciones de plataforma o discontinuación de planes gratuitos fuerzan migraciones costosas en tiempo y riesgo de pérdida de datos.
- La concentración de secretos en infraestructura de terceros crea un objetivo de alto valor (*honeypot*) que atrae atacantes sofisticados y presiones gubernamentales de jurisdicciones extranjeras.

El costo real no es solo la suscripción mensual; es la pérdida irreversible de soberanía sobre el activo más sensible de uno: su identidad digital completa.

**C. Costo por Fricción Operativa y Pérdida de Datos (Impacto del Modelo C):**
Para el usuario técnico que ya eligió soberanía local (herramientas tipo KeePass), el costo de no actuar se paga en fricción diaria:

- **Tiempo perdido en sincronización manual:** Copiar archivos entre dispositivos, verificar versiones, resolver cuál es la más reciente. Este ritual se repite cada vez que el usuario cambia de dispositivo, acumulando minutos que a lo largo de meses se convierten en horas de productividad desperdiciada.
- **Pérdida silenciosa de datos por conflictos de concurrencia:** Editar la bóveda en el teléfono y en el PC durante el mismo periodo offline produce dos archivos divergentes. Sin un motor de resolución de conflictos, el usuario debe elegir uno y sacrificar los cambios del otro — o peor, sobrescribe sin darse cuenta. Cada contraseña perdida implica un proceso de recuperación de cuenta o, en el peor caso, pérdida permanente de acceso.
- **Inaccesibilidad en situaciones críticas:** El usuario que se encuentra fuera de sus dispositivos habituales (viaje, emergencia, equipo prestado) no tiene forma de acceder a sus credenciales. Esta barrera puede bloquear el acceso a servicios financieros, médicos o laborales en el momento en que más se necesitan.

**El Riesgo Compuesto:**
Estos tres vectores de costo no operan de forma aislada. En la práctica, uno termina oscilando entre los modelos: usa un gestor SaaS para la conveniencia pero "guarda aparte" las credenciales más sensibles en un archivo local; mantiene contraseñas memorizadas para servicios críticos y reutiliza variantes débiles para el resto. Esta fragmentación multiplica la superficie de ataque y diluye cualquier garantía de seguridad parcial que cada modelo ofrecía por separado.

**En síntesis:** cada día sin resolver este vacío estructural es un día en que se acumula deuda de seguridad exponencial. No se trata de *si* ocurrirá un compromiso, sino de *cuándo* — y cuando ocurra, el costo será desproporcionadamente mayor al esfuerzo que habría requerido una solución soberana, sincronizada y criptográficamente resiliente desde el inicio.

## 2. Definición del Mercado y Usuarios

### 2.1. Público Objetivo (Arquetipos Principales)

WhiteLightPass nace como una herramienta de uso personal. Su usuario primario es su propio creador: un desarrollador de software que trabaja diariamente alternando entre un PC personal, un PC corporativo y un teléfono Android, y que necesita acceso soberano a sus credenciales desde todos esos dispositivos — incluyendo ocasionalmente un navegador web en un equipo ajeno. La motivación de construirlo es resolver una necesidad propia que ninguna herramienta existente satisface completamente.

Esto no excluye que el producto sea útil para otros. Si la herramienta demuestra su valor en el uso cotidiano real, se compartirá abiertamente con quien enfrente el mismo problema, y si genera tracción e interés, se evaluará su lanzamiento al mercado. Pero la visión inicial es honesta: **el primer y principal usuario es el autor, y el producto se diseña desde esa realidad.**

Dicho esto, el perfil del usuario primario es representativo de un arquetipo más amplio que define a quiénes beneficiaría naturalmente el producto en una fase posterior de apertura:

**Usuario Primario — El Desarrollador Soberano (Perfil del Autor):**
Desarrollador de software que maneja un volumen alto de credenciales sensibles (accesos a servidores, APIs, repositorios, consolas de administración, cuentas personales) distribuidas entre su vida profesional y personal. Trabaja diariamente desde al menos tres dispositivos (PC personal, PC de trabajo, teléfono Android). Es un ex-usuario de KeePass2 que entiende y valora la soberanía criptográfica sobre sus datos, pero está frustrado por la incapacidad de KeePass de sincronizar nativamente su bóveda a través de su propia cuenta de Google Drive sin conflictos de concurrencia. Posee la competencia técnica para valorar decisiones de diseño criptográfico y la exigencia de no delegar sus llaves a ningún tercero.

**Usuarios Potenciales en Fase de Apertura (Futuro Condicional):**
Si el producto se abre al público, los perfiles que se beneficiarían de forma natural son:

- **Profesionales técnicos multi-dispositivo:** Administradores de sistemas, ingenieros DevOps y profesionales de TI con necesidades similares al usuario primario — soberanía, sincronización y multiplataforma.
- **Usuarios conscientes de su privacidad:** Profesionales (periodistas, abogados, académicos) que manejan información confidencial y buscan alternativas que no dependan de proveedores centralizados.
- **Entusiastas del self-hosting y open-source:** Usuarios que ya gestionan su almacenamiento en la nube personal y prefieren herramientas que se integren con su ecosistema existente sin imponer cuentas propietarias adicionales.

### 2.2. Tamaño de la Oportunidad (Mercado)

WhiteLightPass no nace como un producto de mercado. Nace como una herramienta personal para resolver un problema real y concreto de su autor. Por tanto, no se justifica por proyecciones de mercado ni por captación de usuarios, sino por el valor directo que entrega a quien lo construye.

**Justificación Primaria — Valor Personal:**
La inversión en el desarrollo se justifica porque el autor usará activamente el producto todos los días, en todos sus dispositivos, para gestionar la totalidad de sus credenciales digitales. El retorno no es financiero: es la eliminación permanente de la fricción de sincronización, la recuperación de la soberanía sobre sus secretos y la certeza criptográfica de que ningún tercero puede acceder a su bóveda. Cada hora invertida en desarrollo se recupera acumulativamente en seguridad, productividad y tranquilidad operativa a lo largo de años de uso.

**Contexto de Oportunidad (Horizonte Futuro Condicional):**
Si el producto demuestra su solidez en el uso personal sostenido y se decide compartirlo o abrirlo al público, el contexto de mercado en el que entraría es favorable:

- El mercado de gestores de contraseñas está en expansión sostenida, impulsado por el crecimiento de identidades digitales por persona, brechas de alto perfil en proveedores centralizados y una creciente cultura de soberanía digital.
- Existe un segmento específico de usuarios insatisfechos que ya eligió la soberanía local (la comunidad activa de KeePass y sus forks — KeePassXC, KeePassDX) pero carece de sincronización nativa multiplataforma a través de la nube personal. Este es exactamente el vacío que WhiteLightPass resuelve.
- El modelo operativo de WhiteLightPass (sin servidores propios, almacenamiento delegado al usuario) elimina los costos de infraestructura recurrentes que lastran a los productos SaaS, haciendo viable su distribución incluso como herramienta gratuita u open-source.

Este contexto no se presenta como una promesa de negocio, sino como un dato objetivo: si en algún momento se decide dar el paso, el espacio existe y el producto llega con una propuesta diferenciada real.

## 3. La Solución (Propuesta de Valor)

### 3.1. Elevator Pitch (Declaración de Posicionamiento)

- **Para:** Usuarios técnicos multi-dispositivo que gestionan decenas o cientos de credenciales digitales entre su vida personal y profesional.
- **Que tienen:** La necesidad de acceder a sus contraseñas desde su PC personal, su PC de trabajo, su teléfono Android y ocasionalmente un navegador web, sin delegar la custodia de sus secretos a ningún proveedor externo y sin perder datos cuando editan desde dispositivos desconectados entre sí.
- **Nuestro producto es:** Un gestor de contraseñas multiplataforma (escritorio, móvil y web) con cifrado local soberano y sincronización automática a través de la nube personal del usuario.
- **Que proporciona:** Soberanía total sobre las credenciales, fusión automática de cambios concurrentes sin pérdida de datos, y acceso desde cualquier dispositivo — todo sin servidores propietarios, sin suscripciones y sin que ningún tercero pueda leer la bóveda.
- **A diferencia de:** Gestores SaaS centralizados (1Password, LastPass, Dashlane) que custodian los secretos en su infraestructura, y bóvedas offline (KeePass2) que ofrecen soberanía pero carecen de sincronización nativa, resolución de conflictos y acceso web.
- **Nosotros:** Almacenamos el archivo cifrado directamente en la cuenta de Google Drive del usuario, resolvemos conflictos de edición concurrente mediante un motor CRDT descentralizado, y garantizamos que ni la aplicación, ni Google Drive, ni ningún intermediario posean capacidad técnica de descifrar la bóveda.

### 3.2. Pilares de Valor

WhiteLightPass se diferencia de las alternativas existentes sobre cuatro pilares fundamentales:

**Pilar 1 — Soberanía sin Fricción (Tu Nube, Tus Llaves):**
A diferencia de los SaaS que custodian los secretos en su infraestructura, y a diferencia de KeePass que obliga al usuario a gestionar manualmente la distribución del archivo, WhiteLightPass conecta directamente con la cuenta de almacenamiento en la nube personal del usuario (Google Drive como integración primaria). El archivo cifrado vive donde el usuario ya almacena sus cosas, bajo su propia cuenta y su propia cuota. No se introduce ningún servidor intermedio, ninguna cuenta nueva, ningún middleman. La soberanía no se promete como política de privacidad: se garantiza como imposibilidad técnica de acceso por parte de terceros.

**Pilar 2 — Sincronización Inteligente sin Pérdida de Datos (Motor CRDT):**
Es el diferenciador técnico más profundo. Cuando dos dispositivos editan la bóveda de forma independiente estando desconectados — escenario cotidiano y real — el sistema no fuerza al usuario a elegir cuál versión conservar ni aplica "Última Escritura Gana". Un motor de resolución de conflictos basado en CRDTs (Conflict-free Replicated Data Types) fusiona automáticamente las líneas temporales divergentes a nivel de operaciones individuales, preservando cada cambio de cada dispositivo. El usuario recupera una bóveda unificada sin intervención manual y sin pérdida silenciosa de datos.

**Pilar 3 — Disponibilidad Multiplataforma Completa (Escritorio + Móvil + Web):**
Una sola bóveda cifrada accesible desde tres superficies: aplicación de escritorio para el uso diario en PCs, aplicación móvil nativa (Android) para el acceso en movimiento, y aplicación web para situaciones de emergencia o equipos ajenos donde no se tiene la aplicación instalada. Las tres plataformas operan sobre el mismo archivo cifrado en la nube personal, eliminando la fragmentación que obliga a los usuarios de KeePass a mantener copias manuales o renunciar a alguno de sus dispositivos.

**Pilar 4 — Cero Infraestructura Propietaria (Costo Operativo Nulo):**
WhiteLightPass no requiere servidores propios para funcionar. Todo el almacenamiento y la transferencia de datos se delegan a la infraestructura de nube que el usuario ya posee y paga. Esto elimina los costos recurrentes de hosting, ancho de banda y custodia de datos que lastran a los gestores SaaS, y significa que el producto puede existir indefinidamente sin dependencia de suscripciones, sin riesgo de discontinuación por inviabilidad económica del proveedor, y sin el dilema ético de monetizar los datos del usuario para sostener la operación.

## 4. Éxito y Viabilidad Comercial

### 4.1. Objetivos a Corto y Mediano Plazo

WhiteLightPass no tiene objetivos de negocio en el sentido comercial tradicional. Es un proyecto personal con horizontes orientados a la utilidad propia y, condicionalmente, a la apertura comunitaria.

**Corto Plazo — Fase 1 (0–3 meses) — Escritorio como Plataforma Primaria:**

1. **Entregar la aplicación de escritorio (Windows) como primer cliente funcional.** Es la plataforma de uso más intensivo del autor (PC personal y PC de trabajo), por lo que concentra el mayor retorno de valor inmediato. Debe alcanzar paridad funcional mínima con KeePass2 en las operaciones esenciales: crear, leer, editar, eliminar y buscar credenciales; generar contraseñas de alta entropía; autocompletar o copiar al portapapeles con caducidad temporal.
2. **Implementar el formato del Contenedor Criptográfico y el motor CRDT** como núcleo compartido, validando la integridad de la serialización, el cifrado y la fusión de Deltas antes de extender a otras plataformas.
3. **Integrar la sincronización con Google Drive** para que la bóveda se suba y descargue automáticamente tras cada operación de guardado, estableciendo el flujo de trabajo que heredarán los clientes posteriores.

**Corto Plazo — Fase 2 (3–6 meses) — Extensión Móvil:**

1. **Entregar la aplicación móvil nativa (Android) como segundo cliente funcional.** Es la segunda plataforma de uso más frecuente del autor. Debe poder abrir, consultar, editar y sincronizar la misma bóveda que el cliente de escritorio, sin requerir conversión de formato.
2. **Validar la estabilidad del motor CRDT en escenarios reales de concurrencia** entre escritorio y móvil: ediciones simultáneas desde dos dispositivos desconectados durante horas, seguidas de sincronización exitosa sin pérdida ni duplicación de datos.
3. **Reemplazar KeePass2 por completo en la rutina diaria del autor.** Al completar esta fase, el producto debe gestionar la totalidad del inventario de credenciales personales y profesionales desde ambos dispositivos (escritorio + Android), sincronizando a través de Google Drive sin intervención manual. KeePass2 deja de abrirse.

**Mediano Plazo (6–18 meses) — Web, Consolidación y Apertura Condicional:**

1. **Incorporar el cliente web** como tercer punto de acceso, completando la disponibilidad multiplataforma para cubrir situaciones de emergencia o equipos ajenos.
2. **Someter el formato criptográfico y la implementación del motor CRDT a una auditoría de seguridad** (puede ser autoauditoría rigurosa o revisión por pares en comunidades técnicas) para validar la integridad de las invariantes definidas en el Documento de Definición del Sistema.
3. **Evaluar la apertura al público.** Si el producto ha demostrado solidez en el uso personal sostenido y existe interés de terceros, compartirlo como herramienta open-source o de distribución libre. Esta decisión no está comprometida de antemano: depende exclusivamente de la madurez y confianza que el producto haya alcanzado en el uso real.

### 4.2. Indicadores Clave de Rendimiento (KPIs)

Al tratarse de un producto de uso personal, los KPIs no miden adquisición de clientes ni retorno financiero. Miden si el producto cumple su razón de existir: **reemplazar completamente a KeePass2 como gestor de contraseñas diario, con soberanía, sincronización y disponibilidad multiplataforma.**

**KPI 1 — Adopción Personal Completa:**

- *Métrica:* Porcentaje de credenciales del autor migradas a WhiteLightPass y gestionadas exclusivamente desde él.
- *Objetivo:* 100% de las credenciales activas migradas. KeePass2 deja de abrirse.
- *Señal de fracaso:* Si después de 3 meses de uso el autor sigue recurriendo a KeePass2 o a otro gestor para algún subconjunto de credenciales, el producto no ha alcanzado paridad funcional suficiente.

**KPI 2 — Tasa de Resolución Limpia de Conflictos (TRL):**

- *Métrica:* Porcentaje de sincronizaciones concurrentes entre dos o más dispositivos que se resuelven automáticamente sin intervención manual ni pérdida de datos.
- *Objetivo:* ≥ 99.9% en escenarios de uso cotidiano real (ediciones desde PC y teléfono durante el mismo día de trabajo, con periodos de desconexión de horas).
- *Señal de fracaso:* Si más de 1 de cada 1,000 sincronizaciones requiere intervención manual o produce duplicados/pérdida, el motor CRDT necesita revisión.

**KPI 3 — Cobertura de Dispositivos Activos:**

- *Métrica:* Número de dispositivos desde los cuales el autor usa activamente WhiteLightPass en una semana típica.
- *Objetivo:* 3 dispositivos (PC personal, PC de trabajo, Android). En mediano plazo, 4 (sumando acceso web ocasional).
- *Señal de fracaso:* Si alguno de los dispositivos objetivo queda excluido del flujo diario (ej. el teléfono no se usa porque la experiencia es deficiente), la promesa multiplataforma no se cumple.

**KPI 4 — Fricción de Sincronización Percibida:**

- *Métrica:* Número de veces por semana en que el autor debe realizar alguna acción manual para que la sincronización funcione (forzar descarga, resolver conflicto, reiniciar la app).
- *Objetivo:* 0 intervenciones manuales por semana. La sincronización debe ser invisible.
- *Señal de fracaso:* Si en una semana típica se requiere más de una acción manual de sincronización, el flujo no es transparente y el usuario volverá a la inercia del Modelo C.

## 5. Fronteras de Alcance (Lo que NO es el producto)

### 5.1. Anti-Objetivos

Las siguientes declaraciones definen las fronteras explícitas e innegociables del producto. Cualquier solicitud, idea o feature que caiga dentro de estos límites debe ser rechazada sin debate, independientemente de su atractivo percibido:

1. **WhiteLightPass NO es un gestor de contraseñas empresarial ni colaborativo.** No se diseñarán funcionalidades de bóvedas compartidas entre equipos, roles de acceso, administración centralizada de políticas de contraseñas ni auditoría corporativa. Es una herramienta de soberanía individual. Un solo usuario, una sola bóveda, múltiples dispositivos propios.

2. **WhiteLightPass NO es un proveedor de almacenamiento en la nube.** El producto no almacena, hospeda ni transmite el archivo cifrado del usuario a través de servidores propios. Toda persistencia remota se delega al proveedor de nube personal del usuario (Google Drive como integración primaria). Si el proveedor de nube del usuario falla, WhiteLightPass no es responsable de la disponibilidad del archivo.

3. **WhiteLightPass NO es un servicio de identidad ni un autenticador.** No gestiona tokens TOTP/2FA, no actúa como proveedor de identidad (IdP), no implementa protocolos de SSO (Single Sign-On) ni emite certificados. Su alcance se limita estrictamente al almacenamiento, generación y recuperación de secretos estáticos (contraseñas, notas cifradas, claves API).

4. **WhiteLightPass NO es compatible con el formato KDBX ni con KeePass.** Aunque el diseño se informó mediante ingeniería inversa de la arquitectura KDBX, el producto define su propio formato de contenedor criptográfico con un motor CRDT nativo. No se implementará importación/exportación nativa de archivos `.kdbx` como requisito de primera clase. Si en el futuro se añade como utilidad de migración, será una herramienta auxiliar, no una garantía de compatibilidad bidireccional permanente.

5. **WhiteLightPass NO ofrecerá sincronización por canales alternativos ni P2P directo.** Aunque el sistema abstracto definido en el Documento de Definición del Sistema es arquitectónicamente agnóstico al medio de transporte (principio de Equifinalidad), WhiteLightPass como implementación concreta de ese sistema restringe deliberadamente su Canal Simétrico Ciego a la nube personal del usuario (Google Drive como integración primaria). No se construirán protocolos de sincronización directa entre dispositivos (Bluetooth, LAN, WebRTC peer-to-peer), ni soporte para medios extraíbles (USB), ni un relay server centralizado.

6. **WhiteLightPass NO implementará extensiones de navegador en su ciclo inicial.** El acceso web se proveerá mediante una aplicación web dedicada (PWA o webapp). No se desarrollarán extensiones nativas para Chrome, Firefox u otros navegadores como parte del alcance previsible. El autocompletado en navegador no es un objetivo de primera clase.

7. **WhiteLightPass NO es un producto iOS.** La plataforma móvil objetivo es Android. No se contempla desarrollo para iOS en el ciclo de vida previsible del producto. Si la apertura futura lo justifica, se evaluará como un proyecto independiente.
