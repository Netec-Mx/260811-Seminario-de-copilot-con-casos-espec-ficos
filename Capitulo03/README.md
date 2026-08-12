# 2 Demostración: Análisis de correos (Outlook), chats (Teams) y documentos (Word o PDF) relacionados con un incidente

## 1. Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |
| **Tecnologías** | Microsoft Graph, Outlook, Teams, Word/PDF, Microsoft 365 Copilot |

---

## 2. Descripción General

En este laboratorio aplicarás los conceptos de contexto organizacional y Microsoft Graph para analizar información dispersa en correos electrónicos (Outlook), mensajes de chat (Teams) y documentos (Word o PDF) relacionados con un incidente ficticio de seguridad. Utilizarás el Explorador de Microsoft Graph y Copilot en Microsoft 365 para localizar, correlacionar y sintetizar datos provenientes de múltiples fuentes, simulando un escenario real de respuesta a incidentes dentro de una organización.

---

## 3. Objetivos de Aprendizaje

Al completar este laboratorio, serás capaz de:

- [ ] Construir y ejecutar consultas a Microsoft Graph para recuperar correos, chats y archivos relacionados con un incidente específico.
- [ ] Utilizar Copilot en Microsoft Teams para sintetizar información proveniente de múltiples fuentes del ecosistema Microsoft 365.
- [ ] Correlacionar datos de Outlook, Teams y documentos para generar una línea de tiempo básica del incidente.
- [ ] Interpretar los resultados obtenidos a través de Microsoft Graph verificando que los permisos delegados limitan el acceso solo a la información autorizada.

---

## 4. Prerrequisitos

### Conocimientos previos

| Requisito | Detalle |
|-----------|---------|
| Microsoft Graph | Comprensión de la API unificada, rutas de recursos y modelo de permisos (lección 3.1) |
| Microsoft 365 | Familiaridad básica con Outlook, Teams y OneDrive/SharePoint |
| API REST | Comprensión básica de métodos HTTP (GET) y parámetros de consulta |

### Acceso requerido

| Recurso | Detalle |
|---------|---------|
| Cuenta Microsoft 365 | Con licencia que incluya Copilot para Microsoft 365 (o tenant de prueba/desarrollo) |
| Explorador de Microsoft Graph | Acceso a https://developer.microsoft.com/graph/graph-explorer |
| Permisos | `Mail.Read`, `Chat.Read`, `Files.Read.All`, `Calendars.Read` (consentimiento delegado) |
| Navegador web | Microsoft Edge o Google Chrome actualizado |

---

## 5. Entorno del Laboratorio

### Software necesario

| Herramienta | Versión / URL |
|-------------|---------------|
| Explorador de Microsoft Graph | https://developer.microsoft.com/graph/graph-explorer |
| Microsoft Teams | Aplicación de escritorio o web (https://teams.microsoft.com) |
| Microsoft Outlook | Aplicación de escritorio o web (https://outlook.office.com) |
| Microsoft 365 Copilot | Integrado en Teams y/o Microsoft 365 Chat |

### Preparación del escenario (datos de prueba)

Antes de iniciar, debes asegurarte de tener datos de prueba en tu tenant. Si utilizas un tenant de desarrollo, ejecuta los siguientes pasos de preparación:

**Paso A:** Envía un correo electrónico de prueba desde Outlook con el siguiente contenido:

- **Asunto:** `[INCIDENTE-2025-042] Acceso no autorizado detectado en servidor de producción`
- **Cuerpo:** `Se ha detectado un acceso no autorizado al servidor PROD-DB-01 el día 15 de enero de 2025 a las 03:22 UTC. Se requiere investigación inmediata. El usuario comprometido parece ser svc_backup. Se adjunta el log preliminar.`
- **Destinatarios:** Tu propia cuenta y al menos un colega del tenant de prueba.

**Paso B:** Crea un mensaje en un canal de Teams llamado "Seguridad" (o cualquier canal disponible):

- **Mensaje:** `Equipo: hemos confirmado el incidente INCIDENTE-2025-042. El vector de ataque fue credenciales filtradas del servicio svc_backup. Necesitamos revisar los accesos de las últimas 48 horas. @todos por favor revisen sus sistemas.`

**Paso C:** Sube un documento Word o PDF a OneDrive con el nombre:

- **Nombre del archivo:** `Reporte_Incidente_2025_042.docx`
- **Contenido mínimo:** Un párrafo describiendo el incidente, incluyendo la línea de tiempo preliminar y las acciones tomadas.

---

## 6. Instrucciones Paso a Paso

### Paso 1: Consultar correos relacionados con el incidente mediante Microsoft Graph

**Objetivo:** Recuperar correos electrónicos de Outlook que contengan información sobre el incidente INCIDENTE-2025-042 utilizando la API de Microsoft Graph.

**Instrucciones:**

1. Abre el navegador y navega a https://developer.microsoft.com/graph/graph-explorer.

2. Haz clic en **"Sign in to Graph Explorer"** e inicia sesión con tu cuenta de Microsoft 365.

3. Una vez autenticado, verifica que tu nombre de usuario aparece en la esquina superior derecha del explorador.

4. En el campo de consulta, selecciona el método **GET** y escribe la siguiente URL:

```
https://graph.microsoft.com/v1.0/me/messages?$search="INCIDENTE-2025-042"&$select=subject,from,receivedDateTime,bodyPreview&$top=5
```

5. Haz clic en **"Run query"** (Ejecutar consulta).

6. Si el explorador solicita permisos adicionales, haz clic en **"Consent"** (Consentir) para otorgar el permiso `Mail.Read`.

7. Observa la respuesta JSON devuelta en el panel inferior.

**Resultado esperado:**

```json
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users('...')/messages(subject,from,receivedDateTime,bodyPreview)",
  "value": [
    {
      "subject": "[INCIDENTE-2025-042] Acceso no autorizado detectado en servidor de producción",
      "from": {
        "emailAddress": {
          "name": "Tu Nombre",
          "address": "tu.correo@tudominio.com"
        }
      },
      "receivedDateTime": "2025-01-15T08:30:00Z",
      "bodyPreview": "Se ha detectado un acceso no autorizado al servidor PROD-DB-01 el día 15 de enero de 2025 a las 03:22 UTC..."
    }
  ]
}
```

**Verificación:**

- [ ] La respuesta contiene al menos un mensaje con el asunto que incluye "INCIDENTE-2025-042".
- [ ] Los campos `subject`, `from`, `receivedDateTime` y `bodyPreview` están presentes en la respuesta.
- [ ] El código de estado HTTP es **200 OK**.

---

### Paso 2: Consultar mensajes de chat en Teams relacionados con el incidente

**Objetivo:** Buscar mensajes en los chats y canales de Teams que mencionen el incidente, demostrando cómo Microsoft Graph unifica el acceso a información de múltiples servicios.

**Instrucciones:**

1. En el Explorador de Microsoft Graph, mantén tu sesión activa.

2. Modifica la consulta en el campo de URL para buscar mensajes en los equipos a los que perteneces. Escribe:

```
https://graph.microsoft.com/v1.0/me/joinedTeams
```

3. Haz clic en **"Run query"**. Identifica en la respuesta el `id` del equipo donde publicaste el mensaje sobre el incidente (busca el equipo que contiene el canal "Seguridad" o el nombre del equipo correspondiente).

4. Copia el `id` del equipo (por ejemplo: `a]b1c2d3-e4f5-6789-abcd-ef0123456789`).

5. Ahora consulta los canales de ese equipo. Sustituye `{team-id}` por el ID copiado:

```
https://graph.microsoft.com/v1.0/teams/{team-id}/channels
```

6. Haz clic en **"Run query"**. Identifica el `id` del canal donde publicaste el mensaje del incidente.

7. Finalmente, consulta los mensajes del canal sustituyendo ambos IDs:

```
https://graph.microsoft.com/v1.0/teams/{team-id}/channels/{channel-id}/messages?$top=10
```

8. Si se requiere el permiso `ChannelMessage.Read.All`, otórgalo cuando el explorador lo solicite.

9. Revisa la respuesta y localiza el mensaje que menciona "INCIDENTE-2025-042".

**Resultado esperado:**

```json
{
  "value": [
    {
      "id": "1705312200000",
      "createdDateTime": "2025-01-15T09:15:00Z",
      "body": {
        "contentType": "html",
        "content": "<p>Equipo: hemos confirmado el incidente INCIDENTE-2025-042. El vector de ataque fue credenciales filtradas del servicio svc_backup...</p>"
      },
      "from": {
        "user": {
          "displayName": "Tu Nombre"
        }
      }
    }
  ]
}
```

**Verificación:**

- [ ] Se obtuvo la lista de equipos del usuario con código 200.
- [ ] Se identificó correctamente el canal objetivo.
- [ ] El mensaje sobre el incidente aparece en la respuesta con su contenido y marca temporal.

---

### Paso 3: Buscar documentos relacionados con el incidente en OneDrive/SharePoint

**Objetivo:** Localizar documentos (Word o PDF) almacenados en OneDrive o SharePoint que estén vinculados al incidente, completando así la triangulación de fuentes de datos.

**Instrucciones:**

1. En el Explorador de Microsoft Graph, escribe la siguiente consulta para buscar archivos por nombre:

```
https://graph.microsoft.com/v1.0/me/drive/search(q='Incidente_2025_042')
```

2. Haz clic en **"Run query"**.

3. Si se requiere el permiso `Files.Read.All`, otórgalo.

4. En la respuesta, identifica el archivo `Reporte_Incidente_2025_042.docx` y anota los siguientes campos:
   - `name`: nombre del archivo
   - `lastModifiedDateTime`: última fecha de modificación
   - `webUrl`: URL para acceder al documento
   - `createdBy`: usuario que creó el archivo

5. Opcionalmente, para obtener una vista previa del contenido, copia el `id` del archivo y ejecuta:

```
https://graph.microsoft.com/v1.0/me/drive/items/{item-id}/content
```

> **Nota:** Esta última consulta descargará el archivo. En un escenario real, Copilot procesa internamente el contenido del documento para incluirlo en su análisis.

**Resultado esperado:**

```json
{
  "value": [
    {
      "name": "Reporte_Incidente_2025_042.docx",
      "lastModifiedDateTime": "2025-01-15T14:00:00Z",
      "webUrl": "https://tudominio-my.sharepoint.com/personal/.../Reporte_Incidente_2025_042.docx",
      "createdBy": {
        "user": {
          "displayName": "Tu Nombre"
        }
      }
    }
  ]
}
```

**Verificación:**

- [ ] El archivo del reporte aparece en los resultados de búsqueda.
- [ ] Los metadatos (`name`, `lastModifiedDateTime`, `webUrl`) son correctos y corresponden al archivo creado en la preparación.
- [ ] El código de estado es 200 OK.

---

### Paso 4: Usar Copilot para sintetizar la información del incidente

**Objetivo:** Utilizar Microsoft 365 Copilot en Teams para que analice y correlacione automáticamente la información de correos, chats y documentos relacionados con el incidente, demostrando el valor del contexto organizacional.

**Instrucciones:**

1. Abre **Microsoft Teams** (aplicación de escritorio o web).

2. Navega al chat de **Microsoft 365 Copilot** (también llamado "Microsoft 365 Chat" o el ícono de Copilot en la barra lateral).

3. Escribe el siguiente prompt en el chat de Copilot:

```
Necesito un resumen completo del incidente INCIDENTE-2025-042. 
Busca en mis correos de Outlook, mensajes de Teams y documentos 
en OneDrive/SharePoint toda la información relacionada. 
Incluye: qué ocurrió, cuándo, quiénes están involucrados, 
qué acciones se han tomado y qué documentos existen al respecto.
Presenta la información como una línea de tiempo.
```

4. Presiona **Enter** y espera la respuesta de Copilot (puede tomar entre 5 y 15 segundos).

5. Revisa la respuesta generada por Copilot. Debe incluir:
   - Referencias a los correos encontrados con sus fechas.
   - Mención de los mensajes de Teams sobre el incidente.
   - Referencia al documento del reporte con enlace.
   - Una línea de tiempo o cronología de eventos.

6. Verifica que Copilot cite las fuentes. Generalmente incluye enlaces o referencias numeradas que puedes expandir para ver de dónde proviene cada dato.

**Resultado esperado:**

Copilot debería generar una respuesta similar a:

> **Resumen del Incidente INCIDENTE-2025-042**
>
> **Línea de tiempo:**
> - **15 ene 2025, 03:22 UTC** — Se detectó acceso no autorizado al servidor PROD-DB-01. [Correo: "[INCIDENTE-2025-042] Acceso no autorizado..."]
> - **15 ene 2025, ~09:15** — Se confirmó el incidente en el canal de Seguridad en Teams. El vector de ataque fueron credenciales filtradas del servicio svc_backup. [Mensaje de Teams]
> - **15 ene 2025, 14:00** — Se creó/modificó el documento "Reporte_Incidente_2025_042.docx" con el análisis preliminar. [OneDrive]
>
> **Personas involucradas:** [Tu nombre], [Colega mencionado]
> **Acciones tomadas:** Investigación iniciada, revisión de accesos de 48 horas solicitada al equipo.
> **Documentos:** [Enlace al reporte]

**Verificación:**

- [ ] Copilot generó una respuesta que integra información de al menos dos fuentes distintas (correo + Teams, o correo + documento).
- [ ] Las fechas y datos mencionados son consistentes con la información que creaste en la preparación.
- [ ] Copilot incluye referencias o citas a las fuentes originales.
- [ ] La respuesta está organizada de forma cronológica o estructurada.

---

## 7. Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

| # | Criterio de validación | Cumplido |
|---|------------------------|----------|
| 1 | Se ejecutó exitosamente la consulta de correos en Graph Explorer con código 200 | ☐ |
| 2 | Se navegó la jerarquía Teams → Canal → Mensajes y se localizó el mensaje del incidente | ☐ |
| 3 | Se encontró el documento del reporte mediante la búsqueda en Graph | ☐ |
| 4 | Copilot generó un resumen que correlaciona datos de múltiples fuentes | ☐ |
| 5 | Se verificó que los permisos delegados limitan el acceso solo a datos del usuario autenticado | ☐ |

**Prueba adicional de permisos:** Para verificar el modelo de seguridad, intenta ejecutar la siguiente consulta que accede a los mensajes de otro usuario (reemplaza con un ID de usuario diferente):

```
https://graph.microsoft.com/v1.0/users/{otro-user-id}/messages
```

Deberías recibir un error **403 Forbidden** o **401 Unauthorized**, confirmando que los permisos delegados no permiten acceder a datos de otros usuarios sin autorización explícita.

---

## 8. Solución de Problemas

### Problema 1: Error 403 Forbidden al ejecutar consultas en Graph Explorer

**Síntomas:**
- Al ejecutar cualquier consulta GET en el Explorador de Microsoft Graph, la respuesta devuelve el código de estado `403 Forbidden`.
- El cuerpo de la respuesta indica: `"Insufficient privileges to complete the operation"`.

**Causa:**
Los permisos necesarios (`Mail.Read`, `Chat.Read`, `Files.Read.All`) no han sido consentidos por el usuario o no están habilitados en el tenant. El Explorador de Graph requiere que el usuario otorgue consentimiento explícito para cada categoría de permiso antes de acceder a los datos correspondientes.

**Solución:**

1. En el Explorador de Microsoft Graph, haz clic en la pestaña **"Modify permissions"** (Modificar permisos) ubicada debajo del campo de consulta.
2. Localiza los permisos requeridos: `Mail.Read`, `Chat.Read`, `Files.Read.All`, `ChannelMessage.Read.All`.
3. Haz clic en **"Consent"** junto a cada permiso que muestre el estado "Not consented".
4. Se abrirá una ventana emergente de Azure AD solicitando tu aprobación. Haz clic en **"Aceptar"**.
5. Vuelve a ejecutar la consulta. Si el problema persiste, cierra sesión del explorador y vuelve a iniciar sesión.

---

### Problema 2: Copilot no encuentra información sobre el incidente o responde de forma genérica

**Síntomas:**
- Al solicitar el resumen del incidente en Copilot, la respuesta es vaga: "No encontré información específica sobre ese incidente" o genera contenido genérico sin citar fuentes reales.
- No aparecen referencias a correos, mensajes de Teams ni documentos.

**Causa:**
Esto puede ocurrir por varias razones: (a) los datos de prueba se crearon hace muy poco tiempo y el índice de búsqueda de Microsoft 365 aún no los ha procesado (la indexación puede tardar entre 5 y 30 minutos); (b) el identificador del incidente en el prompt no coincide exactamente con el texto utilizado en los correos/mensajes/documentos; (c) el tenant no tiene habilitado Copilot para Microsoft 365 o la licencia del usuario no lo incluye.

**Solución:**

1. **Espera la indexación:** Si los datos se crearon hace menos de 15 minutos, espera al menos 10-15 minutos adicionales y vuelve a intentar.
2. **Verifica la consistencia del texto:** Asegúrate de que el prompt utilice exactamente la cadena "INCIDENTE-2025-042" tal como aparece en el asunto del correo y el mensaje de Teams.
3. **Refina el prompt:** Intenta un prompt más específico:
   ```
   Busca en mis correos recientes el asunto que contenga "INCIDENTE-2025-042" 
   y en mis chats de Teams mensajes que mencionen ese mismo código. 
   También busca archivos con "Incidente_2025_042" en mi OneDrive.
   ```
4. **Verifica la licencia:** Confirma en el portal de administración de Microsoft 365 (https://admin.microsoft.com) que tu usuario tiene asignada la licencia de Microsoft 365 Copilot.

---

## 9. Limpieza

Una vez completado el laboratorio, realiza las siguientes acciones para mantener limpio tu entorno:

1. **Eliminar el correo de prueba:** En Outlook, busca el correo con asunto "[INCIDENTE-2025-042]..." y elimínalo. Vacía la papelera si deseas eliminarlo permanentemente.

2. **Eliminar el mensaje de Teams:** En el canal donde publicaste el mensaje del incidente, haz clic en los tres puntos (⋯) junto al mensaje y selecciona **"Eliminar"**.

3. **Eliminar el documento de prueba:** En OneDrive, localiza el archivo `Reporte_Incidente_2025_042.docx`, haz clic derecho y selecciona **"Eliminar"**. Vacía la papelera de reciclaje de OneDrive si es necesario.

4. **Revocar permisos opcionales:** Si deseas revocar los permisos otorgados al Explorador de Graph, navega a https://myapps.microsoft.com → Configuración de la cuenta → Permisos de aplicación, y revoca el acceso de "Graph Explorer".

---

## 10. Resumen

En este laboratorio aplicaste los conceptos de contexto organizacional y Microsoft Graph para realizar un análisis forense básico de un incidente de seguridad. Los puntos clave demostrados fueron:

- **Microsoft Graph como punto de acceso unificado:** Con una sola herramienta (Graph Explorer) consultaste tres fuentes de datos distintas: correo (Outlook), mensajes (Teams) y documentos (OneDrive).
- **Consultas estructuradas por recurso:** Cada tipo de información tiene una ruta específica en la API (`/me/messages`, `/teams/{id}/channels/{id}/messages`, `/me/drive/search`), pero todas comparten el mismo patrón de autenticación y autorización.
- **Copilot como sintetizador de contexto:** Al formular un prompt bien estructurado, Copilot consultó Microsoft Graph internamente para correlacionar información de múltiples fuentes y generar una línea de tiempo coherente del incidente.
- **Seguridad basada en permisos:** El modelo de permisos delegados garantiza que ni Graph Explorer ni Copilot acceden a datos que el usuario no tiene derecho a ver.

### Recursos adicionales

| Recurso | URL |
|---------|-----|
| Explorador de Microsoft Graph | https://developer.microsoft.com/graph/graph-explorer |
| Documentación de la API de mensajes | https://learn.microsoft.com/graph/api/message-list |
| Documentación de la API de Teams | https://learn.microsoft.com/graph/api/channel-list-messages |
| Documentación de búsqueda en OneDrive | https://learn.microsoft.com/graph/api/driveitem-search |
| Guía de prompts para Copilot | https://learn.microsoft.com/copilot/microsoft-365/microsoft-365-copilot-overview |

---

# 3 Demostración: Generación de insights ejecutivos a partir de archivos en OneDrive y SharePoint

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |

## Descripción General

En este laboratorio aplicarás los conceptos de contexto organizacional y Microsoft Graph para generar insights ejecutivos a partir de archivos almacenados en OneDrive y SharePoint. Utilizarás Microsoft Copilot dentro de Microsoft 365 para formular prompts que extraigan información consolidada de documentos dispersos, simulando un escenario real en el que un directivo necesita obtener un resumen ejecutivo sin abrir cada archivo individualmente. Comprenderás cómo Copilot consulta Microsoft Graph para localizar, leer y sintetizar contenido de archivos bajo los permisos del usuario autenticado.

## Objetivos de Aprendizaje

- [ ] Aplicar prompts en Microsoft Copilot para generar resúmenes ejecutivos a partir de archivos almacenados en OneDrive y SharePoint.
- [ ] Identificar cómo Microsoft Graph actúa como intermediario para localizar y acceder a documentos organizacionales.
- [ ] Interpretar las respuestas de Copilot reconociendo las fuentes de datos consultadas (archivos específicos, ubicaciones).
- [ ] Evaluar la calidad y relevancia de los insights generados en función del contexto organizacional disponible.

## Prerrequisitos

### Conocimientos Previos

| Requisito | Descripción |
|-----------|-------------|
| Microsoft Graph | Comprensión básica de qué es y cómo conecta servicios de Microsoft 365 (lección 3.1) |
| OneDrive y SharePoint | Familiaridad con la navegación y almacenamiento de archivos en ambas plataformas |
| Copilot para Microsoft 365 | Conocimiento básico de cómo interactuar con Copilot mediante prompts |

### Acceso Necesario

| Recurso | Detalle |
|---------|---------|
| Licencia Microsoft 365 | Con Copilot habilitado (E3/E5 + complemento Copilot, o Microsoft 365 Copilot) |
| OneDrive | Cuenta activa con archivos accesibles |
| SharePoint | Acceso a al menos un sitio de equipo con documentos compartidos |
| Navegador web | Microsoft Edge o Google Chrome actualizado |

## Entorno del Laboratorio

### Software Requerido

| Componente | Versión / Requisito |
|------------|---------------------|
| Microsoft 365 | Con licencia Copilot activa |
| Microsoft Teams o Copilot en Microsoft365.com | Versión web o de escritorio |
| OneDrive for Business | Con al menos 3 archivos de ejemplo |
| SharePoint Online | Sitio de equipo con biblioteca de documentos |

### Preparación del Entorno

Antes de iniciar el laboratorio, asegúrate de tener archivos de ejemplo disponibles. Si no cuentas con documentos reales del proyecto, crea los siguientes archivos de prueba:

**Paso de preparación: Crear archivos de ejemplo**

1. Abre OneDrive (https://onedrive.com) e inicia sesión con tu cuenta organizacional.

2. Crea una carpeta llamada `Proyecto-Ejecutivo-Lab` en la raíz de tu OneDrive.

3. Dentro de esa carpeta, crea los siguientes tres archivos de Word:

**Archivo 1: `Resumen-Financiero-Q4.docx`**
Contenido sugerido:
```
Resumen Financiero Q4 2024
- Ingresos totales: $2.4M (incremento del 12% vs Q3)
- Gastos operativos: $1.8M (reducción del 5%)
- Margen neto: 25%
- Principales drivers: expansión en mercado latinoamericano y reducción de costos logísticos.
- Riesgo identificado: dependencia del 40% en un solo cliente corporativo.
```

**Archivo 2: `Avance-Proyecto-Transformacion.docx`**
Contenido sugerido:
```
Avance Proyecto Transformación Digital - Enero 2025
- Fase 1 completada: migración a la nube (100%)
- Fase 2 en progreso: automatización de procesos (65%)
- Bloqueante: integración con sistema legacy ERP prevista para febrero
- Equipo: 12 personas asignadas, 2 vacantes por cubrir
- Presupuesto ejecutado: 70% del total aprobado
- Próximo hito: demo ejecutiva programada para el 15 de febrero
```

**Archivo 3: `Indicadores-RRHH-Enero.docx`**
Contenido sugerido:
```
Indicadores de Recursos Humanos - Enero 2025
- Headcount: 245 empleados (3 nuevas incorporaciones)
- Rotación mensual: 1.2% (por debajo del objetivo del 2%)
- Satisfacción laboral (encuesta): 4.2/5.0
- Capacitaciones completadas: 18 sesiones, 89% de asistencia
- Incidencias reportadas: 2 (resueltas)
- Prioridad: plan de sucesión para 3 posiciones directivas
```

4. Opcionalmente, sube uno de los archivos a un sitio de SharePoint al que tengas acceso, para simular documentos distribuidos entre OneDrive y SharePoint.

## Procedimiento Paso a Paso

### Paso 1: Verificar la Disponibilidad de Archivos mediante Copilot

**Objetivo:** Confirmar que Copilot puede acceder a los archivos almacenados en OneDrive y SharePoint a través de Microsoft Graph.

**Instrucciones:**

1. Abre Microsoft 365 Copilot. Puedes hacerlo de las siguientes formas:
   - Navega a https://microsoft365.com/copilot en tu navegador.
   - O abre Microsoft Teams y accede al chat de Copilot.

2. En el campo de prompt, escribe la siguiente consulta:

   ```
   ¿Qué archivos he modificado recientemente en mi OneDrive relacionados con "Proyecto" o "Financiero"?
   ```

3. Presiona Enter y espera la respuesta de Copilot.

4. Observa cómo Copilot lista los archivos encontrados, incluyendo nombres, ubicaciones y fechas de modificación.

**Resultado Esperado:**

Copilot debería devolver una lista que incluya al menos los archivos `Resumen-Financiero-Q4.docx` y `Avance-Proyecto-Transformacion.docx` que creaste en la preparación. La respuesta incluirá referencias con enlaces directos a los archivos.

**Verificación:**

- [ ] Copilot muestra al menos 2 de los 3 archivos creados.
- [ ] Las referencias incluyen la ubicación (OneDrive o SharePoint).
- [ ] Los archivos listados coinciden con aquellos a los que tienes permisos de acceso.

> **Nota conceptual:** En este momento, Copilot está realizando una consulta equivalente a `GET https://graph.microsoft.com/v1.0/me/drive/search(q='Proyecto')` a través de Microsoft Graph, utilizando tus permisos delegados para acceder únicamente a tus archivos.

---

### Paso 2: Generar un Resumen Ejecutivo Consolidado

**Objetivo:** Utilizar Copilot para sintetizar información de múltiples archivos en un único resumen ejecutivo, demostrando la capacidad de Microsoft Graph para agregar datos de distintas fuentes.

**Instrucciones:**

1. En el mismo chat de Copilot, escribe el siguiente prompt:

   ```
   Basándote en los archivos "Resumen-Financiero-Q4", "Avance-Proyecto-Transformacion" y "Indicadores-RRHH-Enero" que tengo en mi OneDrive, genera un resumen ejecutivo de una página para presentar al comité directivo. Incluye: situación financiera, avance de proyectos estratégicos y estado del capital humano. Destaca los 3 riesgos principales y las 3 oportunidades más relevantes.
   ```

2. Presiona Enter y espera que Copilot procese la solicitud.

3. Lee la respuesta generada. Copilot debería producir un documento estructurado con secciones claras.

4. Observa las **referencias** al final de la respuesta: Copilot indicará de qué archivos específicos extrajo cada dato.

**Resultado Esperado:**

Una respuesta estructurada similar a:

```
## Resumen Ejecutivo - Comité Directivo

### Situación Financiera
- Ingresos Q4: $2.4M (+12% vs Q3)
- Margen neto: 25%
- Reducción de gastos operativos del 5%

### Proyectos Estratégicos
- Transformación Digital: Fase 1 completa, Fase 2 al 65%
- Próximo hito: demo ejecutiva el 15 de febrero
- Presupuesto ejecutado: 70%

### Capital Humano
- 245 empleados, rotación del 1.2%
- Satisfacción: 4.2/5.0
- 18 capacitaciones realizadas

### Riesgos Principales
1. Dependencia del 40% en un solo cliente corporativo
2. Bloqueante en integración con sistema legacy ERP
3. 3 posiciones directivas sin plan de sucesión definido

### Oportunidades
1. Expansión en mercado latinoamericano como driver de crecimiento
2. Automatización de procesos (Fase 2) para eficiencia operativa
3. Alta satisfacción laboral como base para retención de talento

[Referencias: Resumen-Financiero-Q4.docx, Avance-Proyecto-Transformacion.docx, Indicadores-RRHH-Enero.docx]
```

**Verificación:**

- [ ] El resumen incluye información de los tres archivos fuente.
- [ ] La estructura es clara y apta para un comité directivo.
- [ ] Se identifican riesgos y oportunidades extraídos del contenido real de los archivos.
- [ ] Las referencias al pie confirman las fuentes consultadas.

---

### Paso 3: Refinar el Insight con Preguntas de Seguimiento

**Objetivo:** Demostrar que Copilot mantiene el contexto de la conversación y puede profundizar en aspectos específicos de los archivos consultados, aprovechando la conexión persistente con Microsoft Graph.

**Instrucciones:**

1. Sin abrir un nuevo chat, escribe el siguiente prompt de seguimiento:

   ```
   Del resumen anterior, profundiza en el riesgo de dependencia del cliente corporativo. ¿Qué datos del archivo financiero respaldan esta preocupación? Sugiere 2 acciones concretas para mitigarlo.
   ```

2. Presiona Enter y revisa la respuesta.

3. A continuación, escribe un segundo prompt de refinamiento:

   ```
   Ahora formatea todo el resumen ejecutivo que generaste como una tabla comparativa con columnas: Área, Estado Actual, Riesgo Asociado, Acción Recomendada.
   ```

4. Revisa la tabla generada.

**Resultado Esperado:**

Para el primer prompt de seguimiento, Copilot debería:
- Citar el dato específico del 40% de dependencia del archivo financiero.
- Proponer acciones como diversificación de cartera de clientes o negociación de contratos a largo plazo.

Para el segundo prompt, debería generar una tabla con formato similar a:

| Área | Estado Actual | Riesgo Asociado | Acción Recomendada |
|------|---------------|-----------------|-------------------|
| Finanzas | Margen 25%, crecimiento 12% | Concentración en un cliente (40%) | Diversificar cartera comercial |
| Proyectos | Fase 2 al 65% | Integración ERP bloqueada | Asignar equipo dedicado a integración |
| RRHH | Satisfacción 4.2/5, rotación 1.2% | Sucesión directiva sin plan | Implementar programa de sucesión Q1 |

**Verificación:**

- [ ] Copilot mantiene el contexto de los archivos consultados previamente sin necesidad de volver a nombrarlos.
- [ ] Las respuestas de seguimiento son coherentes con los datos originales.
- [ ] La tabla formateada es clara y accionable para un público ejecutivo.

---

### Paso 4: Identificar las Consultas de Microsoft Graph Implícitas

**Objetivo:** Relacionar la experiencia práctica con la arquitectura técnica, identificando qué rutas de Microsoft Graph se invocan detrás de cada interacción con Copilot.

**Instrucciones:**

1. Revisa las interacciones realizadas en los pasos anteriores.

2. Para cada acción, identifica la consulta de Microsoft Graph que Copilot ejecutó de forma implícita. Completa la siguiente tabla en un documento o en tus notas:

| Acción del Usuario | Ruta de Microsoft Graph (estimada) | Tipo de Permiso |
|---|---|---|
| Buscar archivos recientes relacionados con "Proyecto" | `/me/drive/search(q='Proyecto')` | Delegado |
| Acceder al contenido del archivo financiero | `/me/drive/items/{item-id}/content` | Delegado |
| Buscar archivos en SharePoint | `/sites/{site-id}/drive/search(q='...')` | Delegado |
| Obtener metadatos de archivos (fecha, autor) | `/me/drive/items/{item-id}` | Delegado |

3. Reflexiona sobre la siguiente pregunta: ¿Qué sucedería si un colega sin acceso a la carpeta `Proyecto-Ejecutivo-Lab` hiciera la misma pregunta a Copilot?

**Resultado Esperado:**

El participante comprende que:
- Cada interacción con Copilot genera múltiples consultas a Microsoft Graph.
- Todas las consultas utilizan permisos delegados (en nombre del usuario).
- Un usuario sin permisos sobre esos archivos NO recibiría la misma información, ya que Graph respeta los controles de acceso.

**Verificación:**

- [ ] Se identifican al menos 3 rutas de Microsoft Graph correctas.
- [ ] Se comprende que el modelo de seguridad impide el acceso no autorizado.
- [ ] Se distingue entre permisos delegados y de aplicación en este contexto.

## Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

| Criterio | Cumplido |
|----------|----------|
| Copilot accedió correctamente a los archivos en OneDrive | ☐ |
| Se generó un resumen ejecutivo consolidado de múltiples fuentes | ☐ |
| Las referencias en la respuesta apuntan a los archivos correctos | ☐ |
| Los prompts de seguimiento mantuvieron el contexto | ☐ |
| Se identificaron las rutas de Microsoft Graph implícitas | ☐ |
| Se comprende el rol de los permisos delegados en la seguridad | ☐ |

**Prueba adicional (opcional):** Pide a un compañero que NO tenga acceso a tu carpeta `Proyecto-Ejecutivo-Lab` que realice el mismo prompt. Confirma que Copilot no le devuelve información de tus archivos, validando así el modelo de seguridad de Microsoft Graph.

## Solución de Problemas

### Problema 1: Copilot no encuentra los archivos creados

**Síntomas:** Al preguntar por archivos recientes o buscar por nombre, Copilot responde que no encuentra documentos relevantes o devuelve resultados vacíos.

**Causa:** Microsoft Graph indexa los archivos de forma asíncrona. Los archivos recién creados o subidos pueden tardar entre 5 y 15 minutos en ser indexados y estar disponibles para búsqueda. Además, si los archivos se crearon en una ubicación no sincronizada o en una cuenta personal en lugar de la organizacional, Graph no los localizará.

**Solución:**
1. Espera al menos 10 minutos después de crear los archivos antes de realizar la consulta.
2. Verifica que los archivos están en OneDrive **for Business** (cuenta organizacional), no en OneDrive personal.
3. Abre uno de los archivos desde OneDrive web para forzar la indexación.
4. Intenta buscar directamente en OneDrive web para confirmar que el índice de búsqueda los incluye.
5. Si persiste, cierra sesión y vuelve a iniciar sesión en Copilot para refrescar el token de autenticación.

---

### Problema 2: Copilot genera un resumen genérico sin datos específicos de los archivos

**Síntomas:** La respuesta de Copilot es un resumen ejecutivo con estructura correcta pero con contenido genérico o inventado, sin citar datos reales de los archivos (por ejemplo, no menciona los $2.4M ni el 65% de avance).

**Causa:** Esto puede ocurrir cuando el prompt no es suficientemente específico para que Copilot identifique los archivos correctos, o cuando existen múltiples archivos con nombres similares y Copilot selecciona otros. También puede suceder si los archivos tienen muy poco contenido o están en formato no compatible (por ejemplo, PDF escaneado sin OCR).

**Solución:**
1. Reformula el prompt incluyendo el nombre exacto de los archivos entre comillas: `"Resumen-Financiero-Q4.docx"`.
2. Usa el botón de adjuntar archivo (icono de clip) en la interfaz de Copilot para referenciar directamente los archivos antes de escribir el prompt.
3. Verifica que los archivos están en formato `.docx` editable y no como PDF imagen.
4. Si hay ambigüedad, especifica la ubicación: "los archivos en mi carpeta Proyecto-Ejecutivo-Lab de OneDrive".
5. Revisa que las referencias al final de la respuesta apunten a tus archivos; si no aparecen referencias, Copilot no consultó Graph exitosamente.

## Limpieza

Una vez completado el laboratorio, realiza las siguientes acciones para mantener ordenado tu entorno:

1. **Opcional — Conservar archivos:** Si deseas conservar los archivos de ejemplo para prácticas futuras, muévelos a una carpeta de archivo:
   - En OneDrive, renombra la carpeta a `Lab-Completado-Proyecto-Ejecutivo`.

2. **Eliminar archivos de prueba:** Si no necesitas los archivos:
   - Navega a OneDrive > `Proyecto-Ejecutivo-Lab`.
   - Selecciona los tres archivos y la carpeta.
   - Haz clic en **Eliminar**.
   - Vacía la papelera de reciclaje de OneDrive si deseas una eliminación permanente.

3. **Limpiar historial de chat (opcional):**
   - En Copilot, puedes iniciar un nuevo tema para que el contexto de esta sesión no interfiera con futuras consultas.

## Resumen

En este laboratorio has aplicado de forma práctica los conceptos de Microsoft Graph y contexto organizacional para:

- **Demostrar** que Copilot accede a archivos en OneDrive y SharePoint mediante consultas implícitas a Microsoft Graph.
- **Generar** un resumen ejecutivo consolidado a partir de múltiples documentos sin necesidad de abrirlos individualmente.
- **Refinar** los insights mediante prompts de seguimiento, aprovechando la persistencia del contexto conversacional.
- **Identificar** las rutas de API y el modelo de permisos delegados que garantizan la seguridad del acceso a datos.

### Conceptos Clave Reforzados

| Concepto | Aplicación en el Lab |
|----------|---------------------|
| Microsoft Graph como API unificada | Copilot consultó `/me/drive/search` y `/me/drive/items` para localizar y leer archivos |
| Contexto organizacional | Los archivos personales del usuario formaron la base del resumen ejecutivo |
| Permisos delegados | Solo se accedió a archivos que el usuario autenticado podía ver |
| Síntesis multi-fuente | Copilot combinó datos de 3 archivos distintos en una respuesta coherente |

### Recursos Adicionales

- [Microsoft Graph Explorer — Herramienta interactiva](https://developer.microsoft.com/es-es/graph/graph-explorer)
- [Documentación: Buscar archivos en OneDrive vía Graph](https://learn.microsoft.com/es-es/graph/api/driveitem-search)
- [Mejores prácticas para prompts en Copilot para Microsoft 365](https://learn.microsoft.com/es-es/copilot/microsoft-365/microsoft-365-copilot-prompt-guide)
- [Permisos de archivos en Microsoft Graph](https://learn.microsoft.com/es-es/graph/api/resources/permission)

---

# 4 Demostración: Generación de reportes operativos integrando información de Teams, Outlook, Excel, Word y PowerPoint almacenada en OneDrive y SharePoint

## Metadata

| Campo | Detalle |
|-------|---------|
| **Duración** | 15 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Crear |

## Descripción General

En este laboratorio crearás un reporte operativo semanal utilizando Microsoft 365 Copilot como motor de integración. Aplicarás los conceptos de Microsoft Graph y contexto organizacional para solicitar a Copilot que consolide información dispersa en múltiples servicios (Teams, Outlook, Excel, Word y PowerPoint) almacenada en OneDrive y SharePoint, generando un documento unificado que sintetice el estado operativo de un proyecto ficticio.

## Objetivos de Aprendizaje

Al finalizar este laboratorio serás capaz de:

- [ ] Formular prompts efectivos en Copilot que integren datos de múltiples fuentes del ecosistema Microsoft 365
- [ ] Generar un reporte operativo consolidado que combine información de Teams, Outlook, Excel, Word y PowerPoint
- [ ] Verificar que Copilot accede al contexto organizacional correcto mediante Microsoft Graph para producir respuestas relevantes
- [ ] Identificar las fuentes de datos que Copilot consulta al generar reportes integrales

## Prerrequisitos

### Conocimientos Previos

| Requisito | Nivel |
|-----------|-------|
| Comprensión de Microsoft Graph y contexto organizacional (Lección 3.1) | Esencial |
| Uso básico de Microsoft Teams, Outlook y OneDrive | Esencial |
| Navegación en SharePoint | Recomendado |
| Formulación de prompts para Copilot | Recomendado |

### Acceso Requerido

| Recurso | Detalle |
|---------|---------|
| Licencia Microsoft 365 | Con Copilot habilitado (E3/E5 + complemento Copilot) |
| Microsoft Teams | Acceso con al menos un equipo activo |
| OneDrive for Business | Almacenamiento personal activo |
| SharePoint Online | Acceso a al menos un sitio de equipo |
| Outlook (web o escritorio) | Buzón activo con correos recientes |

## Entorno del Laboratorio

### Software Necesario

| Aplicación | Versión Mínima | Propósito |
|------------|----------------|-----------|
| Microsoft Teams | Última versión disponible | Chat con Copilot y fuente de datos |
| Microsoft Word | Microsoft 365 (web o escritorio) | Generación del reporte final |
| Microsoft Excel | Microsoft 365 (web o escritorio) | Fuente de datos numéricos |
| Microsoft PowerPoint | Microsoft 365 (web o escritorio) | Fuente de datos de presentación |
| Navegador web | Edge/Chrome actualizado | Acceso a SharePoint y OneDrive |

### Preparación del Entorno

Antes de iniciar los pasos del laboratorio, necesitas crear los archivos de muestra que simularán la información operativa del proyecto. Esto garantiza que Copilot tenga contexto suficiente para generar el reporte.

**Paso de preparación 1:** Crea un archivo Excel en OneDrive con datos operativos.

1. Abre OneDrive for Business (`https://onedrive.com`)
2. Crea una nueva carpeta llamada `Proyecto-Operaciones-Lab`
3. Dentro de la carpeta, crea un nuevo libro de Excel llamado `KPIs_Semanales.xlsx`
4. Ingresa los siguientes datos en la Hoja1:

| Métrica | Lunes | Martes | Miércoles | Jueves | Viernes |
|---------|-------|--------|-----------|--------|---------|
| Tickets resueltos | 12 | 15 | 9 | 18 | 14 |
| Tiempo promedio resolución (hrs) | 2.3 | 1.8 | 3.1 | 1.5 | 2.0 |
| Satisfacción cliente (%) | 87 | 91 | 82 | 94 | 89 |
| Incidentes críticos | 1 | 0 | 2 | 0 | 1 |

5. Guarda el archivo.

**Paso de preparación 2:** Crea un documento Word con el resumen del proyecto.

1. En la misma carpeta `Proyecto-Operaciones-Lab`, crea un documento Word llamado `Resumen_Proyecto_Alfa.docx`
2. Escribe el siguiente contenido:

```
Proyecto Alfa - Resumen Ejecutivo

Estado: En progreso
Fase actual: Implementación (Semana 3 de 8)
Líder del proyecto: Laura Martínez
Equipo: 6 personas

Hitos completados:
- Definición de alcance (completado)
- Diseño de arquitectura (completado)
- Configuración de infraestructura (completado)

Próximos hitos:
- Migración de datos (en progreso, 60% completado)
- Pruebas de integración (pendiente, inicio semana 4)

Riesgos identificados:
- Retraso potencial en migración de datos por dependencia con proveedor externo
- Disponibilidad limitada del equipo de QA en semana 5
```

3. Guarda el documento.

**Paso de preparación 3:** Crea una presentación PowerPoint con avances.

1. En la misma carpeta, crea una presentación llamada `Avance_Semanal_Alfa.pptx`
2. Crea 3 diapositivas con el siguiente contenido:

- **Diapositiva 1 (Título):** "Proyecto Alfa - Avance Semana 3"
- **Diapositiva 2 (Contenido):** "Logros de la semana: Completada migración del módulo de facturación. Integración con API de pagos validada. Documentación técnica actualizada al 80%."
- **Diapositiva 3 (Contenido):** "Bloqueos: Esperando credenciales del proveedor CloudSync. Servidor de staging con problemas de rendimiento desde miércoles."

3. Guarda la presentación.

**Paso de preparación 4:** Envía un correo de prueba desde Outlook.

1. Abre Outlook y envíate un correo a ti mismo con:
   - **Asunto:** "Actualización Proyecto Alfa - Reunión con proveedor"
   - **Cuerpo:** "Hola equipo, confirmo que la reunión con CloudSync se reprogramó para el jueves a las 10:00. Necesitamos tener listos los requisitos de migración antes de esa fecha. Saludos, Laura"

2. Asegúrate de que el correo aparezca en tu bandeja de entrada.

**Paso de preparación 5:** Publica un mensaje en Teams.

1. Abre Microsoft Teams
2. En un canal de cualquier equipo al que pertenezcas (o crea un chat grupal de prueba), publica el siguiente mensaje:

```
Equipo Alfa - Actualización rápida: Los KPIs de esta semana muestran mejora en satisfacción del cliente (promedio 88.6%). El incidente crítico del miércoles fue resuelto en 4 horas. Seguimos en verde para el hito de migración.
```

## Procedimiento Paso a Paso

### Paso 1: Solicitar a Copilot un resumen integrado del proyecto

**Objetivo:** Utilizar Copilot en Microsoft Teams para generar una primera síntesis que demuestre cómo consulta múltiples fuentes a través de Microsoft Graph.

**Instrucciones:**

1. Abre Microsoft Teams y navega al chat de **Copilot** (ícono de Copilot en la barra lateral izquierda o en el cuadro de chat).

2. Escribe el siguiente prompt:

```
Resume el estado actual del Proyecto Alfa considerando:
- Los documentos almacenados en mi OneDrive en la carpeta "Proyecto-Operaciones-Lab"
- Los correos recientes que mencionen "Proyecto Alfa"
- Los mensajes de Teams relacionados con "Equipo Alfa"

Incluye: estado general, métricas clave, logros recientes y bloqueos pendientes.
```

3. Presiona Enter y espera la respuesta de Copilot.

4. Observa cómo Copilot referencia las fuentes consultadas (normalmente aparecen como enlaces o citas numeradas al final de la respuesta).

**Resultado Esperado:**

Copilot generará un resumen que integra información del documento Word (estado del proyecto, hitos), la presentación PowerPoint (logros y bloqueos), los datos de Excel (métricas) y los mensajes/correos relacionados. La respuesta incluirá referencias a los archivos fuente consultados.

Ejemplo de estructura de respuesta esperada:
```
📋 Estado del Proyecto Alfa - Resumen Integrado

**Estado general:** En progreso - Fase de Implementación (Semana 3/8)

**Métricas clave de la semana:**
- Tickets resueltos: 68 total (promedio 13.6/día)
- Satisfacción del cliente: 88.6% promedio
- Incidentes críticos: 4 en la semana

**Logros recientes:**
- Migración del módulo de facturación completada
- Integración con API de pagos validada
[...]

**Bloqueos:**
- Pendiente recibir credenciales de CloudSync
- Problemas de rendimiento en servidor de staging
[...]

Fuentes: [1] Resumen_Proyecto_Alfa.docx [2] KPIs_Semanales.xlsx [3] Avance_Semanal_Alfa.pptx
```

**Verificación:**

- [ ] La respuesta menciona datos específicos del archivo Excel (números de KPIs)
- [ ] La respuesta incluye información del documento Word (fase, hitos)
- [ ] La respuesta referencia logros/bloqueos de la presentación PowerPoint
- [ ] Copilot muestra las fuentes/referencias consultadas

### Paso 2: Generar el reporte operativo formal en Word con Copilot

**Objetivo:** Crear un documento de reporte operativo estructurado utilizando Copilot en Word, que integre las múltiples fuentes de datos del proyecto.

**Instrucciones:**

1. Abre Microsoft Word (versión web en `https://word.new` o aplicación de escritorio).

2. En un documento nuevo, activa Copilot haciendo clic en el ícono de Copilot en la barra de herramientas (o presiona Alt+I para invocar "Redactar con Copilot").

3. En el cuadro de prompt de Copilot, escribe:

```
Crea un reporte operativo semanal profesional para el Proyecto Alfa con las siguientes secciones:

1. Resumen ejecutivo
2. KPIs de la semana (usa los datos de /KPIs_Semanales.xlsx)
3. Logros alcanzados (basado en /Avance_Semanal_Alfa.pptx)
4. Riesgos y bloqueos
5. Próximos pasos
6. Comunicaciones relevantes (basado en correos y mensajes de Teams sobre Proyecto Alfa)

Usa un tono profesional y ejecutivo. Incluye una tabla para los KPIs.
```

4. Para referenciar archivos específicos, utiliza el carácter `/` dentro del prompt de Copilot en Word, lo que desplegará un selector de archivos. Selecciona:
   - `KPIs_Semanales.xlsx`
   - `Resumen_Proyecto_Alfa.docx`
   - `Avance_Semanal_Alfa.pptx`

5. Haz clic en **Generar** y espera que Copilot produzca el documento.

6. Revisa el contenido generado. Si alguna sección necesita más detalle, posiciona el cursor en esa sección y solicita a Copilot:

```
Expande esta sección con más detalle basándote en los archivos del Proyecto Alfa en mi OneDrive.
```

**Resultado Esperado:**

Word generará un documento con formato profesional que incluya:
- Encabezado con título, fecha y autor
- Sección de resumen ejecutivo con el estado general
- Tabla de KPIs con los valores numéricos del Excel
- Lista de logros extraídos de la presentación
- Sección de riesgos combinando información del Word y PowerPoint fuente
- Próximos pasos basados en los hitos pendientes
- Referencia a comunicaciones del correo sobre la reunión con CloudSync

**Verificación:**

- [ ] El documento contiene una tabla con datos numéricos provenientes del archivo Excel
- [ ] Los logros mencionados corresponden al contenido de la presentación PowerPoint
- [ ] Los riesgos incluyen información del documento Word fuente
- [ ] El formato es profesional con encabezados, secciones numeradas y tabla

### Paso 3: Enriquecer el reporte con análisis de tendencias desde Excel

**Objetivo:** Demostrar cómo Copilot en Excel puede generar análisis complementarios que alimenten el reporte operativo.

**Instrucciones:**

1. Abre el archivo `KPIs_Semanales.xlsx` desde OneDrive.

2. Activa Copilot en Excel (panel lateral derecho o botón de Copilot en la cinta).

3. Escribe el siguiente prompt en Copilot de Excel:

```
Analiza las tendencias de esta semana. ¿Cuál fue el mejor día en satisfacción del cliente? ¿Hay correlación entre los incidentes críticos y la caída en satisfacción? Resume los hallazgos clave en 3-4 puntos.
```

4. Revisa la respuesta de Copilot. Debería identificar:
   - Jueves como el mejor día (94% satisfacción, 0 incidentes)
   - Miércoles como el peor día (82% satisfacción, 2 incidentes)
   - Correlación negativa entre incidentes y satisfacción

5. Solicita a Copilot que genere una fórmula de promedio:

```
Agrega una fila con los promedios de cada métrica al final de la tabla.
```

6. Copia los hallazgos clave para incorporarlos al reporte de Word.

**Resultado Esperado:**

Copilot en Excel proporcionará:
- Análisis textual identificando patrones en los datos
- Identificación del día con mejor/peor rendimiento
- Observación sobre la relación incidentes-satisfacción
- Fórmula de promedio aplicada (ejemplo: `=AVERAGE(B2:F2)`)

**Verificación:**

- [ ] Copilot identifica correctamente el jueves como mejor día en satisfacción
- [ ] El análisis menciona la correlación entre incidentes críticos y satisfacción
- [ ] Se genera la fila de promedios con valores calculados

### Paso 4: Consolidar el reporte final y validar las fuentes

**Objetivo:** Completar el reporte operativo incorporando los hallazgos de Excel y verificar que todas las fuentes de Microsoft Graph fueron consultadas correctamente.

**Instrucciones:**

1. Regresa al documento de reporte en Word creado en el Paso 2.

2. Posiciona el cursor al final de la sección de KPIs.

3. Invoca Copilot y escribe:

```
Agrega un párrafo de análisis después de la tabla de KPIs que incluya:
- El promedio semanal de satisfacción del cliente fue 88.6%
- El jueves fue el día con mejor desempeño (94% satisfacción, 0 incidentes)
- Existe correlación entre incidentes críticos y caída en satisfacción
- Recomendación: investigar causa raíz de incidentes del miércoles
```

4. Acepta la inserción generada por Copilot.

5. Al final del documento, solicita a Copilot agregar una sección de cierre:

```
Agrega una sección final llamada "Conclusión y Recomendaciones" que sintetice los puntos más importantes del reporte y liste 3 acciones prioritarias para la próxima semana basándote en toda la información del documento.
```

6. Revisa el documento completo y guárdalo como `Reporte_Operativo_Semanal_Alfa_S3.docx` en la carpeta `Proyecto-Operaciones-Lab`.

**Resultado Esperado:**

El documento final contendrá:
- Resumen ejecutivo (fuente: Word + contexto general)
- Tabla de KPIs con análisis (fuente: Excel)
- Logros de la semana (fuente: PowerPoint)
- Riesgos y bloqueos (fuente: Word + PowerPoint)
- Comunicaciones relevantes (fuente: Outlook + Teams)
- Conclusión con acciones prioritarias (síntesis de Copilot)

Esto demuestra cómo Microsoft Graph permite a Copilot acceder simultáneamente a datos de 5 servicios diferentes (Teams, Outlook, Excel, Word, PowerPoint) almacenados en OneDrive/SharePoint.

**Verificación:**

- [ ] El reporte final contiene información verificable de al menos 4 fuentes distintas
- [ ] La sección de conclusiones refleja una síntesis coherente de todo el contenido
- [ ] El documento está guardado en OneDrive con nombre descriptivo
- [ ] Las acciones prioritarias son relevantes y se derivan lógicamente de los datos presentados

## Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

| # | Criterio de Validación | Cumplido |
|---|------------------------|----------|
| 1 | Copilot en Teams generó un resumen que referencia múltiples fuentes | ☐ |
| 2 | El reporte en Word contiene datos numéricos del archivo Excel | ☐ |
| 3 | El reporte incluye logros extraídos de la presentación PowerPoint | ☐ |
| 4 | El reporte menciona comunicaciones de Outlook y/o Teams | ☐ |
| 5 | Copilot en Excel identificó tendencias en los KPIs | ☐ |
| 6 | El documento final tiene estructura profesional con al menos 5 secciones | ☐ |
| 7 | Se puede identificar qué consultas de Microsoft Graph realizó Copilot (por las fuentes citadas) | ☐ |

**Prueba final:** Abre el reporte terminado y verifica que puedas trazar cada dato hasta su fuente original:

```
Dato en el reporte          →  Fuente original (servicio de M365)
─────────────────────────────────────────────────────────────────
KPIs numéricos              →  Excel (OneDrive)
Estado del proyecto/hitos   →  Word (OneDrive)
Logros/bloqueos técnicos    →  PowerPoint (OneDrive)
Reunión con proveedor       →  Outlook (correo)
Actualización de equipo     →  Teams (mensaje en canal)
```

## Solución de Problemas

### Problema 1: Copilot no encuentra los archivos referenciados

**Síntomas:** Al usar el carácter `/` en Word o al mencionar los archivos en el prompt, Copilot responde que no puede encontrar los documentos o genera contenido genérico sin datos específicos del proyecto.

**Causa:** Los archivos fueron creados hace menos de unos minutos y el índice de Microsoft Graph aún no los ha procesado. Microsoft Graph indexa contenido de forma asíncrona y puede tardar entre 5 y 15 minutos en hacer disponibles archivos nuevos para búsqueda.

**Solución:**
1. Espera 5-10 minutos después de crear los archivos antes de invocar Copilot.
2. Abre cada archivo al menos una vez para acelerar la indexación.
3. Verifica que los archivos aparecen en la búsqueda de Microsoft 365 (barra de búsqueda en `office.com`). Si aparecen allí, Copilot debería poder acceder a ellos.
4. Como alternativa, usa el selector de archivos (`/`) en el prompt de Copilot en Word y navega manualmente hasta la carpeta en OneDrive para seleccionar los archivos.

### Problema 2: Copilot genera contenido genérico sin integrar las fuentes específicas

**Síntomas:** La respuesta de Copilot tiene estructura correcta pero usa datos inventados o genéricos en lugar de los valores reales de los archivos (por ejemplo, muestra KPIs diferentes a los ingresados en Excel).

**Causa:** El prompt no fue suficientemente específico en la referencia a los archivos, o se alcanzó el límite de contexto de Copilot al intentar procesar múltiples fuentes simultáneamente.

**Solución:**
1. Reformula el prompt siendo más explícito con los nombres de archivo exactos:
   ```
   Usa específicamente el archivo "KPIs_Semanales.xlsx" de mi carpeta 
   "Proyecto-Operaciones-Lab" en OneDrive para la tabla de métricas.
   ```
2. Divide la solicitud en pasos más pequeños: primero genera el reporte base, luego pide insertar los datos de Excel en una segunda interacción.
3. Verifica que los archivos no están protegidos con permisos restrictivos que impidan a Copilot leerlos (deben tener al menos permiso de lectura para tu usuario).
4. Si persiste, abre el archivo Excel directamente y usa Copilot dentro de Excel para obtener el análisis, luego cópialo manualmente al reporte de Word.

## Limpieza

Una vez completado el laboratorio, puedes optar por conservar o eliminar los archivos de práctica:

**Para conservar (recomendado para referencia futura):**
- Mantén la carpeta `Proyecto-Operaciones-Lab` en OneDrive como ejemplo de integración de fuentes.

**Para eliminar:**
1. Navega a OneDrive → carpeta `Proyecto-Operaciones-Lab`
2. Selecciona todos los archivos y elimínalos
3. Vacía la papelera de reciclaje de OneDrive si deseas eliminación permanente
4. Elimina el correo de prueba de Outlook
5. Elimina el mensaje de prueba en Teams (clic derecho → Eliminar)

## Resumen

En este laboratorio has demostrado cómo Microsoft 365 Copilot, respaldado por Microsoft Graph, actúa como integrador de información organizacional para generar reportes operativos completos. Los conceptos clave aplicados fueron:

- **Microsoft Graph como API unificada:** Copilot consultó simultáneamente datos de Teams (`/me/joinedTeams`), Outlook (`/me/messages`), y OneDrive (`/me/drive`) a través de un único punto de acceso.
- **Contexto organizacional dinámico:** El reporte generado refleja el estado actual y real del proyecto, no información estática.
- **Permisos delegados en acción:** Copilot accedió únicamente a los archivos y mensajes que tu usuario tiene autorización para ver.
- **Integración multi-servicio:** Un solo documento consolidó información de 5 aplicaciones diferentes (Teams, Outlook, Excel, Word, PowerPoint) sin necesidad de abrir cada una por separado.

### Conexión con la Teoría

Las consultas que Copilot realizó internamente durante este laboratorio siguen el mismo patrón descrito en la lección 3.1:

```
GET /me/drive/search(q='Alfa')          → Archivos en OneDrive
GET /me/messages?$search="Alfa"         → Correos en Outlook  
GET /me/joinedTeams/.../messages        → Mensajes en Teams
```

Esto confirma que el contexto organizacional es el ingrediente que transforma a Copilot de un generador de texto genérico en un asistente que produce reportes basados en datos reales de tu organización.

### Recursos Adicionales

- [Documentación de Microsoft Graph — Búsqueda de archivos](https://learn.microsoft.com/es-es/graph/api/driveitem-search)
- [Copilot en Word — Crear contenido con referencias](https://support.microsoft.com/es-es/copilot-word)
- [Copilot en Excel — Análisis de datos](https://support.microsoft.com/es-es/copilot-excel)
- [Explorador de Microsoft Graph](https://developer.microsoft.com/es-es/graph/graph-explorer)
