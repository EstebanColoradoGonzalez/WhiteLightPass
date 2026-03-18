# Documento de Estándares y Convenciones de Código

## 1. Propósito y Alcance general

Este documento establece las reglas obligatorias y las prácticas recomendadas para la escritura de código fuente en el proyecto. Su propósito es garantizar la legibilidad, la mantenibilidad y la uniformidad del código, mitigando la entropía técnica generada por los diferentes estilos de programación de los miembros del equipo. Este manual debe ser la principal referencia durante las revisiones de código (*Code Reviews*).

## 2. Estándares Obligatorios de Nomenclatura (Naming Conventions)

*Define las reglas estrictas sobre cómo se deben nombrar los distintos elementos del código para mantener una semántica universal.*

* **Idioma del Código Fuente:** `[Definir en qué idioma se escribirán las variables, métodos y clases, y en qué idioma se escribirán los comentarios]`.
* **Convenciones de Capitalización (Casing):** `[Establecer qué formato geométrico se usará para cada tipo de elemento. Ej. Qué formato llevan las variables, cuál llevan las constantes, cuál las clases/estructuras]`.
* **Sufijos y Prefijos Estructurales:** `[Definir cómo se identificarán visualmente los roles de los archivos o clases. Ej. Todos los controladores deben terminar con una palabra específica, todas las interfaces deben empezar con una letra específica]`.
* **Nomenclatura de Archivos y Directorios:** `[Establecer la regla de nombrado físico en el sistema operativo para los archivos fuente y las carpetas]`.

## 3. Formato y Sintaxis del Código (Estilo)

*Establece las reglas visuales y tipográficas del código fuente para garantizar que las herramientas de control de versiones (Git) no generen conflictos por diferencias de formato.*

* **Indentación y Espaciado:** `[Definir si se usan espacios o tabulaciones, la cantidad exacta de espacios por nivel, y el límite máximo de caracteres por línea]`.
* **Estructura Interna del Archivo:** `[Definir el orden exacto en el que deben aparecer los elementos dentro de un archivo. Ej. 1. Importaciones, 2. Variables globales, 3. Constructor, 4. Métodos públicos, 5. Métodos privados]`.
* **Organización de Importaciones / Dependencias:** `[Establecer cómo se agrupan alfabética o lógicamente las dependencias externas vs. las internas en la cabecera del archivo]`.

## 4. Reglas de Implementación Arquitectónica

*Traduce las decisiones tomadas en los Diagramas de Componentes y Clases a reglas de acoplamiento físico en el código.*

* **Dirección de Dependencias (Imports Prohibidos):** `[Declarar explícitamente qué carpetas o módulos tienen prohibido importar código de otras carpetas para proteger la arquitectura. Ej. La capa de dominio nunca debe importar librerías de infraestructura]`.
* **Manejo de Estados y Mutabilidad:** `[Definir las políticas sobre la inmutabilidad de los objetos y cómo se deben gestionar los estados globales o compartidos]`.
* **Gestión de Errores y Excepciones:** `[Establecer el estándar para atrapar, registrar (log) y propagar errores a través de las capas del sistema]`.

## 5. Estándares de Pruebas Automatizadas (Testing)

*Define las reglas inquebrantables para asegurar la calidad del código mediante la verificación automatizada.*

* **Patrón de Estructura de Pruebas:** `[Definir el marco lógico obligatorio para escribir cada prueba, asegurando que la preparación, la acción y la validación estén claramente separadas]`.
* **Nomenclatura de Casos de Prueba:** `[Establecer cómo se debe nombrar una función de prueba para que describa claramente el escenario y el resultado esperado]`.
* **Uso de Datos de Prueba (Mocking y Stubs):** `[Definir las reglas sobre cómo sim
