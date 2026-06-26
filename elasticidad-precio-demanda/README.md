# Elasticidad Precio Demanda – Vega Spec para Deneb

![Elasticidad Precio-Demanda](https://sentidoanalitica.com/wp-content/uploads/2026/06/elasticidad-precio-demanda-v2.gif)

## 📌 Descripción

Este proyecto implementa un objeto visual personalizado en **Power BI** utilizando **Deneb (Vega)** para analizar la **elasticidad precio-demanda** de productos.

La versión actual del visual va más allá de un gráfico de dispersión clásico: calcula automáticamente elasticidad, clasifica el comportamiento comercial, organiza la lectura por cuadrantes y añade trazabilidad temporal cuando se filtra un solo producto. También incorpora tooltips adaptativos, KPIs superiores y una línea de tendencia opcional para apoyar la interpretación.

---

# 🎯 ¿Qué hace este visual?

El visual permite:

- Calcular automáticamente la elasticidad precio-demanda.
- Clasificar productos según su sensibilidad al precio.
- Identificar productos elásticos, inelásticos y de comportamiento atípico.
- Analizar el impacto de cambios de precio sobre las ventas.
- Explorar información mediante tooltips e interacción con Power BI.
- Leer el gráfico por cuadrantes con una semántica comercial clara.
- Mostrar trazabilidad temporal y numeración de puntos cuando se selecciona un solo producto.
- Activar una línea de tendencia solo cuando hay suficientes puntos para que sea interpretable.

---

# 📦 Contenido del repositorio

Este repositorio incluye todo lo necesario para utilizar y explorar el visual.

### 📄 Especificación Vega (JSON)

Contiene la definición completa del objeto visual, incluyendo:

- Transformaciones de datos.
- Cálculos estadísticos.
- Reglas de clasificación.
- Diseño del gráfico.
- Tooltips e interacciones.
- Cuadrantes con color semántico y etiquetas dinámicas.
- Trazabilidad por producto filtrado.
- Línea de tendencia condicional.

### 📊 Archivo Power BI (.pbix)

Incluye un ejemplo completamente funcional con datos de muestra y el visual configurado en Deneb.

> ℹ️ Los nombres de los archivos pueden variar, pero su propósito y estructura permanecen iguales.

---

# 📥 Requisitos de datos

El visual trabaja con cinco campos obligatorios para calcular elasticidad y puede enriquecerse con dos campos opcionales para mejorar etiquetas y tooltips.

## Campos obligatorios

| Campo | Tipo | Descripción |
|--------|------|-------------|
| Fecha | Fecha | Período de análisis. |
| SKU | Texto | Identificador único del producto. |
| Precio_Unitario | Numérico | Precio de venta del producto. |
| Unidades_Vendidas | Numérico | Cantidad vendida en cada período. |
| Ingresos | Numérico | Ventas totales del período. |

## Campos opcionales

| Campo | Tipo | Descripción |
|--------|------|-------------|
| Producto | Texto | Nombre descriptivo del producto. |
| Categoria | Texto | Categoría comercial del producto. |

> ℹ️ El visual funciona correctamente utilizando únicamente los campos obligatorios. Los campos opcionales se utilizan para mejorar la información mostrada en etiquetas, cuadrantes y tooltips.

---

# 📐 Cálculos realizados automáticamente

Todos los cálculos se ejecutan directamente dentro de Vega. No es necesario crear medidas DAX, columnas calculadas ni realizar transformaciones adicionales en Power Query.

El visual calcula automáticamente:

- Variación porcentual del precio.
- Variación porcentual de las unidades vendidas.
- Elasticidad precio-demanda.
- Clasificación automática de elasticidad.
- Variación de ingresos.
- Tendencia de precio.
- Tendencia de demanda.
- Trazabilidad temporal por producto filtrado.
- Regresión lineal de referencia cuando la muestra es suficiente.

---

# 📊 Clasificación de elasticidad

Cada observación se clasifica automáticamente según el valor calculado.

| Elasticidad | Interpretación |
|--------------|---------------|
| Mayor que 1 | Demanda elástica |
| Entre 0 y 1 | Demanda inelástica |
| Igual a 1 | Elasticidad unitaria |
| Menor que 0 | Comportamiento atípico |

---

# 📊 Elementos visuales

El objeto visual combina distintos componentes para facilitar una lectura técnica y comercial de la elasticidad precio-demanda.

Incluye:

- **Dispersión principal**: muestra cada observación en el plano precio-demanda. Identifica patrones de elasticidad, concentración y dispersión.
- **Cuadrantes comerciales**: separan el comportamiento en impulso promocional, captura de valor, deterioro mixto y riesgo de volumen. Traducen el punto técnico a una lectura accionable de negocio.
- **Líneas de referencia**: marcan el cruce de los ejes y el promedio general. Sirven como base de comparación para cada observación.
- **Colores por clasificación**: asignan una familia visual según elasticidad elástica, unitaria, inelástica o anómala. Aceleran la lectura del tipo de comportamiento.
- **Etiquetas inteligentes**: ajustan el texto de los cuadrantes según el contexto visible. Mejoran la legibilidad sin recargar la vista.
- **Tooltips adaptativos**: cambian el nivel de detalle según el filtro activo. Muestran menos o más campos según si se analiza un producto, una categoría o todo el portafolio.
- **Trazabilidad por producto**: ordena los puntos por fecha cuando se filtra un solo SKU. Permite seguir la evolución temporal del producto dentro del scatter.
- **Línea de tendencia condicional**: aparece solo cuando hay suficientes puntos y una regresión válida. Resume la dirección general del comportamiento sin competir con la trayectoria histórica.
- **Interacción con filtros de Power BI**: responde al contexto de segmentadores y filtros cruzados. Permite analizar el mismo visual desde distintas vistas de portafolio.

---

# 🧾 Indicadores (KPIs)

Además del gráfico principal, el visual presenta indicadores resumidos en la parte superior que permiten obtener una visión general del comportamiento del conjunto de datos.

## KPIs superiores

Estos indicadores se calculan automáticamente a partir del último estado visible del filtro y se leen mejor como grupos:

### Portafolio sensible

Mide qué parte del portafolio visible se comporta con elasticidad alta.

- Fórmula: `elastic_sku_count / sku_count`
- Base de cálculo: `sku_count` = SKUs visibles en el contexto actual y `elastic_sku_count` = SKUs cuyo último punto está clasificado como elástico.

### Riesgo de volumen y captura de valor

Estos dos KPIs muestran la distribución comercial de los SKUs visibles según su último punto en el scatter.

- Riesgo de volumen: `risk_sku_count / sku_count`
- Captura de valor: `capture_sku_count / sku_count`
- Base de cálculo: `risk_sku_count` = SKUs cuyo último punto cae en precio al alza y demanda a la baja; `capture_sku_count` = SKUs cuyo último punto cae en precio al alza y demanda al alza.

### Ventas expuestas

Indica qué proporción de los ingresos visibles está asociada a SKUs con riesgo o sensibilidad alta.

- Fórmula: `exposed_ingresos / visible_ingresos`
- Base de cálculo: `exposed_ingresos` = ingresos de SKUs expuestos a riesgo de volumen o elasticidad alta; `visible_ingresos` = ingresos totales visibles.

### Elasticidad ponderada

Resume la elasticidad del conjunto visible dando más peso a los SKUs con más ingresos.

- Fórmula: `weighted_elasticidad_sum / valid_elasticidad_ingresos_sum`
- Base de cálculo: `weighted_elasticidad_sum` = suma de elasticidad por ingresos; `valid_elasticidad_ingresos_sum` = ingresos solo de filas con elasticidad válida.

---

# 💡 Casos de uso

Este visual resulta especialmente útil para:

- Análisis de precios.
- Revenue Management.
- Pricing estratégico.
- Retail.
- Consumo masivo.
- Manufactura.
- Inteligencia Comercial.
- Category Management.
