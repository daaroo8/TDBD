# Ejercicios de Consultas SQL (Parte 2)

### 1. NSS de los empleados que tienen tanto hijos como hijas
Se usa `INTERSECT` para obtener los NSS que aparecen en ambos grupos (los que tienen hijos y los que tienen hijas).

```sql
SELECT nss FROM familiares WHERE relacion = 'hijo'
INTERSECT
SELECT nss FROM familiares WHERE relacion = 'hija';
```

### 2. Suma, máximo y mínimo de los salarios del departamento de ‘Contabilidad’
Uso de funciones de grupo (`SUM`, `MAX`, `MIN`) filtrando por el departamento obtenido en la subconsulta.

```sql
SELECT SUM(salario), MAX(salario), MIN(salario) 
FROM empleados
WHERE depno = (SELECT depno FROM departamentos WHERE nombre = 'Contabilidad');
```

### 3. Empleados que tienen más de 2 familiares
Uso de una subconsulta correlacionada para contar los familiares de cada empleado.

```sql
SELECT * FROM empleados
WHERE (SELECT COUNT(*) FROM familiares WHERE familiares.nss = empleados.nss) > 2;
```

### 4. Nombre, apellidos y NSS de los empleados que trabajan en el departamento de ‘Contabilidad’
Consulta básica con subconsulta para obtener el ID del departamento.

```sql
SELECT nombre, apellidos, nss 
FROM empleados
WHERE depno = (SELECT depno FROM departamentos WHERE nombre = 'Contabilidad');
```

### 5. Nombre y apellidos de los empleados que no trabajan en el proyecto número 3
Uso de `NOT EXISTS` para excluir a aquellos que tienen relación con el proyecto 3.

```sql
SELECT nombre, apellidos 
FROM empleados
WHERE NOT EXISTS (SELECT * FROM trabajar WHERE empleados.nss = trabajar.nss AND proyno = 3);
```

### 6. Nombre y apellidos de los empleados que no tienen familiares
Uso de `NOT EXISTS` para encontrar empleados sin registros en la tabla familiares.

```sql
SELECT nombre, apellidos 
FROM empleados
WHERE NOT EXISTS (SELECT * FROM familiares WHERE empleados.nss = familiares.nss);
```

### 7. Nombre y apellidos de los supervisores que tienen al menos un familiar
Se verifica que el NSS esté en la lista de jefes (`supernss`) y que tenga registros en familiares.

```sql
SELECT nombre, apellidos 
FROM empleados
WHERE nss IN (SELECT supernss FROM empleados)
  AND (SELECT COUNT(*) FROM familiares WHERE familiares.nss = empleados.nss