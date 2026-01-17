---
title: Arquitectura Flexible del Sistema CreaConstruye
tags: [arquitectura, tecnologia, backend, frontend, integracion]
status: especificacion-tecnica
version: 1.0
---

# 🏗️ Arquitectura Flexible del Sistema CreaConstruye

**Objetivo:** Documentar una arquitectura que sea flexible, escalable y agnóstica a tecnologías específicas, priorizando resultados sobre dogmas tecnológicos.

---

## 1. Principios Arquitectónicos

### 1.1 Flexibilidad Over Dogmatism

```
❌ NO HACEMOS:
   "Usaremos React PORQUE es React"
   "Backend DEBE ser Python"
   "Base de datos TIENE QUE ser PostgreSQL"

✅ HACEMOS:
   "¿Cuál herramienta resuelve MEJOR el problema?"
   "¿Cuál tiene mejor comunidad/soporte?"
   "¿Cuál escala mejor para nuestro caso?"
   "¿Cuál es más costo-efectiva?"
```

### 1.2 Modularidad Extrema

Cada herramienta funciona:
- De forma independiente
- Con sus propias dependencias
- Interfaz agnóstica (APIs REST/GraphQL)
- Escalable por separado

### 1.3 Resultados Primero

La métrica: ¿Los usuarios consiguen proformas precisas rápido?

No importa si:
- Frontend es React, Vue, Angular, Svelte
- Backend es Node, Python, Go, Java
- Base de datos es PostgreSQL, MongoDB, Firestore

---

## 2. Arquitectura General

```
┌─────────────────────────────────────────────────────────────┐
│                      USUARIO FINAL                           │
│              (Web, Mobile, Desktop)                          │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                     FRONTEND LAYER                           │
│        Next.js / React / Vue / Flutter / Native             │
│  [UI responsive, dashboards, manejo de estado]              │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  API GATEWAY / ROUTER                        │
│     Kong / AWS API Gateway / Custom Middleware               │
│     [Autenticación, Rate limiting, Routing]                 │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│              MICROSERVICIOS (8 Herramientas)                 │
├─────────────────────────────────────────────────────────────┤
│ 01-Terrenos      │ Node.js + Express + MongoDB              │
│ 02-Costos        │ Python + FastAPI + PostgreSQL            │
│ 03-Mercado       │ Go + Gin + MySQL                         │
│ 04-Financiero    │ Python + FastAPI + PostgreSQL            │
│ 05-Zonificación  │ Node.js + GraphQL                        │
│ 06-ROI           │ Python + Flask + MongoDB                 │
│ 07-Tiempos       │ Node.js + Express                        │
│ 08-Riesgos       │ Python + FastAPI                         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  SERVICIOS COMPARTIDOS                       │
├─────────────────────────────────────────────────────────────┤
│ Autenticación (Auth0/Firebase) │ Caché (Redis)              │
│ Base de datos central (PostgreSQL) │ Message Queue (RabbitMQ) │
│ Storage (S3) │ Monitoring (Datadog) │ Analytics             │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                 INTEGRACIONES EXTERNAS                       │
├─────────────────────────────────────────────────────────────┤
│ APIs Geoespaciales: Google Maps, Mapbox, PostGIS            │
│ APIs Mercado: Zillow, Redfin, Inmuebles24                   │
│ APIs Financieras: Banxico, Yahoo Finance, Bloomberg         │
│ APIs de Construcción: Costos, proveedores, materiales       │
│ Blockchain: Ethereum, Hyperledger Fabric                    │
│ Crowdfunding: Stripe, PayPal, Open Banking                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Componentes Detallados

### 3.1 Frontend (Múltiples Opciones)

#### Opción A: Next.js (Recomendado)

```javascript
// next.js permite:
// ✓ SSR (Server-side rendering)
// ✓ API routes (mismo proyecto)
// ✓ Incremental Static Regeneration
// ✓ Zero-config deployment (Vercel)
// ✓ File-based routing (simple)

// Estructura:
pages/
├── dashboard.tsx
├── projects/
│  ├── [id].tsx
│  └── new.tsx
├── tools/
│  ├── [toolId]/
│  │  └── [projectId].tsx
│  └── results.tsx
└── api/
   ├── auth/
   ├── projects/
   └── tools/

// Stack UI:
├─ Tailwind CSS (estilos)
├─ Shadcn/ui (componentes)
├─ TanStack Query (state management de datos)
├─ Zustand (estado global simple)
└─ Recharts/D3.js (visualización)
```

#### Opción B: React Puro

```javascript
// Para máxima flexibilidad en styling/build
// Usar Vite para build (más rápido que CRA)

vite-app/
├── src/
│  ├── components/
│  ├── pages/
│  ├── hooks/
│  ├── services/
│  └── store/
└── vite.config.ts
```

#### Opción C: Vue/Nuxt

```javascript
// Alternativa ligera a React/Next
// Mejor para equipos pequeños
// Performance similar
```

### 3.2 Backend (Múltiples Opciones)

#### Herramienta 1: Análisis de Terrenos (Node.js)

```javascript
// Stack: Node.js + Express + MongoDB + GraphQL

// Endpoints:
POST   /api/terrenos/analizar
GET    /api/terrenos/:id
PUT    /api/terrenos/:id
DELETE /api/terrenos/:id

// GraphQL mutations:
mutation AnalizarTerreno($ubicacion: String!, $area: Float!) {
  analizarTerreno(input: {
    ubicacion: $ubicacion
    area: $area
  }) {
    id
    puntuacion
    recomendacion
  }
}

// Proceso:
1. Recibe parámetros (ubicación, área)
2. Llama Google Maps API
3. Procesa datos geoespaciales
4. Consulta BD de comparables
5. Calcula score
6. Retorna resultado
```

#### Herramienta 2: Costos (Python + FastAPI)

```python
# Stack: Python + FastAPI + PostgreSQL + SQLAlchemy

# Endpoints:
@app.post("/api/costos/calcular")
async def calcular_costos(proyecto: ProyectoModel) -> CostosResult:
    """
    Calcula costos de construcción usando:
    - BD de precios unitarios
    - Curvas de inflación
    - Índice de actividad de mercado
    """
    costos = await service.calcular(proyecto)
    return costos

# Estructura:
proyectos/
├── routers/
│  └── costos.py
├── models/
│  └── costo.py
├── services/
│  └── costo_service.py
├── db/
│  └── database.py
└── main.py
```

#### Herramienta 3: Análisis de Mercado (Go + Gin)

```go
// Stack: Go + Gin + MySQL + Redis

// Endpoints:
func AnalyzarMercado(c *gin.Context) {
    var req AnalisisRequest
    c.BindJSON(&req)

    // Scraping de portales
    // Análisis de comparables
    // Tendencias de precios

    result := service.Analizar(req)
    c.JSON(200, result)
}

// Razones para Go:
✓ Performance excepcional
✓ Concurrencia (goroutines)
✓ Compilado (no necesita runtime)
✓ Excelente para web scraping
```

#### Herramientas 4-8: Python + FastAPI

```python
# Todas usan FastAPI por:
✓ Type hints
✓ Async/await nativo
✓ Auto-documentación (Swagger)
✓ Validación automática (Pydantic)
✓ Muy rápido (competidor de Go)

# Cada herramienta es micro-servicio
# Pueden escalar independientemente
```

### 3.3 Bases de Datos (Estrategia Mixta)

```
DATOS TRANSACCIONALES
└─ PostgreSQL (ACID, relaciones complejas)
   ├─ Usuarios y roles
   ├─ Proyectos
   ├─ Transacciones financieras
   └─ Auditoría

DATOS ANALÍTICOS / NO ESTRUCTURADOS
└─ MongoDB (flexibilidad, documentos)
   ├─ Resultados de análisis (cambian formato)
   ├─ Histórico de mercado
   ├─ Logs de operaciones
   └─ Datos experimentales

TIME-SERIES DATA
└─ InfluxDB o TimescaleDB (optimizado para series)
   ├─ Precios históricos (por hora)
   ├─ Indicadores económicos
   ├─ IoT data (sensores de construcción)
   └─ Métricas del sistema

BÚSQUEDAS COMPLEJAS
└─ Elasticsearch (búsqueda full-text)
   ├─ Búsqueda de proyectos
   ├─ Análisis de sentimiento
   ├─ Documentos de contratos
   └─ Búsqueda por palabras clave

CACHÉ / SESIONES
└─ Redis
   ├─ Caché de APIs externas
   ├─ Sesiones de usuario
   ├─ Rate limiting
   └─ Pub/Sub para notificaciones
```

### 3.4 Integraciones Externas

```
CAPAS DE INTEGRACIÓN

Nivel 1: Direct API Calls
├─ Google Maps → Ubicaciones
├─ Banxico → Tasas de interés
└─ [APIs simples y confiables]

Nivel 2: Data Providers
├─ Zillow API → Precios de mercado
├─ Inmuebles24 → Listados
└─ [APIs que requieren normalización]

Nivel 3: Custom ETL
├─ Web scraping (BeautifulSoup, Scrapy)
├─ Parsing de documentos (PDFs, Excel)
├─ Transformación de datos
└─ [Datos no-API que necesitan procesar]

Nivel 4: Blockchain / Crypto
├─ Web3.py (Python)
├─ Ethers.js (JavaScript)
├─ Smart contracts en Solidity
└─ [Integración con redes blockchain]
```

---

## 4. Flujos de Datos

### 4.1 Flujo de Análisis Completo

```
Usuario ingresa en UI:
  ├─ Ubicación: Naucalpan
  ├─ Terreno: 5,000 m²
  └─ Presupuesto: $3.5M

        ↓ [Frontend valida]

POST /api/proyectos
  ├─ Crea proyecto en BD
  ├─ Retorna projectId: abc123
  └─ Guarda en estado local

        ↓ [UI muestra progress]

POST /api/proyectos/abc123/analizar
  └─ Body: { tools: [1,2,3,4,5,6,7,8] }

        ↓ [API Gateway recibe]
        ↓ [Enruta a Message Queue]

Message Queue (RabbitMQ/Kafka):
  ├─ { task: "terrenos", projectId: abc123 }
  ├─ { task: "costos", projectId: abc123 }
  ├─ { task: "mercado", projectId: abc123 }
  └─ [8 mensajes en paralelo]

        ↓ [Workers consumen]

Worker 1 (Terrenos - Node.js):
  ├─ Consulta Google Maps
  ├─ Procesa datos geoespaciales
  ├─ Guarda en MongoDB
  ├─ Publica evento: "terrenos_completo"
  └─ Resultado: { score: 8.5, recommendation: "GO" }

Worker 2 (Costos - Python):
  ├─ Consulta BD de precios
  ├─ Aplica curva de inflación
  ├─ Calcula desglose
  ├─ Publica evento: "costos_completo"
  └─ Resultado: { presupuesto: $3.8M, detalle: [...] }

[Otros workers en paralelo...]

        ↓ [UI recibe eventos via WebSocket]

WebSocket subscription:
  ├─ Usuario ve "Terrenos: 100%"
  ├─ Usuario ve "Costos: 100%"
  ├─ Usuario ve "Mercado: 100%"
  └─ ... todas completan

        ↓ [Compilación de resultados]

POST /api/proyectos/abc123/generar-proforma
  ├─ Consulta todos resultados
  ├─ Compila en documento
  ├─ Genera PDF
  ├─ Guarda en S3
  └─ Retorna URL: s3://bucket/abc123-proforma.pdf

        ↓ [UI muestra resultado]

Usuario descarga proforma
├─ 25 páginas
├─ Gráficos interactivos
├─ Análisis de sensibilidad
└─ Recomendación final
```

---

## 5. Tecnologías por Componente

### 5.1 Stack Recomendado (Pero Flexible)

```
┌──────────────────────────────────────────────────┐
│ FRONTEND                                         │
├──────────────────────────────────────────────────┤
│ • Next.js 14 (React meta-framework)              │
│ • Tailwind CSS (utilidad-first CSS)              │
│ • TanStack Query (caching de datos)              │
│ • Zustand (state management)                     │
│ • Recharts (gráficos simples)                    │
│ • D3.js (gráficos avanzados)                     │
│ • Zod (validación)                               │
│ • TypeScript (seguridad de tipos)                │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ API GATEWAY                                      │
├──────────────────────────────────────────────────┤
│ • Kong (API Gateway de código abierto)           │
│ • O: AWS API Gateway (cloud-managed)             │
│ • O: Nginx + custom middleware (DIY)             │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ MICROSERVICIOS                                   │
├──────────────────────────────────────────────────┤
│ Node.js + Express:  Terrenos, Zonificación      │
│ Python + FastAPI:   Costos, Financiero, ROI     │
│ Go + Gin:          Mercado (web scraping)        │
│ Ruby + Rails:      Timeline (si necesitamos)     │
│ Java + Spring:     Riesgos (análisis complejo)   │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ PERSISTENCIA                                     │
├──────────────────────────────────────────────────┤
│ • PostgreSQL 15 (relacional principal)           │
│ • MongoDB 7 (documentos flexibles)               │
│ • Redis 7 (caché y sesiones)                     │
│ • Elasticsearch 8 (búsquedas)                    │
│ • S3 (almacenamiento de archivos)                │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ MESSAGE QUEUE                                    │
├──────────────────────────────────────────────────┤
│ • RabbitMQ (confiable, AMQP)                     │
│ • O: Apache Kafka (distribuido, streaming)       │
│ • O: AWS SQS (si es cloud-only)                  │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ ORQUESTACIÓN                                     │
├──────────────────────────────────────────────────┤
│ • Docker (containerización)                      │
│ • Kubernetes (orquestación a escala)             │
│ • O: Docker Compose (desarrollo)                 │
│ • O: AWS ECS (si es cloud)                       │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ MONITOREO / OBSERVABILIDAD                       │
├──────────────────────────────────────────────────┤
│ • Datadog (APM full-stack)                       │
│ • O: Prometheus + Grafana (open-source)          │
│ • ELK Stack (logs)                               │
│ • Sentry (error tracking)                        │
└──────────────────────────────────────────────────┘
```

---

## 6. Escalabilidad

### 6.1 Horizontal Scaling

```
HERRAMIENTA DE MERCADO (Web scraping pesado)
├─ 1 instancia: Procesa 100 análisis/día
├─ 5 instancias: Procesa 500 análisis/día
├─ 10 instancias: Procesa 1,000 análisis/día
└─ Escala automática: Por número de mensajes en cola

HERRAMIENTA DE COSTOS (Computación intensa)
├─ 1 CPU: Procesa 50 análisis/día
├─ 4 CPU: Procesa 200 análisis/día
└─ Auto-scale: Cuando CPU > 70%

HERRAMIENTA DE TERRENOS (API calls)
├─ Rate limit Google Maps: 50 análisis/seg
├─ Caché en Redis: 80% de repetidas
└─ Escalable hasta N instancias
```

### 6.2 Crecimiento Esperado

```
AÑO 1 (MVP)
├─ 100 usuarios
├─ 10 proyectos/mes
├─ 1 servidor (todo in-one)
└─ Costo: $500/mes

AÑO 2 (Growth)
├─ 1,000 usuarios
├─ 200 proyectos/mes
├─ 3 servidores (separados)
├─ 1 BD principal + caché
└─ Costo: $5,000/mes

AÑO 3 (Scale)
├─ 10,000 usuarios
├─ 2,000 proyectos/mes
├─ 20+ microservicios
├─ Multi-región
├─ Análisis en tiempo real
└─ Costo: $50,000/mes
```

---

## 7. Seguridad

### 7.1 Capas de Seguridad

```
┌─────────────────────────────────────┐
│ CAPA 1: TRANSPORT                   │
├─────────────────────────────────────┤
│ ✓ HTTPS/TLS 1.3                     │
│ ✓ Rate limiting (DDoS protection)   │
│ ✓ WAF (Web Application Firewall)    │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ CAPA 2: AUTENTICACIÓN               │
├─────────────────────────────────────┤
│ ✓ OAuth2 (social login)             │
│ ✓ JWT tokens (stateless)            │
│ ✓ 2FA (two-factor auth)             │
│ ✓ MFA (multi-factor auth)           │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ CAPA 3: AUTORIZACIÓN                │
├─────────────────────────────────────┤
│ ✓ RBAC (role-based access control)  │
│ ✓ ABAC (attribute-based)            │
│ ✓ Políticas granulares              │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ CAPA 4: ENCRIPTACIÓN DE DATOS       │
├─────────────────────────────────────┤
│ ✓ Encripción en tránsito (TLS)      │
│ ✓ Encripción en reposo (AES-256)    │
│ ✓ Hashing de contraseñas (bcrypt)   │
│ ✓ Secrets management (Vault)        │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ CAPA 5: AUDITORÍA                   │
├─────────────────────────────────────┤
│ ✓ Logs de acceso                    │
│ ✓ Logs de cambios                   │
│ ✓ Blockchain para inmutabilidad     │
│ ✓ Alertas en tiempo real            │
└─────────────────────────────────────┘
```

---

## 8. Deployment

### 8.1 Opciones de Hosting

#### Opción 1: On-Premise (Control Total)

```
Hardware:
├─ 2x Load Balancers (Nginx)
├─ 4x Servidores app (Node.js + Python)
├─ 2x BD primarias + backup
├─ 2x Redis/Caché
└─ Storage (NAS)

Costo: $2,000-5,000/mes
Ventaja: Control total, datos locales
Desventaja: Mantenimiento, escalabilidad manual
```

#### Opción 2: Cloud (AWS/GCP/Azure)

```
Infraestructura:
├─ ECS/EKS (contenedores orquestados)
├─ RDS (bases de datos managed)
├─ ElastiCache (Redis managed)
├─ CloudFront (CDN)
├─ S3 (almacenamiento)
└─ Route 53 (DNS)

Costo: $1,000-3,000/mes (initial)
Ventaja: Escala automática, backup automático
Desventaja: Vendor lock-in, costos por uso
```

#### Opción 3: Hybrid (Best of Both)

```
On-Premise:
├─ Datos sensibles en servidor local
├─ Datos públicos en cloud
└─ Sincronización automática

Cloud:
├─ APIs públicas
├─ Frontend CDN
└─ Escalabilidad en demanda
```

### 8.2 CI/CD Pipeline

```
┌─────────────────────────────────────┐
│ Developer pushes to main            │
└─────────────────────────────────────┘
            ↓
┌─────────────────────────────────────┐
│ GitHub Actions                      │
├─────────────────────────────────────┤
│ 1. Lint & Format check              │
│ 2. Unit tests                       │
│ 3. Integration tests                │
│ 4. Build Docker image               │
│ 5. Push to registry                 │
└─────────────────────────────────────┘
            ↓
┌─────────────────────────────────────┐
│ Deploy to Staging                   │
├─────────────────────────────────────┤
│ 1. Pull image                       │
│ 2. Run migrations                   │
│ 3. Deploy containers                │
│ 4. Run smoke tests                  │
└─────────────────────────────────────┘
            ↓
┌─────────────────────────────────────┐
│ Manual Approval                     │
└─────────────────────────────────────┘
            ↓
┌─────────────────────────────────────┐
│ Deploy to Production                │
├─────────────────────────────────────┤
│ 1. Blue-Green deployment            │
│ 2. Health checks                    │
│ 3. Gradual rollout (canary)         │
│ 4. Monitoring                       │
└─────────────────────────────────────┘
```

---

## 9. Evolución del Stack

```
FASE 1 (MVP - Mes 1-3)
├─ Frontend: React + Vite
├─ Backend: Node.js single instance
├─ BD: SQLite o PostgreSQL pequeña
└─ Hosting: 1 servidor

FASE 2 (Grow - Mes 4-9)
├─ Frontend: Next.js
├─ Backend: Microservicios separados
├─ BD: PostgreSQL + MongoDB
├─ Caché: Redis
├─ Hosting: 3-5 servidores

FASE 3 (Scale - Mes 10+)
├─ Frontend: Next.js + PWA
├─ Backend: Kubernetes + 20+ servicios
├─ BD: Múltiples especialidades
├─ Message Queue: RabbitMQ/Kafka
├─ Monitoring: Datadog full-stack
├─ Hosting: Multi-región

FASE 4 (Enterprise - Año 2+)
├─ Frontend: Federated micro-frontends
├─ Backend: Serverless + serverful hybrid
├─ BD: Graph DB para relaciones complejas
├─ Blockchain: Layer 2 para transacciones
├─ IA: ML models entrenados in-house
└─ Hosting: Multi-cloud
```

---

## 10. Decisiones Tecnológicas (Matriz)

```
COMPONENTE              OPCIÓN A         OPCIÓN B         OPCIÓN C        ELECCIÓN
─────────────────────────────────────────────────────────────────────────────────
Frontend               React            Vue              Angular         Next.js
Backend API            Node.js          Python           Go              Mixto
Base datos relacional  PostgreSQL       MySQL            Oracle          PostgreSQL
Base datos NoSQL       MongoDB          CouchDB          DynamoDB        MongoDB
Caché                  Redis            Memcached        Hazelcast       Redis
Message Queue          RabbitMQ         Kafka            SQS             RabbitMQ
Contenedores          Docker           Podman           (none)          Docker
Orquestación          Kubernetes       Docker Swarm     ECS             Kubernetes
Cloud                 AWS              GCP              Azure           Multi-cloud
Monitoreo             Datadog          Prometheus       New Relic       Datadog
```

---

## 11. Roadmap Técnico

### Q1 2025: MVP Core
- [ ] Frontend: Next.js básico
- [ ] Backend: 3 microservicios (Terrenos, Costos, Mercado)
- [ ] BD: PostgreSQL + MongoDB
- [ ] Hosting: 1 servidor on-premise
- [ ] CI/CD: GitHub Actions simple

### Q2 2025: Escalabilidad
- [ ] Backend: 8 microservicios completos
- [ ] Kubernetes: Orquestación
- [ ] Redis: Caché distribuida
- [ ] RabbitMQ: Message queue
- [ ] Monitoring: Prometheus + Grafana

### Q3 2025: Seguridad + Blockchain
- [ ] WAF + Rate limiting
- [ ] Blockchain testnet
- [ ] Smart contracts
- [ ] Auditoría de seguridad externa

### Q4 2025: Global Scale
- [ ] Multi-región deployment
- [ ] CDN global
- [ ] IA models entrenados
- [ ] API marketplace abierto

---

## 12. Dependencias y Versiones

```
LOCK VERSIONS (Mantener compatibilidad)

Frontend:
├─ Node.js 20 LTS
├─ Next.js 14
├─ React 18
└─ TypeScript 5.3

Backend:
├─ Python 3.11 (FastAPI services)
├─ Node.js 20 LTS (Express services)
├─ Go 1.21 (Gin services)
└─ PostgreSQL 15

Infraestructura:
├─ Docker 24
├─ Kubernetes 1.27
├─ RabbitMQ 3.12
└─ Redis 7.2
```

---

**Documento actualizado:** 2025-02-10
**Revisor:** Architecture Team
**Aprobación:** [Firma]

**[[../../HERRAMIENTAS-8-AUTOMATIZACION|← Volver al Vault]]**
