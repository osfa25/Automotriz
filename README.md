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
