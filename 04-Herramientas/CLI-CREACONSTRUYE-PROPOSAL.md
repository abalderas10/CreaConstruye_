---
title: Propuesta - CreaConstruye CLI (Node.js)
date: 2025-11-06
tags: [cli, proposal, node.js, termux, automation, herramientas]
status: planning
---

# 🖥️ CreaConstruye CLI - Propuesta de Implementación
## Herramienta de Línea de Comandos para Automatizar Proformas en Termux

---

## 📋 RESUMEN EJECUTIVO

### ¿Qué es CreaConstruye CLI?

Una **herramienta de línea de comandos en Node.js** que permite:
- ✅ Generar proformas completas desde terminal
- ✅ Analizar proyectos inmobiliarios automáticamente
- ✅ Exportar reportes en múltiples formatos (PDF, Excel, JSON)
- ✅ Ejecutarse en **Termux** (terminal Android)
- ✅ Funcionar como **Claude Code pero para proformas**

### Por qué es una buena idea

```
PROBLEMA ACTUAL:
├─ Proforma requiere interfaz web
├─ No se puede usar en móvil/Termux
├─ Requiere servidor backend
└─ Difícil de automatizar scripts

SOLUCIÓN CON CLI:
├─ ✅ Funciona desde terminal
├─ ✅ Ejecutable en Termux (Android)
├─ ✅ Sin dependencias de servidor
├─ ✅ Fácil de scriptear y automatizar
├─ ✅ Ideal para developers
└─ ✅ Democratiza acceso a la herramienta
```

### Diferenciador vs Web App

| Aspecto | Web App | CLI (Propuesta) |
|---------|---------|-----------------|
| **Acceso** | Navegador | Terminal anywhere |
| **Plataforma** | Desktop/Web | Desktop/Mobile/Server |
| **Termux** | ❌ Difícil | ✅ Nativo |
| **Scripting** | ❌ Complejo | ✅ Fácil (stdin/stdout) |
| **Automatización** | ❌ Manual | ✅ CRON jobs |
| **Desarrollo** | 💰 Caro | 💰 Más económico |
| **Experiencia** | 🎨 Bonita | 🚀 Poderosa |

---

## 🎯 VISIÓN Y OBJETIVOS

### Objetivo Principal
Crear una **CLI profesional, modular y extensible** que permita ejecutar análisis de proformas desde terminal, con especial soporte para **Termux**.

### Objetivos Secundarios
1. Ser **standalone** (sin dependencias de servidor)
2. Fácil de **instalar** en cualquier sistema
3. Fácil de **usar** (CLI intuitiva)
4. Fácil de **extender** (plugins/modules)
5. **Rápido** (respuestas < 5 segundos)
6. **Confiable** (testing, manejo de errores)

---

## 🏗️ ARQUITECTURA CONCEPTUAL

### Flujo de la CLI

```
┌─────────────────────────────────────────────────────────┐
│                      USUARIO                             │
│                  (Terminal/Termux)                      │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
        ┌────────────────────────────┐
        │   CreaConstruye CLI        │
        │   (Node.js executable)     │
        └────┬──────────────────┬────┘
             │                  │
             ▼                  ▼
    ┌──────────────────┐  ┌──────────────────┐
    │  Command Router  │  │ Argument Parser  │
    │  (generador)     │  │ (commander.js)   │
    │  (analizar)      │  │                  │
    │  (exportar)      │  │                  │
    └────────┬─────────┘  └──────────────────┘
             │
    ┌────────▼──────────────────────────────┐
    │    8 Módulos de Análisis             │
    ├─────────────────────────────────────┤
    │ 1. Land Analysis   5. Zoning Check  │
    │ 2. Cost Est.       6. ROI Calc.     │
    │ 3. Market Analysis 7. Timeline      │
    │ 4. Financial Proj. 8. Risk Analysis │
    └────────┬──────────────────────────────┘
             │
    ┌────────▼──────────────────────────────┐
    │     Data Processing Layer            │
    │  • JSON validation                   │
    │  • Database queries (SQLite)         │
    │  • API calls (cached)                │
    │  • Calculations                      │
    └────────┬──────────────────────────────┘
             │
    ┌────────▼──────────────────────────────┐
    │      Storage & Export                │
    │  • Local SQLite DB                   │
    │  • File exports (PDF/Excel/JSON)     │
    │  • Stdout (piping-friendly)          │
    └────────┬──────────────────────────────┘
             │
             ▼
        ┌──────────────────┐
        │   OUTPUT         │
        │  • Console table │
        │  • Files (PDF)   │
        │  • JSON stdout   │
        │  • Excel export  │
        └──────────────────┘
```

### Componentes Principales

```yaml
creaconstruye-cli/
├── bin/
│   └── cc (executable)
│
├── src/
│   ├── commands/          # Comandos principales
│   │   ├── generate.ts    # Generar proforma
│   │   ├── analyze.ts     # Analizar proyecto
│   │   └── export.ts      # Exportar reporte
│   │
│   ├── modules/           # 8 herramientas
│   │   ├── land-analysis/
│   │   ├── cost-estimation/
│   │   ├── market-analysis/
│   │   ├── financial-projection/
│   │   ├── zoning-analysis/
│   │   ├── roi-metrics/
│   │   ├── timeline-estimation/
│   │   └── risk-analysis/
│   │
│   ├── services/
│   │   ├── database.ts    # SQLite
│   │   ├── api-client.ts  # External APIs
│   │   ├── cache.ts       # Local cache
│   │   └── export.ts      # PDF/Excel/JSON
│   │
│   ├── utils/
│   │   ├── validators.ts
│   │   ├── formatters.ts
│   │   ├── calculations.ts
│   │   └── helpers.ts
│   │
│   └── types/
│       └── index.ts
│
├── config/
│   ├── default.json       # Default config
│   ├── termux.json        # Termux-specific
│   └── README.md
│
├── data/
│   ├── seed/              # Initial data
│   ├── cache/             # API cache
│   └── projects/          # Local projects
│
├── templates/
│   ├── pdf/               # PDF templates
│   ├── excel/             # Excel templates
│   └── json/              # JSON schemas
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── docs/
│   ├── README.md
│   ├── INSTALLATION.md
│   ├── USAGE.md
│   ├── TERMUX.md
│   └── API.md
│
├── package.json
├── tsconfig.json
└── .gitignore
```

---

## 💻 ESPECIFICACIÓN DE COMANDOS

### Comando 1: `cc generate`
Generar proforma completa

```bash
# Uso básico
cc generate --name "Residencial Verde" --location "Naucalpan" --units 40

# Con archivo de configuración
cc generate --config project.json

# Modo interactivo
cc generate --interactive

# Con salida JSON para piping
cc generate --name "Proyecto X" --format json > proforma.json
```

**Inputs:**
```typescript
interface GenerateOptions {
  name: string;
  location: string;
  municipality: string;
  latitude: number;
  longitude: number;
  projectType: 'residential' | 'commercial' | 'mixed';
  units?: number;
  surfaceArea?: number;
  budget?: number;
  config?: string;          // Archivo de config
  interactive?: boolean;    // Modo interactivo
  format?: 'table' | 'json' | 'all';  // Output format
  output?: string;          // Save to file
}
```

**Output:**
```json
{
  "projectId": "uuid-123",
  "name": "Residencial Verde",
  "location": "Naucalpan",
  "analysis": {
    "landAnalysis": { ... },
    "costEstimation": { ... },
    "marketAnalysis": { ... },
    "financialProjection": { ... },
    "zoningAnalysis": { ... },
    "roiMetrics": { ... },
    "timeline": { ... },
    "riskAnalysis": { ... }
  },
  "executiveSummary": { ... },
  "recommendation": "PROCEED",
  "createdAt": "2025-11-06T10:00:00Z"
}
```

### Comando 2: `cc analyze`
Analizar proyecto existente

```bash
# Analizar proyecto por ID
cc analyze --project-id abc123

# Analizar desde archivo JSON
cc analyze --file project.json

# Análisis específico
cc analyze --project-id abc123 --tool land-analysis
cc analyze --project-id abc123 --tool financial-projection

# Con escenarios
cc analyze --project-id abc123 --scenario pessimistic
cc analyze --project-id abc123 --scenario optimistic --scenario base
```

**Opciones:**
- `--project-id`: ID del proyecto a analizar
- `--file`: Archivo JSON con datos del proyecto
- `--tool`: Específico (land-analysis, cost-est., market, financial, zoning, roi, timeline, risk)
- `--scenario`: optimistic / base / pessimistic
- `--format`: table / json / detailed
- `--refresh`: Forzar refresh de datos (no cache)

### Comando 3: `cc export`
Exportar reportes en múltiples formatos

```bash
# Exportar a PDF (completo)
cc export --project-id abc123 --format pdf --output proforma.pdf

# Exportar a Excel (con gráficos)
cc export --project-id abc123 --format excel --output proforma.xlsx

# Exportar a JSON
cc export --project-id abc123 --format json --output proforma.json

# Exportar múltiples formatos
cc export --project-id abc123 --formats pdf,excel,json --output proforma

# Solo ejecutivo (1 página)
cc export --project-id abc123 --format pdf --template executive
```

**Formatos soportados:**
- 📄 **PDF**: Reporte profesional con gráficos
- 📊 **Excel**: Con sheets, gráficos, tablas dinámicas
- 📋 **JSON**: Raw data (fácil de procesar)
- 📝 **Markdown**: Para documentación
- 📊 **CSV**: Para análisis

### Comando 4: `cc list`
Listar proyectos locales

```bash
# Listar todos
cc list

# Filtrar por estado
cc list --status completed
cc list --status in-progress

# Búsqueda
cc list --search "Naucalpan"

# Formato
cc list --format json
cc list --format table
```

### Comando 5: `cc config`
Gestionar configuración

```bash
# Ver config actual
cc config show

# Setear valores
cc config set api-key YOUR_API_KEY
cc config set default-format json
cc config set termux true

# Reset a defaults
cc config reset
```

### Comando 6: `cc update`
Actualizar datos y caché

```bash
# Actualizar todos los datos
cc update --all

# Actualizar caché de mercado
cc update --market-data

# Actualizar precios de construcción
cc update --construction-costs

# Update specific municipality
cc update --municipality naucalpan
```

### Comando 7: `cc batch`
Procesar múltiples proyectos

```bash
# Procesar lote de CSV
cc batch --input projects.csv --output results/

# Con configuración específica
cc batch --input projects.csv --config batch.json

# Mostrar progreso
cc batch --input projects.csv --verbose
```

### Comando 8: `cc validate`
Validar datos de proyecto

```bash
# Validar archivo
cc validate --file project.json

# Validar ID
cc validate --project-id abc123

# Validar y mostrar errores
cc validate --file project.json --verbose
```

---

## 🖥️ INTERFAZ DE USUARIO (TUI)

### 1. Tabla de Resumen (generate)
```
┌─────────────────────────────────────────────────────────────┐
│  CreaConstruye Proforma - Residencial Verde                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📍 UBICACIÓN                                                │
│  ├─ Municipio: Naucalpan, Estado de México                 │
│  ├─ Coordenadas: 25.6866°N, 100.2161°W                     │
│  └─ Score de Ubicación: 8.2/10                             │
│                                                              │
│  💰 FINANCIERO                                               │
│  ├─ Inversión Total: $4.0M                                 │
│  ├─ Ingresos Esperados: $3.9M                              │
│  ├─ Margen: 2.3% ⚠️ (Bajo)                                │
│  ├─ ROI: 11% (Medio)                                       │
│  └─ TIR: 11% (Bajo)                                        │
│                                                              │
│  ⏱️ TIMELINE                                                 │
│  ├─ Duración Total: 12 meses                               │
│  ├─ Inicio: 2026-01-01                                     │
│  ├─ Entrega: 2026-12-31                                    │
│  └─ Ruta Crítica: Permisos → Estructura → Entrega          │
│                                                              │
│  ⚠️ RIESGOS                                                  │
│  ├─ Score General: 5.5/10 (Medio)                          │
│  ├─ Top Risk: Demanda de mercado (Prob: 2, Impact: 3)     │
│  └─ Mitigación: Pre-venta mínima 50%                       │
│                                                              │
│  ✅ RECOMENDACIÓN: PROCEDER CON CONDICIONES                 │
│                                                              │
│  Condiciones críticas:                                      │
│  • Aumentar margen a 5% mínimo                             │
│  • Pre-venta 50% antes de construcción                     │
│  • Monitoreo mensual de KPIs                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 2. Modo Interactivo
```
? Nombre del proyecto: Residencial Verde
? Municipio: Naucalpan
? Tipo de proyecto: (Use arrow keys)
❯ Residencial
  Comercial
  Mixto
  Industrial

? Número de unidades: 40
? Presupuesto total ($): 4000000
? Tipo de análisis:
 ◉ Completo (todas 8 herramientas)
 ○ Rápido (resume top 3)
 ○ Personalizado

⏳ Analizando...
```

### 3. Progreso Bar
```
Generando Proforma: Residencial Verde
│████████████████░░░░░░░░░░░░│ 60%

Herramientas procesadas:
✓ Análisis de Terreno
✓ Estimación de Costos
✓ Análisis de Mercado
⏳ Proyecciones Financieras (40%)
○ Análisis de Zonificación
○ Cálculo de ROI
○ Estimación de Tiempos
○ Análisis de Riesgos

Tiempo restante: ~45 segundos
```

---

## 🔧 STACK TECNOLÓGICO DE LA CLI

### Core
```typescript
// Commander.js para CLI
import { program } from 'commander';

// Chalk para colores
import chalk from 'chalk';

// Table para tablas
import Table from 'cli-table3';

// Ora para spinners
import ora from 'ora';

// Inquirer para prompts interactivos
import inquirer from 'inquirer';

// PDFKit para generar PDFs
import PDFDocument from 'pdfkit';

// ExcelJS para generar Excel
import ExcelJS from 'exceljs';

// SQLite3 para base de datos local
import sqlite3 from 'sqlite3';

// Axios para API calls
import axios from 'axios';

// Zod para validación
import { z } from 'zod';
```

### Desarrollo
```yaml
Language: TypeScript (compilado a JS)
Runtime: Node.js 18+
Package Manager: npm o pnpm
Testing: Jest + Supertest
Linting: ESLint
Formatting: Prettier
Build: esbuild
```

### Distribución
```yaml
NPM Package: @creaconstruye/cli
Executable: cc (symlink a node script)
Installation: npm install -g @creaconstruye/cli
Uninstall: npm uninstall -g @creaconstruye/cli
```

---

## 📱 SOPORTE ESPECIAL PARA TERMUX

### Por qué Termux?

```
Termux = Terminal emulator para Android
├─ Acceso a Node.js nativo
├─ Package manager (pkg install)
├─ Full Linux environment
├─ Puede correr scripts 24/7
└─ Perfecto para developers en móvil
```

### Instalación en Termux

```bash
# 1. Actualizar packages
pkg update && pkg upgrade

# 2. Instalar Node.js
pkg install nodejs

# 3. Instalar CreaConstruye CLI
npm install -g @creaconstruye/cli

# 4. Verificar
cc --version

# 5. Configurar para Termux
cc config set termux true
```

### Configuración Termux-Specific

```json
{
  "termux": {
    "enabled": true,
    "dataPath": "$HOME/storage/documents/creaconstruye",
    "exportPath": "$HOME/storage/downloads/proformas",
    "fontSize": 12,
    "theme": "dark",
    "updateCheck": false,
    "largeOutput": "file"  // Guardar en archivo si output > 10MB
  }
}
```

### Casos de Uso en Termux

```bash
# 1. Generar proforma y guardar en Downloads
cc generate --name "Proyecto X" --location "Naucalpan" \
  --output ~/storage/downloads/proyecto-x.pdf

# 2. Script automático (cron)
# Crear archivo: ~/bin/daily-analysis.sh
#!/bin/bash
cc generate --config ~/projects/default.json \
  --format excel \
  --output ~/storage/downloads/$(date +%Y-%m-%d).xlsx

# 3. Procesar lote de proyectos
cc batch --input ~/storage/documents/projects.csv \
  --format pdf \
  --output ~/storage/downloads/results/

# 4. Análisis rápido desde clipboard
cc generate --file ~/temp/project.json --format table

# 5. Monitorear en tiempo real
watch -n 300 'cc list --status in-progress'
```

---

## 🚀 PLAN DE IMPLEMENTACIÓN

### Fase 1: Setup Inicial (Semana 1)
- [ ] Crear repo GitHub
- [ ] Setup TypeScript + Node.js
- [ ] Setup testing framework (Jest)
- [ ] Crear estructura de carpetas
- [ ] Hello world CLI

### Fase 2: Core Functionality (Semana 2-3)
- [ ] Implementar command router (Commander.js)
- [ ] Crear modelos de datos (Zod)
- [ ] Implementar `cc generate`
- [ ] Implementar `cc analyze`
- [ ] Implementar `cc export` (JSON)

### Fase 3: UI & UX (Semana 4)
- [ ] Tablas bonitas (cli-table3)
- [ ] Colores con Chalk
- [ ] Progress bars (Ora)
- [ ] Modo interactivo (Inquirer)
- [ ] Help menus

### Fase 4: Exportación Avanzada (Semana 5)
- [ ] PDF generation (PDFKit)
- [ ] Excel export (ExcelJS)
- [ ] Markdown export
- [ ] CSV export

### Fase 5: Base de Datos (Semana 6)
- [ ] Setup SQLite
- [ ] Schemas de proyectos
- [ ] CRUD operations
- [ ] Queries optimizadas

### Fase 6: APIs & Cache (Semana 7)
- [ ] Integración con APIs externas
- [ ] Local caching (Redis o SQLite)
- [ ] API retry logic
- [ ] Error handling

### Fase 7: Termux & Distribution (Semana 8)
- [ ] Termux-specific config
- [ ] NPM package publishing
- [ ] Installation guide
- [ ] Termux documentation

### Fase 8: Testing & Polish (Semana 9)
- [ ] Unit tests (80%+ coverage)
- [ ] Integration tests
- [ ] E2E tests
- [ ] Bug fixes
- [ ] Performance optimization

### Fase 9: Documentation & Launch (Semana 10)
- [ ] README completo
- [ ] Installation guide
- [ ] Usage guide
- [ ] API documentation
- [ ] Troubleshooting
- [ ] Beta launch

---

## 📊 COMPARACIÓN: CLI vs WEB vs HYBRID

### Opción 1: Solo Web App (Status Quo)
```
Pros:
✅ Interfaz bonita
✅ Visualización avanzada
✅ Fácil para usuarios no-técnicos

Cons:
❌ No funciona en Termux
❌ Requiere servidor
❌ No es scripteable
❌ Caro de mantener
```

### Opción 2: Solo CLI (Propuesta)
```
Pros:
✅ Funciona en Termux
✅ Standalone (sin servidor)
✅ Fácil de scriptear
✅ Pequeño y rápido
✅ Para developers

Cons:
❌ No tan visual
❌ Requiere conocimiento de terminal
❌ Menos amigable para no-técnicos
```

### Opción 3: Hybrid (Recomendado)
```
Pros:
✅ AMBOS: Web + CLI
✅ Web para usuarios normales
✅ CLI para developers/Termux
✅ APIs compartidas (reutilización)
✅ Mejor cobertura de mercado

Cons:
⚠️ Más trabajo inicial
⚠️ Más mantenimiento
```

**RECOMENDACIÓN: HYBRID**

---

## 💰 ANÁLISIS DE ROI

### Costo de Desarrollo

```yaml
Costo Base:
├─ 1 Full-stack Dev × 10 semanas @ $80/h
│  └─ 400 horas × $80 = $32,000
│
├─ Testing & QA (15% extra)
│  └─ $4,800
│
└─ Documentación & Setup (10% extra)
   └─ $3,200

TOTAL: ~$40,000 (MVP)
```

### Beneficios

```yaml
Monetarios:
├─ Nuevos usuarios (no pueden usar web)
│  └─ +30% usuarios potenciales
│
├─ Automación (venden servicios de CLI)
│  └─ Ingresos por integración
│
└─ Premium features
   └─ Usuarios de pago en CLI

No Monetarios:
├─ Developer satisfaction
├─ Market differentiation
├─ Community engagement
└─ Future expansion (API first)
```

---

## 🎯 VENTAJAS CLAVE

### 1. Accesibilidad
```
✅ Funciona en Termux (Android)
✅ Funciona en Mac/Linux/Windows
✅ Funciona en servers (headless)
✅ Bajo requirement de recursos
```

### 2. Developer Experience
```
✅ Fácil de instalar (npm install -g)
✅ Intuitivo (similar a git, npm, docker)
✅ Bien documentado
✅ Extensible (plugins)
```

### 3. Automatización
```
✅ Scripteable (bash, python, cron)
✅ Piping-friendly (stdout/stdin)
✅ Batch processing (CSV input)
✅ CI/CD integration
```

### 4. Performance
```
✅ Rápido (respuestas < 5s)
✅ Bajo consumo de recursos
✅ Offline-capable (local data)
✅ Escalable (distribución)
```

### 5. Monetización
```
✅ Free tier: CLI básica
✅ Pro tier: Features avanzadas
✅ Enterprise: Soporte + custom
✅ B2B: Integración con sistemas
```

---

## 📝 ROADMAP POST-MVP

### V1.1 (Mes 2)
- [ ] Plugin system
- [ ] Template customization
- [ ] Advanced filters

### V1.2 (Mes 3)
- [ ] Cloud sync (opcional)
- [ ] Collaborative features
- [ ] API server mode

### V2.0 (Mes 4-5)
- [ ] Desktop app (Electron)
- [ ] Web interface (Next.js)
- [ ] Mobile app (React Native)
- [ ] REST API

---

## ✅ CONCLUSIÓN

### ¿Vale la pena hacer CreaConstruye CLI?

**SÍ, definitivamente:**

1. **Diferenciador tecnológico** - Nadie más tiene CLI para proformas
2. **Accesibilidad** - Termux + developers = mercado sin explotar
3. **Comunidad** - Desarrolladores aman las CLIs
4. **Escalabilidad** - Puede crecer a web/desktop después
5. **Bajo costo** - ~$40k para MVP
6. **Alto ROI** - Usuarios nuevos + ingresos adicionales

### Próximos Pasos

1. ✅ Validación con usuarios (¿Te gusta la idea?)
2. 📋 Crear backlog detallado
3. 🏗️ Iniciar Fase 1 de implementación
4. 📅 Estimado: 2-3 meses para MVP

---

**¿Te parece bien? ¿Empezamos con el blueprint de desarrollo?**

*Documento creado: 2025-11-06*
