# <p align="center"><strong>🧩 ***Unidad 1*** 🧩</strong></p>


##    <p align="center"><strong>🔀 ***Aprendizaje autónomo:***</strong></p>
###   🟢 Ejemplo 1: Sistema de Biblioteca:
💡 Descripción: 

Una biblioteca registra libros y autores. Un libro puede tener varios autores y un autor varios libros. Los socios toman libros prestados; de cada préstamo interesan la fecha de retiro y la de devolución.

####   🖥️ Diagrama Entidad-Relación:
<img width="865" height="630" alt="image" src="https://github.com/user-attachments/assets/a21380ef-0c6b-47b0-971c-69a1869eb7dd" />

####   🗺️ Modelo Relacional:
<img width="693" height="903" alt="image" src="https://github.com/user-attachments/assets/47e46617-6be2-4da9-8ef1-a4ecf6f29cc1" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Autor (
    id_autor VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Libro (
    id_libro VARCHAR(20) PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL
);

CREATE TABLE Escritor (
    id_autor VARCHAR(15) NOT NULL,
    id_libro VARCHAR(20) NOT NULL,
    PRIMARY KEY (id_autor, id_libro),
    FOREIGN KEY (id_autor) REFERENCES Autor(id_autor),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro)
);

CREATE TABLE Socio (
    id_socio VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Prestamo (
    id_libro VARCHAR(20) NOT NULL,
    id_socio VARCHAR(15) NOT NULL,
    fecha_retiro DATE NOT NULL,
    fecha_devolucion DATE, 
    PRIMARY KEY (id_libro, id_socio, fecha_retiro),
    FOREIGN KEY (id_libro) REFERENCES Libro(id_libro),
    FOREIGN KEY (id_socio) REFERENCES Socio(id_socio)
);
```

####   🖥️ Base de Datos:
<img width="272" height="207" alt="image" src="https://github.com/user-attachments/assets/ada24bdb-197e-419a-9afb-a419e62b6030" />



####  [Link de Drive](https://drive.google.com/drive/folders/1hQhupWU6kbmxWzCAd7a4b5TH6CNw8TLQ?usp=sharing)

---
###   🔵 Ejemplo 2: Pedidos de Restaurante:
💡 Descripción: 

Los clientes realizan pedidos que contienen varios platos con distinta cantidad. Los platos se agrupan en categorías. Cada pedido lo atiende un único mesero. El precio del plato debe registrarse tal como estaba al momento del pedido, aunque luego cambie.

####   🖥️ Diagrama Entidad-Relación:
<img width="1208" height="644" alt="image" src="https://github.com/user-attachments/assets/cb724d02-852d-4495-ae12-c5b48c23e88b" />

####   🗺️ Modelo Relacional:
<img width="692" height="923" alt="image" src="https://github.com/user-attachments/assets/bf0898f5-ee0b-41bd-ad54-67ed7009aada" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Cliente (
    id_cliente VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Mesero (
    id_mesero VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Pedido (
    id_pedido VARCHAR(20) PRIMARY KEY,
    id_cliente VARCHAR(15) NOT NULL,
    id_mesero VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente),
    FOREIGN KEY (id_mesero) REFERENCES Mesero(id_mesero)
);

CREATE TABLE Categoria (
    id_categoria VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);

CREATE TABLE Plato (
    id_plato VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    precio DECIMAL(6,2) NOT NULL,
    id_categoria VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_categoria) REFERENCES Categoria(id_categoria)
);

CREATE TABLE Detalle_pedido (
    id_pedido VARCHAR(20) NOT NULL,
    id_plato VARCHAR(15) NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(6,2) NOT NULL,
    PRIMARY KEY (id_pedido, id_plato),
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_plato) REFERENCES Plato(id_plato)
);
```

####   🖥️ Base de Datos:
<img width="274" height="232" alt="image" src="https://github.com/user-attachments/assets/f6a1463c-1a0d-48ec-b9a8-cdf0e89181c6" />



####  [Link de Drive](https://drive.google.com/drive/folders/15EojpwPuwIkJmkd_PfBqXsZQg5T4C80O?usp=sharing)


---
###   🟡 Ejemplo 3: Clínica Médica:
💡 Descripción: 

Los pacientes solicitan citas; cada cita la atiende un médico a un paciente en fecha, hora y consultorio. De algunas citas se genera una receta que incluye medicamentos con dosis y duración. Cada médico pertenece a una especialidad.

####   🖥️ Diagrama Entidad-Relación:
<img width="1202" height="669" alt="image" src="https://github.com/user-attachments/assets/56105f83-c648-4295-a6d8-09613bcad5db" />

####   🗺️ Modelo Relacional:
<img width="970" height="1318" alt="image" src="https://github.com/user-attachments/assets/85aaf4b5-f6dc-4844-acdb-443dcad0fd83" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Especialidad (
    id_especialidad VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Medico (
    id_medico VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    id_especialidad VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_especialidad) REFERENCES Especialidad(id_especialidad)
);

CREATE TABLE Paciente (
    id_paciente VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Cita (
    id_cita VARCHAR(20) PRIMARY KEY,
    fecha DATE NOT NULL,
    hora TIME NOT NULL,
    consultorio VARCHAR(20) NOT NULL,
    id_paciente VARCHAR(15) NOT NULL,
    id_medico VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES Paciente(id_paciente),
    FOREIGN KEY (id_medico) REFERENCES Medico(id_medico)
);

CREATE TABLE Receta (
    id_receta VARCHAR(20) PRIMARY KEY,
    dosis VARCHAR(100) NOT NULL,
    duracion VARCHAR(100) NOT NULL,
    id_cita VARCHAR(20) NOT NULL UNIQUE,
    FOREIGN KEY (id_cita) REFERENCES Cita(id_cita)
);

CREATE TABLE Medicamento (
    id_medicamento VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Incluye (
    id_receta VARCHAR(20) NOT NULL,
    id_medicamento VARCHAR(15) NOT NULL,
    PRIMARY KEY (id_receta, id_medicamento),
    FOREIGN KEY (id_receta) REFERENCES Receta(id_receta),
    FOREIGN KEY (id_medicamento) REFERENCES Medicamento(id_medicamento)
);
```

####   🖥️ Base de Datos:
<img width="248" height="255" alt="image" src="https://github.com/user-attachments/assets/50a6e821-f20e-4fe7-acc4-59beaa8c76fb" />



####  [Link de Drive](https://drive.google.com/drive/folders/17ilOvlSg-OAD6ACTYzot7X1PMIALhDFV?usp=sharing)


---
###   🟢 Ejemplo 4: Sistema de Aerolínea:
💡 Descripción: 

Una aerolínea opera vuelos (cada uno con número, fecha, origen y destino), cada vuelo lo realiza un avión. Los pasajeros realizan reservas para un vuelo específico, indicando asiento y tarifa pagada en el momento de la reserva.

####   🖥️ Diagrama Entidad-Relación:
<img width="1409" height="560" alt="image" src="https://github.com/user-attachments/assets/ee260e5f-38b7-498f-ad94-24c8d644c5e6" />

####   🗺️ Modelo Relacional:
<img width="1006" height="1007" alt="image" src="https://github.com/user-attachments/assets/3641763b-fb5b-48fd-bfe0-718d9d781d7d" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Avion (
    id_avion VARCHAR(15) PRIMARY KEY
);

CREATE TABLE Vuelo (
    numero VARCHAR(15) PRIMARY KEY,
    fecha DATE NOT NULL,
    origen VARCHAR(50) NOT NULL,
    destino VARCHAR(50) NOT NULL,
    id_avion VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_avion) REFERENCES Avion(id_avion)
);

CREATE TABLE Pasajero (
    id_pasajero VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Reserva (
    id_pasajero VARCHAR(20) NOT NULL,
    numero VARCHAR(15) NOT NULL,
    asiento VARCHAR(10) NOT NULL,
    tarifa DECIMAL(8,2) NOT NULL,
    PRIMARY KEY (id_pasajero, numero),
    FOREIGN KEY (id_pasajero) REFERENCES Pasajero(id_pasajero),
    FOREIGN KEY (numero) REFERENCES Vuelo(numero)
);
```

####   🖥️ Base de Datos:
<img width="264" height="197" alt="image" src="https://github.com/user-attachments/assets/ae7fe45c-1fda-41ae-914c-01c2129ef983" />



####  [Link de Drive](https://drive.google.com/drive/folders/1MsvnrqFU1QINvmm7HHQGngf_oMVNanH-?usp=sharing)


---
###   🔵 Ejercicio 5: Empresa Discográfica:
💡 Descripción: 

Una empresa discográfica necesita modelar los datos sobre sus diferentes recursos según las siguientes características:

🔹 Manager: de cada mánager se almacena un identificador de manager y su nombre. Un manager representa a una serie de artistas.

🔹 Artista: un artista es representado por un manager. De los artistas se almacena su nombre completo (usando un único atributo) y su NIF.

🔹 Evento de promoción: los artistas participan en eventos de promoción para dar a conocer sus trabajos. En un evento de promoción pueden participar varios artistas. De un evento se almacena un identificador único, la fecha de celebración y el número de asistentes.


####   🖥️ Diagrama Entidad-Relación:
<img width="1349" height="352" alt="image" src="https://github.com/user-attachments/assets/5bd405b3-2cf6-400b-9322-0c976f3440a1" />

####   🗺️ Modelo Relacional:
<img width="1079" height="837" alt="image" src="https://github.com/user-attachments/assets/e33acbd3-f82e-4982-a6f6-359aa8a313a4" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Manager (
    id_manager VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Artista (
    NIF VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(150) NOT NULL,
    id_manager VARCHAR(20) NOT NULL,
    FOREIGN KEY (id_manager) REFERENCES Manager(id_manager)
);

CREATE TABLE Evento_promocion (
    id_evento VARCHAR(20) PRIMARY KEY,
    fecha_celebracion DATE NOT NULL,
    numero_asistentes INT NOT NULL
);

CREATE TABLE Participa (
    NIF VARCHAR(20) NOT NULL,
    id_evento VARCHAR(20) NOT NULL,
    PRIMARY KEY (NIF, id_evento),
    FOREIGN KEY (NIF) REFERENCES Artista(NIF),
    FOREIGN KEY (id_evento) REFERENCES Evento_promocion(id_evento)
);
```

####   🖥️ Base de Datos:
<img width="295" height="195" alt="image" src="https://github.com/user-attachments/assets/6a9a2cd5-8eec-4d52-9fa8-f2c951fd2a41" />



####  [Link de Drive](https://drive.google.com/drive/folders/1gvZnFkp6V38QNySb1mzMLyA6Oi3OHEk6?usp=sharing)


---
###   🟡 Ejercicio 6: Tienda Informática:
💡 Descripción: 

Se desea crear una aplicación para la gestión de una tienda informática. La tienda dispone de una serie de productos que se venden a los clientes. De cada producto informático se desea guardar el código, descripción, precio y número de existencias. De cada cliente se desea guardar el código, nombre, apellidos, dirección y número telefónico. Un cliente puede comprar varios productos en la tienda y un mismo producto puede ser comprado por varios clientes. Cada vez que se compre un artículo quedará registrada la compra en la base de datos junto con la fecha en la que se compró el artículo. La tienda tiene contactos con varios proveedores que son los que suministran los productos. Un mismo producto puede ser suministrado por varios proveedores. De cada proveedor se desea guardar el código, nombres, apellidos, dirección, provincia y número de teléfono.

####   🖥️ Diagrama Entidad-Relación:
<img width="1859" height="366" alt="image" src="https://github.com/user-attachments/assets/f0ef1899-d412-4928-9bcd-ca65735a144f" />

####   🗺️ Modelo Relacional:
<img width="1001" height="1083" alt="image" src="https://github.com/user-attachments/assets/d0d7ed61-10f1-42b8-ac06-68a2612fbe52" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Cliente (
    codigo VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellidos VARCHAR(50) NOT NULL,
    direccion VARCHAR(200),
    telefono VARCHAR(15)
);

CREATE TABLE Producto (
    codigo VARCHAR(15) PRIMARY KEY,
    descripcion VARCHAR(200) NOT NULL,
    precio DECIMAL(8,2) NOT NULL,
    existencias INT NOT NULL
);

CREATE TABLE Compra (
    id_compra VARCHAR(20) PRIMARY KEY,
    codigo_cliente VARCHAR(15) NOT NULL,
    codigo_producto VARCHAR(15) NOT NULL,
    fecha DATE NOT NULL,
    FOREIGN KEY (codigo_cliente) REFERENCES Cliente(codigo),
    FOREIGN KEY (codigo_producto) REFERENCES Producto(codigo)
);

CREATE TABLE Proveedor (
    codigo VARCHAR(15) PRIMARY KEY,
    nombres VARCHAR(50) NOT NULL,
    apellidos VARCHAR(50) NOT NULL,
    direccion VARCHAR(200),
    provincia VARCHAR(50),
    telefono VARCHAR(15)
);

CREATE TABLE Suministra (
    codigo_producto VARCHAR(15) NOT NULL,
    codigo_proveedor VARCHAR(15) NOT NULL,
    PRIMARY KEY (codigo_producto, codigo_proveedor),
    FOREIGN KEY (codigo_producto) REFERENCES Producto(codigo),
    FOREIGN KEY (codigo_proveedor) REFERENCES Proveedor(codigo)
);
```

####   🖥️ Base de Datos:
<img width="243" height="218" alt="image" src="https://github.com/user-attachments/assets/46c12545-f812-48ec-9605-d4e0cce35df4" />



####  [Link de Drive](https://drive.google.com/drive/folders/1tC8E_G8xD8Vg-YQP8zLLZIoqTPgkaejA?usp=sharing)


---
###   🟢 Ejercicio 7: Discos Musicales:
💡 Descripción:

🔹 La tienda vende discos de diferentes géneros musicales y cantantes

🔹 Cada género musical tiene un identificador

🔹 Cada disco tiene título, género musical, precio y cantante

🔹 De un cantante se registra el nombre y su país, Todo disco pertenece a un cantante y un cantante puede tener muchos discos. Además, un disco tiene un conjunto de canciones (identificador y título)

🔹 Un disco tiene muchas canciones y una canción puede estar en varios discos. Interesa conocer la posición de una canción en un determinado disco


####   🖥️ Diagrama Entidad-Relación:
<img width="1579" height="670" alt="image" src="https://github.com/user-attachments/assets/7b19d096-db7f-4ebb-ae77-cd370e65425e" />

####   🗺️ Modelo Relacional:
<img width="999" height="1307" alt="image" src="https://github.com/user-attachments/assets/c44b3140-8c15-431a-af60-d47bb850bc7f" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Genero (
    id_genero VARCHAR(15) PRIMARY KEY
);

CREATE TABLE Cantante (
    id_cantante VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    pais VARCHAR(50) NOT NULL
);

CREATE TABLE Disco (
    identificador VARCHAR(20) PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    precio DECIMAL(6,2) NOT NULL,
    id_genero VARCHAR(15) NOT NULL,
    id_cantante VARCHAR(15) NOT NULL,
    FOREIGN KEY (id_genero) REFERENCES Genero(id_genero),
    FOREIGN KEY (id_cantante) REFERENCES Cantante(id_cantante)
);

CREATE TABLE Cancion (
    id_cancion VARCHAR(20) PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL
);

CREATE TABLE Contiene (
    id_disco VARCHAR(20) NOT NULL,
    id_cancion VARCHAR(20) NOT NULL,
    posicion INT NOT NULL,
    PRIMARY KEY (id_disco, id_cancion),
    FOREIGN KEY (id_disco) REFERENCES Disco(identificador),
    FOREIGN KEY (id_cancion) REFERENCES Cancion(id_cancion)
);
```

####   🖥️ Base de Datos:
<img width="243" height="209" alt="image" src="https://github.com/user-attachments/assets/5404d114-06a6-4814-b34f-0fca1a1e00a5" />



####  [Link de Drive](https://drive.google.com/drive/folders/1G3xQmpwxqGkur2PfI7_OYtHAcB4PGqLa?usp=sharing)


---
###   🔵 Ejercicio 8: Camiones:
💡 Descripción: 

Se desea informatizar la gestión de una empresa de transporte que reparte paquetes por toda España. Los encargados de llevar los paquetes son los camioneros, de los que se quiere guardar la cédula, nombre, teléfono, dirección, salario y población en que vive. De los paquetes transportados interesa conocer el código del paquete, descripción, destinatario y dirección del destinatario.

Un camionero distribuye muchos paquetes, y un paquete sólo puede ser distribuido por un camionero. De las provincias a las que llegan los paquetes interesa guardar el código de provincia y el nombre. Un paquete sólo puede llegar a una provincia. Sin embargo, a una provincia pueden llegar varios paquetes.

De los camiones que llevan los camioneros, interesa conocer la matrícula, modelo, tipo y potencia. Un camionero puede conducir diferentes camiones en fechas diferentes, y un camión puede ser conducido por varios camioneros.


####   🖥️ Diagrama Entidad-Relación:
<img width="1245" height="834" alt="image" src="https://github.com/user-attachments/assets/5b3e415d-1636-43c0-87e3-94223a868308" />

####   🗺️ Modelo Relacional:
<img width="1110" height="906" alt="image" src="https://github.com/user-attachments/assets/f054c39b-b983-4594-a91d-a68e18080805" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Provincia (
    codigo VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);

CREATE TABLE Camionero (
    cedula VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(15),
    direccion VARCHAR(200),
    salario DECIMAL(8,2) NOT NULL,
    poblacion VARCHAR(50)
);

CREATE TABLE Paquete (
    codigo VARCHAR(20) PRIMARY KEY,
    descripcion VARCHAR(200),
    destinatario VARCHAR(100) NOT NULL,
    direccion_destinatario VARCHAR(200) NOT NULL,
    codigo_provincia VARCHAR(15) NOT NULL,
    cedula_camionero VARCHAR(15) NOT NULL,
    FOREIGN KEY (codigo_provincia) REFERENCES Provincia(codigo),
    FOREIGN KEY (cedula_camionero) REFERENCES Camionero(cedula)
);

CREATE TABLE Camion (
    matricula VARCHAR(15) PRIMARY KEY,
    modelo VARCHAR(50),
    tipo VARCHAR(50),
    potencia VARCHAR(20)
);

CREATE TABLE Conduce (
    cedula VARCHAR(15) NOT NULL,
    matricula VARCHAR(15) NOT NULL,
    fecha DATE NOT NULL,
    PRIMARY KEY (cedula, matricula, fecha),
    FOREIGN KEY (cedula) REFERENCES Camionero(cedula),
    FOREIGN KEY (matricula) REFERENCES Camion(matricula)
);
```

####   🖥️ Base de Datos:
<img width="251" height="215" alt="image" src="https://github.com/user-attachments/assets/def4a244-c51b-4749-b555-96fd9bc44143" />



####  [Link de Drive](https://drive.google.com/drive/folders/1RZKvtSmcNUIOk9PDdZWvTyZTa-PSODGP?usp=sharing)


---
###   🟡 Ejercicio 9: Veterinaria:
💡 Descripción: 

🔹 Del propietario interesa: cédula, apellidos, nombres, dirección, teléfonos

🔹 De la mascota interesa: identificador, nombre, fecha de nacimiento, tipo

🔹 Un propietario puede llevar una o varias mascotas

🔹 Una mascota la lleva uno y solo un propietario

🔹 Del personal de la clínica veterinaria interesa un código, la cédula, nombre

🔹 Entre el personal de la clínica se distingue a los veterinarios y los auxiliares, pudiendo haber otros tipos
   - Del veterinario se guarda además su fecha_alta y especialidad
   - De los auxiliares interesa su base de cotización

🔹Las mascotas pasan consulta con los veterinarios. De manera que:
   - Un veterinario puede pasar consulta a ninguna o a varias mascotas, y una misma mascota de la clínica ha pasado consulta una o más veces en fechas diferentes con varios veterinarios.
   - AL pasar consulta se realiza un diagnóstico.

🔹 Cada propietario tiene un familiar de contacto para casos de emergencia.
   - De este familiar interesa la cédula, nombre y teléfono.
   - Si el propietario se da de alta en la clínica veterinaria, el familiar ya no interesa.
   - El familiar es contacto de un solo propietario


####   🖥️ Diagrama Entidad-Relación:
<img width="1328" height="751" alt="image" src="https://github.com/user-attachments/assets/e468d47c-cc4b-4175-917d-94f9a2ce92b1" />

####   🗺️ Modelo Relacional:
<img width="1040" height="1762" alt="image" src="https://github.com/user-attachments/assets/2e5954f4-c114-43ac-946e-e2061743a083" />

####   📋 Codigo SQL:
```SQL
CREATE TABLE Propietario (
    cedula VARCHAR(15) PRIMARY KEY,
    nombres VARCHAR(50) NOT NULL,
    apellidos VARCHAR(50) NOT NULL,
    direccion VARCHAR(200)
);

CREATE TABLE Telefono_propietario (
    cedula VARCHAR(15) NOT NULL,
    telefono VARCHAR(15) NOT NULL,
    PRIMARY KEY (cedula, telefono),
    FOREIGN KEY (cedula) REFERENCES Propietario(cedula)
);

CREATE TABLE Familiar_contacto (
    cedula VARCHAR(15) PRIMARY KEY,
    cedula_propietario VARCHAR(15) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(15),
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Mascota (
    id_mascota VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    tipo VARCHAR(30) NOT NULL,
    fecha_nacimiento DATE,
    cedula_propietario VARCHAR(15) NOT NULL,
    FOREIGN KEY (cedula_propietario) REFERENCES Propietario(cedula)
);

CREATE TABLE Personal (
    codigo VARCHAR(15) PRIMARY KEY,
    cedula VARCHAR(15) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Veterinario (
    codigo VARCHAR(15) PRIMARY KEY,
    fecha_alta DATE NOT NULL,
    especialidad VARCHAR(50),
    FOREIGN KEY (codigo) REFERENCES Personal(codigo)
);

CREATE TABLE Consulta (
    id_mascota VARCHAR(15) NOT NULL,
    codigo_veterinario VARCHAR(15) NOT NULL,
    fecha DATE NOT NULL,
    diagnostico VARCHAR(255),
    PRIMARY KEY (id_mascota, codigo_veterinario, fecha),
    FOREIGN KEY (id_mascota) REFERENCES Mascota(id_mascota),
    FOREIGN KEY (codigo_veterinario) REFERENCES Veterinario(codigo)
);

CREATE TABLE Auxiliar (
    codigo VARCHAR(15) PRIMARY KEY,
    base_cotizacion DECIMAL(8,2) NOT NULL,
    FOREIGN KEY (codigo) REFERENCES Personal(codigo)
);
```

####   🖥️ Base de Datos:
<img width="308" height="278" alt="image" src="https://github.com/user-attachments/assets/2bd16183-b73f-4c5f-83cf-9639e79e5a1f" />



####  [Link de Drive](https://drive.google.com/drive/folders/1tK3lw2Ddf1jrLu0VS-DtI63q4aQorKpj?usp=sharing)


---

##   📝 ***Reflexiones y Conclusiones***
###   📌 Conclusiones:
🔹 

🔹 

🔹 



###   💡 Reflexión crítica:

🔹 

🔹 

🔹 


---

###    <p align="center"><strong>[***Volver atrás***](Portafolio.md)</strong></p>




