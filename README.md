# UDY-PowerAutomate

## Conectores 

- Input
- Delay 
- Delay Unitil (Parate hasta que se cumpla alguna condicion)

## Video 4
![1752516073584](image/README/1752516073584.png)

## Video 5, 6

![1752624854393](image/README/1752624854393.png)
![1752625188392](image/README/1752625188392.png)
![1752625294350](image/README/1752625294350.png)
![1752625510857](image/README/1752625510857.png)
![1752625535032](image/README/1752625535032.png)

## Video 9 Casos de Uso
![1752625821315](image/README/1752625821315.png)

## Video 10 Introduccion a la Power Platform
![1752638398920](image/README/1752638398920.png)

PowerBi
![1752638430648](image/README/1752638430648.png)

PowerApps
![1752638448778](image/README/1752638448778.png)

PowerAutomate
![1752638477847](image/README/1752638477847.png)

PowerPages
![1752638516773](image/README/1752638516773.png)

CopilotStudio - Facilita la integracion de Inteligencia Artificial

DataConector 

Dataverse - DB propia de Microsoft
![1752638632602](image/README/1752638632602.png)

## Video 11 Que es Power Automate?

Utilizar la inteligencia artifical para sacar informacion de una factura, catalogar correos dependiendo el area, ya sea finanzas, RH, Etc.

## Video 12

PowerAutomateDesktop se utiliza para automatizar tareas dentro de la maquina donde esta instalada

PowerAutomate Cloud puede ejecutar procesos en la nube e integraciones.

## Video 13 Automatizacion Web Vs RPA

![1752639328496](image/README/1752639328496.png)

## Video 14 Conectores y Aplicaciones

Un conector es un vinculo entre una plataforma externa con PowerAutomate.
![1752639926134](image/README/1752639926134.png)

## Video 15 Que es un Flujo?

![1752640069325](image/README/1752640069325.png)

## Video 16 Tipos de Flujo

- Cloud 
    - Automatizado 
        - Se ejecuta cuando hay un disparador
    - Instantaneo
        - Se ejecuta de manera manual
    - Programado
        - Es lo que se ejecuta a una hora determinada en un periodo (CRON)
- Escritorio
    - El proceso se correo dentro de la maquina donde esta instalado

## Video 17 Licencia M365

![1752640528640](image/README/1752640528640.png)
![1752640506594](image/README/1752640506594.png)

## Video 18 Inicializando Power Automate

- **Solucion**

Ejemplo.- Gracias a las soluciones de PowerAutomate, se va a poder vincular un proceso automatico de power automate con una aplicacion de power apps y que se empaqueten todas juntas para que si algun dia se tiene que migrar a otra organizacion o otro entorno y que tienen que entrar en otro proceso de ciclo de vida de aplicaciones en otro sistema de gobernaza, este todo empaquetado y junto permitiendo la mayor eficiencia

![1752641603390](image/README/1752641603390.png)

- **Mineria de Procesos**

Nos permitira analizar los puntos flacos de nuestos procesos, con la finalidad de ver donde podemos agilizarlos y hacerlos mejores

- **Centro de IA**

Crear proceso automaticos de Power Automate que incorporen inteligencia artificial de todo tipo
![1752641859958](image/README/1752641859958.png)

- **Tablas**

Son las escructuras de datos que ya vienen predefinidas 

![1752641953790](image/README/1752641953790.png)

- **Conexiones**

Se muestran las aplicaciones a las que tengo conectadas mis credenciales 
## Investigacion "Sistema de Gobernanza"

- **Colas de trabajo**

Serian como procesos que se pueden ejecutar uno detras de otro, es decir yo puedo vincular varios procesos entre si y que cuando se ejecute y finalize uno, luego se ejecute el siguiente

![1752642952536](image/README/1752642952536.png)

- **Maquinas**

Lugar donde se pueden llegar a gestionar los servidores o maquinas virtuales en las que se ejecutan los procesos de escritorio, por lo que en esa seccion se puede monitorearlas, cuales tenemos creadas.

![1752642995626](image/README/1752642995626.png)


## Investigacion "Sistema de Gobernaza"


En Power Automate, cuando se habla de un sistema de gobernanza, se refiere al conjunto de políticas, controles, roles y herramientas que una organización implementa para administrar de forma segura, eficiente y conforme el uso de los flujos (flows) y automatizaciones que los usuarios crean.

🔍 ¿Qué incluye un sistema de gobernanza en Power Automate?
1. Políticas de entorno (DLP – Data Loss Prevention)
Controlan qué conectores se pueden usar juntos.

Ejemplo: Evitar que datos de SharePoint se filtren hacia Gmail o Twitter.

2. Gestión de entornos
Crear entornos separados para desarrollo, pruebas y producción.

Permite aislar flujos críticos de pruebas personales o temporales.

3. Control de acceso y roles
Definir quién puede crear flujos, administrarlos o compartirlos.

Uso de roles como Administrador de Entorno, Maker, Viewer, etc.

4. Monitoreo y auditoría
Seguimiento de qué flujos están activos, cuántas veces se ejecutan, si fallan, etc.

Uso de Power Platform Admin Center y Microsoft Purview.

5. Ciclo de vida de desarrollo
Estrategias para desplegar flujos entre entornos (desarrollo → producción).

Uso de soluciones para empaquetar y mover flujos de forma ordenada.

6. Capacitación y políticas internas
Asegurar que los usuarios sepan cómo usar Power Automate responsablemente.

Crear documentación y normas internas de buenas prácticas.

✅ ¿Por qué es importante tener gobernanza?
Sin una buena gobernanza:

Los flujos pueden volverse difíciles de mantener.

Pueden exponerse datos sensibles sin control.

Se generan duplicidades y automatizaciones ineficientes.

## Investigacion "Ejemplo de un sistema de Gobernaza"

Perfecto. Vamos a construir una estrategia de gobernanza para Power Automate paso a paso. Te presento un mapa estructurado que puedes adaptar a tu organización o equipo:

🧭 Estrategia de Gobernanza para Power Automate
1. Definir Roles y Responsabilidades
Rol	Responsabilidad Principal
Administrador Global	Gestiona licencias, monitoreo global, acceso a todo.
Administrador de Entorno	Administra entornos, asigna roles, configura políticas DLP.
Creador de Flujos (Maker)	Desarrolla flujos dentro de los límites definidos.
Auditor / Seguridad	Revisa logs, auditorías, cumplimiento de normas.

2. Estructurar Entornos
Entorno	Uso
Desarrollo	Pruebas y creación de flujos.
QA / Validación	Validación funcional por otros usuarios.
Producción	Flujos aprobados y en uso formal.
Personal	Espacio libre para pruebas individuales.

Nota: Usa nombres claros como PROD_Finanzas, DEV_RecursosHumanos, etc.

3. Implementar Políticas de Prevención de Pérdida de Datos (DLP)
Clasifica conectores como:

Business (seguros): SharePoint, SQL, Dataverse.

Non-business (riesgosos): Twitter, Gmail, Dropbox.

Evita que se combinen conectores de distintos tipos.

Ejemplo:

plaintext
Copy
Edit
No permitir combinar SharePoint (Business) con Gmail (Non-business)
4. Uso de Soluciones
Utiliza "Soluciones" para empaquetar flujos, tablas y apps como una sola unidad.

Permite mover fácilmente automatizaciones entre entornos (DEV → QA → PROD).

Ideal para proyectos de varios desarrolladores.

5. Monitoreo y Auditoría
Herramientas clave:

Power Platform Admin Center (https://admin.powerplatform.microsoft.com/)

Microsoft Purview (auditoría y cumplimiento).

Indicadores a revisar:

Flujos fallidos o con errores.

Flujos que ejecutan acciones sensibles (ej. envíos de correo, borrado de datos).

Quién creó y comparte flujos.

6. Control de Versiones y Ciclo de Vida
Define procesos para:

Solicitar nuevos flujos.

Revisar y aprobar antes de ir a producción.

Documentar flujos y actualizaciones.

7. Documentación y Buenas Prácticas
Crear una guía interna que incluya:

Reglas de nombrado (ej. FLW_ValidarFactura_Prod)

Recomendaciones de seguridad.

Cómo reportar errores o mejoras.

8. Capacitación Continua
Dar capacitaciones mensuales o trimestrales.

Promover una comunidad interna de makers.

Compartir flujos reutilizables, tips y templates.

## Video 19 Plantillas

Son soluciones que pone a disposicion Microsoft para que podamos utilizar y agilizar el diseño de nuestros procesos automaticos

![1752643511341](image/README/1752643511341.png)
![1752643523930](image/README/1752643523930.png)

## Video 20 Conexiones

Permite ver las aplicaciones que esta utilizando las credenciales

## Video 21 Entornos

En este caso todo lo que creemos en nuestro entorno, solo sera visible por nosotros a menos que lo quieramos compartir con alguien mas los flujos, etc.

https://admin.powerplatform.microsoft.com/home

Liga para ver los entornos para Power Platform, solo si eres Admin se puede acceder a esa pantalla

https://admin.powerplatform.microsoft.com/enviroments

![1752644300642](image/README/1752644300642.png)

# Seccion 4: Crear un flujo desde cero

## Video 22 Diseñador Clasico

![1752644466239](image/README/1752644466239.png)

## Video 23, 24, 25 Flujo de nube automatico, Instantaneo, Programado

Vemos los distintos tipos de flujos que podemos crear 

![1752806921653](image/README/1752806921653.png)

## Video 26 Conexiones

## Video 32 Revisar errores

![1753315151803](image/README/1753315151803.png)

## Video 33 Pausa entre Bloques (Delay)

![1753317071857](image/README/1753317071857.png)
![1753317089143](image/README/1753317089143.png)

![1753317040747](image/README/1753317040747.png)

## Video 34 Ejecuciones Paralelas

Le damos click derecho para que salga la opcion en el signo de +
![1753318729981](image/README/1753318729981.png)

![1753319304225](image/README/1753319304225.png)

## Video 35 Clonar Procesos

Va a ver ocaciones donde tengamos muchas ramas, muchos proceso y tengamos que hacer clonaciones

Click Derecho sobre el Action
* ![1753319576238](image/README/1753319576238.png)
Pegamos dando Click Derecho en el Boton de + y escojemos Paste an Action
* ![1753319681579](image/README/1753319681579.png)

