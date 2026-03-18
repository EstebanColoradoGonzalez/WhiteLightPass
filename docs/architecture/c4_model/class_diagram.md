# Diagrama de Clases y Estructura Lógica Atómica

## 1. Propósito

Este documento define la topología microscópica del código fuente. Su objetivo es mapear las estructuras de datos en memoria (clases, interfaces o tipos abstractos), sus estados encapsulados (atributos) y las operaciones que pueden ejecutar (métodos). Actúa como el plano de construcción definitivo para los programadores, estableciendo los contratos estrictos de acoplamiento, herencia y composición antes de escribir la primera línea de código.

## 2. Convenciones de Notación Orientada a Objetos (o Estructuras)

*Establece el estándar de lenguaje visual estricto para leer e interpretar el diagrama de clases.*

* **Modificadores de Visibilidad:** `[Definir símbolos de encapsulamiento. Ej. (+) Público, (-) Privado, (#) Protegido, (~) Paquete]`.
* **Tipos de Relación:** `[Definir notación para dependencias lógicas. Ej. Flecha hueca para Herencia/Realización, Rombo sólido para Composición (muerte compartida), Rombo hueco para Agregación (ciclos de vida independientes)]`.
* **Estereotipos:** `[Definir marcas para categorizar la naturaleza de la clase. Ej. <<Interface>>, <<Abstract>>, <<Service>>, <<Entity>>]`.

## 3. Diagrama de Clases (Visualización del Código)

*Representación gráfica estática que detalla las clases internas de un componente específico (definido en el paso anterior), mostrando sus atributos, métodos y la red de relaciones estructurales entre ellas.*

`[Espacio para insertar el diagrama UML o equivalente estructural exportado desde la herramienta de modelado]`

## 4. Catálogo Ontológico de Clases / Estructuras

*Inventario de todos los nodos computacionales presentes en el diagrama. Por cada clase o estructura, se debe definir su única razón de existir (Single Responsibility Principle).*

| Nombre de la Estructura (Clase/Interfaz) | Estereotipo / Rol Arquitectónico | Responsabilidad Única (SRP) | Componente Padre |
| :--- | :--- | :--- | :--- |
| `[Nombre exacto en el código]` | `[Ej. Interfaz de contrato, Clase concreta, Fábrica, Servicio Lógico]` | `[La única función computacional o abstracción de negocio que esta clase gestiona en memoria]` | `[ID del componente del Paso 13 al que pertenece]` |
| `[Nombre]` | `[Rol]` | `[Responsabilidad]` | `[Componente]` |

## 5. Contratos de Comportamiento (Métodos y Mutaciones)

*Desglose estricto de las acciones que cada clase puede realizar. Define cómo la clase altera su propio estado o cómo interactúa con otras, estableciendo las firmas exactas de las funciones.*

### 5.1. Clase: [Nombre de la Clase]

| Método / Operación | Visibilidad | Parámetros de Entrada (Tipos) | Tipo de Retorno | Efectos Secundarios (Mutaciones) |
| :--- | :--- | :--- | :--- | :--- |
| `[Nombre de la función]` | `[Pública/Privada]` | `[Datos que exige para ejecutarse]` | `[Qué devuelve al finalizar]` | `[Qué altera en el sistema, ej. Modifica el estado interno, dispara un evento asíncrono, lanza una excepción]` |
| `[Nombre]` | `[Visibilidad]` | `[Parámetros]` | `[Retorno]` | `[Efectos]` |

*(Replicar la subsección 5.1 para todas las clases que expongan lógica de negocio compleja)*

## 6. Estado Interno y Encapsulamiento (Atributos Temporales)

*Documentación de las variables que mantienen el estado de la clase en memoria RAM durante su ciclo de vida operativo. (Se omiten mapeos directos 1:1 a bases de datos documentados en el Diccionario de Datos).*

| Atributo / Variable de Instancia | Tipo de Dato | Visibilidad | Naturaleza del Estado |
| :--- | :--- | :--- | :--- |
| `[Nombre de la variable]` | `[Tipo de dato lógico]` | `[Ej. Privado]` | `[Ej. Estado inmutable, Estado derivado/calculado, Inyección de dependencia externa]` |

## 7. Topología de Relaciones y Acoplamiento Fuerte

*Explicación textual de las conexiones estructurales dibujadas en el diagrama. Define cómo los objetos colaboran o se componen entre sí en tiempo de ejecución.*

| Estructura Origen | Tipo de Relación Lógica | Estructura Destino | Justificación del Acoplamiento |
| :--- | :--- | :--- | :--- |
| `[Nombre de la Clase A]` | `[Ej. Hereda de (Is-A)]` | `[Nombre de la Clase B]` | `[Por qué el diseño exige esta jerarquía abstracta]` |
| `[Nombre de la Clase C]` | `[Ej. Se compone de (Has-A)]` | `[Nombre de la Clase D]` | `[Por qué el ciclo de vida de D depende absolutamente de la existencia de C]` |
| `[Nombre de la Clase E]` | `[Ej. Implementa contrato]` | `[Nombre de la Interfaz F]` | `[Por qué se asegura el polimorfismo a través de esta firma]` |
