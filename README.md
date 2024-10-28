# CampusBike Database 

Este repositorio contiene el esquema de base de datos para gestionar información relacionada con una tienda de bicicletas, incluyendo detalles de clientes, ventas, inventario, proveedores y sucursales.

## Creación de la Base de Datos


```sql
CREATE DATABASE campusbike;
USE campusbike;

CREATE TABLE pais(
id int NOT NULL ,
nombre VARCHAR(50),
CONSTRAINT PK_PAIS_Id PRIMARY KEY (Id)
);

CREATE TABLE region(
id int NOT NULL,
nombre VARCHAR(50),
idpais_fk int (11),
CONSTRAINT PK_REGIONES_Id PRIMARY KEY (Id),
CONSTRAINT FK_RegionesPais_Id FOREIGN KEY (idpais_fk ) REFERENCES pais(id)
);


CREATE TABLE ciudad(
id int NOT NULL,
nombre VARCHAR(50),
idregion_fk int (11),
CONSTRAINT PK_CIUDAD_Id PRIMARY KEY (Id),
CONSTRAINT FK_CiudaRegion_fk_Id FOREIGN KEY (idregion_fk) REFERENCES region (Id)
);

CREATE TABLE clientes(
id int NOT NULL AUTO_INCREMENT,
nombre VARCHAR(50),
direccion VARCHAR(50),
telefono int ,
idciudad_fk int (11),
CONSTRAINT PK_CLIENTES_Id PRIMARY KEY (Id),
CONSTRAINT FK_ClientesCiudad_Id FOREIGN KEY (idciudad_fk ) REFERENCES ciudad (id)
);
CREATE TABLE estado(
id int NOT NULL,
descripcion VARCHAR(200),
CONSTRAINT PK_ESTADO_Id PRIMARY KEY (Id)
);
CREATE TABLE ventas(
id int NOT NULL AUTO_INCREMENT,
fecha_venta DATE,
total_venta DECIMAL(10,2),
idclientes_fk int (11),
CONSTRAINT PK_VENTAS_Id PRIMARY KEY (Id),
CONSTRAINT FK_VentasClientes_Id FOREIGN KEY (idclientes_fk ) REFERENCES clientes (id)
);
CREATE TABLE facturas(
id int NOT NULL AUTO_INCREMENT,
fecha_emision DATE,
total DECIMAL (10,2),
idclientes_fk int (11),
idventas_fk int (11),
CONSTRAINT PK_FACTURA_Id PRIMARY KEY (Id),
CONSTRAINT FK_FacturaClientes_fk_Id FOREIGN KEY (idclientes_fk ) REFERENCES clientes (Id),
CONSTRAINT FK_Facturaventas_fk_Id FOREIGN KEY (idventas_fk) REFERENCES ventas (Id)
);
CREATE TABLE pagos(
id int NOT NULL AUTO_INCREMENT,
fecha_pago DATE,
monto DECIMAL(10,2),
metodo_pago VARCHAR(50),
idfactura_fk int (11),
CONSTRAINT PK_PAGOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_PagosFacturas_fk_Id FOREIGN KEY (idfactura_fk ) REFERENCES facturas (Id)
);
CREATE TABLE pedidos(
id int NOT NULL AUTO_INCREMENT,
fecha_pedido DATE,
idestado_fk int (11),
idclientes_fk int(11),
idfacturas_fk int (11),
CONSTRAINT PK_PEDIDOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_PedidosEstado_Isd FOREIGN KEY (idestado_fk ) REFERENCES estado (id),
CONSTRAINT FK_PedidosFacturas_Id FOREIGN KEY (idfacturas_fk ) REFERENCES facturas(id),
CONSTRAINT FK_PedidosClientes_Id FOREIGN KEY (idclientes_fk ) REFERENCES clientes(id)
);

CREATE TABLE sucursal(
id int NOT NULL ,
nombre VARCHAR(50),
direccion VARCHAR(55),
telefono int ,
idciudad_fk int (11),
idregion_fk int(11),
CONSTRAINT PK_SUCURSAL_Id PRIMARY KEY (Id),
CONSTRAINT FK_SucursalCiudad_Id FOREIGN KEY (idciudad_fk ) REFERENCES ciudad (id),
CONSTRAINT FK_SucursalRegion_id FOREIGN KEY (idregion_fk ) REFERENCES region (id)
);
CREATE TABLE stock(
id int NOT NULL AUTO_INCREMENT,
cantidad int,
idproducto_fk int (11),
idsucursal_fk int(11),
CONSTRAINT PK_STOCK_Id PRIMARY KEY (Id),
CONSTRAINT FK_StockSucursal_Id FOREIGN KEY (idsucursal_fk ) REFERENCES sucursal (id)
);
CREATE TABLE proveedor(
id int NOT NULL,
nombre VARCHAR(50),
direccion VARCHAR(200),
CONSTRAINT PK_PROVEEDOR_Id PRIMARY KEY (id)
);
CREATE TABLE bicicletas(
id int NOT NULL AUTO_INCREMENT,
marca VARCHAR(50),
modelo VARCHAR(50),
precio DECIMAL(10,2),
idstock_fk int (11),
idproveedor_fk int (11),
idsucursal_fk int (11),
CONSTRAINT PK_BICICLETAS_Id PRIMARY KEY (Id),
CONSTRAINT FK_BicicletaStock_Id FOREIGN KEY (idstock_fk ) REFERENCES stock (Id),
 CONSTRAINT FK_BicicletaProveedor_Id FOREIGN KEY (idproveedor_fk ) REFERENCES proveedor(Id),
CONSTRAINT FK_BicicletaSucursal_Id FOREIGN KEY (idsucursal_fk ) REFERENCES sucursal (Id)
);
CREATE TABLE repuestos(
id int NOT NULL AUTO_INCREMENT,
descripcion VARCHAR(255),
precio DECIMAL(10,2),
idstock_fk int (11),
idproveedor_fk int (11),
idsucursal_fk int (11),
CONSTRAINT PK_REPUESTOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_RepuestosStock_Id FOREIGN KEY (idstock_fk ) REFERENCES stock (Id),
 CONSTRAINT FK_RepuestosProveedor_Id FOREIGN KEY (idproveedor_fk ) REFERENCES proveedor(Id),
CONSTRAINT FK_RepuestosSucursal_Id FOREIGN KEY (idsucursal_fk ) REFERENCES sucursal (Id)
);

CREATE TABLE productos(
id int NOT NULL AUTO_INCREMENT,
nombre VARCHAR(50),
tipo_documento VARCHAR(50),
precio DECIMAL (10,2),
idstock_fk int (11),
idbicicleta_fk int(11),
idrepuesto_fk int(11),
CONSTRAINT PK_PRODUCTOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_ProductosStock_Id FOREIGN KEY (idstock_fk ) REFERENCES stock(id),
CONSTRAINT FK_ProductosBicicletas_Id FOREIGN KEY (idbicicleta_fk) REFERENCES bicicletas(id),
CONSTRAINT FK_ProductosRepuesto_Id FOREIGN KEY (idrepuesto_fk) REFERENCES repuestos(id)
);

ALTER TABLE stock
ADD CONSTRAINT FK_StockProducto_Id FOREIGN KEY (idproducto_fk ) REFERENCES productos (id);
ALTER TABLE ventas
ADD idbicicleta_fk int (11),
ADD CONSTRAINT FK_VentasBicicleta_Id FOREIGN KEY (idbicicleta_fk ) REFERENCES bicicletas (id);

```
# Agregar Información 

## Insertar información
```sql


INSERT INTO clientes (nombre, direccion, telefono) VALUES 
('Juan Pérez', 'Calle 123', 123456789),
('Ana López', 'Avenida Siempre Viva', 987654321),
('Carlos García', 'Carrera 45', 456123789);

INSERT INTO estado (id, descripcion) VALUES 
(1, 'Pendiente'),
(2, 'En proceso'),
(3, 'Completado');

INSERT INTO ventas (fecha_venta, total_venta, idclientes_fk) VALUES 
('2024-10-01', 150000.00, 1),
('2024-10-02', 250000.00, 2),
('2024-10-03', 30000.00, 3);


INSERT INTO facturas (fecha_emision, total, idclientes_fk, idventas_fk) VALUES 
('2024-10-01', 150000.00, 1, 1),
('2024-10-02', 250000.00, 2, 2),
('2024-10-03', 30000.00, 3, 3);

INSERT INTO pagos (fecha_pago, monto, metodo_pago, idfactura_fk) VALUES 
('2024-10-05', 150000.00, 'Tarjeta de crédito', 1),
('2024-10-06', 250000.00, 'Efectivo', 2),
('2024-10-07', 30000.00, 'Transferencia bancaria', 3);

INSERT INTO pais (id, nombre) VALUES 
(1, 'Colombia'),
(2, 'México'),
(3, 'España');

INSERT INTO region (id, nombre, idpais_fk) VALUES 
(1, 'Antioquia', 1),
(2, 'Ciudad de México', 2),
(3, 'Madrid', 3);

INSERT INTO ciudad (id, nombre, idregion_fk) VALUES 
(1, 'Medellín', 1),
(2, 'Monterrey', 2),
(3, 'Barcelona', 3);

INSERT INTO sucursal (id, nombre, direccion, telefono, idciudad_fk, idregion_fk) VALUES 
(1, 'Sucursal Medellín', 'Calle 1', 1234567, 1, 1),
(2, 'Sucursal Monterrey', 'Avenida 2', 2345678, 2, 2),
(3, 'Sucursal Barcelona', 'Avenida 3', 3456789, 3, 3);

INSERT INTO productos (nombre, tipo_documento, precio) VALUES 
('Bicicleta Trek', 'Factura', 1000.00),
('Llanta Giant', 'Factura', 75.00),
('Sillín Specialized', 'Factura', 100.00);

INSERT INTO stock (cantidad, idproducto_fk, idsucursal_fk) VALUES 
(10, 1, 1),
(5, 2, 2),
(7, 3, 3);

INSERT INTO proveedor (id, nombre, direccion) VALUES 
(1, 'Proveedor A', 'Calle Proveedor 1'),
(2, 'Proveedor B', 'Avenida Proveedor 2'),
(3, 'Proveedor C', 'Carrera Proveedor 3');
INSERT INTO bicicletas (marca, modelo, precio, idstock_fk, idproveedor_fk, idsucursal_fk) VALUES 
('Trek', 'Marlin 7', 1000.00, 1, 1, 1),
('Giant', 'Talon 3', 750.00, 2, 2, 2),
('Specialized', 'Rockhopper', 850.00, 3, 3, 3);

INSERT INTO repuestos (descripcion, precio, idstock_fk, idproveedor_fk, idsucursal_fk) VALUES 
('Llanta Giant', 75.00, 2, 2, 2),
('Sillín Specialized', 100.00, 3, 3, 3),
('Manillar Trek', 50.00, 1, 1, 1);

INSERT INTO productos (nombre, tipo_documento, precio, idstock_fk, idbicicleta_fk, idrepuesto_fk) VALUES
('Bicicleta Trek', 'Factura', 1000.00, 1, 3, NULL), 
('Llanta Giant', 'Factura', 75.00, 2, NULL, 1),      
('Sillín Specialized', 'Factura', 100.00, 3, NULL, 2); 
```
## Consultas
```sql
SELECT b.marca AS Marca, b.modelo AS Modelo, s.nombre AS Sucursal, st.cantidad AS Stock_Disponible
FROM bicicletas b
JOIN stock st ON b.idstock_fk = st.id
JOIN sucursal s ON st.idsucursal_fk = s.id
ORDER BY s.nombre, b.marca, b.modelo;

SELECT c.nombre AS Cliente, c.direccion AS Direccion, c.telefono AS Telefono, 
       SUM(v.total_venta) AS Total_Ventas
FROM clientes c
JOIN ventas v ON c.id = v.idclientes_fk
GROUP BY c.id
ORDER BY Total_Ventas DESC;

UPDATE ventas SET idbicicleta_fk = 1 WHERE id = 1;
UPDATE ventas SET idbicicleta_fk = 2 WHERE id = 2;
UPDATE ventas SET idbicicleta_fk = 3 WHERE id = 3;

SELECT b.marca AS Marca, b.modelo AS Modelo, COUNT(v.idbicicleta_fk) AS Total_Ventas
FROM bicicletas b
JOIN ventas v ON b.id = v.idbicicleta_fk
GROUP BY b.id
ORDER BY Total_Ventas DESC
LIMIT 1;

UPDATE clientes SET idciudad_fk = 1 WHERE id = 1;  -- Juan Pérez -> Medellín
UPDATE clientes SET idciudad_fk = 2 WHERE id = 2;  -- Ana López -> Monterrey
UPDATE clientes SET idciudad_fk = 3 WHERE id = 3;  -- Carlos García -> Barcelona

SELECT 
    c.nombre AS cliente, 
    ci.nombre AS ciudad, 
    SUM(v.total_venta) AS total_ventas
FROM 
    clientes c
JOIN 
    ciudad ci ON c.idciudad_fk = ci.id  
JOIN 
    ventas v ON c.id = v.idclientes_fk  
GROUP BY 
    c.nombre, ci.nombre  
ORDER BY 
    total_ventas DESC;  

SELECT 
    pr.nombre AS proveedor,
    ci.nombre AS ciudad,
    s.cantidad
FROM 
    proveedor pr
JOIN 
    bicicletas b ON pr.id = b.idproveedor_fk
JOIN 
    stock s ON b.idstock_fk = s.id
JOIN 
    sucursal su ON s.idsucursal_fk = su.id
JOIN 
    ciudad ci ON su.idciudad_fk = ci.id
WHERE 
    s.cantidad > 0;






