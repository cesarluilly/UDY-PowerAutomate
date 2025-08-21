# Compose VS Variable


Perfecto 👌 aquí tienes una  **tabla comparativa entre *Compose* y *Variables* en Power Automate** , con ejemplos prácticos para que la uses como guía rápida:

---

## 📊 Tabla de comparación: *Compose* vs *Variables*

| Característica              | **Compose**                                                                                     | **Variable**                                                                           |
| ---------------------------- | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Propósito**         | Guardar un valor temporal o constante que no cambia                                                   | Guardar un valor que se puede modificar a lo largo del flujo                                 |
| **Inicialización**    | Se crea directamente en la acción*Compose*con el valor inicial                                     | Se crea con*Initialize variable*(tipo + valor inicial)                                     |
| **Modificación**      | ❌ No se puede modificar después de asignarlo                                                        | ✅ Se puede cambiar con*Set variable* ,*Increment variable*o*Append to array variable* |
| **Alcance**            | Solo sirve como referencia de un valor calculado o transformado                                       | Sirve para valores dinámicos que cambian durante la ejecución                              |
| **Legibilidad**        | Ideal para simplificar expresiones largas o mejorar la claridad del flujo                             | Ideal para almacenar datos intermedios que cambian paso a paso                               |
| **Uso en expresiones** | `outputs('Compose_MiValor')`                                                                        | `variables('str_nombre')`                                                                  |
| **Rendimiento**        | Ligeramente más rápido porque no mantiene estado                                                    | Puede impactar si hay muchas operaciones de escritura sobre la misma variable                |
| **Casos típicos**     | - Guardar concatenaciones``- Dar formato a fechas``- Depuración de valores intermedios | - Contadores``- Listas de IDs recolectados``- Flags (booleanos) de control     |

---

## ✅ Ejemplos prácticos

### Ejemplo con *Compose*

* **Escenario** : Tienes un correo con fecha en formato UTC y quieres mostrarlo como `dd/MM/yyyy`.
* Acción:
  * *Compose* : `formatDateTime(triggerOutputs()?['headers']['x-ms-file-last-modified'],'dd/MM/yyyy')`
* Resultado: Queda guardado en `outputs('Compose_FormatDate')`.

---

### Ejemplo con *Variable*

* **Escenario** : Quieres contar cuántos correos cumplen una condición.
* Acciones:
  * *Initialize variable* : `int_counter = 0`
  * Dentro del `Apply to each`: *Increment variable* `int_counter` en `1`.
* Resultado: Al final del flujo, `variables('int_counter')` trae el total.

---

👉 **Regla rápida de decisión:**

* Si solo necesitas un valor calculado y fijo → usa  *Compose* .
* Si necesitas cambiar el valor varias veces → usa  *Variable* .
