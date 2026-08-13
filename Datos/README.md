# Datos del seminario

Archivos ficticios que alimentan las guías prácticas. **Banco Aurora** es una institución inventada, igual que sus empleados, clientes, proveedores y todas las cifras que aparecen aquí.

## Preparación única antes del seminario

Haz esto **una vez**, al menos 30 minutos antes de la sesión, para que Microsoft 365 Copilot alcance a indexar el contenido.

### 1. Subir los documentos a OneDrive

1. Abre [onedrive.com](https://onedrive.com) e inicia sesión con tu cuenta **corporativa** (OneDrive for Business).
2. Pulsa **+ Agregar nuevo** → **Carpeta** y crea una carpeta llamada `Seminario Copilot`.
3. Entra en la carpeta y crea dentro tres subcarpetas: `Capitulo01`, `Capitulo02` y `Capitulo03`.
4. Arrastra a cada subcarpeta los archivos `.docx`, `.xlsx` y `.pptx` de la carpeta correspondiente de este repositorio.
5. Abre una vez cada archivo desde OneDrive. Eso acelera la indexación.

Los archivos `.eml` y `.txt` se quedan fuera de OneDrive: su contenido va a Outlook y a Teams.

### 2. Importar los correos a Outlook

Solo aplica al capítulo 3. Ver [Cómo importar los correos .eml a Outlook](#cómo-importar-los-correos-eml-a-outlook).

### 3. Publicar los mensajes de Teams

Abre `Capitulo03/Mensajes_Teams_Capitulo03.txt`, copia cada mensaje y publícalo en un canal o chat de grupo al que pertenezcas. El archivo indica a qué laboratorio corresponde cada uno.

> Los pasos de las guías funcionan adjuntando los archivos directamente en el prompt, así que toleran un índice desactualizado. La indexación es lo que permite que Copilot los encuentre **sin** adjuntarlos, que es la parte vistosa de la demostración.

---

## Cómo importar los correos .eml a Outlook

Los correos vienen en formato `.eml`, el estándar de mensaje de correo. Para que Copilot los encuentre tienen que estar **dentro del buzón**, y para eso hay que importarlos.

### Opción A — Arrastrar a Outlook de escritorio (la más rápida)

1. Descarga el archivo `.eml` a una carpeta local de tu equipo.
2. Abre Outlook de escritorio, clásico o nuevo, con tu cuenta corporativa.
3. Abre el Explorador de archivos junto a Outlook.
4. Arrastra el `.eml` desde el Explorador hasta la carpeta **Bandeja de entrada** en el panel izquierdo de Outlook.
5. El mensaje aparece en la bandeja y se sincroniza con Exchange en unos segundos.

### Opción B — Reenviarlo desde el mensaje abierto

Útil cuando trabajas en Outlook desde el navegador.

1. Haz doble clic en el archivo `.eml`. Se abre como un mensaje.
2. Pulsa **Reenviar**, ponte a ti mismo como destinatario y envíalo.
3. El mensaje llega a tu bandeja con el contenido completo.

### Opción C — Adjuntar el documento de respaldo

Cuando la importación no sea viable, `Capitulo03/Correos_Capitulo03.docx` contiene el texto de los dos correos. Adjúntalo a Microsoft 365 Copilot con el símbolo **+** → **Agregar contenido de trabajo** y las guías funcionan igual, con el contenido del correo llegando como documento.

### Ajustar la fecha del mensaje

Los `.eml` traen la fecha del escenario ficticio (enero y marzo de 2025), así que el mensaje se ordena entre los correos antiguos de la bandeja. Los prompts de las guías buscan por el código del incidente y por el nombre del proyecto, de modo que la fecha resulta indiferente para el ejercicio. Si prefieres que aparezcan entre los mensajes recientes, abre el `.eml` con el Bloc de notas antes de importarlo y cambia la línea que empieza por `Date:` a la fecha del seminario.

---

## Inventario

### Capítulo 1 — Ingeniería de prompts

| Archivo | Se usa en | Contenido |
|---|---|---|
| `Informe_Trimestral_GBS_Q3.docx` | Guía 1.4 | Informe trimestral de la dirección de servicios compartidos: cifras de volumen y costo, cuatro logros, cuatro desafíos y perspectivas del siguiente trimestre |
| `POL-COM-014_Comunicaciones_Externas.docx` | Guía 1.5 | Política interna de comunicaciones externas con seis reglas numeradas |
| `Borrador_Correo_Cliente.docx` | Guía 1.5 | Borrador de correo a un cliente que incumple las seis reglas de la política |
| `Informe_Operativo_Mensual_Marzo.docx` | Guía 1.6 | Informe operativo con seis secciones que contienen riesgos latentes: capacidad, plantilla, seguridad, proveedor único, licencias por vencer y hallazgos de auditoría |

### Capítulo 2 — Agent Builder

| Archivo | Se usa en | Contenido |
|---|---|---|
| `Contrato_CS-2024-0891_Servicios_TI.docx` | Guía 2.1 | Contrato con niveles de servicio, penalizaciones, capacidad contratada y obligaciones de reporte |
| `Reporte_Proveedor_CS-2024-0891_Marzo.docx` | Guía 2.1 | Reporte del proveedor con **nueve incumplimientos** frente al contrato |
| `Reporte_Proveedor_CS-2024-0891_Abril.docx` | Guía 2.1 | Segundo periodo, con **un solo incumplimiento**. Sirve para probar que el agente se reutiliza sin reconfigurarlo |

### Capítulo 3 — Múltiples fuentes

| Archivo | Destino | Se usa en | Contenido |
|---|---|---|---|
| `Reporte_Tecnico_INC-2025-042.docx` | OneDrive | Guía 3.2 | Reporte preliminar de un incidente de seguridad, con línea de tiempo y acciones |
| `Correo_INC-2025-042.eml` | Outlook | Guía 3.2 | Correo de escalamiento del incidente, con tres peticiones al equipo |
| `Correo_Proyecto_Nexo.eml` | Outlook | Guía 3.4 | Correo sobre la reunión reprogramada con el proveedor de integración |
| `Correos_Capitulo03.docx` | Respaldo | Guías 3.2 y 3.4 | Texto de los dos correos anteriores, para adjuntar a Copilot cuando la importación no sea viable |
| `Mensajes_Teams_Capitulo03.txt` | Teams | Guías 3.2 y 3.4 | Los dos mensajes para publicar, cada uno rotulado con su laboratorio |
| `Resultados_Financieros_Q4.docx` | OneDrive | Guía 3.3 | Cifras del trimestre y tres riesgos financieros |
| `Avance_Programa_Digitalizacion.docx` | OneDrive | Guía 3.3 | Estado de un programa por fases, bloqueantes y presupuesto ejecutado |
| `Indicadores_Talento_Enero.docx` | OneDrive | Guía 3.3 | Plantilla, rotación, clima y dos brechas de cumplimiento |
| `KPIs_Semanales_Mesa_Servicio.xlsx` | OneDrive | Guía 3.4 | Cinco métricas por día de la semana, en una sola hoja |
| `Resumen_Proyecto_Nexo.docx` | OneDrive | Guía 3.4 | Ficha de proyecto: fase, hitos, equipo y riesgos |
| `Avance_Semanal_Nexo.pptx` | OneDrive | Guía 3.4 | Tres diapositivas: portada, logros y bloqueos |

---
