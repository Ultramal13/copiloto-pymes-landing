# ARQUITECTURA TECNICA - COPILOTO PYME

**Version 1.0 | Marzo 2026**
**Estado: PLANIFICACION - PRE-DESARROLLO**

---

## 1. VISION GENERAL

### Que estamos construyendo
Una plataforma SaaS web que funciona como brujula IA proactiva para pymes chilenas. La app se conecta al SII, analiza datos tributarios/financieros, y envia alertas predictivas via WhatsApp y dashboard web con semaforo visual.

### Principios de arquitectura
1. **TypeScript everywhere** - Un solo lenguaje, frontend y backend
2. **Monorepo** - Todo el codigo en un repositorio, paquetes compartidos
3. **API-first** - Backend desacoplado, permite agregar app movil despues
4. **Serverless-ready** - Deploy en Vercel/Railway, escala automaticamente
5. **Security-first** - Claves SII cifradas AES-256, JWT, OAuth 2.0
6. **Claude Code friendly** - Estructura clara para desarrollo asistido por IA

---

## 2. STACK TECNOLOGICO

### 2.1 Frontend

| Componente | Tecnologia | Justificacion |
|-----------|-----------|---------------|
| Framework | **Next.js 14+ (App Router)** | SSR para landing/SEO, API routes integradas, deploy facil en Vercel |
| UI | **React 18** | Ecosistema maduro, componentes reutilizables |
| Estilos | **Tailwind CSS 3.4+** | Desarrollo rapido, responsive nativo, sin CSS custom |
| Componentes | **shadcn/ui** | Componentes accesibles, personalizables, sin vendor lock-in |
| Graficos | **Recharts** | Graficos React nativos para dashboard, flujo de caja |
| Estado | **Zustand** | Liviano (1KB), sin boilerplate, TypeScript nativo |
| Forms | **React Hook Form + Zod** | Validacion tipada, performance optima |
| Iconos | **Lucide React** | Consistente, tree-shakeable |
| PWA | **next-pwa** | Permite instalar como app en celular sin app store |

### 2.2 Backend

| Componente | Tecnologia | Justificacion |
|-----------|-----------|---------------|
| Runtime | **Node.js 20 LTS** | Estable, soporte largo plazo |
| Framework | **Next.js API Routes (Fase 1)** | Sin servidor separado en MVP, migrar a Fastify si escala |
| ORM | **Prisma 5+** | Tipado fuerte, migraciones, compatible Supabase |
| Validacion | **Zod** | Schema compartido frontend/backend |
| Auth | **NextAuth.js v5 (Auth.js)** | OAuth, JWT, session management integrado |
| Cron/Jobs | **BullMQ + Redis** | Colas de trabajo para sync SII, envio alertas |
| Email | **Resend** | API moderna, React Email para templates |
| Logs | **Pino** | Structured logging, rapido |

### 2.3 Base de datos

| Componente | Tecnologia | Justificacion |
|-----------|-----------|---------------|
| DB principal | **PostgreSQL (Supabase)** | Gratis hasta 500MB, auth integrada, real-time |
| Cache | **Redis (Upstash)** | Serverless Redis, cache de sesiones SII, colas BullMQ |
| ORM | **Prisma** | Migraciones, tipado, compatible Supabase |

### 2.4 Integraciones externas

| Servicio | Proveedor | Funcion | Costo estimado |
|---------|-----------|---------|---------------|
| **SII Chile** | API Gateway (apigateway.cl) o BaseAPI | Registro compras/ventas, BHE, datos contribuyente | Desde gratis (prueba) hasta ~$30-50k CLP/ano |
| **WhatsApp** | Twilio WhatsApp API | Alertas, chatbot, notificaciones | $0.005 USD/mensaje + Meta fees (~$0.003/utility) |
| **IA Chatbot** | Anthropic Claude API (Sonnet) | Asistente tributario/laboral 24/7 | ~$3 USD/1M input tokens |
| **Email** | Resend | Transaccional, reportes semanales | Gratis hasta 3k emails/mes |
| **Pagos** | Mercado Pago o Flow.cl | Suscripciones mensuales | 3.49% + IVA por transaccion |

### 2.5 Infraestructura y deploy

| Componente | Servicio | Justificacion |
|-----------|---------|---------------|
| Frontend hosting | **Vercel** | Deploy automatico desde GitHub, CDN global, free tier generoso |
| Backend/Workers | **Railway** | Containers Node.js, cron jobs, $5/mes para empezar |
| Base de datos | **Supabase** | Postgres managed, free tier 500MB |
| Redis | **Upstash** | Serverless, pay-per-request, free tier 10k/dia |
| CI/CD | **GitHub Actions** | Automatizado en push a main/develop |
| Monitoreo | **Sentry** | Error tracking, free tier 5k events/mes |
| Analytics | **PostHog** | Product analytics, free tier 1M events/mes |

---

## 3. ESTRUCTURA DEL MONOREPO

```
copiloto-pyme/
├── apps/
│   └── web/                    # Next.js app (frontend + API routes)
│       ├── app/                # App Router
│       │   ├── (auth)/         # Rutas de autenticacion
│       │   │   ├── login/
│       │   │   ├── register/
│       │   │   └── onboarding/
│       │   ├── (dashboard)/    # Rutas protegidas
│       │   │   ├── dashboard/  # Vista principal + semaforo
│       │   │   ├── alertas/    # Lista de alertas activas
│       │   │   ├── chat/       # Chatbot IA tributario
│       │   │   ├── facturas/   # Registro compras/ventas SII
│       │   │   └── settings/   # Configuracion cuenta
│       │   ├── api/            # API Routes
│       │   │   ├── auth/       # NextAuth endpoints
│       │   │   ├── sii/        # Sync SII, consultas
│       │   │   ├── alerts/     # Motor de alertas
│       │   │   ├── chat/       # Proxy Claude API
│       │   │   ├── whatsapp/   # Webhook Twilio
│       │   │   └── webhooks/   # Pagos, etc.
│       │   ├── layout.tsx
│       │   └── page.tsx        # Landing page
│       ├── components/
│       │   ├── ui/             # shadcn/ui components
│       │   ├── dashboard/      # Semaforo, KPIs, graficos
│       │   ├── alerts/         # Cards de alertas
│       │   ├── chat/           # Interfaz chatbot
│       │   └── onboarding/     # Wizard conexion SII
│       ├── lib/
│       │   ├── auth.ts         # Config NextAuth
│       │   ├── db.ts           # Cliente Prisma
│       │   └── utils.ts
│       ├── next.config.js
│       ├── tailwind.config.ts
│       └── tsconfig.json
│
├── packages/
│   ├── db/                     # Schema Prisma + migraciones
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   └── migrations/
│   │   └── index.ts            # Re-export Prisma client
│   │
│   ├── sii/                    # Modulo conexion SII
│   │   ├── client.ts           # Cliente API Gateway/BaseAPI
│   │   ├── sync.ts             # Logica de sincronizacion
│   │   ├── parser.ts           # Parseo de datos SII
│   │   ├── crypto.ts           # Cifrado/descifrado claves
│   │   └── types.ts            # Tipos TypeScript del SII
│   │
│   ├── alerts/                 # Motor de reglas de alertas
│   │   ├── engine.ts           # Evaluador de reglas
│   │   ├── rules/              # Reglas por categoria
│   │   │   ├── iva.ts          # Reglas IVA/F29
│   │   │   ├── retenciones.ts  # Reglas retenciones
│   │   │   ├── facturas.ts     # Reglas facturas
│   │   │   └── general.ts      # Reglas generales
│   │   ├── semaforo.ts         # Logica semaforo verde/amarillo/rojo
│   │   └── types.ts
│   │
│   ├── whatsapp/               # Integracion WhatsApp
│   │   ├── client.ts           # Cliente Twilio
│   │   ├── templates.ts        # Templates de mensajes
│   │   ├── webhook.ts          # Handler de webhooks entrantes
│   │   └── types.ts
│   │
│   ├── ai/                     # Modulo IA (Claude)
│   │   ├── client.ts           # Cliente Anthropic API
│   │   ├── prompts/            # System prompts
│   │   │   ├── tributario.ts   # Prompt base tributario
│   │   │   └── laboral.ts      # Prompt base laboral (Fase 2)
│   │   ├── context.ts          # Builder de contexto por usuario
│   │   └── types.ts
│   │
│   └── shared/                 # Tipos y utilidades compartidas
│       ├── types.ts            # Tipos globales
│       ├── constants.ts        # Constantes (fechas SII, umbrales)
│       ├── rut.ts              # Validador/formateador RUT
│       └── money.ts            # Utilidades monetarias CLP
│
├── docs/                       # Documentacion de negocio
│   ├── reglas-alertas.md       # Reglas del motor de alertas
│   ├── base-conocimiento-tributaria.md
│   ├── base-conocimiento-laboral.md
│   ├── compliance-score.md     # Logica del score
│   ├── calendario-tributario.md
│   └── mensajes-whatsapp.md    # Templates aprobados
│
├── .github/
│   └── workflows/
│       ├── ci.yml              # Lint + test en PR
│       └── deploy.yml          # Deploy a Vercel/Railway
│
├── package.json
├── pnpm-workspace.yaml
├── turbo.json                  # Turborepo config
├── tsconfig.base.json
├── .env.example
└── README.md
```

---

## 4. MODELO DE DATOS (PRISMA SCHEMA)

### 4.1 Esquema principal

```prisma
// packages/db/prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// ============================================
// USUARIOS Y AUTENTICACION
// ============================================

model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  phone         String?   // Para WhatsApp
  whatsappOptIn Boolean   @default(false)
  role          UserRole  @default(OWNER)
  plan          Plan      @default(FREE)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  // Relaciones
  empresas      EmpresaUser[]
  sessions      Session[]
  accounts      Account[]

  @@map("users")
}

enum UserRole {
  OWNER       // Dueno de pyme
  CONTADOR    // Contador asociado
  ADMIN       // Admin plataforma
}

enum Plan {
  FREE          // Plan Emprendedor
  CUMPLIMIENTO  // $19.990/mes
  ESCALABILIDAD // $39.990/mes
}

// ============================================
// EMPRESAS (PYMES)
// ============================================

model Empresa {
  id              String    @id @default(cuid())
  rut             String    @unique  // RUT empresa
  razonSocial     String
  giro            String?
  comuna          String?
  region          String?
  inicioActividades DateTime?
  regimenTributario RegimenTributario?

  // Credenciales SII (cifradas AES-256)
  siiClaveCifrada String?
  siiUltimaSync   DateTime?
  siiSyncActiva   Boolean   @default(false)

  // Metricas
  complianceScore Int       @default(50)

  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt

  // Relaciones
  usuarios        EmpresaUser[]
  facturas        Factura[]
  alertas         Alerta[]
  obligaciones    ObligacionTributaria[]
  syncLogs        SiiSyncLog[]

  @@map("empresas")
}

enum RegimenTributario {
  REGIMEN_14A     // Renta efectiva
  REGIMEN_14D3    // Pro Pyme general
  REGIMEN_14D8    // Pro Pyme transparente
  REGIMEN_14E     // Renta presunta
  SEGUNDA_CATEGORIA // Honorarios
}

model EmpresaUser {
  id        String   @id @default(cuid())
  userId    String
  empresaId String
  role      String   @default("owner") // owner, viewer, contador

  user      User     @relation(fields: [userId], references: [id])
  empresa   Empresa  @relation(fields: [empresaId], references: [id])

  @@unique([userId, empresaId])
  @@map("empresa_users")
}

// ============================================
// DATOS TRIBUTARIOS (SYNC SII)
// ============================================

model Factura {
  id            String        @id @default(cuid())
  empresaId     String
  tipo          TipoDocumento
  folio         Int
  fechaEmision  DateTime
  rutEmisor     String
  rutReceptor   String
  montoNeto     Int           // En CLP
  montoIva      Int
  montoTotal    Int
  estado        EstadoFactura @default(REGISTRADO)
  periodo       String        // "2026-03"
  direccion     DireccionFactura // COMPRA o VENTA

  empresa       Empresa       @relation(fields: [empresaId], references: [id])
  createdAt     DateTime      @default(now())

  @@index([empresaId, periodo])
  @@index([empresaId, direccion, periodo])
  @@map("facturas")
}

enum TipoDocumento {
  FACTURA_ELECTRONICA       // 33
  FACTURA_EXENTA            // 34
  BOLETA_ELECTRONICA        // 39
  BOLETA_EXENTA             // 41
  NOTA_CREDITO              // 61
  NOTA_DEBITO               // 56
  GUIA_DESPACHO             // 52
  BOLETA_HONORARIOS         // BHE
  BOLETA_TERCEROS           // BTE
}

enum EstadoFactura {
  REGISTRADO
  PENDIENTE
  ACEPTADO
  RECHAZADO
  RECLAMADO
  NO_INCLUIR
}

enum DireccionFactura {
  COMPRA
  VENTA
}

// ============================================
// OBLIGACIONES TRIBUTARIAS
// ============================================

model ObligacionTributaria {
  id              String    @id @default(cuid())
  empresaId       String
  tipo            TipoObligacion
  periodo         String    // "2026-03"
  fechaVencimiento DateTime
  montoEstimado   Int?      // Monto calculado
  montoPagado     Int?
  estado          EstadoObligacion @default(PENDIENTE)

  empresa         Empresa   @relation(fields: [empresaId], references: [id])
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt

  @@unique([empresaId, tipo, periodo])
  @@map("obligaciones_tributarias")
}

enum TipoObligacion {
  IVA_F29         // Declaracion mensual IVA
  PPM             // Pago Provisional Mensual
  RETENCION_HONORARIOS
  RETENCION_SUELDOS
  OPERACION_RENTA // Declaracion anual
}

enum EstadoObligacion {
  PENDIENTE
  PAGADO
  VENCIDO
  PARCIAL
}

// ============================================
// ALERTAS
// ============================================

model Alerta {
  id          String      @id @default(cuid())
  empresaId   String
  tipo        TipoAlerta
  severidad   Severidad
  titulo      String
  mensaje     String
  accion      String?     // Recomendacion accionable
  montoRiesgo Int?        // Monto en juego (multa potencial)
  leida       Boolean     @default(false)
  resuelta    Boolean     @default(false)
  enviadaWhatsApp Boolean @default(false)
  fechaVencimiento DateTime? // Si aplica

  empresa     Empresa     @relation(fields: [empresaId], references: [id])
  createdAt   DateTime    @default(now())

  @@index([empresaId, resuelta])
  @@index([empresaId, severidad])
  @@map("alertas")
}

enum TipoAlerta {
  IVA_VENCIMIENTO
  IVA_MONTO_ALTO
  FACTURA_RECHAZADA
  FACTURA_SIN_ACEPTAR
  RETENCION_PENDIENTE
  PPM_PENDIENTE
  COMPLIANCE_BAJO
  NORMATIVA_NUEVA
  FLUJO_CAJA_RIESGO
}

enum Severidad {
  VERDE     // Informativa
  AMARILLO  // Atencion requerida
  ROJO      // Accion urgente
}

// ============================================
// LOGS Y AUDITORIA
// ============================================

model SiiSyncLog {
  id          String    @id @default(cuid())
  empresaId   String
  tipo        String    // "compras", "ventas", "bhe"
  periodo     String
  estado      String    // "success", "error", "partial"
  registros   Int       @default(0)
  error       String?
  duracionMs  Int?

  empresa     Empresa   @relation(fields: [empresaId], references: [id])
  createdAt   DateTime  @default(now())

  @@map("sii_sync_logs")
}

model ChatMessage {
  id          String    @id @default(cuid())
  empresaId   String
  role        String    // "user" o "assistant"
  content     String
  tokens      Int?      // Tokens usados
  createdAt   DateTime  @default(now())

  @@index([empresaId, createdAt])
  @@map("chat_messages")
}

// ============================================
// AUTH (NextAuth)
// ============================================

model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String
  provider          String
  providerAccountId String
  refresh_token     String? @db.Text
  access_token      String? @db.Text
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String? @db.Text

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([provider, providerAccountId])
  @@map("accounts")
}

model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@map("sessions")
}
```

---

## 5. INTEGRACION SII - ESTRATEGIA TECNICA

### 5.1 Como nos conectamos al SII

El SII de Chile NO tiene una API publica oficial para terceros. La conexion se hace via:

**Opcion A (Recomendada para MVP): Usar API Gateway de terceros**
- **apigateway.cl** o **baseapi.cl** - APIs REST que hacen scraping del SII
- Envias RUT + clave SII del usuario
- Recibes datos estructurados en JSON (compras, ventas, BHE, datos contribuyente)
- Costo: desde gratis (prueba) hasta planes pagos por volumen

**Opcion B (Futuro): Integracion directa con web services SII**
- El SII tiene web services SOAP para DTE (envio/consulta de facturas)
- Requiere certificado digital del contribuyente
- Mas complejo pero sin intermediarios
- Reservar para Fase 2-3

### 5.2 Flujo de sincronizacion

```
1. Usuario registra empresa (RUT + clave SII)
2. Clave se cifra con AES-256 y se almacena en DB
3. Cron job (cada 6 horas) descifra clave temporalmente
4. Llama a API Gateway: GET /sii/rcv/compras?periodo=2026-03
5. Llama a API Gateway: GET /sii/rcv/ventas?periodo=2026-03
6. Parsea respuesta JSON
7. Upsert en tabla facturas (evita duplicados por folio)
8. Motor de alertas evalua reglas sobre datos nuevos
9. Si hay alertas ROJAS/AMARILLAS, enviar via WhatsApp
10. Log de sincronizacion
```

### 5.3 Seguridad de credenciales SII

```typescript
// packages/sii/crypto.ts
import { createCipheriv, createDecipheriv, randomBytes } from 'crypto';

const ALGORITHM = 'aes-256-gcm';
const KEY = Buffer.from(process.env.SII_ENCRYPTION_KEY!, 'hex'); // 32 bytes

export function encrypt(text: string): string {
  const iv = randomBytes(16);
  const cipher = createCipheriv(ALGORITHM, KEY, iv);
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  const authTag = cipher.getAuthTag().toString('hex');
  return `${iv.toString('hex')}:${authTag}:${encrypted}`;
}

export function decrypt(encryptedText: string): string {
  const [ivHex, authTagHex, encrypted] = encryptedText.split(':');
  const iv = Buffer.from(ivHex, 'hex');
  const authTag = Buffer.from(authTagHex, 'hex');
  const decipher = createDecipheriv(ALGORITHM, KEY, iv);
  decipher.setAuthTag(authTag);
  let decrypted = decipher.update(encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  return decrypted;
}
```

---

## 6. MOTOR DE ALERTAS - ARQUITECTURA

### 6.1 Como funciona

El motor de alertas es un evaluador de reglas que corre despues de cada sincronizacion SII y tambien en cron diario.

```
Datos SII frescos
    ↓
Motor de reglas (evalua todas las reglas activas)
    ↓
¿Alerta nueva? → SI → Crear alerta en DB
                        ↓
                  ¿Severidad ROJA? → Enviar WhatsApp inmediato
                  ¿Severidad AMARILLA? → Agrupar en resumen diario
                  ¿Severidad VERDE? → Solo mostrar en dashboard
```

### 6.2 Estructura de una regla

```typescript
// packages/alerts/types.ts
interface AlertRule {
  id: string;
  name: string;
  category: 'iva' | 'retenciones' | 'facturas' | 'general';
  evaluate: (context: EmpresaContext) => AlertResult | null;
}

interface EmpresaContext {
  empresa: Empresa;
  facturas: Factura[];         // Del periodo actual
  obligaciones: ObligacionTributaria[];
  hoy: Date;
  diasParaVencimiento: (fecha: Date) => number;
}

interface AlertResult {
  tipo: TipoAlerta;
  severidad: Severidad;
  titulo: string;
  mensaje: string;
  accion: string;             // Recomendacion concreta
  montoRiesgo?: number;       // Multa potencial
}
```

### 6.3 Semaforo visual

```
ROJO:    Hay obligaciones vencidas O vencen en < 3 dias O facturas rechazadas sin resolver
AMARILLO: Obligaciones vencen en 3-7 dias O hay facturas pendientes de aceptar > 5 dias
VERDE:   Todo al dia, sin alertas activas de severidad alta
```

---

## 7. INTEGRACION WHATSAPP

### 7.1 Flujo tecnico

```
Copiloto Backend
    ↓ (HTTP POST)
Twilio WhatsApp API
    ↓
Meta WhatsApp Cloud
    ↓
Telefono del usuario
    ↓ (responde)
Meta → Twilio Webhook → Copiloto API /api/whatsapp/webhook
    ↓
Si es consulta → Enviar a Claude API → Responder
Si es accion → Ejecutar (ej: "Si" para marcar alerta resuelta)
```

### 7.2 Templates de mensajes (requieren aprobacion Meta)

**Template: alerta_iva_vencimiento**
```
🔴 *Alerta IVA - {{empresa}}*

Tu declaracion F29 vence el {{fecha}}.
Monto estimado: ${{monto}}

💡 *Recomendacion:* {{recomendacion}}

Responde "OK" para marcar como visto.
```

**Template: resumen_semanal**
```
📊 *Resumen semanal - {{empresa}}*

Semaforo: {{estado}} {{emoji}}
Ventas del periodo: ${{ventas}}
Compras del periodo: ${{compras}}
IVA estimado: ${{iva}}

Proxima obligacion: {{proxima}} ({{dias}} dias)

Responde "DETALLE" para mas info.
```

### 7.3 Costos estimados WhatsApp

Para 500 usuarios con ~8 mensajes/mes promedio:
- Twilio fee: 500 × 8 × $0.005 = $20 USD/mes
- Meta utility fee: ~500 × 4 × $0.003 = $6 USD/mes (solo templates fuera de ventana)
- **Total: ~$26 USD/mes (~$24.000 CLP)**
- Costo por usuario: ~$48 CLP/mes (irrelevante vs $19.990 suscripcion)

---

## 8. CHATBOT IA (CLAUDE)

### 8.1 Estrategia

No hacemos fine-tuning. Usamos Claude Sonnet via API con un system prompt masivo que contiene la normativa tributaria y laboral chilena, mas el contexto especifico de la empresa del usuario (sus datos SII).

### 8.2 Arquitectura del prompt

```
SYSTEM PROMPT (fijo, ~4000 tokens)
├── Rol: "Eres el asistente tributario de Copiloto Pyme..."
├── Normativa tributaria vigente (resumen estructurado)
├── Tasas y fechas clave 2025-2026
├── Reglas de respuesta (lenguaje simple, sin tecnicismos)
└── Restricciones (no es abogado, recomendar consultar contador)

CONTEXTO EMPRESA (dinamico, ~1000 tokens)
├── RUT, regimen tributario, giro
├── Ultimas facturas y montos
├── Obligaciones pendientes
├── Score de compliance actual
└── Alertas activas

HISTORIAL CONVERSACION (ultimos 10 mensajes)

MENSAJE DEL USUARIO
```

### 8.3 Costo estimado Claude API

- Prompt promedio: ~5000 tokens input + ~500 tokens output
- Claude Sonnet: $3/1M input, $15/1M output
- Costo por consulta: ~$0.022 USD
- 500 usuarios × 10 consultas/mes = $110 USD/mes (~$100.000 CLP)

---

## 9. ROADMAP DE DESARROLLO

### Fase 0: Setup (Semana 1-2)
- [ ] Crear monorepo con pnpm + Turborepo
- [ ] Configurar Next.js + Tailwind + shadcn/ui
- [ ] Setup Prisma + Supabase
- [ ] Configurar GitHub + CI/CD basico
- [ ] Deploy inicial Vercel (landing page)
- [ ] Crear cuenta API Gateway / BaseAPI (prueba gratis)

### Fase 1A: Auth + Onboarding (Semana 3-4)
- [ ] Registro/login con email (NextAuth)
- [ ] Flujo onboarding: crear empresa + conectar SII
- [ ] Cifrado y almacenamiento seguro de credenciales
- [ ] Primera sincronizacion manual con SII
- [ ] Pantalla de confirmacion "datos cargados"

### Fase 1B: Dashboard + Semaforo (Semana 5-7)
- [ ] Dashboard principal con KPIs
- [ ] Semaforo tributario (verde/amarillo/rojo)
- [ ] Vista de facturas (compras y ventas del periodo)
- [ ] Calculo IVA estimado del mes
- [ ] Detalle de obligaciones y vencimientos

### Fase 1C: Motor de Alertas (Semana 8-9)
- [ ] Implementar engine de reglas
- [ ] Reglas IVA (vencimiento, monto)
- [ ] Reglas facturas (rechazadas, pendientes 8 dias)
- [ ] Reglas retenciones
- [ ] Cron job de evaluacion diaria
- [ ] Notificaciones en-app

### Fase 1D: WhatsApp + Chat IA (Semana 10-12)
- [ ] Integracion Twilio WhatsApp
- [ ] Templates basicos (alerta IVA, resumen semanal)
- [ ] Webhook para mensajes entrantes
- [ ] Chatbot IA con Claude API
- [ ] System prompt tributario completo
- [ ] Historial de conversaciones

### Fase 1E: Pagos + Launch (Semana 13-14)
- [ ] Integracion Mercado Pago / Flow.cl
- [ ] Logica de planes (Free / Cumplimiento / Escalabilidad)
- [ ] Paywall en features premium
- [ ] Landing page final
- [ ] Beta cerrada (50 usuarios)

---

## 10. CONTENIDO QUE NECESITAMOS REDACTAR

Antes de programar, necesitamos estos documentos de contenido:

### 10.1 Reglas del motor de alertas (PRIORIDAD 1)
- Calendario tributario completo con fechas de vencimiento
- Reglas logicas para cada tipo de alerta
- Umbrales del semaforo
- Textos de cada alerta (titulo, mensaje, recomendacion)

### 10.2 Base de conocimiento tributaria para Claude (PRIORIDAD 2)
- Obligaciones por tipo de contribuyente
- Tasas vigentes 2025-2026
- 50-100 preguntas frecuentes con respuestas
- Cambios de Ley 21.713
- Multas y sanciones por tipo

### 10.3 Base de conocimiento laboral (PRIORIDAD 3 - Fase 2)
- Jornada maxima y ley 40 horas (progresion)
- Calculo horas extra
- Multas Direccion del Trabajo
- Feriados, permisos, licencias
- Cotizaciones (AFP, salud, seguro cesantia)

### 10.4 Logica Compliance Score
- Variables y pesos
- Formula de calculo
- Umbrales por nivel
- Acciones que suben/bajan score

### 10.5 UX Writing
- Textos de onboarding
- Templates WhatsApp (para aprobacion Meta)
- Mensajes del semaforo
- Tono de voz de la marca

---

## 11. VARIABLES DE ENTORNO

```env
# .env.example

# Base de datos
DATABASE_URL="postgresql://..."

# Auth
NEXTAUTH_SECRET="..."
NEXTAUTH_URL="http://localhost:3000"

# SII Integration
SII_API_PROVIDER="apigateway"  # o "baseapi"
SII_API_KEY="..."
SII_ENCRYPTION_KEY="..."  # 64 hex chars (32 bytes)

# WhatsApp (Twilio)
TWILIO_ACCOUNT_SID="..."
TWILIO_AUTH_TOKEN="..."
TWILIO_WHATSAPP_FROM="whatsapp:+14155238886"

# IA (Claude)
ANTHROPIC_API_KEY="..."
ANTHROPIC_MODEL="claude-sonnet-4-20250514"

# Redis
UPSTASH_REDIS_URL="..."
UPSTASH_REDIS_TOKEN="..."

# Pagos
FLOW_API_KEY="..."        # o MERCADOPAGO_ACCESS_TOKEN
FLOW_SECRET_KEY="..."

# Monitoreo
SENTRY_DSN="..."
POSTHOG_KEY="..."

# Email
RESEND_API_KEY="..."
```

---

## 12. SEGURIDAD

### Checklist de seguridad Fase 1
- [ ] Claves SII cifradas AES-256-GCM en reposo
- [ ] JWT con expiracion 24h + refresh token
- [ ] Rate limiting en API routes (100 req/min por IP)
- [ ] CORS configurado solo para dominios propios
- [ ] Headers de seguridad (CSP, HSTS, X-Frame-Options)
- [ ] Input validation con Zod en todas las rutas
- [ ] SQL injection prevenido por Prisma (parametrizado)
- [ ] XSS prevenido por React (escape automatico)
- [ ] Logs sin datos sensibles (nunca loguear claves)
- [ ] HTTPS obligatorio en produccion
- [ ] Cumplimiento ley proteccion datos Chile (19.628)

---

*Documento preparado: Marzo 2026*
*Proyecto: Copiloto Pyme*
*Version: 1.0 - Pre-desarrollo*
