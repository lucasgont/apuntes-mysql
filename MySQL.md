# MySQL

## referencias

https://www.youtube.com/watch?v=Cz3WcZLRaWc

COMANDOS MYSQL

Comentarios:	
	Linea -- comment
	Multilines /* comment */

-- data types

boolean: true, false
varchar text: 'string'
date: 'YYYY-MM-DD'
time: 'HH:MM:SS'

--- Crear tablas ---

```sql
CREATE TABLE Pasajeros(
	id INT PRIMARY KEY AUTO_INCREMENT,
	nombre VARCHAR(255),
	direccion TEXT,
	n_telefono VARCHAR(255)
);

CREATE TABLE Usuarios(
	id INT PRIMARY KEY AUTO_INCREMENT,
	nombre VARCHAR(255),
	email VARCHAR(255) NOT NULL UNIQUE,
	direccion TEXT,
	esMayorEdad BOOLEAN,
	fechaNacimiento DATE
);
```

-- relaciones entre tablas

```sql
CREATE TABLE Vuelos(
	id INT PRIMARY KEY AUTO_INCREMENT,
	nombre_piloto VARCHAR(255),
	apellido_piloto VARCHAR(255),
	id_r INT,
	id_a INT,
	id_h INT,
	FOREIGN KEY (id_r) REFERENCES Rutas(id),
	FOREIGN KEY (id_a) REFERENCES Aviones(id),
	FOREIGN KEY (id_h) REFERENCES Horarios(id)
);

```

-- Insertar valores ---

```sql
INSERT INTO Asignaturas (nombre, descripcion, creditos, horario)
VALUES
	('Lenguaje de Marcas', 'HTML y CSS', 6.25, '09:00:00'),
	('Programación', 'PSeint y C++', 6.6, '10:00:00'),
	('Bases de Datos', 'Entidad-Relación', 6.95, '11:00:00')
;
```

--- Modificar columnas ----

ALTER TABLE Table_name ADD color TEXT;

ALTER TABLE Table_name DROP color;

ALTER TABLE Table_name MODIFY COLUMN color INT;



---- Consultas SIMPLES ----

SELECT properties 
FROM Table_Name;

// Filtros

SELECT prop1, prop2 
FROM Table_Name 
WHERE (prop1 = 1) -> AND, OR, NOT (solo un igual para comparar);

// Nuevas columnas sobre la marcha

SELECT id, prop1 * prop2 AS dinero_total
FROM Table_Name;

?? Clientes.productos? -> https://chatgpt.com/c/674ee7c2-d5c8-800f-b631-56f640ae7364
?? productos p

// Orden
SELECT prop1, prop2
FROM Table_Name
WHERE ...
ORDER BY price DESC

// Orden en caso de empate (al empatar se ordena segun el coste DESC)
ORDER BY price DESC, cost DESC

// Limitar resultados
LIMIT 100

// Mostrar el que más
ORDER BY price DESC
LIMIT 1

---------------

SELECT * FROM Table_Name

---------------

Funciones de agregación (no se pueden usar funciones de agregación junto con ???datos de registro???)

SELECT avg(creditos), max(creditos)
FROM Clientes c;


* Agrupar con funciones de agregacion:

SELECT pais, avg(creditos), max(creditos)
FROM Clientes c
GROUP BY pais;


[hacer tabla:]
avg() - Media
max() - Maximo
min() - Minimo
sum() - Suma
count() - Contador


* Dar nombre a cada consulta:

´´´´mysql
``´
SELECT count(*) AS nEmpleados FROM empleado e;

*
YEAR()
look for others...


* Termina en 'S.A.'
WHERE name LIKE '%S.A.' 

* Incluye 'director'
WHERE puesto LIKE '%director%' 

%: Representa cero o más caracteres.
_: Representa un solo carácter.

------

- se pone el nombre de las tablas en singular


---------

* Filtrar datos después de las funciones de agregacion (siempre que los datos hayan sido agrupados. HAVING realiza la comparación dentro de cada bloque agrupado (previamente))

GROUP BY ...
HAVING nCLIENTES < 100;

------ CONSULTAS COMPLEJAS?? ---------

* Unión de tablas (Juntar tablas mediante Foreign Keys)

select * from empleado e
inner join oficina o ON o.codigo_oficina = e.codigo_oficina

INNER JOIN: Combina filas de dos o más tablas basándose en una condición de
relación.

LEFT JOIN: Devuelve todas las filas de la tabla izquierda y las filas coincidentes de la
tabla derecha.

RIGHT JOIN: Devuelve todas las filas de la tabla derecha y las filas coincidentes de la
tabla izquierda.

Nota: Siempre identificar columnas con su respectiva tabla:

SELECT c.nombre_cliente, o.linea_direccion1, o.linea_direccion2 FROM cliente c

-> hay veces que se pueden unir por ids distintos al primary key segun cada consulta

---

para ver si es null
IS NULL 
IS NOT NULL


------------

subconsultas

a) Un única dato: Usar numero de una consulta en otra

b) Una única columna (WHERE [property] IN (Columna o ('A', 'B', 'C')) )

c) Una tabla (JOINS)


--------

array of options

Where property in ('PS4', 'Nintendo')


_______________

NUEVO MYSQL

UPDATE [table]
SET [propiedad] = [valor]
WHERE [condicion]

DELETE FROM [table]
WHERE [condicion]
