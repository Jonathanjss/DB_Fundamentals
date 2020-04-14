# Historia de la persistencia de la informacion

Guardar informacion que no cambiara y que persistiera mas alla de la vida del hombre.

piedras, tablillas de arcilla (no transportables y fragiles)
papiros y pergamino (basado en materia animal/ vegetal, mas ligeros)
papel
microfilm (maquinas especializadas para almacenar, leer/escribir)
medios digitales (CD, HDD, SSD)
Cloud (Accesible desde cualquier parte del mundo, medio centralizado, puede ser usado por varias personas).

## BASE DE DATOS
Entran en el periodo en el que se hizo la transicion a medios digitales y ahora a la nube

### Tipos de bases de datos

### Relacionales
SQL Server, Oracle, PostgresSQL, MySQL, MariaDB

### No Relacionales
Memcash
Cassandra
DinamoDB
neo4j
nodejs

### Servicios
### Auto administrados
BD que instalamos, actualizamos y mantenimiento (update, parches, consistencia de datos) 

### Administrados
Servicios que ofrecen las nubes modernas Amazon, Google, Azure. Ofrecen el servicio de DB, vamos a usarla sin necesidad de instalar ni dar mantenimiento.


## HISTORIA DE LAS RDB.
### Introduccion a las bases de datos relacionales.

Surgen de la necesidad de conservar la informacion mas alla de la memoria RAM.

Necesidad de empezar a guardar datos y extraer.

DB basada en archivos.- Texto plano con datos separados por comas.

Al tener la necesidad de tener algo mas estructurado, surgen las RDB.

### RDB
Inventadas por Edgar Codd. Los 12 mandamientos de Codd para establecer un estandar mas consistente/ solido.

### Algebra relacional
Esta relacionada en como tenemos datos que se pueden empezar a mezclar y unir a traves de diferentes propiedades y caracteristicas.

## ENTIDADES Y ATRIBUTOS
Entidad.- Similar a un objeto. Representa algo en el mundo real.

Ejemplos:
|--------|
|Manzana |
|--------|

|---------|
|Automovil|
|---------|

Las entidades tienen atributos (cosas que lo hacen ser una entidad).

Automovil, tiene:
		-Volante (se encierra en un ovalo) y se une al rectangulo automovil.
		-Llantas como tiene mas de 1 llanta (atributo multivaluado), se encierra en doble ovalo y se une al rectangulo. 
		-Motor (en ovalo), atributo compuesto porque esta compuesto de otros atributos, el motor por si mismo esta formado por diversas partes.
		-Modelo del auto
		-Antiguedad.- Si quisieramos sacar la antiguedad del automovil, ese seria un atributo especial, la antiguedad la puedo diferir sabiendo el anio en que salio.





Entidad:

Laptops (Entidadess en plural)

Laptop (Rectangulo)
	-color (ovalo)
	-anio (ovalo)
	-modelo (ovalo)
	-no de serie (ovalo y la palabra subrayada ya que es una llave primary)
	-disco duro (doble ovalo - una computadora puede tener uno o mas discos duros)
	-metodo de entrada (ovalo-atributo compuesto) puede tener trackpad, teclado.
	-antiguedad (ovalo border dotted) (atributo especial) se infiere en el anio en que salio (anio)
	-pantalla 

El no de serie nos ayuda a identificarla de manera unica dentro del conjunto.
Si tenemos 2 laptops iguales, el id es el que nos ayudara a diferenciarla.

|-----|----|--------|
|color|anio|pantalla|
|-----|----|--------|
|gris |2017|AX4829i |
|negro|2019|AX4930i |
|negro|2018|AX4930i |
|gris |2017|AX4829i |
|-----|----|--------|

Tenemos en los atributos el color, tenemos computadoras de color gris, negro, negro y gris. Tenemos el anio, computadoras del 2017, 2019, 2018, 2017 y pantallas de diferentes modelos.
El problema es que hasta este punto tenemos computadoras que son iguales. Ejemplo, la 1 y la 4, ambas son grises, ambas son del 2017 y tienen el mismo modelo de pantalla, esto no debemos permitirlo al crear una entidad en nuestra BD.
Por lo tanto agregamos un atributo extra, el atributo llave que en este caso seria el ID de la laptop (no de serie)

## Atributos llave
Existen 2 grandes tipos:

### Atributos naturales.
Que es inherente al objeto, por ejemplo, el numero de serie viene en la computadora y no se pueden separar.
Otro ejemplo seria el ISBN en el caso de los libros que son estandares numericos que son unicos.

### Atributos artificiales.
En caso de que no tenga se puede asignar una clave que sea artificial, por que no viene inherente al objeto en si mismo sino que nosotros por conveniencia y para poder diferenciarlo se lo asignamos de manera arbitraria.

## 2 tipos de entidades
### Entidades fuertes.
No depende de ninguna otra entidad para existir.

### Entidades debiles.
Significa que una entidad debil no puede existir sin una entidad fuerte.
Representadas con un cuadrado de doble linea.

Ejemplo:
                |------------|
                ||----------||
|------|        ||          ||
|Libros|--------||Ejemplares||
|------|        ||          ||
                | ---------- |
                --------------
Libros es la entidad fuerte (conjunto de libros)
Ejemplares es la entidad debil

No puedes tener en una bibliteca un ejemplar de un libro que no tienes, necesitas forzozamente tener un libro para tener varios ejemplares de ese libro. Entonces si no tienes el libro para empezar, no puede tener ejemplares en tu biblioteca.


Las entidades debiles pueden ser debiles por 2 motivos:
1.- Por identidad.
Las entidades debiles por identidad quiere decir que no se diferencian entre si mas que por la clave de su entidad fuerte (llevan el id de la identidad fuerte)..
En el caso de los libros, la tabla Ejemplares deberia llevar como llave el ID del libro.
Eso la hara dependiente y debil haci el libro, a traves de su campo de identificacion (del libro). 

2.- Por existencia
Si se le agrega un ID propio a la tabla de Ejemplares, se vuelven debiles, ya no por identidad por que se pueden identificar solas, pero si por existencia y esto significa que aunque agregues un ID que es diferente del libro y es propia del ejemplar, aun asi no puede conceptualmente tener un ejemplar sin un libro entonces aunque no depende por el ID o por la identidad de la entidad fuerte aun asi conceptualmente no podemos tener un ejemplar sin un libro primero.

Les asignamos una clave propia y en este caso se vuelven debiles por existencia, y esto significa que aunque agreguemos un Id que es diferente al del libro y es propio de la tabla ejemplares, aun asi no conceptualmente no podemos tener un ejemplar sin un libro,
Tiene su propio Id que es diferente del libro pero propio del ejemplar, aun asi conceptualmente no podemos tener un ejemplar sin un libro.
Entonces aunque no depende por el Id o por la identidad de la entidad fuerte, aun asi conceptualmente no podemos tener un ejemplar si un libro primero.

## Entidades PlatziBlog
### Diagrama ER:PlatziBlog

### 1er paso: Identificar las entidades y pensar en los atributos.
Las entidades son cualquier objeto del mundo real (incluso virtual) que se puedan representar en un grupo.

### 2do: Relaciones
Manera en la que ligamos nuestras entidades.

Cuando tenemos atributos multivaluados los convertimos en entidades separadas por que tienen una vida por si misma y se pueden relacionar de varias maneras con otras entidades.

### Cardinalidad
Cuantos de un lado pertenecen a cuantos de otro lado.

--Cardinalidad 1 a 1.

--Cardinalidad 0 a 1 (o 1 a 1 opcional).
  Puede haber la opcion que no existan ninguno de uno de los lados.

--Cardinalidad 1 a N (uno a muchos).

--Cardinalidad 0 a N (1 a muchos opcional).

### Multiples a muchos.


## Diagrama ER


## Diagrama Fisico
### Tipos de Dato
Texto
 - CHAR(n)
 - VARCHAR(n) limite de 255
 - TEXT 500 para arriba

Numeros
 - INTEGER.- Numero que no tiene numero decimal o fraccion.
 - BIGINT.- Declarar de 99 o mas
 - SMALLINT.- Declarar numero de 99 o menos
 - DECIMAL (n,s) n es numero y lo siguiente los numeros decimales
 - NUMERIC (n,s)  

Fecha
 - DATE Solo anio mes y dia
 - TIME la hora del dia en formato 24 hrs
 - DATETIME tanto dia como la hora
 - TIMESTAMP igual que datetime

Logicos
 - BOOLEAN


CONSTRAINS (RESTRICCIONES)

NOT NULL - Se asegura que la columna no tenga valores nulos.
UNIQUE   - Se asegura que cada valor de la columna no se repita.
PRIMARY KEY - Es una combinacion de NOT NULL y UNIQUE.
FOREIGN KEY - Identifica de manera unica una tupla en otra tabla. Que quiere decir con que en la tabla foranea si se puede repetir la llave foranea?
CHECK    - Se asegura que el valor en la columna cumpla con una condicion dada.
DEFAULT  - Coloca un valor por defecto cuando no hay un valor especificado. El valor por DEFAULT es NULL. Podemos indicar que valor mostrara por default.
INDEX    - Se crea por columna para permitir busquedas mas rapidas.


## Normalizacion.
Primera forma normal (1FN): Atributos atómicos (Sin campos repetidos)
Segunda forma normal (2FN): Cumple 1FN y cada campo de la tabla debe depender de una clave única.
Tercera forma normal (3FN): Cumple 1FN y 2FN y los campos que NO son clave, NO deben tener dependencias.
Cuarta forma normal (4FN): Cumple 1FN, 2FN, 3FN y los campos multivaluados se identifican por una clave única.

## IMPORTANTE CARDINALIDAD.
La regla en general aqui es que cuando tienes 1 a 1 no importa a cual tabla le pongas la referencia de la otra tabla, es indistinto.
Cuando tienes cardinalidad 1 a muchos es muy importante que en la tabla donde tienes la terminacion muchos le pongas la llave foranea de la tabla que tiene 1.































# Fundamentos de base de datos.
Es un conjunto de informacion relacionada que se encuentra agrupada o estructurada.

### Entre las principales caracteristicas de los sistemas de base de datos podemos mencionar:

Independencia logica y fisica de datos.
Acceso concurrente por parte de multiples usuarios.
Consultas complejas optimizadas.
Respaldo y recuperacion.
Acceso a traves de lenguajes de programacion estandar.
















# YOUTUBE
### Que es un dato?
atributo (informacion) que refleja el valor de una caracteristica de un objeto.

Rojo es un dato del tipo cualitativo, indica cualidades como textura, color, experiencia.
3 es un dato de tipo cuantitativo, que se puede contar.

3 y rojo juntas no nos dice nada, pero tres manzanas de color rojo, en esta oracion se tiene una info mas completa y a esta oracion se le llamara "informacion".

Mucha informacion, ordenada, clasificada y relacionada con flechas para indicar que la info esta clasificada y enlazada.

### Que es una base de datos?
Es un conjunto de datos almacenados, organizados y relacionados entre si. El diseno de la base de datos se llama esquema de base de datos, las bases de datos cambian con el tiempo, conforme la informacion se inserte o se borre.
El esquema es el diseno de la base de datos.

Existen 2 esquemas:

Esquema fisico.- Describe el diseno fisico
Esquema logico.- Describe el diseno de BD en el nivel logico


### Ventajas de una base de datos.
Independencia de los datos
Menor redundancia
Mayor seguridad de los datos 
Datos mas documentados
Acceso eficiente a los datos
Menor espacio de almacenamiento

## Fundamentos basicos

Una tabla tiene fila/tupla y columna/campo


Tabla
____________________________________
|__________|_____________|_________|
|__________|_____________|_________|
|__________|_____________|_________|
|__________|_____________|_________|

Fila o Tupla
____________________________________
|__________|_____________|_________|

Columna o Ccampo
___________
|_________|
|_________|
|_________|


El atributo seran las cabeceras de la columna
____________________________________________
|  PAIS  |  CIUDAD  | NOMBRE  |  APELLIDO  |   ATRIBUTO.- Propiedad de las entidades (tabla)
--------------------------------------------

Los datos de la tabla se llaman registros
Grado:  numero de atributos o columnas en una tabla.
Cardinalidad: numero de filas en una tabla

## Diagramas entidad - relacion
PK es una columna que identifican de forma UNICA un registro en la tabla.
La seleccion de una clave primaria se debe realizar con cuidado, una mala eleccion podria afectar el diseno.

Reglas de la PK
no debe poseer valores nulos (NULL)
tiene que ser unica y que no se repita en el tiempo.



### Los atributos compuestos reemplazan los atributos (padre) y se convierten en campos.

### Los atributos multivaluados se convierten en tablas.



## Cardinalidad.
Es el numero de entidades con la cual otra entidad puede asociar mediante una relacion.

Para mostrar las cardinalidades se suele poner etiquetas en las lineas que unen las relaciones con las entidades.

Ejercicio:
Dado un codigo de departamento, conocer su nombre, director y empleados de ese Departamento con su nombre, categoria y dedicacion.

Dado un codigo de profesor, determinar su nombre, cargo y categoria, asi como el conjunto de asignaturas que imparte con el codigo de esa asignatura, su nombre, el colegio en el que se imparte y el n de horas



Los profesores de la asignatura de Bases de Datos de una  Universidad deciden crear una,base de datos que contenga la información de los resultados de las pruebas realizadas a los
alumnos. Para realizar el diseño se sabe que:

Los alumnos están definidos por su n° de matrícula, nombre y el grupo al que asisten a clase.

 Dichos alumnos realizan dos tipos de pruebas a lo largo del curso académico:

Exámenes escritos: cada alumno realiza varios a lo largo del curso, y se definen por el
n° de examen, el n° de preguntas de que consta y la fecha de realización (la misma
para todos los alumnos que realizan el mismo examen). Evidentemente, es importante
almacenar la nota de cada alumno por examen.

Prácticas: se realiza un n° indeterminado de ellas durante el curso académico, algunas serán en grupo y otras individuales. Se definen por un código de práctica, título y el grado de dificultad. En este caso los alumnos pueden examinarse de cualquier práctica cuando lo deseen, debiéndose almacenar la fecha y nota obtenida.

 En cuanto a los profesores, únicamente interesa conocer (además de sus datos personales: CI y nombre), quien es el qué ha diseñado cada práctica, sabiendo que en el diseño de una práctica puede colaborar más de uno, y que un profesor puede diseñar más de una práctica. Interesa, además, la fecha en que ha sido diseñada cada práctica por el profesor
correspondiente.




IMPORTANTE:
Cardinalidad de 1 a muchos
Cuando la cardinalidad es de uno a muchos automaticamente la llave primaria de la que tiene 1 simple relacion (cardinalidad 1) pasa a ser parte de la otra tabla.
La PK de la tabla 1 pasa a ser FK de la tabla muchos.

Normalizacion.
Proceso de aplicacion de normas  con el fin de ordenar y mejorar una base de datos.

Formas normales.
--Relaciones no normalizadas
--Primera forma normal
--Segunda forma normal
--Tercera forma normal
--Boyce codd
--Cuarta forma normal
--Quinta forma normal



Primera forma normal.
Todos los atributos tiene que tener valores atomicos. (No puede haber mas de un valor de un atributo en una tupla)(No debe existir registros duplicados)(No hay atributos mutivaluados).
La regla de la 1FN establece que las columnas repetidas deben eliminarse y colocarse en tablas separadas.

Segunda forma normal.
Tiene que estar en primera forma normal y que todos los atributos no clave dependan por completo de cualquier clave candidata.
Evitara redundancia por mezcla de propiedades de dos entidades en la misma relacion cuando existen dependencias incompletas.

Tercera formal normal.
Eliminar cualquier columna no clave que sea dependiente de otra columna no clave.







# RDB Que???
### RDBMS Relation Database Management System.
Programa que se encarga de cumplir las reglas de Cood y que se encarga de hacer BD.
 - MySQL
 - PostresSQL
 - Oracle



