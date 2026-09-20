# Contribuir a ContestaQuejas ISSSTE 🤝

¡Gracias por tu interés en contribuir! Este proyecto busca mejorar la atención a personas derechohabientes del ISSSTE mediante IA.

## 🚀 Formas de contribuir

- 🐛 **Reportar bugs** abriendo un [Issue](../../issues)
- 💡 **Sugerir funcionalidades** abriendo un Issue con el label `enhancement`
- 📝 **Mejorar la documentación** (README, comentarios, ejemplos)
- 🔧 **Enviar Pull Requests** con correcciones o nuevas funcionalidades
- 🌐 **Traducir** la interfaz a otros idiomas
- ⚖️ **Revisar el marco normativo** y sugerir actualizaciones legales

## 🛠️ Configuración del entorno de desarrollo

```bash
# 1. Fork y clona el repo
git clone https://github.com/TU_USUARIO/contestaquejas-issste.git
cd contestaquejas-issste

# 2. Instala dependencias
bun install

# 3. Configura variables de entorno
cp .env.example .env

# 4. Inicia el servidor de desarrollo
bun run dev

# 5. Verifica que el lint pase
bun run lint
```

## 📋 Estándares de código

### TypeScript
- Usa **tipos explícitos** en firmas de funciones públicas
- Evita `any`; prefiere `unknown` cuando no conoces el tipo
- Mantén los archivos bajo 500 líneas

### Componentes React
- Usa **functional components** con hooks
- Marca componentes de cliente con `"use client"` al inicio
- Prefiere **shadcn/ui** sobre componentes custom
- Nombres de archivos en **kebab-case** (`upload-zone.tsx`)

### API Routes
- Usa `export const runtime = "nodejs"` para endpoints con SDK de IA
- Implementa **manejo de errores** con try/catch
- Devuelve `NextResponse.json()` con códigos HTTP apropiados
- Documenta la interfaz en comentarios JSDoc

### Estilos
- Usa **Tailwind CSS** (no CSS modules)
- Componentes shadcn/ui con variante **New York**
- Respeta el sistema de colores de `globals.css`
- No uses indigo ni azul como primario

### Commits
Sigue [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: agrega exportación a Word
fix: corrige destinatario del oficio
docs: actualiza README
style: ajusta márgenes del PDF
refactor: simplifica lógica de polling
test: agrega pruebas del endpoint analyze
chore: actualiza dependencias
```

## 🧪 Antes de enviar un Pull Request

- [ ] `bun run lint` pasa sin errores
- [ ] `bun run build` compila correctamente
- [ ] Probaste manualmente el flujo completo
- [ ] Actualizaste la documentación si aplica
- [ ] Agregaste tests si es una nueva funcionalidad
- [ ] Los commits siguen Conventional Commits

## 🏛️ Consideraciones especiales

### Marco normativo
Cualquier cambio a `src/lib/normatividad.ts` debe:
- Citar la fuente legal exacta (artículo, fracción, inciso)
- Respetar las restricciones de redacción (no Art. 33, no plazos, no canalización externa)
- Ser revisado por alguien con conocimiento legal

### Formato PDF
Cambios a `src/lib/pdf-client.ts` deben:
- Mantener la garantía de **una sola página**
- Respetar el formato institucional del Hospital López Mateos
- Probar con textos de diferentes longitudes

### Reglas de IA
Cambios a los prompts en `src/app/api/*/route.ts` deben:
- Mantener las 16 reglas innegociables
- No introducir frases prohibidas
- Probar con la imagen de ejemplo (`download/queja_ejemplo.png`)

## 🐛 Reportar bugs

Incluye en el Issue:
1. **Descripción** clara del problema
2. **Pasos para reproducirlo**
3. **Comportamiento esperado** vs **actual**
4. **Capturas de pantalla** si aplica
5. **Entorno**: navegador, OS, versión de Bun/Node
6. **Logs** relevantes (sin datos sensibles)

## 💡 Sugerir funcionalidades

Incluye:
1. **Problema** que resuelve
2. **Solución propuesta**
3. **Alternativas** consideradas
4. **Contexto adicional**

## ❓ Preguntas

Abre un Issue con el label `question`. No hay preguntas tontas.

## 📜 Código de Conducta

Al participar, aceptas respetar nuestro [Código de Conducta](./CODE_OF_CONDUCT.md). Sé respetuoso, inclusivo y constructivo.

---

¡Gracias por contribuir a mejorar la atención en el ISSSTE! 🏥
