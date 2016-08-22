# Tecnologías

## PHP

* Codecademy: https://www.codecademy.com/learn/php (interactivo, en inglés)
* PHP Ya: http://www.phpya.com.ar/ (en español)

## MySQL

* Codecademy: https://www.codecademy.com/learn/learn-sql (interactivo, en inglés)
* MySQL Ya: http://www.mysqlya.com.ar/ (en español)

## HTML

* Codecademy: https://www.codecademy.com/learn/web (HTML + CSS; interactivo, en inglés)
* HTML5 Ya: http://www.htmlya.com.ar/html5/ (en español)

## CSS

* Codecademy: https://www.codecademy.com/learn/web (HTML + CSS; interactivo, en inglés)
* CSS Ya: http://www.cssya.com.ar/ (en español)

## JavaScript

* Codecademy: https://www.codecademy.com/learn/javascript (interactivo, en inglés)
* JavaScript YA: http://www.javascriptya.com.ar/ (en español)

## jQuery

* Codecademy: https://www.codecademy.com/learn/jquery (interactivo, en inglés)

# Herramientas

## AMP Stacks (Apache + MySQL + PHP)

* XAMPP (Windows, Linux)
* MAMP (OS X)

## Editores de texto

* Sublime Text 3
* Atom

## IDEs

* Aptana (Eclipse)
* PHPStorm

## Pre-procesadores de archivos (SASS, LESS, JavaScript)

* Koala: http://koala-app.com/
* Prepros: https://prepros.io/
* Codekit: https://incident57.com/codekit/

## VCS (Version Control Software)

### Git

* Sitio oficial: http://git-scm.com
* Try Git (Code School): https://try.github.io/levels/1/challenges/4
* GitHub: http://www.github.com
* BitBucket: http://bitbucket.com
* GitHub Desktop: https://desktop.github.com/
* SourceTree: https://www.sourcetreeapp.com/
* SmartGit: http://www.syntevo.com/smartgit/

### SVN

* Sitio oficial: https://subversion.apache.org/
* Tutorial (Lihuen): https://lihuen.linti.unlp.edu.ar/index.php/C%C3%B3mo_usar_SVN
* Tortoise SVN: https://tortoisesvn.net/
* SmartSVN: http://www.smartsvn.com/

## Plugins

* Developer: https://github.com/Automattic/developer
* Airplane Mode: https://github.com/norcross/airplane-mode

---

# Recursos

## Codex

https://codex.wordpress.org/

Consiste en una wiki mantenida por la comunidad, y contiene documentación acerca del uso de las distintas APIs que ofrece WordPress. Parte de la misma está siendo reemplazada por las diferentes partes de WordPress Developer Resources, un nuevo portal para desarrolladores, especialmente lo referente a desarrollo de plugins, themes, y características internas del core que no están pensadas para ser reutilizadas en extensiones.

## WordPress Developer Resources

Nuevo portal para desarrolladores, que hizo su aparición durante 2015. Está dividido en tres partes:

1. **Plugin Handbook:** recomendaciones y buenas prácticas para el desarrollo de plugins. https://developer.wordpress.org/plugins/
2. **Theme Handbook:** recomendaciones y buenas prácticas para el desarrollo de themes. Actualmente no disponible, ya que se sigue trabajando en una primera versión. Está pensada como reemplazo de la sección [Theme Development](https://codex.wordpress.org/Theme_Development) del Codex, la cual se debe tomar como referencia hasta el lanzamiento.
3. **Code Reference:** contiene documentación específica acerca de funciones, hooks, clases y métodos. Está reemplazando progresivamente al Codex como referencia acerca de las características internas de WordPress. https://developer.wordpress.org/reference/

## Coding Standards

https://make.wordpress.org/core/handbook/best-practices/coding-standards/

Seguir epecificaciones estandarizadas para escribir código ayuda a prevenir errores comunes, mejora la legibilidad, simplifica modificaciones y colaboración entre desarrolladores, y en cierta manera garantiza que sea comprensible sin importar quién lo haya escrito.

## Sitios a seguir: noticias, desarrolladores y soporte

* **Make WordPress:** https://make.wordpress.org/
* **WordPress Tavern:** https://wptavern.com/
* **ManageWP:** https://managewp.org/
* **Smashing Magazine:** https://www.smashingmagazine.com/
* **A List Apart:** http://alistapart.com/
* **Tom McFarlin:** https://tommcfarlin.com/
* **Jeremy Felt:** https://jeremyfelt.com/
* **Ayuda WordPress:** http://ayudawp.com/

## Artículos útiles

* [Programar no es (tan) complicado](http://www.andrezrv.com/2015/10/15/programar-no-es-tan-complicado/)
* [Psicología del programador](http://www.andrezrv.com/2015/11/04/psicologia-del-programador/)
* [Some Thoughts On Writing Accessible Code](http://www.andrezrv.com/2014/05/09/thoughts-writing-accesible-code/)
* [More Thoughts on Writing Accessible Code](http://www.andrezrv.com/2014/08/07/thoughts-writing-accessible-code/)
* [The Single Biggest Mistake Programmers Make Every Day](https://medium.com/javascript-scene/the-single-biggest-mistake-programmers-make-every-day-62366b432308)

# Themes y Child Themes

## Estructura básica de un theme

Para tener un amplio conocimiento sobre todo lo que representa WordPress es necesario conocer cómo es la estructura de una página, para qué sirve cada uno de los archivos que son parte del theme a desarrollar y cómo es el funcionamiento interno para que este gestor de contenido nos muestre los post y algunos extras.

### Anatomía de un Theme de WordPress

![](http://i.imgur.com/IpmDtmT.png)

Los themes de WordPress se componen de una carpeta de archivos, cada uno de los cuales controla una parte específica del theme.

<div align="center">
![](http://i.imgur.com/rH3pngl.png)
</div>

### El ciclo

Es la parte más poderosa del theme de WordPress, se inicia con una consulta (que determina los mensajes y páginas a mostrar) y termina con un end while de PHP. Todo lo demás de ti, puedes específicar cómo se muestran los títulos, el contenido de cada post, los metadatos, campos personalizados y de los comentarios.

Se pueden configurar múltiples ciclos en una misma página.

<div align="center">
![](http://i.imgur.com/CSJIb4z.png)
</div>

---

## Jerarquía de ejecución y lectura de archivos (Template Hierarchy)

https://developer.wordpress.org/themes/basics/template-hierarchy/
https://wphierarchy.com/

WordPress usa la Cadena de Consulta (o query string) —información contenida dentro de cada URL del sitio— para decidir qué modelo o conjunto de las plantillas se utiliza para mostrar la página.

En primer lugar, WordPress se relaciona cada query string al tipo de consulta —es decir, decide qué tipo de página (una página de búsqueda, una página de categoría, la página de inicio, etc) se está solicitando.

Las plantillas son elegidas entonces —y el contenido de la página web se genera— en el orden sugerido por la jerarquía de plantillas de WordPress, dependiendo de la disponibilidad de las plantillas (templates) en el theme activo.

WordPress busca templates con nombres específicos en el directorio del theme y utiliza la primera coincidencia con el template listado bajo la consulta apropiada. Si WordPress no puede encontrar un template con el nombre correspondiente, pasa al siguiente nombre de archivo en el orden de jerarquía. Si WordPress no puede encontrar ningún template, se usará `index.php`, que es el único template obligatorio para construir un theme. 

### Ejemplos

Si un sitio está en `http://domain.com/` y un visitante visita el enlace de una página de categoría como `http://domain.com/category/your-cat/`, WordPress busca un template en el directorio del Tema actual que conincida con el ID de la categoría. Si el ID de la categoría es 4, WordPress busca un template llamado `category-4.php`. Si no aparece, después WordPress busca un template para una categoría genérica, `category.php`. Si este archivo tampoco existe, WordPress busca un template generico, `archive.php`. Si este tampoco aparece, WordPress vuelve al archivo principal del, `index.php`.

Si un visitante va a tu página de inicio en `http://domain.com/`, WordPress busca un template llamado `home.php` y lo usa para general la página pedida. Si `home.php` no aparece, WordPress busca un archivo llamado `index.php`, y lo usa para generar la página.

### Panorama visual

El siguiente diagrama muesta qué template es llamado para generar una página WordPress basado en la jerarquía de Templates de WordPress. 

![](http://i.imgur.com/yQdUcbz.png)

### La Jerarquía de Plantillas en detalle

Las siguientes secciones describen el orden en el cual son llamadas los archivos de plantillas por WordPress para cada uno de los tipos de consultas.

#### Página principal

1. front-page.php
2. home.php
3. index.php

#### Blog

1. home.php
2. index.php

#### Post

1. single.php 
2. index.php 

#### Página

1. La plantilla seleccionada desde el desplegable "Plantilla de la página (Page Template)" al editar la página.
2. page.php
3. index.php 

#### Categoría

1. La Plantilla de Categoría con una coincidencia de ID. Si el ID de la categoría fuera 6, WordPress buscaría `category-6.php`.
2. category.php
3. archive.php
4. index.php


#### Etiqueta (Tag)

1. La Plantilla de Etiqueta que coincida con el slug. Si el slug de la etiqueta fuera `sometag`, WordPress buscaría `tag-sometag.php`.
2. tag.php
3. archive.php
4. index.php

#### Autor

1. author.php
2. archive.php
3. index.php

#### Fecha (por archivo mensual)

1. date.php
2. archive.php
3. index.php 

#### Resultados de Búsqueda

1. search.php
2. index.php 

#### Error 404 (No encontrado)

1. 404.php
2. index.php 

#### Archivo adjunto

1. image.php 
2. attachment.php 
3. index.php 

---

## Templates tags y funciones básicas

En resumen, los template tags son funciones PHP que imprimen algún tipo de contenido de forma directa, y que están destinadas a mostrar diferentes piezas de información dentro de los templates de WordPress.

### Funciones básicas

`get_header()`

Incluye el archivo `header.php` del theme activo. Si se especifica un nombre como parámetro, se intentará incluir `header-{nombre}.php`.

Si el theme no contiene el archivo `header.php`, entonces se incluirá el header proveniente desde el archivo `wp-includes/theme-compat/header.php`.

`get_sidebar()`

Incluye el archivo `sidebar.php` del theme activo. Si se especifica un nombre como parámetro, se intentará incluir `sidebar-{nombre}.php`.

`get_footer()`

Incluye el archivo `footer.php` del theme activo. Si se especifica un nombre como parámetro, se intentará incluir `footer-{nombre}.php`.

`get_template_part()`

Permite cargar un template parcial dentro de otro, y además hace que sea fácil para un theme la reutilización de secciones de código. Llamamos tempate parcial a un archivo que no consiste en un template completo, sino en código que puede ser reutilizado por varios templates.

`get_search_form()`

Incluye el formulario de búsqueda del sitio. Intenta localizar el archivo `searchform.php`. Si no existe, entonces se mostrará el formulario de búsqueda predeterminado.

---

## ¿Cómo extender un theme a partir de un child theme?

Un child theme o theme hijo en WordPress es un theme que modifica o extiende la funcionalidad de otro theme, llamado “theme padre”. El theme hijo hereda la funcionalidad completa y los estilos del theme padre, y permite modificarlo o añadir nuevas funcionalidades de una manera inteligente sin tocar el código fuente del theme padre. Todos estos cambios que hagamos en el theme hijo prevalecen sobre el theme padre.

En muchas ocasiones compramos un theme premium porque nos gusta o nos encaja al 90%, y para completar ese 10% restante debemos plantearnos extender o modificar la funcionalidad del padre a través de un theme hijo.

Si hiciéramos estos cambios directamente sobre el theme principal, puede ocurrir que, al lanzarse una nueva versión del theme con nuevas mejoras o solución de bugs y/o vulnerabilidades, al instalarla se pierdan todos nuestros cambios.

#### Creando un theme hijo

Vamos a ver con un ejemplo práctico cómo extender el theme por defecto de WordPress a día de hoy: TwentySixteen. 

**1. Creamos la carpeta y los archivos del theme hijo**

Lo primero es crear una carpeta dentro de `wp-content/themes/` a la que llamaremos `twentysixteen-child`. Dentro de la carpeta `twentysixteen-child` crearemos un archivo `style.css`, que es el único archivo obligatorio para hacer un theme hijo. En la cabecera de esta hoja de estilos colocaremos las siguientes líneas:

```
/*
Theme Name: Twenty Sixteen Child
Theme URI: http://example.com/twentysixteen-child/
Description: Twenty Sixteen Child Theme
Author: Tu nombre
Author URI: http://tudominio.com
Template: twentysixteen
Version: 1.0
Tags: twentysixteen, extension, child, etc
Text Domain: twentysixteen-child
*/
 
@import url("../twentysixteen/style.css");
 
/* A partir de aquí empieza la personalización de tu theme */
```

Cabe destacar:

* **Theme Name:** Es el nombre de nuestro theme, debe ser diferente al nombre del theme padre, en este caso los hemos llamado igual pero añadiéndole el “Child”.

* **Template:** Debe ser el nombre de la carpeta del theme padre, tal cual. Importante que sea así, de lo contrario no funcionará.

* **@import:** Importamos la hoja de estilos principal del theme padre. También podríamos crear un archivo functions.php en el theme hijo y cargarla a través de wp_enqueue_style(), que sería más correcto, pero para éste ejemplo es suficiente con importarla.

**2. Screenshot para su identificación visual en el back-end**

Vamos a incluir también un pantallazo de nuestro theme, para identificarlo mejor desde el back-end. Para esto basta con añadir un archivo screenshot.png a la raíz del theme hijo. Podríamos simplemente utilizar el screenshot del theme padre y añadirle una máscara con el nombre del theme hijo.

<div align="center">
![](http://i.imgur.com/i5qFlPr.jpg)
</div>
<br>

**3. Archivos**

Ahora es cuando toca agregar los archivos a nuestro theme hijo. La regla general es que si agregamos cualquier archivo del theme padre en nuestro theme hijo, prevalecerá el archivo del hijo sobre el padre. Dicho de otro modo, en el ejemplo que vamos a ver, copiaremos el archivo footer.php de twentysixteen en la carpeta twentysixteen-child, y es éste último el que se cargará en nuestra web.

También se pueden incluir archivos nuevos. Si por ejemplo queremos añadir una nueva plantilla de página con un layout a nuestro gusto, bastará con crear el archivo nuevo en twentysixteen-child.

Si queremos modificar algún estilo, bastará con empezar a escribir CSS en el archivo style.css del theme hijo a continuación del import del style.css del theme padre.

Sólo hay una excepción en estas dependencias de archivos, y es el functions.php. El functions.php de un theme hijo no sobrescribe al theme padre. En lugar de eso, se carga de manera adicional (antes del theme padre). Esto es así porque en este archivo se suele recoger toda la funcionalidad del theme. Si queremos añadir una función nueva, bastará con añadir sólo esa función en el theme hijo, y no todo el functions.php completo más la nueva función.

¿Y qué pasa si quieres sobrescribir una función del padre? Bastaría con crearla en el theme hijo y modificarla a nuestro antojo e invocarla de la siguiente forma:

```
<?php
if ( ! function_exists( 'mi_funcion' ) ) {
  function mi_funcion() {
    // Mi Nuevo código
  }
}?>
```

**4. Nuestro ejemplo**

Vamos a cambiar el pie de twentysixteen a través de un theme hijo, por lo tanto copiaremos el archivo footer.php de twentysixteen a la carpeta twentysixteen-child. Abriremos este archivo y modificaremos las líneas 50 y 51:

```
//Cambiamos estas líneas
<span class="site-title"><a href="<?php echo esc_url( home_url( '/' ) ); ?>" rel="home"><?php bloginfo( 'name' ); ?></a></span>
<a href="<?php echo esc_url( __( 'https://wordpress.org/', 'twentysixteen' ) ); ?>"><?php printf( __( 'Proudly powered by %s', 'twentysixteen' ), 'WordPress' ); ?></a>
 
//Por estas otras
<a href="<?php echo esc_url( home_url( '/' ) ); ?>" rel="home"><?php bloginfo( 'name' ); ?></a><br>
Calle lorem ipsum, número X<br>
Tel: 655 555 555 – email@email.com
```

Es un ejemplo muy sencillo, que pretende servir de base y mostrar cual es el camino para modificar un theme padre a través de un child theme sin tocar el código fuente original. A partir de aquí las posibilidades son infinitas: crear nuevas plantillas, añadir nuevos custom post type, modificar el diseño general del sitio, etc.
