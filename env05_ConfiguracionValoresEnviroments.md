Exacto: en **Desarrollo** creas la *Solution* y desde ahí preparas todo para que  **cambie automáticamente por ambiente** . Se hace con dos piezas:

1. **Environment Variables (EV)** → para valores de **config** (URLs, claves, flags, correos, etc.).
2. **Connection References (CR)** → para **conexiones** (SQL, SharePoint, HTTP…) que deben apuntar a recursos distintos en cada ambiente.

Abajo te dejo el “cómo” y ejemplos.

---

# Qué va en EV vs qué va en CR

| Caso                                            | Dónde se define                            | Ejemplos                                                                       | Cómo se usa en el flow                                                                     |
| ----------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Valor de configuración que cambia por ambiente | **Environment Variable**(Text/Secret) | `EV_ApiBaseUrl`,`EV_SubjectSuffix`,`EV_To_Default`,`EV_SendGridApiKey` | Acción**Get environment variable value**→`outputs('Get_EV_ApiBaseUrl')?['Value']` |
| Conexión a un sistema (credenciales, servidor) | **Connection Reference**              | `CR_SQL_ERP`,`CR_HTTP`,`CR_SP`                                           | La acción usa la**CR** ; al importar en QA/Prod mapeas a la conexión del ambiente   |

---

# Patrón recomendado (paso a paso)

## 1) En **DEV** (crear Solution y preparar “multientorno”)

* En tu **Solution** crea:
  * EVs:
    * `EV_ApiBaseUrl` (Text) → p.ej. `https://dev.api.cu.com`
    * `EV_SQL_DbName` (Text) → opcional si la DB también cambia
    * `EV_SubjectSuffix` (Text) → `[DEV]`
    * `EV_SendGridApiKey` (Secret)
  * CRs:
    * `CR_HTTP` (conector HTTP)
    * `CR_SQL_ERP` (conector SQL)
* En el  **flow** :
  * Agrega **Get environment variable value** para cada EV, y (opcional) **Compose** para leer su `Value`:
    <pre class="overflow-visible!" data-start="1553" data-end="1610"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>@{</span><span>outputs</span><span>('Get_EV_ApiBaseUrl')?</span><span>['Value'</span><span>]}
    </span></span></code></div></div></pre>
  * Las **acciones HTTP** construyen la URL con la EV:
    <pre class="overflow-visible!" data-start="1670" data-end="1767"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>concat</span><span>(</span><span>outputs</span><span>(</span><span>'Compose_ApiBaseUrl'</span><span>), </span><span>'/v1/invoices?from='</span><span>, </span><span>variables</span><span>(</span><span>'dt_from'</span><span>))
    </span></span></code></div></div></pre>
  * Las **acciones SQL** usan la **Connection Reference** `CR_SQL_ERP`. (En Dev la CR apunta a la conexión de Dev).

> Resultado: el **mismo flow** usa valores de EV y CR, que luego **cambiarán solos** por ambiente.

## 2) **Export** de la Solution (Managed)

## 3) **Import** en **QA**

* Durante el import:
  * **Mapea las Connection References** :
  * `CR_HTTP` → conexión HTTP creada con **svc-qa**
  * `CR_SQL_ERP` → conexión SQL **QA** (servidor/DB/credenciales de QA)
  * **Asigna Current Values de EV** :
  * `EV_ApiBaseUrl` → `https://qa.api.cu.com`
  * `EV_SubjectSuffix` → `[QA]`
  * `EV_SendGridApiKey` → clave de QA
  * (si aplica) `EV_SQL_DbName` → `ERP_QA`
* Guarda y prueba: el flow en QA **ya apunta** a QA sin tocar el diseño.

## 4) **Import** en **PROD** (igual que QA)

* `CR_HTTP` → conexión HTTP con **svc-prod**
* `CR_SQL_ERP` → conexión SQL **Prod**
* EVs:
  * `EV_ApiBaseUrl` → `https://api.cu.com`
  * `EV_Subject_Suffix` → *(vacío)*
  * `EV_SendGridApiKey` → clave de Prod

---

# Ejemplos de expresiones (listas para pegar)

**Base URL por ambiente (HTTP):**

<pre class="overflow-visible!" data-start="2864" data-end="2941"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>@{concat(outputs('Get_EV_ApiBaseUrl')?['Value'], '/v1/invoices')}
</span></span></code></div></div></pre>

**Subject con sufijo por ambiente:**

<pre class="overflow-visible!" data-start="2980" data-end="3089"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>@{concat(coalesce(outputs('Get_EV_SubjectSuffix')?['Value'], ''), ' Documentos de sus Facturas')}
</span></span></code></div></div></pre>

**Cadena de conexión/servidor SQL (si usas EVs dentro de la acción):**

* *Server name* : (normalmente lo fija la conexión; si necesitas variarlo en una Custom Query podrías tenerlo en la EV)

<pre class="overflow-visible!" data-start="3281" data-end="3334"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>@{outputs('Get_EV_SQL_Server')?['Value']}
</span></span></code></div></div></pre>

**Clave API (header Authorization):**

<pre class="overflow-visible!" data-start="3374" data-end="3450"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span>@{concat('Bearer ', outputs('Get_EV_SendGridApiKey')?['Value'])}
</span></span></code></div></div></pre>

---

# Consejos finales

* **Todo lo que cambie por ambiente** → EV (Text/Secret).
* **Toda conexión a sistemas** → CR, y mapeo por ambiente a la conexión de QA/Prod creada con **cuenta de servicio** del ambiente.
* **Nunca** hardcodees URLs/keys en el flow.
* En **Dev** puedes poner defaults, pero en QA/Prod **siempre** establece **Current Values** al importar.
* Documenta tus EV/CR en la Solution (qué son, qué esperan por ambiente).
