¡Hecho! Aquí tienes un **estándar compacto** para nombrar todo en Power Automate (cloud). Úsalo tal cual como “guía de estilo” para tu equipo.

# Tabla de convenciones (nombres)

| Artefacto                                          | Prefijo / Patrón                   | Ejemplos                                                                                      | Notas prácticas                          |
| -------------------------------------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------------------- |
| **Solution**                                 | `ORG.Area.Proceso`                | `CU.Invoicing`,`CU.Sales.Orders`                                                          | Una por dominio/proyecto.                 |
| **Environment Variable**                     | `EV_<Dominio>_<Concepto>`         | `EV_SendGrid_ApiKey`,`EV_BaseUrl`,`EV_To_Prod`                                          | `Secret`p/credenciales.                 |
| **Connection Reference**                     | `CR_<Conector                       | Sistema>`                                                                                     | `CR_HTTP`,`CR_SQL_ERP`,`CR_SP`      |
| **Custom Connector**                         | `CC_<Sistema>_<Uso>`              | `CC_SendGrid_Mail`,`CC_BI_Reports`                                                        | Versiona en el nombre si cambia contrato. |
| **Flow (padre)**                             | `ORG.Area.Proceso.Acción`        | `CU.Invoicing.SendGrid.Notify`                                                              | Dentro de Solution.                       |
| **Child Flow**                               | `CF_<Acción>`                    | `CF_SendInvoiceEmail`                                                                       | Entradas/salidas explícitas.             |
| **Trigger**                                  | `TR_<Tipo>_<Contexto>`            | `TR_Recurrence_Hourly`,`TR_OnCreate_Invoice`                                              | Ajusta zona horaria.                      |
| **Scope**                                    | `Scope_<Propósito>`              | `Scope_Main`,`Scope_ErrorHandling`                                                        | Agrupa por fases.                         |
| **Initialize Variable (acción / “caja”)** | `var_<Nombre>`                    | `var_NombreUsuario`,`var_FechaInicio`                                                     | Solo “título” visible.                 |
| **Variable (campo Name)**                    | Prefijo por**tipo**+ nombre   | `str_nombre`,`int_intentos`,`bool_isProd`,`arr_contactos`,`obj_invoice`,`dt_from` | Usa estos nombres en expresiones.         |
| **Compose**                                  | `Compose_<Qué>`                  | `Compose_Subject`,`Compose_NowCST`                                                        | Útil para expresiones largas.            |
| **Condition**                                | `If_<Condición>`                 | `If_HasContacts`,`If_IsProd`                                                              | Deja la lógica dentro (expresión).      |
| **Switch**                                   | `Switch_<Campo>`                  | `Switch_Environment`                                                                        | Casos Dev/QA/Prod.                        |
| **Apply to each**                            | `ForEach_<Plural>`                | `ForEach_Invoices`,`ForEach_Recipients`                                                   | Mantén singular/plural consistente.      |
| **HTTP**                                     | `HTTP_<Sistema>_<Verbo/Objetivo>` | `HTTP_SendGrid_SendMail`,`HTTP_API_GetInvoices`                                           | Headers/keys desde EV.                    |
| **SQL (acción)**                            | `SQL_<BD                            | Sistema>_<Acción>`                                                                           | `SQL_ERP_GetInvoice_ById`               |
| **SharePoint**                               | `SP_<Lista>_<Acción>`            | `SP_TaxDocs_CreateFile`                                                                     | Usa rutas claras.                         |
| **Dataverse**                                | `DV_<Entidad>_<Acción>`          | `DV_Invoice_UpdateStatus`                                                                   |                                           |
| **Parse JSON**                               | `Parse_<Objeto>`                  | `Parse_Invoice_Payload`                                                                     | Conserva esquema.                         |
| **Terminate**                                | `Terminate_<Resultado>`           | `Terminate_Success`,`Terminate_Failed_Retry`                                              | Úsalo en manejo de errores.              |
| **Salida (child/response)**                  | `out_<Concepto>`                  | `out_Status`,`out_Count`                                                                  | Defínelo en “Respond to a flow”.       |
| **Entrada (child/trigger)**                  | `in_<Concepto>`                   | `in_InvoiceId`,`in_EmailHtml`                                                             | Defínelo en el trigger del child.        |
| **Archivos**                                 | `file_<Qué>`                     | `file_Zip_Attachments`,`file_Pdf_Invoice_#{id}`                                           | Incluye extensión al crear.              |
| **Flags / feature**                          | `ff_<Nombre>`                     | `ff_SkipEmail`,`ff_DryRun`                                                                | Actívalos por EV.                        |

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
