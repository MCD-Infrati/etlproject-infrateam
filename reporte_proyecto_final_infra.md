## Entregables: 

Un reporte de la experimentaci�n que incluya:
- Introducci�n.
- Las actividades realizadas y lecciones aprendidas.
- Incluir referencias en caso de tenerlas (IEEE, ACM). - Proyecto PDI.
- ArchivoSalida csv.
<br><br>

### Paso a paso del Pipeline

**1. Creamos una nueva transformaci�n:**

![No se pudo cargar la imagen crear_transformacion.png](images/1-crear_transformacion.png)<br><br>




**2. Configuraci�n de la conexi�n:**

>>Asignamos las instancias correspondientes a los par�metros Host Name, Database Name, Port Number, Username y Password.

![No se pudo cargar la imagen configuracion_bds.PNG](images/1-configuracion_bds.png)
<br><br>


**3. Nodo amesdbtemp - Agregar nodo tipo "Table input"**

>> Se crea el primer nodo con los datos de la tabla que tiene todas las relaciones a las dem�s tablas, la tabla **amesdbtemp**.  Dentro de este nodo se inserta un consulta sql para:
- Calcular el **Gr Liv Area**.
- Traer campos de las tablas **mssubclass**, **mszoning** y **saleproperty**.
- Extraer el **Mo Sold** y el **Yr Sold**
- Ordenar por el PID de ammesdbtemp.

![No se pudo cargar la imagen amesdbtemp.png](images/1-amesdbtemp.png)
<br><br><br>

**4. Nodo Select values - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, amesdbtemp, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 2-select_values.png](images/2-select_values.png)
<br><br><br>

**5. Nodo Sort pid - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values, para ordenar el resultado de la consulta del nodo amesdbtemp en orden ascendente por el campo pid.

![No se pudo cargar la imagen 3-sort_pid.png](images/3-sort_pid.png)
<br><br><br>


**6. Nodo floordetail - Agregar nodo tipo "Table input"**

>> Agregar nodo tipo Table input, e insertar una consulta sobre la tabla floordetail y en la misma consulta calcular el total de ba�os FullBath, HalfBath, Bedroom, agrupando por el pid y ordenando por este mismo.

![No se pudo cargar la imagen 4-floordetail.png](images/4-floordetail.png)
<br><br><br>


**7. Nodo Merge join - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid y floordetail.  Utilizamos LEFT OUTER JOIN para conservar todos los datos de la tabla resultado del paso Sort pid y adicionando los registros de la tabla floordetail cuyo pid coincida con los registros contenidos en el paso Sort pid.

![No se pudo cargar la imagen 5-merge_join.png](images/5-merge_join.png)
<br><br><br>


**7. Nodo Select values 2 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 5-select_values2.png](images/5-select_values2.png)
<br><br><br>


**8. Nodo Sort pid 2 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 2, para ordenar el resultado de la consulta del nodo Merge join en orden ascendente por el campo pid.

![No se pudo cargar la imagen 3-sort_pid2.png](images/6-sort_pid2.png)
<br><br><br>


**. Nodo AmesProperty - Agregar nodo tipo "CSV file input"**

>> Agregar un nodo tipo CSV file input para importar los datos del archivo AmesProperty.csv, para ordenar el resultado de la consulta del nodo Merge join en orden ascendente por el campo pid.

![No se pudo cargar la imagen 7-amesproperty.png](images/7-amesproperty.png)
<br><br><br>


**. Nodo Nvl - Agregar nodo tipo "Calculator"**

>> Agregar nodo tipo calculator enlaz�ndolo al nodo AmesProperty para realizar la operaci�n de reemplazar los valores nulos del campo Year Remod/Add por el valor del campo Year Built del nodo AmesProperty.

![No se pudo cargar la imagen 8-nvl.png](images/8-nvl.png)
<br><br><br>



**. Nodo Value mapper - Agregar nodo tipo "Value mapper"**

>> Agregar nodo tipo Value mapper enlaz�ndolo al nodo anterior, Nvl, para realizar la operaci�n de reemplazar los valores del campo Condition 1 a nombres mas extensos para hacerlos mas descriptivos.

![No se pudo cargar la 7b-value_mapper.png](images/8b-value_mapper.png)
<br><br><br>


**. Nodo Select values 3 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Value mapper, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 9-select_values3.png](images/9a-select_values3.png)
<br><br><br>


**. Nodo Sort pid 3 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 3, para ordenar el resultado de la consulta del nodo Nvl en orden ascendente por el campo PID.

![No se pudo cargar la imagen 10-sort_pid3.png](images/10-sort_pid3.png)
<br><br><br>


**. Nodo Merge join 2 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid 2 y Sort pid 3.  Utilizamos INNER JOIN para conservar los datos que coincidan en las dos tablas obtenidas del paso Sort pid 2 y Sort pid 3.

![No se pudo cargar la imagen 11-merge_join2.png](images/11-merge_join2.png)
<br><br><br>


**. Nodo Select values 5 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join 2, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 12-select_values5.png](images/12-select_values5.png)
<br><br><br>


**. Nodo Sort pid 4 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 5, para ordenar el resultado de la consulta del nodo Merge join 2 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 13-sort_pid4.png](images/13-sort_pid4.png)
<br><br><br>


**. Nodo garage - Agregar nodo tipo "MongoDB input"**

>> Agregar un nodo tipo MongoDB input para importar los datos de la colecci�n garage desde mongodb.  Aqu� obtenemos como resultado todos los datos de la tabla a la izquierda y los de la colecci�n garage cuyo pid coincida con los registros de la tabla que ya traemos.

![No se pudo cargar la imagen 14-garage.png](images/14-garage.png)
<br><br><br>


**. Nodo Sort pid 5 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, garage, para ordenar el resultado del contenido de la colecci�n garage en orden ascendente por el campo PID.

![No se pudo cargar la imagen 15-sort_pid5.png](images/15-sort_pid5.png)
<br><br><br>


**. Nodo Merge join 3 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort pid 4 y Sort pid 5.  Utilizamos LEFT OUTER JOIN para conservar todos los datos de la tabla resultado del paso Sort pid 4 y adicionando los registros de la tabla obtenida en el paso Sort pid 5 cuyo pid coincida con los registros contenidos en el paso Sort pid 4.

![No se pudo cargar la imagen 16-merge_join3.png](images/16-merge_join3.png)
<br><br><br>


**. Nodo Select values 6 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join 3, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 17-select_values6.png](images/17-select_values6.png)
<br><br><br>



**. Nodo Sort rows 2 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 6, para ordenar el resultado del contenido del nodo Merge join 3 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 18-sort_rows2.png](images/18-sort_rows2.png)
<br><br><br>


**. Nodo pool - Agregar nodo tipo "MongoDB input"**

>> Agregar un nodo tipo MongoDB input para importar los datos de la colecci�n pool desde mongodb.

![No se pudo cargar la imagen 19-pool.png](images/19-pool.png)
<br><br><br>


**. Nodo Sort rows - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, pool, para ordenar el resultado del contenido de la colecci�n pool en orden ascendente por el campo PID.

![No se pudo cargar la imagen 20-sort_rows.png](images/20-sort_rows.png)
<br><br><br>


**. Nodo Merge join 4 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort rows 2 y Sort rows.  Aqu� obtenemos como resultado todos los datos de la tabla obtenida en Sort rows 2 y los de la colecci�n pool cuyo PID coincida con los registros de la tabla que ya traemos.

![No se pudo cargar la imagen 21-merge_join4.png](images/21-merge_join4.png)
<br><br><br>



**. Nodo Select values 4 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join 4, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 22-select_values4.png](images/22-select_values4.png)
<br><br><br>



**. Nodo Sort rows 4 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 4, para ordenar el resultado del contenido del nodo Select values 4 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 20-sort_rows.png](images/20-sort_rows.png)
<br><br><br>



**. Nodo bsmt - Agregar nodo tipo "MongoDB input"**

>> Agregar un nodo tipo MongoDB input para importar los datos de la colecci�n bsmt desde mongodb.

![No se pudo cargar la imagen 22-bsmt.png](images/22-bsmt.png)
<br><br><br>




**. Nodo Sort rows 5 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, bsmt, para ordenar el resultado del contenido de la colecci�n bsmt en orden ascendente por el campo PID.

![No se pudo cargar la imagen 23-sort_rows5.png](images/23-sort_rows5.png)
<br><br><br>


**. Nodo Merge join 5 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort rows 4 y Sort rows 5.  Aqu� obtenemos como resultado todos los datos de la tabla obtenida en Sort rows 4 y los de la colecci�n bsmt cuyo PID coincida con los registros de la tabla de Sort rows 4 que ya tra�amos.

![No se pudo cargar la imagen 24-merge_join5.png](images/24-merge_join5.png)
<br><br><br>



**. Nodo Select values 7 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join 5, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 25-select_values7.png](images/25-select_values7.png)
<br><br><br>




**. Nodo Sort rows 6 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, Select values 7, para ordenar el resultado del contenido del nodo Select values 7 en orden ascendente por el campo pid.

![No se pudo cargar la imagen 26-sort_rows6.png](images/26-sort_rows6.png)
<br><br><br>


**. Nodo misc - Agregar nodo tipo "MongoDB input"**

>> Agregar un nodo tipo MongoDB input para importar los datos de la colecci�n **misc** desde mongodb.

![No se pudo cargar la imagen 27-misc.png](images/27-misc.png)
<br><br><br>


**. Nodo Sort rows 3 - Agregar nodo tipo "Sort"**

>> Agregar un nodo tipo Sort uni�ndolo al nodo anterior, misc, para ordenar el resultado del contenido de la colecci�n misc en orden ascendente por el campo PID.

![No se pudo cargar la imagen 27-sort_rows3.png](images/27-sort_rows3.png)
<br><br><br>


**. Nodo Merge join 6 - Agregar nodo tipo "Merge join"**

>> Agregar nodo tipo merge join para unir los datos del resultado del paso Sort rows 6 y Sort rows 3.  Aqu� obtenemos como resultado todos los datos de la tabla obtenida en Sort rows 6 y los de la colecci�n misc cuyo PID coincida con los registros de la tabla de Sort rows 6 que ya tra�amos.

![No se pudo cargar la imagen 28-merge_join6.png](images/28-merge_join6.png)
<br><br><br>



**. Nodo Select values 8 - Agregar nodo tipo "Select values"**

>> Se agrega un nodo tipo Select values conect�ndolo con el nodo anterior, Merge join 6, para especificar qu� columnas se requieren que sigan en el flujo de datos en la siguiente etapa de transformaci�n.

![No se pudo cargar la imagen 29-select_values8.png](images/29-select_values8.png)
<br><br><br>


**. Nodo if field value is null - Agregar nodo tipo "Utility"**

>> Se agrega un nodo tipo Utility conect�ndolo con el nodo anterior, Select values 8, para configurar reemplazar valores nulos por 0 o por NA en todas las columnas de la tabla obtenida en el nodo Merge join 6.

![No se pudo cargar la imagen 30-if_field_value_is_null.png](images/30-if_field_value_is_null.png)
<br><br><br>


**. Nodo ArchivoSalida - Agregar nodo tipo "Text file output"**

>> Se agrega un nodo tipo Text file output conect�ndolo con el nodo anterior, if field value is null, para generar el archivo csv de salida que contendr� el resultado de las transformaciones, este nodo generar� un archivo que se guarda en una ruta espec�fica.

![No se pudo cargar la imagen 31-archivo_salida.png](images/31-archivo_salida.png)
<br><br><br>