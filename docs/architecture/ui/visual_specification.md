# Especificación Visual y Sistema de Diseño (Design System)

## 1. Propósito

Este documento define las variables visuales, los *tokens* de diseño y el comportamiento estético de la interfaz de usuario. Transforma la distribución estructural definida en los Wireframes (Paso 7) en una guía de estilo cuantificable y estandarizada. Este documento actúa como la fuente única de verdad para la capa de presentación (View/UI) del sistema, garantizando consistencia y accesibilidad.

## 2. Restricciones Visuales de Entorno

*Enumera las limitaciones del entorno operativo o del dispositivo físico que dictan reglas inquebrantables sobre el diseño.*

| Restricción Operativa | Impacto Obligatorio en el Diseño |
| :--- | :--- |
| `[Descripción de la restricción, ej. Condiciones de iluminación, tamaño de pantalla físico, normativas de accesibilidad obligatorias]` | `[Cómo el diseño debe adaptarse a esta restricción, ej. Uso obligatorio de modo oscuro, tamaño mínimo de botones]` |

## 3. Identidad Fundamental

*Define la filosofía visual del producto y los elementos de marca invariables.*

### 3.1. Concepto y Lenguaje Visual

*Breve descripción de la emoción, tono o nivel de profesionalismo que la interfaz debe transmitir.*

### 3.2. Marca e Iconografía Principal

*Especificaciones técnicas sobre los logotipos o símbolos universales del sistema.*

* **Dimensiones Base:** `[Ej. Proporción 1:1, Tamaño mínimo permitido]`
* **Zonas de Seguridad / Márgenes:** `[Espacio vacío obligatorio alrededor del símbolo]`

## 4. Paleta de Colores (Color Tokens)

*Define matemáticamente todos los colores que existirán en el sistema. Nunca se debe usar un color no listado aquí.*

### 4.1. Colores de Marca y Estructura (Esquema Base)

*Define los colores primarios, secundarios, fondos y superficies.*

| Token / Rol Semántico | Valor (Hex/RGB) | Uso Estricto en el Sistema | Color de Contraste (Texto/Icono sobre este fondo) |
| :--- | :--- | :--- | :--- |
| **Primario (Acción Principal)** | `[Valor]` | `[Ej. Botones de confirmación, enlaces activos]` | `[Valor]` |
| **Secundario (Soporte)** | `[Valor]` | `[Ej. Elementos decorativos, botones secundarios]` | `[Valor]` |
| **Fondo / Superficie (Base)** | `[Valor]` | `[Ej. Color base de la pantalla]` | `[Valor]` |
| **Contorno / Borde** | `[Valor]` | `[Ej. Líneas divisorias, bordes de formularios]` | N/A |

### 4.2. Colores de Estado (Feedback del Sistema)

*Define los colores reservados exclusivamente para comunicar cambios de estado o anomalías al usuario.*

| Estado Lógico | Valor (Hex/RGB) | Uso Estricto en el Sistema |
| :--- | :--- | :--- |
| **Éxito / Confirmación** | `[Valor]` | `[Ej. Operación completada, validación correcta]` |
| **Advertencia / Precaución** | `[Valor]` | `[Ej. Acción destructiva inminente, datos incompletos]` |
| **Error / Falla Crítica** | `[Valor]` | `[Ej. Fallo de conexión, validación rechazada]` |
| **Inactivo / Deshabilitado** | `[Valor]` | `[Ej. Botones sin los requisitos previos cumplidos]` |

*(Nota: Si el sistema soporta múltiples esquemas, como Modo Claro/Oscuro, duplicar las tablas 4.1 y 4.2 para el esquema alternativo).*

## 5. Tipografía y Jerarquía Textual

*Establece las familias de fuentes y la escala matemática de los tamaños de texto para garantizar legibilidad y orden visual.*

### 5.1. Familias Tipográficas

* **Fuente Principal:** `[Nombre de la fuente]` (Uso: `[Ej. Títulos y lectura densa]`).
* **Fuente Secundaria / Monoespaciada (Si aplica):** `[Nombre de la fuente]` (Uso: `[Ej. Datos numéricos, código o identificadores]`).

### 5.2. Escala Tipográfica (Type Tokens)

| Token / Nivel | Tamaño Base | Peso (Grosor) | Altura de Línea (Line Height) | Uso Específico en el Sistema |
| :--- | :--- | :--- | :--- | :--- |
| **Título Nivel 1 (Display)** | `[Valor]` | `[Valor]` | `[Valor]` | `[Ej. Título principal de la pantalla]` |
| **Título Nivel 2 (Header)** | `[Valor]` | `[Valor]` | `[Valor]` | `[Ej. Nombres de secciones o agrupaciones]` |
| **Cuerpo Principal (Body)** | `[Valor]` | `[Valor]` | `[Valor]` | `[Ej. Texto de lectura regular, descripciones]` |
| **Etiqueta (Label / Caption)** | `[Valor]` | `[Valor]` | `[Valor]` | `[Ej. Texto dentro de botones, metadata pequeña]` |

## 6. Sistema de Espaciado y Dimensiones

*Establece la métrica universal (grid) que dicta la distancia entre elementos y el tamaño de los contenedores.*

### 6.1. Métrica Base (Grid Unit)

* **Unidad Base:** `[Ej. 4px, 8dp, 1rem]` (Todos los márgenes y tamaños deben ser múltiplos de este valor).

### 6.2. Variables de Espaciado (Spacing Tokens)

| Token | Valor | Aplicación Estricta |
| :--- | :--- | :--- |
| **Espaciado Interno (Padding)** | `[Valor]` | `[Ej. Espacio dentro de un contenedor o botón]` |
| **Espaciado Externo (Margin)** | `[Valor]` | `[Ej. Separación entre dos componentes distintos]` |
| **Separación de Agrupación** | `[Valor]` | `[Ej. Separación principal entre grandes bloques de contenido]` |

## 7. Librería de Componentes (UI Kit)

*Define el comportamiento visual exacto de los elementos interactivos definidos en los Wireframes.*

### 7.1. Controles de Acción (Botones)

*Especifica propiedades como radio de borde, elevación/sombra y estados de interacción.*

* **Botón Primario:** `[Definición de color, forma y comportamiento al ser presionado/enfocado]`.
* **Botón Secundario / Fantasma:** `[Definición visual]`.

### 7.2. Controles de Entrada (Formularios)

*Especifica cómo se ven los lugares donde el usuario ingresa información.*

* **Campo de Texto Reposo:** `[Color de borde, color de fondo, estilo de la etiqueta]`.
* **Campo de Texto Activo (Focus):** `[Cambio visual cuando el usuario interactúa con él]`.
* **Campo con Error:** `[Cambio visual cuando el dato ingresado es inválido]`.

### 7.3. Contenedores y Superficies (Cards/Modales)

*Especifica cómo se agrupa visualmente la información.*

* **Superficie Estándar:** `[Ej. Radio de esquina, color de fondo, nivel de sombra/elevación]`.
* **Superficie Superpuesta (Diálogo):** `[Ej. Nivel de sombra profundo, color del fondo difuminado (scrim)]`.

## 8. Especificación de Feedback y Transiciones

*Define cómo el sistema comunica cambios de estado a lo largo del tiempo (animaciones).*

| Tipo de Transición / Interacción | Comportamiento Visual / Duración |
| :--- | :--- |
| **Navegación de Avance** | `[Ej. Desplazamiento desde la derecha, 300ms]` |
| **Aparición de Alertas / Diálogos** | `[Ej. Desvanecimiento (Fade-in) y escalado, 200ms]` |
| **Carga de Datos (Processing)** | `[Ej. Indicador circular en color Primario girando en bucle]` |

## 9. Directrices de Accesibilidad Visual

*Lista de verificación final para garantizar que el diseño es operable por cualquier persona.*

* **Contraste:** `[Ej. Todo texto debe cumplir un ratio mínimo de contraste de 4.5:1 respecto a su fondo]`.
* **Independencia del Color:** `[Ej. Ninguna información crítica o estado de error debe comunicarse EXCLUSIVAMENTE mediante color; debe acompañarse de un símbolo o texto]`.
* **Área Interactiva:** `[Ej. El tamaño mínimo para que un elemento sea considerado 'clickeable' es de 44x44 unidades]`.
