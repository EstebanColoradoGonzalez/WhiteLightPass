# Diccionario de Datos y Semántica de la Información

## 1. Propósito

Este documento define la estructura atómica y el significado de negocio de cada unidad de información que el sistema almacena. Su objetivo es eliminar cualquier ambigüedad técnica o funcional sobre los datos en reposo, estableciendo los tipos, restricciones y el propósito exacto de cada campo dentro de las entidades previamente mapeadas en el Modelo Entidad-Relación (MER). Actúa como el contrato estricto entre el diseño lógico y la implementación física en la base de datos.

## 2. Estándares y Tipos de Datos (Dominios Base)

*Define las convenciones universales aplicadas en todo el documento para evitar redundancias, estandarizando cómo se representa la información repetitiva.*

* **Manejo de Tiempos y Fechas:** `[Definir la norma absoluta del sistema, ej. Todas las fechas se almacenarán en UTC bajo la norma ISO 8601]`.
* **Manejo de Estados Lógicos (Booleanos):** `[Definir cómo se representa la dualidad, ej. 0 para falso y 1 para verdadero, o TRUE/FALSE]`.
* **Manejo de Valores Monetarios o de Alta Precisión:** `[Definir el tipo de dato estricto para evitar pérdidas por redondeo, ej. Uso exclusivo de DECIMAL o almacenamiento en centavos]`.

## 3. Catálogo Detallado de Entidades

*Esta es la sección central del documento. Por cada entidad abstracta definida en el MER, se debe detallar su anatomía interna.*

### 3.1. [Nombre de la Entidad / Tabla]

* **Descripción Semántica:** `[Breve explicación de qué representa una fila individual o documento dentro de esta entidad para el negocio]`.

| Atributo / Columna | Tipo de Dato Lógico (y Tamaño) | Restricciones y Naturaleza de Llave | Valor por Defecto | Descripción Semántica y Reglas de Negocio |
| :--- | :--- | :--- | :--- | :--- |
| `[Nombre exacto del campo]` | `[Ej. Entero, Texto(255), Decimal(10,2)]` | `[Ej. PK, FK, Obligatorio (NOT NULL), Único]` | `[Ej. Nulo, Fecha del sistema, 'Activo']` | `[Qué significa exactamente este dato. Si es una FK, indicar a qué entidad apunta. Si el campo tiene lógica de cálculo previa, explicarla brevemente]`.
| `[Nombre del campo]` | `[Tipo]` | `[Restricciones]` | `[Defecto]` | `[Descripción detallada...]` |

*(Nota: Replicar la subsección 3.1 para todas las entidades operativas del sistema).*

## 4. Dominios Cerrados y Estados (Enums / Catálogos Fijos)

*Define los conjuntos de valores estáticos y predefinidos que limitan la entrada de datos en ciertos atributos, asegurando la consistencia de la información (homeostasis).*

### 4.1. [Nombre del Dominio o Conjunto de Estados]

* **Atributos que lo consumen:** `[Listar las entidades y columnas de la sección 3 que utilizan este conjunto de valores]`.

| Código / Valor Físico | Etiqueta de Presentación | Significado Lógico / Implicación en el Sistema |
| :--- | :--- | :--- |
| `[Ej. Valor exacto en BD]` | `[Ej. Cómo se lee en la interfaz]` | `[Explicación de qué significa este estado o clasificación y qué comportamientos habilita o bloquea en el sistema]` |
| `[Valor]` | `[Etiqueta]` | `[Significado]` |

## 5. Datos Semilla (Condiciones de Inicialización)

*Define la información que debe existir obligatoriamente en la base de datos en el momento exacto del despliegue del sistema (el "Big Bang" de la memoria).*

* **[Nombre de la Entidad Afectada]:** `[Explicación de los registros iniciales que deben pre-cargarse. Ej. Un usuario administrador raíz, un listado de países base, o configuraciones maestras]`.

## 6. Retención de Información y Volumen (Termodinámica del Dato)

*Establece las reglas de envejecimiento y depuración de la información para evitar el colapso de la memoria a lo largo del tiempo.*

* **Tasa de Crecimiento Esperada:** `[Estimación del volumen de registros que se generarán por ciclo operativo o marco temporal]`.
* **Políticas de Depuración (Purge / Archiving):** `[Definir si hay datos temporales que el sistema debe borrar automáticamente después de cierto tiempo para mantener la eficiencia, ej. Tokens de sesión expiran en 24h, registros de auditoría se archivan a los 5 años]`.
