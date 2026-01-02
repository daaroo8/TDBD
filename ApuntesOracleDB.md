# Oracle DB
## Comandos útiles (metadatos)
select table_name from all_tables; -- Listar todas las tablas
select table_name from user_tables; -- Listar tablas del usuario actual
desc nombre_tabla; -- Describir la estructura de una tabla
select sysdate from dual; -- Obtener la fecha y hora actual del sistema
select user from dual; -- Obtener el usuario actual

## Consultas básicas
### SELECT
select * from EMPLEADOS; -- Seleccionar todos los registros de una tabla
select nombre, salario from EMPLEADOS where salario > 50000; -- Seleccionar columnas específicas con condición

#### Conjunto de datos
select nombre, salario from EMPLEADOS where departamento in ('Ventas', 'Marketing'); -- Filtrar y ordenar resultados

#### Rangos
select nombre, salario from EMPLEADOS where salario between 40000 and 80000; -- Filtrar por rango de valores

#### Patrones
select nombre, salario from EMPLEADOS where nombre like 'J%'; -- Filtrar por patrón de texto

#### Operadores lógicos
select nombre, salario from EMPLEADOS where salario > 50000 and departamento = 'Ventas'; -- Usar operadores lógicos

#### Personalizar fecha
select nombre, to_char(fecha_contratacion, 'YYYY-MM-DD') from EMPLEADOS where fecha_contratacion > to_date('2020-01-01', 'YYYY-MM-DD'); -- Filtrar por fecha personalizada

#### Eliminar duplicados
select distinct departamento from EMPLEADOS; -- Seleccionar valores únicos

#### Media, suma, máximo y mínimo
select avg(salario) from EMPLEADOS; -- Calcular la media de una columna
select sum(salario) from EMPLEADOS; -- Calcular la suma de una columna
select max(salario) from EMPLEADOS; -- Obtener el valor máximo de una columna
select min(salario) from EMPLEADOS; -- Obtener el valor mínimo de una columna

#### Contar registros
select count(*) from EMPLEADOS; -- Contar el número de registros

#### Contar distintos 
select count(distinct departamento) from EMPLEADOS; -- Contar valores distintos en una columna

### INSERT
insert into EMPLEADOS (nombre, salario, departamento) values ('Juan Perez', 60000, 'Ventas'); -- Insertar un nuevo registro

### UPDATE
update EMPLEADOS set salario = salario * 1.1 where departamento = 'Ventas'; -- Actualizar registros existentes

### DELETE
delete from EMPLEADOS where nombre = 'Juan Perez'; -- Eliminar registros

### Subconsultas
select nombre from EMPLEADOS where salario > (select avg(salario) from EMPLEADOS); -- Usar subconsulta en condición

#### Subconsultas con EXISTS
select nombre from empleados where exists (select *
from familiares where nss = empleados.nss and relacion = 'hija'); -- Filtrar registros basados en la existencia de registros relacionados

## Comandos
### Crear fichero de comandos
edit nombre_fichero.sql; -- Crear o editar un fichero de comandos

### Ejecutar fichero de comandos
@nombre_fichero.sql; -- Ejecutar un fichero de comandos

### Grabar historial de comandos
spool nombre_fichero; -- Iniciar grabación del historial
spool off; -- Detener grabación del historial
