# Themes y Child Themes

## Estructura básica de un theme

Para tener un amplio conocimiento sobre todo lo que representa WordPress es necesario conocer cómo es la estructura de una página, para qué sirve cada uno de los archivos que son parte del theme a desarrollar y cómo es el funcionamiento interno para que este gestor de contenido nos muestre los post y algunos extras.

### Anatomía de un Theme de WordPress

![](http://i.imgur.com/IpmDtmT.png)

Los themes de WordPress se componen de una carpeta de archivos, cada uno de los cuales controla una parte específica del theme.

![](http://i.imgur.com/rH3pngl.png)

### El ciclo

Es la parte más poderosa del theme de WordPress, se inicia con una consulta (que determina los mensajes y páginas a mostrar) y termina con un end while de PHP. Todo lo demás de ti, puedes específicar cómo se muestran los títulos, el contenido de cada post, los metadatos, campos personalizados y de los comentarios.

Se pueden configurar múltiples ciclos en una misma página.

![](http://i.imgur.com/CSJIb4z.png)

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

---

# Qué es un plugin y para qué sirve

En un sentido conceptual, podríamos decir que todo sitio o aplicación web se divide en tres partes: **contenido**, **presentación** y **funcionalidad**. **El contenido es aquella información variable que nuestro sitio o aplicación le muestra al usuario final**, y que alguien con los permisos adecuados puede agregar, modificar o eliminar. En la mayoría de las aplicaciones modernas es provisto de manera externa por un usuario, sin necesidad de modificar de manera manual los archivos que constituyen a la aplicación, y normalmente queda guardado en algún tipo de base de datos. **La presentación es la forma en la que esa información se le muestra al usuario**, y tiene que ver con la implementación de algún tipo de diseño gráfico sobre una interfaz; es básicamente cómo se ve nuestro proyecto. **La funcionalidad tiene que ver con todos los procesos internos que manejan el contenido** (por ejemplo la carga, edición y eliminación) y lo dejan preparado para ser presentado al usuario.

WordPress, como todo CMS (por *Content Management System*) o sistema de gestión de contenidos, se encarga por su cuenta de gran parte de las cuestiones técnicas relacionadas con la manipulación de información. Sin embargo, muchas veces vamos a encontrarnos con que necesitamos poder manejar algún tipo de información que no está disponible desde la instalación, o que se ejecuten ciertos procesos internos invisibles al usuario, o que necesitamos mostrar algo de una manera que no estaba prevista por las opciones de presentación que tenemos a nuestro alcance. Ese es el momento en el cual entran en acción los plugins y themes.

**Lo que se dice más típicamente en el ámbito de los desarrolladores que trabajan con WordPress es que, mientras los themes están pensados para manejar cuestiones de presentación, los plugins apuntan exclusivamente a agregar nueva funcionalidad.** Sin embargo, en este libro vamos a ponernos un poco más específicos con estas definiciones, y a decir que eso no siempre es tan así. Eso pasa porque la funcionalidad y la presentación no siempre son dos conceptos inseparables, sino que a veces están muy entrelazados. Y si bien siempre es una excelente práctica intentar separarlos cuanto sea posible, a veces se presentan algunas situaciones problemáticas en las que es complicado distinguirlos. Por eso nos vamos a encontrar muy típicamente con themes que manejan algunas cuestiones de funcionalidad, o con plugins que ofrecen algún tipo de presentación. Incluso hay cosas que bien podrían ser incluidas tanto en plugins como en themes, y en ese punto nos vemos obligados a decidir dónde es mejor colocarlas.

Teniendo en cuenta todo esto, podemos decir que **los themes, más que tener que ver específicamente con la presentación, en realidad tienen el foco puesto en ella, sin dejar completamente de lado la funcionalidad**. Son aquello que, a grandes rasgos, va a definir de qué manera se va a ver nuestro sitio, pero pueden ofrecer ciertas características de comportamiento. **Lo mismo se aplica a los plugins, pero a la inversa: tienen el foco puesto en agregar nueva funcionalidad a nuestro sitio, pero pueden ofrecer nuevas formas de visualización**.

A partir de tener presentes cuáles son estas diferencias y similitudes conceptuales básicas entre plugins y themes, podemos empezar a construir nuestros propios plugins.

# Programación Orientada a Eventos

Un primer paso muy recomendable para empezar a entender cómo funciona WordPress a nivel código, independientemente de si trabajamos con plugins o themes, es empezar a entender el paradigma de programación orientada a eventos, ya que en él se basan la mayoría de las posibilidades de extensión que tenemos disponibles. Es importantísimo conocer este paradigma a la hora de interactuar con el código de WordPress, ya que desconocerlo puede llevar a muchísimo trabajo innecesario, o a mucho tiempo perdido en mantenimiento, mejoras y arreglo de bugs.

### ¿Qué es la programación orientada a eventos?

La herramienta principal que nos ofrece WordPress para construir nuestras propias extensiones, ya se trate de plugins o themes, es un conjunto de funciones al que comúnmente se llama *[Plugin API](https://codex.wordpress.org/Plugin_API)*.

Esta API está basada en el ya mencionado paradigma de [Programación Orientada a Eventos](https://en.wikipedia.org/wiki/Event-driven_programming), o Programación Basada en Eventos (en inglés, *Event-oriented programming* o *Event-driven programming*). Es combinable con otros paradigmas populares, como el estructurado, el orientado a objetos y el funcional, y tiene unos conceptos fundamentales muy sencillos.

Este paradigma es extremadamente útil cuando necesitamos que un proceso se ejecute en algún punto determinado, pero tenemos un acceso limitado al código que se encuentra en ese punto. Al trabajar con WordPress es casi inevitable usar el paradigma, porque no podemos modificar libremente el código que descargamos sin perder lo que hayamos hecho cuando necesitemos actualizar la versión. Si ya trabajaron con jQuery, probablemente hayan visto el paradigma de eventos al usar los métodos `trigger()` y `bind()`.

El paradigma de eventos dicta que, en ciertos lugares puntuales de nuestro programa, van a ocurrir determinados eventos o "sucesos". Estos eventos, por sí mismos, no hacen nada. Podríamos decir que son simples contenedores, porque solamente van a empezar a tener algún efecto sobre nuestra aplicación cuando les asignemos procesos, es decir cuando indiquemos que al ocurrir un determinado evento tiene que ejecutarse un proceso determinado.

Normalmente, vamos a tener en alguna parte de nuestro código la ejecución de un evento con un nombre determinado. Supongamos que tenemos un evento llamado `mesa_servida`.

```php
<?php
evento( 'mesa_servida' );
```

Por otra parte, vamos a necesitar que, al ocurrir ese evento, también se ejecute un proceso. Vamos a suponer que, al ocurrir el evento `mesa_servida`, queremos que se procese la función `sentarse_a_comer()`. Para eso, necesitamos que la función haya sido declarada antes de que ocurra el evento.

```php
<?php
function sentarse_a_comer() {
	echo 'a comer!';
}

evento( 'mesa_servida' );
```

Sin embargo, ese código por sí mismo todavía no hace nada. Para que la función se procese en el momento en el que se ejecuta el evento, necesitamos asignar la función al evento.

```php
<?php
function sentarse_a_comer() {
	echo 'a comer!';
}

asignar_proceso( 'mesa_servida', 'sentarse_a_comer' );

evento( 'mesa_servida' ); // Se imprime "a comer!"
```

De esta manera, al usar `asignar_proceso()` con el nombre del evento como primer parámetro y el nombre de la función como segundo parámetro (lo cual se llama *callback*), indicamos que, al ocurrir el evento `mesa_servida`, va a procesarse el código declarado dentro de la función `sentarse_a_comer`. La función asignada a un evento es lo que dentro de este paradigma se suele llamar *hook*.

El importante notar que, al menos en PHP, no es necesario que la función exista antes de asignarla a un evento, pero sí tiene que haber sido declarada antes de que el evento ocurra. La misma asignación también tiene que hacerse antes de que ocurra el evento. De esta manera, el siguiente código es equivalente al anterior:

```php
<?php
asignar_proceso( 'mesa_servida', 'sentarse_a_comer' );

function sentarse_a_comer() {
	echo 'a comer!';
}

evento( 'mesa_servida' );
```

Ahora bien, conociendo los conceptos fundamentales de la programación orientada a eventos, podemos ver de qué manera WordPress nos permite aplicarlos para construir nuestras extensiones. Para esto, WordPress nos ofrece dos diferentes tipos de eventos: acciones y filtros.

### Acciones

Uno de los dos tipos de eventos ofrecidos por WordPress se llama ***[Action](https://codex.wordpress.org/Plugin_API#Actions)***, o acción. El propósito de las acciones es permitir la ejecución de procesos propios durante la carga de una página. Algunos ejemplos de estos procesos pueden consistir en modificar información de la base de datos, enviar un mail, registrar nuevos tipos de contenido, imprimir cierta información en pantalla, etc.

La interacción básica entre eventos y procesos es muy similar a los ejemplos que acabamos de ver. Las acciones se ejecutan por medio de la función `do_action()`, mientras los hooks se registran usando `add_action()`.

```php
<?php
add_action( 'mesa_servida', 'sentarse_a_comer' );

function sentarse_a_comer() {
	echo 'a comer!';
}

do_action( 'mesa_servida' );
```

Sin embargo, este uso básico a veces puede resultar un poco limitado. Por ejemplo, es posible que queramos añadir un segundo hook a `mesa_servida`, y necesitemos especificar cuál de ellos va a ejecutarse primero. Supongamos que introducimos la función `comer()`, y queremos que se ejecute inmediatamente después de `sentarse_a_comer()`.

```php
<?php
add_action( 'mesa_servida', 'comer' );

function comer() {
	echo 'comiendo ...';
}

add_action( 'mesa_servida', 'sentarse_a_comer' );

function sentarse_a_comer() {
	echo 'a comer!';
}

do_action( 'mesa_servida' );
```

Con este código, la función `comer()` siempre se va a ejecutar antes de `sentarse_a_comer()`. Si no tenemos la posibilidad de cambiar la ubicación de la asignación de `add_action( 'mesa_servida', 'comer' )`, necesitamos encontrar una manera de hacer que `comer()` se ejecute después de `sentarse_a_comer()`. Para este tipo de necesidades, la función `add_action()` admite un tercer parámetro llamado `$priority`, en el cual se especifica un valor numérico. Los hooks con valores menores van a ser ejecutados con anterioridad a los hooks con valores más altos, y el valor por defecto es 10. Teniendo esto en cuenta, podemos modificar nuestro código de esta manera.

```php
<?php
add_action( 'mesa_servida', 'comer', 20 );

function comer() {
	echo 'comiendo ...';
}

add_action( 'mesa_servida', 'sentarse_a_comer', 10 );

function sentarse_a_comer() {
	echo 'a comer!';
}

do_action( 'mesa_servida' );
// Se imprime "a comer!"
// Se imprime "comiendo ..."
```

Puede pasar, también, que dentro de las funciones que usamos como hooks necesitemos algún tipo de información contextual con respecto al momento en el que se ejecuta la acción. Supongamos que dentro de `sentarse_a_comer()` necesitamos evaluar cuántos comensales vamos a tener.

```php
<?php
add_action( 'mesa_servida', 'sentarse_a_comer', 10 );

function sentarse_a_comer( $comensales ) {
	if ( $comensales < 5 ) {
		echo 'a comer!';
	} else {
		echo 'acá hay demasiada gente, Roberto!'
	}
}

$comensales = 10;

do_action( 'mesa_servida' );
// Se imprime "a comer!"
```

De alguna manera necesitamos hacer que ese número de comensales declarado en el mismo contexto de ejecución de `do_action( 'mesa_servida' )` llegue a la función `sentarse_a_comer()`. Para esto podemos adjuntar a `do_action()` todas las variables que queramos, y en `add_action()` especificar, en un cuarto parámetro llamado `$accepted_args`, el número de parámetros que va a recibir nuestro hook.

```php
<?php
add_action( 'mesa_servida', 'sentarse_a_comer', 10, 1 );

function sentarse_a_comer( $comensales ) {
	if ( $comensales < 5 ) {
		echo 'a comer!';
	} else {
		echo 'acá hay demasiada gente, Roberto!';
	}
}

$comensales = 10;

do_action( 'mesa_servida' , $comensales );
// Se imprime "acá hay demasiada gente, Roberto!"
```

El valor por defecto de `$accepted_args` es 1, por lo cual, si vamos a tener un solo parámetro, podemos incluso no especificar este valor.

```php
<?php
add_action( 'mesa_servida', 'sentarse_a_comer', 10 );

function sentarse_a_comer( $comensales ) {
	if ( $comensales < 5 ) {
		echo 'a comer!';
	} else {
		echo 'acá hay demasiada gente, Roberto!';
	}
}

$comensales = 10;

do_action( 'mesa_servida' , $comensales );
// Se imprime "acá hay demasiada gente, Roberto!"
```

Sin embargo, podemos pasarle a `do_action()` tantos parámetros como necesitemos en nuestra función, pero siempre especificando la cantidad en `add_action()`, en caso de que sea más de uno.

```php
<?php
add_action( 'mesa_servida', 'sentarse_a_comer', 10, 2 );

function sentarse_a_comer( $comensales, $comida ) {
	if ( $comensales < 5 ) {
		echo 'A comer!';
	} else {
		echo 'Acá hay demasiada gente, Roberto! Tenemos solamente ' . $comida;
	}
}

$comensales = 10;
$comida = 'Fideos con pesto';

do_action( 'mesa_servida' , $comensales, $comida );
// Se imprime "acá hay demasiada gente, Roberto! Tenemos solamente fideos con pesto"
```

Sabiendo cómo opera este tipo de evento, también podemos agregar hooks a las acciones nativas de WordPress. Por ejemplo, podemos mandarle un mail al administrador del sitio cada vez que se publique un nuevo post. Para esto, definimos la función que envía el mail, y se la asignamos como hook a la acción `publish_post`.

```php
<?php
add_action( 'publish_posts' ,'avisar_amigos' );

function avisar_amigos() {
    $amigos = 'juan@example.org,pedro@example.org';

    mail( $amigos, 'Actualización de blog', 'Acabo de actualizar mi blog: http://blog.example.com' );
}
```

Una vez que activemos este nuevo plugin y publiquemos un nuevo post, vamos a poder ver que llega un nuevo mail a la casilla de correo del administrador.

Noten que no estamos llamando directamente a `do_action( 'publish_posts' );`. Esto es porque ese llamado se va a estar haciendo en algún lugar del código de base de WordPress, que llamamos Core. WordPress cuenta con una [lista](http://codex.wordpress.org/Plugin_API/Action_Reference) bastante larga de acciones propias a las que podemos asignar nuestros propios hooks, que nos pueden servir a manera de guía.

- - -

### Filtros

El otro tipo de eventos que nos ofrece WordPress son los ***[Filters](https://codex.wordpress.org/Plugin_API#Filters)***, o filtros. Este tipo de eventos ya no pone tanto el foco en la ejecución de procesos, como pasa con las acciones, sino en la manipulación de datos internos de la aplicación. Por ejemplo, un filtro puede ser utilizado para cambiar el valor de una variable, o modificar el valor de retorno de una función. Usos típicos de los filtros pueden ser activar o desactivar ciertas características de la aplicación, modificar alguna parte del HTML que se va a imprimir, cambiar valores de configuración o alterar consultas a la base de datos.

Una característica importante de los filtros es que siempre tienen un valor de retorno, a diferencia de las acciones. Debido a esto, los filtros siempre están asignados a una variable, a una evaluación o a un valor de retorno de una función o método.

Teniendo en cuenta esta diferencia, su uso es muy similar al de las acciones. En algún punto de nuestro código vamos a tener un llamado a la función `apply_filters()`, que es equivalente a `do_action()`. Los parámetros que va a recibir esta función son el nombre del evento y el valor que va a tener por defecto.

```php
<?php
$comensales = apply_filters( 'cantidad_de_comensales', 10 );
```

Ahora bien, antes de que ese código se ejecute, necesitamos definir cuáles van a ser los filtros que se asignen al evento. Esto podemos hacerlo a través de la función `add_filter()`.

```php
<?php
add_filter( 'cantidad_de_comensales', 'nueva_cantidad_de_comensales' );

function nueva_cantidad_de_comensales( $comensales ) {
	if ( viene_ramon() ) {
		$comensales++;
	}

	return $comensales;
}

$comensales = apply_filters( 'cantidad_de_comensales', 10 );
```

De esta manera, al momento de definirse la variable `$comensales`, el valor por defecto (10) se le va a pasar a la función `nueva_cantidad_de_comensales()`, y va a sumar 1 en caso de que Ramón venga (dando por resultado 11). Noten que antes de finalizar la función que opera como filtro siempre necesitamos devolver un valor; de lo contrario el valor que estemos filtrando va a terminar siendo `null`.

Los filtros, al igual que las acciones, pueden recibir prioridad de ejecución (`$priority`) y número de argumentos (`$accepted_args`) como tercer y cuarto parámetro, respectivamente.

```php
<?php
add_filter( 'cantidad_de_comensales', 'descartar_vegetarianos', 20, 2 );

function descartar_vegetarianos( $comensales, $comida ) {
	if ( 'asado' == $comida && viene_jose() ) {
		$comensales--;
	}

	return $comensales;
}

add_filter( 'cantidad_de_comensales', 'nueva_cantidad_de_comensales', 10 );

function nueva_cantidad_de_comensales( $comensales ) {
	if ( viene_ramon() ) {
		$comensales++;
	}

	return $comensales;
}

$comida = 'asado';
$comensales = apply_filters( 'cantidad_de_comensales', 10, $comida );
```

De esta forma, el primer filtro a ejecutarse va a ser `nueva_cantidad_de_comensales()`, por más que se haya declarado en segundo lugar. No necesitamos especificar la cantidad de argumentos para ese filtro, porque el valor por defecto es 1, y solamente necesitamos la primera variable, `$comensales`. En segundo lugar se va a ejecutar la función `descartar_vegetarianos()`, que va a restar un comensal en caso de que la comida sea asado y venga José. Al asignar este filtro sí especificamos que vamos a recibir dos parámetros, ya que necesitamos las variables `$comensales` y `$comida`, que se están pasando al filtro por medio de `apply_filters()`.

Al igual que pasa con las acciones, también tenemos muchos filtros que vienen con WordPress por defecto. Uno de ellos es `the_content`, que se aplica al contenido de cada post o página antes de ser impreso. Supongamos que queremos modificar levemente este contenido. Para eso podemos hacer algo así:

```php
<?php
add_filter( 'the_content', 'modify_post_content' );

function modify_post_content( $content ) {
	if ( is_single() ) {
		$content = '<p>Esto es un post.</p>' . $content;
	}

	return $content;
}
```

Una vez que guardemos este código en nuestro plugin y refresquemos cualquier página correspondiente a un post, vamos a ver el texto que agregamos en nuestra función inmediatamente arriba del resto del contenido.

- - -

### Remover eventos del registro

Algo que podemos llegar a necesitar mientras desarrollamos nuestras propias extensiones es que ciertas acciones o filtros se dejen de ejecutar. Por ejemplo, podemos querer desactivar alguna funcionalidad nativa de WordPress, o de un plugin de terceros, o que alguna de nuestras acciones o filtros solamente se ejecuten en determinados contextos. Para lograr eso, WordPress nos ofrece cuatro funciones:

* `remove_action()`
* `remove_filter()`
* `remove_all_actions()`
* `remove_all_filters()`

Cuando asignamos un filtro o una acción a un evento, WordPress lo guarda en una lista, más específicamente en la variable global `$wp_filters`, donde también queda el detalle de sus prioridades y la cantidad de argumentos que acepta. Lo que nos permiten estas funciones es remover de esa lista los procesos que necesitemos.

Por ejemplo, supongamos que tenemos dos eventos, una acción y un filtro, y a cada uno de ellos queremos asignarle una función.

```php
<?php
add_action( 'my_action', 'my_action_callback', 10, 1 );

function my_action_callback() {
	echo 'Hello world!';
}

add_filter( 'my_filter', 'my_filter_callback', 10, 1 );

function my_filter_callback( $value ) {
	return 'some other value';
}

do_action( 'my_action' );

$my_value = apply_filters( 'my_filter', 'some value' );
```

Sin embargo, en algún punto en el futuro vamos a necesitar que esas funciones que asignamos dejen de ejecutarse, pero por las características de nuestra extensión no podemos remover ni las asignaciones ni las declaraciones de funciones. No podemos simplemente remover código. En ese punto es donde necesitamos usar estas nuevas funciones de las que venimos hablando.

```php
<?php
add_action( 'my_action', 'my_action_callback', 20, 1 );

function my_action_callback() {
	echo 'Hello world!';
}

add_filter( 'my_filter', 'my_filter_callback', 20, 1 );

function my_filter_callback( $value ) {
	return 'some other value';
}

remove_action( 'my_action', 'my_action_callback', 20 );
remove_filter( 'my_filter', 'my_filter_callback', 20 );

do_action( 'my_action' );

$my_value = apply_filters( 'my_filter', 'some value' );
```

De esta manera logramos que las funciones que asignamos previamente con `add_action()` y `add_filter()` dejen de ejecutarse. 

Para usar correctamente `remove_action()` y `remove_filter()` necesitamos tener en cuenta dos cosas: en primer lugar, tienen que ser llamadas después de que los callbacks hayan sido asignados, pero antes de que se ejecuten las acciones. En segundo lugar, ambas funciones reciben un tercer parámetro, que es equivalente a la prioridad con la que se asignaron previamente los callbacks que ahora estamos removiendo del registro. Este parámetro no es obligatorio, y su valor por defecto es 10. Si no lo especificamos, tenemos que asegurarnos de que la prioridad del callback también sea 10, o que tampoco esté especificada.

Por último, tenemos las funciones `remove_all_actions()` y `remove_all_filters()`. Estas nos permiten remover todos los callbacks que hayan sido asignados a una acción o filtro determinados, sin necesidad de especificar más que el nombre del evento. Por ejemplo, podemos suponer que tenemos dos callbacks asignados a una acción y otros dos callbacks asignados a un filtro.

```php
<?php
add_action( 'my_action', 'my_action_callback', 10 );
add_action( 'my_action', 'my_other_callback', 20 );

function my_action_callback() {
	echo 'Hello world!';
}

function my_other_action_callback() {
	echo 'Hello again!';
}

add_filter( 'my_filter', 'my_filter_callback', 10, 1 );
add_filter( 'my_filter', 'my_other_filter_callback', 20, 1 );

function my_filter_callback( $value ) {
	return 'some other value';
}

function my_other_filter_callback( $value ) {
	return 'some other different value';
}

do_action( 'my_action' );

$my_value = apply_filters( 'my_filter', 'some value' );
```

Podemos remover muy fácilmente todos los callbacks para cada evento haciendo algo así:

```php
<?php
add_action( 'my_action', 'my_action_callback', 10 );
add_action( 'my_action', 'my_other_callback', 20 );

function my_action_callback() {
	echo 'Hello world!';
}

function my_other_action_callback() {
	echo 'Hello again!';
}

add_filter( 'my_filter', 'my_filter_callback', 10, 1 );
add_filter( 'my_filter', 'my_other_filter_callback', 20, 1 );

function my_filter_callback( $value ) {
	return 'some other value';
}

function my_other_filter_callback( $value ) {
	return 'some other different value';
}

remove_all_actions( 'my_action' );
remove_all_filters( 'my_filter' );

do_action( 'my_action' );

$my_value = apply_filters( 'my_filter', 'some value' );
```

Tenemos que tener en cuenta que ambas son funciones para usar con mucho cuidado, ya que no es muy común querer remover todos los callbacks para un evento. Sin embargo, es bueno saber que contamos con ellas, y suelen ser muy útiles mientras estamos desarrollando nuestras extensiones, particularmente con fines de testing.

### Conclusiones

Un buen manejo y comprensión de la Plugin API permiten agregarle a WordPress cualquier tipo de funcionalidad que pretendamos. Pero más allá del trabajo que podemos hacer con WordPress, dominar el paradigma de programación orientada a eventos puede abrirnos muchas puertas en el mundo del desarrollo de software en general, y en particular en el desarrollo web, donde se demuestra cada vez más útil, siendo adoptado por muchos otros proyectos open source, como jQuery, Drupal o Symfony.

# Cómo crear un plugin básico

![](https://www.dropbox.com/s/v4x0ws69lhsoy21/Screenshot%202016-07-19%2015.33.36.png?dl=0&raw=1)
En un sentido reduccionista, los plugins son simples archivos PHP que se encuentran en la carpeta `wp-content/plugins` de nuestra instalación de WordPress. El paquete de instalación de WordPress incluye dos plugins: **Akismet** y **Hello Dolly**. Hello Dolly es un plugin que consiste en un simple archivo llamado `hello.php`, y lo podemos encontrar suelto en la carpeta de plugins. Akismet es más complejo: consiste en varios archivos, y por una cuestión de orden lo vamos a encontrar en su propia carpeta. Sin embargo, dentro de esa carpeta vamos a encontrar un archivo principal llamado `akismet.php`. La similitud entre estos dos archivos, `hello.php` y `akismet.php`, es que ambos cuentan con una sección de código comentado al inicio de cada uno, conteniendo una serie de definiciones con los datos de cada plugin. Esa porción de código, que funciona a manera de encabezado del plugin, es lo que permite que WordPress reconozca el archivo como el principal de un plugin, y que a partir de ahí lo pueda mostrar en la lista de la sección *Plugins* de nuestra instalación.

Esta forma de organización nos indica que un plugin puede estar suelto dentro de `wp-content/plugins` o dentro una carpeta propia del plugin. Lo importante es que el archivo cuente con ese encabezado en el que se declaren ciertos datos del plugin. Solamente es obligatorio que nuestro plugin tenga un nombre, pero también podemos agregar descripción, link, autor, versión, etc.

```php
<?php
/*
Plugin Name: Mi plugin
Plugin URI:  http://example.org/mi-plugin
Description: Esta es la descripción de mi plugin.
Version:     1.0
Author:      Andrés Villarreal
Author URI:  http://andrezrv.com
License:     GPL2
License URI: https://www.gnu.org/licenses/gpl-2.0.html
Domain Path: /languages
Text Domain: mi-plugin
*/
```

Una vez que tenemos nuestro archivo principal con su encabezado, podemos ver que WordPress lo reconoce como tal en nuestra lista de plugins instalados. Sin embargo, todavía no está activo. Para activarlo tenemos que hacer click en el link correspondiente. Eso hace que todo el código PHP que escribamos en nuestro archivo empiece a ejecutarse, como pasa con cualquier archivo PHP que podamos ejecutar en un proyecto propio.

![](https://www.dropbox.com/s/m9xg4x3xanolse9/Screenshot%202016-07-19%2015.51.28.png?dl=0&raw=1)

Como todavía no tenemos nada de código PHP propiamente dicho, más allá del encabezado, no tenemos nada ejecutándose. Podemos solucionar eso rápidamente escribiendo algo de código PHP después del encabezado, como por ejemplo un `die( 'Hello world!' );`. Esto va a hacer que, una vez que actualicemos nuestro browser en cualquier página de nuestra instalación de WordPress, se imprima *"Hello world!"* y la ejecución del script termine, tal como dicta el comportamiento de la función `die()`. Claramente, esto no es para nada útil, pero sirve para darnos una idea de la libertad de manejo de código que tenemos  a partir de ese archivo.

```php
<?php
/*
Plugin Name: Mi plugin
Plugin URI:  http://example.org/mi-plugin
Description: Esta es la descripción de mi plugin.
Version:     1.0
Author:      Andrés Villarreal
Author URI:  http://andrezrv.com
License:     GPL2
License URI: https://www.gnu.org/licenses/gpl-2.0.html
Domain Path: /languages
Text Domain: mi-plugin
*/

die( 'Hello world!' );
```

Para ver más posibilidades acerca de lo que nos permite WordPress, una buena idea a la hora de empezar es estudiar Hello Dolly. Dentro de `hello.php` podemos ver que se declara una función, `hello_dolly()`, que toma una línea de la canción "Hello Dolly" al azar, y la imprime al ejecutarse la acción `admin_notices`, por medio del uso de la función `add_action()`. De hecho, si activamos Hello Dolly desde nuestra lista de plugins, podemos ver que la línea que se muestra cambia cada vez que actualizamos cualquier página en la sección de administración, reflejando lo que se define en el código del plugin.

**Extracto de `hello.php`:**

```php
<?php
// This just echoes the chosen line, we'll position it later
function hello_dolly() {
    $chosen = hello_dolly_get_lyric();
    echo "<p id='dolly'>$chosen</p>";
}

// Now we set that function up to execute when the admin_notices action is called
add_action( 'admin_notices', 'hello_dolly' );
```

No importa ahora mismo cómo funciona esta interacción entre funciones por medio de acciones, porque la vamos a ver con más detalle en los próximos capítulos. Lo que es importante saber ahora es que, una vez que creamos nuestro archivo principal de plugin y activamos el plugin desde el panel de administración, dentro de ese archivo podemos hacer cualquier cosa que PHP nos permita hacer.

# Sanitización

Un punto crítico de cualquier tipo de aplicación es la entrada de datos por parte del usuario, ya que, si no se toman las medidas oportunas, un usuario puede (intencionadamente o no) causar fallas en la aplicación.

Muchas veces, por la inexperiencia del desarrollador, se deposita demasiada confianza en la validación de lados del lado de cliente. Esto es algo grave, ya que existen herramientas para enviar datos a una web sin que intervenga para nada un navegador ni JavaScript.


### Validación de datos ingresados (input)

Antes de guardar la información en la base de datos la información ingresada por un usuario, necesitamos **sanitizarlos** para evitar posibles ataques. WordPress provee varias funciones que permiten asegurarnos de que los datos ingresados se guarden en la base de datos con el formato correcto. Algunas de esas funciones son las siguientes:

* **sanitize_email()**
* **sanitize_file_name()**
* **sanitize_html_class()**
* **sanitize_key()**
* **sanitize_meta()**
* **sanitize_mime_type()**
* **sanitize_option()**
* **sanitize_sql_orderby()**
* **sanitize_text_field()**
* **sanitize_title()**
* **sanitize_title_for_query()**
* **sanitize_title_with_dashes()**
* **sanitize_user()**

### Escape de datos obtenidos (output)

Para mostrar información que obtenemos desde la base de datos, WordPress también nos ofrece una serie de funciones de seguridad. Estas funciones nos aseguran que los datos obtenidos se muestren al usuario con el formato correcto:

* **esc_html()**
* **esc_url()**
* **esc_js()**
* **esc_attr()**
* **esc_textarea()**

# Optimización de Funciones

## Pluggable functions

Estas permiten sustituir determinadas funciones básicas a través de plugins. 
WordPress carga las funciones incorporadas sólo si están indefinidas después de que todos los plugins se hayan cargado.


Las funciones condicionales ya no son  añadidas al núcleo de WordPress. 
Todas estas nuevas funciones no utilizan filtros en su producción, para lograr una funcionalidad similar.

**Nota**: una función sólo puede ser reasignado de  esta manera una vez , por lo que no se puede instalar dos plugins que se conectan a la misma función por diferentes razones.

### Lista de Funciones condicionales

* **wp-includes/pluggable.php**   
    * auth_redirect
    * cache_users
    * check_admin_referer
    * check_ajax_referer
    * get_avatar
    * get_currentuserinfo
    * get_user_by_email (obsoleto)
    * get_user_by
    * get_userdatabylogin (obsoleto)
    * get_userdata
    * is_user_logged_in
    * wp_authenticate
    * wp_check_password
    * wp_clear_auth_cookie
    * wp_create_nonce
    * wp_generate_auth_cookie
    * wp_generate_password
    * wp_get_current_user
    * wp_hash_password
    * wp_hash
    * wp_logout
    * wp_mail
    * wp_new_user_notification
    * wp_nonce_tick
    * wp_notify_moderator
    * wp_notify_postauthor
    * wp_parse_auth_cookie
    * wp_password_change_notification
    * wp_rand
    * wp_redirect
    * wp_safe_redirect
    * wp_salt
    * wp_sanitize_redirect
    * wp_set_auth_cookie
    * wp_set_current_user
    * wp_set_password
    * wp_text_diff
    * wp_validate_auth_cookie
    * wp_validate_redirect
    * wp_verify_nonce


###  Referencia

**get_currentuserinfo()**

Toma la información si es que existe del usuario que ha iniciado sesión. 

**get_userdata($userid) **

Retorna la información de un  usuario especificado de la base de datos.


**wp_login($username, $password, $already_md5 = false) **


Retorna verdadero si el nombre de usuario y contraseña corresponden con los registradps por el usuario.

**auth_redirect() **

Si un usuario no está conectado, será redirigido a la página de inicio de sesión de WordPress antes de que se les permita acceder al contenido de la página desde la que se llama a esta función . Al iniciar la sesión con éxito , el usuario es enviado de vuelta a la página en cuestión.

**wp_redirect($location)**

Redirige el navegador a la URI absoluta  especificado por el parámetro $location.

**wp_notify_postauthor($comment_id, $comment_type='')**

Envia un mensajes de correo electrónico al autor cuado le hacen un comentario a uno de sus contenidos.

**wp_notify_moderator($comment_id) **

Informa a la cuenta de correo electrónico administrativo que los comentario especificado deben ser moderados.


### Ejemplo 


Un ejemplo de lo que puede hacer con una pluggable functions activo es reemplazar el controlador de correo electrónico predeterminado. Para ello, lo que se necesita para escribir un plugin que define una función wp_mail(). Por defecto la función wp_mail() se ve como esto:

```
function wp_mail( $to, $subject, $message, $headers = '' ) {
  if( $headers == '' ) {
    $headers = "MIME-Version: 1.0\n" .
      "From: " . get_settings('admin_email') . "\n" . 
      "Content-Type: text/plain; charset=\"" . get_settings('blog_charset') . "\"\n";
  }

  return @mail( $to, $subject, $message, $headers );
}
```

Pero por ejemplo, si desea copiar todo su correo a otra dirección, se puede utilizar este código en un plugin:

```
if( ! function_exists('wp_mail') ) {
  function wp_mail( $to, $subject, $message, $headers = '' ) {
    if( $headers == '' ) {
      $headers = "MIME-Version: 1.0\n" .
        "From: " . get_settings('admin_email') . "\n" . 
        "Cc: dummy@example.com\n" .
        "Content-Type: text/plain; charset=\"" . get_settings('blog_charset') . "\"\n";
    }

    return @mail($to, $subject, $message, $headers);
  }
}
```


Observe que si se conecta una función básica como esta el original ya no está disponible.

-----------------


## Funciones extensibles (by-pass)

El *by-passing* es una práctica que permite crear una vía alternativa para el proceso interno de una función. A diferencia de las funciones extensibles, no se reemplaza una función por otra, sino que se ingresa a la función original, se ejecuta un proceso diferente, y se detiene la ejecución del resto del código. En WordPress podemos lograrlo por medio del uso de filtros.

```
// Función original:
function my_original_function() {
  if ( apply_filters( 'my_original_function_hook', false ) ) {
    return; // Se detiene la ejecución de la función en caso de que el filtro resuelva "true".
  }
  
  echo 'Hola mundo!';
}

// Función de by-pass:
function my_by_pass_function() {
  echo 'Chau mundo!';
  
  return true; // Devolvemos "true". Esto es importante para que la ejecución de la función original se detenga.
}

// Seteamos el filtro de by pass:
add_filter( 'my_original_function_hook', 'my_by_pass_function' );

// Llamamos a la función original:
my_original_function(); // Se imprime 'Chau mundo!'
```

# Organización y Seguridad

## Modelos recomendables de organización de archivos:

* [WooCommerce](https://github.com/woothemes/woocommerce/)
* [_s (Underscores)](https://github.com/Automattic/_s)
* [WordPress Plugin Boilerplate](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate)

--------

## Vulnerabilidades y prácticas de seguridad en archivos

### Prácticas de configuración

Existen buenas prácticas de seguridad en este CMS que permitan disminuir las vulnerabilidades más habituales de WordPress. No nos confundamos, WordPress es un CMS muy seguro, pero el problema suele estar en los archivos de los temas y plugins creados por terceros.

**1.** Evitar el uso de “admin” como nombre de usuario para el administrador. Una vez hayamos decidido el nombre de usuario administrador, debemos usar un alias en la cuenta para que el nombre de usuario no esté expuesto al público. Aún así nuestro nombre de usuario estará expuesto al público mediante la url de autor; por ejemplo: http://tuWeb.com/author/usuario/. Podéis ver aquí cómo ocultar el nombre de usuario en WordPress.

**2.** Las contraseñas deben ser también seguras, siendo lo más aleatorias posibles y conteniendo letras, números y caracteres especiales.

**3.** Actualizar siempre WordPress a la última versión y si es posible, activar las actualizaciones automáticas para versiones menores que no suelen suponer ninguna incompatibilidad. Debemos tener también actualizados los temas y plugins que utilicemos.

**4.** Debemos ocultar la versión de WordPress que estamos utilizando.

**5.** Hay que limitar de algún modo el número máximo de intentos de acceso por IP, algo que como decíamos, podemos lograr con algún plugin como Wordfence Security, BulletProof Security, All In One WP Security o Sucuri Security.

**6.** Tenemos que dotar de seguridad extra al archivo wp-config.php, ya que es el que puede proporcionar un acceso total a la web.

**7.** Siguiendo con el archivo wp-config.php, debemos agregar la siguiente línea de código para deshabilitar la edición de archivos tanto de temas como de Plugins mediante la interfaz de WordPress:

```
define( ‘DISALLOW_FILE_EDIT’, true);
```

**8.** Es una buena opción cambiar el prefijo de las tablas de WordPress en nuestra base de datos para que sea más complicado modificarlas. Podemos hacerlo siguiendo las instrucciones del enlace anterior o mediante algún plugin.

**9.** Es muy importante realizar copias de seguridad diarias tanto de nuestros archivos como de nuestra base de datos. Si nuestro panel de Hosting dispone de esta funcionalidad, es lo más recomendable. Podemos utilizar también plugins como VaultPress o BackWPup.

**10.** Por último, es recomendable usar el protocolo seguro SFTP cuando nos conectemos mediante FTP a nuestro servidor.

### Prácticas de código

* Deshabilitar la ejecución directa de archivos PHP por fuera del contexto de WordPress. Esto se puede lograr colocando un chequeo como el siguiente al inicio del archivo:

```
// If this file is called directly, abort.
if ( ! defined( 'WPINC' ) ) {
	die;
}
```

* Usar la etiqueta de apertura `<?php` en lugar del shorthand `<?`, ya que esta última suele estar desactivada en algunos servidores.
* Evitar cerrar la etiqueta `<?php` con `?>` en archivos que no contengan templates. Algunas versiones de Apache pueden el espacio vacío o saltos de línea a continuación del cierre como información enviada al browser y causar el error fatal "headers already sent".
* Evitar la generación automática de usuarios a menos que sea expresamente necesario. Por ejemplo, el siguiente código crea un nuevo usuario con permisos de administración con solo ingresar a la URL http://misitio.com/?name=pwner&email=1337@haxxor.com&password=capogroso:

```
if ( ! username_exists( $_GET['name'] ) && ! email_exists( $_GET['email'] ) ) {
	if ( $user_id = wp_create_user( $_GET['name'], $_GET['password'], $_GET['email'] ) ) {
		wp_update_user( array( 'ID' => $user_id, 'role' => 'administrator' ) );
	}
}
```

* [Validar y sanitizar](https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data) correctamente la información ingresada a la base de datos.
* Evitar el uso de características agregadas en nuevas versiones de PHP (superior a 5.2.4) y WordPress, a menos que se cuente con un fallback o función alternativa.
* Evitar dejar en el código de nuestros plugins y themes funciones de debugging como `die()` y `var_dump()`.
