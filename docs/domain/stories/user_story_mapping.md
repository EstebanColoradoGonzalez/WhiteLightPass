# Mapa de Historias de Usuario (User Story Map)

## 1. Propósito y Trazabilidad

Este documento traduce las *Capacidades Funcionales* a un flujo cronológico de interacción. Garantiza la trazabilidad bidireccional: ninguna historia existe sin un requisito que la justifique, y ningún requisito queda sin un vector de implementación.

## 2. El Viaje del Agente (Secuencia Base)

*Define el orden cronológico estricto en el que ocurre la interacción con las fronteras del sistema. Esta secuencia secuencial es la columna vertebral que dictará la futura arquitectura de interfaces o navegación.*

1. **[Fase de Entrada / Inicio]:** [Breve descripción de la interacción inicial o validación]
2. **[Fase de Transición / Operación A]:** [Breve descripción de la etapa intermedia en el flujo]
3. **[Fase de Transición / Operación B]:** [Breve descripción de la etapa intermedia en el flujo]
4. **[Fase de Cierre / Salida]:** [Breve descripción de la culminación del ciclo de interacción]

## 3. Matriz del Mapa de Historias

*Por cada Fase del viaje definida en la sección anterior, se desglosan las historias individuales ordenadas verticalmente por prioridad de entrega temporal.*

| Fase del Viaje (Secuencia) | ID Historia | Título Breve de la Historia (Acción y Valor) | RF Vinculados | RNF Específicos |
| :--- | :--- | :--- | :--- | :--- |
| **1. [Nombre Fase de Entrada]** | HU-01 | [Acción específica que realiza el agente] | [ID-RF] | [ID-RNF] |
| | HU-02 | [Acción específica que realiza el agente] | [ID-RF] | --- |
| **2. [Nombre Fase Operación A]** | HU-03 | [Acción específica que realiza el agente] | [ID-RF] | [ID-RNF] |
| | HU-04 | [Acción específica que realiza el agente] | [ID-RF] | [ID-RNF] |

## 4. Requerimientos No Funcionales Transversales (Implícitos)

*Lista de restricciones operativas o invariantes del sistema que NO se asignan a una historia específica porque permean toda la estructura. Serán Criterios de Aceptación implícitos en la validación de todas las historias.*

* **[ID-RNF Transversal]:** [Descripción de la restricción global aplicable a todo el sistema]
* **[ID-RNF Transversal]:** [Descripción de la restricción global aplicable a todo el sistema]
* **[ID-RNF Transversal]:** [Descripción de la restricción global aplicable a todo el sistema]

## 5. Criterios de Liberación (Releases / Slices)

*Define las fronteras de entrega, agrupando las historias de la matriz en incrementos de valor funcional.*

### 5.1. Iteración Base (MVP / Release 1)

* **Objetivo de la Liberación:** [Declaración del propósito de valor principal de esta iteración]
* **Historias incluidas:** [Lista de IDs de Historias de Usuario requeridas]

### 5.2. Iteración de Expansión (Release 2)

* **Objetivo de la Liberación:** [Declaración del propósito de valor secundario o escalamiento]
* **Historias incluidas:** [Lista de IDs de Historias de Usuario requeridas]
