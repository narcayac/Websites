# Websites — agentes de Claude Code para desarrollo web

Monorepo de proyectos web de la agencia. Cada sitio vive en su propia carpeta de primer
nivel, y el repositorio incluye **trece agentes especializados** de Claude Code que cubren
el ciclo completo de una web: del modelo de contenido a la revisión previa a publicar.

Las convenciones de código y de stack están en [`CLAUDE.md`](CLAUDE.md).

## Cómo se usan

Los agentes están en `.claude/agents/`. Claude los invoca solo cuando la tarea encaja con su
`description`, o puedes pedirlos por nombre: «usa el agente `seo-tecnico` en /clinica-dental».

Al trabajar dentro de este repositorio están disponibles sin instalar nada, también en las
subcarpetas de cada proyecto.

### Usarlos en otros proyectos

Van empaquetados como plugin, así que se pueden instalar en cualquier máquina y cualquier
proyecto:

```
/plugin marketplace add narcayac/Websites
/plugin install web-agents@narcayac
```

Se actualizan con `/plugin update web-agents`. El plugin apunta a esta misma carpeta
`.claude/`, así que no hay copias duplicadas que se desincronicen: se editan los agentes
aquí y quien los tenga instalados recibe el cambio.

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
el comando `/agents`. Si añades uno, no lo pongas en `agents/` como README ni como notas:
esa carpeta solo admite agentes, y cualquier otro `.md` se cargaría como un agente fantasma.

Cuidado con multiplicarlos: si dos agentes tienen descripciones que se pisan, Claude elige
peor. Antes de añadir el catorce, comprueba que ninguno de los trece ya lo cubre.
