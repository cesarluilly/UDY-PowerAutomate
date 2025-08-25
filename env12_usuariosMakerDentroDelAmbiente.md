## 🔑 Pasos para agregar Makers a un ambiente en Power Platform

### 1. Acceder al **Power Platform Admin Center**

* Ir a 👉 [https://admin.powerplatform.microsoft.com]()
* Iniciar sesión con la cuenta que tenga permisos de **Administrador Global** o **Administrador de Power Platform** en el tenant.

---

### 2. Seleccionar el ambiente

* En el menú izquierdo, entrar en  **Environments** .
* Elegir el ambiente donde están las soluciones y flujos (en tu caso: `Servicios Cuprum, S.A. de C.V. (default)`).

---

### 3. Ir a la pestaña de **Permissions**

* Dentro del ambiente, abre la pestaña **Settings (⚙️)** → **Users + permissions** →  **Security roles** .
* Aquí puedes administrar los roles y permisos.

---

### 4. Asignar rol de **Environment Maker**

* Seleccionar a los usuarios (tus compañeros).
* Asignarles el rol de  **Environment Maker** .

📌 Con este rol podrán:

* Crear y ver  **soluciones** .
* Crear, editar y ver **flujos** dentro de esas soluciones.
* Crear y administrar conexiones.

---

### 5. (Opcional) Asignar rol de **System Customizer**

* Si además quieres que tengan control más avanzado sobre objetos en Dataverse, puedes darles este rol.
* Sin embargo, para tu caso con flujos en soluciones, con **Environment Maker** basta.

---

### 6. Validar acceso

* Tus compañeros deben ir a  **Power Automate → Soluciones → [Seleccionar la solución]** .
* Ahora sí verán el flujo directamente dentro de la solución (no solo en “Compartido conmigo”).

---

## 📌 Resumen

* **Compartir un flujo** solo lo muestra en  *flujos individuales* .
* **Dar permisos de Maker en el ambiente** les permite ver y trabajar con la **solución completa** (incluyendo flujos, conexiones y variables de ambiente).
