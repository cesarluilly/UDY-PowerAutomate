Muy buena pregunta 👌.

En Power Automate, los **Compose** no convierten todo a string automáticamente; el tipo de dato que sale depende de la  **expresión que pongas dentro** .

---

## 🔎 Regla general

* **Compose** es un “contenedor” neutro → puede devolver  **string, número, booleano, array, objeto o fecha** .
* Lo que ves en la UI a veces parece string (porque todo se muestra como texto), pero internamente conserva el tipo de dato.

---

## 📌 Ejemplos

### **String**

<pre class="overflow-visible!" data-start="479" data-end="518"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>concat('Hola ', 'Mundo')
</span></code></div></div></pre>

Salida → `"Hola Mundo"` (tipo string)

---

### **Entero**

<pre class="overflow-visible!" data-start="578" data-end="602"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>int('42')
</span></code></div></div></pre>

Salida → `42` (tipo integer)

⚠️ Si después lo usas en otra acción, Power Automate sabe que es número (puedes hacer sumas).

---

### **Decimal**

<pre class="overflow-visible!" data-start="750" data-end="778"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>float('3.14')
</span></code></div></div></pre>

Salida → `3.14` (tipo float)

---

### **Booleano**

<pre class="overflow-visible!" data-start="831" data-end="858"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>equals(1, 1)
</span></code></div></div></pre>

Salida → `true` (tipo boolean)

Útil en condiciones.

---

### **Fecha/hora**

<pre class="overflow-visible!" data-start="938" data-end="961"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>utcNow()
</span></code></div></div></pre>

Salida → `"2025-08-25T06:11:10Z"` (tipo string con formato ISO 8601).

👉 Ojo: Power Automate **trata fechas como strings** (ISO), no como un tipo `DateTime` real. Para operar sobre ellas debes usar funciones (`addDays`, `convertTimeZone`, etc.).

---

### **Array**

<pre class="overflow-visible!" data-start="1230" data-end="1269"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>createArray('A','B','C')
</span></code></div></div></pre>

Salida → `["A","B","C"]` (tipo array)

---

### **Objeto (JSON)**

<pre class="overflow-visible!" data-start="1336" data-end="1382"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-powerfx"><span>json('{"id":1,"name":"Cesar"}')
</span></code></div></div></pre>

Salida →

<pre class="overflow-visible!" data-start="1394" data-end="1438"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-json"><span><span>{</span><span>
  </span><span>"id"</span><span>:</span><span></span><span>1</span><span>,</span><span>
  </span><span>"name"</span><span>:</span><span></span><span>"Cesar"</span><span>
</span><span>}</span><span>
</span></span></code></div></div></pre>

(tipo objeto JSON)

---

## ✅ Buenas prácticas

* Usa **Compose** para “cachear” un valor de cualquier tipo → string, int, bool, objeto, array.
* Si necesitas **acumular o cambiar** → mejor Variable.
* Si es fecha, aunque Compose la guarda como string, **úsala siempre en formato ISO** (`yyyy-MM-ddTHH:mm:ssZ`) para evitar errores.
* Para evitar confusión, ponle nombre claro al Compose → `compose_TotalFacturas_int`, `compose_IsValid_bool`, `compose_Timestamp_str`.

---

👉 En resumen:

* **Strings, números, booleanos, arrays y objetos** se mantienen en su tipo en Compose.
* **Fechas** se manejan como string ISO.
* En la salida UI parecen “texto”, pero internamente puedes operar con ellos según el tipo real.


Excelente pregunta 👌. Para el  **action Compose** , al igual que para Variables, SQL, HTTP, etc., lo ideal es seguir un **estándar de nomenclatura** que haga evidente  **qué hace y qué tipo de dato maneja** .

---

## 🔑 Convención recomendada para **Compose**

Formato:

<pre class="overflow-visible!" data-start="271" data-end="307"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">compose_<QuéHace</span></span><span>>_</span><span><TipoDato</span><span>>
</span></span></code></div></div></pre>

### Reglas:

1. **Prefijo `compose_`** → siempre, para identificar que es un  *Data Operation Compose* .
2. **Nombre descriptivo** → explica el propósito o transformación.
3. **Sufijo opcional de tipo de dato** (`_str`, `_int`, `_bool`, `_arr`, `_obj`, `_dt`) → recomendable si no es evidente.

---

## 📌 Ejemplos prácticos

### Strings

* `compose_formatDate_str` → fecha ya formateada como string.
* `compose_customerEmail_str` → email extraído del objeto.

### Enteros / Decimales

* `compose_totalFacturas_int` → número total de facturas.
* `compose_importe_float` → valor numérico con decimales.

### Booleanos

* `compose_isValid_bool` → resultado de una validación.
* `compose_hasItems_bool` → si un array tiene elementos.

### Arrays

* `compose_customers_arr` → lista de clientes.
* `compose_filesToZip_arr` → lista de archivos para comprimir.

### Objetos JSON

* `compose_customer_obj` → objeto del cliente actual.
* `compose_responseAPI_obj` → respuesta completa de un API.

### Fechas

* `compose_executionTime_dt` → fecha/hora en ISO 8601.

---

## ✅ Buenas prácticas

* Usa **Compose** cuando el valor no cambia (snapshot o transformación).
* Si el flujo tiene varios Compose, nómbralos con  **qué hacen** , no como “Compose 1, 2, 3” (evita confusión).
* Si usas muchos, **agruparlos en un Scope** ayuda a organizar.
* Pon el sufijo de tipo **solo cuando el tipo no sea evidente** → si es un email, no necesitas `_str`; si es un array u objeto, sí conviene `_arr` o `_obj`.

---

👉 Entonces, de acuerdo a las best practices, si tu Compose guarda, por ejemplo, un token de autenticación, el nombre sería:

**`compose_authToken_str`**

O si cacheas una lista de archivos:

**`compose_files_arr`**
