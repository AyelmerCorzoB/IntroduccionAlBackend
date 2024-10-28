# Ayelmer Corzo B - Sistema de Gestión de Distribución de Paquetes

## Creación de Tablas

```sql
CREATE TABLE paises (
    pais_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE ciudades (
    ciudad_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    pais_id INT,
    FOREIGN KEY (pais_id) REFERENCES paises(pais_id)
);

CREATE TABLE sucursales (
    sucursal_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(200),
    ciudad_id INT,
    FOREIGN KEY (ciudad_id) REFERENCES ciudades(ciudad_id)
);

CREATE TABLE clientes (
    cliente_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    direccion VARCHAR(200),
    telefono VARCHAR(20)
);

CREATE TABLE conductores (
    conductor_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(20)
);

CREATE TABLE auxiliares (
    auxiliar_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(20)
);

CREATE TABLE vehiculos (
    vehiculo_id INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(20) NOT NULL,
    marca VARCHAR(50),
    modelo VARCHAR(50),
    capacidad_carga DECIMAL(10,2),
    sucursal_id INT,
    FOREIGN KEY (sucursal_id) REFERENCES sucursales(sucursal_id)
);

CREATE TABLE rutas (
    ruta_id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(200),
    sucursal_id INT,
    FOREIGN KEY (sucursal_id) REFERENCES sucursales(sucursal_id)
);

CREATE TABLE ruta_auxiliares (
    ruta_id INT,
    auxiliar_id INT,
    PRIMARY KEY (ruta_id, auxiliar_id),
    FOREIGN KEY (ruta_id) REFERENCES rutas(ruta_id),
    FOREIGN KEY (auxiliar_id) REFERENCES auxiliares(auxiliar_id)
);

CREATE TABLE paquetes (
    paquete_id INT AUTO_INCREMENT PRIMARY KEY,
    numero_seguimiento VARCHAR(50),
    peso DECIMAL(10,2),
    dimensiones VARCHAR(50),
    contenido TEXT,
    valor_declarado DECIMAL(10,2),
    tipo_servicio VARCHAR(50),
    estado VARCHAR(50)
);

CREATE TABLE seguimiento (
    seguimiento_id INT AUTO_INCREMENT PRIMARY KEY,
    paquete_id INT,
    ubicacion VARCHAR(200),
    fecha_hora TIMESTAMP,
    estado VARCHAR(50),
    FOREIGN KEY (paquete_id) REFERENCES paquetes(paquete_id)
);

CREATE TABLE envios (
    envio_id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    paquete_id INT,
    conductor_id INT,
    fecha_envio TIMESTAMP,
    destino VARCHAR(200),
    ruta_id INT,
    sucursal_id INT,
    FOREIGN KEY (cliente_id) REFERENCES clientes(cliente_id),
    FOREIGN KEY (paquete_id) REFERENCES paquetes(paquete_id),
    FOREIGN KEY (conductor_id) REFERENCES conductores(conductor_id),
    FOREIGN KEY (ruta_id) REFERENCES rutas(ruta_id),
    FOREIGN KEY (sucursal_id) REFERENCES sucursales(sucursal_id)
);
```



## Inserción de Datos Iniciales
````sql
INSERT INTO paises (nombre) VALUES 
('México'),
('Estados Unidos'),
('Canadá'),
('España'),
('Colombia');

INSERT INTO ciudades (nombre, pais_id) VALUES 
('Ciudad de México', 1),
('Guadalajara', 1),
('Nueva York', 2),
('Toronto', 3),
('Madrid', 4),
('Bogotá', 5);

INSERT INTO sucursales (nombre, direccion, ciudad_id) VALUES 
('Sucursal Centro CDMX', 'Av. Reforma 123', 1),
('Sucursal Norte GDL', 'Av. Vallarta 456', 2),
('Sucursal Manhattan', '5th Avenue 789', 3),
('Sucursal Downtown TO', 'Queen St. 321', 4),
('Sucursal Central MAD', 'Gran Vía 567', 5);

INSERT INTO clientes (nombre, email, direccion, telefono) VALUES 
('Ayelmer Corzo', 'AyelmerCorzo@email.com', 'Calle 1 #123', '555-0001'),
('Wilmer Gelvez', 'Wilmer Gelvez@email.com', 'Av. Principal 456', '555-0002'),
('Elidallana Cristancho', 'ElidallanaCristancho@email.com', '10th Street 789', '555-0003'),
('Maria Herrera', 'MariaHerrera@email.com', 'Calle Mayor 321', '555-0004');

INSERT INTO conductores (nombre, telefono) VALUES 
('Pedro Martínez', '555-1001'),
('Luis González', '555-1002'),
('Michael Brown', '555-1003'),
('David Wilson ', '555-1004');

INSERT INTO auxiliares (nombre, telefono) VALUES 
('Ana López', '555-2001'),
('Roberto Sánchez', '555-2002'),
('Sarah Johnson', '555-2003'),
('James Davis', '555-2004');

INSERT INTO vehiculos (placa, marca, modelo, capacidad_carga, sucursal_id) VALUES 
('XYZ-123', 'Ford', 'Transit', 2000.00, 1),
('ABC-456', 'Mercedes', 'Sprinter', 2500.00, 2),
('DEF-789', 'Chevrolet', 'Express', 1800.00, 3),
('GHI-012', 'Volkswagen', 'Crafter', 2200.00, 4);

INSERT INTO rutas (descripcion, sucursal_id) VALUES 
('Ruta Centro CDMX', 1),
('Ruta Norte GDL', 2),
('Ruta Manhattan-Brooklyn', 3),
('Ruta Downtown-Suburbs', 4);

INSERT INTO ruta_auxiliares (ruta_id, auxiliar_id) VALUES 
(1, 1),
(1, 2),
(2, 3),
(3, 4);

INSERT INTO paquetes (numero_seguimiento, peso, dimensiones, contenido, valor_declarado, tipo_servicio, estado) VALUES 
('PKT001', 5.5, '30x20x15', 'Documentos', 100.00, 'express', 'en_transito'),
('PKT002', 10.2, '50x40x30', 'Electrónicos', 500.00, 'normal', 'recibido'),
('PKT003', 2.0, '20x15x10', 'Ropa', 150.00, 'express', 'entregado'),
('PKT004', 15.0, '60x45x40', 'Equipamiento', 800.00 , 'normal', 'en_transito');

INSERT INTO seguimiento (paquete_id, ubicacion, fecha_hora, estado) VALUES 
(1, 'Ciudad de México', '2022-01-01 10:00:00', 'en_transito'),
(1, 'Guadalajara', '2022-01-02 12:00:00', 'en_transito'),
(2, 'Nueva York', '2022-01-03 14:00:00', 'recibido'),
(3, 'Madrid', '2022-01-04 16:00:00', 'entregado'),
(4, 'Bogotá', '2022-01-05 18:00:00', 'en_transito');

INSERT INTO envios (cliente_id, paquete_id, conductor_id, fecha_envio, destino, ruta_id, sucursal_id) VALUES 
(1, 1, 1, '2022-01-01 10:00:00', 'Guadalajara', 1, 1),
(2, 2, 2, '2022-01-02 12:00:00', 'Miami', 2, 2),
(3, 3, 3, '2022-01-03 14:00:00', 'Madrid', 3, 3),
(4, 4, 4, '2022-01-04 16:00:00', 'Bogotá', 4, 4);

````
## CONSULTAS FUNCIONES AGREGADAS 
````SQL
SELECT COUNT(*) AS total_clientes FROM clientes;
 +----------------+
| total_clientes |
+----------------+
|              4 |
+----------------+
SELECT SUM(valor_declarado) AS suma_total_valor_declarado FROM paquetes ;
 +----------------------------+
| suma_total_valor_declarado |
+----------------------------+
|                    1550.00 |
+----------------------------+
SELECT AVG(peso) AS peso_promedio FROM paquetes;
+---------------+
| peso_promedio |
+---------------+
|      8.175000 |
+---------------+
SELECT MAX(peso) AS peso_maximo FROM paquetes;
+-------------+
| peso_maximo |
+-------------+
|       15.00 |
+-------------+
SELECT MIN(peso) AS peso_minimo FROM paquetes;
+-------------+
| peso_minimo |
+-------------+
|        2.00 |
+-------------+
SELECT GROUP_CONCAT(nombre SEPARATOR ', ') AS
lista_clientes FROM clientes;
+--------------------------------------------------------------------+
| lista_clientes                                                     |
+--------------------------------------------------------------------+
| Ayelmer Corzo, Wilmer Gelvez, Elidallana Cristancho, Maria Herrera |
+--------------------------------------------------------------------+
````
### Casos de Uso 
#### Caso de Uso 1: Registrar un Nuevo País 
````SQL
 INSERT INTO paises (nombre) VALUES ('México');
 ````

#### Caso de Uso 2: Registrar una Nueva Ciudad 
````SQL
INSERT INTO ciudades (nombre, pais_id) VALUES ('Guadalajara', 1);
````

#### Caso de Uso 3: Registrar una Nueva Sucursal 
````SQL
INSERT INTO sucursales (nombre, direccion, ciudad_id) 
VALUES ('Sucursal Norte GDL', 'Av. Vallarta 456', 2);
````

#### Caso de Uso 4: Registrar un Nuevo Cliente 
````SQL
INSERT INTO clientes (nombre, email, direccion, telefono)
 VALUES ('Ayelmer Corzo', 'AyelmerCorzo@email.com', 'Calle 1 #123', '555-0001');
````

#### Caso de Uso 5: Registrar un Nuevo Teléfono para un Cliente 
````SQL
UPDATE clientes SET telefono = '555-0005' WHERE cliente_id = 1; 
````

#### Caso de Uso 6: Registrar un Nuevo Paquete 
````SQL
INSERT INTO paquetes (
 numero_seguimiento, peso, dimensiones, contenido, valor_declarado, 
tipo_servicio, estado)
 VALUES ('PKT005', 3.5, '25x15x10', 'Libros', 50.00, 'normal', 'en_transito');
````

#### Caso de Uso 7: Registrar un Nuevo Envío 
````SQL
INSERT INTO envios (
 cliente_id, paquete_id, conductor_id, fecha_envio, destino, ruta_id, 
sucursal_id) VALUES (1, 1, 1, '2022-01-01 10:00:00', 'Guadalajara', 1, 1);
````
#### Caso de Uso 8: Registrar un Nuevo Vehículo 
````SQL
INSERT INTO vehiculos (
 placa, marca, modelo, capacidad_carga, sucursal_id) 
VALUES ('JKL-345', 'Toyota', 'Hiace', 3000.00, 1);
````

#### Caso de Uso 9: Registrar un Nuevo Conductor 
````SQL
INSERT INTO conductores (nombre, telefono) 
VALUES ('Juan Pérez', '555-1005');
````

#### Caso de Uso 10: Registrar un Nuevo Teléfono para un Conductor 
````SQL
UPDATE conductores SET telefono = '555-1006' WHERE conductor_id = 1;
````

#### Caso de Uso 11: Asignar un Conductor a una Ruta y un Vehículo 
````SQL
INSERT INTO rutas (descripcion, sucursal_id) VALUES ('Ruta Centro CDMX', 1);####
Caso de Uso 12: Registrar un Nuevo Auxiliar 
INSERT INTO auxiliares (nombre, telefono) 
VALUES ('Laura Martínez', '555-2005');
````

#### Caso de Uso 13: Asignar un Auxiliar a una Ruta 
````SQL
INSERT INTO ruta_auxiliares (ruta_id, auxiliar_id) VALUES (1, 1);
````

#### Caso de Uso 14: Registrar un Evento de Seguimiento para un Paquete
````SQL
INSERT INTO seguimiento (paquete_id, ubicacion, fecha_hora, estado) 
VALUES (1, 'Guadalajara', '2022-01-02 12:00:00', 'en_transito');
```` 

#### Caso de Uso 15: Generar un Reporte de Envíos por Cliente 
````SQL
SELECT * FROM envios WHERE cliente_id = 1;

````

#### Caso de Uso 16: Actualizar el Estado de un Paquete 
````SQL
UPDATE paquetes SET estado = 'entregado' WHERE paquete_id = 1; 
````

#### Caso de Uso 17: Rastrear la Ubicación Actual de un Paquete 
````SQL
SELECT * FROM seguimiento WHERE paquete_id = 1 ORDER BY fecha_hora DESC LIMIT 1; 
````

## CASOS DE USO MULTITABLA 
#### Caso de Uso 1: Obtener Información Completa de Envíos 
````SQL
SELECT e.*, c.nombre AS cliente_nombre, p.numero_seguimiento, r.descripcion AS 
ruta_descripcion, s.nombre AS sucursal_nombre
 FROM envios e
 JOIN clientes c ON e.cliente_id = c.cliente_id
 JOIN paquetes p ON e.paquete_id = p.paquete_id
 JOIN rutas r ON e.ruta_id = r.ruta_id
 JOIN sucursales s ON e.sucursal_id = s.sucursal_id;

````

#### Caso de Uso 2: Obtener Historial de Envíos de un Cliente 
````SQL
SELECT e.*, p.numero_seguimiento, s.nombre AS sucursal_nombre, r.descripcion AS 
ruta_descripcion
 FROM envios e
 JOIN paquetes p ON e.paquete_id = p.paquete_id
 JOIN sucursales s ON e.sucursal_id = s.sucursal_id
 JOIN rutas r ON e.ruta_id = r.ruta_id
 WHERE e.cliente_id = 1;
````
####
#### Caso de Uso 3: Listar Conductores y sus Rutas Asignadas 
````SQL
SELECT c.nombre AS conductor_nombre, r.descripcion AS ruta_descripcion, v.placa 
AS vehiculo_placa
 FROM conductores c
 JOIN rutas r ON c.conductor_id = r.conductor_id
 JOIN vehiculos v ON r.vehiculo_id = v.vehiculo_id;
````

#### Caso de Uso 4: Obtener Detalles de Rutas y Auxiliares Asignados 
````SQL
SELECT r.descripcion AS ruta_descripcion, a.nombre AS auxiliar_nombre
 FROM rutas r
 JOIN ruta_auxiliares ra ON r.ruta_id = ra.ruta_id
 JOIN auxiliares a ON ra.auxiliar_id = a.auxiliar_id;
````

#### Caso de Uso 5: Generar Reporte de Paquetes por Sucursal y Estado 
````SQL
SELECT s.nombre AS sucursal_nombre, p.estado, COUNT(p.paquete_id) AS 
total_paquetes
 FROM paquetes p
 JOIN envios e ON p.paquete_id = e.paquete_id
 JOIN sucursales s ON e.sucursal_id = s.sucursal_id
 GROUP BY s.nombre, p.estado;
````

#### Caso de Uso 6: Obtener Información Completa de un Paquete y su Historial de Seguimiento
````SQL
 SELECT p.*, s.ubicacion, s.fecha_hora, s.estado
 FROM paquetes p
 LEFT JOIN seguimiento s ON p.paquete_id = s.paquete_id
 WHERE p.paquete_id = 1; 
````
### Casos de Uso Between, In y Not In 
#### Caso de Uso 1: Obtener Paquetes Enviados Dentro de un Rango de Fechas 
````SQL
SELECT * FROM envios 
WHERE fecha_envio BETWEEN '2022-01-01' AND '2022-01-31';
````

#### Caso de Uso 2: Obtener Paquetes con Ciertos Estados 
````SQL
SELECT * FROM envios 
WHERE fecha_envio BETWEEN '2022-01-01' AND '2022-01-31';
 SELECT * FROM paquetes 
WHERE estado IN ('en_transito', 'entregado');
````

#### ZCaso de Uso 3: Obtener Paquetes Excluyendo Ciertos Estados 
````SQL
SELECT * FROM paquetes 
WHERE estado NOT IN ('recibido', 'retenido en aduana');
````

#### Caso de Uso 4: Obtener Clientes con Envíos Realizados Dentro de un Rango de Fechas

````SQL
 SELECT DISTINCT c.*
 FROM clientes c
 JOIN envios e ON c.cliente_id = e.cliente_id
 WHERE e.fecha_envio BETWEEN '2022-01-01' AND '2022-01-31';
````

#### Caso de Uso 5: Obtener Conductores Disponibles que No Están Asignados a Ciertas Rutas
````SQL
 SELECT * FROM conductores 
WHERE conductor_id NOT IN (SELECT conductor_id FROM rutas);
````

#### Caso de Uso 6: Obtener Información de Paquetes con Valor Declarado Dentro de un Rango Específico
````SQL
 SELECT * FROM paquetes 
WHERE valor_declarado BETWEEN 50 AND 150;
````

#### Caso de Uso 7: Obtener Auxiliares Asignados a Rutas Específicas 
 ````SQL
 SELECT a.*
 FROM auxiliares a
 JOIN ruta_auxiliares ra ON a.auxiliar_id = ra.auxiliar_id
 WHERE ra.ruta_id IN (1, 2, 3); 
````

#### Caso de Uso 8: Obtener Envíos a Destinos Excluyendo Ciertas Ciudades 
````SQL
SELECT * FROM envios 
WHERE destino NOT IN ('Ciudad de México', 'Guadalajara');
````

#### Caso de Uso 9: Obtener Seguimientos de Paquetes en un Rango de Fechas 
 ````SQL
 SELECT * FROM seguimiento 
WHERE fecha_hora BETWEEN '2022-01-01' AND '2022-01-31';
````

#### Caso de Uso 10: Obtener Clientes que Tienen Ciertos Tipos de Paquetes 
 ````SQL
 SELECT DISTINCT c.*
 FROM clientes c
 JOIN envios e ON c.cliente_id = e.cliente_id
 JOIN paquetes p ON e.paquete_id = p.paquete_id
 WHERE p.tipo_servicio IN ('nacional', 'internacional');
````

 
