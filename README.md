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

Laptops (atributos en plural)

Laptop (Cuadrado)
	-color (ovalo)
	-anio (ovalo)
	-modelo (ovalo)
	-no de serie (ovalo)
	-disco duro (doble ovalo)
	-metodo de entrada (ovalo-atributo compuesto) puede tener trackpad, teclado.
	-antiguedad (ovalo border dotted)
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
Las entidades debiles por identidad quiere decir que no se diferencian entre si mas que por la clave de su entidad fuerte
En el caso de los libros, la tabla Ejemplares deberia llevar como llave el ID del libro.
Eso la hara dependiente y debil haci el libro, a traves de su campo de identificacion (del libro). 

2.- Por existencia
Si se le agrega un ID propio a la tabla de Ejemplares, se vuelven debiles, ya no por identidad por que se pueden identificar solas, pero si por existencia y esto significa que aunque agregues un ID que es diferente del libro y es propia del ejemplar, aun asi no puede conceptualmente tener un ejemplar sin un libro entonces aunque no depende por el ID o por la identidad de la entidad fuerte aun asi conceptualmente no podemos tener un ejemplar sin un libro primero.
 


