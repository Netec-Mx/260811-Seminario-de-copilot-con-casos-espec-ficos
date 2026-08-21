# Transformación operativa con Microsoft 365 Copilot

Seminario ejecutivo orientado a demostrar cómo Microsoft 365 Copilot y Agent Builder pueden fortalecer la productividad, el análisis de información y la toma de decisiones dentro de entornos corporativos. A través de demostraciones en vivo, se presentarán escenarios relacionados con ingeniería de prompts, creación de agentes personalizados, análisis y comparación de documentos, así como consolidación de información desde distintas herramientas del ecosistema Microsoft 365.

**Duración total:** 2 horas. **Audiencia:** líderes y analistas de GBS, operaciones bancarias, compliance y auditoría, finanzas y control interno, riesgos operativos, calidad y áreas de soporte corporativo.

## Estructura

| Ruta | Contenido |
|---|---|
| `CapituloXX/README.md` | Guías de laboratorio del capítulo |
| [`Datos/`](Datos/) | Archivos ficticios que alimentan las guías, listos para subir a OneDrive e importar a Outlook |
| [`DIAGNOSTICO-VERSION-INICIAL.md`](DIAGNOSTICO-VERSION-INICIAL.md) | Revisión de la versión anterior de las guías y criterios aplicados en la reescritura |

## Lista de laboratorios

### Capítulo 1 — Ingeniería de prompts (50 min)

- [1.4 Creación y uso de prompt para análisis estructurado de documentos](Capitulo01/README.md#14-demostración-creación-y-uso-de-prompt-para-análisis-estructurado-de-documentos)
  - Descripción: Compara un prompt vago contra uno con los cinco componentes, sobre el mismo informe trimestral.
  - Duración: 10 min
- [1.5 Creación y uso de prompt para comparación de documentos contra políticas internas](Capitulo01/README.md#15-demostración-creación-y-uso-de-prompt-para-comparación-de-documentos-contra-políticas-internas)
  - Descripción: Audita un borrador de correo contra una política interna de seis reglas y genera la versión corregida.
  - Duración: 10 min
- [1.6 Creación y uso de prompt para identificación de riesgos operativos a partir del análisis del contenido](Capitulo01/README.md#16-demostración-creación-y-uso-de-prompt-para-identificación-de-riesgos-operativos-a-partir-del-análisis-del-contenido)
  - Descripción: Convierte un informe operativo en una matriz de riesgos priorizada y la reordena según el criterio del comité.
  - Duración: 10 min

### Capítulo 2 — Creación de agentes con Agent Builder (30 min)

- [2.1 Flujo completo para la creación y uso de agente para comparación de información en contratos y reportes](Capitulo02/README.md#21-demostración-flujo-completo-para-la-creación-y-uso-de-agente-para-comparación-de-información-en-contratos-y-reportes)
  - Descripción: Construye un agente auditor de contratos con Agent Builder, lo prueba, lo comparte y lo reutiliza con un segundo periodo.
  - Duración: 30 min

### Capítulo 3 — Análisis de información desde múltiples fuentes (40 min)

- [3.2 Análisis de correos (Outlook), chats (Teams) y documentos (Word o PDF) relacionados con un incidente](Capitulo03/README.md#32-demostración-análisis-de-correos-outlook-chats-teams-y-documentos-word-o-pdf-relacionados-con-un-incidente)
  - Descripción: Reconstruye la línea de tiempo de un incidente cruzando correo, Teams y OneDrive, y detecta los compromisos abiertos.
  - Duración: 10 min
- [3.3 Generación de insights ejecutivos a partir de archivos en OneDrive y SharePoint](Capitulo03/README.md#33-demostración-generación-de-insights-ejecutivos-a-partir-de-archivos-en-onedrive-y-sharepoint)
  - Descripción: Consolida tres informes de áreas distintas y obtiene las conexiones que aparecen al leerlos juntos.
  - Duración: 10 min
- [3.4 Generación de reportes operativos integrando información de Teams, Outlook, Excel, Word y PowerPoint](Capitulo03/README.md#34-demostración-generación-de-reportes-operativos-integrando-información-de-teams-outlook-excel-word-y-powerpoint-almacenada-en-onedrive-y-sharepoint)
  - Descripción: Produce un reporte operativo semanal usando Copilot en el chat, en Excel y en Word.
  - Duración: 15 min

## Escenario de las guías

Todas las guías transcurren en **Banco Aurora**, una institución financiera ficticia. Sus empleados, clientes, proveedores y cifras son inventados. El escenario está orientado al sector bancario para que la audiencia reconozca sus propios procesos: conciliación, atención a sucursales, auditoría de proveedores, cumplimiento normativo y riesgo operativo.

## Flujo de colaboración

- Trabajar en `changes_course`.
- Crear Pull Request hacia `main`.
- Merge por `Squash and merge`.
