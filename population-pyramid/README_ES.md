# Pirámide poblacional - Especificación Vega para Deneb

![Pirámide poblacional](https://sentidoanalitica.com/wp-content/uploads/2026/07/population-pyramid.png)

Esta carpeta contiene especificaciones Vega diseñadas para usarse con Deneb en Power BI:

`population-pyramid.json`: especificación Vega (versión en inglés)

`piramide-poblacional.json`: especificación Vega (versión en español)

`Population-pyramid-v1.pbix`: archivo de Power BI con el gráfico renderizado en Deneb

Estas especificaciones son completas e independientes, y están enfocadas en claridad analítica, lógica de comparación y uso práctico en reportes.

---

## 🚀 ¿Qué hace este visual?

Este visual es una pirámide poblacional diseñada para comparar la distribución de personas por edad y sexo en uno o dos años seleccionados.

Permite:

* Visualizar la estructura poblacional por bandas de edad y sexo.
* Comparar el año visible más reciente contra un segundo año histórico cuando está disponible.
* Mostrar los valores masculinos a la izquierda y los femeninos a la derecha.
* Mostrar etiquetas de edad centradas para facilitar la lectura.
* Resaltar la banda de edad con mayor variación cuando se seleccionan dos años.
* Mostrar totales, variaciones absolutas y cambios porcentuales en tarjetas KPI.
* Usar tooltips para inspeccionar edad, sexo, población y contexto de comparación.

El visual resulta especialmente útil cuando necesitas entender la estructura demográfica, la concentración por edades y cómo cambia la población a lo largo del tiempo.

---

## 📦 ¿Qué incluye?

Esta carpeta incluye todo lo necesario para usar el visual en Deneb:

* `population-pyramid.json` - especificación Vega en inglés.
* `piramide-poblacional.json` - especificación Vega en español.
* `Population-pyramid-v1.pbix` - archivo de ejemplo de Power BI.

---

## 📥 Requisitos de datos

El visual espera cuatro campos del modelo:

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| year | Numérico | Año del registro. |
| age | Numérico | Edad de la persona o grupo poblacional. |
| sex | Numérico | Código de sexo utilizado por el modelo. La especificación actual asigna `1` a masculino y `0` a femenino. |
| people | Numérico | Cantidad de población para la fila. |

> ℹ️ El visual funciona mejor cuando los datos ya están limpios y cada fila representa un año, edad, sexo y cantidad de población.

---

## 📐 Cálculos automáticos

El visual realiza los siguientes cálculos dentro de Vega:

* Convierte los campos de entrada a valores numéricos.
* Agrupa los registros por año, edad y sexo.
* Conserva el año visible más reciente como vista actual.
* Usa un segundo año seleccionado como vista de comparación cuando está disponible.
* Agrega totales por sexo y banda de edad.
* Calcula totales actuales, totales de comparación, variación absoluta y variación relativa.
* Identifica la banda de edad con mayor cambio al comparar dos años.
* Formatea dinámicamente los valores de los KPI y de los tooltips.

No se requieren medidas DAX adicionales ni transformaciones extra en Power Query para la lógica base.

---

## 📊 Elementos visuales

### Barras poblacionales

* Los valores masculinos se dibujan a la izquierda del eje central.
* Los valores femeninos se dibujan a la derecha del eje central.
* Las barras se desplazan ligeramente para mantener legibles las etiquetas centrales.
* El año activo se representa como la forma principal de la pirámide.

### Capa de comparación

* Cuando se selecciona un segundo año, aparece una capa de comparación sobre la pirámide principal.
* Esta capa destaca el cambio relativo entre el año actual y el año de comparación.
* Ayuda a identificar dónde creció o disminuyó la estructura poblacional.

### Etiquetas de edad centradas

* Las etiquetas de edad se muestran en el centro del gráfico.
* Las edades se agrupan en bandas de cinco años.
* Esto mejora la legibilidad y mantiene el gráfico simétrico.

### Tarjetas KPI

Encima del gráfico, el visual muestra:

* Población total actual
* Población total de comparación
* Variación absoluta
* Variación relativa
* Banda de edad con mayor cambio

Estos indicadores se actualizan automáticamente con el contexto de filtros seleccionado.

### Tooltips

El tooltip muestra:

* Edad
* Sexo
* Población
* Porcentaje del total del sexo
* Contexto del año activo o del año de comparación

Cuando la comparación está activa, el tooltip también expone la variación relativa.

---

## 🧭 Cómo leer el gráfico

El gráfico está diseñado para una lectura demográfica rápida:

* El eje central representa las bandas de edad.
* El lado izquierdo representa la población masculina.
* El lado derecho representa la población femenina.
* La longitud de cada barra representa la cantidad de población.
* Las tarjetas KPI resumen el cambio general en el contexto seleccionado.

Esto hace que el visual sea útil para comparar la estructura poblacional entre años, regiones o segmentos.

---

## 🧩 Cómo usarlo en Deneb

1. Agrega un visual **Deneb** a tu reporte de Power BI.
2. Elige **Vega** como tipo de especificación.
3. Pega el contenido completo de `population-pyramid.json` o `piramide-poblacional.json`.
4. Mapea tus campos a `year`, `age`, `sex` y `people`.
5. Opcionalmente filtra el reporte a uno o dos años para activar la vista de comparación.

No se requiere configuración adicional más allá del mapeo de campos.

---

## 💡 Casos de uso recomendados

Este visual es útil para:

* Análisis demográfico
* Reportes de estructura poblacional
* Paneles del sector público
* Planeación sanitaria y social
* Análisis territorial
* Comparación de bandas de edad en el tiempo
* Análisis de distribución por sexo

---

## ⚠️ Notas y supuestos

* Asume entradas numéricas para año, edad, sexo y población.
* Usa `1` para masculino y `0` para femenino en el mapeo actual.
* La vista de comparación aparece solo cuando hay dos años disponibles en el contexto de filtros.
* Está diseñado para priorizar legibilidad y comparación equilibrada por encima de un estilo decorativo.

---