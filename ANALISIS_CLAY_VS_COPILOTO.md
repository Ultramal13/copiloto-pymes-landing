# Análisis Competitivo: Clay.cl vs Copiloto Pymes

**Fecha:** 31 de marzo 2026
**Objetivo:** Entender posicionamiento de Clay y definir diferenciadores de Copiloto

---

## 1. VISIÓN GENERAL DE CLAY.CL

**Clay** es una fintech chilena establecida (9 años) que posiciona como "Software Financiero y Contable que automatiza y simplifica finanzas."

### Características principales:
- **Usuarios:** 3.500+ empresas confían en Clay
- **Enfoque:** Financiero + Contable + Cobranza + Recursos Humanos
- **Diferenciales:** Conciliación bancaria automática, asistente IA (Grac.IA), integración 14 bancos + SII + TGR
- **Soporte:** Equipo dedicado + capacitaciones + servicio contable complementario

---

## 2. COMPARATIVA DETALLADA

### **Stack & Integraciones**

| Dimensión | Clay | Copiloto |
|-----------|------|---------|
| **Integraciones Bancos** | 14 bancos | 0 (concepto) |
| **Integración SII** | ✅ Consulta vencimientos, DTEs, F29, F22 | ❌ Solo alertas genéricas |
| **Integración TGR** | ✅ Tesorería General República | ❌ No |
| **Integración HR** | ✅ Previred, Buk, Talana, HCM Front | ❌ No |
| **Integraciones Facturación** | ✅ Bsale, Nubox, Global EC | ❌ No |
| **Integraciones Cobranza** | ✅ Duemint, CobranzaOnline, Chipax | ❌ No |
| **Crypto** | ✅ Buda, Skipo | ❌ No |
| **IA Nativa** | ✅ Grac.IA (experta en Clay) | ⚠️ Chatbot keyword-based (no IA real) |

### **Funcionalidades Financieras**

| Feature | Clay | Copiloto |
|---------|------|---------|
| **Conciliación Bancaria** | ✅ 100% automática + asistida + manual | ❌ No |
| **Flujo de Caja** | ✅ Proyección 12 meses + reportes | ⚠️ Concepto, sin datos reales |
| **EERR / Balances** | ✅ Automatizado, 8 columnas, en línea | ❌ No |
| **Gestión Proveedores** | ✅ Cartera, pagos, seguimiento | ❌ No |
| **Gestión Clientes** | ✅ Cobranza automática, seguimiento | ❌ No |
| **Asientos Contables** | ✅ 80% automatizados | ❌ No |

### **Funcionalidades Tributarias & Laborales**

| Feature | Clay | Copiloto |
|---------|------|---------|
| **Gestión Tributaria** | ✅ F29, F22, RLI, Auditor RCV | ✅ Alertas tributarias (diferenciador) |
| **Alertas Laborales** | ❌ Solo integración HR | ✅ Alertas laborales (diferenciador) |
| **Predictividad** | ❌ Histórico/reactivo | ✅ 30 días antes (diferenciador) |
| **Cumplimiento Score** | ❌ No | ✅ Ring score visual (diferenciador) |

### **Precios & Modelos**

| Plan | Clay | Copiloto |
|------|------|---------|
| **Entrada Gratis** | ✅ 7 días trial sin tarjeta | ✅ Plan Emprendedor Gratis |
| **Plan Básico** | 3 UF/mes (~$135k CLP) | $19.990 CLP |
| **Plan Pro** | 6 UF/mes (~$270k CLP) | $39.990 CLP |
| **Usuarios** | ✅ Ilimitados en todos planes | ❌ No definido |
| **Factor diferencial** | Establecido, probado | **2-3x más accesible** |

---

## 3. FORTALEZAS DE CADA UNO

### **Strengths de Clay:**
✅ **Experiencia probada** — 9 años, 3.500+ usuarios, casos de éxito documentados
✅ **Automatización contable profunda** — 80% asientos automatizados (Clay es especialista)
✅ **Ecosistema integrado** — 14 integraciones + servicio contable complementario
✅ **Soporte personalizado** — Equipo dedicado, capacitaciones, onboarding estructurado
✅ **Multi-empresa** — Gestiona múltiples negocios desde un panel
✅ **IA especializada** — Grac.IA experta en Clay, no genérica

### **Strengths de Copiloto:**
✅ **Posicionamiento predictivo** — "30 días antes" es fortaleza única que Clay no tiene
✅ **Cobertura única** — Tributario + Laboral + Finanzas (sin competidor directo)
✅ **Precio accesible** — $19.990 vs 3-6 UF (2-3x más barato)
✅ **Simplicidad** — Diseñado para dueños sin contador, no tech-heavy
✅ **Alertas multicanal** — WhatsApp + email + in-app (Clay solo in-app)
✅ **Enfoque PyMEs micro** — Clay apunta a empresas medianas

---

## 4. BRECHAS CRÍTICAS DE COPILOTO

### **Para competir realmente, necesitas:**

#### **Inmediato (antes de 20-30 usuarios):**
1. ❌ **Integración real SII**
   - Clay: consulta vencimientos, DTEs, F29 directamente
   - Copiloto: solo alertas genéricas
   - Impacto: Sin SII integration, las alertas son "adivinanzas"
   - Acción: Implementar API SII directo o usar servicio como Clarifai/Plaid

2. ❌ **IA real en chatbot**
   - Clay: Grac.IA integrada, contextual a datos del usuario
   - Copiloto: Keyword-based ("si dice IVA → respuesta IVA")
   - Impacto: Usuarios descubren que no es IA
   - Acción: Integrar Claude API para chatbot contextual

3. ❌ **Dashboard en producción**
   - Clay: Web app profesional con datos sincronizados
   - Copiloto: HTML vanilla, datos hardcodeados
   - Impacto: No se puede validar valor propuesto
   - Acción: Deploy dashboard real con datos demo dinámicos (React + Tailwind)

4. ❌ **Onboarding autónomo**
   - Clay: Sign up → 7 días trial → datos de prueba automáticos
   - Copiloto: "Déjame tu WhatsApp"
   - Impacto: Fricción de entrada, requiere trabajo manual
   - Acción: Trial auto-setup con empresa demo + alertas de ejemplo

#### **A los 3 meses (20-100 usuarios):**
5. ❌ **Multi-usuario + roles**
   - Clay: Usuarios ilimitados con permisos granulares
   - Copiloto: Sin mencionar
   - Impacto: Contador no puede ver datos, solo dueño
   - Acción: RBAC básico (Admin, Contador, Readonly)

6. ❌ **Reportes personalizables**
   - Clay: EERR, balances, flujo de caja con detalles
   - Copiloto: Solo KPIs dashboard
   - Impacto: Usuario no puede exportar/compartir con contador
   - Acción: Reportes PDF exportables (flujo proyectado, resumen tributario/laboral)

7. ❌ **Integración HR (Previred mínimo)**
   - Clay: Previred, Buk, Talana
   - Copiloto: Alertas genéricas de horas/AFP/Isapre
   - Impacto: No sabe datos reales de nómina
   - Acción: Integración Previred API (Copiloto obtiene datos reales de empleados)

8. ❌ **Backend real + Base de datos**
   - Clay: Arquitectura escalable, múltiples empresas
   - Copiloto: Datos hardcodeados en HTML
   - Impacto: Imposible agregar usuarios reales
   - Acción: Migrar a React + Node.js + PostgreSQL (según tech stack definido)

---

## 5. ESTRATEGIA RECOMENDADA

### **NO intentes ser "Clay para PyMEs"** ❌
Clay ya domina contable + financiero. No ganarás ahí.

### **Sí sé "El sistema de prevención tributaria-laboral con predictividad"** ✅

```
POSICIONAMIENTO CLARO:

Clay  = "Ordena, cierra y reporta tus números al día"
       → Para: Empresas que necesitan contabilidad profesional
       → Valor: Automatización contable 80%, reportes EERR, integración 14 bancos

Copiloto = "Te avisa 30 DÍAS antes de cada riesgo tributario, laboral y de flujo"
         → Para: PyMEs sin contador que no quieren sorpresas
         → Valor: Prevención + IA + simpleza + precio accesible
```

### **Roadmap de Diferenciación:**

**Fase 1 (MVP real, ya):**
- ✅ Integración SII real (vencimientos tributarios)
- ✅ Chatbot IA con Claude API
- ✅ Dashboard con datos demo dinámicos
- ✅ Trial autónomo sin WhatsApp obligatorio

**Fase 2 (3 meses, 20-100 usuarios):**
- ✅ Integración Previred (alertas laborales reales)
- ✅ Reportes exportables (PDF flujo + resumen tributario/laboral)
- ✅ Multi-usuario básico
- ✅ Backend real (Node.js + PostgreSQL)

**Fase 3 (6 meses, 100-500 usuarios):**
- ✅ Automatización de trámites (declaración F29, PPM)
- ✅ Más integraciones HR (Buk, Talana)
- ✅ Integración nómina avanzada
- ✅ Contabilidad simplificada para 14-bis/RITAE

**Fase 4 (12+ meses):**
- Alianzas con contadores (como Clay hace)
- Ecosystem play (ofrecer datos a ERPs)
- Expansión Latinoamérica

---

## 6. RECOMENDACIONES ESPECÍFICAS

### **QUÉ MANTENER DE COPILOTO:**
1. Énfasis en **predictividad** (30 días antes es real diferenciador)
2. **Precio agresivo** ($19.990 es punto de entrada perfecto)
3. **Simplicidad UI** (diseñado para dueños, no contadores)
4. **Alertas WhatsApp** (Clay no lo hace)
5. **Scoring visual** (ring de cumplimiento es memorable)

### **QUÉ CAMBIAR URGENTE:**
1. **Chatbot keyword-based** → Claude API (IA real)
2. **Datos hardcodeados** → Backend con BD real
3. **Sin SII integration** → Integración SII (al menos vencimientos)
4. **Solo HTML/CSS** → React + Tailwind (profesional)
5. **OnBoarding = WhatsApp** → Trial automático

### **QUÉ COPIAR DE CLAY (pero adaptado):**
1. **Trial sin fricción** (7 días → tipo Clay, pero con datos Copiloto)
2. **Multi-usuario** (pero con roles simples: Admin, Contador, View)
3. **Reportes exportables** (pero enfocado en tributario/laboral, no contable full)
4. **Integración HR** (Previred mínimo; Copiloto es especialista en alertas laborales)

---

## 7. VENTAJA COMPETITIVA FINAL

### **Clay vs Copiloto: Quién gana en cada segmento**

| Segmento | Ganador | Por qué |
|----------|--------|--------|
| **Empresa mediana con contador** | Clay | Contabilidad profesional, 80% automatizada |
| **Empresa con CFO dedicado** | Clay | Reportes EERR, multi-empresa, integración ERP |
| **PyME sin contador, sin contable** | **Copiloto** | Alertas preventivas, precio bajo, simple |
| **Micro-empresa (0-5 empleados)** | **Copiloto** | Gratis entry, WhatsApp alerts, muy simple |
| **Compliance tributario exigente** | **Copiloto** | Especialista en alerts + predictividad |
| **Automación contable profunda** | Clay | 80% asientos, reportes complejos |

**Copiloto debería apuntar a:** PyMEs sin contador, micro-empresas, startups en fase temprana.
**Clay domina:** Empresas medianas con estructura contable, CFO o contador dedicado.

---

## 8. CONCLUSIÓN

Clay es fuerte pero **no es invencible**. No compites con Clay en contable—compites en **prevención tributaria-laboral con predictividad**.

**Tu oportunidad:**
- Clay ocupa espacio de "orden y reportería"
- Copiloto ocupa espacio de "prevención y alerta temprana"
- Son complementarios, no competencia directa

**Próximo paso:**
1. Ejecutar Fase 1 (MVP real con SII + IA real)
2. Validar con 10-20 usuarios si la predictividad genera valor real
3. Iterar roadmap basado en feedback real

**El riesgo:** Quedarse en prototipo HTML mientras Clay sigue creciendo. **La oportunidad:** El mercado de PyMEs sin contador es 10x más grande que las que usan Clay.

---

**Documento generado:** 31 de marzo 2026
