# Automotriz

Consultas requeridas: 
```MySQL

1. Obtener el historial de reparaciones de un vehículo específico

mysql> SELECT
    ->  r.fecha,
    ->  v.placa AS Placa_vehiculo,
    ->  CONCAT(e.nombre,' ',e.apellido) AS Empleado_Asignado,
    ->  r.costo_total AS Costo_Reparacion,
    ->  r.descripcion AS Detalles
    -> FROM reparacion AS r
    -> JOIN vehiculo AS v ON v.id = r.vehiculo_id
    -> JOIN marca AS m ON m.id = v.marca_id
    -> JOIN empleado AS e ON e.id = r.empleado_id
    -> WHERE v.placa = 'STU901';



+------------+----------------+-------------------+------------------+----------------------------------------+
| fecha      | Placa_vehiculo | Empleado_Asignado | Costo_Reparacion | Detalles                               |
+------------+----------------+-------------------+------------------+----------------------------------------+
| 2024-05-27 | STU901         | Pedro Hernández   |            99.99 | Detalle de los exteriores del vehículo |
+------------+----------------+-------------------+------------------+----------------------------------------+
1 row in set (0.01 sec)

```

```MySQL

2. Calcular el costo total de todas las reparaciones realizadas por un empleado
específico en un período de tiempo

mysql> SELECT
    ->     e.nombre,
    ->     e.apellido,
    ->     SUM(r.costo_total) AS Costo_Total_Reparaciones
    -> FROM
    ->     reparacion AS r
    -> JOIN
    ->     empleado AS e ON r.empleado_id = e.id
    -> WHERE
    ->     e.id = 4
    ->     AND r.fecha BETWEEN '2024-01-01' AND '2024-06-01'
    -> GROUP BY
    ->     e.nombre, e.apellido;
+--------+----------+--------------------------+
| nombre | apellido | Costo_Total_Reparaciones |
+--------+----------+--------------------------+
| María  | Sánchez  |                    39.99 |
+--------+----------+--------------------------+
1 row in set (0.00 sec)
```

```MySQL 
 3. Listar todos los clientes y los vehículos que poseen instalacion audio

mysql> SELECT
    ->     c.nombre AS Nombre_Cliente,
    ->     c.apellido AS Apellido_Cliente,
    ->     v.placa AS Placa_Vehiculo,
    ->     v.modelo AS Modelo_Vehiculo,
    ->     v.año_fabricacion AS Año_Fabricacion_Vehiculo,
    ->     r.descripcion AS Descripcion_Instalacion_Audio,
    ->     r.fecha AS Fecha_Instalacion
    -> FROM
    ->     cliente AS c
    -> JOIN
    ->     vehiculo AS v ON c.id = v.cliente_id
    -> JOIN
    ->     reparacion AS r ON v.id = r.vehiculo_id
    -> WHERE
    ->     r.descripcion LIKE '%instalacion audio%' OR r.descripcion LIKE '%instalación audio%';
+----------------+------------------+----------------+-----------------+--------------------------+-------------------------------+-------------------+
| Nombre_Cliente | Apellido_Cliente | Placa_Vehiculo | Modelo_Vehiculo | Año_Fabricacion_Vehiculo | Descripcion_Instalacion_Audio | Fecha_Instalacion |
+----------------+------------------+----------------+-----------------+--------------------------+-------------------------------+-------------------+
| Sofía          | Torres           | HIJ456         | CX-5            |                     2020 | instalacion audio             | 2020-04-14        |
+----------------+------------------+----------------+-----------------+--------------------------+-------------------------------+-------------------+
1 row in set (0.00 sec)
```

```MySQL 

4. Obtener la cantidad de piezas en inventario para cada pieza

mysql> SELECT
    ->     p.nombre AS Nombre_Pieza,
    ->     p.descripcion AS Descripcion_Pieza,
    ->     SUM(i.cantidad) AS Cantidad_En_Inventario
    -> FROM
    ->     pieza AS p
    -> JOIN
    ->     pieza_inventario AS pi ON p.id = pi.pieza_id
    -> JOIN
    ->     inventario AS i ON pi.inventario_id = i.id
    -> GROUP BY
    ->     p.nombre, p.descripcion
    -> ORDER BY
    ->     Cantidad_En_Inventario DESC;
+------------------------+-----------------------------------------------------------------------------+------------------------+
| Nombre_Pieza           | Descripcion_Pieza                                                           | Cantidad_En_Inventario |
+------------------------+-----------------------------------------------------------------------------+------------------------+
| Bujía de Encendido     | Bujía de encendido de platino para motores de alta eficiencia.              |                    200 |
| Bomba de Agua          | Bomba de agua de alta presión para sistemas de enfriamiento.                |                    200 |
| Bobina de Encendido    | Bobina de encendido para sistemas de encendido electrónico.                 |                    150 |
| Junta de Culata        | Junta de culata de motor para sellado hermético de cámaras de combustión.   |                    150 |
| Filtro de Aceite       | Filtro de aceite de alta calidad para motores de gasolina y diésel.         |                    100 |
| Amortiguador           | Amortiguador hidráulico para absorción de impactos en suspensión vehicular. |                    100 |
| Correa de Distribución | Correa de distribución de caucho reforzado para motores de gasolina.        |                     75 |
| Sensor de Oxígeno      | Sensor de oxígeno para monitoreo de mezcla de combustible y aire.           |                     75 |
| Pastillas de Freno     | Pastillas de freno semimetálicas para vehículos livianos.                   |                     51 |
| Radiador               | Radiador de aluminio de alto rendimiento para sistemas de enfriamiento.     |                     50 |
+------------------------+-----------------------------------------------------------------------------+------------------------+
10 rows in set (0.00 sec)

```

```MySQL 

5. Obtener las citas programadas para un día específico

mysql> SELECT
    ->     c.id AS Cita_Id,
    ->     cl.nombre AS Nombre_Cliente,
    ->     cl.apellido AS Apellido_Cliente,
    ->     v.placa AS Placa_Vehiculo,
    ->     c.fecha_hora AS Fecha_Hora_Cita
    -> FROM
    ->     cita AS c
    -> JOIN
    ->     cliente AS cl ON c.cliente_id = cl.id
    -> JOIN
    ->     vehiculo AS v ON c.vehiculo_id = v.id
    -> WHERE
    ->     DATE(c.fecha_hora) = '2024-06-10';
+---------+----------------+------------------+----------------+---------------------+
| Cita_Id | Nombre_Cliente | Apellido_Cliente | Placa_Vehiculo | Fecha_Hora_Cita     |
+---------+----------------+------------------+----------------+---------------------+
|       1 | Juan           | Pérez            | ABC123         | 2024-06-10 08:00:00 |
+---------+----------------+------------------+----------------+---------------------+
1 row in set (0.01 sec)

```

```MySQL 
 6. Generar una factura para un cliente específico en una fecha determinada

mysql> SELECT
    ->  f.id AS factura_id,
    ->  CONCAT(c.nombre, ' ', c.apellido) AS Cliente,
    ->  f.fecha AS fecha_factura,
    ->  f.total AS total_a_pagar
    -> FROM factura AS f
    -> JOIN cliente AS c ON c.id = f.cliente_id
    -> WHERE f.fecha = '2024-06-16' AND f.cliente_id = 2;
+------------+-------------+---------------+---------------+
| factura_id | Cliente     | fecha_factura | total_a_pagar |
+------------+-------------+---------------+---------------+
|          2 | María López | 2024-06-16    |        450.00 |
+------------+-------------+---------------+---------------+
1 row in set (0.00 sec)
```

```MySQL 

7. Listar todas las órdenes de compra y sus detalles

mysql> SELECT
    ->  od.orden_id AS orden_id,
    ->  p.nombre AS Pieza,
    ->  od.cantidad,
    ->  od.precio
    -> FROM orden_detalle AS od
    -> JOIN pieza AS p ON p.id = od.pieza_id;
+----------+------------------------+----------+--------+
| orden_id | Pieza                  | cantidad | precio |
+----------+------------------------+----------+--------+
|        1 | Filtro de Aceite       |       10 |  15.00 |
|        1 | Pastillas de Freno     |        5 |  25.00 |
|        2 | Correa de Distribución |       20 |  10.00 |
|        2 | Bujía de Encendido     |       15 |  30.00 |
|        3 | Bobina de Encendido    |        8 |  20.00 |
+----------+------------------------+----------+--------+
5 rows in set (0.01 sec)
```


```MySQL

 8. Obtener el costo total de piezas utilizadas en una reparación específica

mysql> SELECT
    ->  rp.reparacion_id,
    ->  p.nombre AS Pieza,
    ->  rp.cantidad AS Cantidad_Piezas,
    ->  SUM(rp.cantidad * pre.precio_proveedor) AS costo_total_piezas
    -> FROM reparacion_piezas AS rpz
    -> JOIN pieza AS p ON p.id = rp.pieza_id
    -> JOIN precio AS pre ON pre.pieza_id = rp.pieza_id
    -> WHERE rp.reparacion_id = 5
    -> GROUP BY rp.reparacion_id, p.nombre, rp.cantidad;
+---------------+---------------------+-----------------+--------------------+
| reparacion_id | Pieza               | Cantidad_Piezas | costo_total_piezas |
+---------------+---------------------+-----------------+--------------------+
|             5 | Bobina de Encendido |               2 |              25.00 |
+---------------+---------------------+-----------------+--------------------+
1 row in set (0.02 sec)
```

```MySQL

 9. Obtener el inventario de piezas que necesitan ser reabastecidas (cantidad
menor que un umbral)

mysql> SELECT
    ->   p.nombre AS Pieza,
    ->   i.cantidad AS Cantidad,
    ->   i.id AS inventario_id,
    ->   u.nombre AS Ubicacion_inventario
    -> FROM pieza AS p
    -> JOIN pieza_inventario AS pi ON pi.pieza_id = p.id
    -> JOIN inventario AS i ON i.id = pi.inventario_id
    -> JOIN ubicacion AS u ON u.id = i.ubicacion_id
    -> WHERE i.cantidad < 100;
+------------------------+----------+---------------+----------------------+
| Pieza                  | Cantidad | inventario_id | Ubicacion_inventario |
+------------------------+----------+---------------+----------------------+
| Pastillas de Freno     |       50 |             2 | Depósito de Piezas   |
| Radiador               |       50 |             2 | Depósito de Piezas   |
| Correa de Distribución |       75 |             3 | Almacén de Repuestos |
| Sensor de Oxígeno      |       75 |             3 | Almacén de Repuestos |
| Pastillas de Freno     |        1 |             6 | Almacén Principal    |
+------------------------+----------+---------------+----------------------+
5 rows in set (0.09 sec)
```

```MySQL

10. Obtener la lista de servicios más solicitados en un período específico

mysql> SELECT
    ->     s.nombre AS Servicio,
    ->     COUNT(rs.servicio_id) AS Cantidad_Solicitada
    -> FROM reparacion_servicio AS rs
    -> JOIN servicio AS s ON s.id = rs.servicio_id
    -> JOIN reparacion AS r ON r.id = rs.reparacion_id
    -> WHERE  r.fecha BETWEEN '2024-04-14' AND '2024-05-30'
    -> GROUP BY s.nombre
    -> ORDER BY Cantidad_Solicitada DESC
    -> LIMIT 4;
+------------------------+---------------------+
| Servicio               | Cantidad_Solicitada |
+------------------------+---------------------+
| Detalle de Exteriores  |                   4 |
| Rotación de Neumáticos |                   1 |
| Cambio de Batería      |                   1 |
| Reparación de Motor    |                   1 |
+------------------------+---------------------+
4 rows in set (0.05 sec)

```

```MySQL

11. Obtener el costo total de reparaciones para cada cliente en un período
específico

mysql> SELECT
    ->     c.id AS cliente_id,
    ->     c.nombre,
    ->     c.apellido,
    ->     c.email,
    ->     SUM(r.costo_total) AS total_reparaciones
    -> FROM
    ->     cliente c
    -> JOIN
    ->     vehiculo v ON c.id = v.cliente_id
    -> JOIN
    ->     reparacion r ON v.id = r.vehiculo_id
    -> WHERE
    ->     r.fecha BETWEEN '2023-01-01' AND '2023-12-31' -- Specify your date range here
    -> GROUP BY
    ->     c.id, c.nombre, c.apellido, c.email;
+------------+-----------+----------+----------------------+--------------------+
| cliente_id | nombre    | apellido | email                | total_reparaciones |
+------------+-----------+----------+----------------------+--------------------+
|         21 | JuanDiego | Conteras | mizamarzes@gmail.com |               2.21 |
+------------+-----------+----------+----------------------+--------------------+
1 row in set (0.05 sec)

```

```MySQL

12. Listar los empleados con mayor cantidad de reparaciones realizadas en un
período específico

SELECT
    CONCAT(e.nombre, ' ', e.apellido) AS Empleado, 
    COUNT(r.id) AS Cantidad_Reparaciones    
FROM reparacion AS r
JOIN empleado AS e ON e.id = r.empleado_id
WHERE r.fecha BETWEEN '2024-05-15' AND '2024-05-30'
GROUP BY e.id
ORDER BY Cantidad_Reparaciones DESC
LIMIT 3;

+-----------------+-----------------------+
| Empleado        | Cantidad_Reparaciones |
+-----------------+-----------------------+
| Pedro Hernández |                     2 |
| David Gutiérrez |                     2 |
| Elena Díaz      |                     1 |
+-----------------+-----------------------+
3 rows in set (0.04 sec)

```
   

```MySQL

13. Obtener las piezas más utilizadas en reparaciones durante un período
    específico


    SELECT
        p.nombre AS Pieza,
        COUNT(rp.pieza_id) AS Usos_en_Reparaciones
    FROM reparacion_piezas AS rp
    JOIN pieza AS p ON p.id = rp.pieza_id
    JOIN reparacion AS r ON r.id = rp.reparacion_id
    WHERE r.fecha BETWEEN '2024-05-14' AND '2024-05-30'
    GROUP BY Pieza
    ORDER BY Usos_en_Reparaciones DESC
    LIMIT 3;
    +------------------------+----------------------+
    | Pieza                  | Usos_en_Reparaciones |
    +------------------------+----------------------+
    | Filtro de Aceite       |                    1 |
    | Pastillas de Freno     |                    1 |
    | Correa de Distribución |                    1 |
    +------------------------+----------------------+
    -- En esta consulta se listan las 3 primeras piezas con mas usos en reparaicones entre el periodo '2024-05-14' hasta '2024-05-30'
    ```
    
14. Calcular el promedio de costo de reparaciones por vehículo

    ```mysql
    SELECT
        m.nombre AS Marca_vehiculo,
        v.modelo AS Modelo_vehiculo,
        AVG(r.costo_total) AS Costo_Promedio
    FROM reparacion AS r
    JOIN vehiculo AS v ON v.id = r.vehiculo_id
    JOIN marca AS m ON m.id = v.marca_id
    GROUP BY Marca_vehiculo, Modelo_vehiculo
    ORDER BY Costo_Promedio DESC;
    ```

15. Obtener el inventario de piezas por proveedor

    ```mysql
    SELECT 
        p.nombre AS Pieza,
        pro.nombre AS Proveedor,
        pi.inventario_id AS Inventario_id,
        i.cantidad AS Cantidad,
        u.nombre AS Ubicacion_Inventario
    FROM precio AS pre
    JOIN pieza AS p ON p.id = pre.pieza_id
    JOIN proveedor AS pro ON pro.id = pre.proveedor_id
    JOIN pieza_inventario AS pi ON pi.pieza_id = pre.pieza_id
    JOIN inventario AS i ON i.id = pi.inventario_id
    JOIN ubicacion AS u ON u.id = i.ubicacion_id
    ORDER BY Cantidad DESC;
    +------------------------+-----------------------------------------+------------+----------+
    | Pieza                  | Proveedor                               | Inventario | Cantidad |
    +------------------------+-----------------------------------------+------------+----------+
    | Bujía de Encendido     | Distribuidora de Repuestos Automotrices |          4 |      200 |
    | Bomba de Agua          | Autopartes Express                      |          4 |      200 |
    | Bobina de Encendido    | Herramientas de Calidad                 |          5 |      150 |
    | Junta de Culata        | Distribuidora Automotriz                |          5 |      150 |
    | Filtro de Aceite       | AutoPartes Directo                      |          1 |      100 |
    | Amortiguador           | Motores y Refacciones                   |          1 |      100 |
    | Correa de Distribución | Suministros Vehiculares                 |          3 |       75 |
    | Sensor de Oxígeno      | Distribuidora de Herramientas Mecánicas |          3 |       75 |
    | Pastillas de Freno     | Mecánica Avanzada                       |          2 |       50 |
    | Radiador               | Repuestos Automex                       |          2 |       50 |
    +------------------------+-----------------------------------------+------------+----------+
    -- En esta consulta se muestra el inventario de piezas por proveedor de forma de mayor a menor
    ```

16. Listar los clientes que no han realizado reparaciones en el último año

    ```mysql
    SELECT
        CONCAT(cli.nombre, ' ', cli.apellido) AS Cliente
    FROM cliente AS cli
    WHERE cli.id NOT IN(
        SELECT
            v.cliente_id
        FROM reparacion AS r
        JOIN vehiculo as v ON v.id = r.vehiculo_id
        WHERE DATE_FORMAT(r.fecha, '%Y') = '2024'   
    );
    +--------------------+
    | Cliente            |
    +--------------------+
    | JuanDiego Conteras |
    +--------------------+
    -- En esta consulta lista los clientes que no han hecho reparaciones en el año 2024 
    ```

17. Obtener las ganancias totales del taller en un período específico

    ```mysql
    SELECT 
        SUM(r.costo_total) AS Ganancias_Totales
    FROM reparacion AS r
    WHERE r.fecha BETWEEN '2024-05-14' AND '2024-05-30';
    +-------------------+
    | Ganancias_Totales |
    +-------------------+
    |           2545.01 |
    +-------------------+
    -- En esta consulta simplemente suma el costo de las reparaciones entre el periodo 2024-05-14 hasta 2024-05-30
    ```

18. Listar los empleados y el total de horas trabajadas en reparaciones en un
    período específico (asumiendo que se registra la duración de cada reparación)

    ```mysql
    -- Asumi que la duracion estandar por reparacion era de 8 horas, ya que no cuento con duracion_horas en la tabla reparacion
    SELECT
        CONCAT(e.nombre, ' ', e.apellido) AS Empleado,
        COUNT(r.id) * 8 AS Total_horas_Trabajadas
    FROM reparacion AS r
    JOIN empleado AS e ON e.id = r.empleado_id
    WHERE r.fecha BETWEEN '2024-05-14' AND '2024-05-30'
    GROUP BY e.id
    ORDER BY Total_horas_Trabajadas DESC;
    +-------------------+------------------------+
    | Empleado          | Total_horas_Trabajadas |
    +-------------------+------------------------+
    | David Gutiérrez   |                     16 |
    | Pedro Hernández   |                     16 |
    | Diego Rodríguez   |                     16 |
    | Carolina González |                      8 |
    | Ricardo Ruiz      |                      8 |
    | Andrea Gómez      |                      8 |
    | Fernando Torres   |                      8 |
    | Patricia Ramírez  |                      8 |
    | Gabriel Moreno    |                      8 |
    | Adriana Vega      |                      8 |
    | Roberto Gómez     |                      8 |
    | Ana Jiménez       |                      8 |
    | Javier López      |                      8 |
    | María Sánchez     |                      8 |
    | Laura Martínez    |                      8 |
    | Carmen Pérez      |                      8 |
    | Sara García       |                      8 |
    | Elena Díaz        |                      8 |
    +-------------------+------------------------+
    -- En esta consulta se listan los empleados y sus horas trabajadas entre el periodo de 2024-05-14 hasta 2024-05-30
    ```
    
19. Obtener el listado de servicios prestados por cada empleado en un período
    específico

    ```mysql
    SELECT DISTINCT
        CONCAT(e.nombre, ' ', e.apellido) AS Empleado,
        s.nombre AS Servicio_Prestado
    FROM reparacion AS r
    JOIN empleado AS e ON e.id = r.empleado_id
    JOIN reparacion_servicio AS rp ON rp.reparacion_id = r.id
    JOIN servicio AS s ON s.id = rp.servicio_id
    WHERE r.fecha BETWEEN '2024-05-14' AND '2024-05-30'
    ORDER BY Empleado ASC;
    +-------------------+----------------------------------+
    | Empleado          | Servicio_Prestado                |
    +-------------------+----------------------------------+
    | Adriana Vega      | Lavado Completo                  |
    | Ana Jiménez       | Detalle de Interiores            |
    | Andrea Gómez      | Reparación de Motor              |
    | Carmen Pérez      | Sustitución de batería           |
    | Carolina González | Rotación de Neumáticos           |
    | David Gutiérrez   | Alineación de dirección          |
    | David Gutiérrez   | Detalle de Exteriores            |
    | Diego Rodríguez   | Diagnóstico electrónico avanzado |
    | Diego Rodríguez   | Lavado Interior                  |
    | Elena Díaz        | Detalle de Exteriores            |
    | Fernando Torres   | Reparación de Transmisión        |
    | Gabriel Moreno    | Lavado Interior                  |
    | Javier López      | Detalle de Exteriores            |
    | Laura Martínez    | Cambio de amortiguadores         |
    | María Sánchez     | Cambio de aceite y filtro        |
    | Patricia Ramírez  | Lavado Exterior                  |
    | Pedro Hernández   | Reparación de sistema de frenos  |
    | Pedro Hernández   | Detalle de Exteriores            |
    | Ricardo Ruiz      | Cambio de Batería                |
    | Roberto Gómez     | Encerado                         |
    | Sara García       | Cambio de correa de distribución |
    +-------------------+----------------------------------+
    -- En esta consulta muestra los servicios ofrecidos por los empleados en el periodo 2024-05-14 hasta 2024-05-30
    ```
    
    ## Subconsultas
    
    1. Obtener el cliente que ha gastado más en reparaciones durante el último año.
    
    ```mysql
    SELECT 
        CONCAT(c.nombre, ' ', c.apellido) AS cliente
    FROM cliente AS c
    WHERE c.id = (
        SELECT
            v.cliente_id
        FROM reparacion AS r
        JOIN vehiculo AS v ON v.id = r.vehiculo_id
        WHERE YEAR(r.fecha) = '2024'
        GROUP BY v.cliente_id
        ORDER BY SUM(r.costo_total) DESC
        LIMIT 1
    );
    +--------------------+
    | cliente            |
    +--------------------+
    | JuanDiego Conteras |
    +--------------------+
    -- En esta consulta se muestra el cliente que mas gasto en una reparacion en el año 2024
    ```
    
    2. Obtener la pieza más utilizada en reparaciones durante el último mes
    
    ```mysql
    SELECT
        p.nombre AS pieza
    FROM pieza AS p
    WHERE p.id = (
        SELECT
            rp.pieza_id
        FROM reparacion_piezas AS rp
        JOIN reparacion AS r ON r.id = rp.reparacion_id
        WHERE MONTH(r.fecha) = 5
        GROUP BY rp.pieza_id
        ORDER BY COUNT(rp.pieza_id) DESC
        LIMIT 1
    );
    +------------------+
    | pieza            |
    +------------------+
    | Filtro de Aceite |
    +------------------+
    -- Esta consulta busca el nombre la pieza mas utilizada en el mes numero 5
    ```
    
    3. Obtener los proveedores que suministran las piezas más caras
         Diseño Automotriz

   ```mysql
   SELECT 
       p.nombre AS Proveedor
   FROM 
       proveedor AS p
   JOIN (
       SELECT 
           proveedor_id
       FROM 
           precio
       ORDER BY 
           precio_proveedor DESC
       LIMIT 3
   ) AS top_precios ON p.id = top_precios.proveedor_id;
   +-------------------------+
   | Proveedor               |
   +-------------------------+
   | Herramientas Esenciales |
   | Repuestos Automex       |
   | Motores y Refacciones   |
   +-------------------------+
   -- Esta consulta muestra los 3 proveedores que suministran las piezas mas caras
   ```

4. Listar las reparaciones que no utilizaron piezas específicas durante el último
   año

```mysql
SELECT
    p.nombre AS pieza
FROM pieza AS p
WHERE p.id NOT IN(
    SELECT
        rp.pieza_id
    FROM reparacion_piezas AS rp
    JOIN reparacion AS r ON r.id = rp.reparacion_id
    WHERE YEAR(r.fecha) = 2024
);
+---------------------------------+
| pieza                           |
+---------------------------------+
| Filtro de Aire                  |
| Lámpara Frontal                 |
| Pastilla de Embrague            |
| Termostato                      |
| Válvula EGR                     |
| Kit de Distribución             |
| Motor de Arranque               |
| Alternador                      |
| Sensor de Presión de Neumáticos |
| Detector de tombos              |
+---------------------------------+
-- En esta consulta se muestran las piezas que no fueron utilizadas en 2024
```

5. Obtener las piezas que están en inventario por debajo del 10% del stock inicial

   ```mysql
   SELECT
       p.nombre AS pieza
   FROM pieza AS p
   JOIN pieza_inventario AS pi ON pi.pieza_id = p.id
   WHERE pi.inventario_id IN (
       SELECT
           i.id
       FROM inventario AS i
       WHERE i.cantidad < (i.stock_inicial * 0.1)
   );
   
   +--------------------+
   | pieza              |
   +--------------------+
   | Pastillas de Freno |
   +--------------------+
   
   -- Esta consulta muestra las piezas en invenatario que tengan una cantidad menor al 10% del stock inicial
   ```

   ## Procedimientos Almacenados

1. Crear un procedimiento almacenado para insertar una nueva reparación.

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS insertar_reparacion;
CREATE PROCEDURE insertar_reparacion(
    IN fecha DATE,
    IN empleado_id INT,
    IN vehiculo_id INT,
    IN costo_total DECIMAL(10,2),
    IN descripcion TEXT
) 
BEGIN
    INSERT INTO reparacion(id, fecha, empleado_id, vehiculo_id, costo_total, descripcion)
    VALUES (NULL, fecha, empleado_id, vehiculo_id, costo_total, descripcion);
END $$
DELIMITER ;

-- --------------------------------------------------------------------------------------------------------

CALL insertar_reparacion('2024-6-5',7,12,234.5,"Cambio de llantas");

-- Este procedimiento solicita toda la informacion sobre la reparacion para posteriormente insertarla a la base de datos a la tabla reparacion
```

2. Crear un procedimiento almacenado para actualizar el inventario de una pieza.

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS actualizar_inv_pieza;
CREATE PROCEDURE actualizar_inv_pieza(
    IN pieza_id INT,
    IN p_nueva_cantidad INT
)
BEGIN
    UPDATE inventario AS i
    JOIN pieza_inventario AS pi ON pi.inventario_id = i.id
    SET i.cantidad = p_nueva_cantidad
    WHERE pi.pieza_id = pieza_id;
END $$
DELIMITER ;

-- ------------------------------------------------------------------------------------------------------

CALL actualizar_inv_pieza('1','43');

-- Este procedimiento actualiza la cantidad de inventario de una pieza, solicita la pieza y la nueva cantidad
```

3. Crear un procedimiento almacenado para eliminar una cita

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS eliminar_cita;
CREATE PROCEDURE eliminar_cita(
    IN cita_id_eliminar INT
)
BEGIN
    DELETE FROM cita_servicio
    WHERE cita_id = cita_id_eliminar;

    DELETE FROM cita
    WHERE id = cita_id_eliminar;
END $$
DELIMITER ;

-- --------------------------------------------------------------------------------------------------------

CALL eliminar_cita(10);

-- Este procedimiento elimina una cita, eliminando primero la cita_servicio que seria la id de la cita en la tabla intermedia y luego si eliminando la cita de la tabla principal 



```

4. Crear un procedimiento almacenado para generar una factura

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS crear_fractura;
CREATE PROCEDURE crear_factura(
    IN reparacion_id_crear INT
)
BEGIN 
    
    DECLARE total DECIMAL(10,2);
    DECLARE fecha DATE;
    DECLARE cliente_id INT; 


        SELECT 
            r.costo_total into total
        FROM reparacion AS r
        WHERE r.id = reparacion_id_crear;
    
    
        SELECT
            r.fecha into fecha
        FROM reparacion AS r
        WHERE r.id = reparacion_id_crear;
    
    
        SELECT
            v.cliente_id INTO cliente_id
        FROM reparacion AS r
        JOIN vehiculo AS v ON v.id = r.vehiculo_id
        WHERE r.id = reparacion_id_crear;
    
    INSERT INTO factura(id, fecha, cliente_id, total)
    VALUES (NULL, fecha,cliente_id,total);
END $$
DELIMITER ;

-- ----------------------------------------------------------------------------------------------------

CALL crear_factura(31);


-- Este procedimiento primero que todo busca almacena los resultados en variables para posteriormente crear la factura con la informacion de la reparacion y su costo total y su fecha


```

5. Crear un procedimiento almacenado para obtener el historial de reparaciones
     de un vehículo

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS historial_vehiculo;
   CREATE PROCEDURE historial_vehiculo(
    IN vehiculo_id_a_buscar INT
   )
   BEGIN
    SELECT
        r.id,
        r.fecha,
        r.empleado_id,
        r.vehiculo_id,
        r.costo_total,
        r.descripcion
    FROM reparacion AS r
    WHERE r.vehiculo_id = vehiculo_id_a_buscar;
   
   END $$
   DELIMITER ;
   
   -- ·······································································································
   
   CALL historial_vehiculo(1);
   +----+------------+-------------+-------------+-------------+-------------------------------------------+
   | id | fecha      | empleado_id | vehiculo_id | costo_total | descripcion                               |
   +----+------------+-------------+-------------+-------------+-------------------------------------------+
   |  8 | 2024-05-21 |           1 |           1 |      399.99 | Reparación de la transmisión del vehículo |
   | 23 | 2022-05-17 |          12 |           1 |       19.99 | Detalle de tapetes y ventanas EXTERIORES  |
   | 26 | 2024-05-17 |          12 |           1 |       19.99 | Detalle de tapetes y ventanas EXTERIORES  |
   +----+------------+-------------+-------------+-------------+-------------------------------------------+
   
   -- Este procedimiento lista el historial de reparaciones del vehiculo dependiendo de la vehiculo_id que le solicitemos
   ```

6. Crear un procedimiento almacenado para calcular el costo total de
     reparaciones de un cliente en un período

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS calcular_totalReparacion_periodo;
   CREATE PROCEDURE calcular_totalReparacion_periodo(
    IN cliente_id_calcular INT,
    IN periodo_inicio DATE,
    IN periodo_final DATE
   )
   BEGIN
    SELECT
        SUM(r.costo_total) AS Costo_Total_Reparaciones
    FROM reparacion AS r
    JOIN vehiculo AS v ON v.id = r.vehiculo_id
    WHERE v.cliente_id = cliente_id_calcular 
    AND 
    r.fecha BETWEEN periodo_inicio AND periodo_final;
   END $$
   DELIMITER ;
   
   -- .........................................................................................................
   
   CALL calcular_totalReparacion_periodo(1,'2020-01-4', '2024-12-6');
   
   +--------------------------+
   | Costo_Total_Reparaciones |
   +--------------------------+
   |                   439.97 |
   +--------------------------+
   
   -- Este procedimiento SUMA el costo total de todas las reparaciones que ha tenido un vehiculo de un respectivo cliente, solicita la id del cliente, periodo_inicio y periodo_final, en ese respectivo periodo se calcularan el costo total de las reparaciones
   
   
   ```

7. Crear un procedimiento almacenado para obtener la lista de vehículos que
     requieren mantenimiento basado en el kilometraje.

   ```mysql
   -- En este procedimiento utilice el año de fabricacion ya que no contaba con kilometraje en la tabla vehiculo
   DELIMITER $$
   DROP PROCEDURE IF EXISTS listar_vehiculos_mantenimiento_kilometraje;
   CREATE PROCEDURE listar_vehiculos_mantenimiento_kilometraje()
   BEGIN    
   
    SELECT
        v.id AS vehiculo_id,
        CONCAT(c.nombre, ' ', c.apellido) AS Cliente,
        v.placa AS Placa,
        v.marca_id AS Marca,
        v.modelo 
    FROM vehiculo AS v
    JOIN cliente AS c ON c.id = v.cliente_id
    WHERE (2024- v.año_fabricacion) > 5;
   END $$
   DELIMITER ; 
   
   -- -------------------------------------------------------------------------------------------------------
   
   CALL listar_vehiculos_mantenimiento_kilometraje();
   
   +-------------+-----------------+--------+-------+------------+
   | vehiculo_id | Cliente         | Placa  | Marca | modelo     |
   +-------------+-----------------+--------+-------+------------+
   |           2 | María López     | DEF456 |     2 | Mustang    |
   |           7 | José Rodríguez  | STU901 |     7 | C-Class    |
   |           8 | Marta Fernández | VWX234 |     8 | A4         |
   |          15 | Pedro Moreno    | QRS345 |    15 | Challenger |
   |          16 | Elena Rojas     | TUV678 |    16 | Wrangler   |
   |          21 | Mayra Luna      | BHM63D |    21 | Biwis      |
   +-------------+-----------------+--------+-------+------------+
   
   -- En este procedimiento listo los vehiculos que tienen una edad mayor a 5 años y entonces deben realizar mantenimiento
   
   
   ```

8. Crear un procedimiento almacenado para insertar una nueva orden de compra

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS insertar_ordenCompra $$
CREATE PROCEDURE insertar_ordenCompra(
    IN pieza_id_insert INT,
    IN cantidad_insert INT,
    IN fecha_insert DATE,
    IN proveedor_id_insert INT,
    IN empleado_id_insert INT
)
BEGIN
    DECLARE total DECIMAL(10, 2);

    SELECT (pre.precio_proveedor * cantidad_insert) INTO total
    FROM precio AS pre
    WHERE pre.pieza_id = pieza_id_insert AND pre.proveedor_id = proveedor_id_insert;
    
    IF total IS NULL THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Error: El Proveedor no cuenta con la venta de ese producto';
    END IF;

    INSERT INTO orden_compra(fecha, proveedor_id, empleado_id, total)
    VALUES (fecha_insert, proveedor_id_insert, empleado_id_insert, total);

END $$
DELIMITER ;

-- -------------------------------------------------------------------------------------------------------

CALL insertar_ordenCompra(1,10, '2022-11-1', 1, 5);


-- Esta consulta inserta la orden de compra y solicita todo los datos necesarios para insertarla, si el proveedor no coinicide con la pieza_id imprime un mensaje de error sobre eso

```

9. Crear un procedimiento almacenado para actualizar los datos de un cliente

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS actualizar_cliente;
CREATE PROCEDURE actualizar_cliente(
    IN cliente_id INT,
    IN nuevo_nombre VARCHAR(50),
    IN nuevo_apellido VARCHAR(50),
    IN nuevo_email VARCHAR(50)
)
BEGIN

    UPDATE cliente
    SET 
        nombre = nuevo_nombre,
        apellido = nuevo_apellido,
        email = nuevo_email
    WHERE id = cliente_id;

END $$
DELIMITER ;

-- ----------------------------------------------------------------------------------------

CALL actualizar_cliente(21, 'Mayra', 'Luna','mayraLuna232@email.com');

-- Este procedimiento actualiza la informacion del cliente, solicita cliente_id a modificar, nuevo nombre, apellido y email
```

10. Crear un procedimiento almacenado para obtener los servicios más solicitados
    en un período

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS listar_servicios_famosos;
CREATE PROCEDURE listar_servicios_famosos(
    IN periodo_inicio DATE,
    IN periodo_final DATE
)
BEGIN
    SELECT 
        s.nombre AS Servicios_Famosos
    FROM reparacion_servicio AS rs
    JOIN servicio AS s ON s.id = rs.servicio_id
    JOIN reparacion AS r ON r.id = rs.reparacion_id
    WHERE r.fecha BETWEEN periodo_inicio AND periodo_final
    GROUP BY rs.servicio_id
    ORDER BY COUNT(rs.servicio_id) DESC
    LIMIT 3;
END $$
DELIMITER ;

-- --------------------------------------------------------------------------------------------------------

CALL listar_servicios_famosos('2024-05-14', '2024-06-10');

+------------------------+
| Servicios_Famosos      |
+------------------------+
| Detalle de Exteriores  |
| Rotación de Neumáticos |
| Cambio de Batería      |
+------------------------+

-- Este procedimiento muestra los servicios mas famosos o mas solicitados en el periodo que le solicites y muestra los 3 mejores
```





