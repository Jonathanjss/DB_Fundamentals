# SQL COMMANDS

## Create table.
### CREATE TABLE <tablename> (<columnName> <data type> <constraint>, <columnName> <data type> <constraint>, ...);
Como CONSTRAINT se necesita escribir la frase completa, por ejemplo PRIMARY KEY


## Insert values.
### INSERT INTO <tablename> VALUES (column values based in the columns set by us);
Example:
INSERT INTO groceries VALUES (1, "Bananas",4);
INSERT INTO groceries VALUES (2, "Peanut Butter",1);
INSERT INTO groceries VALUES (3, "Dark Chocolate bars",2);

## Consult values.
## Get all table's rows of a column.
### SELECT <column_name> FROM <tablename>;

## Get all table's fields.
### SELECT * FROM <tablename>;

## Order elements by...
### SELECT * FROM <tablename> ORDER BY <column_name>;

## Filter by
### SELECT * FROM <tablename> WHERE <column_name> </>/>=/<= <value> ORDER BY <column_name>;

# Add Data (Adding Function)

## Get total of something
### SELECT SUM(<column_name>) FROM <tablename>;

## Get the bigger quantity of a value in a column.
### SELECT MAX(<column_name>) FROM <tablename>;

## EXAMPLE
We want to be sure we have the correct articles number after we visit each aisle.

### SELECT SUM(quantity) FROM groceries GROUP BY aisle; 
It gives us total sum of elements by gruop, for example if we have 2 chocolate bars in the aisle 2 and 3 cereal boxes in the same aisle (2) the result will be 4.

To know how many articles in total there are in each asile, we make this:
### SELECT asile, SUM(quantity) FROM groceries GROUP BY aisle.

Primero hace el agrupamiento de los renglones con base en el pasillo. En primer lugar hizo el agrupamiento con GROUP BY
Luego sumo las cantidades de cada uno de esos grupos
Finalmente seleccion el valor del primer pasillo que vio en cada grupo


# S-Q-L o SEQUEL?
SQL (fl: ['zs 'kju 'e/] or B: ['si:kwel])

SQL fue inventado por IBM a principio de los 70s.
La primera version fue llamada SEQUEL (Structured English Query Language). Este acronimo fue cambiado despues por SQL (Structured Query Language) ya que SEQUEL era una marca registrada de una compania aerea.

# MORE COMPLEX QUERIES

## AND & OR
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

### Podemos ver que el id aparte de su datatype y su constraint, tiene el valor AUTOINCREMENT, con esto ya no tenemos que preocuparnos por ingresar dicho registro.

### Ahora, como el id es autoincremental, nosotros ya no necesitamos insertar valores en su columna, por lo tanto antes de insertar valores a la tabla, debemos declarar las columnas donde vamos a insertar dichos valores, es decir, insertar en las columnas siguientes a la columna id.
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);



SELECT * FROM exercise_logs WHERE calories > 50 ORDER BY calories;

### AND
SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30;

### OR
SELECT * FROM exercise_logs WHERE calories > 50 OR heart_rate > 100;

### AND tiene prioridad sobre OR
Como tener ambos operadores en una misma consulta y darle prioridad a OR

## Hacer consultas en subconsultas con IN.
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

### Seleccionar actividades al aire libre.
SELECT * FROM exercise_logs WHERE type = "biking" OR type = "hiking" OR type = "tree climbing" OR type = "rowing";

### Seleccionar actividades al aire libre pero con IN.
/* IN */
SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");

### Seleccion inversa para ver actividades dentro de casa.
/* NOT IN */
SELECT * FROM exercise_logs WHERE type NOT IN ("biking", "hiking", "tree climbing", "rowing");






CREATE TABLE drs_favorites
    (id INTEGER PRIMARY KEY,
    type TEXT,
    reason TEXT);

INSERT INTO drs_favorites(type, reason) VALUES ("biking", "Improves endurance and flexibility.");
INSERT INTO drs_favorites(type, reason) VALUES ("hiking", "Increases cardiovascular health.");

### Ver las actividades que recomienda el doctor.
SELECT type FROM drs_favorites;

SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites);
### En esta consulta se van a seleccionar todos los ejercicios de la tabla exercise_logs donde el type sera igual a los types contenidos dentro de la columna type de la tabla drs_favorites.
### Con esto la consulta es mas eficiente y siempre actualizada que poner un "...type IN ("biking", "hiking", "tree climbing", "rowing");", por que puede que en algun momento esos campos (biking, hiking, etc) dejen de existir.
### La consulta estara basada en los datos que hay en la tabla de doctores en ese momento.


### Si quisieramos hacer la busqueda por una palabra clave, por ejemplo "cardiovascular".
SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason = "Increases cardiovascular health.");

### En esta consulta tratara de encontrar la coincidencia exacta "Increases cardiovascular health."
### Pero si por alguna razon nos llega a faltar 1 punto o una coma o una letra, ya no hara match y no encontrara la palabra.


### Para hacer match por coincidencia, usamos el operador LIKE.
/* LIKE */

SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");

# Desafio
CREATE TABLE artists (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    country TEXT,
    genre TEXT);

INSERT INTO artists (name, country, genre)
    VALUES ("Taylor Swift", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Led Zeppelin", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("ABBA", "Sweden", "Disco");
INSERT INTO artists (name, country, genre)
    VALUES ("Queen", "UK", "Rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Celine Dion", "Canada", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Meatloaf", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Garth Brooks", "US", "Country");
INSERT INTO artists (name, country, genre)
    VALUES ("Shania Twain", "Canada", "Country");
INSERT INTO artists (name, country, genre)
    VALUES ("Rihanna", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Guns N' Roses", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Gloria Estefan", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Bob Marley", "Jamaica", "Reggae");

CREATE TABLE songs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    artist TEXT,
    title TEXT);

INSERT INTO songs (artist, title)
    VALUES ("Taylor Swift", "Shake it off");
INSERT INTO songs (artist, title)
    VALUES ("Rihanna", "Stay");
INSERT INTO songs (artist, title)
    VALUES ("Celine Dion", "My heart will go on");
INSERT INTO songs (artist, title)
    VALUES ("Celine Dion", "A new day has come");
INSERT INTO songs (artist, title)
    VALUES ("Shania Twain", "Party for two");
INSERT INTO songs (artist, title)
    VALUES ("Gloria Estefan", "Conga");
INSERT INTO songs (artist, title)
    VALUES ("Led Zeppelin", "Stairway to heaven");
INSERT INTO songs (artist, title)
    VALUES ("ABBA", "Mamma mia");
INSERT INTO songs (artist, title)
    VALUES ("Queen", "Bicycle Race");
INSERT INTO songs (artist, title)
    VALUES ("Queen", "Bohemian Rhapsody");
INSERT INTO songs (artist, title)
    VALUES ("Guns N' Roses", "Don't cry");
    

### Paso 1
Creamos una base de datos de canciones y artistas, y en este desafío vas a hacer listas de reproducción a partir de ellos. En este primer paso, selecciona el title de todas las canciones cuyo artist sea 'Queen'.

SELECT title FROM songs WHERE artist = "Queen";

### Paso 2
Ahora vas a hacer una lista de reproducción 'Pop'. En preparación, selecciona el name (nombre) de todos los artistas del género 'Pop'.
(Ayuda: asegúrate de que lo escribas como 'Pop', SQL considera eso diferente de 'pop').

SELECT name FROM artists WHERE genre = "Pop";

### Paso 3
Para terminar de crear la lista de reproducción 'Pop', agrega otra consulta que seleccionará el title de todas las canciones de los artistas de 'Pop'. Debe usar IN en una subconsulta anidada que esté basada en tu consulta previa.

SELECT title FROM songs WHERE artist IN (SELECT name FROM artists WHERE genre = "Pop");


# Restringir resultados agrupados con HAVING
Cuando usamos HAVING estamos aplicando las condiciones agrupados, no lo estamos haciendo con los valores individuales de los registros individuales.
Podemos usar cualquiera de las funciones de agregacion en columnas agrupadas que nos muestre algun resultado util como SUM, MIN, MAX AVG.

CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 115, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 45, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150
    ;

SELECT type, AVG(calories) AS avg_calories FROM exercise_logs
    GROUP BY type
    HAVING avg_calories > 70
    ; 
    
SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;

## HAVING clause
The  HAVING clause is used in the SELECT statement to specify filter conditions for a group of rows or aggregates.

The HAVING clause is often used with the GROUP BY clause to filter groups based on a specified condition. If the GROUP BY clause is omitted, the HAVING clause behaves like the WHERE clause.

### HAVING clause exercise.
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Prisoner of Azkaban", 107253);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Goblet of Fire", 190637);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Order of the Phoenix", 257045);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Half-Blood Prince", 168923);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Deathly Hallows", 197651);

INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Twilight", 118501);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "New Moon", 132807);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Eclipse", 147930);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Breaking Dawn", 192196);
    
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "The Hobbit", 95022);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Fellowship of the Ring", 177227);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Two Towers", 143436);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Return of the King", 134462);

### Paso 1
Creamos una base de datos de algunos autores populares y sus libros, con el conteo de palabras de cada libro. En este primer paso, selecciona todos los autores que han escrito más de 1 millón de palabras, usando GROUP BY y HAVING. Tu tabla de resultados debe incluir el 'author' y el conteo total de palabras como una columna 'total_words'.
### SELECT author,SUM(words) AS total_words FROM books GROUP BY author HAVING total_words > 1000000;


### Paso 2
Ahora selecciona todos los autores que escriben más de un promedio de 150,000 palabras por libro. Tu tabla de resultados debe incluir 'author' y el número promedio de palabras como una columna 'avg_words'.
### SELECT author,AVG(words) AS avg_words FROM books GROUP BY author HAVING avg_words > 150000;

# Cálculo de resultados con CASE.
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > 220 - 30;

/* 50-90% of max*/
SELECT COUNT(*) FROM exercise_logs WHERE
    heart_rate >= ROUND(0.50 * (220-30)) 
    AND  heart_rate <= ROUND(0.90 * (220-30));
    
/* CASE */
SELECT type, heart_rate,
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs;

SELECT COUNT(*),
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs
GROUP BY hr_zone;



## Final Test
/* Put your data in here and query it! */
/* Marvel Heroes and Villains
 Based on the website http://marvel.wikia.com/Main_Page
 with popularity data from http://observationdeck.io9.com/something-i-found-marvel-character-popularity-poll-cb-1568108064 
 and power grid data from http://marvel.wikia.com/Power_Grid#Power
 Collected by: https://www.khanacademy.org/profile/Mentrasto/
 */

CREATE TABLE marvels (ID INTEGER PRIMARY KEY,
    name TEXT,
    popularity INTEGER,
    alignment TEXT,
    gender TEXT, 
    height_m NUMERIC,
    weight_kg NUMERIC,
    hometown TEXT,
    intelligence INTEGER,
    strength INTEGER,
    speed INTEGER,
    durability INTEGER,
    energy_Projection INTEGER,
    fighting_Skills INTEGER);
    
INSERT INTO marvels VALUES(1, "Spider Man", 1, "Good", "Male", 1.78, 75.75, "USA", 4, 4, 3, 3, 1, 4); 
INSERT INTO marvels VALUES(2, "Iron Man", 20, "Neutral", "Male", 1.98, 102.58, "USA", 6, 6, 5, 6, 6, 4); 
INSERT INTO marvels VALUES(3, "Hulk", 18, "Neutral", "Male", 2.44, 635.29, "USA", 1, 7, 3, 7, 5, 4); 
INSERT INTO marvels VALUES(4, "Wolverine", 3, "Good", "Male", 1.6, 88.46, "Canada", 2, 4, 2, 4, 1, 7);
INSERT INTO marvels VALUES(5, "Thor", 5, "Good", "Male", 1.98, 290.3, "Asgard", 2, 7, 7, 6, 6, 4);
INSERT INTO marvels VALUES(6, "Green Goblin", 91, "Bad", "Male", 1.93, 174.63, "USA", 4, 4, 3, 4, 3, 3);
INSERT INTO marvels VALUES(7, "Magneto", 11, "Neutral", "Male", 1.88, 86.18, "Germany", 6, 3, 5, 4, 6, 4);
INSERT INTO marvels VALUES(8, "Thanos", 47, "Bad", "Male", 2.01, 446.79, "Titan", 6, 7, 7, 6, 6, 4);
INSERT INTO marvels VALUES(9, "Loki", 32, "Bad", "Male", 1.93, 238.14, "Jotunheim", 5, 5, 7, 6, 6, 3);
INSERT INTO marvels VALUES(10, "Doctor Doom", 19, "Bad", "Male", 2.01, 188.24, "Latveria", 6, 4, 5, 6, 6, 4);
INSERT INTO marvels VALUES(11, "Jean Grey", 8, "Good", "Female", 1.68, 52.16, "USA", 3, 2, 7, 7, 7, 4);
INSERT INTO marvels VALUES(12, "Rogue", 4, "Good", "Female", 1.73, 54.43, "USA", 7, 7, 7, 7, 7, 7);
INSERT INTO Marvels VALUES(13, "Storm", 2, "Good", "Female", 1.80, 66, "Kenya", 2, 2, 3, 2, 5, 4);
INSERT INTO Marvels VALUES(14, "Nightcrawler", 6, "Good", "Male", 1.75, 73, "Germany", 3, 2, 7, 2, 1, 3);
INSERT INTO Marvels VALUES(15, "Gambit", 7, "Good", "Male", 1.88, 81, "EUA", 2, 2, 2, 2, 2, 4);
INSERT INTO Marvels VALUES(16, "Captain America", 9, "Good", "Male", 1.88, 108, "EUA", 3, 3, 2, 3, 1, 6);
INSERT INTO Marvels VALUES(17, "Cyclops", 10, "Good", "Male", 1.90, 88, "EUA", 3, 2, 2, 2, 5, 4);
INSERT INTO Marvels VALUES(18, "Emma Frost", 12, "Neutral", "Female", 1.78, 65, "EUA", 4, 4, 2, 5, 5, 3);
INSERT INTO Marvels VALUES(19, "Kitty Pryde", 13, "Good", "Female", 1.68, 50, "EUA", 4, 2, 2, 3, 1, 5);
INSERT INTO Marvels VALUES(20, "Daredevil", 14, "Good", "Male", 1.83, 91, "EUA", 3, 3, 2, 2, 4, 5);
INSERT INTO Marvels VALUES(21, "Punisher", 50, "Neutral", "Male", 1.85, 91, "EUA", 3, 3, 2, 2, 1, 6);
INSERT INTO Marvels VALUES(22, "Silver Surfer", 33, "Good", "Male", 1.93, 102, "Zenn-La", 3, 7, 7, 6, 7, 2);
INSERT INTO Marvels VALUES(23, "Ghost Rider", 86, "Good", "Male", 1.88, 99, "EUA", 2, 4, 3, 5, 4, 2);
INSERT INTO Marvels VALUES(24, "Venon", 78, "Neutral", "Male", 1.90, 118, "EUA", 3, 4, 2, 6, 1, 4);
INSERT INTO Marvels VALUES(25, "Juggernaut", 76, "Neutral", "Male", 2.87, 862, "EUA", 2, 7, 2, 7, 1, 4);
INSERT INTO Marvels VALUES(26, "Professor X", 58, "Good", "Male", 1.83, 86, "EUA", 5, 2, 2, 2, 5, 3);

SELECT name,
    CASE
        WHEN popularity > 90 THEN "Most popular"
        WHEN popularity < 2  THEN "Less popular"
    END AS character_popularity
FROM Marvels
GROUP BY character_popularity;

SELECT alignment,COUNT(*) AS "quantity" FROM Marvels GROUP BY alignment;

SELECT name FROM Marvels WHERE popularity >= 80 OR popularity <= 1;

SELECT alignment, COUNT(*) FROM Marvels GROUP BY alignment;


# Consultas relacionales.
## Separar datos en tablas relacionadas
Hasta ahora, solo hemos trabajado con una tabla a la vez, y visto qué datos interesantes podemos seleccionar de esa tabla. Pero en realidad, la mayor parte del tiempo, tenemos nuestros datos distribuidos en varias tablas, y todas esas tablas están "relacionadas" unas a otras de alguna manera.
Por ejemplo, digamos que tenemos una tabla para registrar qué tan bien les va a los estudiantes en sus exámenes, e incluimos direcciones de correo electrónico en caso de que necesitemos enviar mensajes a los papás acerca de resbalones en las calificaciones:
nombre_estudiante	correo_estudiante	examen	calificacion
Peter Rabbit	peter@rabbit.com	Nutrición	95
Alice Wonderland	alice@wonderland.com	Nutrición	92
Peter Rabbit	peter@rabbit.com	Química	85
Alice Wonderland	alice@wonderland.com	Química	95
También podríamos tener una tabla para registrar qué libros lee cada estudiante:
nombre_estudiante	titulo_libro	autor_libro
Peter Rabbit	El cuento de la señora Tiggy-Winkle	Beatrix Potter
Peter Rabbit	Jabberwocky	Lewis Carroll
Alice Wonderland	La Caza del Snark	Lewis Carroll
Alice Wonderland	Jabberwocky	Lewis Carroll
También podríamos tener una tabla solo para información detallada del estudiante:
id	nombre_estudiante	apellido_estudiante	correo_estudiante	telefono	cumpleaños
1	Peter	Conejo	peter@rabbit.com	555-6666	2001-05-10
2	Alice	Wonderland	alice@wonderland.com	555-4444	2001-04-02
¿Que piensas acerca de estas tablas? ¿Las cambiarías de alguna manera?
Hay una cosa importante que hay que darse cuenta acerca de estas tablas: describen datos relacionales, como en: describen datos que se relacionan unos a otros. Cada una de estas tablas describe datos relacionados a un estudiante en particular, y muchas de las tablas replican los mismos datos. Cuando los mismos datos están replicados en múltples tablas, puede haber consecuencias interesantes.
Por ejemplo, ¿qué pasa si cambia el correo electrónico de un estudiante? ¿Qué tablas serían necesarias cambiar?
Necesitaríamos cambiar la tabla de información del estudiante, pero como también incluimos esos datos en la tabla de calificaciones, también tendríamos que encontrar cada renglón acerca de ese estudiante, y cambiar el correo electrónico ahí también.
A menudo es preferible estar seguros de que una columna de datos en particular esté almacenada en una sola ubicación, de modo que haya menos lugares que actualizar y menos riesgo de tener diferentes datos en diferentes lugares. Si hacemos eso, necesitamos asegurarnos de tener una manera de relacionar los datos en distintas tablas, a lo cual llegaremos más adelante.
Digamos que decidimos quitar el correo electrónico de la tabla de calificaciones, porque nos dimos cuenta de que es redundante con el correo electrónico en la tabla de detalles del estudiante. Esto es lo que tendríamos:
nombre_estudiante	examen	calificacion
Peter Rabbit	Nutrición	95
Alice Wonderland	Nutrición	92
Peter Rabbit	Química	85
Alice Wonderland	Química	95
¿Cómo podríamos averiguar el correo electrónico para cada estudiante? Podríamos encontrar el renglón en la tabla de información de estudiantes, al hacer coincidir los nombres. ¿Qué pasa si 2 estudiantes tienen el mismo nombre? (¿Sabías que en Bali cada persona solo tiene 1 de 4 nombres posibles?) No podemos depender del nombre para buscar un estudiante, y en serio, nunca debemos depender en algo como el nombre para identificar algo de manera única en una tabla.
Así que lo mejor por hacer es quitar nombre_estudiante y reemplazarlo con id_estudiante, ya que ese es un identificador único garantizado:
id_estudiante	examen	calificacion
1	Nutrición	95
2	Nutrición	92
1	Química	85
2	Química	95
Podríamos hacer el mismo cambio en nuestra tabla de libros, al usar id_estudiante en vez de nombre_estudiante:
id_estudiante	titulo_libro	autor_libro
1	El cuento de la señora Tiggy-Winkle	Beatrix Potter
1	Jabberwocky	Lewis Carroll
2	La Caza del Snark	Lewis Carroll
2	Jabberwocky	Lewis Carroll
Te das cuenta de que tenemos el título del libro y autor repetidos dos veces para Jabberwocky? Ese es otro signo de alerta de que podríamos separar nuestra tabla en múltiples tablas relacionadas, de modo que no tengamos que actualizar múltiples lugares si algo cambia acerca de un libro.
Podríamos tener una tabla solo acerca de libros:
id	titulo_libro	autor_libro
1	El cuento de la señora Tiggy-Winkle	Beatrix Potter
2	Jabberwocky	Lewis Carroll
3	La Caza del Snark	Lewis Carroll
Y después nuestra tabla libros_estudiantes se convierte en:
id_estudiante	id_libro
1	1
1	2
2	3
2	2
Ya sé, esta tabla no se ve tan legible como la anterior que tenía toda la información metida en cada renglón. Pero las tablas suelen no estar diseñadas para que las lea un humano, sino para que sean lo más fáciles de mantener y menos propensas a errores. En muchos casos, puede ser mejor separar la información en múltiples tablas relacionadas, de modo que haya menos datos redundantes y menos lugares que actualizar.
Es importante entender cómo usar SQL para lidiar con datos que han sido separados en múltiples tablas relacionadas, y traer de regreso los datos de varias tablas cuando sea necesario. Hacemos eso al usar un concepto llamado "join"s (uniones) y eso es lo que te mostraré a continuación.

# Unir tablas relacionadas con JOIN
CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    test TEXT,
    grade INTEGER);

INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Chemistry", 95);

#Seleccionar todo en la tabla students_grades
### SELECT * FROM student_grades;
Con esto podemos ver el id de los estudiantes, nombre de las pruebas y las calificaciones.

### Ahora queremos hacer una consulta que muestre el nombre del estudiante, su direccion de correo electronico y las calificaciones de las pruebas. Esa informacion esta en dos tablas diferentes.
### Eso quiere decir que tiene que combinar las tablas y existen algunas maneras de hacer eso en SQL.

## Ways to do that

### Combinacion cruzada.
/* cross join */
SELECT * FROM student_grades, students;

Combina las tablas, primero las columnas de student_grades seguidas de students.

Este metodo no nos sirve ya que no queremos que unas filas se combinen con otras, en el ejemplo anterior las 4 filas de student_grades se combina con cada una de la tabla students.

### Solo queremos que se unan cuando los IDs de los estudiantes coincidan y esto se logra con una COMBINACION INTERNA.
### Combinacion interna implicita.

/* implicit inner join */
SELECT * FROM student_grades, students
    WHERE student_grades.student_id = students.id;

Ahora si nos mostrara las calificaciones de las pruebas con relacion a los estudiantes.

Funciona para lo que necesitamos no es la mejor practica.

### Combinacion externa explicita (JOIN).

SELECT * FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id;

### Esto nos dara los mismos resultados que el query anterior, en otro orden pero son los mismos.
### Ahora que estan combinadas estas tablas se quieren reducir las columnas que estan mostrando.

SELECT first_name, last_name, email, test, grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id;
    
### Lo interesante es que ahora que se han hecho las combinaciones podemos seguir usando WHERE y GROUP BY. Por ejemplo, solo queremos las calificaciones mayores a 90.

SELECT first_name, last_name, email, test, grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;

### Que pasa si dos tablas contienen columnas con el mismo nombre
### Que pasaria si mis dos tablas contienen columnas con el mismo nombre?
Es decir que tengan el mismo nombre de columna pero con un significado diferente. Como en este caso, nuestra tabla de calificaciones tiene una columna grade pero mi tabla de estudiantes tambien tiene una columna grade que es para la calificacion global, eso significa que aqui cuando seleccionamos grade no va a saber de cual tabla debe mostrar la columna grade, por que hay una columna grade en las 2 tablas.
Asi que para ser muy seguros debemos poner prefijos en los nombres de nuestras columnas con los nombres de las tablas a las que pertenece.

SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;


## Mostrar nombre de usuarios con sus hobbies.
CREATE TABLE persons (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    age INTEGER);
    
INSERT INTO persons (name, age) VALUES ("Bobby McBobbyFace", 12);
INSERT INTO persons (name, age) VALUES ("Lucy BoBucie", 25);
INSERT INTO persons (name, age) VALUES ("Banana FoFanna", 14);
INSERT INTO persons (name, age) VALUES ("Shish Kabob", 20);
INSERT INTO persons (name, age) VALUES ("Fluffy Sparkles", 8);
INSERT INTO persons (name, age) VALUES ("Jonathan Sosa", 29);

CREATE table hobbies (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    person_id INTEGER,
    name TEXT);
    
INSERT INTO hobbies (person_id, name) VALUES (1, "drawing");
INSERT INTO hobbies (person_id, name) VALUES (1, "coding");
INSERT INTO hobbies (person_id, name) VALUES (2, "dancing");
INSERT INTO hobbies (person_id, name) VALUES (2, "coding");
INSERT INTO hobbies (person_id, name) VALUES (3, "skating");
INSERT INTO hobbies (person_id, name) VALUES (3, "rowing");
INSERT INTO hobbies (person_id, name) VALUES (3, "drawing");
INSERT INTO hobbies (person_id, name) VALUES (4, "coding");
INSERT INTO hobbies (person_id, name) VALUES (4, "dilly-dallying");
INSERT INTO hobbies (person_id, name) VALUES (4, "meowing");
INSERT INTO hobbies (person_id, name) VALUES (6, "videogames");

SELECT persons.name, hobbies.name
        FROM persons
        JOIN hobbies
        ON persons.id = hobbies.person_id;

# Unir tablas relacionadas con LEFT OUTER JOIN
BASICAMENTE PARA QUE MUESTRE TODA LA INFORMACION INCLUYENDO LOS QUE NO TIENEN DATOS (VALORES NULL).

CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    test TEXT,
    grade INTEGER);

INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Chemistry", 95);

CREATE TABLE student_projects (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    title TEXT);
    
INSERT INTO student_projects (student_id, title)
    VALUES (1, "Carrotapault");
   
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    JOIN student_projects
    ON students.id = student_projects.student_id;
    

Usando solo JOIN nos mostrara el proyecto de 1 estudiante, en este caso el proyecto de Peter que es Carrotapault, pero que paso con Alice? Bueno aqui no vemos a Alice porque INNER JOIN solo crea una fila cuando hay registros en las dos tablas, en el ejemplo no vemos una fila para Alice por que no hay ninguna fila en students_projects que tenga un student_id que corresponda con el de Alice.
En este caso queremos una lista amplia que contenga a cada estudiante y su proyecto y queremos a cada estudiante en esta lista aunque no tenga un proyecto todavia. Y aqui es donde usamos LEFT OUTER JOIN..

### LEFT OUTER JOIN.
/* outer join */ 
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    LEFT OUTER JOIN student_projects
    ON students.id = student_projects.student_id;
    
Ahora si nos motrara a Alice y su proyecto con el valor NULL.

### LEFT le indica a SQL que deberia de asegurarse de conservar todo lo que este en la tabla de la izquierda que es la que ponemos despues de FROM, es decir "students".
### OUTER le indica que debe mostrar las filas incluso si no tiene un registro correspondiente en la tabla de la derecha, que es la tabla "student_project".
### RIGHT OUTER JOIN hace exactamente lo opuesto. Se asegura de conservar todo lo de la tabla que esta a la derecha y lo une con lo de la izquierda.
### FULL OUTER JOIN. Combina las filas de los 2 lados (izq y der) y si no ecuentra un registro correspondiente de algun lado, mostrara NULL.


# Unir tablas a sí mismas con SELF-JOIN
CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT,
    buddy_id INTEGER);

INSERT INTO students 
    VALUES (1, "Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24", 2);
INSERT INTO students 
    VALUES (2, "Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04", 1);
INSERT INTO students 
    VALUES (3, "Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10", 4);
INSERT INTO students 
    VALUES (4, "Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24", 3);
    
SELECT id, first_name, last_name, buddy_id FROM students;

/* self join */
SELECT students.first_name, students.last_name, buddies.email as buddy_email
    FROM students
    JOIN students buddies  <--- Se le asigna un alias "buddies"
    ON students.buddy_id = buddies.id;

### ON debe revisar que el id del amigo de la tabla students coincida con el id de la segunda tabla, la tabla buddies


# Combinar multiples JOINs (JOIN y SELF JOIN).
Usa un SELECT con un JOIN para mostrar los nombres de cada par de amigos, con base en los datos en la tabla friends.

CREATE TABLE persons (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    fullname TEXT,
    age INTEGER);
    
INSERT INTO persons (fullname, age) VALUES ("Bobby McBobbyFace", "12");
INSERT INTO persons (fullname, age) VALUES ("Lucy BoBucie", "25");
INSERT INTO persons (fullname, age) VALUES ("Banana FoFanna", "14");
INSERT INTO persons (fullname, age) VALUES ("Shish Kabob", "20");
INSERT INTO persons (fullname, age) VALUES ("Fluffy Sparkles", "8");

CREATE table hobbies (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    person_id INTEGER,
    name TEXT);
    
INSERT INTO hobbies (person_id, name) VALUES (1, "drawing");
INSERT INTO hobbies (person_id, name) VALUES (1, "coding");
INSERT INTO hobbies (person_id, name) VALUES (2, "dancing");
INSERT INTO hobbies (person_id, name) VALUES (2, "coding");
INSERT INTO hobbies (person_id, name) VALUES (3, "skating");
INSERT INTO hobbies (person_id, name) VALUES (3, "rowing");
INSERT INTO hobbies (person_id, name) VALUES (3, "drawing");
INSERT INTO hobbies (person_id, name) VALUES (4, "coding");
INSERT INTO hobbies (person_id, name) VALUES (4, "dilly-dallying");
INSERT INTO hobbies (person_id, name) VALUES (4, "meowing");

CREATE table friends (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    person1_id INTEGER,
    person2_id INTEGER);

INSERT INTO friends (person1_id, person2_id)
    VALUES (1, 4);
INSERT INTO friends (person1_id, person2_id)
    VALUES (2, 3);

## Para hacer un uso multiple de JOINS, debemos:

### SELECT fullname FROM friends
### JOIN persons;

## Esto nos mostrara el nombre de todos por cada valor en la tabla friends, la tabla friends tiene 2 registros, por lo tanto el resultado seran 10 registros de fullname.
## Ahora crearemos la otra columna que seran los amigos de las personas

### SELECT fullname, fullname FROM friends
###    JOIN persons
###    JOIN persons;

## Aqui nos dira que la tabla es ambigua ya que estamos buscando los mismos valores en la misma tabla. Debemos asignar un alias:

### SELECT a.fullname, b.fullname FROM friends
###    JOIN persons a
###    JOIN persons b;

## Ahora debemos usar el ON para hacer la relacion, en este caso primero vamos a buscar el id en la tabla donde queremos hacer la relacion (friends) para poder igualarlo con la tabla (persons) que contiene el valor de ese mismo id, osea
## buscar el id que contiene el valor en la tabla que define el valor del mismo id. 

## SELECT a.fullname, b.fullname FROM friends
##     JOIN persons a
##     ON friends.person1_id = a.id
##     JOIN persons b
##     ON friends.person2_id = b.id;





# SELECT, INSERT UPDATE AND DELETE
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT);
    
CREATE TABLE diary_logs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    date TEXT,
    content TEXT
    );
    
/* After user submitted their new diary log */
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-01",
    "I had a horrible fight with OhNoesGuy and I buried my woes in 3 pounds of dark chocolate.");
    
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-02",
    "We made up and now we're best friends forever and we celebrated with a tub of ice cream.");

SELECT * FROM diary_logs;

UPDATE diary_logs SET content = "I had a horrible fight with OhNoesGuy" WHERE id = 1;

SELECT * FROM diary_logs;

DELETE FROM diary_logs WHERE id = 1;

### RECUERDA! Siempre debes hacer un SELECT despues de un UPDATE para ver los cambios.

# Alterar tablas despues de crearlas.
/* What we used to originally create the table */
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT);
    
CREATE TABLE diary_logs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    date TEXT,
    content TEXT
    );
    
/* After user submits a diary log */
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-02",
    "OhNoesGuy and I made up and now we're best friends forever and we celebrated with a tub of ice cream.");
    
ALTER TABLE diary_logs ADD emotion TEXT default "unknown";  <-- Se pone ALTER TABLE <tablename> ADD <column_name> <datatype> default <whatever>    donde whatever sera el valor que tendran los demas registros en esa columna.

INSERT INTO diary_logs (user_id, date, content, emotion) VALUES (1, "2015-04-03",
    "We went to Disneyland!", "happy");
    
SELECT * FROM diary_logs;

DROP TABLE diary_logs;
