# Elasticidad Precio-Demanda – Documentación

# 📊 Elasticidad Precio-Demanda (Vega / Deneb)

Un visual de **Elasticidad Precio-Demanda** desarrollado con **Vega v6** y renderizado en **Deneb** para Power BI.

Este objeto visual analiza la relación entre las variaciones de precio y demanda mediante un diagrama de dispersión, calculando automáticamente la elasticidad arco para cada producto. Además, clasifica cada observación según su comportamiento comercial y resume los principales indicadores para facilitar la toma de decisiones.

Esta carpeta contiene una especificación Vega diseñada para utilizarse con Deneb en Power BI:

* **price-demand-elasticity.json:** Especificación Vega (versión en español)
* **Price-Demand-Elasticity.pbix:** Archivo de Power BI con el visual renderizado en Deneb

Este archivo JSON contiene una especificación completa y autocontenida de Vega, enfocada en el análisis económico, la interpretación visual y el uso práctico en inteligencia de negocios.

---

# 🚀 ¿Qué hace este visual?

Este visual permite:

* Analizar la relación entre el cambio porcentual del precio y la demanda.
* Calcular automáticamente la elasticidad arco de cada producto.
* Clasificar los productos según su comportamiento de elasticidad.
* Visualizar simultáneamente precio, demanda y elasticidad en un solo gráfico.
* Identificar oportunidades comerciales mediante cuadrantes estratégicos.
* Mostrar indicadores generales del conjunto de datos.

El visual está diseñado para ser completamente autocontenido, adaptable y reutilizable en distintos conjuntos de datos.

---

# 📦 Contenido del repositorio

Este repositorio incluye todo lo necesario para utilizar y explorar el visual:

### 📄 Especificación Vega (JSON)

Definición completa del objeto visual, incluyendo cálculos internos, clasificación de elasticidad, cuadrantes, KPIs, escalas y elementos gráficos.

### 📊 Archivo Power BI (.pbix)

Ejemplo listo para usar con datos de muestra y el visual configurado en Deneb.

> ℹ️ Los nombres de los archivos pueden cambiar, pero su estructura y propósito permanecen iguales.

---

# 🧠 ¿Cuándo utilizar este visual?

Utilice este visual cuando necesite responder preguntas como:

* ¿Cómo responde la demanda ante cambios en el precio?
* ¿Qué productos presentan demanda elástica o inelástica?
* ¿Qué artículos generan oportunidades de incremento de precio?
* ¿Qué productos podrían requerir promociones o estrategias comerciales?
* ¿Cómo se distribuyen los productos según su sensibilidad al precio?

Aplicable a:

* 🛒 Retail
* 📦 Consumo masivo
* 🏪 Comercio electrónico
* 💲 Estrategias de precios
* 📈 Revenue Management

---

# 📥 Requisitos de datos

## Campos requeridos

| Campo             | Tipo     | Descripción                |
| ----------------- | -------- | -------------------------- |
| Fecha             | Fecha    | Período de análisis        |
| SKU               | Texto    | Identificador del producto |
| Producto          | Texto    | Nombre del producto        |
| Categoría         | Texto    | Categoría comercial        |
| Precio Unitario   | Numérico | Precio de venta            |
| Unidades Vendidas | Numérico | Cantidad vendida           |
| Ingresos          | Numérico | Ventas totales             |

> ℹ️ El visual requiere únicamente estas siete columnas. Todos los cálculos se realizan internamente en Vega.

---

# 📐 Cálculos realizados automáticamente

El objeto visual calcula internamente:

* Variación porcentual del precio.
* Variación porcentual de la demanda.
* Elasticidad arco.
* Promedios de referencia.
* Posición dentro de cada cuadrante.
* Clasificación de elasticidad.
* KPIs generales.

No se requieren medidas DAX ni columnas calculadas.

---

# 📊 Elementos visuales

## Diagrama de dispersión

Cada punto representa un producto o SKU.

Los ejes muestran:

* Variación porcentual del precio.
* Variación porcentual de la demanda.

---

## Cuadrantes

Las líneas de referencia dividen el gráfico en cuatro cuadrantes que facilitan la interpretación estratégica del comportamiento de los productos.

---

## Colores

Cada punto se colorea según su clasificación de elasticidad, permitiendo identificar rápidamente diferentes comportamientos comerciales.

---

## Tooltips

Al pasar el cursor sobre un punto se muestran detalles como:

* Producto
* Categoría
* Precio
* Unidades
* Variación del precio
* Variación de la demanda
* Elasticidad arco
* Clasificación

---

# 🧾 Indicadores (KPIs)

En la parte superior del visual se muestran indicadores resumidos como:

* Total de productos analizados.
* Elasticidad promedio.
* Productos con demanda elástica.
* Productos con demanda inelástica.
* Otras métricas relevantes calculadas automáticamente.

Todos los indicadores se actualizan dinámicamente con el contexto de filtros de Power BI.

---

# ⚙️ Configuración

No requiere parámetros adicionales.

Una vez asignadas las columnas necesarias, el visual realiza automáticamente todos los cálculos y genera la representación gráfica.

---

# 🧩 Uso en Deneb (Power BI)

1. Agregue un visual de Deneb al reporte.
2. Seleccione **Vega** como tipo de especificación.
3. Pegue el contenido completo del archivo JSON.
4. Asigne las siete columnas requeridas.

No es necesaria ninguna configuración adicional.

---

# ⚠️ Notas

* Todos los cálculos se realizan internamente mediante transformaciones de Vega.
* No requiere medidas DAX.
* No requiere tablas auxiliares.
* Compatible con filtros y segmentadores de Power BI.
* Diseñado para priorizar la interpretación visual y el análisis comercial.

---

# 📦 Metadatos

**Autor:** José Rafael Escalante

**Versión:** 1.0

**Tecnología:** Vega v6 · Deneb · Power BI

---

🧠 Este visual facilita el análisis de estrategias de precios al combinar medidas de elasticidad con una representación visual intuitiva, completamente autocontenida y lista para reutilizarse en diferentes conjuntos de datos.
