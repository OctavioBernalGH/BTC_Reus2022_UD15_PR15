<h1>Proyecto UD15 Ejercicio número 18</h1>

Este ejercicio ha sido realizado por los miembros del equipo 1. Dicho equipo esta formado por:

[- Octavio Bernal](https://github.com/OctavioBernalGH)<br>
[- David Dalmau](https://github.com/DavidDalmauDieguez)<br>
[- J.Oriol López Bosch](https://github.com/mednologic)

<p align="justify"> En este proyecto simularemos una inmobiliaria , donde un cliente podrá comprar o alquilar un inmueble, ya sea del tipo inmueble (no definido) como del tipo local, garaje o piso. 
	
Para ello hemos diseñado primero el modelo entidad relación y el modelo relacional.</p>

![ER-MR-esquema](https://user-images.githubusercontent.com/103035621/165505620-e75e395d-857d-4de1-a10e-897d3d1d6910.png)

A continuación se muestra una imagen que define el esquema de datos realizado con ingenieria inversa.

![UD_15_PROYECTO](https://user-images.githubusercontent.com/103035621/165376459-91d218d2-4da3-4cbf-a825-843322560e7b.PNG)


<p align="justify">La imagen mostrada anteriomente representa al esquema realizado con ingenieria inversa mediante un gestor de base de datos , a continuación separaré el código de cada tabla creada para poder visualizarlo con mayor claridad, pero antes de todo insertaré en un spoiler todo el fichero SQL.
</p>

<details>
  <summary>Código SQL íntegro.</summary>
<br>
<p align="justify">Este código corresponde al ficherl SQL íntegro , este fichero esta compuesto por sentencias de creación de tablas y también sentencias insert' para comprobar su funcionamiento. Como se puede observar hay comentarios en la zona de los insert's, esos comentarios están ahí para no ejecutar las líneas que pueden producir error por integridad referencial, pertenecen ahí para demostrar su funcionamiento.</p>
  
  ```sql
DROP DATABASE UD14_EJERCICIO_18;
CREATE DATABASE UD14_EJERCICIO_18;
USE UD14_EJERCICIO_18;

CREATE TABLE UD14_EJERCICIO_18.persona
(
	dni VARCHAR (10)PRIMARY KEY, 
	FK_dni VARCHAR(10),
	CONSTRAINT FK_dni FOREIGN KEY (FK_dni) REFERENCES persona(dni) ON DELETE 	CASCADE ON UPDATE CASCADE, 
	nombre VARCHAR (20)NOT NULL, 
	apellidos VARCHAR (20)NOT NULL, 
	teléfono_fijo INT NOT NULL,
	teléfono_movil INT NOT NULL UNIQUE, 
	codigo_personal INT NOT NULL UNIQUE AUTO_INCREMENT
);

CREATE TABLE inmueble 
(
	codigo_inmueble INT AUTO_INCREMENT PRIMARY KEY,
	direccion VARCHAR(40)NOT NULL,
	descripcion VARCHAR(100) NOT NULL,
	metros_inmueble FLOAT NOT NULL
);

CREATE TABLE alquiler 
(
	codigo_alquiler INT AUTO_INCREMENT PRIMARY KEY,
	año INT NOT NULL,
	mes INT NOT NULL,
	valor FLOAT(10,4) NOT NULL,
	FK_personaalquiler VARCHAR(20),
    FK_codigoinmueble_A INT,
	CONSTRAINT FK_personaalquiler FOREIGN KEY (FK_personaalquiler) REFERENCES persona(dni) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT FK_codigoinmueble_A FOREIGN KEY (FK_codigoinmueble_A) REFERENCES inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE compra 
(
	codigo_compra INT AUTO_INCREMENT PRIMARY KEY,
	año INT NOT NULL,
	fecha DATE NOT NULL,
	valor FLOAT(10,4) NOT NULL,
	FK_personacompra VARCHAR(20),
    FK_codigoinmueble_C INT,
	CONSTRAINT FK_personacompra FOREIGN KEY (FK_personacompra) REFERENCES persona(dni) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
	CONSTRAINT FK_codigoinmueble_C FOREIGN KEY (FK_codigoinmueble_C) REFERENCES inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE locales
(
	id_local INT AUTO_INCREMENT PRIMARY KEY,
	uso_local VARCHAR(30)NOT NULL,
	tiene_servicio VARCHAR(30) NOT NULL,
	FK_inmueble INT, 
	CONSTRAINT FK_inmueble FOREIGN KEY (FK_inmueble) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE pisos (
	id_pisos INT AUTO_INCREMENT PRIMARY KEY,
	FK_inmueblepiso INT,
	CONSTRAINT FK_inmueblepiso FOREIGN KEY (FK_inmueblepiso) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
	);

CREATE TABLE garajes (
	id_garaje INT AUTO_INCREMENT PRIMARY KEY,
	num_garaje INT NOT NULL,
	planta_garaje INT NOT NULL,
	FK_inmueblegaraje INT,
	FK_pisosgaraje INT,
	CONSTRAINT FK_inmueblegaraje FOREIGN KEY (FK_inmueblegaraje) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
	CONSTRAINT FK_pisosgaraje FOREIGN KEY (FK_pisosgaraje) REFERENCES pisos(id_pisos) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
	);

/* INSERT EN LA TABLA PERSONA Y SELECT * FROM PERSONA */
/*====================================================*/
INSERT INTO persona (dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('4800000X' ,'OCTAVIO', 'BV' , 977000000, 6000000);
INSERT INTO persona (dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('3802551S' ,'URI', 'LOPEZ' , 94646446, 5000000);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('5698494X', '3802551S' ,'DAVID', 'DAVIDUBI' , 677000000, 4000000);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('1449849X', '3802551S' ,'JOSE', 'APELLIDO' , 35984884, 15656616);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('5425561Z', '3802551S' ,'PEPE', 'BOÑIGAS' , 55884481, 59984149);
/* SELECCIONAR TODO DE LA TABLA PERSONA */
SELECT * FROM persona ORDER BY codigo_personal;

/* INSERT EN LA TABLA INMUEBLE Y SELECT * FROM INMUEBLE */
/*====================================================*/
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/San Jose pepinero nº 10' , 'casa rustica', 120);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Pepapig' , 'piso ocupado', 88);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Cocacola' , 'terreno', 300);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Calsot' , 'casa', 50);
SELECT * FROM inmueble;

/* INSERTAR VALORES EN LA TABLA COMPRA */
/*====================================================*/
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2023, '2018/12/01', 128000, '5698494X',1);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2020, '2020/11/15', 220000, '1449849X',2);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2006, '2006/08/13', 68000, '3802551S',3);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (1998, '1998/03/22', 87000, '5698494X',4);
/* COMO EL DNI NO EXISTE EN LA CLASE REFERENCIADA , DA ERROR Y NO SE INSERTA EN LA TABLA */
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2016, '2016/01/15', 256000, '0000000X');
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2014, '2014/04/26', 78000, '1111111A');
/* COMO NO EXISTE NINGÚN INMUEBLE CON EL CÓDIGO 5 , DARÁ ERROR */
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2018, '2018/12/01', 128000, '5425561Z',5);
/* SELECCIONAR TODO DE LA TABLA COMPRA */
SELECT * FROM compra;

/* INSERTAR VALORES EN LA TABLA ALQUILER CON INTEGREDAD REFERENCIAL */
/*====================================================*/
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2022, 04, 450.61 ,'5425561Z', 1);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2022, 04, 1000.61 ,'5425561Z', 2);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2021, 05, 450.61 ,'3802551S', 3);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2012, 08, 550.61 ,'5698494X', 4);
/* COMO EL DNI NO EXISTE EN LA CLASE REFERENCIADA , DA ERROR Y NO SE INSERTA EN LA TABLA */
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (1996, 11, 800.5 ,'0000000Z');
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2014, 01, 350.5 ,'1111111A');
/* COMO EL CODIGO INMUEBLE NO EXISTE , SALTARÁ MENSAJE DE ERROR */
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2003, 09, 600.15 ,'1449849X', 5);
/* SELECCIONAR TODO DE LA TABLA ALQUILER */
SELECT * FROM alquiler;

/* SE INSERTAN 2 LOCALES CON CÓDIGO DE INMUEBLE*/
/*====================================================*/
INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('restauración' , 'si', 1);
/* LOS SIGUIENTES INSERT DARÁN ERROR POR INTEGRIDAD REFERENCIAL , NO EXISTE EL CODIGO DEL INMUEBLE O ESTÁ VACÍO */
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si', 10);
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si', 0);
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si');
SELECT * FROM locales;

/* SE INSERTAN 2 PISOS DE FORMA CORRECTA EN LA TABLA PISOS*/
/*====================================================*/ 
INSERT INTO pisos (FK_inmueblepiso) VALUES(2);
INSERT INTO pisos (FK_inmueblepiso) VALUES(3);
/* NO SE PUEDEN INSERTAR LOS SIGUIENTES REGISTROS POR QUE ESOS VALORES NO EXISTEN , DA ERROR*/
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(10); */
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(0); */
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(); */
/* MOSTRAMOS LA TABLA PISOS */
SELECT * FROM pisos;

/* SE INSERTAN LOS DIFERENTES GARAJES */
/*====================================================*/
INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 1);
INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 2);
/* SALTA ERROR POR INTEGRIDAD REFERENCIAL , LA CLAVE PISOSGARAJE NO EXISTE */
	/* INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 3); */
/* SALTA ERROR POR INTEGRIDAD REFERENCIAL , LA CLAVE INMUEBLEGARAJE NO EXISTE */
	/* INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 8, 1); */   
/* SE MUESTRA LA TABLA GARAJES */
SELECT * FROM garajes;


  ```
 </details>
 <br>
 <p align="justify">
 1. Primero se crea la tabla persona con sus diferentes campos. Esta tabla estará formada por las columnas (dni, nombre, apellidos, teléfono_fijo, teléfono_movil y código_personal). Al ser reflexiva tendrá la columna FK_dni que tendrá como referencia persona(dni). </p>

![PERSONA](https://user-images.githubusercontent.com/103035621/165376624-76c572f1-218f-4527-8c7b-78294e5ea8a0.PNG)


<details>
  <summary>Código SQL Tabla PERSONA.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de PERSONA donde tomamos todos sus datos personales y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE UD14_EJERCICIO_18.persona
(
	dni VARCHAR (10)PRIMARY KEY, 
	FK_dni VARCHAR(10),
	CONSTRAINT FK_dni FOREIGN KEY (FK_dni) REFERENCES persona(dni) ON DELETE 	CASCADE ON UPDATE CASCADE, 
	nombre VARCHAR (20)NOT NULL, 
	apellidos VARCHAR (20)NOT NULL, 
	teléfono_fijo INT NOT NULL,
	teléfono_movil INT NOT NULL UNIQUE, 
	codigo_personal INT NOT NULL UNIQUE AUTO_INCREMENT
);
  ```
 </details>
 <br>
<p align="justify"> 
2. Se crea la tabla inmueble con los diferentes campos. Definos 4 columnas (codigo_inmueble, direccion, descripcion y metros_inmueble), la primera de ellas como clave primaria.</p>

![INMUEBLE](https://user-images.githubusercontent.com/103035621/165376787-0b5a5e93-9fb0-4ac4-9a99-b7a1ed86eb62.PNG)


 <details>
  <summary>Código SQL Tabla INMUEBLE.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de INMUEBLE donde tomamos todos los datos genericos del imueble y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE inmueble 
(
	codigo_inmueble INT AUTO_INCREMENT PRIMARY KEY,
	direccion VARCHAR(40)NOT NULL,
	descripcion VARCHAR(100) NOT NULL,
	metros_inmueble FLOAT NOT NULL
);
  ```
 </details>
 <br>
 <p align="justify">
  3. Se crea la tabla alquiler con sus diferentes campos. Esta tabla tendrá como columnas (codigo_alquiler, año, mes, valor) , la primera columna "codigo_alquiler" como clave primaria. Esta tabla estará referenciada por la tabla persona y inmueble, para ello tendrá 2 claves foráneas (FK_personaalquiler) que hará referencia a la tabla persona(dni) y (FK_codigoinmueble_A) que hará referencia a la tabla inmueble(codigo_inmueble).</p>


![ALQUILER](https://user-images.githubusercontent.com/103035621/165376909-5b44c2f8-af0d-4a01-a973-f59f00a19bb0.PNG)


<details>
  <summary>Código SQL Tabla ALQUILER.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de ALQUILER donde tomamos todos los datos y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE alquiler 
(
	codigo_alquiler INT AUTO_INCREMENT PRIMARY KEY,
	año INT NOT NULL,
	mes INT NOT NULL,
	valor FLOAT(10,4) NOT NULL,
	FK_personaalquiler VARCHAR(20),
    FK_codigoinmueble_A INT,
	CONSTRAINT FK_personaalquiler FOREIGN KEY (FK_personaalquiler) REFERENCES persona(dni) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT FK_codigoinmueble_A FOREIGN KEY (FK_codigoinmueble_A) REFERENCES inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);
  ```
 </details>
 <br>
 <p align="justify">
   4. Se crea la tabla compra con sus diferentes campos. Dicha tabla tendrá como columnas (codigo_compra, año, fecha, valor) , la primera de ellas como clave primaria, y como clave foránea tendra "FK_personacompra" que referencia persona(dni) y "FK_codigoinmueble_C" que hará referencia a inmueble(codigo_inmueble).</p>

![COMPRA](https://user-images.githubusercontent.com/103035621/165377051-02c574ab-76e9-4004-a116-7590d235502e.PNG)


<details>
  <summary>Código SQL Tabla COMPRA.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de COMPRA donde tomamos todos los datos y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE compra 
(
	codigo_compra INT AUTO_INCREMENT PRIMARY KEY,
	año INT NOT NULL,
	fecha DATE NOT NULL,
	valor FLOAT(10,4) NOT NULL,
	FK_personacompra VARCHAR(20),
    FK_codigoinmueble_C INT,
	CONSTRAINT FK_personacompra FOREIGN KEY (FK_personacompra) REFERENCES persona(dni) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
	CONSTRAINT FK_codigoinmueble_C FOREIGN KEY (FK_codigoinmueble_C) REFERENCES inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);
  ```
 </details>
 <br>
 <p align="justify">
   5. Se crea la tabla locales con sus diferentes campos. Esta tabla estará formada por 4 columnas (id_local, uso_local y tiene_servicio), la primera de ellas como clave primaria y tendrá "FK_inmueble" como clave foránea , esta hará referencia a inmueble(codigo_inmueble).</p>


![LOCALES](https://user-images.githubusercontent.com/103035621/165377207-18367f64-8e15-4914-96f2-5689556a2fd1.PNG)



<details>
  <summary>Código SQL Tabla LOCALES.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de LOCALES donde tomamos todos los datos y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE locales
(
	id_local INT AUTO_INCREMENT PRIMARY KEY,
	uso_local VARCHAR(30)NOT NULL,
	tiene_servicio VARCHAR(30) NOT NULL,
	FK_inmueble INT, 
	CONSTRAINT FK_inmueble FOREIGN KEY (FK_inmueble) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
);
  ```
 </details>
 <br>
 <p align="justify">
   6. Se crea la tabla pisos con sus diferentes campos. Estará formada por (id_pisos) como única columna propia y de tipo clave primaria, y "FK_inmueblepiso" como clave foránea de inmueble(codigo_inmueble).</p>

![PISOS](https://user-images.githubusercontent.com/103035621/165377329-aa15f4eb-bd51-4b51-ab2b-f33b76583068.PNG)


<details>
  <summary>Código SQL Tabla PISOS.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de PISOS donde tomamos todos los datos y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE pisos (
	id_pisos INT AUTO_INCREMENT PRIMARY KEY,
	FK_inmueblepiso INT,
	CONSTRAINT FK_inmueblepiso FOREIGN KEY (FK_inmueblepiso) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
	);
  ```
 </details>
 <br>
 <p align="justify">
   7. Por último se crea la tabla garajes con sus diferentes campos. Esta tabla tendra como columnas (id_garaje, num_garaje, planta_garaje) , la id_garaje como clave primaria, y como claves foráneas tendrá a "FK_inmueblegaraje" que hará referéncia a inmueble(codigo_inmueble) y a "FK_pisosgaraje" que apuntará directamente a pisos(id_pisos).</p>


![GARAJES](https://user-images.githubusercontent.com/103035621/165377477-1406394e-5f0b-412f-84c6-1d463c3a4378.PNG)


<details>
  <summary>Código SQL Tabla GARAJES.</summary>
<br>
<p align="justify">Este código corresponde a la tabla de GARAJES donde tomamos todos los datos y los almacenamos en las diferentes columnas</p>
  
  ```sql
CREATE TABLE garajes (
	id_garaje INT AUTO_INCREMENT PRIMARY KEY,
	num_garaje INT NOT NULL,
	planta_garaje INT NOT NULL,
	FK_inmueblegaraje INT,
	FK_pisosgaraje INT,
	CONSTRAINT FK_inmueblegaraje FOREIGN KEY (FK_inmueblegaraje) REFERENCES 	inmueble(codigo_inmueble) 
    ON 	DELETE CASCADE ON UPDATE CASCADE,
	CONSTRAINT FK_pisosgaraje FOREIGN KEY (FK_pisosgaraje) REFERENCES pisos(id_pisos) 
    ON 	DELETE CASCADE ON UPDATE CASCADE
	);
  ```
 </details>
 <br>
 <br>
 
 <p align="justify">
 8. Tras finalizar el modelo entidad relación, el modelo relacional y el diseño de las diferentes tablas, hemos procedido a realizar insert's en cada una de ellas para comprobar la integridad referencial y el correcto funcionamiento del diseño. Tras realizar las diferentes pruebas, introduzco las sentencias en un spoiler separadas del código principal para su posterior uso.</p>
 
 <details>
  <summary>Sentencias INSERT utilizadas.</summary>
<br>
<p align="justify">Este código corresponde los diferentes insert utilizados para comprobar el funcionamiento.</p>
  
  ```sql
	/* INSERT EN LA TABLA PERSONA Y SELECT * FROM PERSONA */
/*====================================================*/
INSERT INTO persona (dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('4800000X' ,'OCTAVIO', 'BV' , 977000000, 6000000);
INSERT INTO persona (dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('3802551S' ,'URI', 'LOPEZ' , 94646446, 5000000);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('5698494X', '3802551S' ,'DAVID', 'DAVIDUBI' , 677000000, 4000000);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('1449849X', '3802551S' ,'JOSE', 'APELLIDO' , 35984884, 15656616);
INSERT INTO persona (dni, FK_dni, nombre, apellidos, teléfono_fijo, teléfono_movil) VALUES ('5425561Z', '3802551S' ,'PEPE', 'BOÑIGAS' , 55884481, 59984149);
/* SELECCIONAR TODO DE LA TABLA PERSONA */
SELECT * FROM persona ORDER BY codigo_personal;

/* INSERT EN LA TABLA INMUEBLE Y SELECT * FROM INMUEBLE */
/*====================================================*/
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/San Jose pepinero nº 10' , 'casa rustica', 120);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Pepapig' , 'piso ocupado', 88);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Cocacola' , 'terreno', 300);
INSERT INTO inmueble (direccion, descripcion, metros_inmueble) VALUES ('C/Calsot' , 'casa', 50);
SELECT * FROM inmueble;

/* INSERTAR VALORES EN LA TABLA COMPRA */
/*====================================================*/
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2023, '2018/12/01', 128000, '5698494X',1);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2020, '2020/11/15', 220000, '1449849X',2);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2006, '2006/08/13', 68000, '3802551S',3);
INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (1998, '1998/03/22', 87000, '5698494X',4);
/* COMO EL DNI NO EXISTE EN LA CLASE REFERENCIADA , DA ERROR Y NO SE INSERTA EN LA TABLA */
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2016, '2016/01/15', 256000, '0000000X');
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2014, '2014/04/26', 78000, '1111111A');
/* COMO NO EXISTE NINGÚN INMUEBLE CON EL CÓDIGO 5 , DARÁ ERROR */
	-- INSERT INTO compra (año, fecha, valor, FK_personacompra, FK_codigoinmueble_C) VALUES (2018, '2018/12/01', 128000, '5425561Z',5);
/* SELECCIONAR TODO DE LA TABLA COMPRA */
SELECT * FROM compra;

/* INSERTAR VALORES EN LA TABLA ALQUILER CON INTEGREDAD REFERENCIAL */
/*====================================================*/
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2022, 04, 450.61 ,'5425561Z', 1);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2022, 04, 1000.61 ,'5425561Z', 2);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2021, 05, 450.61 ,'3802551S', 3);
INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2012, 08, 550.61 ,'5698494X', 4);
/* COMO EL DNI NO EXISTE EN LA CLASE REFERENCIADA , DA ERROR Y NO SE INSERTA EN LA TABLA */
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (1996, 11, 800.5 ,'0000000Z');
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2014, 01, 350.5 ,'1111111A');
/* COMO EL CODIGO INMUEBLE NO EXISTE , SALTARÁ MENSAJE DE ERROR */
	-- INSERT INTO alquiler (año, mes, valor, FK_personaalquiler, FK_codigoinmueble_A) VALUES (2003, 09, 600.15 ,'1449849X', 5);
/* SELECCIONAR TODO DE LA TABLA ALQUILER */
SELECT * FROM alquiler;

/* SE INSERTAN 2 LOCALES CON CÓDIGO DE INMUEBLE*/
/*====================================================*/
INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('restauración' , 'si', 1);
/* LOS SIGUIENTES INSERT DARÁN ERROR POR INTEGRIDAD REFERENCIAL , NO EXISTE EL CODIGO DEL INMUEBLE O ESTÁ VACÍO */
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si', 10);
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si', 0);
	-- INSERT INTO locales (uso_local, tiene_servicio, FK_inmueble) VALUES('comercial' , 'si');
SELECT * FROM locales;

/* SE INSERTAN 2 PISOS DE FORMA CORRECTA EN LA TABLA PISOS*/
/*====================================================*/ 
INSERT INTO pisos (FK_inmueblepiso) VALUES(2);
INSERT INTO pisos (FK_inmueblepiso) VALUES(3);
/* NO SE PUEDEN INSERTAR LOS SIGUIENTES REGISTROS POR QUE ESOS VALORES NO EXISTEN , DA ERROR*/
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(10); */
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(0); */
	/* INSERT INTO pisos (FK_inmueblepiso) VALUES(); */
/* MOSTRAMOS LA TABLA PISOS */
SELECT * FROM pisos;

/* SE INSERTAN LOS DIFERENTES GARAJES */
/*====================================================*/
INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 1);
INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 2);
/* SALTA ERROR POR INTEGRIDAD REFERENCIAL , LA CLAVE PISOSGARAJE NO EXISTE */
	/* INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 4, 3); */
/* SALTA ERROR POR INTEGRIDAD REFERENCIAL , LA CLAVE INMUEBLEGARAJE NO EXISTE */
	/* INSERT INTO garajes (num_garaje, planta_garaje, FK_inmueblegaraje, FK_pisosgaraje) VALUES (5, 2, 8, 1); */   
/* SE MUESTRA LA TABLA GARAJES */
SELECT * FROM garajes;
  ```
 </details>
 <br>
