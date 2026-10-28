# Agentes especializados en desarrollo web

Trece agentes en `.claude/agents/`. Claude los invoca solo cuando la tarea encaja con su
`description`, o puedes pedirlos por nombre: «usa el agente `seo-tecnico` en /clinica-dental».

## Construcción

| Agente | Para qué |
|---|---|
| `cms-contenido` | Modela el contenido antes de maquetar: colecciones de Astro, CPTs y bloques de WordPress, campos, taxonomías. Prueba las plantillas con contenido real y extremo |
| `ui-designer` | Sistema de diseño (color, tipografía, espaciado), jerarquía visual, estados de componentes, crítica de UI |
| `copy-web` | Titulares, propuesta de valor, CTAs, microcopy de errores y estados vacíos, metadatos |
| `frontend-builder` | Maquetación de páginas, secciones y componentes. Detecta el stack del proyecto |
| `animaciones-motion` | Animaciones e interacciones: scroll, hover, transiciones de página, parallax, carruseles. CSS moderno antes que librerías |
| `formularios-integraciones` | Formularios de punta a punta: validación, antispam, envío real al destino, RGPD |

## Revisión y lanzamiento

| Agente | Para qué |
|---|---|
| `responsive-qa` | Abre el sitio con Playwright y prueba de 320 a 1920 px |
| `accesibilidad-a11y` | WCAG 2.2 AA y EAA: teclado, contraste, lectores de pantalla |
| `seo-tecnico` | Metadatos, Schema.org, sitemap, canónicas, hreflang, SEO local |
| `performance-cwv` | Lighthouse, LCP/INP/CLS, imágenes, fuentes, bundle |
| `analitica-tracking` | GA4, GTM, eventos de conversión, píxeles y consentimiento de cookies |
| `deploy-check` | Última puerta antes de publicar: build, enlaces, formularios, secretos, RGPD |
| `migracion-relanzamiento` | Sustituir una web o cambiar de dominio sin perder posiciones: inventario de URLs y mapa de 301 |

Los seis de construcción heredan el modelo de la sesión; los de revisión van con `sonnet`.

## Orden recomendado

**Sitio nuevo**

```
cms-contenido ─ copy-web ─ ui-designer      (modelo, texto y tokens)
                    ↓
            frontend-builder                 (maquetación)
                    ↓
   animaciones-motion ─ formularios-integraciones
                    ↓
 responsive-qa · accesibilidad-a11y · seo-tecnico · performance-cwv · analitica-tracking
                    ↓
              deploy-check
```

1. **Antes de maquetar**: modelo de contenido, texto real y tokens. Maquetar con lorem ipsum
   y a mano obliga a rehacer la plantilla cuando llega el contenido de verdad.
2. **Maquetación**, y después animaciones y formularios sobre una base que ya funciona.
3. **Revisión**: los cinco son independientes entre sí, puedes lanzarlos en paralelo en un
   mismo mensaje.
4. **Publicación**: `deploy-check`.

**Rediseño de una web que ya existe**: empieza por `migracion-relanzamiento` (inventario de
URLs y tráfico actual) **antes** de diseñar nada, o el mapa de redirecciones se hará tarde y
mal. En un sitio en vivo que solo hay que arreglar, entra directamente por la auditoría que
toque.

## Quién hace qué cuando se solapan

- `animaciones-motion` **construye** la animación; `performance-cwv` **mide** si sale cara.
  Si una animación rompe el LCP, el primero lo avisa con el número.
- `ui-designer` decide tokens y jerarquía; `frontend-builder` los implementa. El diseñador
  no maqueta páginas enteras y el maquetador no inventa paletas.
- `formularios-integraciones` monta el formulario; `copy-web` escribe sus etiquetas y errores;
  `analitica-tracking` mide su conversión.
- `seo-tecnico` ve el SEO del sitio tal como está; `migracion-relanzamiento` se ocupa del
  SEO que se arriesga al cambiar de URLs.

## Reglas comunes a todos

- Las convenciones de stack y código están en el `CLAUDE.md` de la raíz; los agentes las heredan.
- Los agentes de revisión **diagnostican con evidencia** — comando ejecutado, captura,
  archivo:línea, petición de red — y no marcan como correcto nada que no hayan comprobado.
  Si quieres que además arreglen, pídelo explícitamente.
- Ninguno inventa datos del cliente (precios, plazos, teléfonos, cifras): los marca
  como pendientes de confirmar.
- Nada de accesos ni claves inventadas: lo que requiera acceso al hosting, al DNS o a las
  cuentas de Google/Meta del cliente se reporta como pendiente, no como hecho.

## Cómo editar o añadir agentes

Cada archivo es Markdown con frontmatter YAML: `name`, `description`, `tools`, `model`.
La `description` es lo que decide si Claude lo elige solo, así que escríbela en términos de
**cuándo** usarlo. `tools` restringe lo que puede hacer. Para crear uno nuevo también sirve
el comando `/agents`.

Cuidado con multiplicarlos: si dos agentes tienen descripciones que se pisan, Claude elige
peor. Antes de añadir el catorce, comprueba que ninguno de los trece ya lo cubre.
