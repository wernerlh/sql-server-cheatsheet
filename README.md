# SQL Server Cheat Sheet Completo

## Índice de Contenidos
1. [Conceptos Básicos](#conceptos-básicos)
2. [Tipos de Datos](#tipos-de-datos)
3. [Operadores](#operadores)
4. [Consultas SELECT](#consultas-select)
5. [Funciones de Agregación](#funciones-de-agregación)
6. [Joins](#joins)
7. [Manipulación de Datos (DML)](#manipulación-de-datos-dml)
8. [Definición de Datos (DDL)](#definición-de-datos-ddl)
9. [Índices](#índices)
10. [Vistas](#vistas)
11. [Procedimientos Almacenados](#procedimientos-almacenados)
12. [Funciones](#funciones)
13. [Triggers](#triggers)
14. [Transacciones](#transacciones)
15. [Funciones de Fecha y Hora](#funciones-de-fecha-y-hora)
16. [Funciones de Cadena](#funciones-de-cadena)
17. [Funciones Matemáticas](#funciones-matemáticas)
18. [Consultas Avanzadas](#consultas-avanzadas)
19. [Control de Flujo](#control-de-flujo)
20. [Optimización y Rendimiento](#optimización-y-rendimiento)
21. [Seguridad](#seguridad)
22. [Mantenimiento](#mantenimiento)

## Conceptos Básicos

### Comandos Básicos
```sql
-- Comentario de una línea
/* Comentario 
   de múltiples líneas */

USE NombreBaseDatos;  -- Selecciona una base de datos
GO                    -- Separador de lotes
```

### Orden de Ejecución Lógica
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY

## Tipos de Datos

### Numéricos Exactos
- `bit`: 0, 1, o NULL
- `tinyint`: 0 a 255 (1 byte)
- `smallint`: -32,768 a 32,767 (2 bytes)
- `int`: -2^31 a 2^31-1 (4 bytes)
- `bigint`: -2^63 a 2^63-1 (8 bytes)
- `decimal(p,s)` / `numeric(p,s)`: números con precisión fija (p=precisión total, s=decimales)
- `money`: -922,337,203,685,477.5808 a 922,337,203,685,477.5807 (8 bytes)
- `smallmoney`: -214,748.3648 a 214,748.3647 (4 bytes)

### Numéricos Aproximados
- `float(n)`: -1.79E+308 a 1.79E+308 (4 u 8 bytes)
- `real`: -3.40E+38 a 3.40E+38 (4 bytes)

### Fecha y Hora
- `date`: YYYY-MM-DD (3 bytes)
- `time`: HH:MM:SS.nnnnnnn (3-5 bytes)
- `datetime`: 1753-01-01 a 9999-12-31 (8 bytes)
- `datetime2`: 0001-01-01 a 9999-12-31 (6-8 bytes)
- `datetimeoffset`: con zona horaria (8-10 bytes)
- `smalldatetime`: 1900-01-01 a 2079-06-06 (4 bytes)

### Cadenas
- `char(n)`: Longitud fija (1-8000 caracteres)
- `varchar(n)`: Longitud variable (1-8000 caracteres)
- `varchar(max)`: Hasta 2GB
- `nchar(n)`: Unicode de longitud fija (1-4000 caracteres)
- `nvarchar(n)`: Unicode de longitud variable (1-4000 caracteres)
- `nvarchar(max)`: Unicode hasta 2GB
- `text`: Hasta 2GB (obsoleto)
- `ntext`: Unicode hasta 2GB (obsoleto)

### Binarios
- `binary(n)`: Longitud fija (1-8000 bytes)
- `varbinary(n)`: Longitud variable (1-8000 bytes)
- `varbinary(max)`: Hasta 2GB
- `image`: Hasta 2GB (obsoleto)

### Otros
- `xml`: Almacena datos XML
- `uniqueidentifier`: GUID de 16 bytes
- `sql_variant`: Almacena cualquier tipo de datos SQL Server

## Operadores

### Comparación
- `=`: Igual
- `<>` o `!=`: Diferente
- `>`: Mayor que
- `<`: Menor que
- `>=`: Mayor o igual que
- `<=`: Menor o igual que
- `!<`: No menor que
- `!>`: No mayor que

### Lógicos
- `AND`: Y lógico
- `OR`: O lógico
- `NOT`: Negación
- `EXISTS`: Verifica si una subconsulta devuelve resultados
- `IN`: Verifica si un valor está en un conjunto
- `SOME` / `ANY`: Verifica si algún valor cumple la condición
- `ALL`: Verifica si todos los valores cumplen la condición
- `BETWEEN`: Verifica si un valor está en un rango (inclusive)

### Especiales
- `LIKE`: Búsqueda de patrones
  - `%`: Cualquier cadena de caracteres
  - `_`: Un solo carácter
  - `[]`: Cualquier carácter único dentro de los corchetes
  - `[^]`: Cualquier carácter único que no esté dentro de los corchetes
- `IS NULL` / `IS NOT NULL`: Verifica si un valor es o no es NULL
- `ISNULL(expr, valor)`: Reemplaza NULL con un valor alternativo

### Aritméticos
- `+`: Suma (o concatenación para cadenas)
- `-`: Resta
- `*`: Multiplicación
- `/`: División
- `%`: Módulo (resto de la división)

## Consultas SELECT

### Estructura Básica
```sql
SELECT [DISTINCT] columnas | *
FROM tabla
[WHERE condición]
[GROUP BY columnas]
[HAVING condición_grupo]
[ORDER BY columnas [ASC|DESC]]
[OFFSET n ROWS FETCH NEXT m ROWS ONLY];
```

### TOP
```sql
-- Devuelve las primeras n filas
SELECT TOP n columnas FROM tabla;

-- Devuelve un porcentaje de filas
SELECT TOP n PERCENT columnas FROM tabla;

-- Con empates (incluye todas las filas con valores idénticos al último)
SELECT TOP n WITH TIES columnas FROM tabla ORDER BY columna;
```

### DISTINCT
```sql
-- Elimina duplicados
SELECT DISTINCT columna FROM tabla;

-- Con múltiples columnas
SELECT DISTINCT columna1, columna2 FROM tabla;
```

### INTO
```sql
-- Crea una nueva tabla con los resultados
SELECT columnas INTO nueva_tabla FROM tabla WHERE condición;
```

### Alias
```sql
-- Alias para columnas
SELECT columna AS alias FROM tabla;

-- Alias para tablas
SELECT t.columna FROM tabla AS t;
```

### Subconsultas
```sql
-- Subconsulta en el SELECT
SELECT columna1, (SELECT MAX(columna) FROM tabla2) FROM tabla1;

-- Subconsulta en el FROM
SELECT * FROM (SELECT columna1, columna2 FROM tabla) AS t;

-- Subconsulta en el WHERE
SELECT * FROM tabla WHERE columna IN (SELECT columna FROM tabla2);
```

### UNION / INTERSECT / EXCEPT
```sql
-- UNION: Combina resultados y elimina duplicados
SELECT columna FROM tabla1 UNION SELECT columna FROM tabla2;

-- UNION ALL: Combina resultados sin eliminar duplicados
SELECT columna FROM tabla1 UNION ALL SELECT columna FROM tabla2;

-- INTERSECT: Devuelve filas comunes
SELECT columna FROM tabla1 INTERSECT SELECT columna FROM tabla2;

-- EXCEPT: Devuelve filas de la primera consulta que no están en la segunda
SELECT columna FROM tabla1 EXCEPT SELECT columna FROM tabla2;
```

### PIVOT / UNPIVOT
```sql
-- PIVOT: Convierte filas en columnas
SELECT *
FROM (SELECT categoria, producto, cantidad FROM ventas) AS s
PIVOT (
    SUM(cantidad)
    FOR producto IN ([Producto1], [Producto2], [Producto3])
) AS p;

-- UNPIVOT: Convierte columnas en filas
SELECT categoria, producto, cantidad
FROM tabla_pivotada
UNPIVOT (
    cantidad
    FOR producto IN (Producto1, Producto2, Producto3)
) AS u;
```

## Funciones de Agregación

### Básicas
```sql
SELECT 
    COUNT(*),                    -- Cuenta todas las filas
    COUNT(columna),              -- Cuenta filas no NULL
    COUNT(DISTINCT columna),     -- Cuenta valores únicos
    SUM(columna),                -- Suma
    AVG(columna),                -- Promedio
    MIN(columna),                -- Valor mínimo
    MAX(columna),                -- Valor máximo
    STDEV(columna),              -- Desviación estándar
    STDEVP(columna),             -- Desviación estándar poblacional
    VAR(columna),                -- Varianza
    VARP(columna)                -- Varianza poblacional
FROM tabla;
```

### GROUP BY
```sql
-- Agrupación básica
SELECT columna1, COUNT(*) AS total
FROM tabla
GROUP BY columna1;

-- Agrupación por múltiples columnas
SELECT columna1, columna2, SUM(columna3) AS total
FROM tabla
GROUP BY columna1, columna2;

-- Con HAVING (filtro después de agrupar)
SELECT columna1, COUNT(*) AS total
FROM tabla
GROUP BY columna1
HAVING COUNT(*) > 5;

-- Con ROLLUP (genera subtotales y gran total)
SELECT columna1, columna2, SUM(ventas) AS total
FROM tabla
GROUP BY columna1, columna2 WITH ROLLUP;

-- Con CUBE (genera todas las combinaciones de subtotales)
SELECT columna1, columna2, SUM(ventas) AS total
FROM tabla
GROUP BY columna1, columna2 WITH CUBE;

-- Con GROUPING SETS (agrupaciones específicas)
SELECT columna1, columna2, SUM(ventas) AS total
FROM tabla
GROUP BY GROUPING SETS (
    (columna1, columna2),
    (columna1),
    (columna2),
    ()
);
```

### Funciones de Ventana
```sql
-- ROW_NUMBER: Número secuencial para cada fila
SELECT ROW_NUMBER() OVER (ORDER BY columna) AS fila_num, columna FROM tabla;

-- RANK: Asigna el mismo rango a valores iguales, dejando huecos
SELECT RANK() OVER (ORDER BY columna) AS rango, columna FROM tabla;

-- DENSE_RANK: Asigna el mismo rango a valores iguales, sin huecos
SELECT DENSE_RANK() OVER (ORDER BY columna) AS rango, columna FROM tabla;

-- NTILE: Divide en n grupos iguales
SELECT NTILE(4) OVER (ORDER BY columna) AS cuartil, columna FROM tabla;

-- LAG: Accede a una fila previa
SELECT columna, LAG(columna, 1) OVER (ORDER BY fecha) AS valor_anterior FROM tabla;

-- LEAD: Accede a una fila posterior
SELECT columna, LEAD(columna, 1) OVER (ORDER BY fecha) AS valor_siguiente FROM tabla;

-- FIRST_VALUE: Primer valor en la ventana
SELECT FIRST_VALUE(columna) OVER (PARTITION BY categoria ORDER BY fecha) FROM tabla;

-- LAST_VALUE: Último valor en la ventana
SELECT LAST_VALUE(columna) OVER (
    PARTITION BY categoria 
    ORDER BY fecha
    RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
) FROM tabla;

-- Funciones de agregación como ventana
SELECT 
    columna,
    SUM(columna) OVER (PARTITION BY categoria) AS suma_categoria,
    AVG(columna) OVER (
        ORDER BY fecha 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS promedio_movil
FROM tabla;
```

## Joins

### Tipos de Join
```sql
-- INNER JOIN: Registros con coincidencias en ambas tablas
SELECT t1.columna, t2.columna
FROM tabla1 t1
INNER JOIN tabla2 t2 ON t1.id = t2.id;

-- LEFT JOIN: Todos los registros de la tabla izquierda y las coincidencias de la derecha
SELECT t1.columna, t2.columna
FROM tabla1 t1
LEFT JOIN tabla2 t2 ON t1.id = t2.id;

-- RIGHT JOIN: Todos los registros de la tabla derecha y las coincidencias de la izquierda
SELECT t1.columna, t2.columna
FROM tabla1 t1
RIGHT JOIN tabla2 t2 ON t1.id = t2.id;

-- FULL JOIN: Todos los registros de ambas tablas
SELECT t1.columna, t2.columna
FROM tabla1 t1
FULL JOIN tabla2 t2 ON t1.id = t2.id;

-- CROSS JOIN: Producto cartesiano (todas las combinaciones posibles)
SELECT t1.columna, t2.columna
FROM tabla1 t1
CROSS JOIN tabla2 t2;

-- SELF JOIN: Join de una tabla consigo misma
SELECT e1.nombre AS empleado, e2.nombre AS supervisor
FROM empleados e1
JOIN empleados e2 ON e1.supervisor_id = e2.id;
```

### Joins Múltiples
```sql
SELECT o.id, c.nombre, p.descripcion
FROM ordenes o
JOIN clientes c ON o.cliente_id = c.id
JOIN productos p ON o.producto_id = p.id;
```

### Join con Condiciones Adicionales
```sql
SELECT *
FROM tabla1 t1
JOIN tabla2 t2 ON t1.id = t2.id AND t1.fecha > t2.fecha;
```

## Manipulación de Datos (DML)

### INSERT
```sql
-- Insertar una fila
INSERT INTO tabla (columna1, columna2) VALUES (valor1, valor2);

-- Insertar múltiples filas
INSERT INTO tabla (columna1, columna2)
VALUES (valor1, valor2), (valor3, valor4);

-- Insertar desde una consulta
INSERT INTO tabla (columna1, columna2)
SELECT columna1, columna2 FROM otra_tabla WHERE condición;

-- OUTPUT para capturar los valores insertados
INSERT INTO tabla (columna1, columna2)
OUTPUT INSERTED.*
VALUES (valor1, valor2);
```

### UPDATE
```sql
-- Actualizar todas las filas
UPDATE tabla SET columna1 = valor1, columna2 = valor2;

-- Actualizar con condición
UPDATE tabla SET columna1 = valor1 WHERE condición;

-- Actualizar con JOIN
UPDATE t1
SET t1.columna1 = t2.columna2
FROM tabla1 t1
JOIN tabla2 t2 ON t1.id = t2.id;

-- OUTPUT para capturar los valores antes y después
UPDATE tabla
SET columna1 = valor1
OUTPUT DELETED.columna1 AS valor_anterior, INSERTED.columna1 AS valor_nuevo
WHERE condición;
```

### DELETE
```sql
-- Eliminar todas las filas
DELETE FROM tabla;

-- Eliminar con condición
DELETE FROM tabla WHERE condición;

-- Eliminar con JOIN
DELETE t1
FROM tabla1 t1
JOIN tabla2 t2 ON t1.id = t2.id
WHERE t2.condición;

-- OUTPUT para capturar filas eliminadas
DELETE FROM tabla
OUTPUT DELETED.*
WHERE condición;
```

### MERGE
```sql
-- Sincronizar datos (INSERT, UPDATE, DELETE en una operación)
MERGE destino AS d
USING origen AS o ON d.id = o.id
WHEN MATCHED AND o.condición THEN
    UPDATE SET d.columna1 = o.columna1
WHEN MATCHED AND o.otra_condición THEN
    DELETE
WHEN NOT MATCHED BY TARGET THEN
    INSERT (columna1, columna2) VALUES (o.columna1, o.columna2)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE
OUTPUT $action, INSERTED.*, DELETED.*;
```

### TRUNCATE
```sql
-- Elimina todas las filas (más rápido que DELETE)
TRUNCATE TABLE tabla;
```

### BULK INSERT
```sql
-- Cargar datos desde un archivo
BULK INSERT tabla
FROM 'C:\ruta\archivo.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2
);
```

## Definición de Datos (DDL)

### Bases de Datos
```sql
-- Crear base de datos
CREATE DATABASE nombre
ON PRIMARY 
(
    NAME = 'nombre_datos',
    FILENAME = 'C:\ruta\nombre.mdf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 10%
)
LOG ON
(
    NAME = 'nombre_log',
    FILENAME = 'C:\ruta\nombre_log.ldf',
    SIZE = 50MB,
    MAXSIZE = 500MB,
    FILEGROWTH = 10MB
);

-- Modificar base de datos
ALTER DATABASE nombre
MODIFY FILE (NAME = 'nombre_datos', SIZE = 200MB);

-- Eliminar base de datos
DROP DATABASE nombre;

-- Información sobre bases de datos
SELECT * FROM sys.databases;
SELECT * FROM sys.database_files;
```

### Tablas
```sql
-- Crear tabla
CREATE TABLE nombre_tabla (
    columna1 tipo_dato [NULL | NOT NULL],
    columna2 tipo_dato CONSTRAINT constraint_nombre DEFAULT valor,
    columna3 tipo_dato IDENTITY(1,1),
    CONSTRAINT pk_nombre PRIMARY KEY (columna1),
    CONSTRAINT fk_nombre FOREIGN KEY (columna2) REFERENCES otra_tabla(columna),
    CONSTRAINT chk_nombre CHECK (columna3 > 0),
    CONSTRAINT uq_nombre UNIQUE (columna4)
);

-- Modificar tabla (agregar columna)
ALTER TABLE nombre_tabla ADD columna tipo_dato NULL;

-- Modificar tabla (modificar columna)
ALTER TABLE nombre_tabla ALTER COLUMN columna tipo_dato NOT NULL;

-- Modificar tabla (eliminar columna)
ALTER TABLE nombre_tabla DROP COLUMN columna;

-- Agregar restricción
ALTER TABLE nombre_tabla
ADD CONSTRAINT constraint_nombre CHECK (columna > 0);

-- Eliminar restricción
ALTER TABLE nombre_tabla
DROP CONSTRAINT constraint_nombre;

-- Eliminar tabla
DROP TABLE nombre_tabla;

-- Eliminar tabla si existe
IF OBJECT_ID('nombre_tabla', 'U') IS NOT NULL
    DROP TABLE nombre_tabla;

-- Información sobre tablas
SELECT * FROM sys.tables;
SELECT * FROM INFORMATION_SCHEMA.TABLES;
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'nombre_tabla';
```

### Esquemas
```sql
-- Crear esquema
CREATE SCHEMA nombre_esquema;

-- Crear tabla en esquema
CREATE TABLE nombre_esquema.nombre_tabla (columna tipo_dato);

-- Transferir un objeto a otro esquema
ALTER SCHEMA destino TRANSFER origen.objeto;

-- Eliminar esquema (debe estar vacío)
DROP SCHEMA nombre_esquema;
```

### Secuencias
```sql
-- Crear secuencia
CREATE SEQUENCE nombre_secuencia
    START WITH 1
    INCREMENT BY 1
    MINVALUE 1
    MAXVALUE 999999
    CYCLE
    CACHE 50;

-- Usar secuencia
SELECT NEXT VALUE FOR nombre_secuencia;
INSERT INTO tabla (id) VALUES (NEXT VALUE FOR nombre_secuencia);

-- Modificar secuencia
ALTER SEQUENCE nombre_secuencia
    INCREMENT BY 10
    CYCLE;

-- Eliminar secuencia
DROP SEQUENCE nombre_secuencia;
```

## Índices

### Tipos de Índices
```sql
-- Índice agrupado (solo uno por tabla)
CREATE CLUSTERED INDEX ix_nombre ON tabla (columna ASC);

-- Índice no agrupado
CREATE NONCLUSTERED INDEX ix_nombre ON tabla (columna);

-- Índice único
CREATE UNIQUE INDEX ix_nombre ON tabla (columna);

-- Índice de columnas incluidas
CREATE INDEX ix_nombre ON tabla (columna1)
INCLUDE (columna2, columna3);

-- Índice filtrado
CREATE INDEX ix_nombre ON tabla (columna)
WHERE columna > 0;

-- Índice con FILLFACTOR
CREATE INDEX ix_nombre ON tabla (columna)
WITH (FILLFACTOR = 80);

-- Índice columnstore (para data warehouse)
CREATE CLUSTERED COLUMNSTORE INDEX ix_nombre ON tabla;
CREATE NONCLUSTERED COLUMNSTORE INDEX ix_nombre ON tabla (columna1, columna2);

-- Índice espacial
CREATE SPATIAL INDEX ix_nombre ON tabla (columna_geografica)
USING GEOGRAPHY_GRID;

-- Índice XML
CREATE XML INDEX ix_nombre ON tabla (columna_xml);

-- Índice para texto completo (requiere catálogo)
CREATE FULLTEXT INDEX ON tabla (columna)
KEY INDEX pk_nombre
ON catalogo_nombre;
```

### Operaciones con Índices
```sql
-- Reconstruir índice
ALTER INDEX ix_nombre ON tabla REBUILD;

-- Reorganizar índice
ALTER INDEX ix_nombre ON tabla REORGANIZE;

-- Deshabilitar índice
ALTER INDEX ix_nombre ON tabla DISABLE;

-- Eliminar índice
DROP INDEX ix_nombre ON tabla;

-- Estadísticas de índices
SELECT * FROM sys.dm_db_index_physical_stats(
    DB_ID(), 
    OBJECT_ID('tabla'), 
    NULL, 
    NULL, 
    'DETAILED'
);
```

## Vistas

### Operaciones con Vistas
```sql
-- Crear vista
CREATE VIEW nombre_vista
AS
SELECT columna1, columna2
FROM tabla
WHERE condición;

-- Crear vista con esquema
CREATE VIEW esquema.nombre_vista
AS
SELECT * FROM tabla;

-- Crear vista indexada
CREATE VIEW nombre_vista
WITH SCHEMABINDING
AS
SELECT columna1, columna2
FROM dbo.tabla
WHERE condición;

CREATE UNIQUE CLUSTERED INDEX ix_nombre ON nombre_vista (columna1);

-- Crear vista particionada
CREATE VIEW nombre_vista
AS
SELECT *
FROM tabla1
UNION ALL
SELECT *
FROM tabla2;

-- Modificar vista
ALTER VIEW nombre_vista
AS
SELECT columna1, columna2, columna3
FROM tabla;

-- Eliminar vista
DROP VIEW nombre_vista;

-- Información sobre vistas
SELECT * FROM sys.views;
SELECT * FROM INFORMATION_SCHEMA.VIEWS;
```

## Procedimientos Almacenados

### Operaciones con Procedimientos
```sql
-- Crear procedimiento almacenado
CREATE PROCEDURE nombre_proc
    @parametro1 tipo_dato,
    @parametro2 tipo_dato = valor_default,
    @parametro3 tipo_dato OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Código SQL
    SELECT @parametro3 = columna FROM tabla WHERE columna = @parametro1;
    
    RETURN 0; -- Código de retorno
END;

-- Crear procedimiento con encriptación
CREATE PROCEDURE nombre_proc
WITH ENCRYPTION
AS
BEGIN
    -- Código SQL
END;

-- Ejecutar procedimiento
EXEC nombre_proc @parametro1 = valor1, @parametro2 = valor2;

-- Ejecutar con parámetro de salida
DECLARE @resultado tipo_dato;
EXEC nombre_proc @parametro1 = valor1, @parametro3 = @resultado OUTPUT;
SELECT @resultado;

-- Ejecutar con código de retorno
DECLARE @retorno int;
EXEC @retorno = nombre_proc @parametro1 = valor1;
SELECT @retorno;

-- Modificar procedimiento
ALTER PROCEDURE nombre_proc
AS
BEGIN
    -- Nuevo código SQL
END;

-- Eliminar procedimiento
DROP PROCEDURE nombre_proc;

-- Información sobre procedimientos
SELECT * FROM sys.procedures;
SELECT OBJECT_DEFINITION(OBJECT_ID('nombre_proc'));
```

## Funciones

### Tipos de Funciones
```sql
-- Función escalar (devuelve un valor)
CREATE FUNCTION nombre_funcion
(
    @parametro1 tipo_dato,
    @parametro2 tipo_dato
)
RETURNS tipo_dato
AS
BEGIN
    DECLARE @resultado tipo_dato;
    -- Código SQL
    SET @resultado = @parametro1 + @parametro2;
    RETURN @resultado;
END;

-- Uso de función escalar
SELECT dbo.nombre_funcion(valor1, valor2);

-- Función de tabla en línea
CREATE FUNCTION nombre_funcion
(
    @parametro tipo_dato
)
RETURNS TABLE
AS
RETURN
(
    SELECT columna1, columna2
    FROM tabla
    WHERE columna = @parametro
);

-- Uso de función de tabla en línea
SELECT * FROM dbo.nombre_funcion(valor);

-- Función de tabla de múltiples instrucciones
CREATE FUNCTION nombre_funcion
(
    @parametro tipo_dato
)
RETURNS @tabla TABLE
(
    columna1 tipo_dato,
    columna2 tipo_dato
)
AS
BEGIN
    INSERT INTO @tabla
    SELECT columna1, columna2
    FROM otra_tabla
    WHERE columna = @parametro;
    
    UPDATE @tabla SET columna1 = columna1 * 2;
    
    RETURN;
END;

-- Modificar función
ALTER FUNCTION nombre_funcion...

-- Eliminar función
DROP FUNCTION nombre_funcion;

-- Información sobre funciones
SELECT * FROM sys.objects WHERE type IN ('FN', 'IF', 'TF');
```

## Triggers

### Tipos de Triggers
```sql
-- Trigger DML (después de la operación)
CREATE TRIGGER nombre_trigger
ON tabla
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Tablas especiales: inserted (filas nuevas) y deleted (filas anteriores)
    IF EXISTS (SELECT * FROM inserted) AND EXISTS (SELECT * FROM deleted)
    BEGIN
        -- Código para UPDATE
    END
    ELSE IF EXISTS (SELECT * FROM inserted)
    BEGIN
        -- Código para INSERT
    END
    ELSE IF EXISTS (SELECT * FROM deleted)
    BEGIN
        -- Código para DELETE
    END
END;

-- Trigger DML (en lugar de la operación)
CREATE TRIGGER nombre_trigger
ON tabla
INSTEAD OF INSERT
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Código personalizado
    INSERT INTO tabla (columna1)
    SELECT columna1 FROM inserted WHERE condición;
END;

-- Trigger DDL (a nivel de base de datos)
CREATE TRIGGER nombre_trigger
ON DATABASE
FOR CREATE_TABLE, ALTER_TABLE, DROP_TABLE
AS
BEGIN
    PRINT 'Cambio de esquema detectado';
    -- Eventos disponibles en EVENTDATA()
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/EventType)[1]', 'nvarchar(100)');
END;

-- Trigger DDL (a nivel de servidor)
CREATE TRIGGER nombre_trigger
ON ALL SERVER
FOR CREATE_DATABASE
AS
BEGIN
    PRINT 'Nueva base de datos creada';
END;

-- Deshabilitar trigger
DISABLE TRIGGER nombre_trigger ON tabla;

-- Habilitar trigger
ENABLE TRIGGER nombre_trigger ON tabla;

-- Eliminar trigger
DROP TRIGGER nombre_trigger;
DROP TRIGGER nombre_trigger ON DATABASE;
DROP TRIGGER nombre_trigger ON ALL SERVER;

-- Información sobre triggers
SELECT * FROM sys.triggers;
SELECT * FROM sys.server_triggers;
```

## Transacciones

### Operaciones con Transacciones
```sql
-- Transacción básica
BEGIN TRANSACTION;
    -- Instrucciones SQL
    INSERT INTO tabla VALUES (1);
    UPDATE tabla SET columna = valor;
COMMIT TRANSACTION;

-- Con manejo de errores
BEGIN TRY
    BEGIN TRANSACTION;
        -- Instrucciones SQL
        INSERT INTO tabla VALUES (1);
        UPDATE tabla SET columna = valor;
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    SELECT 
        ERROR_NUMBER() AS ErrorNumber,
        ERROR_SEVERITY() AS ErrorSeverity,
        ERROR_STATE() AS ErrorState,
        ERROR_PROCEDURE() AS ErrorProcedure,
        ERROR_LINE() AS ErrorLine,
        ERROR_MESSAGE() AS ErrorMessage;
END CATCH;

-- Puntos de guardado
BEGIN TRANSACTION;
    INSERT INTO tabla VALUES (1);
    SAVE TRANSACTION punto1;
    
    UPDATE tabla SET columna = valor;
    SAVE TRANSACTION punto2;
    
    -- Si hay un error, se puede hacer rollback a un punto específico
    ROLLBACK TRANSACTION punto1;
    
COMMIT TRANSACTION;

-- Niveles de aislamiento
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED; -- Default
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Transacción con retardo
BEGIN TRANSACTION;
    WAITFOR DELAY '00:00:10'; -- Esperar 10 segundos
    -- Instrucciones SQL
COMMIT TRANSACTION;
```

## Funciones de Fecha y Hora

### Obtener Fecha/Hora Actual
```sql
SELECT 
    GETDATE(),                -- Fecha y hora actuales (datetime)
    CURRENT_TIMESTAMP,        -- Igual que GETDATE() (estándar ANSI)
    GETUTCDATE(),             -- Fecha y hora UTC (datetime)
    SYSDATETIME(),            -- Fecha y hora actuales (datetime2)
    SYSUTCDATETIME(),         -- Fecha y hora UTC (datetime2)
    SYSDATETIMEOFFSET();      -- Fecha y hora con zona horaria (datetimeoffset)
```

### Extraer Partes de Fecha
```sql
DECLARE @fecha datetime = '2023-04-15 14:30:25';

SELECT
    YEAR(@fecha),             -- 2023
    MONTH(@fecha),            -- 4
    DAY(@fecha),              -- 15
    DATEPART(year, @fecha),   -- 2023
    DATEPART(quarter, @fecha),-- 2
    DATEPART(week, @fecha),   -- Número de semana
    DATEPART(hour, @fecha),   -- 14
    DATEPART(minute, @fecha), -- 30
    DATEPART(second, @fecha), -- 25
    DATENAME(month, @fecha),  -- 'April' (nombre del mes)
    DATENAME(weekday, @fecha);-- 'Saturday' (nombre del día)
```

### Modificación de Fechas (continuación)
```sql
DECLARE @fecha datetime = '2023-04-15 14:30:25';

SELECT
    -- Sumar intervalos
    DATEADD(year, 1, @fecha),      -- Añadir 1 año
    DATEADD(month, 3, @fecha),     -- Añadir 3 meses
    DATEADD(day, 7, @fecha),       -- Añadir 7 días
    DATEADD(hour, 2, @fecha),      -- Añadir 2 horas
    DATEADD(minute, 15, @fecha),   -- Añadir 15 minutos
    DATEADD(second, 30, @fecha);   -- Añadir 30 segundos

    -- Diferencia entre fechas
DECLARE @fecha1 datetime = '2023-01-01';
DECLARE @fecha2 datetime = '2023-03-15';

SELECT
    DATEDIFF(year, @fecha1, @fecha2),    -- Diferencia en años
    DATEDIFF(month, @fecha1, @fecha2),   -- Diferencia en meses (2)
    DATEDIFF(day, @fecha1, @fecha2),     -- Diferencia en días (73)
    DATEDIFF(hour, @fecha1, @fecha2),    -- Diferencia en horas
    DATEDIFF(minute, @fecha1, @fecha2),  -- Diferencia en minutos
    DATEDIFF(second, @fecha1, @fecha2);  -- Diferencia en segundos
```

### Formateo de Fechas
```sql
DECLARE @fecha datetime = '2023-04-15 14:30:25';

-- Conversión a cadena con formato
SELECT 
    CONVERT(varchar, @fecha, 100),  -- mon dd yyyy hh:miAM (o PM)
    CONVERT(varchar, @fecha, 101),  -- mm/dd/yyyy
    CONVERT(varchar, @fecha, 102),  -- yyyy.mm.dd
    CONVERT(varchar, @fecha, 103),  -- dd/mm/yyyy
    CONVERT(varchar, @fecha, 104),  -- dd.mm.yyyy
    CONVERT(varchar, @fecha, 105),  -- dd-mm-yyyy
    CONVERT(varchar, @fecha, 106),  -- dd mon yyyy
    CONVERT(varchar, @fecha, 107),  -- mon dd, yyyy
    CONVERT(varchar, @fecha, 108),  -- hh:mi:ss
    CONVERT(varchar, @fecha, 109),  -- mon dd yyyy hh:mi:ss:mmmAM (o PM)
    CONVERT(varchar, @fecha, 110),  -- mm-dd-yyyy
    CONVERT(varchar, @fecha, 111),  -- yyyy/mm/dd
    CONVERT(varchar, @fecha, 112),  -- yyyymmdd
    CONVERT(varchar, @fecha, 113),  -- dd mon yyyy hh:mi:ss:mmm
    CONVERT(varchar, @fecha, 114),  -- hh:mi:ss:mmm(24h)
    CONVERT(varchar, @fecha, 120),  -- yyyy-mm-dd hh:mi:ss
    CONVERT(varchar, @fecha, 121),  -- yyyy-mm-dd hh:mi:ss.mmm
    CONVERT(varchar, @fecha, 126),  -- yyyy-mm-ddThh:mi:ss.mmm
    CONVERT(varchar, @fecha, 127);  -- yyyy-mm-ddThh:mi:ss.mmmZ
    
-- FORMAT (SQL Server 2012+)
SELECT 
    FORMAT(@fecha, 'd', 'en-US'),        -- MM/dd/yyyy
    FORMAT(@fecha, 'D', 'en-US'),        -- dddd, MMMM dd, yyyy
    FORMAT(@fecha, 'g', 'en-US'),        -- MM/dd/yyyy HH:mm
    FORMAT(@fecha, 'G', 'en-US'),        -- MM/dd/yyyy HH:mm:ss
    FORMAT(@fecha, 'yyyy-MM-dd'),        -- Formato personalizado
    FORMAT(@fecha, 'dd/MM/yyyy HH:mm');  -- Formato personalizado
```

### Funciones de Fecha Adicionales
```sql
-- Primer y último día del mes
SELECT 
    DATEADD(MONTH, DATEDIFF(MONTH, 0, GETDATE()), 0) AS PrimerDiaDelMes,
    DATEADD(DAY, -1, DATEADD(MONTH, DATEDIFF(MONTH, 0, GETDATE()) + 1, 0)) AS UltimoDiaDelMes;

-- Primer y último día de la semana
SELECT 
    DATEADD(DAY, -(DATEPART(WEEKDAY, GETDATE()) - 1), GETDATE()) AS PrimerDiaDeLaSemana,
    DATEADD(DAY, 7 - DATEPART(WEEKDAY, GETDATE()), GETDATE()) AS UltimoDiaDeLaSemana;

-- Primer y último día del año
SELECT 
    DATEFROMPARTS(YEAR(GETDATE()), 1, 1) AS PrimerDiaDelAño,
    DATEFROMPARTS(YEAR(GETDATE()), 12, 31) AS UltimoDiaDelAño;

-- Comprobar si una fecha es válida
SELECT 
    ISDATE('2023-02-28'),     -- 1 (válida)
    ISDATE('2023-02-30');     -- 0 (inválida)

-- Crear fecha a partir de partes (SQL Server 2012+)
SELECT 
    DATEFROMPARTS(2023, 4, 15),              -- fecha
    DATETIME2FROMPARTS(2023, 4, 15, 14, 30, 25, 500, 7),  -- datetime2
    DATETIMEOFFSETFROMPARTS(2023, 4, 15, 14, 30, 25, 500, 2, 0, 7);  -- datetimeoffset
```

## Funciones de Cadena

### Manipulación Básica
```sql
DECLARE @texto varchar(50) = '  SQL Server Functions  ';

SELECT
    LEN(@texto),                  -- 22 (longitud sin espacios finales)
    DATALENGTH(@texto),           -- 22 (longitud en bytes)
    LEFT(@texto, 5),              -- '  SQL'
    RIGHT(@texto, 10),            -- 'Functions  '
    LTRIM(@texto),                -- 'SQL Server Functions  ' (elimina espacios iniciales)
    RTRIM(@texto),                -- '  SQL Server Functions' (elimina espacios finales)
    TRIM(@texto),                 -- 'SQL Server Functions' (elimina espacios al inicio y final)
    UPPER(@texto),                -- '  SQL SERVER FUNCTIONS  ' (mayúsculas)
    LOWER(@texto),                -- '  sql server functions  ' (minúsculas)
    REVERSE(@texto),              -- '  snoitcnuF revreS LQS  ' (invierte)
    REPLACE(@texto, 'SQL', 'MS SQL'),  -- '  MS SQL Server Functions  '
    STUFF(@texto, 3, 3, 'Microsoft');  -- '  Microsoft Server Functions  '
```

### Extracción y Búsqueda
```sql
DECLARE @texto varchar(50) = 'SQL Server 2022: Funciones Avanzadas';

SELECT
    SUBSTRING(@texto, 5, 6),       -- 'Server'
    CHARINDEX('Server', @texto),   -- 5 (posición)
    PATINDEX('%20%', @texto),      -- 12 (posición del patrón)
    
    -- Dividir cadena (SQL Server 2016+)
    STRING_SPLIT(@texto, ' ');     -- Tabla con las palabras
    
-- Ejemplo de STRING_SPLIT
SELECT value FROM STRING_SPLIT('uno,dos,tres', ',');

-- Extraer por delimitadores
SELECT
    PARSENAME('database.schema.table.column', 1),  -- 'column'
    PARSENAME('database.schema.table.column', 2),  -- 'table'
    PARSENAME('database.schema.table.column', 3),  -- 'schema'
    PARSENAME('database.schema.table.column', 4);  -- 'database'
```

### Concatenación y Agregación
```sql
DECLARE @nombre varchar(20) = 'Juan';
DECLARE @apellido varchar(20) = 'López';

-- Concatenación
SELECT
    @nombre + ' ' + @apellido,            -- 'Juan López'
    CONCAT(@nombre, ' ', @apellido),      -- 'Juan López'
    CONCAT_WS(' ', @nombre, @apellido, 'Jr.');  -- 'Juan López Jr.' (con separador)

-- Agregación de cadenas (SQL Server 2017+)
SELECT 
    STRING_AGG(nombre, ', ') AS nombres
FROM empleados;

-- Con ORDER BY
SELECT 
    STRING_AGG(nombre, ', ') WITHIN GROUP (ORDER BY nombre) AS nombres
FROM empleados;
```

### Formateo y Conversión
```sql
-- Formato personalizado
SELECT
    FORMAT(1234567.89, 'N', 'en-us'),         -- '1,234,567.89'
    FORMAT(1234567.89, 'C', 'es-es'),         -- '1.234.567,89 €'
    FORMAT(0.35, 'P'),                        -- '35.00 %'
    FORMAT(123456789, '###-###-###'),         -- '123-456-789'
    FORMAT(GETDATE(), 'yyyy-MM-dd'),          -- '2023-04-15'
    FORMAT(CAST(GETDATE() AS datetime2), 'o');-- '2023-04-15T14:30:25.5000000'

-- Conversión de tipos
SELECT
    CAST(123.45 AS int),                      -- 123
    CAST('2023-04-15' AS date),               -- 2023-04-15
    CONVERT(varchar(10), GETDATE(), 120),     -- '2023-04-15'
    TRY_CAST('abc' AS int),                   -- NULL (no genera error)
    TRY_CONVERT(datetime, '2023-15-04');      -- NULL (fecha inválida)

-- Formateo de números
SELECT
    STR(123.456, 8, 2),                       -- '   123.46'
    SPACE(4) + 'SQL',                         -- '    SQL'
    REPLICATE('0', 5),                        -- '00000'
    QUOTENAME('tabla'),                       -- '[tabla]'
    QUOTENAME('tabla', '''');                 -- "'tabla'"
```

## Funciones Matemáticas

### Funciones Básicas
```sql
SELECT
    ABS(-15),              -- 15 (valor absoluto)
    SIGN(-15),             -- -1 (signo: -1, 0, 1)
    CEILING(15.2),         -- 16 (redondeo hacia arriba)
    FLOOR(15.7),           -- 15 (redondeo hacia abajo)
    ROUND(15.25, 1),       -- 15.30 (redondear a 1 decimal)
    ROUND(15.25, 0),       -- 15.00 (redondear a entero)
    ROUND(15.25, -1),      -- 20.00 (redondear a decena)
    POWER(2, 3),           -- 8 (potencia)
    SQRT(16),              -- 4 (raíz cuadrada)
    SQUARE(4),             -- 16 (cuadrado)
    LOG(100),              -- 4.60... (logaritmo natural)
    LOG10(100),            -- 2 (logaritmo base 10)
    EXP(1);                -- 2.71... (exponencial)
```

### Trigonometría
```sql
SELECT
    PI(),                  -- 3.14159...
    SIN(PI()/2),           -- 1 (seno)
    COS(0),                -- 1 (coseno)
    TAN(PI()/4),           -- 1 (tangente)
    ASIN(1),               -- 1.57... (arcoseno)
    ACOS(1),               -- 0 (arcocoseno)
    ATAN(1),               -- 0.78... (arcotangente)
    ATAN2(1, 1),           -- 0.78... (arcotangente de y/x)
    DEGREES(PI()),         -- 180 (radianes a grados)
    RADIANS(180);          -- 3.14... (grados a radianes)
```

### Números Aleatorios
```sql
SELECT
    RAND(),                -- Número aleatorio entre 0 y 1
    RAND(CHECKSUM(NEWID())), -- Aleatorio con semilla diferente
    FLOOR(RAND() * 10) + 1;  -- Número aleatorio entre 1 y 10
```

## Consultas Avanzadas

### Common Table Expressions (CTE)
```sql
-- CTE básico
WITH EmpleadosActivos AS (
    SELECT id, nombre, departamento
    FROM empleados
    WHERE activo = 1
)
SELECT * FROM EmpleadosActivos;

-- CTE múltiples
WITH 
    Dept AS (
        SELECT id, nombre FROM departamentos
    ),
    EmpCount AS (
        SELECT departamento_id, COUNT(*) AS num_empleados
        FROM empleados
        GROUP BY departamento_id
    )
SELECT d.nombre, ISNULL(e.num_empleados, 0) AS total
FROM Dept d
LEFT JOIN EmpCount e ON d.id = e.departamento_id;

-- CTE recursivo
WITH EmpleadosJerarquia AS (
    -- Caso base
    SELECT id, nombre, supervisor_id, 0 AS nivel
    FROM empleados
    WHERE supervisor_id IS NULL
    
    UNION ALL
    
    -- Caso recursivo
    SELECT e.id, e.nombre, e.supervisor_id, h.nivel + 1
    FROM empleados e
    INNER JOIN EmpleadosJerarquia h ON e.supervisor_id = h.id
)
SELECT 
    REPLICATE('--', nivel) + nombre AS jerarquia,
    nivel
FROM EmpleadosJerarquia
ORDER BY nivel, nombre;
```

### Subconsultas Avanzadas
```sql
-- Subconsulta en SELECT
SELECT 
    nombre,
    (SELECT COUNT(*) FROM ventas WHERE ventas.cliente_id = clientes.id) AS total_ventas
FROM clientes;

-- Subconsulta con EXISTS
SELECT nombre
FROM productos p
WHERE EXISTS (
    SELECT 1 
    FROM ventas v 
    WHERE v.producto_id = p.id AND v.fecha > '2023-01-01'
);

-- Subconsulta con ALL
SELECT nombre
FROM productos
WHERE precio > ALL (
    SELECT precio FROM productos WHERE categoria = 'Básico'
);

-- Subconsulta con ANY/SOME
SELECT nombre
FROM productos
WHERE precio > ANY (
    SELECT precio FROM productos WHERE categoria = 'Premium'
);

-- Subconsulta correlacionada
SELECT c.nombre,
       (SELECT MAX(v.monto) 
        FROM ventas v 
        WHERE v.cliente_id = c.id) AS compra_mayor
FROM clientes c;

-- Subconsulta en FROM con APPLY
SELECT c.nombre, v.fecha, v.monto
FROM clientes c
CROSS APPLY (
    SELECT TOP 3 fecha, monto
    FROM ventas
    WHERE cliente_id = c.id
    ORDER BY monto DESC
) v;

-- OUTER APPLY (similar a LEFT JOIN)
SELECT c.nombre, v.fecha, v.monto
FROM clientes c
OUTER APPLY (
    SELECT TOP 3 fecha, monto
    FROM ventas
    WHERE cliente_id = c.id
    ORDER BY monto DESC
) v;
```

### Tablas Temporales
```sql
-- Tabla temporal local (#)
CREATE TABLE #TempTable (
    id int PRIMARY KEY,
    dato varchar(50)
);

-- Tabla temporal global (##)
CREATE TABLE ##GlobalTemp (
    id int PRIMARY KEY,
    dato varchar(50)
);

-- Tabla de variables
DECLARE @MiTabla TABLE (
    id int PRIMARY KEY,
    dato varchar(50)
);

-- Tabla temporal del sistema (SQL Server 2016+)
CREATE TABLE dbo.Productos (
    id int PRIMARY KEY,
    nombre varchar(100),
    precio decimal(10,2),
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL,
    SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductosHistorial));

-- Consulta de datos históricos
SELECT * FROM Productos
FOR SYSTEM_TIME
    ALL                                             -- Todas las versiones
    | AS OF '2023-01-01 12:00:00'                  -- Estado en un momento específico
    | FROM '2023-01-01' TO '2023-02-01'            -- Versiones en un rango (exclusivo)
    | BETWEEN '2023-01-01' AND '2023-02-01'        -- Versiones en un rango (inclusivo)
    | CONTAINED IN ('2023-01-01', '2023-02-01');   -- Solo versiones completas en el rango
```

### Manejo de JSON (SQL Server 2016+)
```sql
-- Crear JSON
SELECT 
    nombre,
    edad,
    ciudad
FROM clientes
FOR JSON PATH;

-- Con opciones
SELECT 
    id,
    nombre,
    edad,
    ciudad
FROM clientes
FOR JSON PATH, ROOT('clientes'), INCLUDE_NULL_VALUES;

-- Consultar JSON
DECLARE @json nvarchar(max) = N'{"id": 1, "info": {"nombre": "Juan", "ciudad": "Madrid"}}';

SELECT
    JSON_VALUE(@json, '$.id') AS id,
    JSON_VALUE(@json, '$.info.nombre') AS nombre,
    JSON_QUERY(@json, '$.info') AS info_completa;

-- Modificar JSON
SELECT 
    JSON_MODIFY(@json, '$.id', 2) AS json_modificado,
    JSON_MODIFY(@json, '$.info.nombre', 'Pedro') AS nombre_modificado,
    JSON_MODIFY(@json, 'append $.telefonos', '555-1234') AS agregar_telefono;

-- OPENJSON (conversión de JSON a tabla)
SELECT *
FROM OPENJSON(@json)
WITH (
    id int '$.id',
    nombre nvarchar(50) '$.info.nombre',
    ciudad nvarchar(50) '$.info.ciudad'
);
```

### Manejo de XML
```sql
-- Crear XML
SELECT 
    id,
    nombre,
    edad,
    ciudad
FROM clientes
FOR XML PATH('cliente'), ROOT('clientes');

-- Consultar XML
DECLARE @xml XML = '<clientes><cliente id="1"><nombre>Juan</nombre><ciudad>Madrid</ciudad></cliente></clientes>';

SELECT 
    @xml.value('(/clientes/cliente/@id)[1]', 'int') AS id,
    @xml.value('(/clientes/cliente/nombre/text())[1]', 'nvarchar(50)') AS nombre,
    @xml.query('/clientes/cliente') AS cliente_completo;

-- XPath con nodos XML
SELECT 
    c.query('.') AS cliente,
    c.value('@id', 'int') AS id,
    c.value('nombre[1]', 'nvarchar(50)') AS nombre
FROM @xml.nodes('/clientes/cliente') AS n(c);

-- Modificar XML
SET @xml.modify('
    insert <telefono>555-1234</telefono>
    into (/clientes/cliente)[1]
');

-- Esquema XML
CREATE XML SCHEMA COLLECTION EsquemaClientes AS '
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="clientes">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="cliente" maxOccurs="unbounded">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="nombre" type="xs:string" />
              <xs:element name="ciudad" type="xs:string" />
            </xs:sequence>
            <xs:attribute name="id" type="xs:integer" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
';

-- Usar esquema XML
DECLARE @xml_tipado XML(EsquemaClientes);
SET @xml_tipado = '<clientes><cliente id="1"><nombre>Juan</nombre><ciudad>Madrid</ciudad></cliente></clientes>';
```

## Control de Flujo

### Estructuras de Control
```sql
-- IF-ELSE
IF EXISTS (SELECT * FROM tabla WHERE condición)
BEGIN
    -- Código si la condición es verdadera
    SELECT 'Existe';
END
ELSE
BEGIN
    -- Código si la condición es falsa
    SELECT 'No existe';
END;

-- CASE
SELECT 
    nombre,
    CASE 
        WHEN edad < 18 THEN 'Menor'
        WHEN edad BETWEEN 18 AND 65 THEN 'Adulto'
        ELSE 'Senior'
    END AS categoria
FROM clientes;

-- CASE con expresión
SELECT 
    nombre,
    CASE estado
        WHEN 'A' THEN 'Activo'
        WHEN 'I' THEN 'Inactivo'
        WHEN 'P' THEN 'Pendiente'
        ELSE 'Desconocido'
    END AS estado_desc
FROM clientes;

-- WHILE
DECLARE @contador int = 1;
WHILE @contador <= 5
BEGIN
    PRINT 'Contador: ' + CAST(@contador AS varchar);
    SET @contador = @contador + 1;
    
    IF @contador = 3
        CONTINUE;  -- Salta a la siguiente iteración
        
    IF @contador > 4
        BREAK;     -- Sale del bucle
END;

-- TRY-CATCH
BEGIN TRY
    -- Código que puede causar error
    INSERT INTO tabla VALUES (1);
    DELETE FROM tabla_no_existe;
END TRY
BEGIN CATCH
    SELECT 
        ERROR_NUMBER() AS numero,
        ERROR_SEVERITY() AS severidad,
        ERROR_STATE() AS estado,
        ERROR_PROCEDURE() AS procedimiento,
        ERROR_LINE() AS linea,
        ERROR_MESSAGE() AS mensaje;
        
    -- Re-lanzar el error
    -- THROW;
    
    -- O personalizar el error
    THROW 50000, 'Error personalizado', 1;
END CATCH;

-- WAITFOR
WAITFOR DELAY '00:00:05';  -- Esperar 5 segundos
WAITFOR TIME '15:00:00';   -- Esperar hasta las 15:00
```

### Cursores
```sql
-- Declaración e inicialización de cursor
DECLARE empleados_cursor CURSOR FOR
    SELECT id, nombre FROM empleados;
    
OPEN empleados_cursor;

-- Variables para almacenar valores del cursor
DECLARE @id int, @nombre varchar(50);

-- Leer primer registro
FETCH NEXT FROM empleados_cursor INTO @id, @nombre;

-- Recorrer todos los registros
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Procesar registro actual
    PRINT 'ID: ' + CAST(@id AS varchar) + ', Nombre: ' + @nombre;
    
    -- Leer siguiente registro
    FETCH NEXT FROM empleados_cursor INTO @id, @nombre;
END;

-- Cerrar y liberar cursor
CLOSE empleados_cursor;
DEALLOCATE empleados_cursor;

-- Cursor con opciones
DECLARE empleados_cursor CURSOR
    LOCAL                        -- Solo visible en el batch actual
    STATIC                       -- Copia estática de los datos
    READ_ONLY                    -- No se pueden modificar los datos
    FORWARD_ONLY                 -- Solo avance hacia adelante
FOR
    SELECT id, nombre FROM empleados;
```

### Variables y Operaciones Dinámicas
```sql
-- Variables
DECLARE @tabla_nombre varchar(50) = 'empleados';
DECLARE @sql nvarchar(max);

-- SQL dinámico básico
SET @sql = 'SELECT * FROM ' + @tabla_nombre;
EXEC(@sql);

-- SQL dinámico con parámetros (sp_executesql)
DECLARE @id int = 10;
SET @sql = N'SELECT * FROM empleados WHERE id = @id';
EXEC sp_executesql @sql, N'@id int', @id;

-- SQL dinámico para filtros condicionales
SET @sql = N'SELECT * FROM empleados WHERE 1=1';

IF @id IS NOT NULL
    SET @sql = @sql + N' AND id = @id';
    
EXEC sp_executesql @sql, N'@id int', @id;

-- SQL dinámico para columnas dinámicas
DECLARE @columnas varchar(100) = 'id, nombre, salario';
SET @sql = N'SELECT ' + @columnas + ' FROM empleados';
EXEC(@sql);

-- SQL dinámico para ordenación dinámica
DECLARE @orden varchar(100) = 'nombre ASC';
SET @sql = N'SELECT * FROM empleados ORDER BY ' + @orden;
EXEC(@sql);
```

## Optimización y Rendimiento

### Estadísticas y Planes de Ejecución
```sql
-- Mostrar el plan de ejecución
SET SHOWPLAN_ALL ON;
    SELECT * FROM tabla WHERE columna = valor;
SET SHOWPLAN_ALL OFF;

-- Mostrar estadísticas de E/S y tiempo
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
    SELECT * FROM tabla WHERE columna = valor;
SET STATISTICS IO OFF;
SET STATISTICS TIME OFF;

-- Consulta de estadísticas actuales
SELECT 
    OBJECT_NAME(s.object_id) AS tabla,
    s.name AS estadistica,
    s.auto_created,
    s.user_created,
    STATS_DATE(s.object_id, s.stats_id) AS ultima_actualizacion
FROM sys.stats s
WHERE s.object_id = OBJECT_ID('tabla');

-- Actualizar estadísticas
UPDATE STATISTICS tabla;
UPDATE STATISTICS tabla (estadistica);
UPDATE STATISTICS tabla WITH FULLSCAN;
UPDATE STATISTICS tabla WITH SAMPLE 50 PERCENT;

-- Ver detalles de un plan de ejecución
SELECT 
    p.query_plan,
    t.text
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle) AS p
CROSS APPLY sys.dm_exec_sql_text(cp.plan_handle) AS t
WHERE t.text LIKE '%FROM tabla%';

-- Consultas con alto consumo de CPU
SELECT TOP 10
    qs.total_worker_time / qs.execution_count AS avg_cpu_time,
    qs.execution_count,
    SUBSTRING(qt.text, (qs.statement_start_offset/2) + 1,
        ((CASE qs.statement_end_offset
            WHEN -1 THEN DATALENGTH(qt.text)
            ELSE qs.statement_end_offset
          END - qs.statement_start_offset)/2) + 1) AS sql_text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY avg_cpu_time DESC;
```

### Índices y Optimización
```sql
-- Índices faltantes sugeridos
SELECT 
    d.statement AS tabla,
    d.equality_columns,
    d.inequality_columns,
    d.included_columns,
    s.avg_total_user_cost * s.avg_user_impact * (s.user_seeks + s.user_scans) AS impacto,
    'CREATE INDEX ix_' + 
        OBJECT_NAME(d.object_id, d.database_id) + '_' + 
        REPLACE(REPLACE(REPLACE(
            ISNULL(d.equality_columns, '') + 
            ISNULL('_' + d.inequality_columns, ''), 
        '[', ''), ']', ''), ', ', '_') + 
        ' ON ' + d.statement + 
        ' (' + ISNULL(d.equality_columns, '') + 
        CASE WHEN d.inequality_columns IS NULL THEN '' 
             ELSE CASE WHEN d.equality_columns IS NULL THEN '' ELSE ',' END + d.inequality_columns END + ')' + 
        CASE WHEN d.included_columns IS NULL THEN '' ELSE ' INCLUDE (' + d.included_columns + ')' END AS create_index_stmt
FROM sys.dm_db_missing_index_details d
JOIN sys.dm_db_missing_index_groups g ON d.index_handle = g.index_handle
JOIN sys.dm_db_missing_index_group_stats s ON g.index_group_handle = s.group_handle
ORDER BY impacto DESC;

-- Índices no utilizados
SELECT 
    OBJECT_NAME(i.object_id) AS tabla,
    i.name AS indice,
    i.type_desc,
    STATS_DATE(i.object_id, i.index_id) AS ultima_actualizacion,
    us.user_seeks,
    us.user_scans,
    us.user_lookups,
    us.user_updates
FROM sys.indexes i
LEFT JOIN sys.dm_db_index_usage_stats us ON i.object_id = us.object_id AND i.index_id = us.index_id AND us.database_id = DB_ID()
WHERE i.object_id > 100 -- Filtrar objetos de sistema
  AND i.index_id > 0    -- Filtrar heaps
  AND i.is_primary_key = 0
  AND i.is_unique_constraint = 0
  AND (us.user_seeks + us.user_scans + us.user_lookups) IS NULL OR (us.user_seeks + us.user_scans + us.user_lookups) = 0
ORDER BY OBJECT_NAME(i.object_id), i.name;

-- Fragmentación de índices
SELECT 
    OBJECT_NAME(ps.object_id) AS tabla,
    i.name AS indice,
    ps.index_type_desc,
    ps.avg_fragmentation_in_percent,
    ps.page_count
FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') ps
JOIN sys.indexes i ON ps.object_id = i.object_id AND ps.index_id = i.index_id
WHERE ps.avg_fragmentation_in_percent > 30
ORDER BY ps.avg_fragmentation_in_percent DESC;

-- Reconstruir o reorganizar índices fragmentados
-- Para fragmentación > 30%: reconstruir
ALTER INDEX ALL ON tabla REBUILD;
ALTER INDEX nombre_indice ON tabla REBUILD;
-- Con opciones
ALTER INDEX nombre_indice ON tabla REBUILD WITH 
    (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, ONLINE = ON);

-- Para fragmentación entre 5% y 30%: reorganizar
ALTER INDEX ALL ON tabla REORGANIZE;
ALTER INDEX nombre_indice ON tabla REORGANIZE;

-- Actualizar estadísticas después de mantenimiento
UPDATE STATISTICS tabla WITH FULLSCAN;
```

### Query Store
```sql
-- Habilitar Query Store
ALTER DATABASE nombre_bd SET QUERY_STORE = ON;

-- Configurar Query Store
ALTER DATABASE nombre_bd SET QUERY_STORE (
    OPERATION_MODE = READ_WRITE,
    CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30),
    DATA_FLUSH_INTERVAL_SECONDS = 900,
    MAX_STORAGE_SIZE_MB = 1000,
    INTERVAL_LENGTH_MINUTES = 60
);

-- Consultar planes en Query Store
SELECT 
    qsq.query_id,
    qsq.query_text_id,
    qsqt.query_sql_text,
    qsp.plan_id,
    CAST(qsp.query_plan AS XML) AS plan_xml,
    qsrs.avg_duration,
    qsrs.avg_logical_io_reads,
    qsrs.count_executions
FROM sys.query_store_query qsq
JOIN sys.query_store_query_text qsqt ON qsq.query_text_id = qsqt.query_text_id
JOIN sys.query_store_plan qsp ON qsq.query_id = qsp.query_id
JOIN sys.query_store_runtime_stats qsrs ON qsp.plan_id = qsrs.plan_id
WHERE qsqt.query_sql_text LIKE '%FROM tabla%';

-- Forzar un plan específico
EXEC sys.sp_query_store_force_plan @query_id = 42, @plan_id = 73;

-- Quitar un plan forzado
EXEC sys.sp_query_store_unforce_plan @query_id = 42, @plan_id = 73;
```

### Consejos de Optimización
```sql
-- Usar índices apropiados
-- Evitar funciones en columnas indexadas en el WHERE
SELECT * FROM tabla WHERE YEAR(fecha) = 2023; -- Malo
SELECT * FROM tabla WHERE fecha BETWEEN '20230101' AND '20231231'; -- Mejor

-- Evitar conversiones implícitas
SELECT * FROM tabla WHERE id = '123'; -- Malo (id es INT)
SELECT * FROM tabla WHERE id = 123; -- Mejor

-- Usar JOIN en lugar de subconsultas correlacionadas
SELECT c.nombre, 
       (SELECT MAX(v.monto) FROM ventas v WHERE v.cliente_id = c.id) -- Menos eficiente
FROM clientes c;

-- Mejor alternativa
SELECT c.nombre, v.max_monto
FROM clientes c
LEFT JOIN (
    SELECT cliente_id, MAX(monto) AS max_monto 
    FROM ventas 
    GROUP BY cliente_id
) v ON c.id = v.cliente_id;

-- Usar EXISTS en lugar de COUNT para verificar existencia
IF (SELECT COUNT(*) FROM tabla WHERE id = 1) > 0 -- Menos eficiente
IF EXISTS (SELECT 1 FROM tabla WHERE id = 1) -- Más eficiente
```

## Seguridad

### Usuarios y Roles
```sql
-- Crear login a nivel de servidor
CREATE LOGIN login_name WITH PASSWORD = 'contraseña_segura';

-- Crear usuario de base de datos
USE nombre_bd;
CREATE USER nombre_usuario FOR LOGIN login_name;

-- Crear usuario sin login (para grupos de permisos)
CREATE USER nombre_usuario WITHOUT LOGIN;

-- Crear rol de servidor
CREATE SERVER ROLE nombre_rol;

-- Crear rol de base de datos
CREATE ROLE nombre_rol;

-- Asignar usuarios a roles
ALTER ROLE nombre_rol ADD MEMBER nombre_usuario;
ALTER SERVER ROLE nombre_rol ADD MEMBER login_name;

-- Asignar rol a otro rol (anidamiento)
ALTER ROLE rol_padre ADD MEMBER rol_hijo;

-- Roles fijos de servidor
SELECT name, type_desc FROM sys.server_principals WHERE type = 'R';
-- sysadmin, serveradmin, securityadmin, processadmin, setupadmin, 
-- bulkadmin, diskadmin, dbcreator, public

-- Roles fijos de base de datos
SELECT name, type_desc FROM sys.database_principals WHERE type = 'R';
-- db_owner, db_securityadmin, db_accessadmin, db_backupoperator, 
-- db_ddladmin, db_datawriter, db_datareader, public
```

### Permisos
```sql
-- Sintaxis básica de permisos
GRANT permiso ON objeto TO principal;
DENY permiso ON objeto TO principal;
REVOKE permiso ON objeto FROM principal;

-- Permisos a nivel de servidor
GRANT CREATE ANY DATABASE TO login_name;
GRANT ALTER ANY LOGIN TO login_name;

-- Permisos a nivel de base de datos
GRANT CREATE TABLE TO nombre_usuario;
GRANT ALTER ON SCHEMA::dbo TO nombre_usuario;

-- Permisos a nivel de objeto
GRANT SELECT, INSERT, UPDATE, DELETE ON tabla TO nombre_usuario;
GRANT EXECUTE ON procedimiento TO nombre_usuario;
GRANT SELECT ON vista TO nombre_usuario;

-- Permisos con opciones de cascada
GRANT SELECT ON tabla TO nombre_usuario WITH GRANT OPTION;
REVOKE SELECT ON tabla FROM nombre_usuario CASCADE;

-- Permisos específicos de columna
GRANT SELECT (columna1, columna2) ON tabla TO nombre_usuario;
DENY UPDATE (columna_sensible) ON tabla TO nombre_usuario;

-- Ver permisos efectivos
SELECT * FROM fn_my_permissions(NULL, 'DATABASE'); -- Permisos en la BD actual
SELECT * FROM fn_my_permissions('dbo.tabla', 'OBJECT'); -- Permisos en un objeto
```

### Seguridad a Nivel de Fila
```sql
-- Crear función de predicado para filtrar filas
CREATE FUNCTION dbo.fn_SecurityPredicate(@dept_id int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN
    SELECT 1 AS resultado
    WHERE @dept_id = USER_NAME() -- Simplificado para el ejemplo
    OR IS_MEMBER('rol_admin') = 1;

-- Aplicar política de seguridad a nivel de fila
CREATE SECURITY POLICY FilterPolicy
ADD FILTER PREDICATE dbo.fn_SecurityPredicate(departamento_id)
ON dbo.empleados
WITH (STATE = ON);

-- Modificar política
ALTER SECURITY POLICY FilterPolicy
WITH (STATE = OFF);

-- Eliminar política
DROP SECURITY POLICY FilterPolicy;
```

### Enmascaramiento de Datos
```sql
-- Añadir enmascaramiento a columnas
ALTER TABLE clientes
ALTER COLUMN telefono ADD MASKED WITH (FUNCTION = 'partial(0,"XXX-XXX-XX",4)');

ALTER TABLE clientes
ALTER COLUMN email ADD MASKED WITH (FUNCTION = 'email()');

ALTER TABLE clientes
ALTER COLUMN tarjeta_credito ADD MASKED WITH (FUNCTION = 'credit_card()');

ALTER TABLE empleados
ALTER COLUMN salario ADD MASKED WITH (FUNCTION = 'default()');

-- Quitar enmascaramiento
ALTER TABLE clientes
ALTER COLUMN telefono DROP MASKED;

-- Gestionar permisos para ver datos sin enmascarar
GRANT UNMASK TO nombre_usuario;
REVOKE UNMASK FROM nombre_usuario;

-- Ver configuración de enmascaramiento
SELECT t.name AS tabla, c.name AS columna, c.is_masked, c.masking_function
FROM sys.masked_columns c
JOIN sys.tables t ON c.object_id = t.object_id;
```

### Cifrado y Seguridad Avanzada
```sql
-- Crear clave maestra de base de datos
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'contraseña_compleja';

-- Crear certificado
CREATE CERTIFICATE cert_nombre
WITH SUBJECT = 'Certificado de Prueba',
EXPIRY_DATE = '2030-12-31';

-- Crear clave simétrica
CREATE SYMMETRIC KEY clave_simetrica
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE cert_nombre;

-- Usar cifrado en columnas
-- 1. Crear tabla con columna cifrada
CREATE TABLE datos_sensibles (
    id int PRIMARY KEY,
    dato_plano varchar(50),
    dato_cifrado varbinary(128)
);

-- 2. Cifrar datos al insertar
OPEN SYMMETRIC KEY clave_simetrica DECRYPTION BY CERTIFICATE cert_nombre;

INSERT INTO datos_sensibles (id, dato_plano, dato_cifrado)
VALUES (1, 'texto visible', EncryptByKey(Key_GUID('clave_simetrica'), 'texto secreto'));

CLOSE SYMMETRIC KEY clave_simetrica;

-- 3. Descifrar datos al consultar
OPEN SYMMETRIC KEY clave_simetrica DECRYPTION BY CERTIFICATE cert_nombre;

SELECT 
    id, 
    dato_plano,
    CONVERT(varchar(50), DecryptByKey(dato_cifrado)) AS dato_descifrado
FROM datos_sensibles;

CLOSE SYMMETRIC KEY clave_simetrica;

-- Always Encrypted (sin descifrado en servidor)
-- Requiere configuración en SSMS y aplicación cliente
CREATE TABLE datos_siempre_cifrados (
    id int PRIMARY KEY,
    dato_normal varchar(50),
    dato_deterministico varchar(50) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
                     ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
                     COLUMN_ENCRYPTION_KEY = 'CEK_Auto1') NULL,
    dato_aleatorio varchar(50) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
                     ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
                     COLUMN_ENCRYPTION_KEY = 'CEK_Auto1') NULL
);
```

### Auditoría
```sql
-- Crear especificación de auditoría de servidor
CREATE SERVER AUDIT mi_auditoria
TO FILE (FILEPATH = 'C:\auditorias\');

-- Crear especificación de auditoría de servidor
CREATE SERVER AUDIT SPECIFICATION auditoria_servidor
FOR SERVER AUDIT mi_auditoria
ADD (FAILED_LOGIN_GROUP),
ADD (SUCCESSFUL_LOGIN_GROUP),
ADD (SERVER_OPERATION_GROUP)
WITH (STATE = ON);

-- Crear especificación de auditoría de base de datos
CREATE DATABASE AUDIT SPECIFICATION auditoria_bd
FOR SERVER AUDIT mi_auditoria
ADD (SELECT, INSERT, UPDATE, DELETE ON DATABASE::nombre_bd BY public)
WITH (STATE = ON);

-- Habilitar auditoría
ALTER SERVER AUDIT mi_auditoria WITH (STATE = ON);

-- Consultar datos de auditoría
SELECT * FROM fn_get_audit_file('C:\auditorias\*.sqlaudit', default, default);

-- Deshabilitar y eliminar auditoría
ALTER SERVER AUDIT mi_auditoria WITH (STATE = OFF);
DROP SERVER AUDIT SPECIFICATION auditoria_servidor;
DROP DATABASE AUDIT SPECIFICATION auditoria_bd;
DROP SERVER AUDIT mi_auditoria;
```

## Mantenimiento

### Copias de Seguridad y Restauración
```sql
-- Copia de seguridad completa
BACKUP DATABASE nombre_bd 
TO DISK = 'C:\backups\nombre_bd_full.bak' 
WITH INIT, COMPRESSION, CHECKSUM, STATS = 10;

-- Copia de seguridad diferencial
BACKUP DATABASE nombre_bd 
TO DISK = 'C:\backups\nombre_bd_diff.bak' 
WITH DIFFERENTIAL, COMPRESSION, STATS = 10;

-- Copia de seguridad del log de transacciones
BACKUP LOG nombre_bd 
TO DISK = 'C:\backups\nombre_bd_log.trn' 
WITH COMPRESSION, STATS = 10;

-- Restaurar base de datos completa (con reemplazo)
RESTORE DATABASE nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_full.bak' 
WITH REPLACE, STATS = 10;

-- Restaurar completa + diferencial
RESTORE DATABASE nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_full.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE DATABASE nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_diff.bak' 
WITH RECOVERY, STATS = 10;

-- Restaurar completa + logs (para point-in-time recovery)
RESTORE DATABASE nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_full.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_log1.trn' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG nombre_bd 
FROM DISK = 'C:\backups\nombre_bd_log2.trn' 
WITH RECOVERY, STOPAT = '2023-04-15 14:30:00', STATS = 10;

-- Verificar integridad de backup
RESTORE VERIFYONLY 
FROM DISK = 'C:\backups\nombre_bd_full.bak';
```

### Tareas de Mantenimiento
```sql
-- Verificar integridad de base de datos
DBCC CHECKDB('nombre_bd') WITH NO_INFOMSGS;

-- Verificar integridad de tabla
DBCC CHECKTABLE('tabla') WITH NO_INFOMSGS;

-- Verificar asignación de espacio
DBCC CHECKALLOC('nombre_bd') WITH NO_INFOMSGS;

-- Reparar base de datos (usar con precaución)
DBCC CHECKDB('nombre_bd', REPAIR_ALLOW_DATA_LOSS) WITH NO_INFOMSGS;
DBCC CHECKDB('nombre_bd', REPAIR_REBUILD) WITH NO_INFOMSGS;

-- Encoger archivos de la base de datos
DBCC SHRINKDATABASE('nombre_bd', 10); -- Dejar 10% libre
DBCC SHRINKFILE('nombre_bd_log', 100); -- Reducir a 100MB

-- Recompilar procedimientos almacenados
EXEC sp_recompile 'nombre_procedimiento';

-- Purgar el caché de planes
DBCC FREEPROCCACHE;
DBCC FREEPROCCACHE (plan_handle); -- Un plan específico

-- Purgar el búfer de datos
DBCC DROPCLEANBUFFERS;

-- Limpiar tablas del sistema
EXEC sp_updatestats; -- Actualizar todas las estadísticas
DBCC UPDATEUSAGE('nombre_bd'); -- Corregir información de espacio
```

### Automatización de Tareas
```sql
-- Crear un trabajo de SQL Server Agent
USE msdb;
GO

EXEC sp_add_job
    @job_name = 'MantenimientoDiario',
    @description = 'Tareas diarias de mantenimiento',
    @category_name = 'Database Maintenance',
    @owner_login_name = 'sa';

-- Agregar pasos al trabajo
EXEC sp_add_jobstep
    @job_name = 'MantenimientoDiario',
    @step_name = 'Actualizar Estadísticas',
    @subsystem = 'TSQL',
    @command = 'EXEC sp_updatestats',
    @database_name = 'nombre_bd',
    @retry_attempts = 3,
    @retry_interval = 5;

EXEC sp_add_jobstep
    @job_name = 'MantenimientoDiario',
    @step_name = 'Copia de Seguridad',
    @subsystem = 'TSQL',
    @command = 'BACKUP DATABASE nombre_bd TO DISK = ''C:\backups\nombre_bd_$(ESCAPE_NONE(DATE)).bak'' WITH COMPRESSION',
    @database_name = 'master',
    @retry_attempts = 3,
    @retry_interval = 5;

-- Crear una programación
EXEC sp_add_schedule
    @schedule_name = 'Diario_Medianoche',
    @freq_type = 4, -- Daily
    @freq_interval = 1, -- Every day
    @active_start_time = 000000; -- 00:00:00

-- Asignar el trabajo a la programación
EXEC sp_attach_schedule
    @job_name = 'MantenimientoDiario',
    @schedule_name = 'Diario_Medianoche';

-- Habilitar el trabajo
EXEC sp_update_job
    @job_name = 'MantenimientoDiario',
    @enabled = 1;
```

### Monitorización del sistema
```sql
-- Monitorizar sesiones activas
SELECT
    s.session_id,
    s.login_name,
    s.host_name,
    s.program_name,
    s.login_time,
    s.status,
    r.command,
    r.start_time,
    r.percent_complete,
    r.estimated_completion_time,
    r.cpu_time,
    r.total_elapsed_time,
    r.reads,
    r.writes,
    r.logical_reads,
    st.text AS query_text,
    CASE 
        WHEN r.statement_start_offset = 0 AND r.statement_end_offset = 0 THEN st.text
        ELSE SUBSTRING(st.text, r.statement_start_offset/2 + 1, 
          ((CASE WHEN r.statement_end_offset = -1 THEN DATALENGTH(st.text) 
           ELSE r.statement_end_offset END) - r.statement_start_offset)/2 + 1)
    END AS current_statement
FROM sys.dm_exec_sessions s
LEFT JOIN sys.dm_exec_requests r ON s.session_id = r.session_id
OUTER APPLY sys.dm_exec_sql_text(r.sql_handle) st
WHERE s.session_id > 50 -- Filtrar sesiones del sistema
ORDER BY r.cpu_time DESC;

-- Bloqueos y deadlocks
SELECT
    tl.request_session_id AS session_id,
    s.login_name,
    DB_NAME(tl.resource_database_id) AS database_name,
    OBJECT_NAME(p.object_id) AS blocked_object,
    tl.resource_type,
    tl.request_mode,
    wt.wait_duration_ms,
    wt.wait_type,
    st.text AS query_text,
    CASE 
        WHEN r.statement_start_offset = 0 AND r.statement_end_offset = 0 THEN st.text
        ELSE SUBSTRING(st.text, r.statement_start_offset/2 + 1, 
          ((CASE WHEN r.statement_end_offset = -1 THEN DATALENGTH(st.text) 
           ELSE r.statement_end_offset END) - r.statement_start_offset)/2 + 1)
    END AS current_statement
FROM sys.dm_tran_locks tl
JOIN sys.dm_os_waiting_tasks wt ON tl.lock_owner_address = wt.resource_address
JOIN sys.partitions p ON p.hobt_id = tl.resource_associated_entity_id
JOIN sys.dm_exec_sessions s ON s.session_id = tl.request_session_id
LEFT JOIN sys.dm_exec_requests r ON r.session_id = tl.request_session_id
OUTER APPLY sys.dm_exec_sql_text(r.sql_handle) st;

-- Uso de recursos (CPU, memoria, E/S)
SELECT TOP 10
    r.session_id,
    s.login_name,
    r.status,
    r.command,
    r.cpu_time,
    r.total_elapsed_time,
    r.reads,
    r.writes,
    r.logical_reads,
    r.row_count,
    SUBSTRING(st.text, r.statement_start_offset/2 + 1, 
        ((CASE WHEN r.statement_end_offset = -1 THEN DATALENGTH(st.text) 
          ELSE r.statement_end_offset END) - r.statement_start_offset)/2 + 1) AS current_statement
FROM sys.dm_exec_requests r
JOIN sys.dm_exec_sessions s ON r.session_id = s.session_id
OUTER APPLY sys.dm_exec_sql_text(r.sql_handle) st
WHERE r.session_id > 50
ORDER BY r.cpu_time DESC;

-- Uso de memoria por consulta
SELECT TOP 20
    qs.total_worker_time / qs.execution_count AS avg_cpu_time,
    qs.total_logical_reads / qs.execution_count AS avg_logical_reads,
    qs.execution_count,
    SUBSTRING(qt.text, (qs.statement_start_offset/2) + 1, 
        ((CASE qs.statement_end_offset WHEN -1 THEN DATALENGTH(qt.text) 
          ELSE qs.statement_end_offset END - qs.statement_start_offset)/2) + 1) AS query_text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) as qt
ORDER BY qs.total_logical_reads / qs.execution_count DESC;
```

### Capacidad y Crecimiento
```sql
-- Tamaño de las bases de datos
SELECT
    d.name AS database_name,
    ROUND(SUM(CASE WHEN type = 0 THEN size END) * 8 / 1024.0, 2) AS data_size_mb,
    ROUND(SUM(CASE WHEN type = 1 THEN size END) * 8 / 1024.0, 2) AS log_size_mb,
    ROUND(SUM(size) * 8 / 1024.0, 2) AS total_size_mb
FROM sys.databases d
JOIN sys.master_files mf ON d.database_id = mf.database_id
GROUP BY d.name
ORDER BY total_size_mb DESC;

-- Tamaño de tablas en la base de datos actual
SELECT
    s.name AS schema_name,
    t.name AS table_name,
    p.rows AS row_count,
    ROUND(SUM(a.total_pages) * 8 / 1024.0, 2) AS total_space_mb,
    ROUND(SUM(a.used_pages) * 8 / 1024.0, 2) AS used_space_mb,
    ROUND((SUM(a.total_pages) - SUM(a.used_pages)) * 8 / 1024.0, 2) AS unused_space_mb
FROM sys.tables t
JOIN sys.schemas s ON t.schema_id = s.schema_id
JOIN sys.indexes i ON t.object_id = i.object_id
JOIN sys.partitions p ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN sys.allocation_units a ON p.partition_id = a.container_id
GROUP BY s.name, t.name, p.rows
ORDER BY total_space_mb DESC;

-- Espacio libre en archivos de datos
SELECT
    name AS file_name,
    physical_name,
    ROUND(size / 128.0, 2) AS total_size_mb,
    ROUND(FILEPROPERTY(name, 'SpaceUsed') / 128.0, 2) AS space_used_mb,
    ROUND((size - FILEPROPERTY(name, 'SpaceUsed')) / 128.0, 2) AS free_space_mb,
    ROUND((size - FILEPROPERTY(name, 'SpaceUsed')) / 128.0 * 100 / (size / 128.0), 2) AS free_percent
FROM sys.database_files
WHERE type = 0 -- Data files
ORDER BY free_space_mb DESC;

-- Predicción de crecimiento (requiere información histórica de DataCollector)
SELECT
    database_name,
    YEAR(collection_time) AS year,
    MONTH(collection_time) AS month,
    MAX(data_size_kb) / 1024.0 AS max_data_size_mb,
    MAX(log_size_kb) / 1024.0 AS max_log_size_mb
FROM (
    SELECT
        database_name,
        collection_time,
        SUM(CASE WHEN file_type = 0 THEN size_kb ELSE 0 END) AS data_size_kb,
        SUM(CASE WHEN file_type = 1 THEN size_kb ELSE 0 END) AS log_size_kb
    FROM msdb.dbo.syscollector_collection_items i
    JOIN msdb.dbo.syscollector_collection_sets s ON i.collection_set_id = s.collection_set_id
    JOIN msdb.dbo.database_size_history h ON i.collection_item_id = h.collection_item_id
    GROUP BY database_name, collection_time
) AS t
GROUP BY database_name, YEAR(collection_time), MONTH(collection_time)
ORDER BY database_name, year, month;
```
