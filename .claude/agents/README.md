# Agentes especializados en desarrollo web

Ocho agentes para `.claude/agents/`. Claude los invoca solo cuando la tarea encaja con su
`description`, o puedes pedirlos por nombre: «usa el agente `seo-tecnico` en /clinica-dental».

| Agente | Para qué | Modelo |
|---|---|---|
| `frontend-builder` | Construir páginas, secciones y componentes. Detecta el stack del proyecto | hereda |
| `ui-designer` | Sistema de diseño (color, tipografía, espaciado), jerarquía visual, crítica de UI | hereda |
| `copy-web` | Titulares, propuesta de valor, CTAs, microcopy, metadatos | hereda |
| `responsive-qa` | Abre el sitio con Playwright y prueba de 320 a 1920 px | sonnet |
| `seo-tecnico` | Metadatos, Schema.org, sitemap, canónicas, hreflang, SEO local | sonnet |
| `performance-cwv` | Lighthouse, LCP/INP/CLS, imágenes, fuentes, bundle | sonnet |
| `accesibilidad-a11y` | WCAG 2.2 AA y EAA: teclado, contraste, lectores de pantalla | sonnet |
| `deploy-check` | Última puerta antes de publicar: build, enlaces, formularios, RGPD | sonnet |

## Orden recomendado en un proyecto nuevo

```
copy-web ──┐
           ├──> frontend-builder ──> responsive-qa ──┐
ui-designer┘                                          ├──> deploy-check
           seo-tecnico ─ performance-cwv ─ a11y ──────┘
```

1. **Antes de maquetar**: `copy-web` (texto real, no lorem ipsum) y `ui-designer` (tokens).
   Maquetar con texto de relleno obliga a rehacer el layout cuando llega el texto de verdad.
2. **Maquetación**: `frontend-builder`.
3. **Revisión**: `responsive-qa`, `accesibilidad-a11y`, `seo-tecnico` y `performance-cwv`.
   Son independientes entre sí; puedes lanzarlos en paralelo en un mismo mensaje.
4. **Publicación**: `deploy-check`.

En un sitio que ya existe, entra directamente por la auditoría que toque.

## Reglas comunes a todos

- Las convenciones de stack y código están en el `CLAUDE.md` de la raíz; los agentes las heredan.
- Los agentes de auditoría (`responsive-qa`, `seo-tecnico`, `performance-cwv`,
  `accesibilidad-a11y`, `deploy-check`) **diagnostican con evidencia** —
  comando ejecutado, captura, archivo:línea — y no marcan como correcto nada que no hayan
  comprobado. Si quieres que además arreglen, pídelo explícitamente.
- Ninguno inventa datos del cliente (precios, plazos, teléfonos, cifras): los marca
  como pendientes de confirmar.

## Cómo editar o añadir agentes

Cada archivo es Markdown con frontmatter YAML: `name`, `description`, `tools`, `model`.
La `description` es lo que decide si Claude lo elige solo, así que escríbela en términos de
cuándo usarlo. `tools` restringe lo que puede hacer (los de auditoría no necesitan `Write`
para diagnosticar, pero lo llevan para guardar informes y capturas).
Para crear uno nuevo también sirve el comando `/agents`.
