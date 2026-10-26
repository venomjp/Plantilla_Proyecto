---
title: Plantilla de proyectos
description: Plantilla reutilizable para generar proyectos con Documentación y página web integrada
---
<img src="/favicon.png" 
     alt="Mi icono" 
     style="width: 128px; height: 128px; display: block; margin: 0 auto;">

# Ubicación, nomenclatura y ruta de archivos y carpetas

* La página HOME
    * Está en la raiz de pages/
    * Ahora se llama index.md
    * Genera la ruta -> /
* Las páginas de nivel 1 
    * Se encuentran en la raiz de pages/ 
    * 01-nombre.md
    * Generan una ruta -> /nombre/
* Las páginas de nivel 2
    * Se necesita una carpeta con el nombre de la página de nivel 1 padre (incluyeno el prefijo): pages/01-nombre/
    * Las páginas de nivel 2 se crean en la carpeta anterior (01-nombre)
    * Nomenclatura  -> 01-nivel2.md
    * La numeración vuelve a empezar dentro de la carpeta 01-nivel2.md
    * Generan una ruta -> /nombre/nivel2/
* Las páginas de nivel 3
    * Se necesita una carpeta con el nombre de la página de nivel 2 padre (incluyendo el prefijo): pages/01-nombre/01-nivel2
    * Las páginas de nivel 3 se crean en la carpeta anterior (01-nivel2)
    * Nomenclatura  -> 01-nivel3.md
    * La numeración vuelve a empezar dentro de la carpeta 01-nivel2
    * Generan una ruta -> /nombre/nivel2/nivel3

# Reglas

* Toda carpeta necesita su página: nombre.md o index.md, pero no puede tener las dos.
* Nombres en kebab-case: los nombres deben escribirse en minúsculas, se puede utilizar el guión para separar palabras.
* Los números son únicos por nivel, dentro de cada carpeta la numeración reinicia en 01
* Numera de 10 en 10 (10-, 20-, 30-, ..) para poder insertar una página entre dos sin renombrar las siguientes.
