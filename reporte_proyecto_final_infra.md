## Entregables: 

Un reporte de la experimentación que incluya:
- Introducción.
- Las actividades realizadas y lecciones aprendidas.
- Incluir referencias en caso de tenerlas (IEEE, ACM). - Proyecto PDI.
- ArchivoSalida csv.
<br><br>

### Paso a paso del Pipeline

**1. Creamos una nueva transformación:**

![No se pudo cargar la imagen crear_transformacion.png](images/1-crear_transformacion.png)<br><br>




**2. Configuración de la conexión:**

>>Asignamos las instancias correspondientes a los parámetros Host Name, Database Name, Port Number, Username y Password.

![No se pudo cargar la imagen configuracion_bds.PNG](images/1-configuracion_bds.png)
<br><br>


**3. Nodo amesdbtemp - Agregar nodo tipo "Table input"**

>> Se crea el primer nodo con los datos de la tabla que tiene todas las relaciones a las demás tablas, la tabla **amesdbtemp**.  Dentro de este nodo se inserta un consulta sql para:
- Calcular el **Gr Liv Area**.
- Traer campos de las tablas **mssubclass**, **mszoning** y **saleproperty**.
- Extraer el **Mo Sold** y el **Yr Sold**
- Ordenar por el PID de ammesdbtemp.

![No se pudo cargar la imagen amesdbtemp.png](images/1-amesdbtemp.png)
<br><br><br>

**4. Nodo Select values - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conectándolo con el nodo anterior, amesdbtemp, para especificar qué columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformación.

![No se pudo cargar la imagen 2-select_values.png](images/2-select_values.png)
<br><br><br>

**5. Nodo Sort pid - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, Select values, para ordenar el resultado de la consulta del nodo amesdbtemp en orden ascendente por el campo pid.

![No se pudo cargar la imagen 3-sort_pid.png](images/3-sort_pid.png)
<br><br><br>


**6. Nodo floordetail - Agregar nodo tipo "Table input"**

>> Agregar nodo tipo Table input, e insertar una consulta sobre la tabla floordetail y en la misma consulta calcular el total de baños FullBath, HalfBath, Bedroom, agrupando por el pid y ordenando por este mismo.

![No se pudo cargar la imagen 4-floordetail.png](images/4-floordetail.png)
<br><br><br>


**7. Nodo Merge join - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid y floordetail.

![No se pudo cargar la imagen 5-merge_join.png](images/5-merge_join.png)
<br><br><br>


**7. Nodo Select values 2 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conectándolo con el nodo anterior, Merge join, para especificar qué columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformación.

![No se pudo cargar la imagen 5-select_values2.png](images/5-select_values2.png)
<br><br><br>


**8. Nodo Sort pid 2 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, Select values 2, para ordenar el resultado de la consulta del nodo Merge join en orden ascendente por el campo pid.

![No se pudo cargar la imagen 3-sort_pid2.png](images/6-sort_pid2.png)
<br><br><br>


**. Nodo AmesProperty - Agregar nodo tipo "CSV file input"**

>> Agregar un nodo tipo CSV file input para importar los datos del archivo AmesProperty.csv, para ordenar el resultado de la consulta del nodo Merge join en orden ascendente por el campo pid.

![No se pudo cargar la imagen 7-amesproperty.png](images/7-amesproperty.png)
<br><br><br>


**. Nodo Nvl - Agregar nodo tipo "Calculator"**

>> Agregar nodo tipo calculator enlazándolo al nodo AmesProperty para realizar la operación de reemplazar los valores nulos del campo Year Remod/Add por el valor del campo Year Built del nodo AmesProperty.

![No se pudo cargar la imagen 8-nvl.png](images/8-nvl.png)
<br><br><br>


**. Nodo Select values 3 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conectándolo con el nodo anterior, Nvl, para especificar qué columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformación.

![No se pudo cargar la imagen 9-select_values3.png](images/9-select_values3.png)
<br><br><br>


**. Nodo Sort pid 3 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, Select values 3, para ordenar el resultado de la consulta del nodo Nvl en orden ascendente por el campo PID.

![No se pudo cargar la imagen 10-sort_pid3.png](images/10-sort_pid3.png)
<br><br><br>


**. Nodo Merge join 2 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid 2 y Sort pid 3.

![No se pudo cargar la imagen 11-merge_join2.png](images/11-merge_join2.png)
<br><br><br>


**. Nodo Select values 5 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conectándolo con el nodo anterior, Merge join 2, para especificar qué columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformación.

![No se pudo cargar la imagen 12-select_values5.png](images/12-select_values5.png)
<br><br><br>


**. Nodo Sort pid 4 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, Select values 5, para ordenar el resultado de la consulta del nodo Merge join 2 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 13-sort_pid4.png](images/13-sort_pid4.png)
<br><br><br>


**. Nodo garage - Agregar nodo tipo "MongoDB input"**

>> Agregar un nodo tipo MongoDB input para importar los datos de la colección garage desde mongodb.

![No se pudo cargar la imagen 14-garage.png](images/14-garage.png)
<br><br><br>


**. Nodo Sort pid 5 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, garage, para ordenar el resultado del contenido de la colección garage en orden ascendente por el campo PID.

![No se pudo cargar la imagen 15-sort_pid5.png](images/15-sort_pid5.png)
<br><br><br>


**. Nodo Merge join 3 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid 4 y Sort pid 5.

![No se pudo cargar la imagen 16-merge_join3.png](images/16-merge_join3.png)
<br><br><br>


**. Nodo Select values 6 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conectándolo con el nodo anterior, Merge join 3, para especificar qué columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformación.

![No se pudo cargar la imagen 17-select_values6.png](images/17-select_values6.png)
<br><br><br>



**. Nodo Sort rows 2 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uniéndolo al nodo anterior, Select values 6, para ordenar el resultado del contenido del nodo Merge join 3 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 18-sort_rows2.png](images/18-sort_rows2.png)
<br><br><br>