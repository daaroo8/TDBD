# Ejercicios de Consultas SQL (Parte 3)

## Operaciones previas
Para realizar las pruebas de los ejercicios de `JOIN` externos (Left, Right, Full), necesitamos datos de prueba que no tengan correspondencia.

* Insertar un departamento que no tiene empleados.
* Insertar un empleado que no está asignado a ningún departamento.

```sql
INSERT INTO departamentos VALUES ('SinEmpleados', 9, NULL, NULL);
INSERT INTO empleados VALUES ('Daniel', 'Fernandez', '474747474', NULL, 'Direccion TEST', 'h', 47000, '888665555', NULL);
```

---

### 1. Nombre, apellidos y NSS de los empleados que trabajan en el departamento de ‘Contabilidad’
Uso de subconsulta básica para obtener el ID del departamento.

```sql
SELECT nombre, apellidos, nss
FROM empleados
WHERE depno = (SELECT depno FROM departamentos WHERE nombre = 'Contabilidad');
```

### 2. Nombre y apellidos de los empleados que tienen alguna hija
Uso de `IN` para comparar contra una lista de resultados.

```sql
SELECT nombre, apellidos
FROM empleados
WHERE nss IN (SELECT nss FROM familiares WHERE relacion = 'hija');
```

### 3. Datos combinados de empleados y departamentos (Tipos de JOIN)
Nombre, apellidos, nss, y datos del departamento (nombre y número) de todos los empleados.

**a) Mostrar únicamente empleados asignados a algún departamento y departamentos que tienen empleados**
Uso de `INNER JOIN` (Intersección).

```sql
SELECT e.nombre, e.apellidos, e.nss, d.nombre, d.depno
FROM empleados e
JOIN departamentos d ON e.depno = d.depno;
-- WHERE e.depno IS NOT NULL; (Redundante: el JOIN ya filtra los nulos)
```

**b) Mostrar también los empleados que aún no están asignados a ningún departamento**
Uso de `LEFT JOIN` (Prioridad a la tabla de la izquierda: Empleados).

```sql
SELECT e.nombre, e.apellidos, e.nss, d.nombre, d.depno
FROM empleados e
LEFT JOIN departamentos d ON e.depno = d.depno;
```

**c) Mostrar también los departamentos que no tienen asignados empleados**
Uso de `RIGHT JOIN` (Prioridad a la tabla de la derecha: Departamentos).

```sql
SELECT e.nombre, e.apellidos, e.nss, d.nombre, d.depno
FROM empleados e
RIGHT JOIN departamentos d ON e.depno = d.depno;
```

**d) Mostrar tanto los empleados sin departamento como los departamentos sin empleados**
Uso de `FULL JOIN` (Unión de ambos conjuntos).

```sql
SELECT e.nombre, e.apellidos, e.nss, d.nombre, d.depno
FROM empleados e
FULL JOIN departamentos d ON e.depno = d.depno;
```

### 4. Nombre y apellidos del jefe de cada departamento
Unión estándar entre empleados y departamentos usando la clave foránea del jefe (`jefenss`).

```sql
SELECT departamentos.nombre, empleados.nombre, empleados.apellidos 
FROM empleados
JOIN departamentos ON empleados.nss = departamentos.jefenss;
```

### 5. Proyectos con más de 10 horas de trabajo y datos del empleado
Unión de tres tablas (`trabajar` como puente entre `empleados` y `proyectos`).

```sql
SELECT p.proyno, p.nombre, e.nss, e.nombre, e.depno
FROM trabajar t
JOIN empleados e ON t.nss = e.nss
JOIN proyectos p ON t.proyno = p.proyno
WHERE t.horas > 10;
```

### 6. Proyectos que se localizan en el mismo lugar que el departamento que los gestiona
El `JOIN` requiere dos condiciones simultáneas: coincidencia de departamento y de localidad.

```sql
SELECT p.proyno, p.nombre
FROM proyectos p
JOIN localizaciones l ON p.depno = l.depno AND p.localidad = l.localidad;
```

### 7. Nombre, apellidos y salario de los empleados, junto con el de su supervisor
Ejemplo de **Self Join** (unir la tabla consigo misma).

```sql
SELECT e.nombre, e.apellidos, e.salario, j.nombre, j.apellidos, j.salario
FROM empleados e
JOIN empleados j ON e.supernss = j.nss;
```

### 8. Proyectos con la media de horas máxima
"Mayor o igual que cualquier otro" equivale al máximo. Se requiere `GROUP BY` y `HAVING`.

**Opción A: Usando ALL**
```sql
SELECT p.proyno, p.nombre 
FROM proyectos p
JOIN trabajar t ON p.proyno = t.proyno
GROUP BY p.proyno, p.nombre
HAVING AVG(t.horas) >= ALL (SELECT AVG(horas) FROM trabajar GROUP BY proyno);
```

**Opción B: Usando MAX**
```sql
SELECT p.proyno, p.nombre 
FROM proyectos p
JOIN trabajar t ON p.proyno = t.proyno
GROUP BY p.proyno, p.nombre
HAVING AVG(t.horas) >= (SELECT MAX(AVG(horas)) FROM trabajar GROUP BY proyno);
```

### 9. Estadísticas por departamento (Personal y Salarios)
Uso de `LEFT JOIN` para incluir departamentos vacíos (que aparecerán con 0 trabajadores).

```sql
SELECT d.depno, d.nombre, COUNT(e.nss) AS trabajadores, AVG(e.salario) AS salario_medio
FROM departamentos d
LEFT JOIN empleados e ON d.depno = e.depno
GROUP BY d.depno, d.nombre;
```

### 10. Departamentos con más de 3 empleados
Filtrado de grupos mediante `HAVING`.

```sql
SELECT d.depno, d.nombre, COUNT(e.nss) AS num_empleados
FROM departamentos d
JOIN empleados e ON d.depno = e.depno
GROUP BY d.depno, d.nombre
HAVING COUNT(e.nss) > 3;
```

### 11. Departamentos con más proyectos que cualquier otro
Similar al ejercicio 8, pero contando proyectos. Se usa `>= ALL` para encontrar el máximo.

```sql
SELECT d.nombre, d.depno 
FROM departamentos d
LEFT JOIN proyectos p ON d.depno = p.depno
GROUP BY d.nombre, d.depno
HAVING COUNT(p.proyno) >= ALL (
  SELECT COUNT(proyno) 
  FROM proyectos 
  GROUP BY depno
);
```