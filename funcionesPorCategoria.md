# Tabla rápida de funciones por categoría (con ejemplos)

**https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com**

> Los nombres y ejemplos son  **pegables** . Si te interesa una función no listada aquí, la encuentras en la referencia oficial (tiene TODAS). [Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Texto (strings)

| Función                              | Uso               | Ejemplo                                      | Resultado         |
| ------------------------------------- | ----------------- | -------------------------------------------- | ----------------- |
| `concat(a,b,...)`                   | Concatenar        | `concat('Hola ', variables('str_nombre'))` | `Hola Cesar`    |
| `substring(s, start, len)`          | Subcadena         | `substring('Factura-12345',8,5)`           | `12345`         |
| `length(s)`                         | Largo             | `length('ABC')`                            | `3`             |
| `replace(s, old, new)`              | Reemplazar        | `replace('A,B,C',',',';')`                 | `A;B;C`         |
| `toLower(s)`/`toUpper(s)`         | Min/Mayús        | `toUpper('cuprum')`                        | `CUPRUM`        |
| `trim(s)`                           | Quita espacios    | `trim('  hola  ')`                         | `hola`          |
| `startswith(s,p)`/`endswith(s,p)` | Prefijo/sufijo    | `endswith('file.pdf','.pdf')`              | `true`          |
| `split(s, sep)`                     | Dividir           | `split('a;b;c',';')`                       | `['a','b','c']` |
| `join(arr, sep)`                    | Unir              | `join(createArray('a','b'),'                 | ')`               |
| `guid()`                            | GUID              | `guid()`                                   | `xxxxxxxx-...`  |
| `formatNumber(n, fmt)`              | Formato numérico | `formatNumber(1234.5, '0,0.00')`           | `1,234.50`      |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Arrays / Colecciones

| Función                                             | Uso                    | Ejemplo                                      | Resultado                                     |
| ---------------------------------------------------- | ---------------------- | -------------------------------------------- | --------------------------------------------- |
| `createArray(...)`                                 | Crear array            | `createArray('a',1,true)`                  | `['a',1,true]`                              |
| `first(arr)`/`last(arr)`                         | Primer/último         | `first(body('Get_items')?['value'])`       | Obj 0                                         |
| `take(arr, n)`/`skip(arr, n)`                    | Tomar/saltar           | `take(arr,3)`                              | Primeros 3                                    |
| `length(arr)`                                      | Tamaño                | `length(variables('arr_items'))`           | `N`                                         |
| `contains(col, val)`                               | Contiene               | `contains(split('a;b',';'),'b')`           | `true`                                      |
| `intersection(a,b)`/`union(a,b)`/`except(a,b)` | Conjuntos              | `union(createArray(1,2),createArray(2,3))` | `[1,2,3]`                                   |
| `sort()`                                           | *(No existe nativa)* | —                                           | Usa servicios/Select + Order by en conectores |
| `empty(x)`                                         | Está vacío           | `empty(body('Get_items')?['value'])`       | `true/false`                                |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Objetos JSON

| Función                       | Uso                | Ejemplo                                    | Resultado          |
| ------------------------------ | ------------------ | ------------------------------------------ | ------------------ |
| `json(s)`                    | Texto → objeto    | `json('{ "id": 1 }')?['id']`             | `1`              |
| `setProperty(obj, key, val)` | Agregar/actualizar | `setProperty(item(),'Status','OK')`      | Obj con `Status` |
| `getProperty(obj, key)`      | Obtener prop       | `getProperty(triggerBody(),'AccountId')` | Valor              |
| `removeProperty(obj,key)`    | Quitar prop        | `removeProperty(item(),'secret')`        | Obj sin `secret` |
| `coalesce(a,b,...)`          | Primer no-nulo     | `coalesce(body('X')?['y'], 'N/A')`       | Valor o `N/A`    |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Lógica / Comparación

| Función                                      | Uso      | Ejemplo                                  |
| --------------------------------------------- | -------- | ---------------------------------------- |
| `equals(a,b)`                               | Igualdad | `equals(variables('str_env'),'PROD')`  |
| `if(cond, a, b)`                            | Ternario | `if(empty(arr), 'Vacio', 'Con datos')` |
| `and(a,b,...)`/`or(a,b,...)`/`not(a)`   | Lógicas | `and(greater(n,0), less(n,10))`        |
| `greater/less/greaterOrEquals/lessOrEquals` | Comparar | `greater(length(arr),0)`               |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Fecha y hora

| Función                                         | Uso                    | Ejemplo                                                              | Resultado           |
| ------------------------------------------------ | ---------------------- | -------------------------------------------------------------------- | ------------------- |
| `utcNow()`                                     | Fecha/hora UTC         | `utcNow()`                                                         | `2025-08-21T...Z` |
| `formatDateTime(d, fmt)`                       | Formato                | `formatDateTime(utcNow(),'yyyy-MM-dd')`                            | `2025-08-21`      |
| `addDays/Hours/Minutes/Seconds(d, n, fmt?)`    | Sumar                  | `addDays(utcNow(),3,'yyyy-MM-dd')`                                 | `+3 días`        |
| `addToTime(d, n, 'Hour')`/`subtractFromTime` | Sumar/restar genérico | `addToTime(utcNow(),2,'Hour')`                                     | `+2h`             |
| `convertTimeZone(d, from, to, fmt?)`           | Zona horaria           | `convertTimeZone(utcNow(),'UTC','Central Standard Time (Mexico)')` | Hora CDMX           |
| `dayOfMonth/dayOfWeek/dayOfYear/month/year`    | Partes                 | `dayOfWeek(utcNow())`                                              | `0..6`            |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Conversión / Tipos

| Función                                                                | Uso                                  | Ejemplo                                         |
| ----------------------------------------------------------------------- | ------------------------------------ | ----------------------------------------------- |
| `string(x)`/`int(x)`/`float(x)`/`bool(x)`                       | Casteo                               | `int('5')`                                    |
| `base64(x)`/`base64ToString(x)`/`binary(x)`/`base64ToBinary(x)` | Base64/binario                       | `base64ToBinary(variables('str_pdfBase64'))`  |
| `dataUriToBinary(x)`                                                  | Data URI → binario                  | `dataUriToBinary(outputs('Compose_DataUri'))` |
| **Tip**Data URI → Base64                                         | `base64(dataUriToBinary(dataUri))` | Útil para adjuntos.                            |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)[Encodian](https://www.encodian.com/blog/convert-a-data-uri-to-base64-or-file-contents-in-power-automate/?utm_source=chatgpt.com)[Microsoft Dynamics 365 Community](https://community.dynamics.com/blogs/post/?postid=7d132f60-08ce-4433-be65-7241314c62a2&utm_source=chatgpt.com)

## URI / Web

| Función                                            | Uso                  | Ejemplo                                                  |
| --------------------------------------------------- | -------------------- | -------------------------------------------------------- |
| `encodeUriComponent(s)`/`decodeUriComponent(s)` | Escapar/descodificar | `encodeUriComponent('a b')`→`a%20b`                 |
| `uriComponent(s)`                                 | A URI-safe           | `uriComponent('a&b')`                                  |
| `uriHost/uriPath/uriQuery/uriPort/uriScheme`      | Partes de URL        | `uriHost('https://api.cu.com/v1?a=1')`→`api.cu.com` |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Matemáticas

| Función                          | Uso           | Ejemplo                         |
| --------------------------------- | ------------- | ------------------------------- |
| `add/sub/mul/div/mod(n1,n2)`    | Aritmética   | `add(5,3)`→`8`             |
| `min(a,b,...)`/`max(a,b,...)` | Extremos      | `max(10,3,7)`→`10`         |
| `range(start, count)`           | Rango (array) | `range(1,5)`→`[1,2,3,4,5]` |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

## Workflow / Metadatos

| Función                                                 | Uso                               | Ejemplo                                                  |
| -------------------------------------------------------- | --------------------------------- | -------------------------------------------------------- |
| `workflow()`                                           | Info del flujo (id, nombre, etc.) | `workflow()?['name']`                                  |
| `actionOutputs('Accion')` *(alias de `outputs()`)* | Outputs de acción                | `actionOutputs('HTTP_Send')?['statusCode']`            |
| `triggerOutputs()/triggerBody()`                       | Datos del trigger                 | `triggerOutputs()?['headers']`/`triggerBody()?['x']` |

[Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)

---

# Patrones esenciales (listos para pegar)

* **Ruta segura con null**

  `body('Get_items')?['value']?[0]?['Customer']?['Name']`
* **Array seguro**

  `coalesce(body('Paso')?['value'], createArray())`
* **Validar que `contacts` tenga elementos**

  `greater(length(first(body('Filter_array'))?['contacts']), 0)`
* **Elegir valor no nulo**

  `coalesce(variables('str_rfc'), triggerBody()?['RFC'], 'SIN-RFC')`
* **Solo ejecutar en hh:00 por tolerancia del trigger**

  `equals(minute(utcNow()), 0)`

---

## Dónde leer “todos-todos” con más ejemplos

* **Funciones (todas, por categoría)** : referencia oficial WDL (la que usa Power Automate). [Microsoft Learn](https://learn.microsoft.com/en-us/azure/logic-apps/expression-functions-reference?utm_source=chatgpt.com)
* **Cómo usar expresiones en condiciones / bucles** (Apply to each, Condition): guía de Power Automate. [Microsoft Learn**+1**](https://learn.microsoft.com/en-us/power-automate/use-expressions-in-conditions?utm_source=chatgpt.com)
* **`item()` vs `items('…')`** explicado con capturas. [Ellis Karim&#39;s Blog](https://elliskarim.com/2021/07/14/item-v-itemsapply_to_each/?utm_source=chatgpt.com)
* **`triggerBody()` vs `triggerOutputs()`** (leer body vs headers+body). [D365 Demystified**+1**](https://d365demystified.com/tag/triggeroutputs/?utm_source=chatgpt.com)
* **Base64/Data URI binario** (cuando tratas archivos). [Encodian](https://www.encodian.com/blog/convert-a-data-uri-to-base64-or-file-contents-in-power-automate/?utm_source=chatgpt.com)[Microsoft Dynamics 365 Community](https://community.dynamics.com/blogs/post/?postid=7d132f60-08ce-4433-be65-7241314c62a2&utm_source=chatgpt.com)


# Coalesce


Perfecto 👍, lo que estás viendo en tu acción **ForEachUpdateSentFiles** es un **bucle For each** en Power Automate que está configurado con esta expresión:

<pre class="overflow-visible!" data-start="160" data-end="267"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-plaintext"><span><span>coalesce(
   body('GetRFCsFromSP_AllFields')?['ResultSets']?['Table1'],
   createArray()
)
</span></span></code></div></div></pre>

### 🔎 Explicación paso a paso

1. **`body('GetRFCsFromSP_AllFields')?['ResultSets']?['Table1']`**

   * Aquí se está tomando la salida del action `GetRFCsFromSP_AllFields` (que parece ser una consulta SQL ejecutando un stored procedure).
   * Dentro del body, busca la propiedad `ResultSets → Table1`, que típicamente es el array de resultados devuelto por el SP.
2. **`coalesce(..., createArray())`**

   * `coalesce` devuelve el primer valor que  **no sea nulo o vacío** .
   * Si el SP  **sí devuelve registros** , entonces tomará directamente ese array.
   * Si el SP devuelve `null` (por ejemplo, si no hay resultados), entonces en lugar de romper el flujo, se fuerza a que el `ForEach` reciba un **array vacío** con `createArray()`.

   👉 Esto asegura que tu flujo **no falle por null** y siempre tenga un array válido para iterar.

---

### 🚀 En términos simples

El `ForEachUpdateSentFiles` está **recorriendo todos los resultados devueltos por tu SP** (`Table1`).

* Si hay resultados → los itera.
* Si no hay resultados → simplemente no hace nada, porque iterará sobre un array vacío.

---

### ✅ Ejemplo ilustrativo

Supongamos que el SP devuelve esta salida:

<pre class="overflow-visible!" data-start="1453" data-end="1631"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-json"><span><span>{</span><span>
  </span><span>"ResultSets"</span><span>:</span><span></span><span>{</span><span>
    </span><span>"Table1"</span><span>:</span><span></span><span>[</span><span>
      </span><span>{</span><span></span><span>"RFC"</span><span>:</span><span></span><span>"ABC123"</span><span>,</span><span></span><span>"Email"</span><span>:</span><span></span><span>"cliente1@dominio.com"</span><span></span><span>}</span><span>,</span><span>
      </span><span>{</span><span></span><span>"RFC"</span><span>:</span><span></span><span>"DEF456"</span><span>,</span><span></span><span>"Email"</span><span>:</span><span></span><span>"cliente2@dominio.com"</span><span></span><span>}</span><span>
    </span><span>]</span><span>
  </span><span>}</span><span>
</span><span>}</span><span>
</span></span></code></div></div></pre>

👉 El ForEach va a iterar 2 veces: una por cada objeto.

Si en cambio el SP no devuelve nada:

<pre class="overflow-visible!" data-start="1728" data-end="1784"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-json"><span><span>{</span><span>
  </span><span>"ResultSets"</span><span>:</span><span></span><span>{</span><span>
    </span><span>"Table1"</span><span>:</span><span></span><span>null</span><span>
  </span><span>}</span><span>
</span><span>}</span><span>
</span></span></code></div></div></pre>

👉 Gracias al `coalesce(..., createArray())`, en lugar de romperse, el bucle simplemente recibe un `[]` (array vacío) y  **no ejecuta ninguna iteración** .

---

📌 **Best practice para nombrar este tipo de For Each**

Como está iterando sobre los registros de SP para actualizar archivos enviados, un buen nombre sería:

`ForEach_HubFac_UpdateSentFiles`

ó más claro aún:

`ForEach_UpdateSentFiles_FromRFCs`


# Consumir datos desde una fuente externa, que hacer?

¡Gran pregunta! Cuando un flujo **consume datos externos** (HTTP, SQL, SharePoint, etc.), lo más importante es  **que nunca te “reviente” por nulos** , que el  **formato sea predecible** , y que puedas **depurar** rápido. Aquí van mis **best practices** (con ejemplos listos para pegar):

---

## 1) Normaliza SIEMPRE la forma de los datos

### Arrays (listas)

* **Patrón** : si puede venir `null`, usa `coalesce(..., createArray())`.

<pre class="overflow-visible!" data-start="430" data-end="539"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>coalesce(
  body('GetRFCsFromSP_AllFields')?['ResultSets']?['Table1'],
  createArray()
)
</span></code></div></div></pre>

* ✔️ Garantiza que un `For each` nunca falle por `null` (iterará 0 veces).

### Objetos (JSON)

* **Patrón** : si puede venir `null`, usa `coalesce(..., json('{}'))`.

<pre class="overflow-visible!" data-start="705" data-end="793"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>coalesce(
  body('http_GetCustomerById')?['result'],
  json('{}')
)
</span></code></div></div></pre>

* ✔️ Así podrás hacer `?['prop']` sin que explote.

### Cadenas / Números / Bools

<pre class="overflow-visible!" data-start="876" data-end="1017"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>coalesce(body('http_X')?['name'], '')
coalesce(body('http_X')?['count'], 0)
coalesce(body('http_X')?['isActive'], false)
</span></code></div></div></pre>

> ⚠️ No metas todo en variables “por costumbre”. Usa **variables** solo si reutilizarás el dato varias veces o necesitas mutarlo. Para lecturas puntuales, usa **Compose** o referencia directa.

---

## 2) “Fuerza” el esquema temprano con **Parse JSON**

* Evita trabajar con “cualquier” JSON. **Parse JSON** te da **intellisense** y valida estructura.
* Flujo típico:
  1. HTTP/SQL
  2. **Parse JSON** (pega un ejemplo real para generar el  **schema** )
  3. **Select/Filter** o **For each** con campos tipados.

<pre class="overflow-visible!" data-start="1533" data-end="1770"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-json"><span><span>// Ejemplo de schema (Parse JSON)</span><span>
</span><span>{</span><span>
  </span><span>"type"</span><span>:</span><span></span><span>"object"</span><span>,</span><span>
  </span><span>"properties"</span><span>:</span><span></span><span>{</span><span>
    </span><span>"data"</span><span>:</span><span></span><span>{</span><span>
      </span><span>"type"</span><span>:</span><span></span><span>"array"</span><span>,</span><span>
      </span><span>"items"</span><span>:</span><span></span><span>{</span><span></span><span>"type"</span><span>:</span><span></span><span>"object"</span><span>,</span><span></span><span>"properties"</span><span>:</span><span></span><span>{</span><span></span><span>"id"</span><span>:</span><span>{</span><span>"type"</span><span>:</span><span>"string"</span><span>}</span><span>,</span><span></span><span>"rfc"</span><span>:</span><span>{</span><span>"type"</span><span>:</span><span>"string"</span><span>}</span><span></span><span>}</span><span></span><span>}</span><span>
    </span><span>}</span><span>
  </span><span>}</span><span>
</span><span>}</span><span>
</span></span></code></div></div></pre>

Luego:

<pre class="overflow-visible!" data-start="1778" data-end="1851"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>coalesce(body('Parse_JSON')?['data'], createArray())
</span></code></div></div></pre>

---

## 3) Usa **navegación segura** siempre

* `?['prop']` en vez de `['prop']`.

<pre class="overflow-visible!" data-start="1934" data-end="2005"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>body('http_Get')?['result']?['customer']?['email']
</span></code></div></div></pre>

* ✔️ Si falta una rama, devuelve `null`, no rompe el flujo.

---

## 4) Valida antes de actuar (guard clauses)

* **Arrays** :

<pre class="overflow-visible!" data-start="2131" data-end="2194"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>length(variables('var_customers_arr')) > 0
</span></code></div></div></pre>

* **Texto** :

<pre class="overflow-visible!" data-start="2208" data-end="2267"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerautomate"><span>not(empty(variables('var_token_str')))
</span></code></div></div></pre>

---

## 5) Nombrado + estructuras recomendadas

* **Variables** :

  `var_customers_arr`, `var_customerCurrent_obj`, `var_accessToken_str`, `var_retryCount_int`

* **Data Operations** :

  `select_PDFs_ToAttachments`, `filter_Customers_ByRFC`, `compose_BuildAuthHeader`

* **Control** :

  `if_Customers_HasItems`, `foreach_Customers`, `scope_Try`, `scope_Catch`, `scope_Finally`

---

## 6) Manejo de errores (Try/Catch/Finally)

Usa  **Scopes** :

* `scope_Try` → tus acciones
* `scope_Catch` → condición “has failed” y registra error / notifica
* `scope_Finally` → limpieza / cierre

Y en acciones HTTP ajusta **Retry policy** (Settings):

* Exponential, 4 intentos (o lo que corresponda por API).

---

## 7) Seguridad

* No guardes secretos en variables visibles.
* Activa **Secure inputs/outputs** en pasos sensibles (Settings).
* Usa **Variables de Entorno** (en soluciones) para URLs, claves de API (ideal en Azure Key Vault si aplica).

---

## 8) Rendimiento

* Prefiere **Select/Filter/Join** sobre loops cuando puedas.
* Evita variables dentro de bucles si puedes calcular con  **Compose** .
* Usa **coalesce** + **createArray/json('{}')** para evitar ramas extra de control.

---

## 9) Ejemplo completo (patrón recomendado)

<pre class="overflow-visible!" data-start="3500" data-end="3970"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>http_GetRFCs          --(HTTP/SQL)
  ↓
parse_RFCs            -- Parse JSON con schema
  ↓
set_var_rfcList_arr   -- value: coalesce(body('parse_RFCs')?['ResultSets']?['Table1'], createArray())
  ↓
if_RFCs_HasItems      -- condition: length(variables('var_rfcList_arr')) > 0
  ├─ True:
  │    foreach_RFC in var_rfcList_arr
  │       sql_GetFilesByRFC
  │       select_Files_ToAttachments
  │       http_SendGrid_SendEmail
  └─ False:
       compose_Log_NoData
</span></span></code></div></div></pre>

---

## 10) ¿Uso siempre `coalesce` → variable de objeto?

* **No siempre.** Úsalo cuando:
  * El origen puede venir `null` **y** lo vas a **iterar** o **acceder** varias veces.
  * Quieres **normalizar** una vez (y reusar)
* Si solo vas a usarlo  **1 vez** , puedes meter el `coalesce(...)` **directo** en el `From` del `For each` o `Select` (como ya hiciste), sin crear variable.

---

### TL;DR

* **Normaliza** con `coalesce` a `createArray()` / `json('{}')`.
* **Parse JSON** temprano.
* **Navegación segura** `?['x']`.
* **Valida** con `length()` / `empty()`.
* **Try/Catch** con Scopes + Retry.
* **Variables** solo cuando suman (si no, **Compose** o referencia directa).
* **Config** por  **Variables de Entorno** .

Si me cuentas un endpoint/SQL concreto que usas, te lo dejo armado con estos patrones en 4–5 pasos para que lo copies tal cual.
