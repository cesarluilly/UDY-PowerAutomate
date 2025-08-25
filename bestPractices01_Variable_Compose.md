¡Gran tema! En Power Automate **Compose** y **Variables** parecen similares (ambos “guardan” valores), pero sirven para cosas distintas. Aquí tienes una guía práctica con criterios claros, ejemplos y buenas prácticas.

---

# ¿Qué es cada cosa?

* **Compose** : calcula y “congela” un valor  **una sola vez** . No cambia. Ideal para **transformar/capturar** datos y **reutilizarlos** después sin repetir expresiones.
* **Variable** : almacena un valor que **puede cambiar** a lo largo del flujo (Set/Increment/Append). Sirve para **estado acumulado** o **datos que se usan/modifican** en varios pasos.

---

## Cuándo usar **Compose** (y por qué)

Úsalo cuando necesites:

1. **Transformar** datos y reutilizar el resultado

   * Evitas repetir expresiones largas en varios pasos.

   <pre class="overflow-visible!" data-start="781" data-end="859"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>compose_formatDate  = formatDateTime(utcNow(), 'yyyy-MM-dd')
   </span></span></code></div></div></pre>

   Luego usas: `outputs('compose_formatDate')`
2. **Extraer** un campo de una salida compleja

   <pre class="overflow-visible!" data-start="958" data-end="1033"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>compose_customerEmail = body('httpGetCustomer')?['email']
   </span></span></code></div></div></pre>
3. **“Congelar”** un valor en el tiempo

   (ej. la hora de inicio del flujo, un token antes de modificar encabezados).

   <pre class="overflow-visible!" data-start="1159" data-end="1205"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>compose_startedAt = utcNow()
   </span></span></code></div></div></pre>
4. **Evitar variables innecesarias** para valores que no cambian

   (más simple, menos riesgo con paralelismo).

 **Ventajas** : simple, inmutable, cero riesgos de concurrencia, perfecto para “caching” de expresiones.

---

## Cuándo usar **Variables** (y por qué)

Úsalas cuando necesites:

1. **Acumular o modificar** valores en el tiempo

   * Contadores, banderas, concatenaciones, acumulación en arreglos.

   <pre class="overflow-visible!" data-start="1622" data-end="1853"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>var_count_int        (Initialize = 0)         → Increment variable
   var_log_str          (Initialize = '')        → Set/Append a string
   var_files_arr        (Initialize = [])        → Append to array variable
   </span></span></code></div></div></pre>
2. **Estado** que cambia en un loop o varias ramas

   * Ej. recolectar errores, construir una lista, llevar un total.
3. **Reutilizar muchas veces** y **modificar** entre pasos

   * Ej. `var_accessToken_str` tras renovar token.

 **Ojo** : variables y paralelismo no se llevan bien; si usas **concurrency** en un `Apply to each`, evita modificar la misma variable desde iteraciones concurrentes.

---

## Comparativa rápida

| Aspecto               | **Compose**                              | **Variable**                        |
| --------------------- | ---------------------------------------------- | ----------------------------------------- |
| Mutabilidad           | Inmutable (no se cambia)                       | Mutable (Set, Increment, Append)          |
| Uso típico           | Calcular/extraer un valor una vez y reutilizar | Estado acumulado, contadores, colecciones |
| En loops concurrentes | Seguro (no cambia)                             | Riesgo de condiciones de carrera          |
| Inicialización       | No requiere Initialize                         | Requiere “Initialize variable”          |
| Legibilidad           | Buena para “snapshot” de expresiones         | Buena para lógica de estado              |
| Rendimiento           | Menos pasos si solo necesitas leer             | Más pasos (Initialize + Sets)            |

---

## Ejemplos típicos

### 1) “Cachear” expresiones (mejor con  **Compose** )

<pre class="overflow-visible!" data-start="2933" data-end="3153"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>compose_baseUrl = variables('var_env_apiUrl')           // o environment()
compose_auth    = concat('Bearer ', variables('var_accessToken_str'))

// Reutilizas:
headers.Authorization = outputs('compose_auth')
</span></span></code></div></div></pre>

### 2) Contador en un loop (necesitas  **Variable** )

<pre class="overflow-visible!" data-start="3207" data-end="3329"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>var_processed_int = 0   // Initialize

// Dentro del Apply to each:
Increment variable: var_processed_int by 1
</span></span></code></div></div></pre>

### 3) Construir una lista (mejor  **Append to array variable** )

<pre class="overflow-visible!" data-start="3395" data-end="3551"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>var_failedItems_arr = []

// En cada error:
Append to array variable: var_failedItems_arr ← { id: item()?['Id'], error: outputs('scope_Catch') }
</span></span></code></div></div></pre>

### 4) Preparar adjuntos (evita variable; usa **Select** + Compose si aplica)

<pre class="overflow-visible!" data-start="3631" data-end="3784"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>select_PDFs_ToAttachments:
  Name         = concat(item()?['SourceReference'], '.pdf')
  ContentBytes = base64ToBinary(item()?['StampedPDF'])
</span></span></code></div></div></pre>

> No necesitas variable si no vas a mutar.

### 5) Tomar dato del ForEach externo dentro de un Select

<pre class="overflow-visible!" data-start="3887" data-end="3997"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>// En el map del Select:
customerId = items('ForEach_Customers')?['CustomerId']  // ¡sin variable!
</span></span></code></div></div></pre>

---

## Nomenclatura recomendada

* **Compose** : `compose_<QuéHace>`
* `compose_extractToken`, `compose_formatDate`, `compose_baseUrl`
* **Variables** : `var_<nombre>_<tipo>`
* `var_accessToken_str`, `var_count_int`, `var_errors_arr`, `var_customer_obj`

Tipos sugeridos: `_str`, `_int`, `_bool`, `_obj`, `_arr`.

---

## Antipatrones (evita)

* Crear variables para todo “por costumbre”.

  → Si el valor  **no cambia** , **Compose** es mejor.
* Modificar la **misma variable** en loops con concurrencia activada.

  → Usa **Select/Filter** (Data Operations) o desactiva la concurrencia.
* Guardar **secretos** en variables visibles.

  → Usa **Secure inputs/outputs** y/o **environment variables** / Key Vault.

---

## Decisión express (árbol mental)

1. **¿El valor cambiará más de una vez?**
   * Sí → **Variable**
   * No → **Compose**
2. **¿Necesitas una colección que crecerá?**
   * Sí → **Variable array** (`Append to array`)
   * No → `Select/Union` y guarda el resultado en **Compose**
3. **¿Se usará en paralelo?**
   * Sí → evita Variables mutables; prefiere **Compose** o **Data Operations**

---

## Mini patrón recomendado

* Scopes: `scope_Try` / `scope_Catch` / `scope_Finally`
* Compose para snapshot/transformaciones:
  * `compose_now`, `compose_authHeader`, `compose_customerEmail`
* Variable para estado:
  * `var_retryCount_int`, `var_failed_arr`, `var_total_int`
