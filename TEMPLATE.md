# TEMPLATE

Manual de la plantilla de proyectos: un fork de [eleventy-libdoc](https://github.com/ita-design-system/eleventy-libdoc) con estructura de páginas y bitácora (blog) propia, a la que se le ha añadido una carpeta en la que se distribuyen los recursos del proyecto.

- Qué se ha cambiado respecto a LibDoc: [PATCHES](PATCHES.md).
- Este archivo no se publica en la web (lo ignora `eleventy.project.js`).

Hay tres niveles de repositorio:

```text
LibDoc (upstream) -> Plantilla_Proyecto ("fork") -> Proyecto X (web, docs...)
```

---

## 1. Requisitos

Partimos de la base de que el ordenador que se utilice deberá tener instalado previamente:
* [Git](https://git-scm.com/)
  * `.gitattributes` debe contener: `* text=auto eol=lf`. 
* [GitHub Desktop](https://desktop.github.com/)
* [Visual Studio Code](https://code.visualstudio.com/). Algunas Extensiones interesantes:
  * Markdown All in One
  * GitHub Actions
  * GitHub Copilot Chat
  * Git History
  * Liquid
* [NodeJS](https://nodejs.org/)
  * Node 24 (`.nvmrc`) y npm. Comprobar con `node -v`.
  * `.nvmrc` debe contener una sola línea: `24`.

- Los repositorios viven en una carpeta local normal, **nunca** dentro de Google Drive ni OneDrive (corrompen `.git`). La copia de seguridad es GitHub o el servidor.
- El nombre de la carpeta del proyecto **es** el prefijo de rutas de la web (ver sección 6). Sin espacios ni tildes.

---

## 2. Crear un proyecto nuevo

No uses **"Download ZIP"**: se pierde la historia y con ella las actualizaciones. Clona y cambia el remoto.

Entra en la carpeta donde quieras crear el proyecto y ejecuta:

```powershell
git clone https://github.com/venomjp/Plantilla_Proyecto.git Mi-Proyecto
cd Mi-Proyecto
git remote rename origin template
```

Si el proyecto va a tener repositorio propio en GitHub, crea uno **vacío** con el mismo nombre (`Mi-Proyecto`, sin README ni .gitignore) y:

```powershell
git remote add origin https://github.com/venomjp/Mi-Proyecto.git
git push -u origin main
```

Después, en GitHub Desktop: File > Add local repository, y elige la carpeta. Si el proyecto es solo para tu servidor, puedes no crear `origin`.

* **Primer arranque:**

```powershell
npm ci
npm audit
npm audit fix
npm run clean
npm run start
```
***npm ci*** - (clean install) hace una instalación limpia de las dependencias (borra el directorio `node_modules` y lo vuelve a crear).
***npm audit*** - analiza las dependencias de proyecto
***npm audit fix*** - actualiza automáticamente las dependencias vulnerables de tu proyecto a versiones seguras sin romper la compatibilidad de código.
***npm run clean*** - borra la carpeta `_site/` y la reconstruye desde cero.
***npm run start*** - arranca un servidor local de desarrollo con recarga automática para comprobar que todo funciona.

* **Buenas prácticas:**
Usa siempre `npm ci` (Clean Install) en un clon nuevo: respeta `package-lock.json` instalando exactamente las versiones que se encuentran en él y deja los mismos paquetes en todos tus ordenadores. Si el directorio node_modules existe, lo borra por completo y lo descarga de cero. `npm install` solo cuando cambies dependencias a propósito (que suele definir rangos de versiones, por ejemplo, `^18.2.0`). Si encuentra versiones compatibles más recientes, las descarga y actualiza el archivo `package-lock.json` para reflejar los cambios, después debes hacer un commit en Git que incluya tanto `package.json` como `package-lock.json` actualizado para reflejar los cambios.

La web de desarrollo está en `http://localhost:8080/Mi-Proyecto/` (incluye el prefijo).

---

## 3. Cambios básicos al empezar

* Las indicaciones paso a paso de los cambios que hay que introducir están en el archivo [PATCHES](PATCHES.md).

| Archivo | Qué cambiar |
|---|---|
| `settings.json` | `siteTitle`, `siteDescription`, `author`, `blogTitle`, `blogDescription`, `customLinks` (vacío por defecto: `{ "url": "...", "text": "..." }`), `lang`. Dejar `"blogSlug": "bitacora"`. No añadir `editThisPageRootUrl` (oculta el botón de editar). No hace falta `htmlBasePathPrefix` (ver sección 6). |
| `favicon.png` | Sustituir por la imagen del proyecto (misma ruta y nombre). |
| `README.md` | Sustituir por el README del proyecto. |
| `pages/` | Borrar los ejemplos y escribir el contenido (sección 4). |
| `resources/` | Archivos del proyecto (sección 5). |

JSON no admite comentarios: la documentación de las claves de `settings.json` está en [la web de LibDoc](https://eleventy-libdoc.netlify.app/configuration/).

No hace falta tocar nada más: `.eleventy.js`, `_data/`, `_includes/` y `core/` son de LibDoc y no deben modificarse, cada versión nueva de LibDoc actualizará estos archivos.

---

## 4. Contenido: carpeta `pages/`

La URL, el menú y las migas de pan salen de la ruta del archivo. No se escribe `permalink`, `layout`, ni `eleventyNavigation`.

* Se pueden definir `tags` en el front matter, tanto en páginas como en posts (para filtrar en la bitácora, por ejemplo).

```text
tags:
    - tag1
    - tag2
```
* La estructura de carpetas y archivos en `pages/` define la jerarquía de la web.

```
pages/
  pages.11tydata.js          (no tocar)
  index.md                   -> / (home)
  10-estructura.md           -> /estructura/
  20-guia.md                 -> /guia/
  20-guia/
    10-primeros-pasos.md     -> /guia/primeros-pasos/
    10-primeros-pasos/
      10-detalle.md          -> /guia/primeros-pasos/detalle/
  bitacora/
    bitacora.json            (no tocar)
    primer-post.md           -> /bitacora/primer-post/
    segundo-post.md           -> /bitacora/segundo-post/
```

* **Reglas:**

1. **Prefijo numérico**: `10-`, `20-`... Marca el orden en el menú y en VSCode, y no aparece en la URL. Numera de 10 en 10 para poder insertar páginas sin renombrar. El número no puede repetirse en el mismo nivel. No hace falta numerar los posts de la bitácora.
2. La numeración se reinicia en cada carpeta (los hijos no heredan el número del padre).
3. **Nombres en minúsculas con guiones** (`primeros-pasos`, no `primerosPasos`).
4. **Una página con hijos** lleva al lado una carpeta con el mismo nombre: `20-guia.md` y `20-guia/`. También vale un `index.md` dentro de la carpeta, pero no ambas formas a la vez.
5. **Toda carpeta necesita su página.** Si falta, la build para con el mensaje "Falta la página de la carpeta...".
6. Mover un archivo a otra carpeta cambia su padre y su URL automáticamente. Renombrar el título no rompe nada.

* **Front matter de una página (cabecera):**

```yaml
---
title: Primeros pasos
description: Resumen corto de la página
---
```

* **Front matter de un post (en `pages/bitacora/`):**

Se incluye la fecha en la cabecera, servirá para ordenar los posts y mostrarla en la web. La fecha debe escribirse en formato `YYYY-MM-DD` (ISO 8601).

```yaml
---
title: Primer post
description: Resumen corto del post
date: 2026-10-15
---
```

* **Enlaces entre páginas:** 

Usa la ruta del archivo `.md` relativa a la página actual (por ejemplo `[ver](../10-estructura.md)`). El plugin de Eleventy la traduce a la URL final, aunque renombres o muevas cosas.

---

## 5. Archivos del proyecto: carpeta `resources/`

```
resources/
  files/          audio, imagen, texto, vídeo, scripts...
  source-code/    código fuente de programas
  3d-design/      stl, 3mf, gcode...
```

Los archivos viven aquí, organizados como quieras, **sin copias**. Por defecto no se publican en la web.

Para publicar un archivo, añade `.pub` antes de la extensión:

```
resources/files/manuales/guia.pub.pdf   ->   /files/manuales/guia.pdf
```

Se enlaza sin el `.pub`: `[guía](/files/manuales/guia.pdf)`. Para dejar de publicarlo, quita el `.pub`. Si añades o renombras archivos con `npm start` en marcha, reinicia el servidor.

Ojo: "no publicado" solo significa que no sale en la web. Si el repo es público en GitHub, los archivos siguen siendo públicos allí. Los archivos grandes de `3d-design/` engordan el repo (valora Git LFS).

---

## 6. Publicar

El prefijo de rutas (`/Mi-Proyecto/`) se calcula así, por este orden:

1. Variable de entorno `PATH_PREFIX`.
2. `htmlBasePathPrefix` de `settings.json` (no se usa en la plantilla).
3. El nombre de la carpeta de trabajo.

Por eso el nombre del repo en GitHub, el nombre de la carpeta en el servidor y el prefijo coinciden. Para probar otro prefijo en local:

```powershell
$env:PATH_PREFIX = "/otro/"; npm run build
Remove-Item Env:PATH_PREFIX
```

### GitHub Pages

1. **En cada repositorio nuevo, a mano**: Hay que activar las Pages en `Settings > Pages > Build and deployment > Source: **GitHub Actions**`. Es un ajuste del repositorio, no de los archivos: no viaja con los clones ni con las copias de la plantilla. Mientras no esté activado, el job `build` termina pero el job `deploy` falla. Una vez activado, se mantiene en ese repo. El workflow solo publica si el repositorio es público: en uno privado los jobs aparecen como omitidos (skipped) y no hay error. La visibilidad se comprueba en cada ejecución, así que no hay que tocar el archivo cuando cambia. Al hacer público un repo, activa Pages (ajuste de arriba) y relanza la build desde Actions > Deploy to GitHub Pages > Run workflow, porque cambiar la visibilidad no dispara ninguna ejecución por sí solo.
2. Cada push a `main` ejecuta `.github/workflows/build.yml`, que construye con `PATH_PREFIX=/<nombre-del-repo>/` y publica `_site/`.
3. La web queda en `https://<usuario>.github.io/<repo>/`.

`_site/` nunca se sube al repo: se genera en la build.

### Servidor propio (Proxmox, LXC y Caddy)

El `_site` que genera `npm run build` es el mismo que sirve GitHub Pages: no hace falta nada específico para el servidor. Se construye en una carpeta con el nombre del proyecto (así el prefijo coincide) y se copia `_site/` a la ruta que Caddy sirve como `/<carpeta>/`. Si la carpeta de build no se llama como esa ruta, construye con `PATH_PREFIX=/<ruta-de-caddy>/`.


***Una web privada se publica solo aquí. No uses GitHub Pages para documentación privada: las webs de Pages son públicas.***

---

## 7. Actualizar un proyecto con los cambios de la plantilla

La plantilla ya incluye las actualizaciones de LibDoc y los parches. En el proyecto:

```powershell
git fetch template
git merge template/main -X ours
npm ci
npm run build
```

`-X ours` hace que, si el proyecto y la plantilla tocan la misma línea (típicamente `README.md`, `settings.json` o el contenido de `pages/`), gane la del proyecto. Es seguro porque en un proyecto solo cambias contenido, no el código de LibDoc. Los archivos que no has tocado se actualizan solos. Si la plantilla añade claves nuevas a `settings.json`, pueden quedarse sin aplicar: compara con `git diff template/main -- settings.json` y revisa también `PATCHES.md`.

---

## 8. Mantener la plantilla: actualizar desde LibDoc

Se hace en el repo de la plantilla, no en los proyectos. Los remotos son `origin` (tu repo) y `upstream` (LibDoc).

1. Preparar y ver qué hay nuevo:

```powershell
git checkout main
git pull
git fetch upstream --tags
git tag --sort=-v:refname | Select-Object -First 5
git log --oneline <versión-actual>..upstream/main
```

2. Elegir destino. Lo normal es la última **etiqueta**: es una versión cerrada. Excepción: si `main` va por delante de la etiqueta, mira lo que cambia:

```powershell
git diff --stat <etiqueta> upstream/main
```

Si solo cambian `package.json` y `package-lock.json` (mantenimiento de dependencias), puedes usar `upstream/main`. Si toca código (`_data`, `_includes`, `core`), usa la etiqueta.

3. Fusionar:

```powershell
git merge <etiqueta>      # o: git merge upstream/main
```

4. Resolver conflictos con `PATCHES.md` en la mano (sección 1 = parches, sección 2 = archivos tuyos):

- **Archivos tuyos** (`settings.json`, `README.md`, `.gitignore`): `git checkout --ours <archivo>`. Revisa a mano si upstream añadió algo que te interese.
- **Parches** (`.eleventy.js`, `libdocConfig.js`, `libdocMessages.json`, los dos CSS): quédate con la versión de upstream y reaplica el parche tal como está en `PATCHES.md`. Si upstream ya corrigió el bug de iconos, elimina ese parche.
- **`package.json`**: `git checkout --theirs package.json` y reaplica los tres bloques de `PATCHES.md`, solo con líneas añadidas.
- **`package-lock.json`**: `git checkout --theirs package-lock.json` y luego `npm install` (lo adapta al `package.json` final).
- **Archivos que borraste** (`hello-*.md`, `sandboxes/`, logos) y que upstream ha modificado: `git rm <archivo>`.

5. Comprobar:

```powershell
git diff upstream/main -- package.json        # solo líneas con +
npm install
npm audit                                     # NO usar --force
npm run clean
npm run build
npm start
```

Revisa en la web: iconos, acordeones, imágenes, menú, blog, botón "volver", pie con la versión nueva de LibDoc. En `npm audit`, los avisos de la cadena de Eleventy (`braces`, `chokidar`, `nunjucks`...) no tienen arreglo todavía y no afectan a la web publicada; `audit fix --force` propone versiones antiguas de Eleventy y rompe la plantilla.

6. Registrar y publicar:

- Actualizar `PATCHES.md`: versión base, fecha y cualquier parche cambiado.
- `git commit` (si el merge quedó abierto), push, y comprobar el despliegue en GitHub Pages.
- Después, cada proyecto ejecuta la sección 7 cuando le toque.

---

## 9. Problemas conocidos

| Síntoma | Causa y solución |
|---|---|
| `npm` no se reconoce | Node no instalado o terminal abierta antes de instalarlo. Reabrir el terminal; `node -v` debe dar `v24`. |
| `npm error ENOENT ... package.json` | Terminal fuera de la carpeta del proyecto. |
| `git rev-parse --show-toplevel` devuelve tu carpeta de usuario | La carpeta del proyecto no es un repo (falta `.git`). Reclonar. |
| La build para con "Falta la página de la carpeta..." | Una carpeta de `pages/` sin su `.md` al lado (o `index.md`). |
| `Node does not exist: ...` en las migas de pan | Lo mismo, con una versión antigua de `pages.11tydata.js`. |
| Iconos que no cargan en GitHub Pages | Prefijo mal calculado o parches de los CSS sin aplicar (ver `PATCHES.md`). |
| Avisos `LF will be replaced by CRLF` | Falta `.gitattributes` (`* text=auto eol=lf`). |
| `npm ci` falla en la CI | `package-lock.json` desincronizado: `npm install` y commitear el lock. |
| La CI falla en "Setup Node" | `.nvmrc` vacío o ausente: debe contener `24`. |
| El job `deploy` falla con un error de Pages | Pages no está activado en ese repo (ver sección 6). |
| El botón "volver" no sale | No hay JavaScript, o el pie de LibDoc cambió (selector `main footer nav` en `eleventy.project.js`). |
