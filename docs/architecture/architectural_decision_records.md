# Registros de Decisiones Arquitectónicas (ADR)

## 1. Propósito

Este documento es la bitácora inmutable de las decisiones técnicas y estructurales más significativas tomadas durante el ciclo de vida del sistema. Su objetivo es preservar el contexto histórico: registrar qué problema se intentaba resolver, qué alternativas se evaluaron, por qué se eligió una solución específica y qué consecuencias (positivas y negativas) acarrea dicha decisión. Sirve para evitar que el equipo reabra debates cerrados y para incorporar rápidamente a nuevos ingenieros a la filosofía del proyecto.

## 2. Índice de Decisiones (Log Histórico)

*Tabla de control que lista todas las decisiones tomadas a lo largo del tiempo. Actúa como el índice principal del documento.*

* **Convención de Estados:**
  * **Propuesta:** Bajo evaluación por el equipo de arquitectura.
  * **Adoptada:** Decisión aprobada y actualmente vigente en el sistema.
  * **Sustituida:** Decisión que fue reemplazada por un ADR posterior (nunca se borra, se marca como obsoleta).

| ID ADR | Título Breve de la Decisión | Fecha de Adopción | Estado Actual |
| :--- | :--- | :--- | :--- |
| `ADR-001` | `[Ej. Elección del motor de base de datos principal]` | `[YYYY-MM-DD]` | `[Adoptada / Sustituida por ADR-XXX]` |
| `ADR-002` | `[Ej. Patrón de arquitectura para el contenedor Backend]` | `[YYYY-MM-DD]` | `[Adoptada]` |
| `ADR-003` | `[Título...]` | `[Fecha]` | `[Estado]` |

---

## 3. Catálogo de Decisiones (Registros Individuales)

*Cada decisión arquitectónica significativa debe documentarse utilizando el siguiente formato estructurado.*

### ADR-[Número Secuencial] — [Título Descriptivo de la Decisión]

* **Fecha:** `[YYYY-MM-DD]`
* **Estado:** `[Propuesta / Adoptada / Sustituida por ADR-XXX]`
* **Impulsor (Driver):** `[Referencia estricta al requerimiento no funcional (RNF), restricción del sistema (SDD) o regla de negocio que obliga a tomar esta decisión. Ej. "Impulsado por RNF-12: Alta disponibilidad offline"]`.

**Contexto y Formulación del Problema:**
`[Descripción detallada de la situación tecnológica o de negocio que requiere una decisión. ¿Cuál es el desafío exacto? ¿Qué fuerzas (tiempo, presupuesto, escalabilidad) están en conflicto?]`

**Opciones y Alternativas Evaluadas:**
*Análisis objetivo de las tecnologías, patrones o estrategias que se consideraron viables antes de tomar la decisión final.*

| Alternativa Tecnológica / Estructural | Argumentos a Favor (Pros) | Argumentos en Contra (Contras) |
| :--- | :--- | :--- |
| `[Opción A (La elegida)]` | `[Ventajas técnicas, de costo o de alineación con el equipo]` | `[Deuda técnica asumida, curvas de aprendizaje, limitaciones]` |
| `[Opción B (Descartada)]` | `[Pros...]` | `[Por qué esta desventaja fue inaceptable para el proyecto]` |
| `[Opción C (Descartada)]` | `[Pros...]` | `[Contras...]` |

**Decisión Tomada:**
`[Declaración clara, directa y sin ambigüedades de la opción seleccionada. Ej. "Utilizaremos [Tecnología/Patrón X] como el estándar exclusivo para [Propósito Y] en todo el sistema"]`.

**Consecuencias y Deuda Técnica Asumida:**
`[Toda decisión arquitectónica tiene un costo. Enumera qué impacto directo tendrá esta decisión en el desarrollo futuro, en la infraestructura, en las licencias o en la curva de aprendizaje del equipo. ¿Qué limitación estamos aceptando vivir a cambio de los beneficios?]`

---
*(Replicar el bloque de la Sección 3 por cada decisión arquitectónica mayor)*
