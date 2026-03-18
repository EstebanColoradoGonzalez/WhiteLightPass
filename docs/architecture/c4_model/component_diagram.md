# Diagrama de Componentes (Arquitectura Interna del Contenedor)

## 1. Propósito

Este documento realiza un acercamiento microscópico al interior de un Contenedor específico (definido previamente en el Diagrama de Arquitectura de Software). Su objetivo es identificar los bloques lógicos de código (componentes) que residen dentro de esta unidad desplegable, definir las responsabilidades aisladas de cada bloque y mapear cómo se acoplan entre sí para cumplir con las capacidades funcionales del sistema. Sirve como el plano de distribución para que los ingenieros estructuren el repositorio de código.

## 2. Convenciones de Notación y Acoplamiento

*Establece el estándar visual utilizado para representar los módulos de código y la direccionalidad de sus dependencias.*

* **Componentes Lógicos:** `[Regla visual para identificar el bloque de código, indicando su nombre y responsabilidad principal]`.
* **Interfaces / Puertos:** `[Regla visual para representar los puntos de acceso estructurados que un componente expone para que otros interactúen con él]`.
* **Dependencias (Acoplamiento):** `[Regla visual para las flechas que indican qué componente requiere o consume a otro componente para poder funcionar]`.

## 3. Alcance del Diagrama (Contenedor Objetivo)

*Declaración estricta de la unidad que se está analizando. (Nota: Si el sistema tiene múltiples contenedores complejos, este documento debe replicarse para cada uno).*

* **Contenedor Analizado:** `[Identificador y nombre del contenedor listado en el paso anterior]`.
* **Frontera de Ejecución:** Todo componente listado en este documento se ejecuta dentro del mismo espacio de memoria o proceso físico definido por este contenedor.

## 4. Diagrama Visual de Componentes

*Representación gráfica que muestra los bloques internos del contenedor, cómo se interconectan entre sí y qué componentes se comunican directamente con elementos externos (bases de datos u otros contenedores).*

`[Espacio para insertar el diagrama visual exportado desde la herramienta de modelado]`

## 5. Inventario Ontológico de Componentes

*Desglose textual de cada bloque lógico dibujado en el diagrama. Define el límite estricto de responsabilidad de cada pieza de código.*

| ID Componente | Nombre del Módulo Lógico | Responsabilidad Central (El "Por qué" existe) | Interfaces Expuestas (Contratos de acceso) |
| :--- | :--- | :--- | :--- |
| `[Identificador]` | `[Nombre descriptivo del bloque]` | `[Descripción de la única tarea lógica o regla de negocio de la que es responsable este componente]` | `[Qué canales o métodos abstractos ofrece al resto del sistema]` |
| `[Identificador]` | `[Nombre]` | `[Responsabilidad]` | `[Interfaces]` |

## 6. Matriz de Dependencias (Flujos de Control Interno)

*Mapeo de la red de acoplamiento. Define cómo los componentes se delegan tareas mutuamente para completar un flujo de información.*

| Componente Origen (Consumidor) | Componente Destino (Proveedor) | Propósito de la Delegación |
| :--- | :--- | :--- |
| `[ID Componente que inicia la acción]` | `[ID Componente requerido]` | `[Explicación de qué proceso lógico o dato le solicita el origen al destino]` |
| `[Origen]` | `[Destino]` | `[Propósito]` |

## 7. Interacciones con el Límite Externo

*Identificación de los componentes específicos (puertas de enlace) que tienen el permiso y la responsabilidad de comunicarse con elementos fuera de este contenedor (ej. bases de datos, APIs externas, interfaces de usuario).*

| Componente Fronterizo | Entidad Externa (del Contexto/Contenedor) | Naturaleza de la Integración |
| :--- | :--- | :--- |
| `[ID del componente interno]` | `[Nombre del sistema, contenedor o base de datos externa]` | `[Cómo este componente traduce o transporta la información hacia el exterior]` |
| `[Fronterizo]` | `[Externa]` | `[Integración]` |

## 8. Trazabilidad Funcional (Componentes vs. Historias de Usuario)

*Matriz de validación que garantiza que toda la lógica de negocio requerida tiene un órgano encargado de ejecutarla.*

| ID Historia de Usuario / Requerimiento | Componente(s) Principal(es) Responsable(s) de la Lógica |
| :--- | :--- |
| `[Identificador de la HU mapeada en el Paso 5]` | `[ID del componente o componentes que procesarán las reglas de negocio descritas en esa historia]` |
