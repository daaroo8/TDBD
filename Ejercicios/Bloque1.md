# Ejercicios de Consultas SQL (Parte 1)

### 1. Número de la Seguridad Social de los empleados que trabajan 10 horas o más en algún proyecto
Consulta básica con `JOIN`. Nota: Esta consulta puede devolver duplicados si un empleado trabaja en varios proyectos.

```sql
SELECT nss
FROM trabajar
WHERE trabajar.horas >= 10;
```

### 2. Número de la Seguridad Social de los empleados que trabajan 10 horas o más en algún proyecto (sin repeticiones)
Se elimina la duplicidad. Se presentan dos formas de hacerlo:

**Opción A: Usando DISTINCT**
```sql
SELECT DISTINCT nss 
FROM trabajar
WHERE trabajar.horas >= 10;
```

**Opción B: Usando IN (Más eficiente/elegante)**
```sql
SELECT nss 
FROM empleados
WHERE nss IN (SELECT nss FROM trabajar WHERE horas >= 10);
```

### 3. Empleados que no tienen asignado supervisor
Se filtran aquellos cuyo campo `supernss` es nulo (`IS NULL`).

```sql
SELECT * FROM empleados
WHERE supernss IS NULL;
```

### 4. Empleados cuya dirección está en Valladolid
Uso del operador `LIKE` con comodines `%` para buscar la cadena de texto dentro del campo dirección.

```sql
SELECT * FROM empleados
WHERE direccion LIKE '%Valladolid%';
```

### 5. Número de los departamentos que están ubicados en Valladolid o Palencia
Cuenta cuántos departamentos distintos existen en esas localidades.

```sql
SELECT depno
FROM localizaciones
WHERE localidad = 'Valladolid' OR localidad = 'Palencia';
```

### 6. NSS de los empleados que tienen hijos e hijas
Uso de `INTERSECT` para encontrar empleados que cumplen ambas condiciones simultáneamente.

```sql
SELECT nss FROM familiares WHERE relacion = 'hijo'
INTERSECT
SELECT nss FROM familiares WHERE relacion = 'hija';
```

### 7. Suma, máximo y mínimo de los salarios
Uso de funciones de agregación sobre toda la tabla empleados.

```sql
SELECT SUM(salario), MAX(salario), MIN(salario) 
FROM empleados;
```

### 8. Contar el número de empleados de la compañía
Cuenta el total de filas en la tabla empleados.

```sql
SELECT COUNT(*) 
FROM empleados;
```