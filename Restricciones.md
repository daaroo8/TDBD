# Restricciones en la Base de Datos

Las restricciones en una base de datos son reglas que garantizan la integridad y validez de los datos almacenados. Estas restricciones pueden aplicarse a nivel de tabla, columna o base de datos completa, y ayudan a prevenir errores y mantener la coherencia de la información.

## Aplicada a una sola tabla
Estas restricciones aseguran que los datos dentro de una tabla específica cumplan con ciertas condiciones.

```sql
ALTER TABLE nombre_tabla
ADD CONSTRAINT nombre_restriccion
CHECK (condición);
```

**Ejemplo:**
```sql
ALTER TABLE Empleados
ADD CONSTRAINT chk_salario
CHECK (salario > 0);
```

## Aplicada entre tablas (Integridad Referencial)
Estas restricciones aseguran que las relaciones entre tablas sean válidas.

```sql
CREATE ASSERTION nombre_restriccion
CHECK (condición);
```

**Ejemplo:**
```sql
CREATE ASSERTION chk_departamento_existe
CHECK (NOT EXISTS (
    SELECT *
    FROM Empleados e
    WHERE NOT EXISTS (
        SELECT *
        FROM Departamentos d
        WHERE e.departamento_id = d.id
    )
));