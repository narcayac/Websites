# Websites — convenciones del repositorio

Monorepo de proyectos web (agencia). Cada sitio vive en su propia carpeta de primer nivel.

## Stack por defecto

| Caso | Stack |
|---|---|
| Web de marketing, landing, sitio de contenido | **Astro + Tailwind CSS + TypeScript**, contenido en Markdown/MDX |
| App real (auth, dashboard, formularios complejos, datos en vivo) | **Next.js App Router + TypeScript + Tailwind + shadcn/ui** |
| Cliente ya en WordPress | Tema propio + **bloques Gutenberg nativos** (`block.json`, PHP del tema). Sin page builders pesados |
| Prototipo de una sola página | HTML + CSS en un archivo, sin build |

Razón del default: Astro envía 0 KB de JS salvo donde se pida, lo que da el mejor LCP/INP
sin trabajo extra. No metas React en un sitio que solo tiene contenido.

## Reglas transversales

- **Nada de dependencias por comodidad.** Antes de instalar un paquete, comprueba si CSS o el
  HTML nativo ya lo resuelven (`<details>`, `dialog`, `scroll-snap`, `:has()`, view transitions).
- **Mobile-first.** Se escribe el layout de móvil y se añaden breakpoints hacia arriba.
- **Imágenes**: siempre `width`/`height` (o `aspect-ratio`), formato moderno (AVIF/WebP),
  `loading="lazy"` salvo la imagen del hero, que va `eager` + `fetchpriority="high"`.
- **Accesibilidad no es una fase final**: HTML semántico, un solo `<h1>`, foco visible,
  contraste AA, todo navegable con teclado.
- **Sin texto dentro de imágenes** (mata SEO y accesibilidad).
- Idioma del contenido y del copy: el del cliente. Comentarios de código y commits: español.

## Verificación antes de dar algo por terminado

Ejecuta lo que exista en el proyecto: `npm run build`, `npm run lint`, `astro check` /
`tsc --noEmit`. Un cambio no está hecho si el build no pasa.

## Agentes especializados

Están en `.claude/agents/`. Ver `.claude/agents/README.md` para cuál usar y en qué orden.
