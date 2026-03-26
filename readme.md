# 📊 Proyecto Machine Learning - Predicción de Recompra de Clientes

Este repositorio contiene un proyecto de Machine Learning desarrollado para el **Máster Data Science & IA de Evolve Academy**. El objetivo principal de este análisis es predecir la **probabilidad de recompra** de los clientes a partir de su historial de transacciones, con el fin de diseñar estrategias de marketing y retención eficientes.

👤 **Autor:** Víctor Horcas Puertas

## 📁 Descripción del Dataset
El proyecto hace uso del dataset **Online Retail II**, dividido en dos archivos (2009-2010 y 2010-2011). Contiene todas las transacciones online de una tienda minorista en Reino Unido. 
* **Volumen inicial:** Más de 1 millón de registros (1,067,371 filas).
* **Atributos clave:** Invoice (Factura), StockCode, Description, Quantity (Cantidad), InvoiceDate (Fecha), Price (Precio), Customer ID (ID Cliente), Country (País).

## 🛠️ Estructura del Proyecto

El análisis está contenido en el archivo `mlproject.ipynb` y se divide en las siguientes fases:

### 1. Carga de Datos
* Importación de librerías esenciales (`pandas`, `numpy`, `matplotlib`, `seaborn`, etc.).
* Carga, concatenación y validación inicial de los datasets anuales.

### 2. Limpieza de Datos (Data Cleaning)
Para garantizar la calidad de la información alimentada al modelo, se aplicaron diversos filtros:
* Eliminación de facturas canceladas (aquellas que comienzan por `C`).
* Eliminación de registros sin `Customer ID`.
* Eliminación de cantidades inválidas (`Quantity <= 0`) y precios erróneos (`Price <= 0`).
* Limpieza de duplicados exactos en el conjunto de datos.
* Creación de la variable `Revenue` (Ingresos = Cantidad x Precio).

### 3. Análisis Exploratorio de Datos (EDA)
* Estadísticas descriptivas del comportamiento de las compras.
* Identificación y tratamiento de **valores atípicos (outliers)**. Se recortaron las variables de `Quantity` y `Price` al percentil 99.9% para evitar que casos erróneos o mayoristas extremos distorsionen el entrenamiento del modelo predictivo.

### 4. CUTOFF Y features
Se establece un CUTOFF (01 - octubre - 2011) y se agrupa todo el dataset por cliente.
Las features (variables) usadas para este proyecto son: 
- recencia : días desde la última compra hasta el corte
- frecuencia: número de facturas distintas
- revenue_total: gasto total acumulado
- ticket_medio: revenue medio por factura
- n_productos : productos únicos comprados
- n_paises: países desde los que ha comprado
- pais: país de compra más frecuente

### 5. Modelos
Tras el preprocesamiento (Pipeline), se ha realizado:
- Regresión Logística sin parámetros (baseline)
- Regresión Logística con GridSearchCV
- Decision Tree con GridSearchCV
- Random Forest con GridSearchCV
- XGBoost con GridSearchCV

El mejor modelo es **XGBoost con un valor AUC de 0.7938**.

### 6. Propuesta de Valor y Estrategia de Negocio
El modelo entrenado genera una **probabilidad de recompra ($P$)** para cada cliente, permitiendo al equipo de negocio tomar acciones personalizadas:

* 🟢 **$P > 0.7$ → Fidelización:** Envío de descuentos exclusivos y acceso anticipado a nuevos productos.
* 🟡 **$0.4 \le P \le 0.7$ → Nurturing:** Envío de newsletters personalizadas y recomendaciones basadas en el historial de compras.
* 🔴 **$P < 0.4$ → Retención:** Activación de campañas de reactivación y ofertas especiales de recuperación para evitar el *churn*.

## 🚀 Próximos Pasos (Trabajo Futuro)
Para futuras iteraciones y mejoras del modelo, se establecen las siguientes vías de acción:
- **Ajuste del umbral de decisión** (ej. 0.6 - 0.7) según el coste financiero de cometer errores (Falsos Positivos vs. Falsos Negativos).
- **Ingeniería de características (Feature Engineering):** Añadir variables como la estacionalidad de compra, días transcurridos entre compras y la categoría de producto preferida.
- **Reentrenamiento periódico:** Implementar un *pipeline* para actualizar el modelo conforme el comportamiento de los clientes vaya cambiando con datos recientes.
