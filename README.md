# Análisis de Datos Básico con Excel

## Caso práctico: diagnóstico de ventas, rentabilidad e inventario en AhorraMás S.A.C.

Este caso presenta un ejercicio **básico y práctico de análisis de datos utilizando Microsoft Excel**.

El objetivo es mostrar, de manera general, cómo pasar de un conjunto de archivos de datos a un diagnóstico que permita identificar problemas en las ventas, analizar la rentabilidad, revisar el inventario y generar recomendaciones para la empresa.

> **Nota:** Este es un ejemplo simplificado. En un entorno empresarial real se trabajarían muchos más datos, variables, procesos, áreas y herramientas especializadas.

---

# 1. El caso: AhorraMás S.A.C.

La gerencia plantea el siguiente problema:

> **"Queremos saber qué está pasando con las ventas recientemente. Hay tiendas que parecen vender bien, pero las ganancias están algo bajas. Además, queremos identificar productos que tengan potencial para venderse más."**

Para realizar el análisis se entrega información relacionada con:

* Ventas
* Productos
* Inventario
* Tiendas
* Objetivos

Estos archivos pueden encontrarse en diferentes formatos, como:

* Excel
* CSV
* Archivos de texto
* Bases de datos SQL Server
* Power BI
* Sistemas internos de la empresa
* Almacenamiento en la nube

El primer objetivo será convertir esta información en tablas estructuradas y relacionadas mediante identificadores como `id_producto`, `id_tienda`, entre otros.

### Archivos recibidos


| Archivo      | Información                                 |
| ------------ | ------------------------------------------- |
| `ventas`     | Información de las ventas realizadas        |
| `productos`  | Catálogo y características de los productos |
| `inventario` | Existencias disponibles                     |
| `tiendas`    | Información de las tiendas                  |
| `objetivos`  | Metas establecidas por la empresa           |


> ⚠️ **IMPORTANTE: trabajar siempre con copias de seguridad.**
>
> Antes de modificar, limpiar o transformar información empresarial, conserva una copia original de los archivos. Los errores, modificaciones accidentales o pérdidas de información pueden ocurrir en cualquier momento.

En este caso, los archivos fueron convertidos a formato CSV:

<img width="901" height="453" alt="Archivos de datos convertidos a CSV" src="https://github.com/user-attachments/assets/bee2af8e-98ce-4b00-914c-12de178253bf" />

---

# 2. Cargar los archivos en Excel

Abrimos Excel y nos dirigimos a:

**Datos → Obtener datos**

Esta herramienta permite importar información desde diferentes fuentes.

<img width="1917" height="1025" alt="Obtener datos en Excel" src="https://github.com/user-attachments/assets/91870f98-3086-4dac-a208-c6a1b2a28a97" />

En este ejemplo utilizaremos principalmente la opción **Cargar**.

La opción **Transformar** permite utilizar **Power Query**, una herramienta especialmente útil cuando necesitamos repetir procesos de limpieza y transformación.

Por ejemplo, si tenemos una carpeta con:

```text
mes1.csv
mes2.csv
mes3.csv
mes4.csv
...
```

podemos limpiar y transformar `mes1.csv` y posteriormente aplicar las mismas transformaciones a los demás archivos.

Importamos cada uno de los archivos necesarios.

<img width="1486" height="832" alt="Archivos importados en Excel" src="https://github.com/user-attachments/assets/27c69333-3ce1-49d5-b7c1-13b21db85bea" />

---

## 2.1. Limpieza de datos

Antes de comenzar el análisis debemos revisar la información.

Algunas tareas importantes son:

* Eliminar columnas que no utilizaremos.
* Revisar valores vacíos.
* Revisar valores duplicados.
* Comprobar los tipos de datos.
* Revisar fechas.
* Comprobar monedas.
* Detectar errores de escritura.
* Verificar identificadores.
* Revisar valores negativos.
* Confirmar que los datos tengan sentido para el negocio.

Por ejemplo, podemos encontrar valores como:

```text
S/10
$10
diez
1O
-S/10
```

Estos valores no necesariamente significan lo mismo.

Un valor como `-S/10` podría representar una devolución, un ajuste o un error de registro. **No debemos asumir su significado.**

Cuando exista una duda sobre el origen o significado de un dato, lo correcto es consultar al área responsable de la información.

> **Regla importante:** limpiar datos no significa cambiar información arbitrariamente. Significa identificar problemas y determinar, con evidencia, cuál debería ser el dato correcto.

---

## 2.2. Nombrar las tablas

Una vez limpia la información, seleccionamos la tabla y le asignamos un nombre.

Para la tabla de ventas utilizaremos:

```text
TablaVentas
```

Podemos hacer esto seleccionando la tabla y utilizando el cuadro de nombre ubicado en la parte superior izquierda de Excel.

<img width="1917" height="1078" alt="Nombrar tabla de ventas" src="https://github.com/user-attachments/assets/c28264b1-cc29-41b0-bce4-72853702b46f" />

De la misma manera podemos nombrar las demás tablas:

```text
TablaVentas
TablaProductos
TablaInventario
TablaTiendas
TablaObjetivos
```

---

# 3. Crear el modelo de datos

Una vez preparadas las tablas, podemos construir un modelo relacional utilizando **Power Pivot**.

Agregamos las tablas al:

**Modelo de datos**

<img width="1917" height="897" alt="Agregar tablas al modelo de datos" src="https://github.com/user-attachments/assets/2768741a-c2f1-46fb-9a79-735c1654099b" />

Después podemos utilizar la vista de datos para construir las relaciones.

<img width="1917" height="1078" alt="Vista del modelo de datos" src="https://github.com/user-attachments/assets/3cdb0ce2-c061-4bad-aeb9-6a96f8165aa3" />

<img width="1917" height="1078" alt="Tablas del modelo de datos" src="https://github.com/user-attachments/assets/27631dc8-a56f-4106-ae91-167ee237dca4" />

Las tablas se relacionan mediante identificadores.

Por ejemplo:

```text
TablaVentas.id_producto
        ↓
TablaProductos.id_producto
```

Esto es conceptualmente similar a trabajar con **claves primarias y relaciones entre tablas en SQL**.

<img width="1917" height="1078" alt="Crear relación entre tablas" src="https://github.com/user-attachments/assets/fd88553c-dca3-494e-b1af-fbfa843b27b1" />

También podemos reorganizar visualmente las tablas para que el modelo sea más fácil de entender.

<img width="1500" height="858" alt="Modelo de datos organizado" src="https://github.com/user-attachments/assets/99b6975d-b5a1-4011-bfd7-261b0dcbdc5c" />

---

# 4. Completar información mediante BUSCARV

Regresamos a `TablaVentas`.

Podemos agregar una columna llamada:

```text
Producto
```

<img width="1917" height="1027" alt="Agregar columna Producto" src="https://github.com/user-attachments/assets/5c4172c6-62a4-4098-801a-dae5733eb719" />

Antes de realizar la búsqueda podemos revisar la estructura de `TablaProductos`.

<img width="1917" height="1035" alt="Tabla Productos" src="https://github.com/user-attachments/assets/f94d72fb-b04d-4b47-b57f-d7201f04d3f2" />

Utilizaremos `BUSCARV()` para obtener el nombre del producto a partir de su identificador.

La estructura general es:

```excel
=BUSCARV(valor_buscado;tabla;columna;coincidencia)
```

En nuestro caso:

```excel
=BUSCARV([@[id_producto]];TablaProductos;2;FALSO)
```

Utilizamos `FALSO` para solicitar una coincidencia exacta.

<img width="1917" height="1025" alt="Función BUSCARV" src="https://github.com/user-attachments/assets/d629f8da-d65f-498a-a4f7-b2c28e4e419c" />

<img width="1917" height="1018" alt="Configuración de BUSCARV" src="https://github.com/user-attachments/assets/3f10305e-f6e2-4656-a69f-522b66c51ce4" />

<img width="1917" height="1020" alt="Resultado de BUSCARV" src="https://github.com/user-attachments/assets/43bb88ad-a779-4e05-940e-7fb25573e01f" />

Una vez introducida la fórmula, podemos copiarla al resto de las filas.

<img width="1917" height="1020" alt="Completar fórmula en la tabla" src="https://github.com/user-attachments/assets/a53c4552-8f5f-40d3-8229-2dc5ac293387" />

Repetimos el procedimiento para obtener información adicional como:

* Categoría
* Costo
* Tienda

<img width="1917" height="1027" alt="Completar información de productos y tiendas" src="https://github.com/user-attachments/assets/a6913572-6a34-4793-85f8-a0e3a500f0a1" />

<img width="1917" height="996" alt="Tabla de ventas completada" src="https://github.com/user-attachments/assets/bf84f6fd-3d5a-4e45-b32b-cec0ebb2e8b3" />

---

# 5. Crear las métricas

Ahora podemos agregar columnas calculadas para obtener los principales indicadores económicos de las ventas.

Trabajaremos con valores **sin IGV**, de acuerdo con el criterio definido para este análisis.

Las principales métricas serán:

### Venta bruta

```text
Venta_Bruta = Unidades × Precio_Unitario
```

### Venta neta

```text
Venta_Neta = Venta_Bruta × (1 - Descuento)
```

### Costo total

```text
Costo_Total = Unidades × Costo
```

### Margen

```text
Margen = Venta_Neta - Costo_Total
```

### Margen porcentual

```text
Margen_Porcentual = Margen / Venta_Neta
```

<img width="1917" height="1022" alt="Métricas calculadas" src="https://github.com/user-attachments/assets/d0f7f7a9-be96-443c-ad32-98825ee69c9e" />

Estas métricas serán la base de nuestro análisis.

---

# 6. Primer descubrimiento: analizar las ventas

Con nuestra tabla preparada podemos crear una **tabla dinámica**.

Nos posicionamos dentro de la tabla y seleccionamos:

**Insertar → Tabla dinámica**

<img width="1917" height="1026" alt="Crear tabla dinámica" src="https://github.com/user-attachments/assets/bd799a74-4a0f-4714-94ff-9f58ce2efd03" />

Si posteriormente modificamos los datos de `TablaVentas`, debemos actualizar la tabla dinámica.

Para hacerlo:

**Clic derecho → Actualizar**

<img width="1917" height="1021" alt="Actualizar tabla dinámica" src="https://github.com/user-attachments/assets/42e39194-598c-4869-b8d1-404a10740bfa" />

---

## 6.1. Ventas y margen por tienda

Podemos configurar la tabla dinámica de diferentes maneras.

Por ejemplo:

**Filas**

* Tienda

**Valores**

* Venta
* Margen

<img width="1916" height="1020" alt="Ventas y margen por tienda" src="https://github.com/user-attachments/assets/c3652bd5-982a-4819-b36c-8a2d3ce76103" />

<img width="1917" height="1027" alt="Tabla dinámica por tienda" src="https://github.com/user-attachments/assets/0e9e7884-39f2-4f21-b2ca-25ca11eaf12c" />

Es importante configurar correctamente los formatos:

* Moneda para valores monetarios.
* Porcentaje para indicadores porcentuales.
* Separadores de miles cuando sea necesario.

<img width="1917" height="1026" alt="Formato de valores" src="https://github.com/user-attachments/assets/0c26faa3-1ae4-4707-b3a3-2feb00b1b821" />

Durante la revisión podemos detectar que un porcentaje está siendo calculado de manera incorrecta porque supera el 100 %.

Esto ocurre porque estamos utilizando una suma cuando necesitamos calcular una proporción.

Por ello debemos revisar la configuración del campo y modificar el cálculo.

<img width="1917" height="1026" alt="Corrección de cálculo porcentual" src="https://github.com/user-attachments/assets/01e11fb9-0f9e-4be0-870a-30d21bed4ead" />

También podemos utilizar:

**Analizar tabla dinámica → Campos, elementos y conjuntos → Campo calculado**

<img width="1917" height="1078" alt="Crear campo calculado" src="https://github.com/user-attachments/assets/24413ee1-6137-4b94-a714-80a66b982c2b" />

Creamos un nuevo campo calculado.

<img width="1917" height="1078" alt="Nuevo campo calculado" src="https://github.com/user-attachments/assets/524a18e1-a3ec-48e5-a938-63d1f0d28e6e" />

Después podemos configurar el resultado como porcentaje y ajustar la posición de los valores para facilitar la interpretación.

<img width="1917" height="1078" alt="Resultado porcentual" src="https://github.com/user-attachments/assets/1f6d1f8d-6afc-4bf7-8ebd-2eb17d508928" />

---

# 7. Comparar los resultados contra el objetivo

Ahora debemos responder una pregunta importante:

> **¿Las tiendas están alcanzando el margen establecido por la empresa?**

Para ello obtenemos desde `TablaObjetivos` el valor de `Margen_Objetivo`.

<img width="1917" height="1078" alt="Obtener margen objetivo" src="https://github.com/user-attachments/assets/36578702-6d52-4155-9fc5-f28e34af7289" />

Creamos una nueva columna:

```text
Cumplimiento
```

Utilizando:

```text
Cumplimiento = Margen_Porcentual_Real / Margen_Objetivo
```

<img width="1917" height="1027" alt="Cálculo de cumplimiento" src="https://github.com/user-attachments/assets/6ca95405-ba22-4419-9e85-079bf782a590" />

Aquí aparece nuestro primer hallazgo importante:

> **La cuarta tienda no está alcanzando el margen objetivo.**

El objetivo representa un mínimo que la empresa espera alcanzar. Por lo tanto, si una tienda se encuentra por debajo de este valor, debemos investigar las causas.

---

# 8. Investigar el margen

Un margen bajo puede tener diferentes explicaciones.

Por ejemplo:

* Ventas insuficientes.
* Costos elevados.
* Descuentos excesivos.
* Productos con baja rentabilidad.
* Problemas de inventario.
* Problemas operativos.
* Errores en los datos.

En este caso analizaremos la información disponible dentro de la misma tabla dinámica.

<img width="1917" height="1078" alt="Investigación del margen" src="https://github.com/user-attachments/assets/940aa66e-5a9b-4d18-bcad-4dba8827e412" />

La cuarta tienda presenta aproximadamente:

```text
Ventas: S/ 91,856.29
```

Sin embargo, su margen disminuye considerablemente.

Esto nos indica algo importante:

**No basta con observar cuánto vende una tienda. También debemos analizar cuánto dinero conserva después de costos y descuentos.**

La gerencia podría observar solamente una caída de la venta final, pero el análisis debe profundizar para encontrar la causa.

---

# 9. Analizar producto por producto

Ahora necesitamos descubrir qué productos están afectando el resultado.

Creamos una nueva tabla dinámica utilizando `TablaVentas`.

Configuramos:

### Filas

* Tienda
* Producto

### Valores

* Ventas
* Margen
* Unidades

<img width="1917" height="1027" alt="Nueva tabla dinámica de productos" src="https://github.com/user-attachments/assets/91a97742-a3de-4d41-9d84-7c2d7c6ec6e2" />

<img width="1917" height="1078" alt="Ubicación de la tabla dinámica" src="https://github.com/user-attachments/assets/8c6645b5-8657-4062-b8aa-b8fda8da9c86" />

<img width="1917" height="1026" alt="Ventas por tienda y producto" src="https://github.com/user-attachments/assets/2477de85-6ea9-4922-a4b8-cd4fb12e14cf" />

<img width="1917" height="1026" alt="Análisis de productos" src="https://github.com/user-attachments/assets/86530308-658e-4990-9457-9fa32698e9fd" />

Aquí podemos observar que las ventas del **aceite** disminuyeron considerablemente en la tienda **AhorraMás Sur**.

<img width="1917" height="1025" alt="Caída de ventas del aceite" src="https://github.com/user-attachments/assets/5dd8f0a1-3f26-45c6-aec3-ec2148695a72" />

Pero todavía no podemos afirmar que el aceite sea la causa del problema.

> **Un buen análisis no consiste en encontrar el primer dato extraño y presentarlo como la causa.**

Debemos comparar la situación con otras tiendas y revisar variables adicionales.

---

# 10. Analizar los descuentos

Para investigar el caso de AhorraMás Sur podemos filtrar la tabla dinámica y observar únicamente esa tienda.

<img width="1917" height="1025" alt="Filtrar AhorraMás Sur" src="https://github.com/user-attachments/assets/358a853a-f4fa-41c1-b4df-be98029a466f" />

La investigación muestra que existe una relación entre los descuentos aplicados y la rentabilidad del producto.

Por ello, el problema comienza a tomar forma:

> **Las ventas del aceite disminuyeron, pero los descuentos aplicados también están afectando significativamente el margen.**

---

# 11. Segunda investigación: inventario

Todavía existe otra posibilidad:

> **¿El problema ocurrió porque la tienda se quedó sin productos?**

Para responderlo analizamos la tabla de inventario.

Primero agregamos información de productos para facilitar la lectura.

<img width="1917" height="1022" alt="Preparar tabla de inventario" src="https://github.com/user-attachments/assets/f7516005-ed99-4f73-ad0b-fdeb420c547c" />

Si aparece `####` en alguna celda, normalmente significa que el ancho de la columna no permite mostrar completamente el valor.

A continuación analizamos los datos de inventario.

<img width="1917" height="1077" alt="Análisis del inventario" src="https://github.com/user-attachments/assets/f6271906-3b60-4e1d-9490-b62047f2c483" />

El inventario no muestra una falta general de stock.

Por lo tanto:

* No encontramos evidencia de que el aceite se haya vendido menos por falta de inventario.
* También podemos identificar productos sobrantes.
* No encontramos un producto con un potencial de crecimiento evidente utilizando solamente este análisis.

---

# 12. Analizar el comportamiento del aceite

Para estimar qué podría haber ocurrido con el aceite, podemos comparar su comportamiento con otras tiendas.

Una alternativa es calcular un promedio de las ventas de las demás tiendas y utilizarlo como referencia.

<img width="1917" height="1022" alt="Comparación de ventas del aceite" src="https://github.com/user-attachments/assets/2a3eba4e-37c7-4ac2-bb68-d5219e64fe5f" />

También debemos revisar el comportamiento por fecha.

El análisis muestra que en determinadas fechas se aplicaron descuentos considerablemente altos.

En esos días el producto llegó a venderse en mayor cantidad, pero con una rentabilidad menor.

<img width="1916" height="1022" alt="Ventas y descuentos por fecha" src="https://github.com/user-attachments/assets/98d0ec9b-2c24-4595-bb41-865e4800c45e" />

Esto permite obtener una conclusión importante:

> **Vender más unidades no necesariamente significa ganar más dinero.**

Un descuento demasiado agresivo puede aumentar el volumen vendido, pero reducir considerablemente el margen.

---

# 13. Resumen de los hallazgos

Antes de construir el dashboard, resumimos los principales resultados.

### Problemas identificados

* 🔴 **AhorraMás Sur presenta un margen por debajo del objetivo.**
* 🔴 **Se detectaron descuentos excesivos que reducen la rentabilidad.**
* 🔴 **El aceite presenta una caída importante en las ventas de AhorraMás Sur.**
* 🟠 **Los descuentos más altos coinciden con días de mayor cantidad vendida, pero menor rentabilidad.**
* 🟢 **No se encontró falta de stock como causa principal del problema.**
* 🟡 **No se identificó un producto con potencial evidente de crecimiento mediante este análisis.**

Estos hallazgos serán la base para la presentación final.

---

# 14. Crear un dashboard

Una vez finalizado el análisis podemos crear visualizaciones que permitan comunicar los resultados de manera más sencilla.

El objetivo de un dashboard no es colocar muchas gráficas.

El objetivo es que una persona pueda responder rápidamente:

* ¿Cómo están las ventas?
* ¿Qué tienda presenta problemas?
* ¿Cuál es el margen?
* ¿Qué producto está afectando el resultado?
* ¿Cuál podría ser la causa?
* ¿Qué debería investigarse o corregirse?

Podemos utilizar **KPI (Indicadores Clave de Rendimiento)** para destacar los valores más importantes.

También podemos utilizar:

**Formato condicional → Barras de datos**

<img width="1917" height="1028" alt="Formato condicional para indicadores" src="https://github.com/user-attachments/assets/13675fe9-e043-4952-9e7b-1031a3d443e7" />

---

## 14.1. Crear gráficos

Nos posicionamos sobre la tabla dinámica que queremos representar y seleccionamos:

**Insertar → Gráfico**

Elegimos el tipo de gráfico que permita comprender la información rápidamente.

<img width="1917" height="1030" alt="Crear gráfico para el dashboard" src="https://github.com/user-attachments/assets/05b4335a-b47b-4b6e-ae9a-e3869ecc1751" />

<img width="1917" height="1022" alt="Dashboard de ventas" src="https://github.com/user-attachments/assets/1c36a659-e826-4f7e-88f3-5e50384b1150" />

Para comparar los productos de AhorraMás Sur podemos reutilizar una tabla dinámica existente y modificar sus filtros.

<img width="1917" height="1022" alt="Comparación de productos" src="https://github.com/user-attachments/assets/6055c554-b0a7-40bd-a113-9f82e808c140" />

<img width="1917" height="990" alt="Resultado final del análisis" src="https://github.com/user-attachments/assets/70eaeb23-faef-43a4-8532-0874d0876672" />

---

# 15. Presentación de resultados

Llegado el momento de presentar los resultados a la gerencia, no debemos mostrar únicamente las tablas.

La presentación debería responder tres preguntas:

### 1. ¿Qué está pasando?

AhorraMás Sur presenta un margen inferior al objetivo y existe una caída importante en las ventas de aceite.

### 2. ¿Por qué está pasando?

El análisis apunta a que los descuentos aplicados están afectando considerablemente la rentabilidad.

Además, no encontramos evidencia de que la causa principal sea la falta de inventario.

### 3. ¿Qué podemos hacer?

Algunas acciones que podrían evaluarse son:

* Revisar la política de descuentos.
* Analizar descuentos por fecha y producto.
* Evitar descuentos excesivos cuando reduzcan demasiado el margen.
* Revisar los productos con inventario sobrante.
* Monitorear especialmente el desempeño de AhorraMás Sur.
* Crear indicadores periódicos de ventas y rentabilidad.
* Continuar investigando otras variables operativas que podrían afectar el resultado.

> **Importante:** estas son recomendaciones derivadas del análisis. Antes de tomar decisiones definitivas, la empresa debería validar los resultados con las áreas responsables.

---

# 16. Entrega y manejo de los archivos

Una vez terminado el análisis, los archivos finales deben entregarse al sistema o a las personas responsables dentro de la empresa.

La información empresarial debe manejarse con responsabilidad.

### Buenas prácticas

* Mantener copias de seguridad.
* Conservar los archivos originales.
* Evitar modificar datos sin autorización.
* Proteger información confidencial.
* Compartir los resultados únicamente con las personas correspondientes.
* Documentar las transformaciones realizadas.
* Mantener separados los datos originales y los datos procesados.

El analista debe trabajar dentro del alcance de sus responsabilidades y evitar distribuir información empresarial innecesariamente.

---

# 17. Conclusión

Este ejercicio muestra un flujo básico de análisis de datos:

```text
Archivos - Importación -Limpieza - Tablas - Relaciones - Métricas - Tablas dinámicas - Investigación - Hallazgos - Dashboard - Recomendaciones
```

El punto más importante no es aprender una función específica de Excel.

El verdadero objetivo es aprender a **hacer preguntas sobre los datos y buscar evidencia antes de sacar conclusiones**.

En este caso comenzamos con una pregunta general:

> **"¿Por qué las ganancias están bajas si algunas tiendas parecen vender bien?"**

Y terminamos encontrando una posible explicación:

> **AhorraMás Sur presenta un margen inferior al objetivo y los descuentos aplicados, especialmente en determinados días, están reduciendo considerablemente la rentabilidad.**

Sin embargo, el análisis también permitió descartar parcialmente otras hipótesis, como una falta general de inventario.

---

# 18. Consideraciones finales

Este documento representa únicamente un ejemplo **básico y simplificado** de análisis de datos.

En un entorno empresarial real normalmente existirán:

* Mayor cantidad de registros.
* Más tiendas.
* Más productos.
* Diferentes periodos de tiempo.
* Costos logísticos.
* Devoluciones.
* Impuestos.
* Promociones.
* Proveedores.
* Clientes.
* Canales de venta.
* Variables financieras.
* Información histórica.
* Diferentes responsables y áreas.

También existen herramientas más especializadas para realizar análisis de datos, automatización y visualización.

Excel puede ser un excelente punto de partida para aprender estos conceptos, pero conforme aumenta la cantidad y complejidad de los datos, pueden ser necesarias herramientas como **Power Query, Power Pivot, Power BI, SQL, Python** u otras soluciones empresariales.

> **La herramienta es importante, pero el criterio analítico lo es todavía más.**

Un buen análisis debe realizarse con **integridad, objetividad, trazabilidad y responsabilidad**.

El objetivo final no es simplemente producir una gráfica o una tabla.

**El objetivo es convertir datos en información útil para tomar mejores decisiones.**

---

Referencias bibliográficas
Microsoft. (s. f.). Excel: ayuda y aprendizaje. Microsoft Support.
Microsoft. (s. f.). Power Query. Microsoft Learn.
Microsoft. (s. f.). Power Pivot. Microsoft Support.
