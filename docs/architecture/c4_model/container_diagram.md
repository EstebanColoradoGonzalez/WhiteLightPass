# Diagrama de Arquitectura de Software (Nivel de Contenedores)

## 1. Propósito

Este documento "abre" la entidad central definida previamente en el Diagrama de Contexto para exponer su arquitectura macroscópica. Su objetivo es identificar las unidades de software y almacenamiento que se ejecutan o despliegan de forma independiente (contenedores), las responsabilidades aisladas de cada una de estas unidades y las tecnologías principales que las soportan. Establece la topología técnica sobre la cual interactúan los agentes externos y cómo fluye la información internamente.

## 2. Convenciones de Notación Arquitectónica

*Define el lenguaje visual utilizado para diferenciar la naturaleza técnica de cada bloque dentro del límite del sistema.*

* **El Límite del Sistema:** `[Regla visual para delimitar la frontera que encierra a todos los contenedores propios, separándolos de los actores y sistemas externos heredados del Diagrama de Contexto]`.
* **Contenedores de Almacenamiento:** `[Regla visual para identificar unidades cuyo propósito principal es la retención de datos, ej. forma cilíndrica]`.
* **Contenedores de Ejecución/Lógica:** `[Regla visual para identificar unidades de procesamiento o interfaces de usuario, ej. cajas rectangulares]`.
* **Conexiones y Flujos:** `[Regla visual para las flechas que conectan contenedores, indicando la dirección de la dependencia o del flujo de red]`.

## 3. Diagrama de Contenedores (Visualización Topológica)

*Representación gráfica que muestra el límite del sistema, los actores externos, los sistemas de terceros, y los contenedores internos interactuando entre sí.*

`[Espacio para insertar el diagrama de arquitectura exportado desde la herramienta de modelado]`

## 4. Inventario de Contenedores (Unidades Desplegables)

*Listado exhaustivo de las piezas de software independientes que conforman el sistema. Por cada contenedor, se debe definir su naturaleza técnica y su responsabilidad aislada.*

| ID Contenedor | Nombre de la Unidad | Naturaleza Técnica y Plataforma | Responsabilidad Central |
| :--- | :--- | :--- | :--- |
| `[Identificador único]` | `[Nombre descriptivo]` | `[Clasificación arquitectónica y tecnología base elegida para su construcción o ejecución]` | `[Descripción estricta de la función que cumple este contenedor dentro de la homeostasis del sistema]` |
| `[Identificador]` | `[Nombre]` | `[Clasificación tecnológica]` | `[Responsabilidad]` |

*(Nota: La justificación de las decisiones tecnológicas indicadas en la columna "Naturaleza Técnica" deberá registrarse formalmente en los Registros de Decisiones Arquitectónicas - ADR).*

## 5. Matriz de Interacciones y Protocolos (Flujos de Red)

*Define matemáticamente cómo se comunican las distintas unidades entre sí y con el entorno. Todo flujo dibujado en el diagrama debe tener su contraparte en esta matriz.*

| Nodo Origen | Nodo Destino | Propósito de la Interacción | Protocolo y Estilo de Comunicación | Sincronía |
| :--- | :--- | :--- | :--- | :--- |
| `[ID Contenedor o Actor Externo]` | `[ID Contenedor o Sistema Externo]` | `[Qué datos se transfieren o qué comando se ejecuta]` | `[Mecanismo tecnológico de transmisión o capa de transporte]` | `[Especificar si la comunicación exige respuesta inmediata o es diferida/basada en eventos]` |
| `[Origen]` | `[Destino]` | `[Propósito]` | `[Protocolo]` | `[Sincronía]` |

## 6. Mapeo de Persistencia (Contenedores vs. Entidades)

*Conecta este nivel arquitectónico con el Modelo Entidad-Relación (MER) definido previamente, estableciendo qué unidad física resguarda qué segmento de la memoria lógica.*

| ID Contenedor de Almacenamiento | Dominio de Datos (Subconjunto del MER) | Naturaleza de la Persistencia |
| :--- | :--- | :--- |
| `[Identificador del contenedor de datos listado en la sección 4]` | `[Referencia a las entidades lógicas o agrupaciones del MER que residen físicamente aquí]` | `[Tipo de motor o paradigma de retención de la información]` |

## 7. Despliegue y Topología Física Básica (Opcional a este nivel)

*Declaración de alto nivel sobre la estrategia de alojamiento de estos contenedores, estableciendo las restricciones físicas que afectarán el diseño interno posterior.*

* **Estrategia de Alojamiento:** `[Descripción genérica de dónde vivirán estos contenedores en el mundo real, indicando aislamiento, distribución geográfica o centralización]`.
