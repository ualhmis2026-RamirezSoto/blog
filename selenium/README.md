# Tests Selenium IDE — Blog del equipo (Sesión 04 / parte 1 de Sesión 09)

Suite de tests de aceptación que verifica que el blog del equipo cumple
todos los requisitos del enunciado de la **Sesión 04**:

- Página principal (`index.html`) con título, descripción del equipo,
  listado de entradas con título y fecha, enlaces a páginas individuales
  de los miembros y enlace a contacto.
- Entradas del blog renderizadas como HTML (desde Markdown vía Jekyll).
- Páginas individuales (Bruno, Soto) con nombre, avatar y enlace a GitHub.
- Formulario de contacto con campos nombre / correo / asunto / mensaje,
  validación HTML5 y submit a Formspree.

## Cómo ejecutarlos

### Contra el blog corriendo en local (Jekyll)

1. Arrancar Jekyll en el directorio del blog:
   ```
   cd ../          # estar en hmis/blog
   bundle exec jekyll serve
   ```
   Por defecto Jekyll sirve en `http://localhost:4000/`.

2. Abrir **Selenium IDE** (extensión de Chrome o Firefox).
3. *Open an existing project* → seleccionar `blog-tests.side`.
4. Verificar que la base URL es `http://localhost:4000` (es la del fichero).
5. Pulsar **Run all tests**.

### Contra el blog desplegado en GitHub Pages o Azure Static Web Apps

1. Abrir el proyecto en Selenium IDE.
2. Cambiar la **base URL** (campo arriba a la izquierda) a la URL pública,
   por ejemplo `https://ualhmis2026-equipo.github.io/blog/`.
3. **Run all tests**.

> No hace falta tocar los test cases: todos los `open` usan rutas relativas
> (`/`, `/bruno.html`, etc.).

## Listado de test cases

| ID | Test | Qué verifica |
|---|---|---|
| INDEX-01 | Titulo del blog visible | `<title>` y `<h1>` de la home |
| INDEX-02 | Descripcion del equipo (Sobre nosotros) | Sección "Sobre nosotros" y párrafo descriptivo |
| INDEX-03 | Listado de entradas con titulo y fecha | Cards de posts con `h3` enlazado y texto "Publicado el" |
| INDEX-04 | Enlaces a paginas individuales de miembros | Anchors a `bruno.html` y `soto.html` |
| INDEX-05 | Enlace a la pagina de contacto | Anchor a `contacto.html` que navega correctamente |
| POST-01 | Abrir una entrada desde index | Click en post abre página con contenido y botón volver |
| MIEMBRO-01 | Pagina personal de Bruno | h1, avatar, enlace GitHub, aficiones, volver al inicio |
| MIEMBRO-02 | Pagina personal de Soto | Análogo para Soto |
| CONTACTO-01 | Formulario con 4 campos visibles | inputs nombre/correo/asunto, textarea mensaje, action Formspree |
| CONTACTO-KO-01 | Envio con campos vacios (HTML5) | `validationMessage` no vacío al pulsar enviar |
| CONTACTO-KO-02 | Email mal formado (HTML5) | `validationMessage` en el campo correo |
| CONTACTO-OK | Rellenar todos los campos | `form.checkValidity() === true` (sin enviar) |
| NAV-01 | Volver al inicio desde contacto | Link "Volver al inicio" navega a la home |

## Notas

- Los tests **no envían** el formulario al servidor de Formspree
  (`CONTACTO-OK` solo comprueba `checkValidity()` por JavaScript). Esto evita
  consumir cuota del servicio y generar emails de prueba.
- Los mensajes HTML5 (`validationMessage`) varían según navegador e idioma,
  por eso los asserts comparan con "no vacío" en vez de texto exacto. Si tu
  navegador está en español, los textos serán "Rellena este campo",
  "Introduce una dirección de correo electrónico", etc. (§4.2 del enunciado
  de Sesión 09).
- Si pruebas contra GitHub Pages con repo en subcarpeta
  (`/<repo>/bruno.html`), Jekyll ya genera los enlaces con `relative_url`
  asi que las rutas absolutas en los tests pueden fallar; ajusta la base URL
  para incluir el path del repo.
