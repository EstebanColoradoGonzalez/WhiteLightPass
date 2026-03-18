# Mapa de Navegación y Arquitectura de Interacción

## 1. Propósito

Este documento define la totalidad de los nodos de interacción (vistas, pantallas o estados) del sistema y las rutas de transición entre ellos. Actúa como el puente estructural entre las Historias de Usuario (qué se necesita hacer) y los Wireframes (cómo se verá la interfaz).

Ningún nodo de interacción puede existir si no está respaldado por al menos una Historia de Usuario, garantizando así que la interfaz contenga solo lo estrictamente necesario.

## 2. Nomenclatura y Convenciones

*Define las reglas de nombrado para identificar unívocamente cada nodo en el sistema.*

* **[Prefijo de Flujo] + [Secuencia]:** (Ej. `A1`, `A2` para el Flujo de Entrada; `B1` para el panel principal).
* **Nodo Origen:** El estado o vista actual del agente.
* **Nodo Destino:** El estado o vista al que el agente transita.
* **Evento / Gatillo:** La acción explícita del agente o el proceso interno del sistema que dispara el cambio de estado.

## 3. Inventario de Nodos de Interacción (Vistas)

*Agrupa las vistas por flujos lógicos o módulos del sistema. Por cada vista, define su propósito y vincula las Historias de Usuario que requieren su existencia.*

### Flujo [Letra/Identificador] — [Nombre del Flujo, ej. Autenticación / Operación Principal]

| ID Nodo | Nombre del Nodo | Propósito Lógico del Estado | Historias de Usuario Vinculadas |
| :--- | :--- | :--- | :--- |
| `[ID]` | `[Nombre descriptivo]` | `[Descripción de la información que se muestra y las acciones lógicas permitidas en este estado. Sin detalles visuales.]` | `[HU-XX, HU-YY]` |
| `[ID]` | `[Nombre descriptivo]` | `[Descripción del estado...]` | `[HU-ZZ]` |

*(Repetir la tabla anterior por cada flujo macro del sistema)*

## 4. Matriz de Transiciones (Rutas de Navegación)

*Define todas las conexiones posibles entre los nodos inventariados. Esta tabla es el mapa de rutas estricto; si una conexión no está aquí, el agente no podrá realizar ese movimiento en el sistema real.*

### 4.1. Navegación Global o Transversal

*Listado de nodos que son accesibles desde casi cualquier parte del sistema (ej. menús principales, barras de navegación globales).*

| Destino Global | Evento / Gatillo | Restricciones o Excepciones |
| :--- | :--- | :--- |
| `[ID Destino]` | `[Acción que invoca este destino]` | `[Ej. No disponible durante una transacción activa]` |

### 4.2. Transiciones Específicas por Nodo

*Detalla los movimientos exactos desde un nodo específico hacia otros.*

| Nodo Origen | Nodo Destino | Evento / Gatillo | Condición / Regla de Negocio |
| :--- | :--- | :--- | :--- |
| `[ID Origen]` | `[ID Destino]` | `[Ej. Confirmación de formulario]` | `[Ej. Solo si la validación es exitosa]` |
| `[ID Origen]` | `[ID Origen]` | `[Ej. Cancelación de acción]` | `[Retorno al estado anterior]` |
| `[ID Origen]` | `[ID Destino]` | `[Ej. Selección de un ítem]` | `[---]` |

## 5. Trazabilidad Inversa (Historias de Usuario $\rightarrow$ Vistas)

*Matriz de control de calidad para auditar que el 100% de las Historias de Usuario (Paso 5) tienen un espacio físico/lógico asignado para su ejecución en este mapa.*

| ID Historia | Nodos donde se manifiesta o ejecuta |
| :--- | :--- |
| `HU-01` | `[ID Nodo A]`, `[ID Nodo B]` |
| `HU-02` | `[ID Nodo C]` |
| `HU-XX` | `[ID Nodo N]` |

**Validación:** [Total de HUs mapeadas] / [Total de HUs del documento de Historias] = 100% Cobertura.

## 6. Diagramas de Flujo de Navegación

*Representación visual estandarizada de las matrices anteriores, mostrando la red de nodos y sus direcciones.*

### 6.1. Diagrama Macro (Topología General)

*[Espacio para insertar un diagrama, preferiblemente usando sintaxis de grafo (ej. Mermaid), que muestre cómo los distintos flujos macro se conectan entre sí, ignorando el detalle interno de cada pantalla].*

### 6.2. Diagramas de Flujo Específicos

*[Espacio para insertar diagramas detallados de los flujos más complejos, mostrando cada transición, retorno y bifurcación condicional].*
