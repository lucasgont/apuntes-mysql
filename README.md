<img src="./images/mysql.png" alt="MySQL" style="width: 300px; display: block; margin: auto;">

# MySQL

- Creado: 2024-11-19
- Última edición: 2025-02-26

### Índice

INTRODUCCIÓN

1. [Referencias](#referencias)

2. [Configurar entorno](#configurar-entorno)

3. [Localhost](#localhost)

PRIMER EXAMEN

4. [Comentarios](#comentarios)

5. [Tipos de datos](#tipos-de-datos)

6. [Creación de tablas](#creación-de-tablas)

7. [Relaciones entre tablas](#relaciones-entre-tablas)

8. [Inserción de valores](#inserción-de-valores)

9. [Modificación de tablas](#modificación-de-tablas)

10. [Consultas básicas](#consultas-básicas)

SEGUNDO EXAMEN

. [Modificación de valores de una tabla](#modificación-de-valores-de-una-tabla)

. [PL/MySQL](#plmysql)

<br>

# Referencias

https://www.youtube.com/watch?v=Cz3WcZLRaWc

<br>

# Configurar entorno

Usaremos DBeaver y Docker Desktop. DBeaver es un sistema de gestión de bases de datos (SGBB) y trabajaremos en un servidor simulado dentro de una máquina virtual (Docker).
<div style="display: flex">

<img src="./images/docker.png" alt="MySQL" style="width: 200px; display: inline; margin: auto;">

<img src="./images/dbeaver.png" alt="MySQL" style="width: 180px; display: inline; margin: auto; background-color:white; border-radius: 20px; padding: 5px 15px;">

</div>

## Pasos (en Windows)

1. Descargar DBeaver y Docker Desktop

2. En el terminal, instala WSL (Windows Subsystem for Linux)

`wsl --install`

3. Con Docker abierto ejecuta el siguiente comando para descargar la imagen de MySQL

`docker pull mysql`

4. Crear y ejecutar un contenedor de MySQL

`docker run --name mysql_container -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=testdb -p 3306:3306 -d mysql:latest`

Esto creará y ejecutará un contenedor de MySQL con:
* Nombre: mysql_container
* Contraseña de root: root
* Base de datos: testdb
* Puerto: 3306

5. Conecta DBeaver a la Base de Datos en Docker

	1. Abre DBeaver

	2. Crea una nueva conexión (Archivo → Nueva conexión)

	3. Selecciona el tipo de base de datos: MySQL

	4. Configura los datos de conexión:
	* Servidor: localhost
	* Puerto: 3306
	* Usuario: root
	* Contraseña: root
	* Base de datos: testdb
	* Haz clic en "Probar conexión" y luego en "Finalizar"

6. Verificar la conexión. Ejecuta en DBeaver

`SHOW DATABASES;`

<br>

# Localhost

Basta con encender el contenedor en Docker Desktop

<br>

# Observaciones importantes

* Nombrar el script según la base de datos para identificarlo de manera más fácil.

* Los bloques de código se separan por punto y com `;`.

* **Primer pergamino**: Ejecuta el bloque seleccionado con el cursor (donde se encuentra el cursor en el script). Shortcut: `CTRL + ENTER` con el ratón seleccionado en el bloque.

* **Tercer pergamino**: Ejecuta todo el script.

<br>

# Comentarios

Línea `-- comment`

Varias líneas `/* comment */`

<br>

# Tipos de datos

| Tipo de Dato   | Descripción                                             | Ejemplo                     |
|----------------|---------------------------------------------------------|-----------------------------|
| **BOOLEAN**    | Almacena valores `true` o `false`                       | `TRUE`, `FALSE`             |
| **VARCHAR()**  | Cadena de longitud variable. Caracteres entre [0, 255]  | `'string'`                  |
| **TEXT**       | Cadena de longitud variable (más larga)                 | `'string largo'`            |
| **INT**        | Número entero                                           | `42`                        |
| **FLOAT**      | Número de punto flotante                                | `3.14`                      |
| **DOUBLE**     | Número de punto flotante de doble precisión             | `3.14159265358979`          |
| **DATE**       | Fecha en formato `YYYY-MM-DD`                           | `'2025-02-26'`              |
| **TIME**       | Hora en formato `HH:MM:SS`                              | `'14:30:00'`                |
| **DATETIME**   | Combina fecha y hora                                    | `'2025-02-26 14:30:00'`     |

<br>

# Creación de tablas

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
`PRIMARY KEY`: Cada tabla necesita un atributo que se un identificador único.

`AUTO_INCREMENT`: Incremento automático del atributo al insertar nuevos valores.

`NOT NULL`: Asegura que el valor no sea nulo.

`UNIQUE`: Asegura que el valor insertado sea único al compararlo con otras filas.

<br>

# Relaciones entre tablas

Se declaran los tipos de datos de cada identificador y luego se hace referencia a la tabla que pertencen.

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

<br>

# Inserción de valores

```sql
INSERT INTO Asignaturas (nombre, descripcion, creditos, horario)
VALUES
	('Lenguaje de Marcas', 'HTML y CSS', 6.25, '09:00:00'),
	('Programación', 'PSeint y C++', 6.6, '10:00:00'),
	('Bases de Datos', 'Entidad-Relación', 6.95, '11:00:00')
;
```

<br>

# Modificación de tablas

Añadir una nueva columna (atributo y su tipo de dato)

```sql
ALTER TABLE Coches ADD color TEXT;
```

<br>

Eliminar una columna (atributo)

```sql
ALTER TABLE Coches DROP color;
```

<br>

Modificar el tipo de datos de una columna (atributo)

```sql
ALTER TABLE Coches MODIFY COLUMN color INT;
```

<br>

# Consultas Básicas

Seleccionar propiedades específicas de la tabla

```sql
SELECT id, color, precio FROM Coches;
```

<br>

Seleccionar todas las propiedades de la tabla

```sql
SELECT * FROM Coches c;
```

<br>

Cambiar el nombre de una propiedad sobre la marcha (`AS`)

```sql
SELECT id, (pvp - coste) AS beneficio FROM Coches;
```

## Filtrar

```sql
SELECT prop1, prop2 FROM Table_Name 
WHERE (prop1 = 1)
;
```

* Se puede usar: `AND, OR, NOT`

* Solo se usa un igual para comparar

<br>

Filtrar por texto terminado en 'S.A.':

```sql
WHERE name LIKE '%S.A.' 
```

<br>

Filtrar por texto que incluye la palabra 'director':

```sql
WHERE puesto LIKE '%director%' 
```

`%`: Representa cero o más caracteres.

`_`: Representa un solo carácter.

<br>

Comprobar si un valor no es `NULL`:

```sql
WHERE puesto IS NOT NULL 
```

* Se puede usar: `IS NULL`, `IS NOT NULL`

<br>

Array de opciones:

```sql
WHERE plataforma IN ('PS4', 'Nintendo', 'XBOX')
```

## Ordenar

```sql
SELECT id, color, precio FROM Coches
ORDER BY precio DESC
;
```

* Se puede usar: `DESC` o `ASC`

<br>

Ordenar en caso de empate (al empatar en precio se ordena según el coste)

```sql
ORDER BY precio DESC, coste DESC
```

## Limitar 

Limitar en 100 resultados 

```sql
SELECT * FROM Coches
LIMIT 100
;
```

<br>

Mostrar el que más (en este caso el más caro)

```sql
SELECT * FROM Coches
ORDER BY price DESC
LIMIT 1
;
```

## Funciones de agregación y Agrupación 

| Función    | Descripción                                   |
|------------|-----------------------------------------------|
| **AVG()**  | Calcula el promedio de un conjunto de valores |
| **MAX()**  | Devuelve el valor máximo en un conjunto       |
| **MIN()**  | Devuelve el valor mínimo en un conjunto       |
| **SUM()**  | Calcula la suma total de un conjunto          |
| **COUNT()**| Cuenta el número de registros                 |

Ejemplo:

```sql
SELECT avg(creditos), max(creditos) FROM Clientes;
```

<br>

Agrupar con funciones de agregación:

```sql
SELECT pais, avg(creditos) FROM Clientes c
GROUP BY pais
;
```

<br>

No se pueden usar funciones de agregación `avg(creditos), max(creditos)` junto con datos de registro `pais`. Pero sí si se agrupan antes por estos. 

**Ejemplo de ERROR:**

```sql
SELECT pais, avg(creditos), max(creditos) FROM Clientes c; -- Error!
```

**Ejemplo CORRECTO:**

```sql
SELECT pais, avg(creditos), max(creditos) FROM Clientes c
GROUP BY pais
;
```

<br>

También se puede cambiar el nombre de la columna con `AS`

```sql
SELECT count(*) AS nEmpleados FROM Empleados e;
```

## Filtrar después de Agrupar

Filtrar los datos después de las funciones de agregación, siempre que los datos hayan sido agrupados. `HAVING` realiza la comparación dentro de cada bloque (agrupado previamente):

```sql
SELECT tienda, avg(*) FROM Clientes
GROUP BY tienda
HAVING count(*) AS nClientes < 100;
```

Consulta: El promedio de clientes de todas las tiendas que hayan tenido menos de 100 clientes

<br>

# Consultas Complejas

Para diferencia de que tabla hace referencia un atributo se le añade el sufijo identificador al hacer referencia de una tabla:

```sql
SELECT c.id, c.color, c.precio FROM Coches c;
```

## Unión de tablas

| Tipo de Unión   | Descripción                                                                                                                          |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **INNER JOIN**  | Combina filas de dos o más tablas basándose en una condición de relación. Solo devuelve las coincidencias.                           |
| **LEFT JOIN**   | Devuelve todas las filas de la tabla izquierda y solo las coincidencias de la tabla derecha. Si no hay coincidencia, muestra `NULL`. |
| **RIGHT JOIN**  | Devuelve todas las filas de la tabla derecha y solo las coincidencias de la tabla izquierda. Si no hay coincidencia, muestra `NULL`. |

<br>

Juntar tablas mediante `FOREING KEYS`:

```sql
SELECT * FROM Empleados e
INNER JOIN Oficinas o ON o.codigo_oficina = e.codigo_oficina
;
```

<br>

Hay veces que se pueden unir por IDs distintos al PRIMARY KEY según la consulta:

```sql
SELECT e.nombre, p.nombre_proyecto FROM Empleados e
INNER JOIN Proyectos p ON e.email = p.empleado_email
;
```

## Subconsultas

| Tipo de Subconsulta | Descripción | Ejemplo |
|------|------------|---------------|
| **Devuelve un único dato** | Se usa para obtener un valor único (como un número o una fecha) y usarlo en otra consulta. | ```SELECT nombre FROM Empleados WHERE salario = (SELECT max(salario) FROM Empleados); ``` |
| **Devuelve una única columna** | Se usa con `IN` para filtrar resultados basados en una lista obtenida de otra consulta. | ```SELECT nombre FROM Empleados WHERE departamento_id IN (SELECT id FROM Departamentos WHERE nombre = 'Ventas');``` |
| **Devuelve una tabla** | Se usa dentro de un `JOIN` para combinar datos de una consulta con otra tabla. | Mirar abajo |

<br>

Uso de una subconsulta que devuelve una tabla:

```sql
SELECT e.nombre, subquery.total_ventas FROM Empleados e 
JOIN (
	SELECT v.empleado_id, sum(v.monto) AS total_ventas FROM Ventas v 
	GROUP BY v.empleado_id
) subquery ON e.id = subquery.empleado_id
;
```

<br>

# Modificación de valores de una tabla

Actualizar valores (según condición):

```sql
UPDATE Coches c
SET c.color = 'rojo'
WHERE c.marca = 'Lamborghini'
;
```

<br>

Borrar valores (según condición):

```sql
DELETE FROM Servicios s
WHERE s.valoracion == 1
;
```

<br>

# PL/MySQL

PL significa Procedural Language (Lenguaje de Programación Procedimental). En el contexto de bases de datos, este término se utiliza para describir lenguajes de programación que se pueden usar para escribir funciones y procedimientos que se ejecutan en el servidor de bases de datos.

## Crear una función

```sql
DELIMITER $$ 
CREATE FUNCTION suma(a int, b int)
RETURNS int
BEGIN
	-- Cuerpo de la función
	RETURN (a+b);
END 
$$
DELIMITER;
```

<br>

## Usar función

```sql
SELECT suma(2, 4);
```

<br>

## Variables

```sql
DELIMITER $$ 
CREATE FUNCTION suma(a int, b int)
RETURNS int
BEGIN
	-- Variables
	DECLARE resultado int;
	SET resultado = (a+b);
	RETURN resultado;
END 
$$
DELIMITER;
```

<br>

## Uso de consultas dentro de funciones

Asignar valor de la variable con el resultado de una consulta:

```sql
BEGIN
	DECLARE resultado int;

	SET resultado = (
        SELECT COUNT(*) FROM Results r
        WHERE r.Resultado = equipo
    );
    
	RETURN resultado;
END 
```
<br>

Asignar valor de la variable dentro de una consulta (con `INTO`):

```sql
BEGIN
    DECLARE resultado int;

	SELECT count(*) INTO resultado FROM Results r
	WHERE r.Resultado = equipo
	;
	
    RETURN resultado;
END
```

## If Else

```sql
BEGIN
    DECLARE resultado varchar(255);

	IF (horasOcupadas = 0) THEN
		SET resultado = 'Disponible';
	ELSE
		SET resultado = 'Ocupado';
	END IF;
END
```

<br>

## Procedimientos

Los procedimientos almacenados son similares a las funciones, pero pueden realizar operaciones más complejas, incluidas las operaciones de manipulación de datos (INSERT, UPDATE, DELETE).

```sql
-- Cuerpo del procedimiento

```
