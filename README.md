# Juan Diego Contreras Melendez (Gourmet Delight)

## Consultas Requeridas

1. Obtener la lista de todos los menús con sus precios
2. Encontrar todos los pedidos realizados por el cliente 'Juan Perez'
3. Listar los detalles de todos los pedidos, incluyendo el nombre del menú, cantidad y precio unitario
4. Calcular el total gastado por cada cliente en todos sus pedidos
5. Encontrar los menús con un precio mayor a $10
6. Obtener el menú más caro pedido al menos una vez
7. Listar los clientes que han realizado más de un pedido
8. Obtener el cliente con el mayor gasto total
9. Mostrar el pedido más reciente de cada cliente
10. Obtener el detalle de pedidos (menús y cantidades) para el cliente 'Juan Perez'.

## Procedimientos Almacenados

### Crear un procedimiento almacenado para agregar un nuevo cliente

Enunciado: Crea un procedimiento almacenado llamado AgregarCliente que reciba como
parámetros el nombre, correo electrónico, teléfono y fecha de registro de un nuevo cliente y lo
inserte en la tabla Clientes .

### Crear un procedimiento almacenado para obtener los
detalles de un pedido

Enunciado: Crea un procedimiento almacenado llamado ObtenerDetallesPedido que reciba
como parámetro el ID del pedido y devuelva los detalles del pedido, incluyendo el nombre del
menú, cantidad y precio unitario.

### Crear un procedimiento almacenado para actualizar el
precio de un menú

Enunciado: Crea un procedimiento almacenado llamado ActualizarPrecioMenu que reciba
como parámetros el ID del menú y el nuevo precio, y actualice el precio del menú en la tabla
Menus .

### Crear un procedimiento almacenado para eliminar un cliente
y sus pedidos

Enunciado: Crea un procedimiento almacenado llamado EliminarCliente que reciba como
parámetro el ID del cliente y elimine el cliente junto con todos sus pedidos y los detalles de los
pedidos.

### Crear un procedimiento almacenado para obtener el total
gastado por un cliente

Enunciado: Crea un procedimiento almacenado llamado TotalGastadoPorCliente que reciba
como parámetro el ID del cliente y devuelva el total gastado por ese cliente en todos sus pedidos.