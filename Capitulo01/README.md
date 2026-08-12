# 4 Demostración: Creación y uso de prompt para análisis estructurado de documentos

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |

## Descripción General

En este laboratorio aplicarás la estructura de cinco componentes (Rol, Contexto, Tarea, Formato y Restricciones) para construir un prompt profesional que realice un análisis estructurado de un documento de texto. Partirás de un documento de ejemplo simulado, diseñarás el prompt paso a paso y verificarás que la respuesta generada por la IA cumpla con los criterios de calidad esperados según las mejores prácticas de ingeniería de prompts.

## Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Construir un prompt completo que integre los cinco componentes estructurales (Rol, Contexto, Tarea, Formato, Restricciones) para el análisis de un documento.
- [ ] Aplicar verbos de acción específicos y verificables al formular la tarea dentro del prompt.
- [ ] Evaluar la calidad de la respuesta generada comparándola contra los criterios definidos en el formato y las restricciones.
- [ ] Iterar sobre un prompt inicial para mejorar la precisión y relevancia de la salida obtenida.

## Prerrequisitos

### Conocimientos previos

| Requisito | Descripción |
|-----------|-------------|
| Estructura de prompts | Comprensión de los cinco componentes: Rol, Contexto, Tarea, Formato y Restricciones (Lección 1.1) |
| Verbos de acción | Familiaridad con verbos específicos como *resume*, *clasifica*, *extrae*, *compara* |
| Manejo responsable de información | Conciencia de no incluir datos sensibles reales en prompts (Lección 1.3) |

### Acceso requerido

| Recurso | Detalle |
|---------|---------|
| Herramienta de IA generativa | Microsoft Copilot (copilot.microsoft.com), ChatGPT u otra herramienta de IA conversacional accesible |
| Navegador web | Cualquier navegador moderno actualizado |
| Editor de texto | Bloc de notas, VS Code o cualquier editor para redactar los prompts antes de enviarlos |

## Entorno del Laboratorio

Este laboratorio no requiere configuración de hardware especializado ni instalación de software adicional. Solo necesitas:

- Acceso a internet con un navegador web.
- Una cuenta activa en una herramienta de IA generativa (Microsoft Copilot, Bing Chat, ChatGPT o equivalente).
- Un editor de texto para componer tus prompts antes de enviarlos.

### Preparación inicial

1. Abre tu navegador y accede a tu herramienta de IA generativa preferida.
2. Inicia una nueva conversación (sesión limpia sin historial previo).
3. Ten abierto un editor de texto en paralelo para redactar y refinar tus prompts.

## Paso a Paso

### Paso 1: Preparar el documento de ejemplo para el análisis

**Objetivo:** Disponer de un documento de texto simulado que servirá como insumo para el análisis estructurado.

**Instrucciones:**

1. Copia el siguiente documento de ejemplo en tu editor de texto. Este simula un informe ejecutivo breve de una empresa ficticia:

```text
INFORME TRIMESTRAL - TechNova Solutions S.A.
Periodo: Q3 2024 (julio - septiembre)

RESUMEN EJECUTIVO:
TechNova Solutions registró un crecimiento del 12% en ingresos respecto al
trimestre anterior, alcanzando $4.8M USD. La línea de productos SaaS representó
el 68% de los ingresos totales, mientras que los servicios de consultoría
contribuyeron con el 32% restante.

LOGROS PRINCIPALES:
- Lanzamiento exitoso de la plataforma CloudSync 3.0 con 2,400 nuevos usuarios
  en el primer mes.
- Reducción del 18% en el tiempo de resolución de tickets de soporte gracias a
  la implementación de un chatbot interno basado en IA.
- Firma de 3 contratos enterprise con empresas del sector financiero por un
  valor combinado de $1.2M USD.
- Certificación ISO 27001 obtenida en agosto 2024.

DESAFÍOS IDENTIFICADOS:
- Rotación de personal técnico del 15%, superior al promedio de la industria (10%).
- Retrasos de 3 semanas en el proyecto de migración a infraestructura multi-cloud.
- Incremento del 22% en costos de adquisición de clientes (CAC) en el canal digital.

PERSPECTIVAS Q4 2024:
- Lanzamiento planificado de módulo de analytics avanzado para CloudSync.
- Meta de reducción del CAC en un 10% mediante optimización de campañas.
- Objetivo de contratación de 15 ingenieros senior para cubrir vacantes críticas.
- Expansión al mercado latinoamericano con oficina en Ciudad de México.
```

2. Revisa el documento y familiarízate con su contenido: secciones, datos numéricos, logros y desafíos.

3. Identifica mentalmente qué tipo de análisis sería valioso: extracción de métricas clave, identificación de riesgos, recomendaciones, etc.

**Resultado esperado:** Tienes el documento completo copiado y disponible para ser incluido en tu prompt.

**Verificación:** Confirma que el texto del documento está íntegro y no tiene errores de copia.

---

### Paso 2: Construir el prompt estructurado con los cinco componentes

**Objetivo:** Redactar un prompt completo que aplique los cinco componentes (Rol, Contexto, Tarea, Formato, Restricciones) para solicitar un análisis estructurado del documento.

**Instrucciones:**

1. En tu editor de texto, comienza redactando cada componente por separado. Usa las siguientes guías:

   **Componente 1 — Rol:**
   ```text
   Actúa como un consultor de estrategia empresarial con experiencia en
   empresas de tecnología SaaS.
   ```

   **Componente 2 — Contexto:**
   ```text
   Soy el director de operaciones de TechNova Solutions. Necesito presentar
   un análisis del informe trimestral Q3 2024 ante la junta directiva para
   tomar decisiones sobre la asignación de presupuesto del próximo trimestre.
   ```

   **Componente 3 — Tarea:**
   ```text
   Analiza el siguiente informe trimestral y realiza lo siguiente:
   1. Extrae las 5 métricas cuantitativas más relevantes.
   2. Clasifica los hallazgos en: Fortalezas, Riesgos y Oportunidades.
   3. Genera 3 recomendaciones accionables priorizadas por impacto.
   ```

   **Componente 4 — Formato:**
   ```text
   Presenta el resultado en tres secciones con encabezados claros:
   - "Métricas Clave" en formato de tabla (Métrica | Valor | Interpretación).
   - "Clasificación Estratégica" en listas con viñetas agrupadas por categoría.
   - "Recomendaciones" numeradas del 1 al 3, cada una con máximo 2 oraciones.
   ```

   **Componente 5 — Restricciones:**
   ```text
   - Usa lenguaje ejecutivo, directo y sin tecnicismos de TI.
   - No inventes datos que no estén en el documento.
   - Extensión total máxima: 400 palabras.
   - Idioma: español.
   ```

2. Ahora integra todos los componentes en un único prompt fluido. Redáctalo de la siguiente forma:

```text
Actúa como un consultor de estrategia empresarial con experiencia en empresas
de tecnología SaaS.

Contexto: Soy el director de operaciones de TechNova Solutions. Necesito
presentar un análisis del informe trimestral Q3 2024 ante la junta directiva
para tomar decisiones sobre la asignación de presupuesto del próximo trimestre.

Tarea: Analiza el siguiente informe trimestral y realiza lo siguiente:
1. Extrae las 5 métricas cuantitativas más relevantes.
2. Clasifica los hallazgos en: Fortalezas, Riesgos y Oportunidades.
3. Genera 3 recomendaciones accionables priorizadas por impacto.

Formato de salida:
- "Métricas Clave" en formato de tabla con columnas: Métrica | Valor | Interpretación.
- "Clasificación Estratégica" en listas con viñetas agrupadas por categoría
  (Fortalezas, Riesgos, Oportunidades).
- "Recomendaciones" numeradas del 1 al 3, cada una con máximo 2 oraciones.

Restricciones:
- Lenguaje ejecutivo, directo y sin tecnicismos de TI.
- No inventes datos que no estén en el documento proporcionado.
- Extensión total máxima: 400 palabras.
- Idioma: español.

DOCUMENTO A ANALIZAR:
---
[Pega aquí el informe trimestral del Paso 1]
---
```

3. Sustituye `[Pega aquí el informe trimestral del Paso 1]` con el texto completo del documento de ejemplo.

4. Revisa el prompt completo verificando que cada componente esté presente y sea coherente.

**Resultado esperado:** Un prompt completo de aproximadamente 350-450 palabras que integra los cinco componentes estructurales y el documento a analizar.

**Verificación:** Usa la siguiente lista de chequeo:

| Componente | ¿Presente? | ¿Específico? |
|------------|:----------:|:-------------:|
| Rol | ✓ | Consultor de estrategia / SaaS |
| Contexto | ✓ | Director de ops, junta directiva, presupuesto |
| Tarea | ✓ | 3 subtareas con verbos: extrae, clasifica, genera |
| Formato | ✓ | Tabla + listas + numeración definidas |
| Restricciones | ✓ | Tono, veracidad, extensión, idioma |

---

### Paso 3: Enviar el prompt y evaluar la respuesta

**Objetivo:** Ejecutar el prompt en la herramienta de IA y evaluar si la respuesta cumple con los criterios definidos.

**Instrucciones:**

1. Copia el prompt completo (incluyendo el documento) desde tu editor de texto.

2. Pégalo en la interfaz de tu herramienta de IA generativa y envíalo.

3. Espera la respuesta completa del modelo.

4. Evalúa la respuesta utilizando la siguiente rúbrica de calidad:

| Criterio | Cumple (Sí/No) | Observación |
|----------|:---------------:|-------------|
| ¿Incluye una tabla de métricas con las 3 columnas solicitadas? | | |
| ¿Las métricas extraídas son datos reales del documento (no inventados)? | | |
| ¿La clasificación estratégica tiene las 3 categorías (Fortalezas, Riesgos, Oportunidades)? | | |
| ¿Las recomendaciones son exactamente 3 y están numeradas? | | |
| ¿Cada recomendación tiene máximo 2 oraciones? | | |
| ¿El lenguaje es ejecutivo y libre de tecnicismos de TI? | | |
| ¿La extensión total es ≤ 400 palabras aproximadamente? | | |
| ¿Está en español? | | |

5. Registra tus observaciones en el editor de texto para uso en el siguiente paso.

**Resultado esperado:** La respuesta debe contener las tres secciones solicitadas (Métricas Clave, Clasificación Estratégica, Recomendaciones) con el formato especificado. Ejemplo parcial esperado:

```text
## Métricas Clave

| Métrica | Valor | Interpretación |
|---------|-------|----------------|
| Crecimiento de ingresos | 12% trimestral | Tendencia positiva sostenida |
| Ingresos totales Q3 | $4.8M USD | Superación del umbral de $4M |
| Nuevos usuarios CloudSync 3.0 | 2,400 | Adopción fuerte en primer mes |
| Rotación de personal técnico | 15% | 5 puntos sobre promedio industria |
| Incremento CAC digital | 22% | Presión sobre rentabilidad |

## Clasificación Estratégica

**Fortalezas:**
- Crecimiento sostenido de ingresos (12%)
- Lanzamiento exitoso de CloudSync 3.0
- Certificación ISO 27001 lograda
...
```

**Verificación:** Al menos 6 de los 8 criterios de la rúbrica deben cumplirse para considerar el prompt exitoso. Si se cumplen menos de 6, procede al Paso 4 para iterar.

---

### Paso 4: Iterar y refinar el prompt

**Objetivo:** Mejorar el prompt original corrigiendo las deficiencias detectadas en la evaluación para obtener una respuesta más precisa.

**Instrucciones:**

1. Identifica qué criterios de la rúbrica NO se cumplieron. Los problemas más comunes son:
   - La IA inventó datos no presentes en el documento.
   - El formato no coincide exactamente con lo solicitado.
   - La extensión excede el límite.
   - Las recomendaciones son vagas en lugar de accionables.

2. Para cada deficiencia, aplica una de estas técnicas de refinamiento:

   | Problema detectado | Técnica de refinamiento |
   |-------------------|------------------------|
   | Datos inventados | Añadir restricción: "Cita entre paréntesis la sección del documento de donde extraes cada dato" |
   | Formato incorrecto | Ser más explícito: "Usa exactamente el formato Markdown de tabla con pipes (|)" |
   | Extensión excesiva | Reducir el número de elementos: "Extrae solo las 3 métricas más críticas" |
   | Recomendaciones vagas | Añadir criterio: "Cada recomendación debe incluir una acción concreta, un responsable sugerido y un plazo" |

3. Redacta la versión refinada del prompt. Por ejemplo, si las recomendaciones fueron vagas, modifica ese segmento:

```text
3. Genera 3 recomendaciones accionables priorizadas por impacto potencial
   en ingresos. Cada recomendación debe seguir el formato:
   "[Acción concreta] → [Resultado esperado] → [Plazo sugerido]"
```

4. Envía el prompt refinado a la herramienta de IA.

5. Evalúa nuevamente con la misma rúbrica del Paso 3.

**Resultado esperado:** La segunda iteración debe cumplir 7 u 8 de los 8 criterios de la rúbrica. La respuesta debe ser notablemente más precisa y ajustada al formato solicitado.

**Verificación:** Compara las dos respuestas (original y refinada) lado a lado. Documenta las mejoras observadas con al menos 2 diferencias concretas.

---

### Paso 5: Documentar el aprendizaje y las mejores prácticas aplicadas

**Objetivo:** Consolidar el aprendizaje registrando la estructura del prompt efectivo y las lecciones aprendidas durante la iteración.

**Instrucciones:**

1. En tu editor de texto, crea una sección de "Lecciones Aprendidas" con el siguiente formato:

```text
## Lecciones Aprendidas - Lab Análisis Estructurado

### Prompt Final (versión refinada):
[Pega aquí tu prompt final después de la iteración]

### Componentes utilizados:
- Rol: [Describe brevemente]
- Contexto: [Describe brevemente]
- Tarea: [Describe brevemente]
- Formato: [Describe brevemente]
- Restricciones: [Describe brevemente]

### Qué funcionó bien:
1. [Observación 1]
2. [Observación 2]

### Qué requirió iteración:
1. [Problema → Solución aplicada]
2. [Problema → Solución aplicada]

### Principio clave para recordar:
[Una oración que capture tu aprendizaje principal]
```

2. Completa cada sección basándote en tu experiencia durante el laboratorio.

3. Guarda el archivo con el nombre `lab-prompt-analisis-estructurado.md`.

**Resultado esperado:** Un documento de referencia personal que podrás reutilizar como plantilla para futuros análisis estructurados de documentos.

**Verificación:** Tu documento debe contener el prompt final completo, al menos 2 lecciones en "Qué funcionó bien" y al menos 1 en "Qué requirió iteración".

---

## Validación y Pruebas

Para confirmar que has completado exitosamente el laboratorio, verifica los siguientes criterios de éxito:

| # | Criterio de validación | Estado |
|---|------------------------|:------:|
| 1 | El prompt final contiene los 5 componentes estructurales identificables | ☐ |
| 2 | La tarea utiliza al menos 3 verbos de acción específicos (extrae, clasifica, genera u otros) | ☐ |
| 3 | La respuesta de la IA incluye las 3 secciones solicitadas en el formato correcto | ☐ |
| 4 | Ningún dato en la respuesta es inventado (todos provienen del documento fuente) | ☐ |
| 5 | Se realizó al menos 1 iteración de refinamiento con mejora documentada | ☐ |
| 6 | El documento de lecciones aprendidas está completo y guardado | ☐ |

**Criterio de aprobación:** Mínimo 5 de 6 criterios cumplidos.

---

## Solución de Problemas

### Problema 1: La IA genera información que no está en el documento

**Síntomas:** La respuesta incluye métricas, porcentajes o hechos que no aparecen en el informe trimestral proporcionado. Por ejemplo, menciona "un crecimiento del 25% en satisfacción del cliente" cuando ese dato no existe en el documento.

**Causa:** El modelo está complementando con conocimiento general o "alucinando" datos para completar la estructura solicitada cuando no encuentra suficientes datos en el documento para llenar todos los campos requeridos.

**Solución:** Añade una restricción explícita más fuerte al prompt:

```text
RESTRICCIÓN CRÍTICA: Utiliza ÚNICAMENTE datos explícitamente mencionados en el
documento proporcionado. Si no hay suficientes datos para completar una sección,
indica "No disponible en el documento" en lugar de generar información. Cita la
sección del documento de donde extraes cada dato.
```

Adicionalmente, reduce la cantidad de métricas solicitadas (de 5 a 3) si el documento no contiene suficientes datos cuantitativos distintos.

---

### Problema 2: El formato de la respuesta no coincide con lo solicitado

**Síntomas:** Solicitaste una tabla con columnas específicas pero la IA responde con listas con viñetas. O pediste 3 recomendaciones y entrega 5. O las secciones no tienen los encabezados exactos que definiste.

**Causa:** Las instrucciones de formato están mezcladas con otras partes del prompt y el modelo no las prioriza adecuadamente, o la herramienta de IA tiene limitaciones en la renderización de ciertos formatos.

**Solución:** Separa las instrucciones de formato al final del prompt (justo antes del documento) y hazlas más explícitas con un ejemplo:

```text
FORMATO OBLIGATORIO (respeta exactamente esta estructura):

Sección 1 - Título: "Métricas Clave"
Formato: tabla Markdown con exactamente 3 columnas separadas por |
Ejemplo de fila: | Ingresos Q3 | $4.8M USD | Crecimiento positivo |

Sección 2 - Título: "Clasificación Estratégica"
Formato: 3 sublistas con encabezados en negrita: **Fortalezas**, **Riesgos**, **Oportunidades**

Sección 3 - Título: "Recomendaciones"
Formato: exactamente 3 items numerados (1. 2. 3.), máximo 2 oraciones cada uno.

NO agregues secciones adicionales. NO cambies los nombres de las secciones.
```

Proporcionar un ejemplo concreto de la primera fila de la tabla ayuda significativamente al modelo a replicar el formato deseado.

---

## Limpieza

1. Si utilizaste una sesión de chat en Microsoft Copilot o herramienta equivalente, puedes cerrar la conversación o eliminarla del historial si contiene información que prefieres no conservar.
2. Conserva tu archivo `lab-prompt-analisis-estructurado.md` como referencia para futuros ejercicios.
3. No es necesario eliminar ningún recurso adicional ya que este laboratorio no involucra infraestructura ni instalaciones.

---

## Resumen

En este laboratorio has aplicado de forma práctica la estructura de cinco componentes para crear un prompt de análisis estructurado de documentos. Los puntos clave que debes retener son:

- **La estructura importa:** Un prompt con Rol + Contexto + Tarea + Formato + Restricciones produce resultados significativamente superiores a una solicitud vaga.
- **Los verbos de acción dirigen al modelo:** Usar *extrae*, *clasifica* y *genera* produce respuestas más accionables que *ayúdame con* o *dime sobre*.
- **El formato explícito ahorra tiempo:** Definir tablas, listas y límites de extensión evita reformateo posterior.
- **La iteración es parte del proceso:** Raramente el primer prompt es perfecto; refinar basándose en la evaluación de la respuesta es una práctica profesional estándar.
- **Las restricciones protegen la calidad:** Limitar la invención de datos y definir el tono garantiza respuestas confiables y apropiadas para el contexto empresarial.

### Recursos adicionales

- [Microsoft Copilot Lab — Ejemplos de prompts por categoría](https://copilot.cloud.microsoft/prompts)
- [Guía de adopción de Copilot para Microsoft 365](https://adoption.microsoft.com/es-es/copilot/)
- [Prompt Engineering Guide — DAIR.AI](https://www.promptingguide.ai/es)

---

# 5 Demostración: Creación y uso de prompt para comparación de documentos contra políticas internas

## Metadata

| Campo | Detalle |
|-------|---------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |

## Descripción General

En este laboratorio aplicarás la estructura de prompts efectivos (Rol, Contexto, Tarea, Formato y Restricciones) para construir un prompt que compare un documento de trabajo contra una política interna de la organización. Simularás un escenario real en el que un profesional necesita verificar si un contrato, procedimiento o comunicación cumple con los lineamientos corporativos, utilizando Microsoft Copilot como herramienta de análisis.

## Objetivos de Aprendizaje

- [ ] Construir un prompt estructurado con los cinco componentes (Rol, Contexto, Tarea, Formato, Restricciones) orientado a la comparación de documentos.
- [ ] Aplicar verbos de acción específicos para guiar a la IA en tareas de análisis comparativo.
- [ ] Evaluar la respuesta generada por la IA e iterar sobre el prompt para mejorar la precisión del resultado.
- [ ] Incorporar restricciones de seguridad y manejo responsable de información en la formulación del prompt.

## Prerrequisitos

### Conocimientos previos

| Concepto | Nivel requerido |
|----------|----------------|
| Estructura de prompts efectivos (Lección 1.1) | Comprendido |
| Ejemplo de prompt efectivo (Lección 1.2) | Comprendido |
| Seguridad y manejo responsable de información (Lección 1.3) | Comprendido |

### Acceso necesario

| Recurso | Detalle |
|---------|---------|
| Microsoft Copilot | Acceso a Microsoft 365 Copilot (chat en navegador, Teams o Word) o Copilot gratuito en copilot.microsoft.com |
| Navegador web | Microsoft Edge o Google Chrome actualizado |
| Documentos de ejemplo | Se proporcionan en este laboratorio (texto simulado) |

## Entorno del Laboratorio

No se requiere hardware o software especializado. El laboratorio se ejecuta completamente en un navegador web con acceso a Microsoft Copilot. Los documentos de ejemplo se proporcionan como texto dentro de las instrucciones para que puedas copiarlos y pegarlos directamente en la herramienta.

### Preparación inicial

1. Abre tu navegador y navega a [copilot.microsoft.com](https://copilot.microsoft.com) o abre Microsoft Copilot dentro de Microsoft 365.
2. Inicia sesión con tu cuenta corporativa o personal.
3. Verifica que puedes escribir en el campo de chat y recibir respuestas.

---

## Paso a Paso

### Paso 1: Preparar los documentos de referencia

**Objetivo:** Disponer de un extracto de política interna y un documento de trabajo que serán comparados mediante el prompt.

**Instrucciones:**

1. Copia el siguiente texto que simula una **política interna de comunicaciones corporativas**:

```text
POLÍTICA INTERNA: COMUNICACIONES EXTERNAS (POL-COM-2024)

1. Toda comunicación externa dirigida a clientes debe incluir el nombre
   completo del remitente, cargo y datos de contacto institucional.
2. Está prohibido incluir información financiera no pública (proyecciones,
   márgenes, costos internos) en comunicaciones a clientes.
3. El tono debe ser formal y profesional. No se permiten expresiones
   coloquiales ni emojis.
4. Toda promesa de plazos de entrega debe ser aprobada previamente por
   el gerente de área.
5. Se debe incluir la cláusula de confidencialidad estándar al pie del
   mensaje cuando se traten temas de proyectos en desarrollo.
6. Los correos deben enviarse únicamente desde cuentas corporativas
   (@empresa.com).
```

2. Copia el siguiente texto que simula un **correo electrónico borrador** que un empleado desea enviar a un cliente:

```text
BORRADOR DE CORREO:

De: carlos.mendez@gmail.com
Para: ana.torres@clienteXYZ.com
Asunto: Actualización del proyecto Alpha

Hola Ana! 😊

Te escribo para contarte que el proyecto Alpha va super bien. Estimamos
que los márgenes de ganancia serán del 35% este trimestre, así que
estamos muy contentos.

Te prometo que tendremos el entregable listo para el 15 de marzo sin
falta. Si necesitas algo más, me avisas.

Saludos,
Carlos
```

3. Guarda ambos textos en un archivo de notas local o tenlos listos para pegar en el siguiente paso.

**Resultado esperado:** Dispones de dos bloques de texto claramente diferenciados: la política interna y el borrador de correo.

**Verificación:** Confirma que puedes distinguir ambos documentos y que el texto está completo sin cortes.

---

### Paso 2: Construir el prompt estructurado con los cinco componentes

**Objetivo:** Redactar un prompt que aplique los cinco componentes (Rol, Contexto, Tarea, Formato, Restricciones) para solicitar a Copilot una comparación del borrador contra la política interna.

**Instrucciones:**

1. En un editor de texto (Bloc de notas, Word o directamente en el campo de chat), redacta tu prompt siguiendo esta estructura:

```text
Actúa como un auditor de cumplimiento normativo interno de una empresa
corporativa.

Contexto: Soy un coordinador de proyectos que necesita verificar si un
borrador de correo electrónico destinado a un cliente externo cumple con
nuestra política interna de comunicaciones externas (POL-COM-2024). A
continuación te proporcionaré ambos documentos.

Tarea: Compara el borrador de correo contra cada punto de la política
interna. Identifica todas las violaciones o incumplimientos, indicando
qué regla específica se incumple y qué fragmento del borrador lo evidencia.
Además, proporciona una recomendación concreta para corregir cada violación.

Formato: Presenta los resultados en una tabla con las siguientes columnas:
- Número de regla incumplida
- Texto de la regla
- Fragmento del borrador que viola la regla
- Recomendación de corrección

Al final de la tabla, incluye un veredicto general: "CUMPLE" o "NO CUMPLE".

Restricciones:
- No inventes reglas que no estén en la política proporcionada.
- No incluyas información confidencial adicional en tu respuesta.
- Usa lenguaje formal y profesional.
- Si alguna regla no puede evaluarse con la información disponible,
  indícalo como "No evaluable" en lugar de asumir cumplimiento.

--- POLÍTICA INTERNA ---
[Pegar aquí el texto de la política]

--- BORRADOR DE CORREO ---
[Pegar aquí el texto del borrador]
```

2. Reemplaza los marcadores `[Pegar aquí el texto de la política]` y `[Pegar aquí el texto del borrador]` con los textos del Paso 1.

3. Revisa que tu prompt contenga explícitamente los cinco componentes:
   - ✅ **Rol:** "Actúa como un auditor de cumplimiento normativo interno"
   - ✅ **Contexto:** Situación del coordinador de proyectos
   - ✅ **Tarea:** Comparar, identificar violaciones, recomendar correcciones
   - ✅ **Formato:** Tabla con columnas específicas + veredicto
   - ✅ **Restricciones:** No inventar reglas, lenguaje formal, manejo de incertidumbre

**Resultado esperado:** Un prompt completo de aproximadamente 250-350 palabras que integra los cinco componentes estructurales y ambos documentos.

**Verificación:** Lee el prompt en voz alta. ¿Queda claro qué debe hacer la IA, cómo y con qué limitaciones? Si la respuesta es sí, estás listo para el siguiente paso.

---

### Paso 3: Ejecutar el prompt en Microsoft Copilot

**Objetivo:** Enviar el prompt a Copilot y obtener el análisis comparativo.

**Instrucciones:**

1. Abre Microsoft Copilot en tu navegador o aplicación.
2. Pega el prompt completo (con los documentos incluidos) en el campo de entrada.
3. Presiona **Enter** o haz clic en el botón de enviar.
4. Espera a que Copilot genere la respuesta completa.

**Resultado esperado:** Copilot debe generar una tabla que identifique al menos las siguientes violaciones:

| # Regla | Regla | Fragmento violatorio | Recomendación |
|---------|-------|---------------------|---------------|
| 1 | Incluir nombre completo, cargo y datos de contacto institucional | Solo firma "Carlos" sin apellido, cargo ni datos de contacto | Agregar nombre completo, cargo y teléfono/correo institucional |
| 2 | Prohibido incluir información financiera no pública | "márgenes de ganancia serán del 35%" | Eliminar referencia a márgenes financieros |
| 3 | Tono formal, sin expresiones coloquiales ni emojis | "Hola Ana! 😊", "super bien", "me avisas" | Reformular con saludo formal y eliminar emoji |
| 4 | Promesas de plazos deben ser aprobadas por gerente | "Te prometo que tendremos el entregable listo para el 15 de marzo" | Indicar que el plazo está sujeto a confirmación o evidenciar aprobación |
| 5 | Cláusula de confidencialidad cuando se traten proyectos en desarrollo | No se incluye cláusula al pie del mensaje | Agregar cláusula de confidencialidad estándar |
| 6 | Envío desde cuentas corporativas (@empresa.com) | "carlos.mendez@gmail.com" | Cambiar remitente a cuenta corporativa |

Veredicto: **NO CUMPLE**

**Verificación:** Compara la respuesta de Copilot con la tabla anterior. ¿Identificó al menos 4 de las 6 violaciones? Si es así, el prompt fue efectivo.

---

### Paso 4: Iterar y refinar el prompt

**Objetivo:** Mejorar el prompt si la respuesta inicial fue incompleta o imprecisa.

**Instrucciones:**

1. Si Copilot omitió alguna violación, envía un mensaje de seguimiento:

```text
Revisa nuevamente la regla número [X] de la política. ¿El borrador
cumple o no cumple con esa regla específica? Justifica tu respuesta
citando el texto relevante del borrador.
```

2. Si el formato de la respuesta no fue una tabla como solicitaste, refuerza la instrucción:

```text
Por favor, reformatea tu análisis anterior como una tabla con exactamente
4 columnas: Número de regla, Texto de la regla, Fragmento violatorio,
Recomendación. No uses párrafos narrativos.
```

3. Si deseas una versión corregida del correo, agrega:

```text
Ahora, basándote en las violaciones identificadas, redacta una versión
corregida del correo que cumpla con todas las reglas de la política
POL-COM-2024. Mantén la intención original del mensaje.
```

**Resultado esperado:** Copilot responde con mayor precisión al recibir instrucciones de refinamiento, demostrando el principio de iteración en la ingeniería de prompts.

**Verificación:** La respuesta refinada debe abordar específicamente lo solicitado sin desviarse del tema ni inventar información no presente en los documentos originales.

---

### Paso 5: Aplicar principios de seguridad y manejo responsable

**Objetivo:** Reflexionar sobre las consideraciones de seguridad al usar IA para comparar documentos contra políticas internas.

**Instrucciones:**

1. Revisa tu prompt y responde mentalmente las siguientes preguntas de seguridad:

   - ¿Los documentos que proporcioné contienen datos personales reales? (En este caso no, son simulados ✅)
   - ¿La política interna que compartí es clasificada o de uso restringido? (Evalúa en un caso real)
   - ¿La respuesta de Copilot podría ser compartida externamente sin riesgo? (Verifica antes de reenviar)

2. Modifica tu prompt original agregando una restricción de seguridad adicional:

```text
Restricción adicional: Esta información es de uso interno exclusivo.
No la utilices para entrenar modelos ni la retengas para futuras
conversaciones. Tu respuesta será tratada como documento interno
confidencial.
```

3. En un escenario real, considera estas mejores prácticas:
   - **Nunca** incluyas datos personales reales de empleados o clientes en prompts de prueba.
   - **Anonimiza** nombres, cifras y datos sensibles antes de pegar documentos reales.
   - **Verifica** las políticas de retención de datos de tu organización antes de usar IA con documentos internos.
   - **Utiliza** la versión empresarial de Copilot (Microsoft 365 Copilot) que ofrece protección de datos comerciales.

**Resultado esperado:** Comprensión práctica de cómo integrar consideraciones de seguridad en el flujo de trabajo con IA.

**Verificación:** Puedes articular al menos 3 prácticas de seguridad que aplicarías antes de usar un prompt de comparación de documentos con información real de tu organización.

---

## Validación y Pruebas

Para confirmar que has completado exitosamente este laboratorio, verifica los siguientes criterios:

| Criterio | Cumplido |
|----------|----------|
| Tu prompt incluye los 5 componentes (Rol, Contexto, Tarea, Formato, Restricciones) | ☐ |
| Copilot identificó al menos 4 de 6 violaciones en el borrador | ☐ |
| La respuesta se presentó en formato tabla (o lo lograste tras iterar) | ☐ |
| Pudiste refinar el prompt para obtener una respuesta más precisa | ☐ |
| Identificaste al menos 3 consideraciones de seguridad aplicables | ☐ |

**Prueba adicional (opcional):** Toma un documento real de tu trabajo (anonimizado) y una política interna pública de tu organización. Adapta el prompt de este laboratorio para realizar una comparación real. Evalúa si la estructura de cinco componentes produce resultados útiles y accionables.

---

## Solución de Problemas

### Problema 1: Copilot no genera una tabla y responde en párrafos narrativos

**Síntomas:** A pesar de solicitar formato tabla, Copilot presenta el análisis como texto corrido con viñetas o párrafos.

**Causa:** Algunos modelos interpretan la instrucción de formato con menor prioridad cuando el contenido es extenso, o la instrucción de formato está "enterrada" entre mucho texto contextual.

**Solución:**
1. Mueve la instrucción de formato al inicio del prompt, inmediatamente después del rol:
```text
Actúa como un auditor de cumplimiento. IMPORTANTE: Toda tu respuesta
debe estar en formato de tabla Markdown con 4 columnas. No uses
párrafos narrativos.
```
2. Alternativamente, envía un mensaje de seguimiento:
```text
Convierte tu respuesta anterior a una tabla con columnas: Regla |
Descripción | Violación encontrada | Corrección sugerida
```

---

### Problema 2: Copilot "inventa" violaciones que no existen en la política proporcionada

**Síntomas:** La respuesta incluye supuestas violaciones a reglas que no están en el texto de la política (por ejemplo, menciona requisitos de firma digital o aprobación legal que nunca se especificaron).

**Causa:** El modelo complementa con conocimiento general sobre políticas corporativas típicas, "alucinando" reglas adicionales cuando la restricción de ceñirse al documento no es suficientemente explícita.

**Solución:**
1. Refuerza la restricción en el prompt:
```text
RESTRICCIÓN CRÍTICA: Evalúa ÚNICAMENTE las 6 reglas numeradas que
aparecen en la política proporcionada. No añadas reglas adicionales
de ninguna fuente. Si una regla no se viola, no la incluyas en la tabla.
```
2. Al recibir la respuesta, solicita justificación:
```text
Para cada violación que identificaste, cita textualmente la regla de
la política que se incumple. Si no puedes citar una regla exacta del
documento proporcionado, elimina esa entrada de tu análisis.
```

---

## Limpieza

1. Si utilizaste documentos sensibles durante la práctica, elimina el historial de conversación en Copilot haciendo clic en el icono de **nueva conversación** o **eliminar chat**.
2. Borra cualquier archivo temporal donde hayas guardado los textos de ejemplo.
3. Si trabajaste en un equipo compartido, cierra la sesión de Microsoft Copilot.

---

## Resumen

En este laboratorio aplicaste la estructura de cinco componentes para construir un prompt profesional de comparación de documentos contra políticas internas. Los aprendizajes clave son:

- **La estructura importa:** Un prompt con Rol, Contexto, Tarea, Formato y Restricciones produce análisis significativamente más útiles que una solicitud vaga.
- **Los verbos de acción guían la IA:** "Compara", "identifica", "recomienda" son más efectivos que "ayúdame con" o "revisa".
- **La iteración es parte del proceso:** Refinar el prompt tras la primera respuesta es una práctica normal y esperada.
- **La seguridad no es opcional:** Antes de usar documentos reales, siempre anonimiza, evalúa la clasificación de la información y utiliza herramientas con protección de datos empresarial.

### Recursos adicionales

- [Mejores prácticas de prompts en Microsoft Copilot](https://support.microsoft.com/es-es/copilot)
- [Guía de adopción de Copilot para empresas](https://adoption.microsoft.com/es-es/copilot/)
- [Principios de IA responsable de Microsoft](https://www.microsoft.com/es-es/ai/responsible-ai)

---

# 6 Demostración: Creación y uso de prompt para identificación de riesgos operativos a partir del análisis del contenido

## Metadata

| Campo | Valor |
|---|---|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |
| **Tecnologías** | Estructura de prompts efectivos, Ingeniería de prompts, Microsoft Copilot (o cualquier LLM disponible) |

## Descripción General

En este laboratorio aplicarás la estructura de cinco componentes de un prompt efectivo (Rol, Contexto, Tarea, Formato y Restricciones) para construir un prompt especializado en la identificación de riesgos operativos a partir del análisis de contenido textual. Trabajarás con un escenario simulado de un informe operativo y verificarás que el prompt genera una respuesta accionable, estructurada y alineada con las necesidades de gestión de riesgos empresariales.

## Objetivos de Aprendizaje

- [ ] Construir un prompt estructurado con los cinco componentes (Rol, Contexto, Tarea, Formato, Restricciones) orientado a la identificación de riesgos operativos.
- [ ] Analizar contenido textual simulado para extraer información relevante sobre riesgos mediante el uso de inteligencia artificial generativa.
- [ ] Evaluar la calidad de la respuesta generada y refinar el prompt de forma iterativa para mejorar la precisión del análisis de riesgos.
- [ ] Aplicar principios de seguridad y manejo responsable de información al formular prompts que procesan contenido operativo sensible.

## Prerrequisitos

### Conocimientos Previos

| Conocimiento | Nivel |
|---|---|
| Componentes de un prompt efectivo (Lección 1.1) | Comprensión básica |
| Ejemplo de prompt efectivo (Lección 1.2) | Familiaridad |
| Seguridad y manejo responsable de información (Lección 1.3) | Conocimiento general |
| Conceptos básicos de gestión de riesgos operativos | Nociones generales |

### Acceso Requerido

| Recurso | Detalle |
|---|---|
| Herramienta de IA generativa | Microsoft Copilot (copilot.microsoft.com), ChatGPT, o cualquier LLM accesible vía navegador |
| Navegador web | Cualquier navegador moderno actualizado |
| Editor de texto | Bloc de notas, VS Code, o cualquier editor para redactar prompts |

## Entorno del Laboratorio

### Software Necesario

| Herramienta | Propósito |
|---|---|
| Microsoft Copilot / ChatGPT / LLM equivalente | Ejecutar los prompts y obtener respuestas |
| Editor de texto plano | Redactar y refinar los prompts antes de enviarlos |

### Preparación Inicial

1. Abre tu navegador y accede a la herramienta de IA generativa disponible (por ejemplo, `https://copilot.microsoft.com`).
2. Inicia sesión con tus credenciales si es necesario.
3. Abre un editor de texto en paralelo para redactar tus prompts antes de enviarlos.
4. Copia el siguiente **texto simulado de informe operativo** en tu editor de texto, ya que lo utilizarás como contenido de entrada para el análisis:

```text
INFORME OPERATIVO MENSUAL - EMPRESA LOGÍSTICA NORTE S.A.
Período: Marzo 2024

1. Operaciones de Almacén:
El sistema de gestión de inventarios (WMS) presentó tres caídas no programadas
durante la segunda semana del mes, con una duración promedio de 4.5 horas cada una.
El equipo de TI identificó que el servidor principal opera al 94% de capacidad
de forma constante. No se ha aprobado presupuesto para la migración a la nube
prevista originalmente para Q1 2024.

2. Transporte y Distribución:
Se registraron 12 entregas fuera del plazo acordado con el cliente corporativo
MEGAMART (SLA de 24 horas). La causa principal fue la escasez de conductores
certificados para rutas interprovinciales. Actualmente operamos con 15 conductores
cuando la plantilla óptima es de 22. El proceso de contratación lleva 8 semanas
detenido por falta de aprobación de recursos humanos.

3. Seguridad Laboral:
Se reportaron 5 incidentes menores en zona de carga (golpes y caídas). La
capacitación en seguridad programada para febrero fue pospuesta indefinidamente.
Los equipos de protección personal (EPP) de 30% del personal están vencidos
desde enero 2024.

4. Proveedores:
El proveedor principal de embalaje (PACKPRO) notificó un incremento del 18% en
precios efectivo desde abril. No se cuenta con proveedor alternativo calificado.
El contrato actual vence en 60 días y no se han iniciado negociaciones de renovación.

5. Tecnología:
Las licencias del software de rastreo GPS de flota vencen el 15 de abril.
El área de compras no ha procesado la orden de renovación. Sin este sistema,
se pierde visibilidad en tiempo real de 45 unidades de transporte.
```

## Procedimiento Paso a Paso

### Paso 1: Diseñar el prompt con los cinco componentes estructurales

**Objetivo:** Construir un prompt completo que integre Rol, Contexto, Tarea, Formato y Restricciones para solicitar a la IA un análisis de riesgos operativos del informe proporcionado.

**Instrucciones:**

1. En tu editor de texto, crea un nuevo documento o sección titulada "Prompt v1".

2. Redacta el componente **Rol** definiendo la perspectiva experta que necesitas:
   ```text
   [Rol] Actúa como un consultor senior en gestión de riesgos operativos con
   experiencia en empresas de logística y cadena de suministro.
   ```

3. Redacta el componente **Contexto** proporcionando la situación y el material de entrada:
   ```text
   [Contexto] Soy el gerente de operaciones de una empresa de logística mediana.
   He recibido el informe operativo mensual de marzo 2024 y necesito presentar
   al comité directivo una evaluación de riesgos antes de la reunión del próximo
   lunes. El comité espera una visión clara de los riesgos más críticos que
   podrían afectar la continuidad operativa en los próximos 30-60 días.
   ```

4. Redacta el componente **Tarea** con un verbo de acción específico y verificable:
   ```text
   [Tarea] Analiza el siguiente informe operativo e identifica los riesgos
   operativos principales. Para cada riesgo, determina: la descripción del riesgo,
   el área afectada, la probabilidad de materialización (alta/media/baja),
   el impacto potencial (alto/medio/bajo) y una recomendación de mitigación
   inmediata.
   ```

5. Redacta el componente **Formato** especificando la estructura de salida:
   ```text
   [Formato] Presenta los resultados en una tabla con las columnas: N°, Riesgo
   Identificado, Área, Probabilidad, Impacto, Nivel de Prioridad (combinación
   de probabilidad e impacto), Acción de Mitigación Recomendada. Ordena la tabla
   de mayor a menor prioridad. Al final incluye un párrafo de resumen ejecutivo
   de no más de 80 palabras.
   ```

6. Redacta el componente **Restricciones** estableciendo los límites:
   ```text
   [Restricciones] Limita el análisis a un máximo de 6 riesgos principales.
   Usa lenguaje ejecutivo y directo, evitando jerga técnica excesiva. No inventes
   información que no esté contenida en el informe. Las recomendaciones deben
   ser acciones concretas ejecutables en un plazo de 30 días.
   ```

7. Ensambla todos los componentes en un único prompt coherente. Puedes usar las etiquetas entre corchetes o integrarlos en prosa fluida:

```text
Actúa como un consultor senior en gestión de riesgos operativos con experiencia
en empresas de logística y cadena de suministro.

Soy el gerente de operaciones de una empresa de logística mediana. He recibido
el informe operativo mensual de marzo 2024 y necesito presentar al comité
directivo una evaluación de riesgos antes de la reunión del próximo lunes.
El comité espera una visión clara de los riesgos más críticos que podrían
afectar la continuidad operativa en los próximos 30-60 días.

Analiza el siguiente informe operativo e identifica los riesgos operativos
principales. Para cada riesgo, determina: la descripción del riesgo, el área
afectada, la probabilidad de materialización (alta/media/baja), el impacto
potencial (alto/medio/bajo) y una recomendación de mitigación inmediata.

Presenta los resultados en una tabla con las columnas: N°, Riesgo Identificado,
Área, Probabilidad, Impacto, Nivel de Prioridad, Acción de Mitigación
Recomendada. Ordena la tabla de mayor a menor prioridad. Al final incluye un
párrafo de resumen ejecutivo de no más de 80 palabras.

Restricciones: Máximo 6 riesgos principales. Lenguaje ejecutivo y directo.
No inventes información fuera del informe. Recomendaciones ejecutables en 30 días.

INFORME A ANALIZAR:
[Pega aquí el texto completo del informe operativo proporcionado anteriormente]
```

**Resultado Esperado:** Un prompt completo de aproximadamente 250-350 palabras que integra los cinco componentes de forma clara y coherente, seguido del contenido del informe operativo.

**Verificación:**
- [ ] El prompt contiene un rol especializado definido.
- [ ] Se incluye contexto sobre la situación y la audiencia (comité directivo).
- [ ] La tarea utiliza verbos de acción específicos ("analiza", "identifica", "determina").
- [ ] El formato de salida está claramente especificado (tabla con columnas definidas + resumen).
- [ ] Las restricciones establecen límites concretos (máximo 6 riesgos, 30 días, sin inventar datos).

---

### Paso 2: Ejecutar el prompt en la herramienta de IA y analizar la respuesta

**Objetivo:** Enviar el prompt construido a la herramienta de IA generativa y evaluar si la respuesta cumple con los criterios solicitados.

**Instrucciones:**

1. Copia el prompt completo ensamblado en el Paso 1 (incluyendo el texto del informe operativo al final).

2. Pégalo en la interfaz de Microsoft Copilot (o la herramienta de IA disponible) y envíalo.

3. Espera la respuesta completa del modelo.

4. Evalúa la respuesta verificando los siguientes criterios:

   | Criterio | Pregunta de Verificación | ¿Cumple? (Sí/No) |
   |---|---|---|
   | Formato de tabla | ¿La respuesta incluye una tabla con las columnas solicitadas? | |
   | Cantidad de riesgos | ¿Se identificaron entre 4 y 6 riesgos (no más de 6)? | |
   | Ordenamiento | ¿Los riesgos están ordenados por prioridad de mayor a menor? | |
   | Fidelidad al contenido | ¿Todos los riesgos se derivan directamente del informe? | |
   | Resumen ejecutivo | ¿Incluye un párrafo de resumen al final? | |
   | Extensión del resumen | ¿El resumen tiene aproximadamente 80 palabras o menos? | |
   | Lenguaje | ¿El tono es ejecutivo y directo? | |
   | Acciones concretas | ¿Las mitigaciones son ejecutables en 30 días? | |

5. Copia la respuesta generada en tu editor de texto para referencia.

**Resultado Esperado:** La IA debe generar una tabla con 4-6 riesgos operativos identificados (por ejemplo: caída del sistema WMS por capacidad del servidor, incumplimiento de SLA por falta de conductores, riesgo de accidentes por EPP vencidos, dependencia de proveedor único de embalaje, pérdida de visibilidad de flota por vencimiento de licencias GPS). Cada riesgo debe incluir probabilidad, impacto, prioridad y acción de mitigación. Al final debe aparecer un resumen ejecutivo breve.

**Verificación:**
- [ ] La respuesta contiene una tabla estructurada (no texto plano sin formato).
- [ ] Los riesgos identificados son coherentes con el contenido del informe (no hay invenciones).
- [ ] Las acciones de mitigación son específicas y realizables (no genéricas como "mejorar procesos").

---

### Paso 3: Refinar el prompt de forma iterativa

**Objetivo:** Identificar áreas de mejora en la respuesta obtenida y ajustar el prompt para obtener un resultado más preciso o útil.

**Instrucciones:**

1. Revisa la respuesta obtenida en el Paso 2 e identifica al menos un aspecto que pueda mejorarse. Ejemplos comunes:
   - Los riesgos no están suficientemente priorizados.
   - Falta una estimación temporal de cuándo podría materializarse cada riesgo.
   - Las acciones de mitigación son demasiado generales.
   - El resumen ejecutivo no destaca el riesgo más urgente.

2. Redacta un **prompt de refinamiento** (follow-up) que aborde la mejora identificada. Por ejemplo:

   ```text
   Gracias por el análisis. Necesito que refines la tabla agregando una columna
   adicional llamada "Plazo Crítico" que indique en cuántos días podría
   materializarse cada riesgo según la información del informe. Además, en la
   columna de Acción de Mitigación, incluye quién sería el responsable sugerido
   (área o rol) para ejecutar cada acción. Mantén el mismo formato y restricciones
   anteriores.
   ```

3. Envía el prompt de refinamiento a la herramienta de IA.

4. Compara la nueva respuesta con la anterior y documenta las diferencias en tu editor:
   ```text
   MEJORAS OBSERVADAS EN LA ITERACIÓN 2:
   - [Describe qué cambió]
   - [Describe si la nueva columna aporta valor]
   - [Describe si los responsables asignados son coherentes]
   ```

5. Si la respuesta aún no es satisfactoria, realiza una segunda iteración ajustando las restricciones o el formato. Por ejemplo:

   ```text
   Ajusta la priorización considerando que el comité directivo valora
   especialmente los riesgos que afectan directamente al cliente (MEGAMART)
   y los que tienen implicaciones legales (seguridad laboral). Reordena la
   tabla con este criterio.
   ```

**Resultado Esperado:** Una versión mejorada de la tabla de riesgos que incluye información adicional (plazos críticos, responsables) y una priorización ajustada según los criterios del negocio. La respuesta iterada debe ser más accionable que la primera versión.

**Verificación:**
- [ ] El prompt de refinamiento mantiene coherencia con el contexto original (no reinicia la conversación).
- [ ] La respuesta refinada incorpora los ajustes solicitados sin perder la estructura base.
- [ ] Se evidencia una mejora tangible entre la primera y la segunda respuesta.

---

### Paso 4: Documentar el prompt final y aplicar principios de seguridad

**Objetivo:** Consolidar la versión final del prompt como una plantilla reutilizable, incorporando consideraciones de seguridad y manejo responsable de información.

**Instrucciones:**

1. En tu editor de texto, crea una sección titulada "PLANTILLA FINAL - Identificación de Riesgos Operativos".

2. Documenta la versión final del prompt incorporando todas las mejoras iterativas:

   ```text
   PLANTILLA: IDENTIFICACIÓN DE RIESGOS OPERATIVOS

   Rol: Actúa como un consultor senior en gestión de riesgos operativos con
   experiencia en [SECTOR DE LA EMPRESA].

   Contexto: Soy [ROL DEL USUARIO] de [TIPO DE EMPRESA]. He recibido
   [TIPO DE DOCUMENTO] correspondiente al período [PERÍODO] y necesito
   presentar una evaluación de riesgos a [AUDIENCIA] en un plazo de [TIEMPO].
   La audiencia prioriza riesgos que afectan [CRITERIOS DE PRIORIZACIÓN].

   Tarea: Analiza el siguiente [TIPO DE DOCUMENTO] e identifica los riesgos
   operativos principales. Para cada riesgo determina: descripción, área
   afectada, probabilidad (alta/media/baja), impacto (alto/medio/bajo),
   plazo crítico de materialización y acción de mitigación con responsable
   sugerido.

   Formato: Tabla ordenada por prioridad (mayor a menor) con columnas:
   N° | Riesgo | Área | Probabilidad | Impacto | Prioridad | Plazo Crítico |
   Mitigación | Responsable. Incluir resumen ejecutivo de máximo [N] palabras.

   Restricciones: Máximo [N] riesgos. Lenguaje [TONO]. No inventar información
   externa al documento. Acciones ejecutables en [PLAZO].

   NOTA DE SEGURIDAD: [Indicar si el contenido es confidencial y si se han
   anonimizado datos sensibles antes de enviarlo a la herramienta de IA]

   DOCUMENTO A ANALIZAR:
   [INSERTAR CONTENIDO]
   ```

3. Agrega una sección de **consideraciones de seguridad** debajo de la plantilla:

   ```text
   CONSIDERACIONES DE SEGURIDAD Y USO RESPONSABLE:
   
   1. Antes de enviar cualquier informe a una herramienta de IA externa,
      verificar la política de datos de la organización.
   2. Anonimizar nombres de clientes, proveedores y empleados si la política
      lo requiere (ej: reemplazar "MEGAMART" por "Cliente A").
   3. No incluir datos financieros exactos, números de contrato o información
      personal identificable (PII) salvo que se use una instancia corporativa
      aprobada (ej: Microsoft Copilot para Microsoft 365 con tenant propio).
   4. Etiquetar el nivel de confidencialidad del documento antes de procesarlo.
   5. La respuesta generada por la IA debe validarse siempre con criterio humano
      antes de presentarse al comité directivo.
   ```

4. Guarda el documento con un nombre descriptivo: `plantilla_riesgos_operativos_prompt_v1.md`

**Resultado Esperado:** Un documento que contiene la plantilla parametrizada del prompt, lista para ser reutilizada con diferentes informes operativos, junto con las directrices de seguridad para su uso responsable.

**Verificación:**
- [ ] La plantilla tiene campos parametrizados (marcados con corchetes) que permiten adaptarla a diferentes contextos.
- [ ] Se incluyen las cinco secciones de consideraciones de seguridad.
- [ ] El documento está guardado y es reutilizable.

---

## Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes entregables:

| # | Entregable | Criterio de Éxito |
|---|---|---|
| 1 | Prompt v1 completo | Contiene los 5 componentes + texto del informe |
| 2 | Respuesta de la IA evaluada | Tabla con criterios de verificación completados (mínimo 6 de 8 criterios cumplidos) |
| 3 | Prompt de refinamiento | Al menos 1 iteración documentada con mejoras observadas |
| 4 | Plantilla final parametrizada | Campos adaptables + consideraciones de seguridad |

**Prueba final:** Toma la plantilla final y reemplaza los parámetros con un escenario diferente (por ejemplo, imagina que eres gerente de TI analizando un reporte de incidentes de ciberseguridad). Verifica mentalmente que la plantilla se adapta sin necesidad de reestructurarla.

---

## Solución de Problemas

### Problema 1: La IA genera riesgos inventados que no están en el informe

**Síntomas:** La tabla de riesgos incluye elementos como "riesgo de fraude interno" o "riesgo cambiario" que no se mencionan en ninguna parte del informe operativo proporcionado.

**Causa:** La restricción de no inventar información no fue suficientemente explícita, o el modelo interpretó el rol de "consultor senior" como una invitación a agregar riesgos basados en su conocimiento general del sector.

**Solución:** Refuerza la restricción en el prompt añadiendo una instrucción más directa:
```text
IMPORTANTE: Basa tu análisis EXCLUSIVAMENTE en la información explícita del
informe proporcionado. No agregues riesgos inferidos de conocimiento general
del sector. Si un riesgo no tiene evidencia directa en el texto, no lo incluyas.
Cita la sección del informe de donde se deriva cada riesgo identificado.
```

---

### Problema 2: La respuesta no respeta el formato de tabla solicitado

**Síntomas:** La IA devuelve los riesgos en formato de lista con viñetas o en párrafos narrativos en lugar de una tabla con las columnas especificadas.

**Causa:** Algunas herramientas de IA pueden ignorar instrucciones de formato cuando el prompt es muy extenso y el formato se menciona en medio del texto. También puede ocurrir con modelos que tienen limitaciones en la generación de tablas en Markdown.

**Solución:** Mueve la instrucción de formato al final del prompt (justo antes del contenido a analizar) y hazla más explícita:
```text
FORMATO OBLIGATORIO DE RESPUESTA: Genera una tabla en formato Markdown con
exactamente estas columnas separadas por pipes (|):
| N° | Riesgo Identificado | Área | Probabilidad | Impacto | Prioridad | Mitigación |

No uses otro formato. Si no puedes generar tabla, usa el formato:
RIESGO 1: [nombre] / ÁREA: [área] / PROB: [valor] / IMPACTO: [valor] / 
PRIORIDAD: [valor] / MITIGACIÓN: [acción]
```

---

## Limpieza

1. Si utilizaste una sesión de chat en la herramienta de IA, puedes cerrarla o eliminar el historial de conversación si la política de tu organización lo requiere.
2. Conserva el archivo `plantilla_riesgos_operativos_prompt_v1.md` en una ubicación segura para uso futuro.
3. Si utilizaste datos reales de tu organización (no recomendado en este lab), elimina la conversación del historial de la herramienta de IA y verifica que no queden copias en caché.

---

## Resumen

En este laboratorio has aplicado la estructura de cinco componentes de un prompt efectivo para crear una herramienta práctica de identificación de riesgos operativos. Los aprendizajes clave son:

- **La estructura importa:** Un prompt con Rol, Contexto, Tarea, Formato y Restricciones produce resultados significativamente superiores a una solicitud genérica como "identifica riesgos en este documento".
- **La iteración es esencial:** El primer prompt rara vez produce el resultado perfecto. Refinar añadiendo columnas, ajustando priorización o precisando restricciones mejora la utilidad del análisis.
- **La seguridad no es opcional:** Antes de enviar contenido operativo a una herramienta de IA, siempre se deben considerar las implicaciones de confidencialidad y las políticas organizacionales.
- **Las plantillas multiplican el valor:** Parametrizar el prompt permite reutilizarlo en diferentes contextos sin reconstruirlo desde cero.

### Recursos Adicionales

- [Microsoft Copilot - Mejores prácticas para prompts](https://support.microsoft.com/es-es/copilot)
- [Marco de gestión de riesgos ISO 31000](https://www.iso.org/iso-31000-risk-management.html)
- [Guía de uso responsable de IA - Microsoft](https://www.microsoft.com/es-es/ai/responsible-ai)
