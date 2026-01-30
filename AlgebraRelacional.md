# Algebra relacional

El álgebra relacional es un conjunto de operaciones que se aplican a las relaciones (tablas) en una base de datos relacional para manipular y consultar los datos almacenados en ellas. Estas operaciones permiten combinar, filtrar, proyectar y transformar las relaciones de diversas maneras para obtener resultados específicos. Es lo que está detrás de las consultas SQL.

## Operadores Unarios (operan sobre una sola relación)
### Proyección (π)
- Selecciona columnas específicas de una relación.
- Sintaxis: <code>π<sub>A1, A2, ...</sub>(R)</code> donde `A1, A2, ...` son los atributos a seleccionar y `R` es la relación original.
- Ejemplo: <code>π<sub>Nombre, Edad</sub>(Empleados)</code> devuelve una relación con solo las columnas Nombre y Edad de la tabla Empleados.
- Equivalente SQL: `SELECT Nombre, Edad FROM Empleados;`

> [!WARNING]
> La proyección elimina duplicados automáticamente, ya que el álgebra relacional trabaja con conjuntos.

### Selección (σ)
- Filtra filas basadas en una condición.
- Sintaxis: <code>σ<sub>condición</sub>(R)</code> donde `condición` es una expresión lógica que define qué filas incluir y `R` es la relación original.
- Ejemplo: <code>σ<sub>Edad > 30</sub>(Empleados)</code> devuelve una relación con solo las filas donde la Edad es mayor que 30.
- Equivalente SQL: `SELECT * FROM Empleados WHERE Edad > 30;`

### Renombrar (ρ)
- Cambia el nombre de una relación o de sus atributos.
- Sintaxis: <code>ρ<sub>NuevoNombre(A1, A2, ...)</sub>(R)</code> donde `NuevoNombre` es el nuevo nombre de la relación, `A1, A2, ...` (los cuales son opcionales) son los nuevos nombres de los atributos y `R` es la relación original.
- Ejemplo: <code>ρ<sub>EmpleadosRenombrados(Nombre, Edad)</sub>(Empleados)</code> cambia el nombre de la relación a EmpleadosRenombrados y los atributos a Nombre y Edad.
- Equivalente SQL: `AS` en la cláusula `FROM` o `SELECT`.

## Operadores de Conjuntos (operan sobre dos relaciones)

> [!WARNING]
> Para poder usar estos operadores, las relaciones deben ser "*compatibles*": deben tener **el mismo** número de columnas y **el mismo** tipo de datos.

### Unión (∪)
- Combina las filas de dos relaciones, eliminando duplicados.
- Sintaxis: R1 ∪ R2 donde `R1` y `R2` son las dos relaciones.
- Ejemplo: <code>π<sub>Nombre</sub>(Empleados1) ∪ π<sub>Nombre</sub>(Empleados2)</code> devuelve una relación que contiene todas las filas de Empleados1 y Empleados2, sin duplicados.
- Equivalente SQL: `UNION`

### Intersección (∩)
- Devuelve las filas que son comunes a ambas relaciones.
- Sintaxis: R1 ∩ R2 donde `R1` y `R2` son las dos relaciones.
- Ejemplo: <code>π<sub>Nombre</sub>(Empleados1) ∩ π<sub>Nombre</sub>(Empleados2)</code> devuelve una relación con las filas que están en ambas tablas.
- Equivalente SQL: `INTERSECT` (aunque a veces se simula con `JOIN` o subconsultas).

### Diferencia (−)
- Devuelve las filas que están en la primera relación pero no en la segunda.
- Sintaxis: R1 − R2 donde `R1` y `R2` son las dos relaciones.
- Ejemplo: <code>π<sub>Nombre</sub>(Empleados1) − π<sub>Nombre</sub>(Empleados2)</code> devuelve una relación con las filas que están en Empleados1 pero no en Empleados2.
- Equivalente SQL: `EXCEPT`

## Operadores de Combbinación (los JOINs)
### Producto Cartesiano (×)
- Combina todas las filas de la primera relación con todas las filas de la segunda relación.
- Sintaxis: R1 × R2 donde `R1` y `R2` son las dos relaciones.
- Ejemplo: <code>Empleados × Departamentos</code> devuelve una relación que contiene todas las combinaciones posibles de filas entre Empleados y Departamentos.
- Equivalente SQL: `CROSS JOIN` o simplemente listando ambas tablas en el `FROM`.
- Nota: El producto cartesiano puede generar un número muy grande de filas, por lo que generalmente se usa junto con una condición de selección.

### Reunión (JOIN) Natural (⋈)
- Combina filas de dos relaciones basándose en atributos comunes. Elimina las columnas duplicadas.
- Sintaxis: R1 ⋈ R2 donde `R1` y `R2` son las dos relaciones.
- Ejemplo: <code>Empleados ⋈ Departamentos</code> combina las filas de Empleados y Departamentos donde los valores de los atributos comunes coinciden.
- Equivalente SQL: `NATURAL JOIN`
- Proceso interno:
    * Realiza el producto cartesiano de R1 y R2.
    * Selecciona las filas donde los atributos comunes son iguales.
    * Proyecta las columnas necesarias, eliminando duplicados.

### Reunión (JOIN) Theta o Condicional (⨝<sub>condición</sub>)
- Igual que el JOIN natural, pero permite especificar una condición personalizada para combinar las filas.
- Sintaxis: R1 ⨝<sub>condición</sub> R2 donde `R1` y `R2` son las dos relaciones y `condición` es la expresión lógica que define cómo combinar las filas.
- Ejemplo: <code>Empleados ⨝<sub>Empleados.DepartamentoID = Departamentos.ID</sub> Departamentos</code> combina las filas de Empleados y Departamentos donde el DepartamentoID de Empleados coincide con el ID de Departamentos.
- Equivalente SQL: `SELECT * FROM EMPLEADOS JOIN DEPARTAMENTOS ON EMPLEADOS.DepartamentoID = DEPARTAMENTOS.ID;`

### Reunión Externa (OUTER JOIN)
> [!NOTE]
> Esta operación los apuntes no la profundizan mucho, pero es importante conocerla.
- Similar al JOIN natural o condicional, pero incluye filas que no tienen coincidencias en la otra relación, rellenando con NULL donde no hay coincidencias.
- Tipos:
    * LEFT OUTER JOIN: Incluye todas las filas de la primera relación. `R1 ⊐⋈ R2`
    * RIGHT OUTER JOIN: Incluye todas las filas de la segunda relación. `R1 ⋈⊏ R2`
    * FULL OUTER JOIN: Incluye todas las filas de ambas relaciones. `R1 ⊐⋈⊏ R2`
- Ejemplo: <code>Empleados ⊐⋈ Departamentos</code> incluye todas las filas de Empleados, incluso si no tienen un departamento asociado.
- Equivalente SQL: `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`

### La División (/)
- Utilizada para consultas que involucran "para todos".
- Sintaxis: R1 / R2 donde `R1` es la relación dividida y `R2` es la relación divisor.
- Ejemplo: Si R1 tiene atributos (A, B) y R2 tiene atributo (B), entonces R1 / R2 devuelve los valores de A para los cuales existen todas las combinaciones de B en R2.

> [!TIP]
> Siempre que en el enunciado diga:
>
> **Todas las x** | **Cada x** | **Todo el x** →
> `DIVISIÓN (/)`<br>
> ¿Tiene 1? Sí, ¿Y el 2? Sí...
>
> **Ningún** | **Nadie** | **Nunca** →
> `RESTA (-)` <br>
> Total - (los que sí)
>
> **Solo hicieron x** | **Todos sus x son y** →
> `RESTA(-)`<br>
> Total - (los que hicieron algo distinto a x)
>
> **Al menos dos** | **Más de uno** →
> `AUTO-JOIN` <br>
> A ⨝<sub>A.x = B.x</sub> B
