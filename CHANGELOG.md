# Changelog 📝

Todos los cambios notables de este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/),
y este proyecto se adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### Pendiente
- Historial de contestaciones (con Prisma + SQLite)
- Autenticación con NextAuth.js
- Plantillas preconfiguradas por tipo de queja
- Modo "borrador con comentarios laterales" para revisión jurídica

---

## [1.0.0] — 2026-07-08

### ✨ Added — Versión inicial estable

#### Funcionalidades principales
- **Carga de capturas**: drag & drop multiple con soporte PNG, JPG, WEBP, BMP, GIF
- **Preprocesamiento de imágenes en cliente**: corrección EXIF, redimensionado a 1024px, compresión JPEG calidad 75%
- **Análisis VLM**: extracción de texto + JSON estructurado (motivo, folio, fechas, hechos clave, normas aplicables, clasificación por gravedad/urgencia)
- **Generación LLM**: contestación oficial con fundamento normativo
- **PDF institucional**: formato del Hospital Regional "Lic. Adolfo López Mateos" en **una sola página tamaño carta**

#### Configuración
- Campo **Nombre de la paciente** destacado en el panel
- Selectores de **estilo** (formal/empático/jurídico/conciso)
- Selectores de **tono** (neutral/cálido/enérgico/disculpa)
- Selectores de **tamaño** (corto/medio/extenso) — limitados a 500 palabras máximo
- Selectores de **complejidad** (sencillo/intermedio/jurídico)
- Campo de **sugerencias** libre
- **Datos del oficio** colapsables: número, área emisora, destinatario, asunto, fecha, firmante, año conmemorativo, dirección

#### Reglas de redacción innegociables
- ✅ Saludo en femenino ("Estimada paciente [NOMBRE]:")
- ✅ Cita Art. 4 Constitucional y Art. 51 Ley General de Salud
- ✅ Enfoque exclusivo en Ginecología y Obstetricia
- ✅ Máximo 500 palabras, una sola página
- ❌ Prohibido Art. 33 Ley ISSSTE
- ❌ Prohibidos plazos numéricos (30/15/60 días)
- ❌ Prohibida canalización a CONAMED, Cofepris, CNDH, etc.
- ❌ Prohibidos teléfonos y datos de contacto
- ❌ Prohibidas frases procesales específicas

#### Arquitectura técnica
- **Patrón asíncrono con polling**: POST inicia job → GET consulta cada 3s
  - Evita timeouts del gateway (cada petición <200ms)
  - Soporta análisis de varios minutos
- **Reintentos automáticos** con backoff exponencial (5s, 10s, 20s, 30s)
  - 4 intentos para análisis VLM
  - 3 intentos para generación LLM
  - Solo reintenta errores 502/503/504/network (no 400/401/403/404)
- **TTL de jobs**: 15 minutos en memoria
- **Compresión visible**: el usuario ve "4.5 MB → 123 KB" en cada miniatura

#### UI/UX
- Stepper visual de 4 pasos
- Toasts con mensajes por fases según tiempo transcurrido
- Vista previa HTML del oficio completo
- Tabs Documento / Texto plano
- Botones Copiar, Regenerar, Exportar PDF
- Animaciones Framer Motion
- Responsive móvil-first

#### PDF
- Encabezado: logos Gobierno·ISSSTE·Hospital + ilustración histórica
- Bloque jerárquico alineado a la derecha (Dirección Médica → Coordinación)
- Oficio No. y fecha
- Destinatario (funcionario, no paciente)
- Asunto en negrita
- Cuerpo justificado con tamaño de fuente auto-ajustable
- Firma centrada con línea horizontal
- Pie de página con ilustración + "Año de Margarita Maza" + dirección
- Metadatos discretos (fecha generación, estilo, tono, complejidad)

### 🏛️ Marco normativo
- Constitución Política de los Estados Unidos Mexicanos (Art. 1, 4)
- Ley General de Salud (Art. 6, 32, 51, 72, 73, 91-Bis, 417)
- Ley de los Derechos de las Personas Usuarias y Pacientes del ISSSTE (Art. 2, 4, 5, 7, 14, 21) — Art. 33 excluido por configuración
- Reglamento del ISSSTE
- NOM-024-SSA3-2012 (expediente clínico electrónico)
- NOM-004-SSA3-2012 (expediente clínico)
- NOM-178-SSA1-1998 (infraestructura)
- Ley Federal de Procedimiento Administrativo (Art. 6)

### 🛠️ Stack tecnológico
- **Next.js 16** con App Router y Turbopack
- **TypeScript 5** estricto
- **Tailwind CSS 4** con shadcn/ui (variante New York)
- **z-ai-web-dev-sdk** para VLM (`glm-4.6v`) y LLM
- **jsPDF** + **jspdf-autotable** para generación de PDF en cliente
- **Sonner** para notificaciones
- **Framer Motion** para animaciones
- **Lucide React** para iconos
- **Bun** como runtime recomendado

---

## Cómo versionar

- **PATCH** (`1.0.1`): correcciones de bugs que no rompen compatibilidad
- **MINOR** (`1.1.0`): nuevas funcionalidades compatibles con versiones anteriores
- **MAJOR** (`2.0.0`): cambios que rompen compatibilidad

---

[Unreleased]: https://github.com/TU_USUARIO/contestaquejas-issste/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/TU_USUARIO/contestaquejas-issste/releases/tag/v1.0.0
