# ContestaQuejas ISSSTE 🏥

> Aplicación web interactiva para contestar quejas de servicios de salud del **ISSSTE** (Hospital Regional "Lic. Adolfo López Mateos", CDMX) con IA. Sube capturas de la queja, la IA interpreta el contenido y genera una contestación oficial en formato de oficio institucional, exportable a PDF en **una sola página tamaño carta**.

![Stack](https://img.shields.io/badge/Next.js-16-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript)
![Tailwind](https://img.shields.io/badge/Tailwind-4-38bdf8?logo=tailwindcss)
![License](https://img.shields.io/badge/License-MIT-green)

---

## ✨ Características principales

- 📤 **Carga de capturas**: arrastrar y soltar múltiples imágenes (PNG, JPG, WEBP, BMP, GIF). Preprocesamiento automático con corrección de orientación EXIF, redimensionado a 1024px y compresión JPEG.
- 🔍 **Análisis con IA (VLM)**: extrae el texto íntegro de la queja, la clasifica (tipo, gravedad, urgencia) y estructurara hechos clave, normas aplicables y pretensión del usuario.
- 🎛️ **Configuración personalizable**: estilo (formal/empático/jurídico/conciso), tono, tamaño y complejidad de la contestación.
- 👩‍⚕️ **Campo "Nombre de la paciente"**: aparece en el saludo del oficio.
- 📄 **Generación con IA (LLM)**: redacta la contestación fundamentada en la normatividad mexicana de salud (Constitución, Ley General de Salud, NOM, Reglamento del ISSSTE).
- 📑 **PDF institucional**: replica el formato oficial del Hospital López Mateos con logos, jerarquía institucional, oficio, firma y pie de página. **Garantizado en una sola hoja carta**.
- 🔄 **Patrón asíncrono con polling**: evita timeouts del gateway; cada petición dura <200ms.
- 🔁 **Reintentos automáticos** con backoff exponencial ante saturación del servicio de IA.

---

## 🛡️ Reglas de redacción incorporadas

La IA respeta estrictamente estas restricciones por defecto (configurables en `src/app/api/generate/route.ts`):

| ❌ Prohibido | ✅ En su lugar |
|--------------|---------------|
| Artículo 33 de la Ley de Derechos de Usuarias y Pacientes del ISSSTE | Otros artículos (Constitución Art. 4, Ley General de Salud Art. 51, etc.) |
| Plazos numéricos ("30 días", "15 días", "24 horas") | Expresiones generales ("a la brevedad", "en su oportunidad") |
| Sugerir acudir a CONAMED, Cofepris, CNDH, Contraloría, etc. | Resolver dentro del ámbito de la Coordinación de Ginecología y Obstetricia |
| Números telefónicos y datos de contacto | Sin datos de contacto en el cuerpo |
| Frases procesales ("le notificaremos…", "caso registrado con folio…") | Acciones concretas sin comprometer notificación |
| Aspectos no ginecológicos (laboratorio, rayos X, farmacia) | Enfoque exclusivo en Ginecología y Obstetricia |

---

## 🏗️ Stack tecnológico

| Capa | Tecnología |
|------|------------|
| Framework | **Next.js 16** (App Router, Turbopack) |
| Lenguaje | **TypeScript 5** |
| Estilos | **Tailwind CSS 4** + **shadcn/ui** (New York style) |
| IA Vision (VLM) | `z-ai-web-dev-sdk` → `chat.completions.createVision` |
| IA Texto (LLM) | `z-ai-web-dev-sdk` → `chat.completions.create` |
| PDF | `jspdf` + `jspdf-autotable` (generación 100% en el cliente) |
| Notificaciones | `sonner` |
| Animaciones | `framer-motion` |
| Iconos | `lucide-react` |
| Runtime | **Bun** (recomendado) o Node.js 18+ |

---

## 📁 Estructura del proyecto

```
contestaquejas-issste/
├── public/
│   ├── header_logos.png          ← Logos Gobierno·ISSSTE·Hospital
│   ├── header_ilustracion.png    ← Grabado histórico (encabezado derecho)
│   └── footer_ilustracion.png    ← Ilustración "Año de Margarita Maza"
├── src/
│   ├── app/
│   │   ├── layout.tsx            ← Layout raíz + Sonner toaster
│   │   ├── page.tsx              ← Página principal (header + footer)
│   │   ├── globals.css           ← Tailwind + shadcn theme
│   │   └── api/
│   │       ├── analyze/route.ts  ← VLM: análisis de la queja (POST+GET polling)
│   │       └── generate/route.ts ← LLM: generación de contestación (POST+GET polling)
│   ├── lib/
│   │   ├── normatividad.ts       ← Base de conocimiento legal México/ISSSTE
│   │   ├── image-processing.ts   ← EXIF + resize + compresión en cliente
│   │   ├── pdf-client.ts         ← Generador PDF institucional (1 página)
│   │   └── utils.ts              ← cn() helper
│   └── components/
│       ├── ui/                   ← Componentes shadcn/ui (40+)
│       └── app/
│           ├── queja-app.tsx     ← Orquestador principal + polling
│           ├── upload-zone.tsx   ← Drag&drop + compresión
│           ├── analysis-card.tsx ← Tarjeta de análisis estructurado
│           ├── config-panel.tsx  ← Configuración + datos del oficio
│           └── response-panel.tsx← Vista previa + exportar PDF
├── prisma/schema.prisma          ← Schema (vacío por defecto)
├── .env.example                  ← Variables de entorno de ejemplo
├── .gitignore
├── eslint.config.mjs
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── tailwind.config.ts
└── tsconfig.json
```

---

## 🚀 Instalación y uso

### Requisitos previos

- **Bun** ≥ 1.3 (recomendado) o Node.js ≥ 18
- Acceso al SDK `z-ai-web-dev-sdk` (incluido en el entorno Z.ai)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/TU_USUARIO/contestaquejas-issste.git
cd contestaquejas-issste

# 2. Instalar dependencias
bun install

# 3. Configurar variables de entorno
cp .env.example .env
# (Edita .env si necesitas claves de API)

# 4. Inicializar base de datos (opcional, solo si usas Prisma)
bun run db:push

# 5. Iniciar servidor de desarrollo
bun run dev
```

Abrir [http://localhost:3000](http://localhost:3000) en el navegador.

### Scripts disponibles

```bash
bun run dev        # Servidor de desarrollo (puerto 3000)
bun run build      # Build de producción
bun run start      # Servidor de producción
bun run lint       # ESLint
bun run db:push    # Aplicar schema de Prisma a la DB
bun run db:generate # Generar cliente de Prisma
```

---

## 📖 Flujo de uso

```
1. Subir capturas        2. Analizar            3. Configurar          4. Exportar PDF
┌─────────────┐         ┌─────────────┐        ┌─────────────┐        ┌─────────────┐
│  Drag & drop │   →    │  VLM extrae  │   →   │  Estilo      │   →   │  PDF 1 pág  │
│  de la queja │        │  texto + JSON│       │  Tono        │       │  tamaño oficio│
│  (múltiples) │        │  estructurado│       │  Tamaño      │       │  institucional│
└─────────────┘         └─────────────┘        │  Complejidad│       └─────────────┘
                                               │  Nombre paciente│
                                               │  Datos oficio │
                                               └─────────────┘
```

### Detalle del flujo

1. **Subir capturas**: el usuario arrastra imágenes de la queja. Se comprimen automáticamente (4.5 MB → ~120 KB).
2. **Analizar (VLM)**: la IA de visión extrae texto íntegro, lo estructura en JSON (motivo, folio, fechas, hechos clave, normas aplicables, clasificación por gravedad/urgencia).
3. **Configurar**: el usuario ajusta estilo/tono/tamaño/complejidad, escribe el nombre de la paciente y los datos del oficio, añade sugerencias.
4. **Generar (LLM)**: la IA redacta la contestación respetando las reglas de redacción.
5. **Exportar PDF**: se descarga un PDF con el formato institucional del Hospital López Mateos en **una sola página tamaño carta**.

---

## 🧠 Cómo funciona la IA

### Análisis (VLM)

El endpoint `POST /api/analyze` recibe las imágenes en base64, las envía al modelo `glm-4.6v` (VLM de Z.ai) con un prompt que pide devolver un JSON estructurado:

```json
{
  "texto_completo": "transcripción del texto visible",
  "resumen": { "motivo_queja": "...", "folio_queja": "...", ... },
  "clasificacion": { "tipo": "...", "gravedad": "...", "urgencia": "..." },
  "hechos_clave": ["..."],
  "normas_potencialmente_aplicables": ["..."]
}
```

### Generación (LLM)

El endpoint `POST /api/generate` recibe el análisis + configuración y construye un system prompt con:
- 16 reglas innegociables (lo que SÍ y lo que NO debe aparecer)
- La base de conocimiento legal completa (`normatividad.ts`)
- Las directrices de estilo/tono/tamaño/complejidad

### Patrón asíncrono con polling

Para evitar timeouts del gateway (que corta conexiones >60s), los endpoints usan:

```
Cliente                    Servidor
  │                          │
  │── POST /api/analyze ───→│ (inicia job en background)
  │←── { jobId } ───────────│ (responde en <200ms)
  │                          │
  │── GET ?id=jobId ───────→│
  │←── { status: pending } ─│
  │                          │
  │── GET ?id=jobId ───────→│ (cada 3s)
  │←── { status: done } ────│ (con resultado)
```

Esto permite que análisis de varios minutos no se corten.

---

## 📄 Formato del PDF

El PDF replica el oficio oficial del Hospital Regional "Lic. Adolfo López Mateos":

```
┌────────────────────────────────────────────────────────────┐
│ [Logos Gobierno·ISSSTE·Hospital]   [Grabado histórico]    │
├────────────────────────────────────────────────────────────┤
│                              DIRECCIÓN MÉDICA              │
│              HOSPITAL REGIONAL "LIC. ADOLFO LÓPEZ MATEOS"  │
│                              DIRECCIÓN                     │
│                              SUBDIRECCIÓN MÉDICA           │
│                              COORDINACIÓN DE GINECOLOGÍA.. │
│                              Oficio No. CGO/1234/2026      │
│                              Ciudad de México, a 7 de...   │
│                                                            │
│ LIC. MARÍA DEL ROCÍO PAZ OROPEZA                          │
│ COORD. DE ATENCIÓN AL DERECHOHABIENTE                     │
│ P R E S E N T E.-                                         │
│                                                            │
│ ASUNTO: CONTESTACIÓN A QUEJA POR...                       │
│                                                            │
│ Estimada paciente [NOMBRE]:                                │
│                                                            │
│ [Cuerpo de la contestación redactado por la IA...]         │
│                                                            │
│                    ─────────────────                       │
│                DR. ROBERTO MENDOZA GARCÍA                  │
│            COORDINADOR DE GINECOLOGÍA Y OBSTETRICIA        │
│                                                            │
├────────────────────────────────────────────────────────────┤
│ [Ilustración]   2026, año de       Av. Universidad No.1321│
│                 Margarita Maza     Col. Axotla, C.P. 01030│
└────────────────────────────────────────────────────────────┘
```

**Garantía de una sola página**: el generador prueba tamaños de fuente descendentes (10.5pt → 6pt) hasta encontrar el más grande que cabe en el espacio disponible (~350pt de cuerpo). Si aun así no cabe, trunca preservando "Atentamente,".

---

## 🔧 Personalización

### Cambiar el área emisora

Edita el valor por defecto en `src/components/app/config-panel.tsx`:

```typescript
export const OFICIO_DEFAULT: DatosOficio = {
  area_emisora: "COORDINACIÓN DE GINECOLOGÍA Y OBSTETRICIA", // ← cambia aquí
  // ...
};
```

### Cambiar el destinatario por defecto

```typescript
export const OFICIO_DEFAULT: DatosOficio = {
  destinatario_nombre: "",  // ←Vacío usa "LIC. MARÍA DEL ROCÍO PAZ OROPEZA"
  destinatario_cargo: "COORD. DE ATENCIÓN AL DERECHOHABIENTE",
  // ...
};
```

### Ajustar las reglas de redacción

Edita la función `construirSystemPrompt()` en `src/app/api/generate/route.ts` (reglas 1-16).

### Cambiar el enfoque médico

Edita `src/lib/normatividad.ts` (sección "ÁMBITO DE COMPETENCIA").

### Cambiar los logos del PDF

Sustituye los archivos en `public/`:
- `header_logos.png` (encabezado izquierda)
- `header_ilustracion.png` (encabezado derecha)
- `footer_ilustracion.png` (pie de página izquierda)

---

## ⚠️ Limitaciones y advertencias

- **La IA puede equivocarse**: la contestación generada es un borrador que **debe ser revisado por personal autorizado** antes de su entrega oficial.
- **No sustituye criterio jurídico**: es una herramienta de apoyo, no una fuente de verdad legal.
- **Latencia variable**: el análisis VLM puede tardar de 20s a 5min según la imagen y la saturación del servicio de IA.
- **Sin persistencia**: los jobs se guardan en memoria y se eliminan a los 15 minutos. No hay historial de contestaciones.
- **Uso institucional**: diseñado para el ISSSTE CDMX; adaptar para otras instituciones requiere ajustes en normatividad y formato.

---

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -m 'Agrega nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

Ver [`CONTRIBUTING.md`](./CONTRIBUTING.md) para más detalles.

---

## 📝 Licencia

Distribuido bajo la licencia **MIT**. Ver [`LICENSE`](./LICENSE) para más información.

---

## 🏛️ Créditos

- **Institución**: Hospital Regional "Lic. Adolfo López Mateos" — ISSSTE, Ciudad de México
- **Coordinación emisora**: Coordinación de Ginecología y Obstetricia
- **Marco normativo**: Constitución Política de los Estados Unidos Mexicanos, Ley General de Salud, Ley de los Derechos de las Personas Usuarias y Pacientes del ISSSTE, NOM-024-SSA3-2012, NOM-004-SSA3-2012
- **Tecnología IA**: [Z.ai](https://z.ai) — modelo `glm-4.6v` (VLM) y LLM

---

## 📞 Soporte

Si encuentras un bug o tienes una sugerencia, abre un [Issue](https://github.com/TU_USUARIO/contestaquejas-issste/issues).

---

**Nota**: Este proyecto fue desarrollado como herramienta de apoyo para la gestión de quejas en el sector salud público mexicano. No es un producto oficial del ISSSTE.
