# <p align="center"><strong>🧩 ***Unidad 1*** 🧩</strong></p>


##    <p align="center"><strong>🔀 ***Proyecto de la Unidad 1:***</strong></p>
###   🟢 Titulo:

**"Implementación de una base de datos para la gestión de venta, mantenimiento y reparación de computadoras y laptops en un taller de servicio técnico"**

💡 Contexto del Problema: 
El problema se presenta en un taller de servicio técnico que vende computadoras de escritorio, laptops, repuestos y accesorios, y que además recibe los equipos de sus clientes para mantenimiento preventivo o reparación, se registran diariamente ventas, recepciones de equipos, diagnósticos, reparaciones, compras a proveedores y cobros, lo que genera un volumen considerable de información que debe guardarse y consultarse de forma ordenada.
Sin una base de datos, la información suele llevarse en cuadernos, hojas de cálculo o comprobantes sueltos, esto provoca datos duplicados o incompletos, dificultad para conocer el historial de un cliente o de un equipo, pérdida de órdenes de reparación, errores al calcular totales y pagos, desconocimiento del stock real de repuestos y poco registro sobre qué técnico atendió cada trabajo, qué repuestos se utilizaron y qué garantía se ofreció.
El problema afecta a los clientes, que no reciben información clara sobre el estado de su equipo, los costos ni la garantía, a los técnicos y al personal administrativo, que pierden tiempo buscando datos, y al propietario del taller, que no dispone de información confiable para controlar los ingresos, el inventario y las compras a proveedores.
Se necesita gestionar los datos de clientes, técnicos, equipos recibidos, órdenes de reparación: fecha de ingreso, problema informado, estado y total estimado, productos y su stock, servicios técnicos y sus tarifas, proveedores y precios de suministro, pagos en efectivo o por transferencia y garantías. Gestionarlos correctamente es importante para evitar la redundancia y las inconsistencias, mantener la integridad entre ellos, por ejemplo: que toda orden pertenezca a un equipo y a un técnico existentes y obtener consultas confiables que apoyen la toma de decisiones del negocio.

---

### 💡 Objetivos

**Objetivo General**

Diseñar e implementar una base de datos relacional en MySQL, mediante los modelos Entidad-Relación y relacional y el lenguaje SQL, para gestionar de forma ordenada y confiable la información de ventas, órdenes de reparación, inventario, proveedores, pagos y garantías de un taller de venta y reparación de computadoras y laptops.

**Objetivos Específicos**

1.	Elaborar el diagrama Entidad-Relación y el modelo relacional del sistema, identificando las entidades, atributos, relaciones, cardinalidades y claves primarias y foráneas, para evitar la redundancia de datos.
2.	Implementar el esquema físico en MySQL mediante sentencias SQL con datos de prueba coherentes, y formular cinco consultas en álgebra relacional traducidas a SQL para obtener información útil del sistema.

---

### 📋 Modelos Conceptuales de Bases de Datoos

**Modelo Entidad-Relación (E-R)**

El diagrama Entidad-Relación del sistema está formado por nueve entidades: Cliente, Equipo, Orden_Reparación, Técnico, Servicio, Producto, Proveedor, Pago y Garantía. Las relaciones son: un Cliente posee varios Equipos (1:N), un Equipo genera varias Ordenes de reparación (1:N), un Técnico atiende varias Ordenes (1:N), una Orden registra varios Pagos (1:N) y otorga varias Garantías (1:N), una Orden incluye varios Servicios y un Servicio puede estar en varias Ordenes (N:M, con el atributo costo_cobrado), una Orden usa varios Productos como repuestos y un Producto puede usarse en varias Ordenes (N:M, con los atributos cantidad, precio_unitario y subtotal), y un Producto es suministrado por varios Proveedores y viceversa (N:M, con los atributos precio_compra y fecha_suministro). Las relaciones N:M se convierten en las tablas intermedias Detalle_Servicio, Detalle_Producto y Suministro.

<img width="975" height="812" alt="image" src="https://github.com/user-attachments/assets/1d67757c-c224-4bdd-b23d-d71c9276f2b3" />



**Modelo Relacional**

El modelo relacional se obtuvo al transformar cada entidad en una tabla y cada relación N:M en una tabla intermedia, para un total de 12 tablas. En el diagrama las claves primarias (PK) se muestran subrayadas y las claves foráneas (FK) se representan con flechas hacia la tabla y la clave que referencian. Los tipos de datos sugeridos son INT para identificadores y cantidades, VARCHAR para textos, DATE y DATETIME para fechas, y DECIMAL para valores monetarios.

<img width="975" height="861" alt="image" src="https://github.com/user-attachments/assets/a067605d-ea75-4747-b519-c06411d4bdc9" />

---

###   📋 Codigo SQL:

**Creación de Tablas (DDL)**

```SQL
CREATE TABLE Cliente (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    cedula VARCHAR(10) NOT NULL UNIQUE,
    nombres VARCHAR(100) NOT NULL,
    telefono VARCHAR(15),
    email VARCHAR(100)
);

CREATE TABLE Tecnico (
    id_tecnico INT AUTO_INCREMENT PRIMARY KEY,
    cedula VARCHAR(10) NOT NULL UNIQUE,
    nombres VARCHAR(100) NOT NULL,
    especialidad VARCHAR(50) NOT NULL
);

CREATE TABLE Producto (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    codigo VARCHAR(20) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    precio DECIMAL(8,2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    CONSTRAINT ck_producto_stock CHECK (stock >= 0)
);

CREATE TABLE Servicio (
    id_servicio INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(100) NOT NULL,
    tarifa_base DECIMAL(8,2) NOT NULL
);

CREATE TABLE Proveedor (
    id_proveedor INT AUTO_INCREMENT PRIMARY KEY,
    ruc VARCHAR(13) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(15),
    email VARCHAR(100)
);

CREATE TABLE Equipo (
    id_equipo INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT NOT NULL,
    tipo_equipo VARCHAR(50) NOT NULL,
    marca_modelo VARCHAR(100) NOT NULL,
    numero_serie VARCHAR(50) UNIQUE,
    observaciones TEXT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente) ON DELETE CASCADE
);

CREATE TABLE Orden_Reparacion (
    id_orden INT AUTO_INCREMENT PRIMARY KEY,
    id_equipo INT NOT NULL,
    id_tecnico INT NOT NULL,
    fecha_ingreso DATETIME NOT NULL,
    problema_informado TEXT NOT NULL,
    estado VARCHAR(30) NOT NULL DEFAULT 'En revisión',
    total_estimado DECIMAL(10,2) DEFAULT 0,
    FOREIGN KEY (id_equipo) REFERENCES Equipo(id_equipo) ON DELETE CASCADE,
    FOREIGN KEY (id_tecnico) REFERENCES Tecnico(id_tecnico),
    CONSTRAINT ck_estado_orden CHECK (estado IN ('En revisión','Aprobado','En reparación','Listo','Entregado'))
);

CREATE TABLE Pago (
    id_pago INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT NOT NULL,
    fecha_pago DATETIME NOT NULL,
    monto DECIMAL(10,2) NOT NULL,
    forma_pago VARCHAR(20) NOT NULL,
    FOREIGN KEY (id_orden) REFERENCES Orden_Reparacion(id_orden) ON DELETE CASCADE,
    CONSTRAINT ck_pago_monto CHECK (monto > 0),
    CONSTRAINT ck_pago_forma CHECK (forma_pago IN ('Efectivo','Transferencia'))
);

CREATE TABLE Garantia (
    id_garantia INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT NOT NULL,
    descripcion VARCHAR(150) NOT NULL,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL,
    FOREIGN KEY (id_orden) REFERENCES Orden_Reparacion(id_orden) ON DELETE CASCADE,
    CONSTRAINT ck_garantia_fechas CHECK (fecha_fin >= fecha_inicio)
);

CREATE TABLE Detalle_Producto (
    id_orden INT NOT NULL,
    id_producto INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(8,2) NOT NULL,
    subtotal DECIMAL(8,2) NOT NULL,
    PRIMARY KEY (id_orden, id_producto),
    FOREIGN KEY (id_orden) REFERENCES Orden_Reparacion(id_orden) ON DELETE CASCADE,
    FOREIGN KEY (id_producto) REFERENCES Producto(id_producto),
    CONSTRAINT ck_detalle_cant CHECK (cantidad > 0)
);

CREATE TABLE Detalle_Servicio (
    id_orden INT NOT NULL,
    id_servicio INT NOT NULL,
    costo_cobrado DECIMAL(8,2) NOT NULL,
    PRIMARY KEY (id_orden, id_servicio),
    FOREIGN KEY (id_orden) REFERENCES Orden_Reparacion(id_orden) ON DELETE CASCADE,
    FOREIGN KEY (id_servicio) REFERENCES Servicio(id_servicio)
);

CREATE TABLE Suministro (
    id_proveedor INT NOT NULL,
    id_producto INT NOT NULL,
    precio_compra DECIMAL(8,2) NOT NULL,
    fecha_suministro DATE NOT NULL,
    PRIMARY KEY (id_proveedor, id_producto),
    FOREIGN KEY (id_proveedor) REFERENCES Proveedor(id_proveedor),
    FOREIGN KEY (id_producto) REFERENCES Producto(id_producto),
    CONSTRAINT ck_suministro_precio CHECK (precio_compra >= 0)
);

```

**Inserción de Datos (DML)**

```SQL

-- Datos de Cliente
INSERT INTO Cliente (id_cliente, cedula, nombres, telefono, email)
VALUES
    (1, '1100000001', 'Maria Fernanda Lopez', '0991111111', 'maria.lopez@mail.com'),
    (2, '1100000002', 'Carlos Andres Perez', '0992222222', 'carlos.perez@mail.com'),
    (3, '1100000003', 'Ana Lucia Torres', '0993333333', 'ana.torres@mail.com'),
    (4, '1100000004', 'Jorge Luis Armijos', '0994444444', 'jorge.armijos@mail.com'),
    (5, '1100000005', 'Sofia Valeria Castillo', '0995555555', 'sofia.castillo@mail.com'),
    (6, '1100000006', 'Pedro Pablo Jaramillo', '0996666666', 'pedro.jaramillo@mail.com');

```
<img width="938" height="255" alt="image" src="https://github.com/user-attachments/assets/85843ec3-5988-4829-9e7e-fcda3fa1011a" />


```SQL

-- Datos de Tecnico
INSERT INTO Tecnico (id_tecnico, cedula, nombres, especialidad)
VALUES
    (1, '1100000101', 'Luis Miguel Ochoa', 'Hardware'),
    (2, '1100000102', 'Diego Armando Cueva', 'Software'),
    (3, '1100000103', 'Paola Estefania Rojas', 'Hardware y software'),
    (4, '1100000104', 'Karla Nicole Vega', 'Electronica'),
    (5, '1100000105', 'Andres Felipe Mora', 'Redes y mantenimiento');

```
<img width="937" height="294" alt="image" src="https://github.com/user-attachments/assets/69b45a1a-9307-4b14-96ee-3da2b144f186" />


```SQL

-- Datos de Equipo
INSERT INTO Equipo (id_equipo, id_cliente, tipo_equipo, marca_modelo, numero_serie, observaciones)
VALUES
    (1, 1, 'Laptop', 'HP Pavilion 15', '5CD1234ABC', 'Teclado con teclas desgastadas'),
    (2, 2, 'PC de escritorio', 'Lenovo ThinkCentre M720', 'PC0X9876', 'Sin observaciones'),
    (3, 3, 'Laptop', 'Dell Inspiron 14', 'DL7788XYZ', 'Rayones en la tapa'),
    (4, 1, 'Laptop', 'Asus VivoBook', 'AS5566QWE', 'Bisagra floja'),
    (5, 4, 'PC de escritorio', 'Ensamblada', NULL, 'Gabinete con polvo'),
    (6, 5, 'Laptop', 'Acer Aspire 5', 'AC1122TTT', 'Pantalla con lineas'),
    (7, 6, 'Laptop', 'Lenovo IdeaPad 3', 'LN3344RRR', 'Se apaga con bateria');

```
<img width="941" height="220" alt="image" src="https://github.com/user-attachments/assets/9f69a0f3-9119-4e44-96cf-a0da51291d11" />


```SQL

-- Datos de Orden_Reparacion
INSERT INTO Orden_Reparacion (id_orden, id_equipo, id_tecnico, fecha_ingreso, problema_informado, estado, total_estimado)
VALUES
    (1, 1, 1, '2026-10-05 09:30:00', 'Laptop lenta y se calienta', 'Entregado', 59.0),
    (2, 2, 2, '2026-10-12 11:00:00', 'No inicia el sistema operativo', 'Entregado', 74.0),
    (3, 3, 1, '2026-10-20 15:45:00', 'La pantalla no enciende', 'En reparación', 103.0),
    (4, 4, 3, '2026-10-28 10:15:00', 'No carga la bateria', 'Aprobado', 26.75),
    (5, 5, 2, '2026-11-03 16:20:00', 'Se apaga solo y hace ruido', 'En revisión', 0.0),
    (6, 6, 4, '2026-11-04 14:10:00', 'Pantalla con lineas de colores', 'Listo', 103.0),
    (7, 2, 3, '2026-11-05 09:00:00', 'Equipo lento, requiere mas memoria', 'En revisión', 65.0);

```
<img width="944" height="193" alt="image" src="https://github.com/user-attachments/assets/3212c093-81c0-4564-bd11-4f173837b14d" />


---

###   🔎 Consulta de Algebra Relacional y Traducir a SQL

**Consulta 1 — Selección (operador: Selección σ)**

#### Descripción:

Se desea obtener las órdenes de reparación que están en proceso (estado En reparación, Aprobado o Listo) y cuyo total estimado supera los $50, para dar prioridad a los trabajos de mayor valor.

#### Álgebra Relacional:

```SQL

σ (estado = 'En reparación' ∨ estado = 'Aprobado' ∨ estado = 'Listo') ∧ total_estimado > 50 (Orden_Reparacion)

```

#### SQL:

```SQL

SELECT *
FROM   Orden_Reparacion
WHERE  (estado = 'En reparación' OR estado = 'Aprobado' OR estado = 'Listo')
  AND  total_estimado > 50;

```

#### Resultado esperado:

Se obtienen 2 órdenes: la orden 3 (En reparación, $103.00) y la orden 6 (Listo, $103.00). La orden 4 (Aprobado) no aparece porque su total estimado es de $26.75.





**Consulta 2 — Proyección y Unión (operadores: Proyección π y Unión ∪)**

#### Descripción:

Se desea obtener una lista única de los correos electrónicos de las personas con quienes se comunica el taller (clientes y proveedores), por ejemplo: para enviar un comunicado general. Primero se proyecta la columna email de cada tabla y luego se unen los resultados eliminando los duplicados.

#### Álgebra Relacional:

```SQL

π email (Cliente) ∪ π email (Proveedor)

```

#### SQL:

```SQL

SELECT email FROM Cliente
UNION
SELECT email FROM Proveedor;


```

#### Resultado esperado:

Se muestra una sola columna (email) con 11 correos distintos: los 6 de los clientes y los 5 de los proveedores.





**Consulta 3 — Selección, Proyección y Diferencia (σ + π + −)**

#### Descripción:

Se desea conocer el nombre y el teléfono de los clientes que tienen registrada al menos una laptop, pero cuyos equipos nunca han ingresado a una orden de reparación. Se seleccionan los clientes con equipos de tipo Laptop y a ese conjunto se le resta el de los clientes cuyos equipos aparecen en alguna orden.

#### Álgebra Relacional:

```SQL

π nombres, telefono (Cliente ⋈ (π id_cliente (σ tipo_equipo = 'Laptop' (Equipo)) − π id_cliente (Equipo ⋈ Orden_Reparacion)))

```

#### SQL:

```SQL

SELECT c.nombres, c.telefono
FROM   Cliente c
WHERE  c.id_cliente IN (SELECT id_cliente
                       FROM   Equipo
                       WHERE  tipo_equipo = 'Laptop')
  AND  c.id_cliente NOT IN (SELECT e.id_cliente
                            FROM   Equipo e
                            JOIN   Orden_Reparacion o ON e.id_equipo = o.id_equipo);

```

#### Resultado esperado:

Se obtiene un solo registro: Pedro Pablo Jaramillo (0996666666), cuya laptop Lenovo IdeaPad 3 todavía no ha ingresado a una orden de reparación.





**Consulta 4 — Reunión natural / JOIN (operador: ⋈)**

#### Descripción:

Se desea listar las órdenes de reparación que aún no han sido entregadas, mostrando el cliente, el equipo, el técnico responsable, el estado y el total estimado. Para ello se combinan cuatro tablas mediante sus claves foráneas.

#### Álgebra Relacional:

```SQL

π c.nombres, e.marca_modelo, t.nombres, o.estado, o.total_estimado
   (σ o.estado ≠ 'Entregado' (Cliente c ⋈ c.id_cliente = e.id_cliente Equipo e
      ⋈ e.id_equipo = o.id_equipo Orden_Reparacion o
      ⋈ o.id_tecnico = t.id_tecnico Tecnico t))

```

#### SQL:

```SQL

SELECT c.nombres AS cliente, e.marca_modelo AS equipo,
       t.nombres AS tecnico, o.estado, o.total_estimado
FROM   Cliente c
JOIN   Equipo e ON c.id_cliente = e.id_cliente
JOIN   Orden_Reparacion o ON e.id_equipo = o.id_equipo
JOIN   Tecnico t ON o.id_tecnico = t.id_tecnico
WHERE  o.estado <> 'Entregado'
ORDER BY o.id_orden;

```

#### Resultado esperado:

Se obtienen 5 filas (las órdenes 3 a 7), por ejemplo: Ana Lucia Torres, Dell Inspiron 14, Luis Miguel Ochoa, En reparación, $103.00. Las órdenes 1 y 2 no aparecen porque ya fueron entregadas.






**Consulta 5 — Consulta con agrupamiento y función de agregación (operador: γ)**

#### Descripción:

Se desea calcular, por cada técnico, cuántas órdenes ha atendido y cuánto ha facturado por los servicios realizados (costo_cobrado), mostrando solo a los técnicos con ingresos de al menos $25.

#### Álgebra Relacional:

```SQL

σ ingresos ≥ 25 (γ t.id_tecnico, t.nombres, COUNT(DISTINCT o.id_orden) → ordenes, SUM(ds.costo_cobrado) → ingresos
   (Tecnico t ⋈ t.id_tecnico = o.id_tecnico Orden_Reparacion o ⋈ o.id_orden = ds.id_orden Detalle_Servicio ds))

```

#### SQL:

```SQL

SELECT   t.nombres, COUNT(DISTINCT o.id_orden) AS ordenes,
         SUM(ds.costo_cobrado) AS ingresos
FROM     Tecnico t
JOIN     Orden_Reparacion o ON t.id_tecnico = o.id_tecnico
JOIN     Detalle_Servicio ds ON o.id_orden = ds.id_orden
GROUP BY t.id_tecnico, t.nombres
HAVING   SUM(ds.costo_cobrado) >= 25
ORDER BY ingresos DESC;

```

#### Resultado esperado:

Se obtienen 3 técnicos: Luis Miguel Ochoa (2 órdenes, $50.00), Diego Armando Cueva (1 orden, $32.00) y Karla Nicole Vega (1 orden, $25.00). Paola Estefania Rojas queda excluida porque sus ingresos suman $16.00, y Andres Felipe Mora no aparece porque no tiene órdenes asignadas.

---

####  [Link de Drive](https://drive.google.com/drive/folders/1hzvQwYMZTDoqHU94_xC02pPlOfYM9slm?usp=sharing)

---


##   📝 ***Conclusiones y Bibliografia***
###   📌 Conclusiones:
🔹 Conclusión 1: La elaboración del diagrama Entidad-Relación y del modelo relacional permitió comprender cómo un problema real se traduce en entidades, atributos y relaciones. La mayor dificultad fue decidir qué conceptos debían ser entidades independientes (como Pago y Garantia) y cuáles eran relaciones N:M que requerían una tabla intermedia (Detalle_Producto, Detalle_Servicio y Suministro), además de definir correctamente las cardinalidades y evitar la redundancia.

🔹 Conclusión 2: La escritura del código SQL (DDL y DML) ayudó a entender la estructura de una base de datos relacional y la importancia de la integridad referencial. Las restricciones PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL y CHECK garantizan que los datos sean coherentes, por ejemplo: que no exista una orden sin equipo ni técnico, ni un stock negativo. También resultó relevante respetar el orden de creación e inserción de las tablas según sus dependencias.

🔹 Conclusión 3: El álgebra relacional permitió plantear cada consulta de forma lógica, decidiendo qué operadores aplicar y en qué orden antes de escribir el SQL, lo que facilitó su traducción a SELECT, WHERE, JOIN, UNION, subconsultas y GROUP BY. Las consultas que combinan varias tablas (JOIN) y las de agrupamiento fueron las más útiles para obtener información del taller, como el estado de las órdenes pendientes y los ingresos por técnico.



###   🔗 Bibliografia:

🔹 [1] C. J. Sánchez Aldana y F. I. Mosquera Motta, Modelamiento de bases de datos. Bogotá, Colombia: Universidad Piloto de Colombia, 2020. ISBN: 978-958-5106-22-2. [En línea]. Disponible en: https://www.digitaliapublishing.com/a/111424/modelamiento-de-base-de-datos.

🔹 [2] R. McFadyen, Relational Databases and Microsoft Access 365, Version 4.0. Winnipeg, MB, Canadá: University of Winnipeg, 2021. [En línea]. Disponible en: https://open.umn.edu/opentextbooks/textbooks/relational-databases-and-microsoft-access.

🔹 [3] V. T. N. Chau y S. Chittayasothorn, “A bitemporal SQL database design method from the enhanced entity-relationship model,” en Proc. 2021 7th Int. Conf. Eng., Appl. Sci. Technol. (ICEAST), Pattaya, Tailandia, abr. 2021,  https://doi.org/10.1109/ICEAST52143.2021.9426270. 

🔹 [4] S. Kryshtanovych, O. Ivanytska, M. Markova, Y. Hliudzyk y A. Ivanova, “A graphical language-based approach for database modeling in higher education information systems,” Ingénierie des Systèmes d’Information, vol. 28, n.º 6, pp. 1597–1603, dic. 2023, https://doi.org/10.18280/isi.280616. 

🔹 [5] T. A. Kustitskaya, R. V. Esin, A. A. Kytmanov y T. V. Zykova, “Designing an education database in a higher education institution for the data-driven management of the educational process,” Educ. Sci., vol. 13, n.º 9, Art. n.º 947, sep. 2023, https://doi.org/10.3390/educsci13090947. 

🔹 [6] Oracle Corporation, “MySQL 9.7 Reference Manual,” 2026. [En línea]. Disponible en: https://dev.mysql.com/doc/refman/9.7/en/preface.html. Accedido: 8 de nov. de 2026.

🔹 [7] J. Kessler, “RelaX – relational algebra calculator,” University of Innsbruck, Innsbruck, Austria, 2025. [En línea]. Disponible en: https://dbis-uibk.github.io/relax/calc/local/uibk/local/0. Accedido: 8 de nov. de 2026.

---

###    <p align="center"><strong>[***Volver atrás***](Portafolio.md)</strong></p>




