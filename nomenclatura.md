

¡Excelente que quieras estandarizar! Aquí tienes un **mapa conceptual** de convenciones de nomenclatura para Power Automate (cloud flows) pensado para equipos (Dev/QA/Prod). Mantiene los nombres  **claros, buscables y consistentes** .

# Principios globales

* **Consistencia > preferencia personal**
* **Inglés o español** , pero uno solo en todo el tenant
* **Sin espacios** en identificadores internos (usa `_`), títulos legibles en **Title Case**
* **Nombre = intención + ámbito + tipo** (qué hace, sobre qué, dónde)

# Flujo / Solution / Artefactos

* **Solution** : `ORG.Area.Proceso` → `CU.Invoicing`
* **Flow** : `ORG.Area.Proceso.Accion` → `CU.Invoicing.SendGrid.Notify`
* **Child flow** : prefijo `CF_` → `CF_SendInvoiceEmail`
* **Environment Variable** : `EV_<Dominio>_<Concepto>` → `EV_SendGrid_ApiKey`, `EV_BaseUrl`
* **Connection Reference** : `CR_<Conector|Sistema>` → `CR_HTTP`, `CR_SQL_ERP`
* **Custom Connector** : `CC_<Sistema>_<Proposito>` → `CC_SendGrid_Mail`

# Triggers (desencadenadores)

* **Recurrence** : `TR_Recurrence_Hourly`
* **Cuando llega HTTP** : `TR_Http_Inbound_<Feature>`
* **On create (Dataverse/SharePoint)** : `TR_OnCreate_<Entidad|Lista>`

# Acciones (título visible)

* **Verbo + Objeto + Contexto**
  * HTTP: `HTTP_SendGrid_SendMail`, `HTTP_API_GetInvoices`
  * SQL: `SQL_ERP_GetInvoices_ByDate`
  * SharePoint: `SP_TaxDocs_CreateFile`
  * Dataverse: `DV_Invoice_UpdateStatus`
  * Parse JSON: `Parse_Invoice_Payload`
  * Compose: `Compose_Subject`, `Compose_ToArray`
  * Condition: `If_HasRecipients`, `If_IsProd`
  * Switch: `Switch_Environment`
  * Apply to each: `ForEach_Invoice`, `ForEach_Recipient`
  * Scope: `Scope_Main`, `Scope_ErrorHandling`
  * Terminate: `Terminate_Success`, `Terminate_Failed_Retry`

> Sugerencia: usa **prefijo por conector** (`HTTP_`, `SQL_`, `SP_`, `DV_`) y  **sufijo de propósito** .

# Variables 

## Initialize Variable

* **Nombre del action Inicializar variable**

  * Usar el prefijo **`var_`** seguido de un nombre claro y en *camelCase* o  *snake_case* .
  * `var_NombreUsuario`, `var_TotalFacturas`.
* **Tipos (prefijos)**

  * `str_` (string) → `str_Subject`
  * `num_` (number) → `num_Attempts`
  * `bool_` (boolean) → `bool_IsProd`
  * `arr_` (array) → `arr_Recipients`
  * `obj_` (object) → `obj_Invoice`
  * `dt_` (datetime ISO) → `dt_From`, `dt_To`
* **Flags** : `is_` → `is_QA`, `is_Empty`
* **Contadores/índices** : `i_`, `idx_` → `i_Retry`, `idx_Item`

## Set Variable

* **Objetivo:** Asignar o actualizar el valor de una variable ya creada.
* **Convención recomendada para el Action:**

  <pre class="overflow-visible!" data-start="624" data-end="660"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">var_<nombreVariable</span></span><span>>_Set
  </span></span></code></div></div></pre>

  👉 Se mantiene el mismo prefijo de la variable para relacionar fácilmente el Action con la variable que está modificando.

🔹 Ejemplo:

* `var_strNombreCliente_Set`
* `var_arrFacturasPendientes_Set`
* `var_boolEsValido_Set`


## 3. **Append to Array Variable** (cuando se agregan elementos)

* **Convención recomendada para el Action:**

  <pre class="overflow-visible!" data-start="1014" data-end="1053"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">var_<nombreVariable</span></span><span>>_Append
  </span></span></code></div></div></pre>

  👉 Aplica solo para variables tipo array.

🔹 Ejemplo:

* `var_arrFacturasPendientes_Append`

# Entradas / Salidas (de acciones o child flows)

* **Inputs** : `in_<Concepto>` → `in_InvoiceId`, `in_EmailHtml`
* **Outputs** : `out_<Concepto>` → `out_SendGridResponse`
* **Compose para salidas “limpias”** : `Out_<Algo>` → `Out_RecipientsJson`

# Datos / nombres de campos

* **Objetos** : singular → `invoice`, `customer`
* **Arreglos** : plural → `invoices`, `contacts`
* **Rutas/ids** : `id_`, `key_`, `path_` → `id_Customer`, `path_Zip`

# Condiciones y expresiones (nombres y patrones)

* **Condition** : `If_<CondicionCorta>`
* Ej.: `If_HasContacts` con expresión:

  `length(first(body('Filter_array'))?['contacts']) >= 1`
* **Expresiones reutilizables en Compose** (para legibilidad):

  * `Compose_IsProd`: `equals(variables('str_Environment'),'PROD')`
  * `Compose_NowCST`: `convertTimeZone(utcNow(),'UTC','Central Standard Time (Mexico)')`

# Manejo de errores / reintentos

* **Scope** : `Scope_ErrorHandling`
* **Compose** : `Compose_ErrorMessage`
* **Variables** : `num_RetryCount`, `bool_ShouldRetry`
* **Acciones on-fail** : sufijo `_OnFail` → `HTTP_API_GetInvoices_OnFail`

# Versionado / cambios

* **Sufijo** : `_v2`, `_v3` sólo cuando hay **cambio de contrato**
* **Feature flags** : `ff_<Nombre>` → `ff_SkipEmail`

# Archivos / adjuntos

* **Zip/PDF** : `file_Zip_Attachments`, `file_Pdf_Invoice_<Id>`
* **Carpetas temporales** : `tmp_<uso>` → `tmp_Attach`

# Ejemplos rápidos

* **HTTP SendGrid** (acción): `HTTP_SendGrid_SendMail`
* **Compose con asunto** : `Compose_Subject`
* **Variable array destinatarios** : `arr_To`
* **For each destinatario** : `ForEach_Recipient`
* **Condition** : `If_HasBcc`
* **Scope principal** : `Scope_Main`

# Do / Don’t

* ✅ Do: `HTTP_API_GetInvoices`, `SQL_ERP_GetInvoice_ById`
* ❌ Don’t: `Get data`, `Paso 1`, `Compose 3`
* ✅ Do: `arr_Recipients`, `obj_Invoice`
* ❌ Don’t: `array1`, `temp`, `var`
* ✅ Do: plural para arrays (`contacts`)
* ❌ Don’t: plural/singular mezclado sin criterio

# Plantilla mínima para un flujo

* `TR_Recurrence_Hourly`
* `Switch_Environment` (usa `EV_Environment`)
  * **Case PROD** → `HTTP_SendGrid_SendMail`
  * **Case QA** → `HTTP_SendGrid_SendMail` (con `EV_SendGrid_To_QA`)
  * **Default** (DEV) → `HTTP_SendGrid_SendMail` (to dev)
* `Scope_ErrorHandling` (run after: has failed)





¡Hecho! Aquí tienes un **estándar compacto** para nombrar todo en Power Automate (cloud). Úsalo tal cual como “guía de estilo” para tu equipo.

# Tabla de convenciones (nombres)

| Artefacto                                          | Prefijo / Patrón                   | Ejemplos                                                                                                                                                                              | Notas prácticas                                                     |
| -------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Solution**                                 | `ORG.Area.Proceso`                | `CU.Invoicing`,`CU.Sales.Orders`                                                                                                                                                  | Una por dominio/proyecto.                                            |
| **Environment Variable**                     | `EV_<Dominio>_<Concepto>`         | `EV_SendGrid_ApiKey`,`EV_BaseUrl`,`EV_To_Prod`                                                                                                                                  | `Secret`p/credenciales.                                            |
| **Connection Reference**                     | `CR_<Conector                       | Sistema>`                                                                                                                                                                             | `CR_HTTP`,`CR_SQL_ERP`,`CR_SP`                                 |
| **Custom Connector**                         | `CC_<Sistema>_<Uso>`              | `CC_SendGrid_Mail`,`CC_BI_Reports`                                                                                                                                                | Versiona en el nombre si cambia contrato.                            |
| **Flow (padre)**                             | `.Acción`                        | `CU.Invoicing.SendGrid.Notify` o `Cuprum.Staff.Facturacion.SendCustomerInvoices`  o `Cuprum.Staff.Facturacion.MonitorPayments` o  `Cuprum.Staff.Clientes.RegisterNewCustomer` | Dentro de Solution.  EnviarCorreoClientes o  FlowPagos o ClienteAlta |
| **Child Flow**                               | `CF_<Acción>`                    | `CF_SendInvoiceEmail`                                                                                                                                                               | Entradas/salidas explícitas.                                        |
| **Trigger**                                  | `TR_<Tipo>_<Contexto>`            | `TR_Recurrence_Hourly`,`TR_OnCreate_Invoice`                                                                                                                                      | Ajusta zona horaria.                                                 |
| **Scope**                                    | `Scope_<Propósito>`              | `Scope_Main`,`Scope_ErrorHandling`                                                                                                                                                | Agrupa por fases.                                                    |
| **Initialize Variable (acción / “caja”)** | `var_<Nombre>`                    | `var_NombreUsuario`,`var_FechaInicio`                                                                                                                                             | Solo “título” visible.                                            |
| **Variable (campo Name)**                    | Prefijo por**tipo**+ nombre   | `str_nombre`,`int_intentos`,`bool_isProd`,`arr_contactos`,`obj_invoice`,`dt_from`                                                                                         | Usa estos nombres en expresiones.                                    |
| **Compose**                                  | `Compose_<Qué>`                  | `Compose_Subject`,`Compose_NowCST`                                                                                                                                                | Útil para expresiones largas.                                       |
| **Condition**                                | `If_<Condición>`                 | `If_HasContacts`,`If_IsProd`                                                                                                                                                      | Deja la lógica dentro (expresión).                                 |
| **Switch**                                   | `Switch_<Campo>`                  | `Switch_Environment`                                                                                                                                                                | Casos Dev/QA/Prod.                                                   |
| **Apply to each**                            | `ForEach_<Plural>`                | `ForEach_Invoices`,`ForEach_Recipients`                                                                                                                                           | Mantén singular/plural consistente.                                 |
| **HTTP**                                     | `HTTP_<Sistema>_<Verbo/Objetivo>` | `HTTP_SendGrid_SendMail`,`HTTP_API_GetInvoices`                                                                                                                                   | Headers/keys desde EV.                                               |
| **SQL (acción)**                            | `SQL_<BD                            | Sistema>_<Acción>`                                                                                                                                                                   | `SQL_ERP_GetInvoice_ById`                                          |
| **SharePoint**                               | `SP_<Lista>_<Acción>`            | `SP_TaxDocs_CreateFile`                                                                                                                                                             | Usa rutas claras.                                                    |
| **Dataverse**                                | `DV_<Entidad>_<Acción>`          | `DV_Invoice_UpdateStatus`                                                                                                                                                           |                                                                      |
| **Parse JSON**                               | `Parse_<Objeto>`                  | `Parse_Invoice_Payload`                                                                                                                                                             | Conserva esquema.                                                    |
| **Terminate**                                | `Terminate_<Resultado>`           | `Terminate_Success`,`Terminate_Failed_Retry`                                                                                                                                      | Úsalo en manejo de errores.                                         |
| **Salida (child/response)**                  | `out_<Concepto>`                  | `out_Status`,`out_Count`                                                                                                                                                          | Defínelo en “Respond to a flow”.                                  |
| **Entrada (child/trigger)**                  | `in_<Concepto>`                   | `in_InvoiceId`,`in_EmailHtml`                                                                                                                                                     | Defínelo en el trigger del child.                                   |
| **Archivos**                                 | `file_<Qué>`                     | `file_Zip_Attachments`,`file_Pdf_Invoice_#{id}`                                                                                                                                   | Incluye extensión al crear.                                         |
| **Flags / feature**                          | `ff_<Nombre>`                     | `ff_SkipEmail`,`ff_DryRun`                                                                                                                                                        | Actívalos por EV.                                                   |

# Solutions . Manera correcta de nombrar al Proceso ORG.Area.Proceso


La convención **`ORG.Area.Proceso`** te da un marco claro, pero lo que más cuesta es cómo nombrar ese **proceso** de forma que sea entendible y estándar para todos.

---

## 🔹 Cómo identificar el *Proceso* en el nombre de la Solution

1. **Usar la funcionalidad principal que atiende**

   * Ejemplo:  *Envío de facturas* ,  *Gestión de órdenes de compra* ,  *Notificación de incidencias* .
   * Se recomienda que sea un **sustantivo** o  **acción clara** .

   👉 `Cuprum.Staff.Facturacion`

   👉 `Cuprum.Staff.OrdenesCompra`

---

2. **Basarte en el dominio de negocio**

   * Pregúntate: ¿qué área del negocio impacta este flujo?
   * Ejemplo:  *Clientes* ,  *Empleados* ,  *Inventario* ,  *Pagos* .

   👉 `Cuprum.Staff.Clientes`

   👉 `Cuprum.Staff.EmpleadosVacaciones`

---

3. **Describir el objetivo del flujo o grupo de flujos**

   * Evita nombres genéricos como *Prueba* o  *Workflow1* .
   * Mejor algo que diga qué resuelve.

   👉 `Cuprum.Staff.EnviarFacturasClientes`

   👉 `Cuprum.Automotriz.ControlCalidadReportes`

---

4. **Agregar un sufijo opcional de contexto** (solo si es necesario)

   * Puede ser:  *Test* , *POC* (Proof of Concept),  *Iniciacion* ,  *Legacy* .
   * Así diferencias versiones sin romper el estándar.

   👉 `Cuprum.Staff.Facturacion.TestIniciacion`

---

## 🔹 Buenas prácticas de la comunidad

* **Corto y claro** : menos de 3–4 palabras en el  *Proceso* .
* **Consistente** : todos usan el mismo estilo (CamelCase o PascalCase).
* **Unívoco** : el nombre ya debe dar pista de lo que hace, sin abrir la solución.

---

✅ Entonces, para tu ejemplo:

Si tu Solution maneja correos de facturación, el nombre ideal sería:

**`Cuprum.Staff.EnviarFacturasClientes`**

Y dentro de esa solution ya metes los flujos relacionados a ese proceso.



# 📘 Convención de Nombres para Power Automate

## 1. **Solutions**

Formato recomendado:

<pre class="overflow-visible!" data-start="210" data-end="234"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>ORG</span><span>.</span><span>Area</span><span>.</span><span>Proceso</span><span>
</span></span></code></div></div></pre>

* **ORG** = nombre corto de la organización (ej. Cuprum).
* **Area** = área responsable (ej. Staff, Finanzas, IT).
* **Proceso** = objetivo o dominio funcional (ej. EnviarFacturasClientes).

🔹 **Ejemplos**

* `Cuprum.Staff.EnviarFacturasClientes`
* `Cuprum.Finanzas.ControlPagos`
* `Cuprum.IT.MantenimientoUsuarios`

---

## 2. **Flows dentro de la Solution**



### 🔹 Estilo “corchetes + espacios” (más legible en interfaz)

`[Área] Acción – TipoTrigger`

Ejemplos:

* `[Staff] Enviar Facturas – Scheduled`
* `[Staff] Validar Correos – HTTP Trigger`
* `[Staff] Reintentar Envios – Manual`

✅ Ventaja: rápido de leer en la interfaz de Power Automate.

❌ Desventaja: no queda tan “jerárquico” como el de puntos.

Formato recomendado (más legible en UI):

<pre class="overflow-visible!" data-start="652" data-end="687"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>[</span><span>Area</span><span>] Acción – TipoTrigger
</span></span></code></div></div></pre>

* **[Area]** = mismo área que la solution.
* **Acción** = lo que hace el flujo, en singular y con verbo.
* **TipoTrigger** = cómo se dispara (Scheduled, HTTP, Manual, Instant).

🔹 **Ejemplos**

* `[Staff] Enviar Facturas – Scheduled`
* `[Staff] Validar Correos – HTTP`
* `[Staff] Reintentar Envios – Manual`


### 🔹 Estilo “puntos” (más técnico / jerárquico)

`ORG.Area.Proceso.Acción`

Ejemplos:

* `Cuprum.Staff.EnviarFacturasClientes.EnviarFacturas`
* `Cuprum.Staff.EnviarFacturasClientes.ValidarCorreos`
* `Cuprum.Staff.EnviarFacturasClientes.ReintentarEnvios`

✅ Ventaja: consistente con la solución.

❌ Desventaja: nombres largos, menos legibles para usuarios no técnicos.

👉 Si quisieras ser  **más técnico y jerárquico** , puedes usar también:

<pre class="overflow-visible!" data-start="1084" data-end="1115"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>ORG</span><span>.</span><span>Area</span><span>.</span><span>Proceso</span><span>.</span><span>Acci</span><span>ó</span><span>n</span><span>
</span></span></code></div></div></pre>

Ejemplo:

* `Cuprum.Staff.EnviarFacturasClientes.EnviarFacturas`


## 🔹 Entonces, ¿cuál usar para nombre de flujos?

* Si tu equipo es **muy técnico** y quiere mantener todo **alineado con Solutions y repositorios** → mejor usar el formato con  **puntos** .
* Si tu equipo son **usuarios mixtos (negocio + IT)** y quieres que sea **legible y rápido de identificar en la UI** → mejor el formato con  **corchetes + espacios** .

---


# Caso de Uso de Nombre de Solution


## 📌 1. Nombre sugerido por tu gerente

**`CuprumStaff.Finanzas.EnvíoFacturasClientes`**

### ✔️ Ventajas

* **Jerarquía clara** : Organización (`Cuprum`), Área de soporte (`Staff`), Área funcional (`Finanzas`), Proceso (`EnvíoFacturasClientes`).
* **Precisión** : Se entiende exactamente a qué área pertenece el proceso (aunque lo administre Staff, está ligado a Finanzas).
* **Escalabilidad funcional** : Si más adelante Finanzas tiene otros procesos (Ej. pagos, conciliaciones), cada solución puede mantener el mismo patrón.

### ❌ Desventajas

* **Nombre más largo** : Puede sentirse pesado si se crean muchísimas soluciones.
* **Doble área** : Combinar *Staff* y *Finanzas* puede generar dudas (“¿esto lo administra Staff o Finanzas?”).

---

## 📌 2. Convención que yo te había sugerido antes

**`Cuprum.Staff.EnviarFacturasClientes`**

### ✔️ Ventajas

* **Sencillez** : Más corto y fácil de leer.
* **Responsabilidad clara** : El área Staff es la que construye y administra la solución → queda evidente.
* **Consistencia técnica** : Si el objetivo es que Staff sea dueño de  **todos los automatismos** , no importa de qué área funcional provengan.

### ❌ Desventajas

* **Menos contexto de negocio** : No queda tan claro que se trata de un proceso financiero.
* **Escalabilidad ambigua** : Si Staff desarrolla soluciones para Finanzas, Ventas y RRHH, todas quedarían bajo `Cuprum.Staff`, perdiendo claridad funcional.

---

## 📊 Comparación directa

| Criterio                                 | `CuprumStaff.Finanzas.EnvíoFacturasClientes`   | `Cuprum.Staff.EnviarFacturasClientes` |
| ---------------------------------------- | ------------------------------------------------- | --------------------------------------- |
| **Claridad de negocio**            | ⭐⭐⭐⭐ (indica que es de Finanzas)              | ⭐⭐ (solo indica Staff)                |
| **Claridad de ownership técnico** | ⭐⭐⭐ (parece compartido entre Staff y Finanzas) | ⭐⭐⭐⭐ (Staff dueño)                 |
| **Sencillez y legibilidad**        | ⭐⭐ (largo)                                      | ⭐⭐⭐⭐ (más corto)                   |
| **Escalabilidad funcional**        | ⭐⭐⭐⭐ (varios procesos por área funcional)    | ⭐⭐ (todos procesos caen en Staff)     |
| **Consistencia para TI**           | ⭐⭐⭐ (hay que validar qué área aparece)       | ⭐⭐⭐⭐ (siempre Staff)                |

---

## 📌 ¿Cuál elegir?

* **Si tu empresa quiere que cada solución refleje el área funcional de negocio (Finanzas, Ventas, etc.)** → Mejor el de tu gerente:

  👉 `CuprumStaff.Finanzas.EnvíoFacturasClientes`
* **Si lo que importa es que quede claro quién administra los flujos (Staff como “fábrica de software”)** → Mejor mi sugerencia original:

  👉 `Cuprum.Staff.EnviarFacturasClientes`

---

✅ **Recomendación práctica:**

Yo me inclinaría por la convención de tu gerente (`CuprumStaff.Finanzas...`), porque en auditorías, mantenimiento y soporte, es mucho más valioso que el  **nombre refleje el proceso de negocio** .

Luego, dentro del flujo, en la descripción, ya aclaras que el área Staff es quien lo administra.

## 3. **Variables**

Formato recomendado:

<pre class="overflow-visible!" data-start="1231" data-end="1254"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml"><tipo</span></span><span>>_</span><span><nombre</span><span>>
</span></span></code></div></div></pre>

* **tipo** = prefijo (str, int, arr, bool, obj).
* **nombre** = en minúscula, con guiones bajos.

🔹 **Ejemplos**

* `str_nombreCliente`
* `int_numeroFactura`
* `arr_facturasPendientes`
* `bool_esValido`

---

## 4. **Actions (composes, condiciones, etc.)**

Formato recomendado:

<pre class="overflow-visible!" data-start="1549" data-end="1574"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">ACT_<descripción</span></span><span>>
</span></span></code></div></div></pre>

* **ACT** = prefijo fijo para distinguir acciones renombradas.
* **descripción** = verbo + detalle.

🔹 **Ejemplos**

* `ACT_ObtenerFactura`
* `ACT_ValidarEmailCliente`
* `ACT_FormatearFechaEnvio`

---

## 5. **Entradas y Salidas (I/O)**

Formato recomendado:

<pre class="overflow-visible!" data-start="1847" data-end="1881"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">in_<nombre</span></span><span>>  
out_</span><span><nombre</span><span>>
</span></span></code></div></div></pre>

* **in** = entrada del flujo / acción.
* **out** = salida del flujo / acción.

🔹 **Ejemplos**

* `in_correoCliente`
* `out_respuestaAPI`

---

## 6. **Parámetros de Ambiente**

Formato recomendado:

<pre class="overflow-visible!" data-start="2091" data-end="2111"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span><span class="language-xml">env_<nombre</span></span><span>>
</span></span></code></div></div></pre>

* Para distinguir valores que dependen de ambiente (Dev, QA, Prod).

🔹 **Ejemplos**

* `env_sqlServerName`
* `env_apiUrl`
* `env_sendGridKey`

---

# 📑 Resumen Comparativo

| Elemento           | Convención                   | Ejemplo                                  |
| ------------------ | ----------------------------- | ---------------------------------------- |
| **Solution** | ORG.Area.Proceso              | `Cuprum.Staff.EnviarFacturasClientes`  |
| **Flow**     | [Area] Acción – TipoTrigger | `[Staff] Enviar Facturas – Scheduled` |
| **Variable** | tipo_nombre                   | `str_nombreCliente`                    |
| **Action**   | ACT_Descripción              | `ACT_FormatearFechaEnvio`              |
| **Entrada**  | in_nombre                     | `in_correoCliente`                     |
| **Salida**   | out_nombre                    | `out_respuestaAPI`                     |
| **Ambiente** | env_nombre                    | `env_apiUrl`                           |

# Patrones “Do / Don’t”

* ✅ `HTTP_API_GetInvoices`, `SQL_ERP_GetInvoice_ById`, `var_TotalFacturas`, `arr_Recipients`, `If_HasBcc`
* ❌ `HTTP 2`, `Get data`, `Compose 3`, `Paso 1`, `var1`, `array1`

# Expresiones útiles (listas para pegar)

* **Hora local MX (CST/CDMX)**

  `convertTimeZone(utcNow(),'UTC','Central Standard Time (Mexico)')`
* **Solo ejecutar en hh:00 (tolerancia del trigger)**

  `equals(minute(utcNow()),0)`
* **Array seguro (coalesce)**

  `coalesce(body('PasoX')?['value'], createArray())`
* **Contacts > 0 en primer item filtrado**

  `length(first(body('Filter_array'))?['contacts']) >= 1`
* **Split destinatarios `;` → objetos `{email}` (Select)**

  From: `@split(variables('str_to'),';')` → Map: `email = @item()`
* **Subject con sufijo por ambiente**

  `concat(outputs('Compose_SubjectSuffix'),' Documentos de sus Facturas')`

# Ejemplo mini-flujo con nombres

1. `TR_Recurrence_Hourly`
2. `Scope_Main`
   * `var_Environment` →  **Name** : `str_environment` (Dev/QA/Prod)
   * `Get EV_SendGrid_ApiKey` → `Compose_ApiKey = @{outputs()?['Value']}`
   * `HTTP_SendGrid_SendMail` (CR_HTTP)
     * `Authorization: @{concat('Bearer ', outputs('Compose_ApiKey'))}`
     * Body usa `Select_To` / `Select_Bcc` y `Compose_SubjectSuffix`
3. `Scope_ErrorHandling` (run after: failed)
   * `Compose_ErrorMessage = @{coalesce(outputs()?['body'], string(outputs()))}`
   * `Terminate_Failed_Retry`

# Sugerencias de equipo

* **Un idioma** (ES o EN) y **consistencia** en todo el tenant.
* **Singular** para objetos (`invoice`), **plural** para arrays (`invoices`).
* Variables con  **prefijo por tipo** ; acciones con  **prefijo por conector/sistema** .
* **Environment Variables** para todo lo que cambia por ambiente (URLs, keys, destinatarios, subject-suffix).
* **Connection References** siempre (en Solutions).
* Manejo de errores con  **Scope_ErrorHandling** , **reintentos** (429/5xx) y logging.
* **DLP** : separa conectores de negocio / no negocio; limita HTTP donde corresponde.

---

Si quieres, te genero una **plantilla de Solution** con:

* EVs (`EV_SendGrid_ApiKey`, `EV_Subject_Suffix`, `EV_To`, `EV_Bcc`)
* CR (`CR_HTTP`)
* Flow esqueleto con todas estas convenciones

  para que la importes y la usen de base.
