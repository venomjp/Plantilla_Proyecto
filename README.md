# README del Proyecto X

![Imagen en el README del proyecto X](./assets/imagenReadme.jpg)

# Cómo usar esta plantilla para crear otros proyectos

Esta plantilla es un punto de partida para crear proyectos con documentación elegante y adaptable. Para usar esta plantilla, sigue estos pasos:

1. *Crear un **nuevo repositorio** en GitHub con el nombre del proyecto*
2. ***Clonar el repositorio nuevo** en tu ordenador usando GitHub Desktop.*
3. ***Descargar esta [plantilla para los proyectos](https://github.com/venomjp/Plantilla_Proyecto)** desde GitHub.*
   * Borrar la carpeta .git de la plantilla descargada para desvincularla de su repositorio original.
   * Mover el contenido de la plantilla descargada a la carpeta del nuevo repositorio que has clonado en tu ordenador y borrar la carpeta vacía de la plantilla descargada.
   * Modificar el pathPrefix en el archivo `settings.json` con el nombre del nuevo repositorio.
4. **Abre el proyecto con Visual Studio Code.**
5. ***Instala 11ty** en el proyecto*
   * Abre la terminal integrada de VSCode (preferiblemente PowerShell 7)
   * **`npm install @11ty/eleventy@latest`** permite instalar Eleventy en el proyecto.
   * **`npm audit fix`** corrige vulnerabilidades de seguridad.
   * **`npm i -D rimraf`** instala rimraf, una herramienta para eliminar archivos y carpetas de forma segura.
6. *Comprobar que el **proyecto funciona en modo local***
   * **`npm run start`** inicia el servidor de desarrollo de Eleventy.
   * Puedes ver la página en la dirección http://localhost:8080/
7. ***Configurar GitHub Pages** para publicar la página web*
   * **Settings > Pages > Build and Deployment = GitHub Actions + Save** - permite generar la web automáticamente cada vez que se suba un cambio al repositorio.
8. ***Subir el proyecto a GitHub***
   * **`npm run clean`** limpia la carpeta de salida y la de cache
   * **`npm run build-ghpages`** - genera la web y la sube a GitHub Pages
   * Subir el proyecto ya sea a través de VSCode o con GitHub Desktop.
9. ***Continuar realizando los cambios necesarios en el proyecto***
   1. **package.json**
      * Nombre del proyecto = nombre del repositorio
      * Versión del proyecto
      * Descripción del proyecto
   2. **settings.json**
      * Título del sitio web
      * Descripción del sitio web
      * Autor
      * Favicon
      * Título del blog
      * Descripción del blog
      * Custom Links (enlaces personalizados en el menú de navegación)
      * PathPrefix (nombre del repositorio en GitHub)
      * hljsLanguages (lenguajes de programación para resaltar)
   3. **readme.md**
      * Modificar con la información relevante del proyecto
   4. **myfavicon.png**
      * Cambiar el favicon por uno personalizado (opcional)
10. **Contenido del proyecto**
   * **home.md**: página de inicio del proyecto
   * **x-<nombre>.md**: páginas de nivel 1 = secciones principales
   * **x-y-<nombre>.md**: páginas de nivel 2 = subsecciones
   * **x-y-z-<nombre>.md**: páginas de nivel 3 = sub-subsecciones
   * **bit-<nombre>.md**: entradas del blog (contenidas en la carpeta /pages/bitacora)

* Las plantillas escritas en markdown se encuentran en la carpeta **/pages**, y el contenido de cada página se puede modificar editando el archivo correspondiente.
* Las páginas web se generan en la carpeta
   * **home** está en **/_site/index.html**
   * **El resto de páginas** se generan en **/_site/pages/...** y se van organizando en subcarpetas según el nivel de la página (nivel 1, nivel 2, etc.)
* Una vez completados los cambios se puede empezar a generar la documentación.
* Es recomendable generar un área de trabajo en VSCode para facilitar la gestión de los archivos del proyecto.

---

### [Página del Proyecto](https://venomjp.github.io/Plantilla_Proyecto/)