---
title: Blueprint de Desarrollo - CreaConstruye CLI
date: 2025-11-06
tags: [cli, blueprint, desarrollo, arquitectura, node.js]
status: technical-specification
---

# 🛠️ Blueprint de Desarrollo - CreaConstruye CLI
## Arquitectura Técnica Detallada para Implementación

---

## 1. ARQUITECTURA GENERAL

### 1.1 Capas de Arquitectura

```
┌──────────────────────────────────────────────────┐
│         USER INTERFACE LAYER (TUI)               │
│  • Commander.js (routing)                        │
│  • Chalk (colors)                                │
│  • Table (tablas)                                │
│  • Ora (progress)                                │
│  • Inquirer (prompts)                            │
└────────────────┬─────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│       COMMAND LAYER                              │
│  • Generate command                              │
│  • Analyze command                               │
│  • Export command                                │
│  • List/Config/Update/etc                        │
└────────────────┬─────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│       BUSINESS LOGIC LAYER                       │
│  • 8 Analysis modules                            │
│  • Calculations                                  │
│  • Validation (Zod)                              │
│  • Orchestration                                 │
└────────────────┬─────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│       DATA LAYER                                 │
│  • SQLite database                               │
│  • Local cache                                   │
│  • File system                                   │
│  • External APIs (axios)                         │
└────────────────┬─────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│       OUTPUT LAYER                               │
│  • PDF export (PDFKit)                           │
│  • Excel export (ExcelJS)                        │
│  • JSON output                                   │
│  • Console table                                 │
└──────────────────────────────────────────────────┘
```

### 1.2 Flujo de Datos

```typescript
// User executes command
$ cc generate --name "Proyecto X" --location "Naucalpan"
                     ↓
        [Command Parser - Commander.js]
                     ↓
        [Argument Validation - Zod]
                     ↓
        [Generate Command Handler]
                     ↓
        [Orchestration Service]
                     ↓
    [Load/Create Project] → [Database]
                     ↓
        [Run 8 Analysis Modules in parallel]
          ├─ Land Analysis
          ├─ Cost Estimation
          ├─ Market Analysis
          ├─ Financial Projection
          ├─ Zoning Analysis
          ├─ ROI Metrics
          ├─ Timeline
          └─ Risk Analysis
                     ↓
        [Aggregate Results]
                     ↓
        [Generate Executive Summary]
                     ↓
        [Format Output - Table/JSON/File]
                     ↓
        [Display to Console]
```

---

## 2. ESTRUCTURA DE CARPETAS COMPLETA

```
creaconstruye-cli/
│
├── bin/
│   ├── index.js                      # Entry point
│   └── cc                            # Symlink executable
│
├── src/
│   │
│   ├── commands/
│   │   ├── index.ts                  # Command registry
│   │   ├── generate.command.ts       # 🚀 Main command
│   │   ├── analyze.command.ts
│   │   ├── export.command.ts
│   │   ├── list.command.ts
│   │   ├── config.command.ts
│   │   ├── update.command.ts
│   │   ├── batch.command.ts
│   │   └── validate.command.ts
│   │
│   ├── modules/                      # 8 Analysis tools
│   │   ├── index.ts
│   │   │
│   │   ├── land-analysis/
│   │   │   ├── index.ts
│   │   │   ├── analyzer.ts
│   │   │   ├── validator.ts
│   │   │   ├── schemas.ts
│   │   │   └── calculator.ts
│   │   │
│   │   ├── cost-estimation/
│   │   │   ├── index.ts
│   │   │   ├── analyzer.ts
│   │   │   ├── bimsa-data.ts
│   │   │   └── inflation-model.ts
│   │   │
│   │   ├── market-analysis/
│   │   │   ├── index.ts
│   │   │   ├── scraper.ts
│   │   │   ├── analyzer.ts
│   │   │   └── forecaster.ts
│   │   │
│   │   ├── financial-projection/
│   │   │   ├── index.ts
│   │   │   ├── dcf-model.ts
│   │   │   ├── monte-carlo.ts
│   │   │   └── scenario-analyzer.ts
│   │   │
│   │   ├── zoning-analysis/
│   │   │   ├── index.ts
│   │   │   ├── analyzer.ts
│   │   │   ├── permit-calculator.ts
│   │   │   └── regulations-db.ts
│   │   │
│   │   ├── roi-metrics/
│   │   │   ├── index.ts
│   │   │   ├── calculator.ts
│   │   │   ├── formulas.ts
│   │   │   └── benchmarks.ts
│   │   │
│   │   ├── timeline-estimation/
│   │   │   ├── index.ts
│   │   │   ├── cpm-analyzer.ts
│   │   │   ├── pert-calculator.ts
│   │   │   └── activity-db.ts
│   │   │
│   │   └── risk-analysis/
│   │       ├── index.ts
│   │       ├── identifier.ts
│   │       ├── matrix-calculator.ts
│   │       └── mitigation.ts
│   │
│   ├── services/
│   │   ├── index.ts
│   │   │
│   │   ├── database/
│   │   │   ├── index.ts
│   │   │   ├── connection.ts
│   │   │   ├── migrations.ts
│   │   │   ├── queries.ts
│   │   │   └── schemas.ts
│   │   │
│   │   ├── cache/
│   │   │   ├── index.ts
│   │   │   ├── local-cache.ts        # SQLite-based cache
│   │   │   └── cache-manager.ts
│   │   │
│   │   ├── api-client/
│   │   │   ├── index.ts
│   │   │   ├── http-client.ts        # Axios wrapper
│   │   │   ├── retry-logic.ts
│   │   │   ├── external-apis.ts      # Google Maps, etc
│   │   │   └── rate-limiter.ts
│   │   │
│   │   ├── export/
│   │   │   ├── index.ts
│   │   │   ├── pdf-exporter.ts       # PDFKit
│   │   │   ├── excel-exporter.ts     # ExcelJS
│   │   │   ├── json-exporter.ts
│   │   │   ├── csv-exporter.ts
│   │   │   └── markdown-exporter.ts
│   │   │
│   │   ├── formatter/
│   │   │   ├── index.ts
│   │   │   ├── table-formatter.ts
│   │   │   ├── json-formatter.ts
│   │   │   └── text-formatter.ts
│   │   │
│   │   └── config/
│   │       ├── index.ts
│   │       ├── config-manager.ts
│   │       └── defaults.ts
│   │
│   ├── utils/
│   │   ├── index.ts
│   │   ├── validators.ts             # Zod schemas
│   │   ├── calculations.ts
│   │   ├── formatters.ts
│   │   ├── logger.ts
│   │   ├── helpers.ts
│   │   └── constants.ts
│   │
│   ├── types/
│   │   ├── index.ts
│   │   ├── project.types.ts
│   │   ├── analysis.types.ts
│   │   ├── command.types.ts
│   │   └── common.types.ts
│   │
│   ├── orchestration/
│   │   ├── index.ts
│   │   ├── orchestrator.ts           # Coordina análisis
│   │   └── pipeline.ts
│   │
│   └── index.ts                      # Main export
│
├── config/
│   ├── default.json                  # Default config
│   ├── termux.json                   # Termux-specific
│   └── README.md
│
├── data/
│   ├── seed/
│   │   ├── municipalities.json
│   │   ├── regulations.json
│   │   └── construction-costs.json
│   │
│   ├── templates/
│   │   ├── pdf/
│   │   │   └── proforma-template.html
│   │   │
│   │   └── excel/
│   │       └── proforma-template.xlsx
│   │
│   └── cache/
│       └── .gitkeep
│
├── tests/
│   ├── unit/
│   │   ├── modules/
│   │   │   ├── land-analysis.test.ts
│   │   │   ├── cost-estimation.test.ts
│   │   │   ├── market-analysis.test.ts
│   │   │   ├── financial-projection.test.ts
│   │   │   ├── zoning-analysis.test.ts
│   │   │   ├── roi-metrics.test.ts
│   │   │   ├── timeline-estimation.test.ts
│   │   │   └── risk-analysis.test.ts
│   │   │
│   │   ├── utils/
│   │   │   ├── validators.test.ts
│   │   │   ├── calculations.test.ts
│   │   │   └── formatters.test.ts
│   │   │
│   │   └── services/
│   │       ├── database.test.ts
│   │       └── exporters.test.ts
│   │
│   ├── integration/
│   │   ├── commands.test.ts
│   │   ├── orchestration.test.ts
│   │   └── end-to-end.test.ts
│   │
│   ├── fixtures/
│   │   ├── project.fixture.ts
│   │   ├── market-data.fixture.ts
│   │   └── mock-apis.ts
│   │
│   └── setup.ts                      # Test setup
│
├── docs/
│   ├── README.md
│   ├── INSTALLATION.md
│   │   ├── macOS
│   │   ├── Linux
│   │   ├── Windows
│   │   └── Termux
│   │
│   ├── USAGE.md
│   │   ├── Getting started
│   │   ├── Commands reference
│   │   ├── Configuration
│   │   └── Examples
│   │
│   ├── TERMUX.md
│   │   ├── Installation steps
│   │   ├── Troubleshooting
│   │   └── Use cases
│   │
│   ├── DEVELOPMENT.md
│   │   ├── Setup dev environment
│   │   ├── Architecture
│   │   ├── How to add module
│   │   └── Testing
│   │
│   └── API.md
│       ├── Exported functions
│       └── Module interfaces
│
├── scripts/
│   ├── build.sh
│   ├── test.sh
│   ├── release.sh
│   └── termux-install.sh
│
├── .github/
│   └── workflows/
│       ├── test.yml
│       ├── build.yml
│       └── publish.yml
│
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
├── jest.config.js
├── .eslintrc.json
├── .prettierrc
├── Makefile
└── README.md
```

---

## 3. ESPECIFICACIONES DE MÓDULOS

### 3.1 Estructura de un Módulo

```typescript
// modules/land-analysis/index.ts

import { LandAnalysisInput, LandAnalysisOutput } from '../../types';
import { analyzer } from './analyzer';
import { validator } from './validator';

export class LandAnalysisModule {
  private cache: Map<string, LandAnalysisOutput> = new Map();

  async analyze(input: LandAnalysisInput): Promise<LandAnalysisOutput> {
    // 1. Validar input
    const validated = await validator.validate(input);

    // 2. Verificar cache
    const cacheKey = this.generateCacheKey(validated);
    if (this.cache.has(cacheKey)) {
      return this.cache.get(cacheKey)!;
    }

    // 3. Correr análisis
    const result = await analyzer.run(validated);

    // 4. Guardar en cache
    this.cache.set(cacheKey, result);

    return result;
  }

  private generateCacheKey(input: any): string {
    return `land:${input.latitude}:${input.longitude}`;
  }
}

export const landAnalysisModule = new LandAnalysisModule();
```

### 3.2 Interfaz de Módulo

```typescript
// types/command.types.ts

export interface IAnalysisModule {
  name: string;
  version: string;

  analyze(input: any): Promise<any>;
  validate(input: any): Promise<void>;

  getCacheKey?(input: any): string;
  getDescription?(): string;
}

export interface ModuleRegistry {
  register(name: string, module: IAnalysisModule): void;
  get(name: string): IAnalysisModule;
  list(): string[];
}
```

---

## 4. SPECIFICACIÓN DE COMANDOS DETALLADA

### Comando: Generate

```typescript
// commands/generate.command.ts

import { Command } from 'commander';
import { orchestrator } from '../orchestration';
import { exportService } from '../services/export';

export function registerGenerateCommand(program: Command) {
  program
    .command('generate')
    .alias('gen')
    .description('Generate a complete proforma for a project')

    .option('-n, --name <string>', 'Project name (required)')
    .option('-l, --location <string>', 'Location address')
    .option('-m, --municipality <string>', 'Municipality')
    .option('--lat <number>', 'Latitude')
    .option('--lng <number>', 'Longitude')
    .option('-t, --type <type>', 'Project type: residential, commercial, mixed, industrial')
    .option('-u, --units <number>', 'Number of units')
    .option('-s, --surface <number>', 'Surface area in m²')
    .option('-b, --budget <number>', 'Total budget in $')
    .option('--config <file>', 'Configuration file path')
    .option('-i, --interactive', 'Interactive mode')
    .option('-f, --format <format>', 'Output format: table, json, all (default: table)')
    .option('-o, --output <file>', 'Output file path')
    .option('--no-analysis', 'Skip analysis (quick mode)')
    .option('--verbose', 'Verbose output')

    .action(async (options) => {
      try {
        // 1. Validar inputs
        const projectInput = validateGenerateInput(options);

        // 2. Mostrar spinner
        const spinner = ora('Generating proforma...').start();

        // 3. Correr orchestración
        const result = await orchestrator.generate(projectInput, {
          verbose: options.verbose,
          skipAnalysis: options.noAnalysis,
        });

        spinner.succeed('Proforma generated successfully');

        // 4. Formatear output
        const formatted = formatOutput(result, options.format);

        // 5. Guardar si se especifica output
        if (options.output) {
          await exportService.export(result, options.output, options.format);
          console.log(`✅ Saved to: ${options.output}`);
        } else {
          console.log(formatted);
        }

      } catch (error) {
        throw new CLIError('Failed to generate proforma', error);
      }
    });
}

function validateGenerateInput(options: any) {
  return generateInputSchema.parse(options);
}

function formatOutput(result: any, format: string): string {
  if (format === 'json') {
    return JSON.stringify(result, null, 2);
  } else if (format === 'table') {
    return formatAsTable(result);
  } else {
    return formatAll(result);
  }
}
```

---

## 5. SERVICIO DE ORQUESTACIÓN

```typescript
// orchestration/orchestrator.ts

export class Orchestrator {

  async generate(
    input: ProjectInput,
    options: OrchestrateOptions
  ): Promise<ProformaOutput> {

    // 1. Crear/Cargar proyecto
    const project = await this.projectService.create(input);

    // 2. Correr análisis en paralelo
    const analysisResults = await Promise.all([
      this.modules.landAnalysis.analyze(project),
      this.modules.costEstimation.analyze(project),
      this.modules.marketAnalysis.analyze(project),
      this.modules.financialProjection.analyze(project),
      this.modules.zoningAnalysis.analyze(project),
      this.modules.roiMetrics.analyze(project),
      this.modules.timelineEstimation.analyze(project),
      this.modules.riskAnalysis.analyze(project),
    ]);

    // 3. Guardar resultados
    await this.projectService.saveAnalysis(project.id, analysisResults);

    // 4. Generar resumen ejecutivo
    const executiveSummary = this.generateSummary(analysisResults);

    // 5. Hacer recomendación
    const recommendation = this.makeRecommendation(analysisResults);

    // 6. Compilar output
    return {
      projectId: project.id,
      projectName: project.name,
      analyses: analysisResults,
      executiveSummary,
      recommendation,
      generatedAt: new Date().toISOString(),
    };
  }

  private generateSummary(analyses: any[]): ExecutiveSummary {
    // Lógica para generar resumen
    return {
      investment: analyses[3].totalInvestment,
      roi: analyses[5].roi,
      timeline: analyses[6].totalDuration,
      viability: this.calculateViability(analyses),
    };
  }

  private makeRecommendation(analyses: any[]): Recommendation {
    const viabilityScore = this.calculateViability(analyses);
    const riskScore = analyses[7].totalRiskScore;

    if (viabilityScore > 70 && riskScore < 5) {
      return {
        decision: 'PROCEED',
        confidence: viabilityScore,
        conditions: [],
      };
    } else if (viabilityScore > 50 && riskScore < 7) {
      return {
        decision: 'PROCEED_WITH_CONDITIONS',
        confidence: viabilityScore,
        conditions: this.identifyConditions(analyses),
      };
    } else {
      return {
        decision: 'REVIEW_REQUIRED',
        confidence: 0,
        conditions: this.identifyIssues(analyses),
      };
    }
  }
}

export const orchestrator = new Orchestrator();
```

---

## 6. DATABASE SCHEMA

```sql
-- SQLite schema for CLI

-- Projects table
CREATE TABLE projects (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  location TEXT,
  municipality TEXT,
  latitude REAL,
  longitude REAL,
  type TEXT,
  units INTEGER,
  surface_area REAL,
  budget REAL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  deleted_at TIMESTAMP
);

-- Analyses table
CREATE TABLE analyses (
  id TEXT PRIMARY KEY,
  project_id TEXT NOT NULL,
  module_name TEXT NOT NULL,
  analysis_data JSON NOT NULL,
  cached_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (project_id) REFERENCES projects(id)
);

-- Configuration table
CREATE TABLE config (
  key TEXT PRIMARY KEY,
  value TEXT,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Cache table
CREATE TABLE cache (
  key TEXT PRIMARY KEY,
  value TEXT NOT NULL,
  expires_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create indexes
CREATE INDEX idx_projects_municipality ON projects(municipality);
CREATE INDEX idx_analyses_project_id ON analyses(project_id);
CREATE INDEX idx_cache_expires_at ON cache(expires_at);
```

---

## 7. PACKAGE.JSON COMPLETO

```json
{
  "name": "@creaconstruye/cli",
  "version": "1.0.0",
  "description": "CLI tool for generating real estate proformas - works in Termux",
  "main": "dist/index.js",
  "bin": {
    "cc": "dist/bin/index.js"
  },
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "build:watch": "tsc --watch",
    "start": "node dist/bin/index.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "lint": "eslint src --ext .ts",
    "lint:fix": "eslint src --ext .ts --fix",
    "format": "prettier --write \"src/**/*.ts\"",
    "clean": "rm -rf dist",
    "prepublish": "npm run build && npm run test",
    "release": "semantic-release"
  },
  "dependencies": {
    "commander": "^11.0.0",
    "chalk": "^5.3.0",
    "ora": "^7.0.1",
    "inquirer": "^8.2.5",
    "cli-table3": "^0.6.3",
    "axios": "^1.6.0",
    "sqlite3": "^5.1.6",
    "pdfkit": "^0.13.0",
    "exceljs": "^4.3.0",
    "zod": "^3.22.0",
    "dotenv": "^16.3.1",
    "lodash": "^4.17.21",
    "uuid": "^9.0.1",
    "dayjs": "^1.11.10",
    "node-cache": "^5.1.2"
  },
  "devDependencies": {
    "typescript": "^5.2.0",
    "@types/node": "^20.8.0",
    "@types/jest": "^29.5.5",
    "ts-node": "^10.9.1",
    "jest": "^29.7.0",
    "ts-jest": "^29.1.1",
    "eslint": "^8.50.0",
    "@typescript-eslint/eslint-plugin": "^6.7.0",
    "@typescript-eslint/parser": "^6.7.0",
    "prettier": "^3.0.3",
    "husky": "^8.0.3",
    "lint-staged": "^15.0.2"
  },
  "engines": {
    "node": ">=18.0.0"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/creaconstruye/cli.git"
  },
  "keywords": [
    "cli",
    "proforma",
    "real-estate",
    "analysis",
    "termux"
  ],
  "author": "CreaConstruye Team",
  "license": "MIT"
}
```

---

## 8. CONFIGURACIÓN TYPESCRIPT

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "moduleResolution": "node"
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

---

## 9. EJEMPLO DE EJECUCIÓN

### Escenario: Generar proforma en Termux

```bash
# 1. Install
pkg install nodejs
npm install -g @creaconstruye/cli

# 2. First run (interactive)
cc generate --interactive

? Nombre del proyecto: Residencial Verde Naucalpan
? Municipio: Naucalpan
? Tipo de proyecto: Residencial
? Número de unidades: 40
? Presupuesto total ($): 4000000

⏳ Analyzing...
│████████████████░░░░░░░░░░░░│ 60%

✅ Proforma generated successfully!

┌─────────────────────────────────────────────────────────┐
│  Residencial Verde Naucalpan                            │
├─────────────────────────────────────────────────────────┤
│  Investment:       $4,000,000                          │
│  Expected Return:  $3,900,000                          │
│  ROI:              11%                                 │
│  Timeline:         12 months                           │
│  Risk Level:       Medium                              │
│  Recommendation:   PROCEED WITH CONDITIONS             │
└─────────────────────────────────────────────────────────┘

Save to PDF? (Y/n): Y
✅ Saved to: /storage/downloads/proforma.pdf

# 3. Export from existing project
cc export --project-id abc123 --format excel --output results.xlsx
✅ Saved to: /storage/downloads/results.xlsx

# 4. Batch processing
cc batch --input projects.csv --format pdf --output results/
Processing: 100%
✅ Processed 25 projects in 5 minutes
```

---

## 10. ROADMAP DE DESARROLLO

### Semana 1: Setup
- [ ] Repo + TypeScript setup
- [ ] Commander.js hello world
- [ ] Project structure

### Semana 2-3: Core Logic
- [ ] Implement 8 modules
- [ ] Database schema
- [ ] Orchestrator

### Semana 4: Commands
- [ ] Generate command
- [ ] Analyze command
- [ ] List/Config commands

### Semana 5: Export
- [ ] PDF export
- [ ] Excel export
- [ ] JSON/CSV

### Semana 6: UI/UX
- [ ] Tablas bonitas
- [ ] Colores/Spinners
- [ ] Prompts interactivos

### Semana 7: Termux
- [ ] Termux testing
- [ ] Config específico
- [ ] Documentation

### Semana 8: Testing
- [ ] Unit tests
- [ ] Integration tests
- [ ] E2E tests

### Semana 9: Polish
- [ ] Bug fixes
- [ ] Performance
- [ ] Docs

### Semana 10: Launch
- [ ] NPM publish
- [ ] Beta testers
- [ ] Marketing

---

## ✅ CONCLUSIÓN

Este blueprint proporciona:
- ✅ Arquitectura clara y modular
- ✅ Estructura de carpetas organizada
- ✅ Especificaciones técnicas detalladas
- ✅ Ejemplos de código real
- ✅ Plan de implementación paso a paso
- ✅ Timeline realista
- ✅ Testing strategy
- ✅ Soporte para Termux

**¡Listo para empezar a codificar!**

*Documento creado: 2025-11-06*
