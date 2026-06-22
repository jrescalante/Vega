# 📈 Elasticidad Precio-Demanda en Power BI con Vega

## 📌 Descripción

Este proyecto implementa un objeto visual personalizado en **Power BI** utilizando **Deneb (Vega)** para analizar la **elasticidad precio-demanda** de productos.

El visual calcula automáticamente la elasticidad utilizando variaciones porcentuales entre períodos consecutivos y clasifica cada producto según su sensibilidad al precio. Además, incorpora indicadores resumidos, tooltips enriquecidos e interacciones para facilitar el análisis comercial.

---

# 🎯 ¿Qué hace este visual?

El visual permite:

- Calcular automáticamente la elasticidad precio-demanda.
- Clasificar productos según su sensibilidad al precio.
- Identificar productos elásticos, inelásticos y de comportamiento atípico.
- Analizar el impacto de cambios de precio sobre las ventas.
- Explorar información mediante tooltips e interacción con Power BI.

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

### 📊 Archivo Power BI (.pbix)

Incluye un ejemplo completamente funcional con datos de muestra y el visual configurado en Deneb.

> ℹ️ Los nombres de los archivos pueden variar, pero su propósito y estructura permanecen iguales.

---

# 📥 Requisitos de datos

El visual requiere únicamente cinco campos para realizar todos los cálculos de elasticidad. De forma opcional, es posible incluir información descriptiva para enriquecer las etiquetas y los tooltips.

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

> ℹ️ El visual funciona correctamente utilizando únicamente los campos obligatorios. Los campos opcionales se utilizan para mejorar la información mostrada en etiquetas y tooltips.

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

El objeto visual combina distintos componentes para facilitar la interpretación de la elasticidad precio-demanda.

Incluye:

- Dispersión Precio vs Elasticidad.
- Líneas de referencia.
- Colores por clasificación.
- Etiquetas inteligentes.
- Tooltips enriquecidos.
- Interacción con filtros de Power BI.

---

# 🧾 Indicadores (KPIs)

Además del gráfico principal, el visual presenta indicadores resumidos que permiten obtener una visión general del comportamiento del conjunto de datos.

Los KPIs muestran:

- Número total de productos.
- Elasticidad promedio.
- Productos elásticos.
- Productos inelásticos.
- Variación promedio de precio.
- Variación promedio de demanda.

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

---

# ⚙️ Tecnologías utilizadas

- Power BI
- Deneb
- Vega
- JSON

---

# 🚀 Ventajas

- Sin medidas DAX complejas.
- Sin Power Query adicional.
- Todo el procesamiento ocurre dentro de Vega.
- Fácil integración en cualquier modelo de Power BI.
- Alto rendimiento incluso con grandes volúmenes de datos.
- Totalmente interactivo.

---

# 📄 Licencia

Este proyecto se distribuye bajo la licencia MIT.

---

# 👤 Autor

Desarrollado por **Carlos González**.
