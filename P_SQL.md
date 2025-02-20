# Programming SQL

1. 

<br>

## Crear una función

```sql
DELIMITER $$ 
CREATE FUNCTION suma(a int, b int)
RETURNS int
BEGIN
	RETURN (a+b);
END 
$$
DELIMITER ;
```

<br>

## Usar función

```sql
SELECT suma(2,4);
```

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
DELIMITER ;
```

## Uso de Consultas dentro de funciones

* Declarar variable con el resultado de la consulta

```sql
BEGIN
	DECLARE resultado int;

	SET resultado = (
        SELECT COUNT(*) FROM results r
        WHERE r.Resultado = equipo
        ;
    );
    
	RETURN resultado;
END 
```

* Declarar valor de la variable dentro de la consulta (con INTO)

```sql
BEGIN
    DECLARE resultado int;

	SELECT COUNT(*) INTO resultado FROM results r
	WHERE r.Resultado = equipo
	;
	
    RETURN resultado;
END
```

# If Else