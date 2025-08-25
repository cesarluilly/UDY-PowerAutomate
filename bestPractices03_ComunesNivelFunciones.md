¡Buenísimo! Aquí tienes un “cheat-sheet” de  **best practices a nivel interior** : expresiones, funciones y patrones que hacen los flujos más robustos, legibles y rápidos.

---

## 1) Nulabilidad & navegación segura

* **Usa `?` para navegar** por objetos que podrían venir vacíos:
  <pre class="overflow-visible!" data-start="281" data-end="348"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>body('HTTP_Get')?['data']?['customer']?['email']
  </span></code></div></div></pre>
* **Normaliza de inmediato** con `coalesce`:
  <pre class="overflow-visible!" data-start="396" data-end="654"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>coalesce(body('SQL')?['ResultSets']?['Table1'], createArray())     // arrays
  coalesce(body('HTTP')?['result'], json('{}'))                        // objetos
  coalesce(triggerOutputs()?['headers']?['x-id'], '')                  // string
  </span></code></div></div></pre>

---

## 2) Comparaciones seguras (strings, case, nulos)

* **Case-insensitive** :

<pre class="overflow-visible!" data-start="738" data-end="808"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>toLower(variables('var_env_str')) = toLower('prod')
  </span></code></div></div></pre>

* **String vacío vs null** :

<pre class="overflow-visible!" data-start="839" data-end="910"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>not(empty(coalesce(variables('var_token_str'), '')))
  </span></code></div></div></pre>

* **starts/endsWith** para extensiones:
  <pre class="overflow-visible!" data-start="953" data-end="1065"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>if(endsWith(toLower(item()?['Name']), '.pdf'), 'application/pdf', 'application/octet-stream')
  </span></code></div></div></pre>

---

## 3) Casting & validaciones de tipo

* **Casting explícito** evita sorpresas:
  <pre class="overflow-visible!" data-start="1152" data-end="1300"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>int('42')  float('3.14')  bool('true')  string(variables('var_id'))
  json(outputs('compose_rawJson'))   // parsea texto a objeto
  </span></code></div></div></pre>
* **`isFloat()`, `isInt()`, `isMatch()`** para validar:
  <pre class="overflow-visible!" data-start="1359" data-end="1441"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>isMatch(variables('var_rfc'), '^[A-ZÑ&]{3,4}\d{6}[A-Z0-9]{3}$')
  </span></code></div></div></pre>

---

## 4) Fechas & zonas horarias

* **Siempre en UTC para lógica** ; convierte solo para mostrar:

<pre class="overflow-visible!" data-start="1543" data-end="1734"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>utcNow()
  addDays(utcNow(), -1)
  formatDateTime(utcNow(), 'yyyy-MM-ddTHH:mm:ssZ')
  convertTimeZone(utcNow(), 'UTC', 'Central Standard Time (Mexico)', 'yyyy-MM-dd HH:mm')
  </span></code></div></div></pre>

* **Comparar fechas** : convierte a la **misma** zona y formato antes de comparar.

---

## 5) Arrays & objetos (sin bucles cuando se pueda)

* **Filtrar / mapear / transformar** :

<pre class="overflow-visible!" data-start="1915" data-end="2095"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>// Filter array (en acción Filter)
  @equals(item()?['Active'], true)

  // Select / Map
  { 
    'id': item()?['Id'],
    'email': toLower(item()?['Email'])
  }
  </span></code></div></div></pre>

* **Operaciones de conjunto** :

<pre class="overflow-visible!" data-start="2129" data-end="2256"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>union(arrayA, arrayB)          // elimina duplicados
  intersection(arrayA, arrayB)
  except(arrayA, arrayB)
  </span></code></div></div></pre>

* **Tomar rebanadas / extremos** :

<pre class="overflow-visible!" data-start="2293" data-end="2426"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>first(body('Select'))  last(variables('var_arr'))
  take(variables('var_arr'), 10)  skip(variables('var_arr'), 10)
  </span></code></div></div></pre>

---

## 6) Cadenas (higiene y formato)

* **Sanear espacios y comas** :

<pre class="overflow-visible!" data-start="2500" data-end="2552"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>trim(replace(myStr, '\r\n', ' '))
  </span></code></div></div></pre>

* **Join / Split** :

<pre class="overflow-visible!" data-start="2575" data-end="2682"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>join(select(outputs('select_Emails'), 'email'), ';')
  split(triggerBody()?['csv'], ',')
  </span></code></div></div></pre>

* **URL / Base64 / Binario** :

<pre class="overflow-visible!" data-start="2715" data-end="2865"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>encodeUriComponent('texto con espacios')
  base64(outputs('compose_bytes'))     
  base64ToBinary(triggerBody()?['fileBase64'])
  </span></code></div></div></pre>

---

## 7) Adjuntos & contenido binario

* **Estructura para APIs (p. ej., SendGrid)** :

<pre class="overflow-visible!" data-start="2956" data-end="3136"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-json"><span><span>{</span><span>
    </span><span>"content"</span><span>:</span><span></span><span>"@{item()?['ContentBytes']?['$content']}"</span><span>,</span><span>
    </span><span>"filename"</span><span>:</span><span></span><span>"@{item()?['Name']}"</span><span>,</span><span>
    </span><span>"type"</span><span>:</span><span></span><span>"application/pdf"</span><span>,</span><span>
    </span><span>"disposition"</span><span>:</span><span></span><span>"attachment"</span><span>
  </span><span>}</span><span>
  </span></span></code></div></div></pre>

* **Mapeo típico en `Select`** :

<pre class="overflow-visible!" data-start="3171" data-end="3309"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>{
    'Name': concat(item()?['SourceReference'], '.pdf'),
    'ContentBytes': base64ToBinary(item()?['StampedPDF'])
  }
  </span></code></div></div></pre>

---

## 8) Patrones dentro de `Select` / `Filter`

* **Usa el `item()` del propio Data Operation** ; si necesitas el item del `ForEach` externo:

<pre class="overflow-visible!" data-start="3456" data-end="3516"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>items('ForEach_Customers')?['CustomerId']
  </span></code></div></div></pre>

* **Evita variables dentro de Data Operations** (más limpio y seguro en paralelo).

---

## 9) Referencias entre acciones

* **Preferible** : `body('ActionName')?['prop']`
* **Alternativas** :

<pre class="overflow-visible!" data-start="3709" data-end="3919"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>outputs('ActionName')?['body']?['prop']
  actions('ActionName')?['outputs']?['headers']?['x-id']  // headers específicos
  triggerBody(), triggerOutputs(), workflow()             // metadatos
  </span></code></div></div></pre>

---

## 10) Control de flujo & errores

* **Short-circuit** con `if()` para valores:
  <pre class="overflow-visible!" data-start="4007" data-end="4085"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>if(greater(length(variables('var_arr')), 0), 'OK', 'EMPTY')
  </span></code></div></div></pre>
* **Guarda-falla** (guard clause) antes de un paso caro:
  <pre class="overflow-visible!" data-start="4145" data-end="4245"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>and(not(empty(variables('var_token_str'))), equals(variables('var_env'), 'prod'))
  </span></code></div></div></pre>
* **Scopes Try/Catch/Finally** + **Configure run after** (has failed / timed out).
* **Retry policy** en HTTP: Exponential con reintentos y timeouts razonables.

---

## 11) Performance

* **Evalúa una vez** (usa `Compose`) si repites una expresión larga.
* **Prefiere Data Operations** (`Select`, `Filter`, `Union`) sobre bucles con “Append to array”.
* **Filtra en la fuente** (SQL con WHERE, APIs con query params).
* Desactiva **concurrency** si modificas variables compartidas.

---

## 12) Variables vs Compose (decisión rápida)

* **`Compose`** : valor **inmutable** (snapshot/transformación) que reutilizas.
* **`Variable`** : valor **mutable / acumulable** (contadores, arrays, flags).
* Sufijos de tipo: `_str`, `_int`, `_bool`, `_obj`, `_arr`.

---

## 13) Plantillas de patrones útiles

**A. Array seguro para ForEach**

<pre class="overflow-visible!" data-start="5074" data-end="5151"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>coalesce(body('SQL')?['ResultSets']?['Table1'], createArray())
</span></code></div></div></pre>

**B. JSON seguro para `Parse JSON` posterior**

<pre class="overflow-visible!" data-start="5200" data-end="5260"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>coalesce(body('HTTP')?['result'], json('{}'))
</span></code></div></div></pre>

**C. Comparación de ambiente**

<pre class="overflow-visible!" data-start="5293" data-end="5357"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>equals(toLower(variables('var_env_str')), 'prod')
</span></code></div></div></pre>

**D. Validar que hay elementos**

<pre class="overflow-visible!" data-start="5392" data-end="5453"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>greater(length(variables('var_items_arr')), 0)
</span></code></div></div></pre>

**E. Construir cabecera Bearer**

<pre class="overflow-visible!" data-start="5488" data-end="5554"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>concat('Bearer ', variables('var_accessToken_str'))
</span></code></div></div></pre>

**F. Content-Type por extensión**

<pre class="overflow-visible!" data-start="5590" data-end="5769"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>if(endsWith(toLower(item()?['Name']), '.pdf'), 'application/pdf',
 if(endsWith(toLower(item()?['Name']), '.xml'), 'application/xml',
   'application/octet-stream'))
</span></code></div></div></pre>

---

## 14) Nombres consistentes (mini-guía)

* `sql_<Origen>_<Acción>` → `sql_hubFac_GetRFCs`
* `http_<Destino>_<Acción>` → `http_sendGrid_SendMail`
* `filter_<Fuente>_<Criterio>` → `filter_Customers_ByRFC`
* `select_<Fuente>_To_<Destino>` → `select_StampedPDFs_ToAttachments`
* `if_<QuéEvalúa>` → `if_RFCs_HasItems`
* `foreach_<Entidad>` → `foreach_Customers`
* `compose_<QuéHace>` → `compose_authHeader`
* `var_<nombre>_<tipo>` → `var_failedItems_arr`
