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

**a) Forma canónica**
- [ ] **Derecha simple**, rompe cualquier dependencia funcional con derecha compuesta.
    * `Ej: A → B, C` se convierte en `A → B` y `A → C`

**b) Atributos extraños**
- [ ] **Izquierda limpia**, elimina atributos a la izquierda que sobran.
    * `Ej: F = {A, B → C}`. 
    * Escribir: ¿ $C \in \{B\}^+_F$ ? (para cada tributo que comprobemos)
    * Calcula $\{B\}^+_F$.
    * **Si** (¿Existe C en el cierre transitivo de B?):
        * Elimina el atributo sobrante.
        * Es extraño
    * **Si no**:
        * Mantén ambos atributos.
        * No es extraño
    * Comprobar para todos los que sean extraños
    * Para los que no lo son, que se ven a simple vista, con comprobarlo para un par sobra.

**c) Reglas que sobran**
- [ ] **Elimina reglas redundantes** que se deduzcan por transitividad.
    * Escribir: ¿i  es redundante?
    * Escribir: G = F - i (siendo i (X → Y) la regla que estamos comprobando si es redundante)
    * Calcular con las reglas que nos quedan el cierre transitivo de X sobre G 
      * `Ej: Si tenemos F = {A → B; B → C; A → C}` 
      * `¿3 es redundante?`
      * `G = F - A → C`
      * $\{A\}^+_G$ `= {A, B, C}`
      * Por tanto 3 es redundante

## Paso 2 - Crear las Tablas (Síntesis)

- [ ] **Crear una tabla por d.f.**
    * `Ej: Si tenemos F = {A → B; B → C; A,C → D}`
    * `Clave candidata: A`
    * `Tendremos 3 tablas`
- [ ] **Definir la tabla**:
    * Los atributos de la tabla son: `{Determinante + Determinado}`
    * La clave primaria es el **determinante** (lado izquierdo).

## Paso 3 - Asegurar la Clave Global

- [ ] **Revisa las claves candidatas** que calculaste al principio:
    * **Si** (¿Hay alguna tabla que contenga TODOS los atributos de esa clave juntos?):
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

> [!TIP]
> Siempre que pida "Restricciones de Integridad de Entidad" → piensa en **Claves Primarias**.
> 
> Siempre que pida "Restricciones de Integridad Referencial" → piensa en **Claves Foráneas**.
