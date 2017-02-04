# Clase 1

## Tecnologías

### PHP

* Codecademy: https://www.codecademy.com/learn/php (interactivo, en inglés)
* PHP Ya: http://www.phpya.com.ar/ (en español)

### MySQL

* Codecademy: https://www.codecademy.com/learn/learn-sql (interactivo, en inglés)
* MySQL Ya: http://www.mysqlya.com.ar/ (en español)

### HTML

* Codecademy: https://www.codecademy.com/learn/web (HTML + CSS; interactivo, en inglés)
* HTML5 Ya: http://www.htmlya.com.ar/html5/ (en español)

### CSS

* Codecademy: https://www.codecademy.com/learn/web (HTML + CSS; interactivo, en inglés)
* CSS Ya: http://www.cssya.com.ar/ (en español)

### JavaScript

* Codecademy: https://www.codecademy.com/learn/javascript (interactivo, en inglés)
* JavaScript YA: http://www.javascriptya.com.ar/ (en español)

### jQuery

* Codecademy: https://www.codecademy.com/learn/jquery (interactivo, en inglés)

## Herramientas

### AMP Stacks (Apache + MySQL + PHP)

* XAMPP (Windows, Linux)
* MAMP (OS X)

### Editores de texto

* Sublime Text 3
* Atom

### IDEs

* Aptana (Eclipse)
* PHPStorm

### VCS (Version Control Software)

#### Git

* Sitio oficial: http://git-scm.com
* Try Git (Code School): https://try.github.io/levels/1/challenges/4
* GitHub: http://www.github.com
* BitBucket: http://bitbucket.com
* SourceTree: https://www.sourcetreeapp.com/
* GitHub Desktop: https://desktop.github.com/
* SmartGit: http://www.syntevo.com/smartgit/

#### SVN

* Sitio oficial: https://subversion.apache.org/
* Tutorial (Lihuen): https://lihuen.linti.unlp.edu.ar/index.php/C%C3%B3mo_usar_SVN
* Tortoise SVN: https://tortoisesvn.net/
* SmartSVN: http://www.smartsvn.com/

### Plugins

* Developer: https://github.com/Automattic/developer
* Airplane Mode: https://github.com/norcross/airplane-mode

---

##  Recursos

###  Codex

https://codex.wordpress.org/

Consiste en una wiki mantenida por la comunidad, y contiene documentación acerca del uso de las distintas APIs que ofrece WordPress. Parte de la misma está siendo reemplazada por las diferentes partes de WordPress Developer Resources, un nuevo portal para desarrolladores, especialmente lo referente a desarrollo de plugins, themes, y características internas del core que no están pensadas para ser reutilizadas en extensiones.

###  WordPress Developer Resources

Nuevo portal para desarrolladores, que hizo su aparición durante 2015. Está dividido en tres partes:

1. **Plugin Handbook:** recomendaciones y buenas prácticas para el desarrollo de plugins. https://developer.wordpress.org/plugins/
2. **Theme Handbook:** recomendaciones y buenas prácticas para el desarrollo de themes. Actualmente no disponible, ya que se sigue trabajando en una primera versión. Está pensada como reemplazo de la sección [Theme Development](https://codex.wordpress.org/Theme_Development) del Codex, la cual se debe tomar como referencia hasta el lanzamiento.
3. **Code Reference:** contiene documentación específica acerca de funciones, hooks, clases y métodos. Está reemplazando progresivamente al Codex como referencia acerca de las características internas de WordPress. https://developer.wordpress.org/reference/

###  Coding Standards

https://make.wordpress.org/core/handbook/best-practices/coding-standards/

Seguir epecificaciones estandarizadas para escribir código ayuda a prevenir errores comunes, mejora la legibilidad, simplifica modificaciones y colaboración entre desarrolladores, y en cierta manera garantiza que sea comprensible sin importar quién lo haya escrito.

###  Sitios a seguir: noticias, desarrolladores y soporte

* **Make WordPress:** https://make.wordpress.org/
* **WordPress Tavern:** https://wptavern.com/
* **ManageWP:** https://managewp.org/
* **Smashing Magazine:** https://www.smashingmagazine.com/
* **A List Apart:** http://alistapart.com/
* **Tom McFarlin:** https://tommcfarlin.com/
* **Jeremy Felt:** https://jeremyfelt.com/
* **Ayuda WordPress:** http://ayudawp.com/

###  Artículos útiles

* [Programar no es (tan) complicado](http://www.andrezrv.com/2015/10/15/programar-no-es-tan-complicado/)
* [Psicología del programador](http://www.andrezrv.com/2015/11/04/psicologia-del-programador/)
* [Some Thoughts On Writing Accessible Code](http://www.andrezrv.com/2014/05/09/thoughts-writing-accesible-code/)
* [More Thoughts on Writing Accessible Code](http://www.andrezrv.com/2014/08/07/thoughts-writing-accessible-code/)
* [The Single Biggest Mistake Programmers Make Every Day](https://medium.com/javascript-scene/the-single-biggest-mistake-programmers-make-every-day-62366b432308)

## Introducción a HTML

Dado que WordPress PHP está creado con PHP, que PHP es un lenguaje de procesamiento de texto pensado para desarrollo de sitios web, y que los sitios web se escriben usando HTML, a lo hora de trabajar con WordPress a nivel código es necesario tener al menos un conocimiento básico acerca de cómo opera HTML y cómo usarlo.

HTML significa "HyperText Markup Language" ("lenguaje de marcado de hipertexto", en español), y fue creado en 1993 por Tim Berners-Lee, con la intención de proponer un lenguaje estándar para la creación de sitios web. Tal como su nombre lo indica, está pensado para la creación de hipertexto: texto enriquecido por medio de la aplicación de distintos tipos de formato.

Al contrario de una definición errónea y bastante común, HTML no es un lenguaje de programación, sino puramente descriptivo. No permite tomar decisiones acerca del funcionamiento de un programa o aplicación, como hacen los lenguajes de programación, sino que solo da la posibilidad de declarar la estructura y el contenido de un documento. Esta característica es propia de los lenguajes de marcado, tales como XML, grupo al cual pertenece HTML.

### Sintaxis

La característica principal de HTML como lenguaje es la aplicación de etiquetas o _tags_ a un documento de texto determinado. Estas etiquetas pueden cumplir con dos propósitos: por un lado, darle información al navegador acerca del documento que está procesando,  y por otro, declarar el contenido y la estructura del documento.

Las etiquetas HTML siguen el siguiente formato:

```php
<nombre atributo="valor">Contenido</nombre>
```

El nombre de la etiqueta está relacionado con el propósito que cumple la misma dentro del documento. Por ejemplo, si usamos la etiqueta `<strong>` para envolver texto, estamos indicando que ese texto tiene mayor relevancia, y si usamos `<h1>` estamos indicando un título de primer nivel.

Por ejemplo, el siguiente código mostraría un texto conformado por un título y un párrafo, con parte del texto en negrita:

```php
<h1>Lorem Ipsum</h1>

<p>
  <strong>Lorem ipsum</strong> dolor sit amet.
</p>
```

Un documento HTML válido debe respetar e incluir ciertas etiquetas, más allá de su contenido concreto. Debe incluir el _doctype_ para indicarle al navegador qué versión de HTML debe utilizar, la etiqueta `html` para encerrar el contenido del documento, la etiqueta `head` para proveer información específica para el navegador, y la etiqueta `body` para contener aquello que verá el visitante de la página.

```php
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title></title>
</head>

<body>
  <h1>Lorem Ipsum</h1>

  <p>
    <strong>Lorem ipsum</strong> dolor sit amet.
  </p>
</body>
</html>
```

### Más información

En el [curso de HTML y CSS de Codecademy](https://www.codecademy.com/learn/web) puede encontrarse información más específica acerca de la sintaxis y el uso de HTML, así como varios ejercicios para aprender a usarlo correctamente.

## Introducción CSS

CSS (por "Cascading Style Sheets", u "hojas de estilo en cascada") es un lenguaje creado y utilizado para describir la presentación o el diseño visual de un documento escrito usando un lenguaje de marcado, tal como HTML o XML. Fue creado en 1997 por Håkon Wium Lie  y Bert Bos, mientras trabajaban junto a Tim Berners-Lee (el creador de HTML) en el W3C.

### Enlace

La manera más recomendada de trabajar con CSS consiste en crear un archivo separado con extensión `.css` (por ejemplo, `style.css`), y enlazarlo desde un archivo HTML usando la etiqueta `link`, dentro de `head`.

```php
<!DOCTYPE html>
<html>
 <head>
    <link rel="stylesheet" href="style.css">
    <meta charset="utf-8">
    <title></title>
 </head>
 <body>
     ...    
 </body>
</html>
```
De esta manera, todos los estilos declarados dentro de `style.css`  se aplican al documento HTML desde el cual se lo enlaza.

### Sintaxis

CSS permite aplicar un determinado diseño a un documento por medio de bloques como el siguiente:

```css
selector {
    atributo: valor;
    otro-atributo: valor;
}
```
El selector es el identificador de un elemento o grupo de elementos HTML. Los atributos, con sus respectivos valores, definen qué estilos se le aplicarán a los elementos seleccionados.

Por ejemplo, para aplicarle un texto de color rojo a todos los elementos con la etiqueta `<p>`, debería usarse el siguiente bloque:

```css
p {
 color: red;
}
```

También pueden identificarse elementos HTML por sus IDs, con el caracter `#`, y clases, usando `.`.

Por ejemplo, suponiendo el siguiente código HTML:

```php
<div class="post">
    Lorem ipsum dolor sit amet.
</div>
```

Podríamos aplicarle estilos al elemento con clase `post` de la siguiente manera:

```css
.post {
 color: red;
}
```

Podemos operar de manera similar con IDs.

HTML:

```php
<header id="site-header">
    Lorem ipsum dolor sit amet.
</header>
```

CSS:
```css
#site-header {
 color: red;
}
```

### Más información

En el [curso de HTML y CSS de Codecademy](https://www.codecademy.com/learn/web) puede encontrarse información más específica acerca de la sintaxis y el uso de CSS, así como varios ejercicios para aprender a usarlo correctamente.

--

## Underscores

Underscores (comúnmente llamado "\_S") es un starter theme creado y mantenido por Automattic para facilitar el desarrollo de nuevos themes. Está escrito siguiendo todos los estándares de código presentes en WordPress.org, e incluye los siguientes items, entre otras cosas:

* CSS minimalista y estandarizado para los navegadores ms populares.
* Templates básicos en HTML5, incluyendo uno para la página de error 404.
* Funciones PHP básicas y fáciles de comprender.
* Soporte para navegación multi-nivel.

Para instalar Underscores, podemos generar un theme con un nombre propio desde http://underscores.me, o descargarlo desde ()[https://github.com/Automattic/\_s]. Si optamos por la segunda opción, también necesitamos reemplazar todas las apariciones de la cadena de texto "\_s" por una acorde al nombre de nuestro theme.

Una vez que contamos con el archivo zip que contiene el theme, podemos subirlo a nuestra instalación de WordPress desde el panel de administración, clickeando el botón "Upload Theme" dentro de _Appearance > Themes > Add New_, o descomprimiendo el archivo y copiando la carpeta del theme a `wp-content/themes`.

Cuando hayamos activado el theme, vamos a ver en la parte pública de nuestro sitio un diseño muy básico. Para empezar a modificarlo a nuestro gusto podemos editar el archivo `style.css` con nuestros propios estilos, modificar el HTML de los templates o agregar nuestro propio JavaScript para definir interacciones.
