# Wireframes de Baja Fidelidad y Estructura de Interfaz

## 1. Propósito

Este documento define la distribución espacial (layout) y el contenido estructural de cada nodo de interacción (vista/pantalla) definido previamente en el Mapa de Navegación. Su objetivo es establecer la jerarquía de la información, la ubicación de los controles de acción y el comportamiento de la interfaz ante distintos estados lógicos, sin definir el estilo visual final.

## 2. Convenciones de Representación

*Define la nomenclatura utilizada en los esquemas visuales para diferenciar tipos de contenido y contenedores.*

- **[ Acción / Control ]:** Representa elementos interactivos (botones, enlaces, campos de entrada, selectores).
- **( Texto Informativo ):** Representa datos de solo lectura, etiquetas o mensajes del sistema.
- **{ Contenedor / Agrupador }:** Representa una zona lógica que agrupa múltiples elementos relacionados.
- **`--`**: Representa una división estructural o visual entre secciones.
- **Símbolos Dinámicos:** (Ej. `▼` para menús desplegables, `[ ]` para casillas de selección).

## 3. Estructura General del Sistema (Plantilla Base)

*Define las zonas universales que se repiten a lo largo de múltiples vistas en el sistema (ej. Barras de navegación globales, encabezados persistentes, menús laterales).*

- **Zona Superior / Encabezado:** [Descripción de los elementos fijos en la parte superior].
- **Zona Central / Cuerpo:** [Área principal de contenido, indicar si permite desplazamiento/scroll].
- **Zona Inferior / Pie:** [Descripción de los elementos fijos en la parte inferior].
- **Zona Lateral / Menú:** [Descripción de elementos de navegación lateral, si aplica].

## 4. Definición Estructural por Nodo de Interacción (Vista)

*Para cada nodo definido en el Mapa de Navegación, se debe crear una subsección con su esquema y comportamiento.*

### [ID del Nodo] — [Nombre del Nodo]

**Contexto Lógico:** [Breve descripción de cuándo el agente llega a esta vista y su objetivo principal. Citar las Historias de Usuario de referencia].

**Esquema Estructural (Wireframe Lógico):**

```
┌──────────────────────────────────────┐
│          [ZONA ESTRUCTURAL A]        │
│                                      │
│  (Título o Identificador de vista)   │
│  [ Control de Acción Global ]        │
├──────────────────────────────────────┤
│          [ZONA ESTRUCTURAL B]        │
│                                      │
│  { Agrupador Lógico de Datos }       │
│  (Etiqueta de Dato 1)                │
│  [ Campo de Entrada de Dato 1 ]      │
│    → Restricción / Comportamiento    │
│                                      │
│  (Etiqueta de Dato 2)                │
│  (Valor Dinámico del Dato 2)         │
│                                      │
│  ---                                 │
│                                      │
│  { Agrupador Lógico de Acciones }    │
│  [ Acción Primaria ]                 │
│  [ Acción Secundaria ]               │
│                                      │
└──────────────────────────────────────┘
```

**Inventario de Elementos y Comportamiento:***Tabla que detalla cada elemento del esquema superior, su naturaleza interactiva y el evento que desencadena.*

| Elemento | Tipo Lógico | Ubicación Zonal | Comportamiento / Restricción / Gatillo de Navegación |
| --- | --- | --- | --- |
| `[Nombre del Elemento]` | `[Ej. Input de texto, Botón primario, Lista estática]` | `[Zona en el esquema]` | `[Qué hace al interactuar, qué restricciones de entrada tiene, o hacia qué ID de Nodo transita al activarse]` |
| `[Nombre del Elemento]` | `[Tipo]` | `[Zona]` | `[Comportamiento]` |

**Estados Dinámicos de la Interfaz:** *Describe cómo muta la estructura visual de esta vista en respuesta a diferentes condiciones de los datos o acciones del agente.*

- **Estado [Nombre del Estado 1]:** (Ej. Estado de inicialización o formulario vacío). [Descripción de qué elementos están ocultos, deshabilitados o muestran valores por defecto].
- **Estado [Nombre del Estado 2]:** (Ej. Estado de validación fallida o procesamiento). [Descripción de los mensajes de retroalimentación, indicadores de carga o bloqueo de controles].
- **Estado [Nombre del Estado 3]:** (Ej. Estado vacío o sin datos). [Descripción de la información alternativa que se muestra cuando no hay elementos en una lista o agrupación].

---

*(Replicar la sección "4. Definición Estructural por Nodo" para la totalidad de los nodos del sistema)*
