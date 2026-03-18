# Diagrama de Contexto del Sistema (Visión Macro y Fronteras)

## 1. Propósito

Este documento establece los límites absolutos del sistema dentro de su ecosistema operativo. Trata al software a construir como una única entidad indivisible (caja negra) con el fin de mapear exclusivamente las interacciones que ocurren en sus fronteras: los actores (humanos o lógicos) que lo operan y los sistemas externos de los que depende o a los que provee información. Su objetivo es aislar la responsabilidad de nuestro sistema frente al mundo exterior.

## 2. Convenciones de Notación

*Define el estándar visual utilizado para diferenciar claramente qué está bajo nuestro control y qué pertenece al entorno.*

* **El Sistema Central:** `[Regla visual, ej. Una única caja azul en el centro del lienzo]`. Representa la totalidad del producto a construir.
* **Actores / Usuarios:** `[Regla visual, ej. Siluetas humanas o cajas con un ícono de usuario]`. Representa a los operadores humanos o roles funcionales.
* **Sistemas Externos:** `[Regla visual, ej. Cajas grises o con bordes punteados]`. Representa software, hardware o servicios de terceros que no podemos modificar, pero con los que debemos interactuar.
* **Flujos de Interacción:** `[Regla visual, ej. Flechas direccionales con etiquetas descriptivas]`. Representan el movimiento de información o el envío de comandos.

## 3. Diagrama de Contexto Visual

*Representación gráfica de alto nivel mostrando el sistema central rodeado de sus actores y dependencias externas, unidos por flujos de interacción.*

`[Espacio para insertar el diagrama visual exportado]`

## 4. Inventario del Ecosistema (Nodos Externos)

*Desglose textual de todos los elementos que rodean al sistema central en el diagrama. No se documentan piezas internas.*

### 4.1. Actores (Agentes Operativos)

*Lista de los perfiles que inyectan entropía (acciones) o consumen resultados del sistema.*

| Nombre del Actor / Rol | Descripción Semántica | Naturaleza de la Interacción |
| :--- | :--- | :--- |
| `[Nombre del Rol]` | `[Breve descripción de quién es o qué función cumple este agente en el mundo real]` | `[Ej. Inyecta configuraciones maestras, consume reportes analíticos, opera el flujo diario]` |
| `[Nombre del Rol]` | `[Descripción...]` | `[Naturaleza...]` |

### 4.2. Sistemas Externos (Dependencias y Consumidores)

*Lista de los motores, servicios, normativas o hardware fuera de nuestra jurisdicción con los que el sistema debe obligatoriamente integrarse.*

| Nombre del Sistema Externo | Descripción y Propiedad | Naturaleza de la Dependencia |
| :--- | :--- | :--- |
| `[Nombre del Sistema A]` | `[Qué hace y a quién pertenece este sistema, ej. Sistema contable heredado de la empresa matriz]` | `[Por qué dependemos de él, ej. Valida la identidad corporativa, procesa la transacción final]` |
| `[Nombre del Sistema B]` | `[Descripción...]` | `[Naturaleza...]` |

## 5. Matriz de Flujos e Intercambio de Información

*Detalle normativo de las flechas (conexiones) dibujadas en el diagrama visual. Define exactamente qué viaja a través de las fronteras del sistema.*

| Origen | Destino | Etiqueta del Flujo (Acción/Dato) | Protocolo o Canal Asumido | Regla de Resiliencia / Falla |
| :--- | :--- | :--- | :--- | :--- |
| `[Ej. Actor Humano]` | `[Nuestro Sistema]` | `[Qué acción realiza o qué información envía]` | `[Medio abstracto, ej. Interfaz gráfica interactiva]` | `[Comportamiento esperado si la acción es rechazada]` |
| `[Nuestro Sistema]` | `[Sistema Externo B]` | `[Qué carga útil de información se envía para procesamiento externo]` | `[Medio técnico abstracto, ej. Llamada síncrona a API, Transferencia asíncrona de archivos]` | `[Comportamiento del sistema si el nodo externo está caído (Time-out)]` |
| `[Sistema Externo C]` | `[Nuestro Sistema]` | `[Qué notificación o dato nos inyecta de forma autónoma]` | `[Medio, ej. Webhook o señal de hardware]` | `[Mecanismo de absorción de la anomalía]` |

## 6. Exclusiones Explícitas (Anti-Alcance de Integración)

*Declaración de los sistemas o actores del entorno con los que nuestro sistema asume formalmente que NO se integrará, previniendo la corrupción del alcance arquitectónico.*

* `[Nombre de un sistema o actor cercano al ecosistema]` - **Justificación de Exclusión:** `[Por qué se ha decidido arquitectónicamente aislar nuestro sistema de esta entidad]`.
