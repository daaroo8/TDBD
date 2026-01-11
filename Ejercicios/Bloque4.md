# Ejercicios de Consultas SQL (Parte 4)

### 1. Empleados (NSS) que trabajan en todos los proyectos de la empresa.
```sql
SELECT e.nss
FROM empleados e
WHERE NOT EXISTS (
    SELECT p.proyno
    FROM proyectos p
    WHERE NOT EXISTS (
        SELECT *
        FROM trabajar t
        WHERE t.nss = e.nss
          AND t.proyno = p.proyno
    )
);
```

Hay una forma más sencilla de hacerlo usando `GROUP BY` y `HAVING`:
```sql
SELECT t.nss
FROM trabajar t
GROUP BY t.nss
HAVING COUNT(t.proyno) = (SELECT COUNT(*) FROM proyectos);
```

### 2. Departamentos que participan en todos los proyectos de la empresa.
```sql
SELECT d.nombre
FROM departamentos d
WHERE NOT EXISTS (
    SELECT p.proyno
    FROM proyectos p
    WHERE NOT EXISTS (
        SELECT *
        FROM empleados e
        JOIN trabajar t ON e.nss = t.nss
        WHERE e.depno = d.depno
          AND t.proyno = p.proyno
    )
);
```

### 3. Proyectos en los que han participado todos los empleados del departamento 1.
```sql
SELECT p.nombre, p.proyno
FROM proyectos p
JOIN trabajar t ON p.proyno = t.proyno
JOIN empleados e ON t.nss = e.nss
WHERE e.depno = 1
GROUP BY p.nombre, p.proyno
HAVING COUNT(DISTINCT e.nss) = (
    SELECT COUNT(*) 
    FROM empleados 
    WHERE depno = 1
);
```
  
### 4. Departamentos que llevan proyectos en todas las ciudades donde la empresa lleva proyectos.
```sql
SELECT d.nombre
FROM departamentos d
JOIN proyectos p ON d.depno = p.depno
GROUP BY d.nombre, d.depno
HAVING COUNT(DISTINCT p.localidad) = (
    SELECT COUNT(DISTINCT localidad) 
    FROM proyectos
);
```

### 5. Supervisores que sólo han participado en proyectos coordinados por su departamento.
```sql
SELECT e.nombre, e.apellidos
FROM empleados e
WHERE e.nss IN (SELECT supernss FROM empleados)
AND NOT EXISTS (
    SELECT 1
    FROM trabajar t
    JOIN proyectos p ON t.proyno = p.proyno
    WHERE t.nss = e.nss
      AND p.depno != e.depno
);
```