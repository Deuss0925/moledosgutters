# moledosgutters.com

Sitio web de **Moledo's Gutter Solutions** — Greater Raleigh y el Triangle, NC.

Es un sitio **estático**: no hay build, no hay framework, no hay dependencias.
Netlify publica esta carpeta tal cual. Si editas el HTML y haces push, el sitio
se actualiza solo en un par de minutos.

---

## Cómo publicar un cambio

```bash
git add .
git commit -m "describe el cambio"
git push
```

Eso es todo. Netlify detecta el push y publica. No hay que correr ningún comando
de deploy ni entrar al panel.

---

## Qué hay en cada archivo

| Archivo | Qué es |
|---|---|
| `index.html` | La página principal. Todo el sitio (HTML, CSS y JS) está aquí dentro. |
| `free-estimate.html` | Landing de captura de leads, se ve en `/free-estimate`. |
| `img/` | Las fotos. Están en formato webp y ya vienen optimizadas. |
| `netlify.toml` | Configuración de Netlify: qué se publica, redirecciones y cache. |
| `404.html` | Página de error con la marca del sitio. |
| `robots.txt`, `sitemap.xml` | Para los buscadores. |

---

## Cosas que NO hay que romper

Antes de tocar estas, pregunta:

- **El endpoint del formulario:** `https://formspree.io/f/xdarlrar`. Aparece dos
  veces en `index.html` (formulario del hero y formulario de contacto). Si se
  cambia o se borra, **dejan de llegar los leads**.
- **El teléfono (786) 499-6534** aparece en varios lugares: nav, hero, garantía,
  contacto, footer, los mensajes de error del formulario y el JSON-LD del final.
  Si cambia el número, hay que cambiarlo en todos.
- **El JSON-LD** (los dos bloques `application/ld+json` al final del archivo) es
  lo que Google lee para las búsquedas locales. Si cambias servicios, garantía o
  las preguntas del FAQ en la página, cámbialos también ahí para que coincidan.
- **El campo `source`** de cada formulario (`Homepage hero form` y
  `Homepage contact form`) es lo que permite saber de qué formulario vino cada
  lead. Si se borra, todos los leads llegan iguales.

---

## Si le pides los cambios a una IA (Claude, Codex)

Funciona bien porque todo el sitio es un solo archivo. Un par de cosas que
conviene decirle para que no la riegue:

> El sitio es HTML estático en un solo archivo, `index.html`, con el CSS y el JS
> embebidos. No lo conviertas a React ni le agregues dependencias ni build.
> No toques el endpoint de Formspree ni los campos ocultos del formulario.
> Si cambias contenido de servicios, garantía o FAQ, actualiza también los
> bloques JSON-LD del final para que digan lo mismo.

Y después de cualquier cambio, antes de hacer push, ábrelo en el navegador y
revisa el celular además de la computadora — el sitio tiene menú hamburguesa,
acordeón de FAQ y animaciones que conviene ver funcionando.

---

## Infraestructura

- **Hosting:** Netlify — proyecto `moledosgutters`
- **Dominio:** `moledosgutters.com`, DNS en **Porkbun** (no en Netlify DNS)
  - ALIAS del apex → `apex-loadbalancer.netlify.com`
  - CNAME de `www` → `moledosgutters.netlify.app`
- **Formulario:** Formspree (endpoint `xdarlrar`) → llega al Gmail del negocio
- **Email:** `hello@moledosgutters.com`, reenviado a Gmail

Ojo: el hosting, el DNS y el formulario están en **tres cuentas distintas**. Un
cambio de dueño en una no mueve las otras.

---

## Archivos internos

Las notas de trabajo (`_TRASPASO-A-ALFREDO.md`, respaldos `_backup-*`, scripts `.bat`)
**no van en este repo**: viven fuera de la carpeta que se publica, en
`D:\Moledos Gutters\Automations\lead-gen\_interno\`.

Como Netlify publica toda la carpeta (`publish = "."`), cualquier archivo que quede aquí
dentro queda accesible desde internet. Por eso `netlify.toml` además devuelve 404 para
`/_*`, `*.md` y `*.bat`: es el cinturón por si algo se cuela.
