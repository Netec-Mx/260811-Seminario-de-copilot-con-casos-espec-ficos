# 1 Demostración: Flujo completo para la creación y uso de agente para comparación de información en contratos y reportes

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 30 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Crear |

## Descripción general

En este laboratorio construirás un agente basado en un modelo de lenguaje (LLM) capaz de comparar información clave entre documentos contractuales y reportes operativos. El flujo completo abarca: la ingesta de documentos, la extracción estructurada de cláusulas y métricas, la creación del agente con herramientas personalizadas, y la generación de un reporte de discrepancias. Al finalizar, dispondrás de un sistema funcional que automatiza la verificación cruzada de datos entre contratos y reportes.

## Objetivos de aprendizaje

- [ ] Diseñar e implementar un agente con LangChain que utilice herramientas personalizadas para comparar documentos.
- [ ] Extraer información estructurada de contratos y reportes mediante prompts especializados.
- [ ] Crear funciones-herramienta (tools) que el agente invoque para realizar comparaciones campo a campo.
- [ ] Generar un reporte consolidado de discrepancias entre contrato y reporte operativo.
- [ ] Validar el flujo completo end-to-end con documentos de prueba.

## Prerrequisitos

### Conocimientos previos

| Tema | Nivel requerido |
|------|----------------|
| Python 3.10+ | Intermedio |
| Conceptos de LLMs y prompting | Básico |
| LangChain (agentes, tools, chains) | Básico |
| Lectura de documentos PDF/texto | Básico |

### Acceso requerido

- Cuenta con acceso a la API de OpenAI (o modelo compatible como Azure OpenAI, Anthropic, etc.)
- Clave de API configurada como variable de entorno
- Terminal con acceso a Internet para instalar dependencias

## Entorno del laboratorio

### Software necesario

| Componente | Versión mínima |
|------------|---------------|
| Python | 3.10+ |
| langchain | 0.2+ |
| langchain-openai | 0.1+ |
| pydantic | 2.0+ |
| python-dotenv | 1.0+ |

### Configuración inicial

```bash
# Crear directorio del proyecto
mkdir -p lab-agent-comparador && cd lab-agent-comparador

# Crear entorno virtual
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows

# Instalar dependencias
pip install langchain langchain-openai pydantic python-dotenv

# Crear archivo de configuración
cat > .env << 'EOF'
OPENAI_API_KEY=sk-tu-clave-aqui
OPENAI_MODEL=gpt-4o-mini
EOF
```

### Estructura del proyecto

```
lab-agent-comparador/
├── .env
├── data/
│   ├── contrato_ejemplo.txt
│   └── reporte_ejemplo.txt
├── src/
│   ├── __init__.py
│   ├── models.py
│   ├── extractor.py
│   ├── tools.py
│   └── agent.py
└── main.py
```

```bash
mkdir -p data src
touch src/__init__.py
```

## Paso a paso

### Paso 1: Crear documentos de prueba

**Objetivo:** Preparar un contrato y un reporte de ejemplo que contengan datos comparables con discrepancias intencionales.

**Instrucciones:**

1. Crea el archivo del contrato de ejemplo:

```bash
cat > data/contrato_ejemplo.txt << 'EOF'
CONTRATO DE PRESTACIÓN DE SERVICIOS N° CS-2024-0891

Fecha de firma: 15 de enero de 2024
Vigencia: 15 de enero de 2024 al 31 de diciembre de 2024

PARTES:
- Contratante: Empresa Tecnológica del Norte S.A. de C.V. (ETN)
- Proveedor: Soluciones Cloud México S.A. de C.V. (SCM)

CLÁUSULA 3 - NIVELES DE SERVICIO (SLA):
3.1 Disponibilidad mínima garantizada: 99.5% mensual
3.2 Tiempo máximo de respuesta ante incidentes críticos: 30 minutos
3.3 Tiempo máximo de resolución de incidentes críticos: 4 horas
3.4 Ventana de mantenimiento programado: Domingos 02:00-06:00 hrs

CLÁUSULA 4 - CONTRAPRESTACIÓN:
4.1 Monto mensual fijo: $150,000.00 MXN + IVA
4.2 Penalización por incumplimiento de SLA disponibilidad: 5% del monto mensual por cada 0.1% por debajo del 99.5%
4.3 Penalización por exceder tiempo de respuesta: $5,000.00 MXN por incidente

CLÁUSULA 5 - CAPACIDAD:
5.1 Servidores dedicados: 12 instancias tipo c5.2xlarge
5.2 Almacenamiento: 5 TB SSD
5.3 Ancho de banda garantizado: 1 Gbps simétrico
5.4 Respaldos: Diarios incrementales, semanales completos, retención 90 días

CLÁUSULA 7 - REPORTES:
7.1 El proveedor entregará reporte mensual de operación antes del día 5 de cada mes
7.2 El reporte incluirá: métricas de disponibilidad, incidentes, capacidad utilizada y respaldos
EOF
```

2. Crea el archivo del reporte operativo de ejemplo:

```bash
cat > data/reporte_ejemplo.txt << 'EOF'
REPORTE MENSUAL DE OPERACIÓN - MARZO 2024
Contrato: CS-2024-0891
Fecha de entrega: 7 de abril de 2024
Elaborado por: Soluciones Cloud México S.A. de C.V.

1. MÉTRICAS DE DISPONIBILIDAD
   - Disponibilidad del mes: 99.2%
   - Horas totales del mes: 744
   - Horas de indisponibilidad no programada: 5.95 horas
   - Ventanas de mantenimiento utilizadas: 3 (domingos 03, 10 y 24 de marzo)
   - Horario de mantenimiento: Domingos 01:00-05:00 hrs

2. INCIDENTES
   - Total de incidentes: 8
   - Incidentes críticos: 3
     * INC-001 (05/mar): Respuesta 25 min, Resolución 3.5 hrs
     * INC-002 (12/mar): Respuesta 45 min, Resolución 5.2 hrs
     * INC-003 (28/mar): Respuesta 20 min, Resolución 2.1 hrs
   - Incidentes no críticos: 5

3. CAPACIDAD
   - Servidores activos: 11 instancias tipo c5.2xlarge (1 en mantenimiento)
   - Almacenamiento utilizado: 3.8 TB de 5 TB disponibles (76%)
   - Ancho de banda promedio: 850 Mbps
   - Ancho de banda pico: 1.2 Gbps

4. RESPALDOS
   - Respaldos diarios ejecutados: 29/31 (2 fallidos los días 14 y 15)
   - Respaldos semanales ejecutados: 4/4
   - Retención configurada: 60 días
   - Último restore de prueba: 20 de marzo (exitoso)

5. FACTURACIÓN
   - Monto facturado: $150,000.00 MXN + IVA
   - Penalizaciones aplicadas: Ninguna
EOF
```

**Resultado esperado:** Dos archivos de texto en el directorio `data/` con información deliberadamente inconsistente para que el agente detecte discrepancias.

**Verificación:**

```bash
ls -la data/
# Debe mostrar contrato_ejemplo.txt y reporte_ejemplo.txt
```

### Paso 2: Definir los modelos de datos

**Objetivo:** Crear modelos Pydantic que representen la información estructurada extraída de ambos documentos.

**Instrucciones:**

1. Crea el archivo de modelos:

```python
# src/models.py
from pydantic import BaseModel, Field
from typing import Optional


class SLAContractual(BaseModel):
    """Niveles de servicio definidos en el contrato."""
    disponibilidad_minima: float = Field(description="Porcentaje mínimo de disponibilidad mensual")
    tiempo_respuesta_critico_min: int = Field(description="Tiempo máximo de respuesta a incidentes críticos en minutos")
    tiempo_resolucion_critico_hrs: float = Field(description="Tiempo máximo de resolución de incidentes críticos en horas")
    ventana_mantenimiento: str = Field(description="Horario de ventana de mantenimiento programado")


class CapacidadContractual(BaseModel):
    """Capacidad contratada según el contrato."""
    servidores: int = Field(description="Número de servidores dedicados")
    tipo_instancia: str = Field(description="Tipo de instancia de servidor")
    almacenamiento_tb: float = Field(description="Almacenamiento en TB")
    ancho_banda_gbps: float = Field(description="Ancho de banda garantizado en Gbps")
    retencion_respaldos_dias: int = Field(description="Días de retención de respaldos")


class DatosContrato(BaseModel):
    """Información estructurada extraída del contrato."""
    numero_contrato: str
    fecha_firma: str
    vigencia_fin: str
    contratante: str
    proveedor: str
    sla: SLAContractual
    capacidad: CapacidadContractual
    monto_mensual: float = Field(description="Monto mensual en MXN")
    dia_entrega_reporte: int = Field(description="Día límite para entrega de reporte mensual")


class IncidenteCritico(BaseModel):
    """Datos de un incidente crítico reportado."""
    id: str
    fecha: str
    tiempo_respuesta_min: int
    tiempo_resolucion_hrs: float


class DatosReporte(BaseModel):
    """Información estructurada extraída del reporte operativo."""
    numero_contrato: str
    periodo: str
    fecha_entrega: str
    disponibilidad_porcentaje: float
    incidentes_criticos: list[IncidenteCritico]
    servidores_activos: int
    tipo_instancia: str
    almacenamiento_utilizado_tb: float
    almacenamiento_total_tb: float
    ancho_banda_promedio_mbps: float
    respaldos_diarios_exitosos: int
    respaldos_diarios_totales: int
    retencion_configurada_dias: int
    ventana_mantenimiento_usada: str
    penalizaciones_aplicadas: str
    monto_facturado: float


class Discrepancia(BaseModel):
    """Una discrepancia encontrada entre contrato y reporte."""
    categoria: str = Field(description="Categoría: SLA, Capacidad, Operación, Facturación")
    campo: str = Field(description="Campo específico donde se encontró la discrepancia")
    valor_contrato: str = Field(description="Valor según el contrato")
    valor_reporte: str = Field(description="Valor según el reporte")
    severidad: str = Field(description="Alta, Media o Baja")
    descripcion: str = Field(description="Descripción clara de la discrepancia")
    impacto_financiero: Optional[str] = Field(default=None, description="Impacto financiero estimado si aplica")


class ReporteDiscrepancias(BaseModel):
    """Reporte consolidado de todas las discrepancias encontradas."""
    contrato: str
    periodo_evaluado: str
    total_discrepancias: int
    discrepancias: list[Discrepancia]
    resumen_ejecutivo: str
```

**Resultado esperado:** Modelos Pydantic bien tipados que servirán como esquemas de extracción y como estructura del reporte final.

**Verificación:**

```bash
python -c "from src.models import DatosContrato, DatosReporte, ReporteDiscrepancias; print('Modelos cargados correctamente')"
```

### Paso 3: Implementar el extractor de documentos

**Objetivo:** Crear el módulo que utiliza el LLM para extraer información estructurada de los documentos de texto.

**Instrucciones:**

1. Crea el extractor:

```python
# src/extractor.py
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from src.models import DatosContrato, DatosReporte

load_dotenv()


def get_llm():
    """Obtiene instancia del LLM configurado."""
    return ChatOpenAI(
        model=os.getenv("OPENAI_MODEL", "gpt-4o-mini"),
        temperature=0
    )


def extraer_datos_contrato(texto_contrato: str) -> DatosContrato:
    """Extrae información estructurada de un contrato."""
    llm = get_llm()
    structured_llm = llm.with_structured_output(DatosContrato)

    prompt = ChatPromptTemplate.from_messages([
        ("system", """Eres un analista legal experto en contratos de servicios tecnológicos.
Tu tarea es extraer información clave del contrato proporcionado y devolverla en formato estructurado.
Extrae todos los campos solicitados con precisión. Si un valor es numérico, conviértelo al tipo correcto.
Para montos, usa solo el valor numérico sin símbolos de moneda."""),
        ("human", "Extrae la información estructurada del siguiente contrato:\n\n{contrato}")
    ])

    chain = prompt | structured_llm
    resultado = chain.invoke({"contrato": texto_contrato})
    return resultado


def extraer_datos_reporte(texto_reporte: str) -> DatosReporte:
    """Extrae información estructurada de un reporte operativo."""
    llm = get_llm()
    structured_llm = llm.with_structured_output(DatosReporte)

    prompt = ChatPromptTemplate.from_messages([
        ("system", """Eres un analista de operaciones TI experto en interpretar reportes de servicio.
Tu tarea es extraer información clave del reporte operativo proporcionado y devolverla en formato estructurado.
Extrae todos los campos solicitados con precisión. Para valores numéricos, conviértelos al tipo correcto.
Para montos, usa solo el valor numérico sin símbolos de moneda.
Para el ancho de banda, convierte a Mbps si es necesario."""),
        ("human", "Extrae la información estructurada del siguiente reporte operativo:\n\n{reporte}")
    ])

    chain = prompt | structured_llm
    resultado = chain.invoke({"reporte": texto_reporte})
    return resultado
```

**Resultado esperado:** Módulo funcional que transforma texto libre en objetos Pydantic estructurados.

**Verificación:**

```bash
python -c "from src.extractor import extraer_datos_contrato, extraer_datos_reporte; print('Extractor importado correctamente')"
```

### Paso 4: Crear las herramientas del agente

**Objetivo:** Implementar las tools que el agente utilizará para realizar la comparación entre documentos.

**Instrucciones:**

1. Crea el archivo de herramientas:

```python
# src/tools.py
import json
from langchain_core.tools import tool
from src.models import (
    DatosContrato, DatosReporte, Discrepancia,
    ReporteDiscrepancias
)


# Variables globales para almacenar datos extraídos durante la sesión
_datos_contrato: DatosContrato | None = None
_datos_reporte: DatosReporte | None = None


def set_datos(contrato: DatosContrato, reporte: DatosReporte):
    """Establece los datos extraídos para que las herramientas los utilicen."""
    global _datos_contrato, _datos_reporte
    _datos_contrato = contrato
    _datos_reporte = reporte


@tool
def comparar_disponibilidad() -> str:
    """Compara la disponibilidad reportada contra el SLA contractual.
    Retorna las discrepancias encontradas en formato JSON."""
    if not _datos_contrato or not _datos_reporte:
        return json.dumps({"error": "Datos no cargados"})

    discrepancias = []
    sla_minimo = _datos_contrato.sla.disponibilidad_minima
    disponibilidad_real = _datos_reporte.disponibilidad_porcentaje

    if disponibilidad_real < sla_minimo:
        diferencia = sla_minimo - disponibilidad_real
        # Calcular penalización: 5% por cada 0.1% debajo del SLA
        penalizacion_pct = (diferencia / 0.1) * 5
        penalizacion_monto = _datos_contrato.monto_mensual * (penalizacion_pct / 100)

        discrepancias.append(Discrepancia(
            categoria="SLA",
            campo="Disponibilidad mensual",
            valor_contrato=f"{sla_minimo}% mínimo",
            valor_reporte=f"{disponibilidad_real}%",
            severidad="Alta",
            descripcion=f"La disponibilidad ({disponibilidad_real}%) está {diferencia:.1f}% por debajo del SLA mínimo ({sla_minimo}%)",
            impacto_financiero=f"Penalización estimada: ${penalizacion_monto:,.2f} MXN ({penalizacion_pct:.0f}% del monto mensual)"
        ).model_dump())

    return json.dumps({"discrepancias": discrepancias, "total": len(discrepancias)}, ensure_ascii=False)


@tool
def comparar_tiempos_respuesta() -> str:
    """Compara los tiempos de respuesta y resolución de incidentes críticos contra el SLA.
    Retorna las discrepancias encontradas en formato JSON."""
    if not _datos_contrato or not _datos_reporte:
        return json.dumps({"error": "Datos no cargados"})

    discrepancias = []
    max_respuesta = _datos_contrato.sla.tiempo_respuesta_critico_min
    max_resolucion = _datos_contrato.sla.tiempo_resolucion_critico_hrs

    for inc in _datos_reporte.incidentes_criticos:
        if inc.tiempo_respuesta_min > max_respuesta:
            discrepancias.append(Discrepancia(
                categoria="SLA",
                campo=f"Tiempo de respuesta - {inc.id}",
                valor_contrato=f"{max_respuesta} minutos máximo",
                valor_reporte=f"{inc.tiempo_respuesta_min} minutos",
                severidad="Alta",
                descripcion=f"Incidente {inc.id} ({inc.fecha}): tiempo de respuesta de {inc.tiempo_respuesta_min} min excede el máximo de {max_respuesta} min",
                impacto_financiero=f"Penalización por exceder tiempo de respuesta: $5,000.00 MXN"
            ).model_dump())

        if inc.tiempo_resolucion_hrs > max_resolucion:
            discrepancias.append(Discrepancia(
                categoria="SLA",
                campo=f"Tiempo de resolución - {inc.id}",
                valor_contrato=f"{max_resolucion} horas máximo",
                valor_reporte=f"{inc.tiempo_resolucion_hrs} horas",
                severidad="Alta",
                descripcion=f"Incidente {inc.id} ({inc.fecha}): tiempo de resolución de {inc.tiempo_resolucion_hrs} hrs excede el máximo de {max_resolucion} hrs",
                impacto_financiero=None
            ).model_dump())

    return json.dumps({"discrepancias": discrepancias, "total": len(discrepancias)}, ensure_ascii=False)


@tool
def comparar_capacidad() -> str:
    """Compara la capacidad reportada contra lo contratado (servidores, almacenamiento, ancho de banda, respaldos).
    Retorna las discrepancias encontradas en formato JSON."""
    if not _datos_contrato or not _datos_reporte:
        return json.dumps({"error": "Datos no cargados"})

    discrepancias = []
    cap = _datos_contrato.capacidad

    # Servidores
    if _datos_reporte.servidores_activos < cap.servidores:
        discrepancias.append(Discrepancia(
            categoria="Capacidad",
            campo="Servidores activos",
            valor_contrato=f"{cap.servidores} instancias {cap.tipo_instancia}",
            valor_reporte=f"{_datos_reporte.servidores_activos} instancias activas",
            severidad="Media",
            descripcion=f"Solo {_datos_reporte.servidores_activos} de {cap.servidores} servidores contratados están activos",
            impacto_financiero=None
        ).model_dump())

    # Ancho de banda
    ancho_banda_contratado_mbps = cap.ancho_banda_gbps * 1000
    if _datos_reporte.ancho_banda_promedio_mbps < ancho_banda_contratado_mbps * 0.9:
        discrepancias.append(Discrepancia(
            categoria="Capacidad",
            campo="Ancho de banda promedio",
            valor_contrato=f"{ancho_banda_contratado_mbps:.0f} Mbps garantizado",
            valor_reporte=f"{_datos_reporte.ancho_banda_promedio_mbps:.0f} Mbps promedio",
            severidad="Media",
            descripcion=f"El ancho de banda promedio ({_datos_reporte.ancho_banda_promedio_mbps} Mbps) está por debajo del garantizado ({ancho_banda_contratado_mbps} Mbps)",
            impacto_financiero=None
        ).model_dump())

    # Retención de respaldos
    if _datos_reporte.retencion_configurada_dias < cap.retencion_respaldos_dias:
        discrepancias.append(Discrepancia(
            categoria="Capacidad",
            campo="Retención de respaldos",
            valor_contrato=f"{cap.retencion_respaldos_dias} días",
            valor_reporte=f"{_datos_reporte.retencion_configurada_dias} días",
            severidad="Alta",
            descripcion=f"La retención configurada ({_datos_reporte.retencion_configurada_dias} días) es menor a la contratada ({cap.retencion_respaldos_dias} días)",
            impacto_financiero=None
        ).model_dump())

    # Respaldos fallidos
    if _datos_reporte.respaldos_diarios_exitosos < _datos_reporte.respaldos_diarios_totales:
        fallidos = _datos_reporte.respaldos_diarios_totales - _datos_reporte.respaldos_diarios_exitosos
        discrepancias.append(Discrepancia(
            categoria="Operación",
            campo="Respaldos diarios",
            valor_contrato="Respaldos diarios incrementales (sin fallas)",
            valor_reporte=f"{_datos_reporte.respaldos_diarios_exitosos}/{_datos_reporte.respaldos_diarios_totales} exitosos ({fallidos} fallidos)",
            severidad="Media",
            descripcion=f"Se registraron {fallidos} respaldos diarios fallidos en el periodo",
            impacto_financiero=None
        ).model_dump())

    return json.dumps({"discrepancias": discrepancias, "total": len(discrepancias)}, ensure_ascii=False)


@tool
def comparar_operacion() -> str:
    """Compara aspectos operativos: ventana de mantenimiento, fecha de entrega del reporte.
    Retorna las discrepancias encontradas en formato JSON."""
    if not _datos_contrato or not _datos_reporte:
        return json.dumps({"error": "Datos no cargados"})

    discrepancias = []

    # Ventana de mantenimiento
    ventana_contrato = _datos_contrato.sla.ventana_mantenimiento
    ventana_reporte = _datos_reporte.ventana_mantenimiento_usada
    if ventana_contrato.lower() != ventana_reporte.lower():
        discrepancias.append(Discrepancia(
            categoria="Operación",
            campo="Ventana de mantenimiento",
            valor_contrato=ventana_contrato,
            valor_reporte=ventana_reporte,
            severidad="Media",
            descripcion=f"La ventana de mantenimiento utilizada ({ventana_reporte}) difiere de la contractual ({ventana_contrato})",
            impacto_financiero=None
        ).model_dump())

    # Fecha de entrega del reporte (debe ser antes del día 5)
    dia_limite = _datos_contrato.dia_entrega_reporte
    # Extraer día de la fecha de entrega
    fecha_entrega = _datos_reporte.fecha_entrega
    try:
        dia_entrega = int(''.join(filter(str.isdigit, fecha_entrega.split("de")[0].strip())))
        if dia_entrega > dia_limite:
            discrepancias.append(Discrepancia(
                categoria="Operación",
                campo="Fecha de entrega del reporte",
                valor_contrato=f"Antes del día {dia_limite} de cada mes",
                valor_reporte=f"Entregado el {fecha_entrega}",
                severidad="Baja",
                descripcion=f"El reporte fue entregado el día {dia_entrega}, superando el límite del día {dia_limite}",
                impacto_financiero=None
            ).model_dump())
    except (ValueError, IndexError):
        pass

    return json.dumps({"discrepancias": discrepancias, "total": len(discrepancias)}, ensure_ascii=False)


@tool
def comparar_facturacion() -> str:
    """Compara la facturación contra las penalizaciones que deberían aplicarse.
    Retorna las discrepancias encontradas en formato JSON."""
    if not _datos_contrato or not _datos_reporte:
        return json.dumps({"error": "Datos no cargados"})

    discrepancias = []

    # Verificar si hay penalizaciones que deberían aplicarse
    sla_minimo = _datos_contrato.sla.disponibilidad_minima
    disponibilidad_real = _datos_reporte.disponibilidad_porcentaje
    penalizaciones_reportadas = _datos_reporte.penalizaciones_aplicadas.lower()

    hay_incumplimiento_sla = disponibilidad_real < sla_minimo

    # Verificar incidentes con tiempo de respuesta excedido
    max_respuesta = _datos_contrato.sla.tiempo_respuesta_critico_min
    incidentes_excedidos = [
        inc for inc in _datos_reporte.incidentes_criticos
        if inc.tiempo_respuesta_min > max_respuesta
    ]

    if (hay_incumplimiento_sla or incidentes_excedidos) and "ninguna" in penalizaciones_reportadas:
        # Calcular penalización esperada
        penalizacion_total = 0.0
        detalles = []

        if hay_incumplimiento_sla:
            diferencia = sla_minimo - disponibilidad_real
            penalizacion_pct = (diferencia / 0.1) * 5
            pen_sla = _datos_contrato.monto_mensual * (penalizacion_pct / 100)
            penalizacion_total += pen_sla
            detalles.append(f"SLA disponibilidad: ${pen_sla:,.2f}")

        for inc in incidentes_excedidos:
            penalizacion_total += 5000.0
            detalles.append(f"Respuesta {inc.id}: $5,000.00")

        discrepancias.append(Discrepancia(
            categoria="Facturación",
            campo="Penalizaciones no aplicadas",
            valor_contrato=f"Penalizaciones por incumplimiento: {'; '.join(detalles)}",
            valor_reporte="Penalizaciones aplicadas: Ninguna",
            severidad="Alta",
            descripcion=f"El reporte indica que no se aplicaron penalizaciones, pero se detectaron incumplimientos que generan penalizaciones por ${penalizacion_total:,.2f} MXN",
            impacto_financiero=f"${penalizacion_total:,.2f} MXN en penalizaciones no cobradas"
        ).model_dump())

    return json.dumps({"discrepancias": discrepancias, "total": len(discrepancias)}, ensure_ascii=False)


def get_tools():
    """Retorna la lista de herramientas disponibles para el agente."""
    return [
        comparar_disponibilidad,
        comparar_tiempos_respuesta,
        comparar_capacidad,
        comparar_operacion,
        comparar_facturacion
    ]
```

**Resultado esperado:** Cinco herramientas especializadas que realizan comparaciones por categoría y retornan discrepancias en formato JSON.

**Verificación:**

```bash
python -c "from src.tools import get_tools; tools = get_tools(); print(f'{len(tools)} herramientas cargadas: {[t.name for t in tools]}')"
```

Salida esperada:
```
5 herramientas cargadas: ['comparar_disponibilidad', 'comparar_tiempos_respuesta', 'comparar_capacidad', 'comparar_operacion', 'comparar_facturacion']
```

### Paso 5: Implementar el agente

**Objetivo:** Crear el agente que orquesta las herramientas para realizar la comparación completa.

**Instrucciones:**

1. Crea el archivo del agente:

```python
# src/agent.py
import os
import json
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain.agents import create_tool_calling_agent, AgentExecutor
from src.tools import get_tools, set_datos
from src.extractor import extraer_datos_contrato, extraer_datos_reporte
from src.models import DatosContrato, DatosReporte

load_dotenv()

SYSTEM_PROMPT = """Eres un agente especializado en auditoría de contratos de servicios tecnológicos.
Tu trabajo es comparar la información de un contrato contra un reporte operativo mensual para identificar discrepancias.

PROCESO QUE DEBES SEGUIR:
1. Usa TODAS las herramientas disponibles para comparar cada aspecto:
   - comparar_disponibilidad: verifica el SLA de disponibilidad
   - comparar_tiempos_respuesta: verifica tiempos de respuesta y resolución de incidentes
   - comparar_capacidad: verifica servidores, almacenamiento, ancho de banda y respaldos
   - comparar_operacion: verifica ventanas de mantenimiento y plazos de entrega
   - comparar_facturacion: verifica que las penalizaciones se hayan aplicado correctamente

2. Después de usar todas las herramientas, genera un REPORTE EJECUTIVO consolidado con:
   - Resumen de hallazgos
   - Lista de todas las discrepancias organizadas por severidad (Alta, Media, Baja)
   - Impacto financiero total estimado
   - Recomendaciones

Sé exhaustivo y profesional. Usa TODAS las herramientas antes de generar el reporte final."""


def crear_agente() -> AgentExecutor:
    """Crea y configura el agente con sus herramientas."""
    llm = ChatOpenAI(
        model=os.getenv("OPENAI_MODEL", "gpt-4o-mini"),
        temperature=0
    )

    tools = get_tools()

    prompt = ChatPromptTemplate.from_messages([
        ("system", SYSTEM_PROMPT),
        ("human", "{input}"),
        MessagesPlaceholder(variable_name="agent_scratchpad"),
    ])

    agent = create_tool_calling_agent(llm, tools, prompt)

    executor = AgentExecutor(
        agent=agent,
        tools=tools,
        verbose=True,
        max_iterations=10,
        return_intermediate_steps=True
    )

    return executor


def ejecutar_comparacion(ruta_contrato: str, ruta_reporte: str) -> dict:
    """
    Ejecuta el flujo completo de comparación:
    1. Lee los documentos
    2. Extrae datos estructurados
    3. Ejecuta el agente para comparar
    4. Retorna el resultado

    Args:
        ruta_contrato: Ruta al archivo del contrato
        ruta_reporte: Ruta al archivo del reporte

    Returns:
        dict con los resultados de la comparación
    """
    # 1. Leer documentos
    print("=" * 60)
    print("FASE 1: Lectura de documentos")
    print("=" * 60)

    with open(ruta_contrato, 'r', encoding='utf-8') as f:
        texto_contrato = f.read()
    print(f"✓ Contrato leído: {len(texto_contrato)} caracteres")

    with open(ruta_reporte, 'r', encoding='utf-8') as f:
        texto_reporte = f.read()
    print(f"✓ Reporte leído: {len(texto_reporte)} caracteres")

    # 2. Extraer datos estructurados
    print("\n" + "=" * 60)
    print("FASE 2: Extracción de datos estructurados")
    print("=" * 60)

    datos_contrato = extraer_datos_contrato(texto_contrato)
    print(f"✓ Contrato procesado: {datos_contrato.numero_contrato}")
    print(f"  - SLA Disponibilidad: {datos_contrato.sla.disponibilidad_minima}%")
    print(f"  - Servidores: {datos_contrato.capacidad.servidores}")
    print(f"  - Monto mensual: ${datos_contrato.monto_mensual:,.2f} MXN")

    datos_reporte = extraer_datos_reporte(texto_reporte)
    print(f"✓ Reporte procesado: periodo {datos_reporte.periodo}")
    print(f"  - Disponibilidad: {datos_reporte.disponibilidad_porcentaje}%")
    print(f"  - Incidentes críticos: {len(datos_reporte.incidentes_criticos)}")
    print(f"  - Servidores activos: {datos_reporte.servidores_activos}")

    # 3. Configurar datos para las herramientas
    set_datos(datos_contrato, datos_reporte)

    # 4. Ejecutar el agente
    print("\n" + "=" * 60)
    print("FASE 3: Ejecución del agente de comparación")
    print("=" * 60)

    agente = crear_agente()

    input_message = f"""Realiza una comparación completa entre el contrato {datos_contrato.numero_contrato} 
y el reporte operativo del periodo {datos_reporte.periodo}.

Datos del contrato ya extraídos:
- SLA Disponibilidad mínima: {datos_contrato.sla.disponibilidad_minima}%
- Tiempo respuesta máximo: {datos_contrato.sla.tiempo_respuesta_critico_min} min
- Tiempo resolución máximo: {datos_contrato.sla.tiempo_resolucion_critico_hrs} hrs
- Servidores contratados: {datos_contrato.capacidad.servidores}
- Retención respaldos: {datos_contrato.capacidad.retencion_respaldos_dias} días
- Monto mensual: ${datos_contrato.monto_mensual:,.2f} MXN

Datos del reporte ya extraídos:
- Disponibilidad reportada: {datos_reporte.disponibilidad_porcentaje}%
- Incidentes críticos: {len(datos_reporte.incidentes_criticos)}
- Servidores activos: {datos_reporte.servidores_activos}
- Retención configurada: {datos_reporte.retencion_configurada_dias} días
- Penalizaciones aplicadas: {datos_reporte.penalizaciones_aplicadas}

Usa TODAS las herramientas de comparación y genera el reporte final de discrepancias."""

    resultado = agente.invoke({"input": input_message})

    # 5. Retornar resultados
    return {
        "datos_contrato": datos_contrato.model_dump(),
        "datos_reporte": datos_reporte.model_dump(),
        "reporte_agente": resultado["output"],
        "pasos_intermedios": len(resultado.get("intermediate_steps", []))
    }
```

**Resultado esperado:** Un módulo que orquesta todo el flujo: lectura → extracción → comparación por agente → reporte.

**Verificación:**

```bash
python -c "from src.agent import crear_agente; agent = crear_agente(); print('Agente creado correctamente')"
```

### Paso 6: Crear el script principal y ejecutar

**Objetivo:** Integrar todos los componentes y ejecutar el flujo completo de comparación.

**Instrucciones:**

1. Crea el archivo principal:

```python
# main.py
import json
import sys
from datetime import datetime
from src.agent import ejecutar_comparacion


def main():
    """Punto de entrada principal del laboratorio."""
    print("\n" + "╔" + "═" * 58 + "╗")
    print("║  AGENTE DE COMPARACIÓN: CONTRATOS vs REPORTES            ║")
    print("║  Lab 02-00-01                                            ║")
    print("╚" + "═" * 58 + "╝\n")

    ruta_contrato = "data/contrato_ejemplo.txt"
    ruta_reporte = "data/reporte_ejemplo.txt"

    # Verificar que los archivos existen
    import os
    if not os.path.exists(ruta_contrato):
        print(f"ERROR: No se encontró el archivo {ruta_contrato}")
        sys.exit(1)
    if not os.path.exists(ruta_reporte):
        print(f"ERROR: No se encontró el archivo {ruta_reporte}")
        sys.exit(1)

    # Ejecutar comparación
    inicio = datetime.now()
    resultado = ejecutar_comparacion(ruta_contrato, ruta_reporte)
    duracion = (datetime.now() - inicio).total_seconds()

    # Mostrar reporte final
    print("\n" + "=" * 60)
    print("REPORTE FINAL DEL AGENTE")
    print("=" * 60)
    print(resultado["reporte_agente"])

    print("\n" + "-" * 60)
    print(f"Tiempo de ejecución: {duracion:.1f} segundos")
    print(f"Pasos del agente: {resultado['pasos_intermedios']}")
    print("-" * 60)

    # Guardar resultado completo
    output_path = "data/resultado_comparacion.json"
    with open(output_path, 'w', encoding='utf-8') as f:
        json.dump({
            "timestamp": datetime.now().isoformat(),
            "duracion_segundos": duracion,
            "reporte_agente": resultado["reporte_agente"],
            "pasos_intermedios": resultado["pasos_intermedios"]
        }, f, ensure_ascii=False, indent=2)
    print(f"\n✓ Resultado guardado en: {output_path}")


if __name__ == "__main__":
    main()
```

2. Ejecuta el flujo completo:

```bash
python main.py
```

**Resultado esperado:** El agente debe ejecutarse mostrando las fases del proceso y producir un reporte que identifique al menos las siguientes discrepancias:

- **SLA Disponibilidad:** 99.2% vs 99.5% mínimo contractual
- **Tiempo de respuesta INC-002:** 45 min vs 30 min máximo
- **Tiempo de resolución INC-002:** 5.2 hrs vs 4 hrs máximo
- **Servidores activos:** 11 vs 12 contratados
- **Retención de respaldos:** 60 días vs 90 días contractuales
- **Ventana de mantenimiento:** horario diferente al contractual
- **Fecha de entrega del reporte:** día 7 vs día 5 límite
- **Penalizaciones no aplicadas:** se reporta "Ninguna" cuando hay incumplimientos
- **Respaldos fallidos:** 2 respaldos diarios no ejecutados

**Verificación:**

```bash
# Verificar que se generó el archivo de resultados
cat data/resultado_comparacion.json | python -m json.tool | head -20

# Verificar que el reporte menciona discrepancias clave
grep -i "discrepancia\|penalización\|incumplimiento" data/resultado_comparacion.json
```

### Paso 7: Agregar prueba automatizada

**Objetivo:** Crear un test que valide que el flujo de extracción y las herramientas de comparación funcionan correctamente.

**Instrucciones:**

1. Instala pytest:

```bash
pip install pytest
```

2. Crea el archivo de pruebas:

```python
# test_agent.py
import json
import pytest
from src.models import (
    DatosContrato, DatosReporte, SLAContractual,
    CapacidadContractual, IncidenteCritico
)
from src.tools import (
    set_datos, comparar_disponibilidad, comparar_tiempos_respuesta,
    comparar_capacidad, comparar_operacion, comparar_facturacion
)


@pytest.fixture
def datos_prueba():
    """Fixture con datos de prueba que simulan la extracción."""
    contrato = DatosContrato(
        numero_contrato="CS-2024-0891",
        fecha_firma="15 de enero de 2024",
        vigencia_fin="31 de diciembre de 2024",
        contratante="ETN",
        proveedor="SCM",
        sla=SLAContractual(
            disponibilidad_minima=99.5,
            tiempo_respuesta_critico_min=30,
            tiempo_resolucion_critico_hrs=4.0,
            ventana_mantenimiento="Domingos 02:00-06:00 hrs"
        ),
        capacidad=CapacidadContractual(
            servidores=12,
            tipo_instancia="c5.2xlarge",
            almacenamiento_tb=5.0,
            ancho_banda_gbps=1.0,
            retencion_respaldos_dias=90
        ),
        monto_mensual=150000.0,
        dia_entrega_reporte=5
    )

    reporte = DatosReporte(
        numero_contrato="CS-2024-0891",
        periodo="Marzo 2024",
        fecha_entrega="7 de abril de 2024",
        disponibilidad_porcentaje=99.2,
        incidentes_criticos=[
            IncidenteCritico(id="INC-001", fecha="05/mar", tiempo_respuesta_min=25, tiempo_resolucion_hrs=3.5),
            IncidenteCritico(id="INC-002", fecha="12/mar", tiempo_respuesta_min=45, tiempo_resolucion_hrs=5.2),
            IncidenteCritico(id="INC-003", fecha="28/mar", tiempo_respuesta_min=20, tiempo_resolucion_hrs=2.1),
        ],
        servidores_activos=11,
        tipo_instancia="c5.2xlarge",
        almacenamiento_utilizado_tb=3.8,
        almacenamiento_total_tb=5.0,
        ancho_banda_promedio_mbps=850.0,
        respaldos_diarios_exitosos=29,
        respaldos_diarios_totales=31,
        retencion_configurada_dias=60,
        ventana_mantenimiento_usada="Domingos 01:00-05:00 hrs",
        penalizaciones_aplicadas="Ninguna",
        monto_facturado=150000.0
    )

    set_datos(contrato, reporte)
    return contrato, reporte


def test_comparar_disponibilidad(datos_prueba):
    """Verifica que se detecta incumplimiento de SLA de disponibilidad."""
    resultado = comparar_disponibilidad.invoke({})
    data = json.loads(resultado)
    assert data["total"] >= 1
    assert "99.2" in data["discrepancias"][0]["valor_reporte"]
    assert data["discrepancias"][0]["severidad"] == "Alta"


def test_comparar_tiempos_respuesta(datos_prueba):
    """Verifica que se detectan incidentes con tiempos excedidos."""
    resultado = comparar_tiempos_respuesta.invoke({})
    data = json.loads(resultado)
    assert data["total"] >= 2  # INC-002 excede respuesta Y resolución
    # Verificar que INC-002 está en las discrepancias
    ids_encontrados = [d["campo"] for d in data["discrepancias"]]
    assert any("INC-002" in campo for campo in ids_encontrados)


def test_comparar_capacidad(datos_prueba):
    """Verifica que se detectan discrepancias de capacidad."""
    resultado = comparar_capacidad.invoke({})
    data = json.loads(resultado)
    assert data["total"] >= 2  # Servidores + retención al menos
    campos = [d["campo"] for d in data["discrepancias"]]
    assert any("ervidores" in c for c in campos)
    assert any("etención" in c or "espaldos" in c.lower() for c in campos)


def test_comparar_operacion(datos_prueba):
    """Verifica que se detectan discrepancias operativas."""
    resultado = comparar_operacion.invoke({})
    data = json.loads(resultado)
    assert data["total"] >= 1  # Ventana de mantenimiento diferente
    campos = [d["campo"] for d in data["discrepancias"]]
    assert any("mantenimiento" in c.lower() or "entrega" in c.lower() for c in campos)


def test_comparar_facturacion(datos_prueba):
    """Verifica que se detecta falta de penalizaciones."""
    resultado = comparar_facturacion.invoke({})
    data = json.loads(resultado)
    assert data["total"] >= 1
    assert data["discrepancias"][0]["severidad"] == "Alta"
    assert "penalización" in data["discrepancias"][0]["descripcion"].lower() or \
           "penalizacion" in data["discrepancias"][0]["descripcion"].lower()
```

3. Ejecuta las pruebas:

```bash
pytest test_agent.py -v
```

**Resultado esperado:**

```
test_agent.py::test_comparar_disponibilidad PASSED
test_agent.py::test_comparar_tiempos_respuesta PASSED
test_agent.py::test_comparar_capacidad PASSED
test_agent.py::test_comparar_operacion PASSED
test_agent.py::test_comparar_facturacion PASSED

========================= 5 passed =========================
```

**Verificación:** Todas las pruebas deben pasar, confirmando que la lógica de comparación funciona correctamente sin necesidad de llamadas al LLM (las pruebas usan datos pre-construidos).

## Validación y pruebas

Para validar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

| # | Criterio | Comando de verificación |
|---|----------|------------------------|
| 1 | Documentos de prueba creados | `ls data/contrato_ejemplo.txt data/reporte_ejemplo.txt` |
| 2 | Modelos Pydantic funcionales | `python -c "from src.models import *; print('OK')"` |
| 3 | Extractor importable | `python -c "from src.extractor import *; print('OK')"` |
| 4 | Herramientas registradas | `python -c "from src.tools import get_tools; assert len(get_tools()) == 5"` |
| 5 | Agente creado sin errores | `python -c "from src.agent import crear_agente; crear_agente(); print('OK')"` |
| 6 | Tests unitarios pasan | `pytest test_agent.py -v` |
| 7 | Flujo completo ejecutado | `test -f data/resultado_comparacion.json && echo 'OK'` |

### Validación del reporte generado

El reporte del agente debe contener al menos:

- ✅ Identificación de incumplimiento de SLA de disponibilidad (99.2% < 99.5%)
- ✅ Detección de al menos un incidente con tiempo de respuesta excedido
- ✅ Identificación de servidores faltantes (11 vs 12)
- ✅ Discrepancia en retención de respaldos (60 vs 90 días)
- ✅ Penalizaciones no aplicadas con estimación de monto

## Solución de problemas

### Problema 1: Error de autenticación con la API de OpenAI

**Síntomas:**
```
openai.AuthenticationError: Error code: 401 - Invalid API key
```

**Causa:** La variable de entorno `OPENAI_API_KEY` no está configurada correctamente o el archivo `.env` no se está cargando.

**Solución:**

```bash
# Verificar que el archivo .env existe y tiene la clave
cat .env | grep OPENAI_API_KEY

# Verificar que la variable se carga correctamente
python -c "from dotenv import load_dotenv; import os; load_dotenv(); print(os.getenv('OPENAI_API_KEY', 'NO CONFIGURADA')[:10] + '...')"

# Si no funciona, exportar directamente:
export OPENAI_API_KEY="sk-tu-clave-real-aqui"

# Verificar que la clave es válida
python -c "
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model='gpt-4o-mini', temperature=0)
print(llm.invoke('Hola').content)
"
```

### Problema 2: El agente no utiliza todas las herramientas

**Síntomas:** El reporte final del agente está incompleto o solo menciona una o dos categorías de discrepancias. En los logs con `verbose=True` se observa que el agente solo invocó 1-2 herramientas antes de generar la respuesta.

**Causa:** El modelo puede decidir que ya tiene suficiente información después de usar pocas herramientas, especialmente con modelos más pequeños o con temperature > 0.

**Solución:**

```python
# En src/agent.py, reforzar el prompt del sistema:

# Opción 1: Hacer el prompt más directivo
SYSTEM_PROMPT = """...(mantener texto existente)...

IMPORTANTE: DEBES usar las 5 herramientas en este orden EXACTO antes de generar el reporte:
1. comparar_disponibilidad
2. comparar_tiempos_respuesta
3. comparar_capacidad
4. comparar_operacion
5. comparar_facturacion

NO generes el reporte final hasta haber usado TODAS las herramientas."""

# Opción 2: Aumentar max_iterations si el agente se detiene prematuramente
executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True,
    max_iterations=15,  # Aumentar de 10 a 15
    return_intermediate_steps=True,
    handle_parsing_errors=True  # Manejar errores de parsing
)

# Opción 3: Usar un modelo más capaz
# En .env cambiar:
# OPENAI_MODEL=gpt-4o
```

## Limpieza

```bash
# Desactivar entorno virtual
deactivate

# (Opcional) Eliminar el proyecto completo
cd ..
rm -rf lab-agent-comparador

# (Opcional) Solo eliminar archivos generados, manteniendo el código
rm -f data/resultado_comparacion.json
```

## Resumen

En este laboratorio has construido un sistema completo de comparación automatizada entre contratos y reportes operativos. Los componentes implementados fueron:

1. **Modelos de datos** (Pydantic): Esquemas tipados para representar información contractual y operativa.
2. **Extractor** (LLM + structured output): Transformación de texto libre a datos estructurados.
3. **Herramientas especializadas** (LangChain tools): Cinco funciones de comparación por categoría.
4. **Agente orquestador** (LangChain AgentExecutor): Coordinación inteligente del flujo de comparación.
5. **Pruebas unitarias** (pytest): Validación de la lógica de negocio sin dependencia del LLM.

### Discrepancias detectadas por el sistema

| Categoría | Discrepancia | Severidad |
|-----------|-------------|-----------|
| SLA | Disponibilidad 99.2% < 99.5% | Alta |
| SLA | INC-002 respuesta 45 min > 30 min | Alta |
| SLA | INC-002 resolución 5.2 hrs > 4 hrs | Alta |
| Capacidad | 11 servidores activos vs 12 contratados | Media |
| Capacidad | Retención 60 días vs 90 días | Alta |
| Operación | Ventana mantenimiento diferente | Media |
| Operación | Reporte entregado día 7 (límite: día 5) | Baja |
| Operación | 2 respaldos diarios fallidos | Media |
| Facturación | Penalizaciones no aplicadas (~$27,500 MXN) | Alta |

### Recursos adicionales

- [Documentación de LangChain Agents](https://python.langchain.com/docs/modules/agents/)
- [Structured Output con LangChain](https://python.langchain.com/docs/how_to/structured_output/)
- [Tool Calling en LangChain](https://python.langchain.com/docs/how_to/tool_calling/)
- [Pydantic v2 Documentation](https://docs.pydantic.dev/latest/)
