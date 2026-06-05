# 📊 PROMPT IA — Informe Ejecutivo Semanal de Rentabilidad por Proyecto
**Versión 2.0 | Uso interno | Dirección de Proyectos**

---

> **INSTRUCCIÓN PARA EL PM:**
> Completa la sección **"INFORMACIÓN DEL PROYECTO"** al final de este prompt,
> adjunta los archivos de tu proyecto y envía todo a Claude (claude.ai).
> Recibirás un archivo HTML listo para guardar en la carpeta `semanas/` del dashboard.

---

Actúa como un analista financiero de proyectos TI especializado en control de costos, revenue y rentabilidad.

Tu tarea es generar un **informe ejecutivo semanal en formato HTML completo y autocontenido** para el Project Manager, que compare costos reales vs presupuesto y analice la rentabilidad del proyecto con gráficas comparativas.

---

## 📥 ARCHIVOS QUE DEBES ADJUNTAR JUNTO A ESTE PROMPT

1. **Archivo CET del proyecto** (.xlsx)
   → Usar únicamente la hoja **"Totales"** como fuente oficial del presupuesto

2. **Reporte de nómina** (.xlsx) — entregado por Alejandra
   → Columnas requeridas: Date, User, Timesheets - All hours, Hourly (COP), Total Hora

3. **Archivo de horas extras / recargos** (.xlsx) — No tiene valor 0 entregado por Alejandra
   → Puede tener múltiples hojas (una por mes)
   → Solo se deben incluir conceptos de **RECARGO** (nocturno, festivo, nocturno festivo)
   → **NO incluir HORAS EXTRA** para no duplicar el valor de hora ya registrado en nómina
   → Solo se debe cruzar el personal que aparezca en el archivo de nómina del proyecto

4. **Reporte de costos AWS** (.csv) — extraído directamente de la consola AWS
   → Tal como se descarga, sin modificar

5. **Reporte de control de gastos del proyecto** (.xlsx) No tiene valor 0
   → Incluye licencias, hospedaje, alimentación, auditores externos, otros

---

## ⚙️ REGLAS DE CÁLCULO OBLIGATORIAS

- **TRM fija: $4,000 COP/USD** para todas las conversiones
- **Todo el informe en COP** (no mostrar USD)
- El presupuesto base SIEMPRE se toma del **CET hoja Totales**
- **Nómina + Gastos operativos = Implementación Fase 1** (misma línea)
- Los recargos son costo adicional de personas, NO duplicar el valor hora
- Validar consistencia entre AWS reportado y valores del CET
- Si no hay archivo de horas extras, omitir esa línea sin error

---

## 💰 CLASIFICACIÓN DE COSTOS (OBLIGATORIA)

| Concepto                                 | Línea en el informe    |
|------------------------------------------|------------------------|
| Nómina normal (reporte Alejandra)        | Implementación Fase 1  |
| Recargos nocturno/festivo                | Implementación Fase 1  |
| Hospedaje, alimentación, viáticos        | Implementación Fase 1  |
| Licencias SaaS del producto              | SaaS                   |
| Licencias herramientas (ej. Claude Code) | SaaS                   |
| Consumo AWS                              | Cloud                  |
| Auditores externos / terceros            | Otros Vendors          |
| Otros gastos no clasificados             | Otros Vendors          |

---

## 📢 ORDEN DE IMPACTO EN GROSS MARGIN (seguir estrictamente)

1. Implementación Fase 1 (personas + gastos) — MAYOR IMPACTO
2. Operación (personas) — si aplica
3. SaaS / Licencias
4. Cloud (AWS)
5. Otros Vendors — MENOR IMPACTO

---

## 📋 ESTRUCTURA DEL INFORME HTML

El informe debe tener exactamente estas 7 secciones:

### Sección 1 — Resumen general
- 4 tarjetas KPI: Venta total CET · Costo total real · Gross Margin real % · Horas acumuladas
- Badges de estado en el encabezado (verde/amarillo/rojo según desviación)

### Sección 2 — Costos reales vs presupuesto CET
- **Gráfica de barras agrupadas**: Presupuesto CET vs Real por línea (Impl / Cloud / SaaS / OV)
- Tabla de desviaciones con variación en COP y porcentaje

### Sección 3 — Personas (nómina + recargos + gastos)
- **Gráfica de líneas acumulado**: horas reales acumuladas vs presupuesto acumulado mes a mes
- **Gráfica donut**: composición del costo Implementación Fase 1 (nómina / recargos / gastos op.)

### Sección 4 — Cloud AWS
- **Gráfica de barras**: consumo AWS mensual real vs presupuesto prorrateado (degradado azul→naranja→rojo según pico)
- **Gráfica de líneas**: AWS acumulado real vs línea techo del presupuesto CET

### Sección 5 — Gross Margin comparativo
- **Gráfica de barras agrupadas**: GM % real vs objetivo CET por línea
- **Gráfica de barras**: GM en COP por línea (mostrar negativo si aplica)

### Sección 6 — Revenue Realization
- **Gráfica donut**: revenue facturado estimado vs pendiente
- 2 tarjetas: revenue pendiente + riesgo cloud en operación

### Sección 7 — Alertas y acciones (PM)
- Caja de alertas roja con bullets: solo causas críticas con impacto en COP
- 1 bloque de acciones para PM únicamente (sin comercial ni preventa)

---

## 🎨 ESPECIFICACIONES TÉCNICAS DEL HTML

Genera una **página HTML completa y autocontenida** (con DOCTYPE, head y body) usando
exactamente esta estructura de wrapper — reemplaza los valores entre [CORCHETES]:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sem [NN] · [DD Mes AAAA] — [NOMBRE_PROYECTO]</title>
  <style>
    :root {
      --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Arial, sans-serif;
      --color-text-primary:         #111827;
      --color-text-secondary:       #4B5563;
      --color-background-primary:   #ffffff;
      --color-background-secondary: #f7f8fa;
      --color-border-primary:       #d1d5db;
      --color-border-tertiary:      #e5e7eb;
      --border-radius-md:           6px;
      --border-radius-lg:           8px;
    }
    .nav-bar {
      background: #1F3864; padding: 10px 20px;
      display: flex; align-items: center; gap: 16px;
    }
    .nav-bar a { color: #9DC3E6; font-size: 12px; text-decoration: none;
                 font-family: var(--font-sans); }
    .nav-bar a:hover { color: #fff; }
    .nav-bar .sep { color: #4B6A9B; font-size: 12px; }
    .nav-bar .current { color: #fff; font-weight: 600; font-size: 12px;
                        font-family: var(--font-sans); }
    .informe-wrap { max-width: 960px; margin: 20px auto; padding: 0 20px 40px; }
  </style>
</head>
<body style="background:#f0f4f8;margin:0;">

<div class="nav-bar">
  <a href="../../../index.html">← PMO Dashboard</a>
  <span class="sep">/</span>
  <a href="../index.html">[CODIGO_PROYECTO] · [NOMBRE_CORTO]</a>
  <span class="sep">/</span>
  <span class="current">Sem [NN] · [DD Mes AAAA]</span>
</div>

<div class="informe-wrap">
  [INSERTAR AQUÍ EL CONTENIDO DEL INFORME]
</div>

</body>
</html>
```

**El contenido del informe** debe seguir estas reglas:
- Usar **Chart.js 4.4.1** desde CDN: `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js`
- Usar las variables CSS del wrapper: `var(--font-sans)`, `var(--color-text-primary)`, etc.
- Layout con CSS Grid (clases `.g2`, `.g3`, `.g4` para columnas)
- Alturas de canvas: `130px` principales · `110px` secundarias
- Colores fijos:

| Estado   | Texto     | Fondo                       |
|----------|-----------|-----------------------------|
| Verde    | `#3B9B5C` | `#EAF3DE`                   |
| Ámbar    | `#EF9F27` | `#FAEEDA`                   |
| Rojo     | `#A32D2D` | `#FCEBEB`                   |
| Azul CET | `#3572BA` | `rgba(53,114,186,0.2)`      |

- **NO incluir** bloque de fuentes al pie ni secciones de Comercial / Preventa

**Nombre del archivo de salida** (indicarlo al inicio de la respuesta): `semana_[NN]_[AAAAMMDD].html`

---

## ⚠️ REGLAS DE PRESENTACIÓN

- Todo en COP — nunca mostrar USD
- Formato colombiano: `$1,277.7M` para millones · `$44,390` para miles
- Badges según estado real: 🟢 GM ≥ objetivo · 🟡 objetivo −3pp · 🔴 por debajo
- Gráficas con valores reales de los archivos, no datos de ejemplo
- Si una línea no existe en el proyecto (ej. no hay OV), omitirla completamente
- Si un archivo no fue adjuntado, indicarlo como supuesto y continuar

---

## 📦 BLOQUE PARA EL DASHBOARD (incluir AL FINAL del HTML, como comentario)

Al terminar el HTML, agrega este bloque con los valores calculados.
La PMO lo usará para actualizar el dashboard sin leer el informe manualmente.

```html
<!--
╔══════════════════════════════════════════════════════════╗
║  DATOS PARA EL DASHBOARD — copiar en index.html y JSON  ║
╚══════════════════════════════════════════════════════════╝

▸ Para proyectos/[CODIGO]/index.html → array SEMANAS[]
  Agrega este objeto al final del array:

  {
    semana:  "Sem [NN] · [DD Mes AAAA]",
    fecha:   "[AAAA-MM-DD]",
    archivo: "semanas/semana_[NN]_[AAAAMMDD].html",
    gm:      [GM_TOTAL_%],
    costo:   [COSTO_TOTAL_M_COP],
    cloud:   [CLOUD_ACUMULADO_M_COP],
    horas:   [HORAS_ACUMULADAS],
    implGM:  [GM_IMPL_%],
    cloudGM: [GM_CLOUD_%],
    saasGM:  [GM_SAAS_%],
    ovGM:    [GM_OV_%],
    estado:  "[ok|wn|er]",
  },

▸ Para _data/proyectos.json → semanas[] del proyecto:

  { "semana": "Sem [NN] · [DD Mes AAAA]", "fecha": "[AAAA-MM-DD]",
    "archivo": "semanas/semana_[NN]_[AAAAMMDD].html",
    "gm": [GM_TOTAL_%], "costo": [COSTO_TOTAL_M_COP],
    "cloud": [CLOUD_ACUMULADO_M_COP], "horas": [HORAS_ACUMULADAS],
    "estado": "[ok|wn|er]" }

  Criterio de estado:
  - "ok" → GM ≥ objetivo CET
  - "wn" → GM entre objetivo −3pp y objetivo
  - "er" → GM < objetivo −3pp
-->
```

---

## 🎯 ENTREGA ESPERADA

1. Nombre del archivo: `semana_[NN]_[AAAAMMDD].html`
2. HTML completo con DOCTYPE, head, body y barra de navegación
3. Todas las gráficas funcionales con datos reales
4. Bloque `DATOS PARA EL DASHBOARD` al final como comentario HTML
5. Listo para guardar en `pmo-dashboards/proyectos/[CODIGO]/semanas/`

---

## 📝 INFORMACIÓN DEL PROYECTO (completar antes de enviar)

```
Nombre del proyecto     : CO - Nueva EPS - Migracion ROSA
Código proyecto (PMO)   : P0411
Código OA / ID interno  : P2941
Nombre corto (topbar)   : POC Nueva EPS - Migracion ROSA
Semana del informe      : Sem 1 · 28 de mayo 2026
Número de semana (NN)   : 01 (dos dígitos, ej. 04)
Fecha del informe       : 20260602 (AAAAMMDD para nombre archivo)
Mes actual del proyecto : M 1 de M 1 totales - acaba de emepezar
Tipo de proyecto        : [X] Implementación Fixed
                          
Incluye servicios SaaS  :  [x] No
Meses restantes         : 1
TRM aplicada            : $4,000 COP/USD (fija — no modificar)
```

---

*Dirección de Proyectos · BLD Engineering · Versión 2.0 — May 2026*
