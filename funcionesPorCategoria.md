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
