# Juan Diego Contreras Melendez (Gourmet Delight)

## Consultas Requeridas

1. Obtener la lista de todos los menús con sus precios

   ```mysql
   SELECT
    m.nombre AS Menu,
    m.precio AS Precio
   FROM menus AS m;
   ```

2. Encontrar todos los pedidos realizados por el cliente 'Juan Perez'

   ```mysql
   SELECT 
    p.id,
    p.fecha,
    p.total
   FROM pedidos AS p
   WHERE p.id_cliente = 1;
   ```

3. Listar los detalles de todos los pedidos, incluyendo el nombre del menú, cantidad y precio unitario

   ```mysql
   SELECT
    dp.id_pedido AS PedidoID,
    m.nombre AS Menu,
    dp.cantidad,
    dp.precio_unitario
   FROM detallesPedidos AS dp
   JOIN menus AS m ON m.id = dp.id_menu;
   ```

4. Calcular el total gastado por cada cliente en todos sus pedidos

   ```mysql
   SELECT
    c.nombre,
    SUM(p.total) AS TotalGastado
   FROM pedidos AS p
   JOIN clientes AS c ON c.id = p.id_cliente
   GROUP BY c.id;
   ```

5. Encontrar los menús con un precio mayor a $10

   ```mysql
   SELECT
    m.nombre,
    m.precio
   FROM menus AS m
   WHERE m.precio > 10;
   ```

6. Obtener el menú más caro pedido al menos una vez

   ```mysql
   SELECT
    m.nombre,
    (dp.precio_unitario * dp.cantidad) AS precio
   FROM detallesPedidos AS dp
   JOIN menus AS m ON m.id = dp.id_menu
   ORDER BY precio DESC, m.nombre DESC
   LIMIT 1;
   ```

7. Listar los clientes que han realizado más de un pedido

   ```mysql
   SELECT DISTINCT
    c.nombre,
    c.correo_electronico AS correo_electronico
   FROM clientes AS c
   JOIN pedidos AS p ON c.id = p.id_cliente
   WHERE 1 < (
    SELECT
        COUNT(p.id_cliente) AS contador
    FROM pedidos AS p
    WHERE p.id_cliente = c.id
   );
   ```

8. Obtener el cliente con el mayor gasto total

   ```mysql
   SELECT
    c.nombre
   FROM clientes AS c
   WHERE c.id = (
    SELECT
        p.id_cliente
    FROM pedidos AS p
    GROUP BY p.id_cliente
    ORDER BY SUM(p.total) DESC
    LIMIT 1
   );
   ```

9. Mostrar el pedido más reciente de cada cliente

   ```mysql
   SELECT
    c.nombre,
    p.fecha,
    p.total
   FROM pedidos AS p
   JOIN clientes AS c ON p.id_cliente = c.id
   JOIN(
    SELECT
        p.id_cliente,
        MAX(p.fecha) AS MaxDate
    FROM pedidos AS p
    GROUP BY id_cliente
   ) AS fecha_max ON p.id_cliente = fecha_max.id_cliente
   AND
   p.fecha =  fecha_max.MaxDate;
   ```

10. Obtener el detalle de pedidos (menús y cantidades) para el cliente 'Juan Perez'.

    ```mysql
    SELECT
        dp.id_pedido AS id,
        m.nombre,
        dp.cantidad,
        dp.precio_unitario
    FROM detallesPedidos AS dp
    JOIN pedidos AS p ON p.id = dp.id_pedido
    JOIN menus AS m ON m.id = dp.id_menu
    WHERE p.id_cliente = 1;
    ```

## Procedimientos Almacenados

### Crear un procedimiento almacenado para agregar un nuevo cliente

**Enunciado:** Crea un procedimiento almacenado llamado **AgregarCliente** que reciba como
parámetros el nombre, correo electrónico, teléfono y fecha de registro de un nuevo cliente y lo
inserte en la tabla Clientes .

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS AgregarCliente;
CREATE PROCEDURE AgregarCliente(
    IN nombre VARCHAR(100),
    IN correo_electronico VARCHAR(100),
    IN telefono VARCHAR(15),
    IN fecha_registro DATE
)
BEGIN
    INSERT INTO clientes(id, nombre, correo_electronico, telefono, fecha_registro) VALUES
    (NULL, nombre, correo_electronico, telefono, fecha_registro);
END $$
DELIMITER ;
```

### Crear un procedimiento almacenado para obtener los detalles de un pedido
**Enunciado**: Crea un procedimiento almacenado llamado **ObtenerDetallesPedido** que reciba
como parámetro el ID del pedido y devuelva los detalles del pedido, incluyendo el nombre del
menú, cantidad y precio unitario.

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS ObtenerDetallesPedido;
CREATE PROCEDURE ObtenerDetallesPedido(
    IN pedido_id INT
)
BEGIN
    SELECT
        m.nombre AS menu,
        dp.cantidad,
        dp.precio_unitario
    FROM detallesPedidos AS dp
    JOIN menus AS m ON m.id = dp.id_menu
    WHERE dp.id_pedido = pedido_id;
END $$
DELIMITER ;
```

### Crear un procedimiento almacenado para actualizar el precio de un menú
**Enunciado**: Crea un procedimiento almacenado llamado **ActualizarPrecioMenu** que reciba
como parámetros el ID del menú y el nuevo precio, y actualice el precio del menú en la tabla
Menus .

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS ActualizarPrecioMenu;
CREATE PROCEDURE ActualizarPrecioMenu(
    IN menu_id INT,
    IN nuevo_precio DECIMAL(10,2)
)
BEGIN

    UPDATE menus AS m
    SET m.precio = nuevo_precio
    WHERE m.id = menu_id;

END $$
DELIMITER ;
```

### Crear un procedimiento almacenado para eliminar un cliente y sus pedidos
**Enunciado**: Crea un procedimiento almacenado llamado **EliminarCliente** que reciba como
parámetro el ID del cliente y elimine el cliente junto con todos sus pedidos y los detalles de los
pedidos.

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS EliminarCliente;
CREATE PROCEDURE EliminarCliente(
    IN cliente_id INT
)
BEGIN
    START TRANSACTION;
    
    DELETE dp FROM detallesPedidos AS dp
    JOIN pedidos AS p1 ON dp.id_pedido = p1.id
    WHERE p1.id_cliente = cliente_id;
    
    DELETE FROM pedidos AS p
    WHERE p.id_cliente = cliente_id;
    
    DELETE FROM clientes AS c
    WHERE c.id = cliente_id;

END $$
DELIMITER ;
```

### Crear un procedimiento almacenado para obtener el total gastado por un cliente
**Enunciado**: Crea un procedimiento almacenado llamado **TotalGastadoPorCliente** que reciba
como parámetro el ID del cliente y devuelva el total gastado por ese cliente en todos sus pedidos.

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS TotalGastadoPorCliente;
CREATE PROCEDURE TotalGastadoPorCliente(
    IN cliente_id INT
)
BEGIN
    SELECT
        c.nombre,
        SUM(p.total) AS Total_Gastado
    FROM pedidos AS p
    JOIN clientes AS c ON c.id = p.id_cliente
    WHERE p.id_cliente = cliente_id
    GROUP BY c.id;

END $$
DELIMITER ;
```
