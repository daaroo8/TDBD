# Ejericiocios de Consultas SQL (Parte 3)

## Operaciones previas
- Insertar un departamento que no tiene empleados: (‘SinEmpleados’, 9, NULL, NULL).
- Insertar un empleados que no está asignado a ningún departamento.

```sql
INSERT INTO departamentos VALUES ('SinEmpleados', 9, NULL, NULL)
INSERT INTO empleados VALUES ('Daniel', 'Fernandez', '474747474', NULL, 'Direccion TEST', 'h', 47000, '888665555', NULL);
```

1. Nombre, apellidos y NSS de los empleados que trabajan en el departamento de
‘Contabilidad’
```sql
SELECT nombre, apellidos, nss
FROM empleados
WHERE depno = (SELECT depno FROM departamentos WHERE nombre = 'Contabilidad');
```

2. Nombre y apellidos de los empleados que tienen alguna hija.
```sql
SELECT nombre, apellidos
FROM empleados
WHERE nss IN (SELECT nss FROM familiares WHERE relacion = 'hija');
```

3. Nombre, apellidos, nss, y datos del departamento (nombre y número) de todos los
empleados:
a) Mostrar únicamente empleados asignados a algún departamento y
departamentos que tienen empleados
```sql
SELECT e.nombre, e.apellidos, e.nss, d.nombre, d.depno
FROM empleados e
JOIN departamentos d ON e.depno = d.depno;
-- WHERE e.depno IS NOT NULL; Esto sería redundante con ya que NULL = NULL siempre es falso
```
b) Mostrar también los empleados que aún no están asignados a ningún
departamento