# Calcular Claves Candidatas

- [ ] **Identificar atributos** que SOLO aparecen en la izquierda (huerfanos) de las dependencias funcionales, son **OBLIGATORIOS** en la clave candidata.
- [ ] **Calcular el cierre (`X+`)** de los atributos identificados:
    * **Si** (El cierre incluye todos los atributos de la relación):
        * Los atributos huerfanos forman **LA** clave candidata.
    * **Si no**:
        * Prueba a añadir atributos que no hayas alcanzado en el cierre, uno a uno, y calcula el cierre de nuevo.
        * Repite hasta que el cierre incluya todos los atributos de la relación.
        * Todas las combinaciones posibles de atributos que cumplen esta condición son claves candidatas, siempre que no contengan atributos redundantes.
- [ ] **Verificar minimalidad**

# Justificar en que forma normal se encuentra una tabla
- [ ] **Tercera Forma Normal (3FN)**:
    * Una relacion R con un conjunto D.F. está en 3FN si siempre que una D.F. `X → Y` cumple que:
        * X es una superclave (super conjunto de clave candidata), o
        * Y es un atributo básico (forma parte de alguna clave candidata).
* [ ] **Forma normal de Boyce-Codd (FNBC)**:
    * Una relación R con un conjunto D.F. está en FNBC si siempre que una D.F. `X → Y` cumple que:
        * X es una superclave (super conjunto de clave candidata).
  
> [!TIP] 
> Si una tabla está en FNBC, entonces también está en 3FN.
> 
> `FNBC ⟹ 3FN`
>
> Si una tabla NO está en 3FN, entonces tampoco está en FNBC.
>
> `¬3FN ⟹ ¬FNBC`

# Normalizar a 3FN (Algoritmo de Síntesis)

## Paso 1 - El Recubrimiento Minimal

**(Forma canónica)**
- [ ] **Derecha simple**, rompe cualquier dependencia funcional con derecha compuesta.
    * `Ej: A → B, C` se convierte en `A → B` y `A → C`

**(Atributos extraños)**
- [ ] **Izquierda limpia**, elimina atributos a la izquierda que sobran.
    * `Ej: A, B → C`. Calcula el cierre de A (`A+`).
    * **Si** (¿Se puede llegar a C sin B? ¿Y viceversa?):
        * Elimina el atributo sobrante.
    * **Si no**:
        * Mantén ambos atributos.

**(Reglas que sobran)**
- [ ] **Elimina reglas redundantes** que se deduzcan por transitividad.
    * `Ej: Si tenemos A → B; B → C; A → C` entonces `A → C` es redundante y se elimina.

## Paso 2 - Crear las Tablas (Síntesis)

- [ ] **Junta todas las reglas** que tengan el mismo lado izquierdo.
    * Cada conjunto de reglas con el mismo lado izquierdo formará una tabla.
    * *Extra:* Si tienes una equivalencia `A <→ B`, juntalas en la misma tabla.
- [ ] **Definir la tabla**:
    * Los atributos de la tabla son: `{Determinante + Todos los de la derecha}`
    * La clave primaria es el **determinante** (lado izquierdo).

## Paso 3 - Asegurar la Clave Global

- [ ] **Revisa las claves candidatas** que calculaste al principio:
    * **Si** (¿Hay alguna tabla que contenga TODOS los atribuytos de esa clave jutnos?):
        * **FIN**, la base de datos está en 3FN.
    * **Si no**:
        * Crea una tabla nueva extra que solo contenga los atributos de la clave candidata.

# Normalizar a FNBC (Algoritmo de Descomposición)
## Paso 0 - Preparación
- [ ] **Mete tu tabla en la lista de resultados.**
    * Al principio, tu lista de tables es solo la tabla original gigante.
    * Estado inicial: `List<Tablas> Resultado = { Tabla_Original }`

## Paso 1 - Identificar el culpable
- [ ] **Revisa cada tabla** que tengas actualmente en tu lista `Resultado`.
- [ ] **Pregunta:** ¿Hay alguna tabla que viole la FNBC?
    * Esto sucede si encuentras una dependencia funcional `X → Y` donde `X` NO es superclave en esa tabla.
- [ ] **Decision:**
    * **No hay tablas que violen la FNBC**:
        * **FIN**, todas las tablas en `Resultado` están en FNBC.
    * **Hay tablas que violen la FNBC**:
        * Escoge una tabla que viole la FNBC y una dependencia funcional `X → Y` que la viole y seguimos al Paso 2.

## Paso 2 - El Corte (Descomposición)
Ahora mismo tienes una tabla <code>R<sub>i</sub></code> y una dependencia "mala" <code>α → β</code> (donde α es el determinante y β lo determinado).
- [ ] **Elimina la tabla original R<sub>i</sub>** de la lista `Resultado`.
- [ ] **Crea la Tabla 1 (La del problema):**
    * Atributos: `{α} ∪ {β}`
    * Clave primaria: `α`
    * Añádela a la lista `Resultado`.
- [ ] **Crea la Tabla 2 (El resto):**
    * Atributos: Todos los de la tabla original R<sub>i</sub> menos los de β.
    * Clave primaria: La clave primaria original de R<sub>i</sub> (puedes recalcularla si tienes dudas).
    * Añádela a la lista `Resultado`.

## Paso 3 - Vuelta a empezar
- [ ] **Vuelve al Paso 1.**
    * Ahora tienes más tablas en tu lista. Tienes que comprobar si las *nuevas* tablas cumplen **FNBC**. A veces, al romper una tabla, surge un problema nuevo en los trozos resultantes, por eso hay que volver a comprobar.


# Retricciones de Integridad Referencial
Básicamente te pregunta: "¿Qué columnas son flechas que apuntan a otra tabla?".

- [ ] **Identificar claves foráneas**:
    * Revisa las relaciones entre tablas.
    * Identifica las columnas que hacen referencia a claves primarias en otras tablas.