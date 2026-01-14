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