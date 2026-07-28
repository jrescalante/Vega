# Panel de Rentabilidad Comercial - Especificación Vega para Deneb

![Panel de Rentabilidad Comercial](https://sentidoanalitica.com/wp-content/uploads/2026/07/commercial-profitability-cockpit.gif)

## 📌 Descripción

Este proyecto implementa un visual personalizado de **Power BI** con **Deneb (Vega)** para analizar la rentabilidad comercial frente a objetivos de margen trimestrales por SKU.

La versión actual va más allá de un gráfico de dispersión tradicional: compara el crecimiento de ingresos frente a la brecha de margen, clasifica cada SKU en cuadrantes comerciales, agrega KPI en el encabezado y utiliza una tabla de objetivos para evaluar cada producto contra su objetivo específico de margen. El resultado es un panel visual para detectar crecimiento saludable, riesgo de margen, destrucción de valor y oportunidades de defensa comercial.

---

# 🎯 ¿Qué hace este visual?

Este visual se construye alrededor de una pregunta comercial simple pero crítica:

¿Cómo podemos ver, en una sola vista, qué productos crecen rentablemente y cuáles crecen solo porque se está sacrificando el margen?

Es más que un gráfico de dispersión.
En una sola vista muestra cómo crece cada SKU, qué tan lejos está de su objetivo de margen y dónde está expuesto el negocio.

Por eso, la conversación cambia de «¿estamos vendiendo más?» a «¿estamos vendiendo mejor?».

El visual permite:

- Comparar el crecimiento de ingresos de cada SKU frente a su primer período visible.
- Calcular el margen bruto a partir de ingresos, unidades y costo unitario.
- Evaluar cada SKU frente a su objetivo trimestral específico de margen.
- Medir la brecha entre el margen real y el margen objetivo.
- Clasificar SKU en cuadrantes de crecimiento y rentabilidad comercial.
- Identificar los ingresos expuestos de SKU por debajo del objetivo.
- Leer KPI de portafolio, rentabilidad y riesgo en la parte superior del visual.
- Explorar detalles mediante tooltips adaptativos de Power BI.
- Responder a segmentadores, filtros y contexto de selección cruzada.

---

# 📦 Contenido del repositorio

Este repositorio incluye los elementos necesarios para utilizar y explorar el visual.

### 📄 Especificación Vega (JSON)

Contiene la definición completa del visual, incluyendo:

- Transformaciones de datos.
- Cálculo del margen bruto.
- Cálculo del crecimiento de ingresos.
- Cálculo de la brecha de margen frente al objetivo.
- Reglas de clasificación por cuadrante.
- Diseño del gráfico.
- Tooltips e interacciones.
- KPI de cabecera.
- Cuadrantes con colores semánticos y etiquetas dinámicas.

La versión en español utiliza `commercial-profitability-cockpit-es.json`, `commercial-profitability-cockpit-es.csv` y `commercial-profitability-targets-es.csv`.

### 📊 Archivo Power BI (.pbix)

Incluye un ejemplo funcional con datos de muestra, tabla de objetivos, medidas configuradas y el visual cargado en Deneb.

> ℹ️ Los nombres de los archivos pueden variar, pero su propósito y estructura son los mismos.

---

# 📥 Requisitos de datos

La versión en español usa una tabla de hechos a nivel SKU-período y una tabla de objetivos de margen a nivel SKU-trimestre. Los nombres de campos siguientes son los que espera `commercial-profitability-cockpit-es.json`.

## Campos obligatorios de la tabla de hechos

| Campo | Tipo | Descripción |
|--------|------|-------------|
| Fecha | Fecha | Período de análisis. |
| SKU | Texto | Identificador único del producto. |
| Ingresos | Numérico | Ingresos del período. |
| Unidades_Vendidas | Numérico | Unidades vendidas en el período. |
| Costo_Unitario | Numérico | Costo unitario usado para calcular el margen bruto. |
| ClaveSKUTrimestre | Texto | Clave compuesta SKU-trimestre, por ejemplo `SKU-001|2024Q1`. |

## Campos recomendados de la tabla de hechos

| Campo | Tipo | Descripción |
|--------|------|-------------|
| Producto | Texto | Nombre descriptivo del producto. |
| **Categoría** | Texto | Categoría comercial del producto. Se recomienda para segmentar el portafolio y mejorar el contexto de los tooltips. |
| **Canal** | Texto | Canal de venta. Se recomienda para analizar la presión de margen por ruta al mercado o canal comercial. |
| Precio_Unitario | Numérico | Precio unitario de venta. |
| Año | Numérico | Año calendario usado, junto con `Numero_Trimestre`, para construir `ClaveTrimestre`. |
| Numero_Trimestre | Numérico | Número de trimestre usado, junto con `Año`, para construir `ClaveTrimestre`. |
| Trimestre | Texto | Etiqueta del trimestre, por ejemplo `Q1`. |
| ClaveTrimestre | Texto | Clave año-trimestre, por ejemplo `2024Q1`. |

> ℹ️ `Producto`, `Categoría` y `Canal` son opcionales para el funcionamiento del visual, pero `Categoría` y `Canal` son especialmente útiles para segmentar los patrones de rentabilidad. `ClaveTrimestre` se deriva de `Año` y `Numero_Trimestre`, y luego se combina con `SKU` para crear `ClaveSKUTrimestre`.

## Tabla de objetivos de margen

La tabla `Objetivos de Margen` contiene el objetivo de margen por SKU y trimestre. Esta tabla evita crear una tabla calculada DAX extensa y conserva los objetivos como una entrada de negocio.

| Campo | Tipo | Descripción |
|--------|------|-------------|
| SKU | Texto | Identificador del producto para facilitar la lectura y las comprobaciones de auditoría. |
| Año | Numérico | Año del objetivo. |
| Trimestre | Texto | Trimestre objetivo, por ejemplo `Q1`. |
| Clave Trimestre | Texto | Clave año-trimestre, por ejemplo `2024Q1`. |
| Clave SKU Trimestre | Texto | Clave de relación con `Datos_ES[ClaveSKUTrimestre]`. |
| Margen Objetivo SKU-T | Numérico | Margen objetivo para el SKU-trimestre. |

Relación esperada:

```text
Datos_ES[ClaveSKUTrimestre] -> Objetivos de Margen[Clave SKU Trimestre]
```

Cada `ClaveSKUTrimestre` visible en la tabla de hechos debe tener una fila correspondiente en `Objetivos de Margen`. Las medidas no incluyen un objetivo predeterminado; si falta un objetivo, el resultado debe devolver vacío para que el problema de cobertura de datos permanezca visible.

---

# 📐 Medidas obligatorias de Power BI

A diferencia de los visuales que calculan todo directamente en Vega, este visual utiliza dos medidas DAX para leer correctamente la tabla de objetivos desde el modelo semántico.

## Margen objetivo SKU

Devuelve el margen objetivo para el SKU-trimestre actual buscando la `ClaveSKUTrimestre` actual en `Objetivos de Margen`.

```DAX
Margen Objetivo SKU =
VAR ClaveSKUTrimestreActual =
    SELECTEDVALUE ( Datos_ES[ClaveSKUTrimestre] )
RETURN
    LOOKUPVALUE (
        'Objetivos de Margen'[Margen Objetivo SKU-T],
        'Objetivos de Margen'[Clave SKU Trimestre], ClaveSKUTrimestreActual
    )
```

## Margen objetivo contexto

Calcula el margen objetivo ponderado por ingresos para todos los SKU-trimestres visibles en el contexto de filtro actual.

```DAX
Margen Objetivo Contexto =
VAR ClavesVisibles =
    VALUES ( Datos_ES[ClaveSKUTrimestre] )
VAR ObjetivoPonderado =
    SUMX (
        ClavesVisibles,
        VAR ClaveActual = Datos_ES[ClaveSKUTrimestre]
        VAR PesoIngresos =
            CALCULATE ( SUM ( Datos_ES[Ingresos] ) )
        VAR MargenObjetivo =
            LOOKUPVALUE (
                'Objetivos de Margen'[Margen Objetivo SKU-T],
                'Objetivos de Margen'[Clave SKU Trimestre], ClaveActual
            )
        RETURN
            PesoIngresos * MargenObjetivo
    )
VAR IngresosVisibles =
    SUMX ( ClavesVisibles, CALCULATE ( SUM ( Datos_ES[Ingresos] ) ) )
RETURN
    DIVIDE ( ObjetivoPonderado, IngresosVisibles )
```

Estas dos medidas deben agregarse al visual de Deneb junto con los campos obligatorios. `Margen Objetivo SKU` clasifica cada punto, mientras que `Margen Objetivo Contexto` proporciona el objetivo de referencia ponderado para el contexto de filtro visible.

---

# 📐 Cálculos automáticos

Cuando los campos y las medidas están disponibles en Deneb, Vega calcula automáticamente:

- Margen bruto a nivel de fila.
- Porcentaje de margen bruto.
- Crecimiento de ingresos frente al primer período visible.
- Crecimiento alternativo frente al promedio del portafolio cuando solo existe un punto histórico.
- Brecha de margen frente al objetivo del SKU-trimestre.
- Clasificación por cuadrante comercial.
- SKU rentables y críticos.
- Ingresos expuestos de SKU por debajo del objetivo.
- Margen bruto ponderado visible.
- Conteos visibles de categorías y productos.

---

# 📊 Clasificación comercial

Cada SKU se clasifica utilizando dos ejes: crecimiento de ingresos y brecha de margen frente al objetivo.

| Cuadrante | Condición | Interpretación |
|-----------|-----------|----------------|
| Defender volumen | Crecimiento negativo y margen sobre el objetivo | El margen se mantiene mientras caen los ingresos. |
| Crecer con rentabilidad | Crecimiento positivo y margen sobre el objetivo | Crecimiento saludable con la rentabilidad objetivo. |
| Destrucción de valor | Crecimiento negativo y margen bajo el objetivo | Los ingresos y el margen se deterioran al mismo tiempo. |
| Crecimiento con riesgo de margen | Crecimiento positivo y margen bajo el objetivo | Hay crecimiento, pero la rentabilidad está bajo presión. |

La línea horizontal representa `En objetivo`: una brecha de margen igual a cero. Por encima de esa línea, el SKU está por encima de su objetivo; por debajo, el SKU está por debajo de su objetivo.

---

# 📊 Elementos visuales

El visual combina varios componentes para facilitar una lectura ejecutiva y analítica de la rentabilidad.

Incluye:

- **Gráfico de dispersión principal**: muestra cada SKU por crecimiento de ingresos y brecha de margen frente al objetivo. Ayuda a detectar concentración, dispersión y puntos críticos.
- **Cuadrantes comerciales**: traducen los resultados numéricos en una lectura accionable: defender volumen, crecer rentablemente, destruir valor o crecer con riesgo de margen.
- **Líneas de referencia**: marcan el crecimiento cero de ingresos y la brecha cero de margen frente al objetivo. Proporcionan la base para interpretar cada punto.
- **Colores por cuadrante**: asignan una familia visual a cada condición comercial y agilizan la lectura del estado de cada SKU.
- **Tamaño de punto según ingresos**: da mayor peso visual a los SKU con mayores ingresos visibles.
- **KPI de cabecera**: resumen el portafolio visible por SKU, rentabilidad, criticidad, exposición y margen.
- **Tooltips adaptativos**: muestran período, SKU, producto, categoría, cuadrante, crecimiento, margen real, margen objetivo, brecha de margen e ingresos según los campos disponibles.
- **Interacción con filtros de Power BI**: responde a segmentadores, filtros cruzados y selecciones de categoría o producto.

---

# 🧾 Indicadores (KPI)

Además del gráfico principal, el visual presenta indicadores de resumen en la parte superior para leer rápidamente el estado del portafolio visible.

## KPI de cabecera

### SKU visibles

Cuenta los SKU representados por el último punto visible de cada producto.

- Fórmula: `count(sku_key)` sobre los últimos puntos visibles.
- Base de cálculo: un punto por SKU después de aplicar el contexto de filtro.

### SKU rentables

Muestra la proporción de SKU visibles que están en o por encima de su objetivo de margen.

- Fórmula: `profitable_sku_count / sku_count`
- Base de cálculo: `profitable_sku_count` = SKU con brecha de margen mayor o igual a cero.

### SKU críticos

Muestra la proporción de SKU visibles que están por debajo de su objetivo de margen.

- Fórmula: `critical_sku_count / sku_count`
- Base de cálculo: `critical_sku_count` = SKU con brecha de margen negativa.

### Ingresos expuestos

Mide la proporción de ingresos visibles asociada a SKU por debajo del objetivo.

- Fórmula: `exposed_revenue_total / visible_revenue`
- Base de cálculo: `exposed_revenue_total` = ingresos de SKU con brecha de margen negativa; `visible_revenue` = ingresos visibles totales.

### Margen bruto ponderado

Resume el margen bruto del conjunto visible, ponderado por ingresos.

- Fórmula: `gross_margin_total / visible_revenue`
- Base de cálculo: `gross_margin_total` = ingresos menos costo total; `visible_revenue` = ingresos visibles totales.

---

# 💡 Casos de uso

Este visual es especialmente útil para:

- Gestión de ingresos.
- Control de rentabilidad comercial.
- Gestión de categorías.
- Seguimiento de objetivos de margen.
- Priorización de SKU críticos.
- Gestión de portafolio.
- Planificación comercial trimestral.
- Análisis de crecimiento rentable.
- Identificación de ingresos expuestos.
- Conversaciones entre equipos de ventas, finanzas y precios.