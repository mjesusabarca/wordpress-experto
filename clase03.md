# Sidebars

Podemos crear barras laterales para un theme mediante una serie de funciones. Las funciones listadas a continuación  se utilizan para añadir barras laterales funcionales al tema.

**Registrar Varias Barras Laterales (Sidebars)**

```
register_sidebars( $count, $args );
```

Registra una o más barras laterales para utilizar en el tema actual. Muchos temas disponen de una única barra lateral. Por esta razón, el parámetro **count** es opcional y el valor por defecto es 1.

El parámetro **$args** se pasará a **register_sidebar() ** y sigue su formato, con la excepción del nombre, que es tratado con **sprintf()** para insertar o añadir un número único a cada barra lateral si count es mayor que uno.

Por ejemplo, la siguiente línea creará barras laterales con los nombres "Foobar 1" y "Foobar 2":

```
register_sidebars( 2, array( 'name' => 'Foobar %d' ) );
```

**Registrar una única Barra Lateral**

```
register_sidebar( $args );
```

El parámetro opcional **$args** es un array asociativo que será pasado como primer argumento en toda respuesta de widget activos. El uso básico de estos argumentos es pasar etiquetas HTML específicas de un theme para envolver al widget y a su título. Estos son los valores por defecto:

```
 'before_widget' => '<li id="%1$s" class="widget %2$s">',
 'after_widget' => '</li>',
 'before_title' => '<h2 class="widgettitle">',
 'after_title' => '</h2>',
```

**Mostrar la Barra Lateral**

```
dynamic_sidebar( $sidebar );
```

Esta función llama a cada una de las respuestas de widget activos por orden, e imprime el marcado para las barras laterales. Si se dispone de más de una barra lateral, se debería dar a esta función el nombre o el número de la barra lateral que se desea mostrar. Esta función resuelve `true` si genera HTML, o `false` si no tiene contenidos.

El valor de retorno podría utilizarse para determinar si se va a mostrar una barra lateral estática. Esto aseguraría que el theme se mostrará correctamente cuando no haya widgets activos. El siguiente es el uso recomendado de la función:

```
    <ul id="sidebar">
        <?php if ( dynamic_sidebar( 'primary' ) ) : ?>
            <li>{static sidebar item 1}</li>
            <li>{static sidebar item 2}</li>
        <?php endif; ?>
    </ul>
```
    
Si las barras laterales fueron registradas con un número, deberían ser recuperadas con un número. Si tenían nombres cuando se las registró, se deberían utilizar sus nombres.

# Menús de Navegación
En la sección *Appearance > Menus* podemos crear nuestros propios menús de navegación, los cuales podemos reutilizar como widgets, asignándolos a una sidebar. Sin embargo, existen casos en los cuales necesitamos arear navegables fijas dentro de nuestros themes. 

WordPress nos permite registrar estas áreas navegables por medio de las funciones `register_nav_menu()` y `register_nav_menus()`.

En el caso de `register_nav_menu()`, podemos registrar un área de navegación de esta manera:

```
<?php
add_action( 'after_setup_theme', 'register_my_menu' );

function register_my_menu() {
  register_nav_menu( 'primary', __( 'Primary Menu', 'theme-slug' ) );
}
```

El primer parámetro es un identificador interno para el área de navegación, al cual WordPress llama `$location`. Esto permite asignar un menú creado desde *Appearance > Menus* a una ubicación específica, que será justamente la nueva área de navegación. El segundo parámetro es el nombre público del área de navegación, es decir el que vemos en *Appearance > Menus* al seleccionar una ubicación para un menú.

También podemos crear múltiples áreas navegables en una sola operación usando la función `register_nav_menus()`. En este caso, como único parámetro de la función usamos un array. Cada elemento de ese array va a tener, como índice, el nombre interno de la ubicación, y como valor su nombre público.

```
<?php
add_action( 'after_setup_theme', 'register_my_menu' );

function register_my_menu() {
  register_nav_menus( array(
    'primary' => __( 'Primary Menu', 'theme-slug' ),
    'secondary' => __( 'Secondary Menu', 'theme-slug' ),
  ) );
}
```

# Tipos de Contenido
## Post Types predefinidos

Al construir una aplicación o un sitio web con cierto nivel de complejidad, es muy común que en algún momento se necesite algún tipo de contenido diferente de los que ya tenemos, alguna nueva entidad para la que sea posible crear, editar y modificar registros.

Si nuestra aplicación está basada en PHP y MySQL, normalmente se tiende a diseñar e implementar nuevas tablas en la base de datos para que contengan la información de las nuevas entidades. A continuación se escribe el código que gestione la carga, edición y eliminación de contenidos, el acceso de los usuarios de la aplicación a esos contenidos, y las relaciones e interacciones con otras entidades.

Lo que suele pasar con este enfoque es que, a medida que la aplicación se vuelve más grande y se necesitan más tipos de contenido, también se vuelve más complejo el mantenimiento de nuestras entidades, ya que sus datos y funcionalidad pueden variar mucho de una a otra, por tener diferentes características.

Sin embargo, si se trabaja con un framework, a menudo este suele proveer una serie de herramientas para crear nuevos tipos de contenido de una manera previamente definida y estandarizada, de forma que se fuerce una cierta coherencia interna para lo que vayamos a crear.

En el caso de WordPress, esta serie de herramientas esta cubierta por la API de ***[Post Types](https://codex.wordpress.org/Post_Types)***, la cual permite interactuar con tipos de contenidos nuevos o ya existentes siguiendo un conjunto de reglas y entendiendo ciertos conceptos fundamentales.

Por defecto, con su instalación básica, WordPress presenta cinco tipos de contenido: posts, páginas, revisiones, archivos y menús de navegación. Todos tienen diferencias entre ellos, las cuales se pueden notar al cargar o editar información, y todos están construidos usando la API de Post Types. Esta API permite manejar diferentes tipos de datos para diferentes tipos de contenidos de una forma estandarizada.

## Custom Post Types

Una necesidad típica al construir sitios con WordPress es la de gestionar tipos de contenido diferentes de los que se incluyen por defecto. Normalmente esto se logra con la instalación de un plugin que agregue un nuevo post type. También es común encontrarse con themes que lo hagan, pero no es una práctica recomendable, salvo en casos muy específicos, ya que al cambiar el theme se perdería el nuevo post type.

Para mostrar cómo funciona este tipo de solución, vamos a crear un plugin que agregue a WordPress características de portfolio. Este plugin va permitir que un usuario cargue sus proyectos de trabajo, con una imagen asociada a cada uno de ellos, y también va a ofrecer opciones para cargar el nombre del cliente del trabajo, su fecha de finalización, y un link externo al proyecto terminado.

A partir de este momento vamos a poner en práctica una recomendación típica al trabajar con WordPress y otros CMSs: prefijar los nombres de  funciones y clases con una palabra clave similar al nombre del componente que estamos creando. Nuestro plugin se va a llamar simplemente **Portfolio**, por lo cual el prefijo va a ser `portfolio_`. Hacemos esto para evitar colisiones con funciones de otros plugins, o incluso nativas de WordPress, que puedan llegar a tener el mismo nombre que las nuestras. Vale aclarar que esta es solo una de las maneras de resolver conflictos de nombres, y que a partir de PHP 5.3 podemos usar *namespaces*, pero no siempre podemos estar seguros de que nuestro plugin va a ser instalado en un servidor con PHP 5.3 o superior, por lo cual tenemos que evaluar qué es lo más conveniente. Otra opción sería decidir que nuestro plugin va a soportar una versión mínima de PHP, que puede ser 5.3 o superior. Por el momento vamos a usar prefijos, ya que es la solución más comúnmente usada.

```php
<?php
add_action( 'init', 'portfolio_create_project_post_type' );

function portfolio_create_project_post_type() {
	register_post_type( 'project', array(
			/**
			 * @arg $labels
			 * 
			 * Define etiquetas de texto utilizadas por el post type.
			 */
			'labels' => array(
				'name' => __( 'Projects', 'portfolio' ),          // Nombre (en plural) para este tipo de contenido.
				'singular_name' => __( 'Product' , 'portfolio' ), // Nombre (en singular) para este tipo de contenido.
			),
			/**
			 * @arg $supports
			 * 
			 * Define características soportadas por el post type.
			 */
			'supports' => array(
				'title',     // Permite ingresar título
				'editor',    // Permite usar el editor de texto
				'author',    // Permite definir y modificar el autor
				'excerpt',   // Permite usar el campo de extracto de la entrada
				'thumbnail', // Permite ingresar imagen destacada
				'comments'   // Permite ingresar comentarios en la entrada
			),
			'public' => true, // Hace accesibles las entradas desde el front-end.
			'has_archive' => true, // Hace accesible el sumario de entradas de este tipo.
		)
	);
}
```

Para crear un nuevo post type, vamos a usar la función `register_post_type()` al momento de ejecutarse el evento `init`. La función va a recibir dos parámetros: el primero es el nombre interno del post type, y el segundo es un array conteniendo su descripción. Dentro de este array podemos usar una serie de valores bastante completa, a partir de los cuales nuestro post type va a contar o no con ciertas características. Todos estos valores se describen con detalle en la [documentación oficial de la función](https://codex.wordpress.org/Function_Reference/register_post_type), y en este momento solamente vamos a usar aquellos que nos van a permitir que nuestros proyectos se vean de la manera más similar posible a un post. Vamos a definir el nombre del post type en plural y en singular, vamos a indicar cuáles de los campos predefinidos por WordPress necesitamos, y a indicar que las entradas que generemos puedan verse desde la parte pública de nuestro sitio, y también a través de una página de archivo.

De esta manera, cuando guardemos nuestro código, vamos a ver una nueva sección en el menú de administración: ***Projects***. Podemos cargar un nuevo proyecto, con su correspondiente título, contenido, extracto, autor e imagen destacada, y una vez que lo guardemos WordPress nos va a dar un link para visualizarlo en el front-end. Si todo anduvo bien, deberíamos ver algo muy parecido a un post, quizás con algunas diferencias menores, dependiendo del theme que estemos usando.

# Taxonomías

Los posts cuentan con dos tipos de taxonomías para organizar contenidos: las categorías y las etiquetas (o tags). La diferencia fundamental entre estos dos tipos de taxonomías es que las categorías pueden organizarse jerárquicamente, es decir que puede haber categorías principales, y además otras secundarias que las tengan como padres, mientras que las etiquetas están siempre al mismo nivel.

El problema con el que nos encontramos al trabajar con *custom post types* es que no tenemos disponibles por defecto esas dos taxonomías para ordenar nuestros nuevos contenidos. Para hacerlo necesitamos crear taxonomías propias. Y con ese fin es que WordPress ofrece la función `register_taxonomy()`.

Usarla es muy sencillo: como primer parámetro necesitamos el nombre que va a tener nuestra nueva taxonomía (podemos usar *"Category"*, *"Tag"*, o cualquier otro que se nos ocurra); en el segundo parámetro debemos especificar el nombre del post type al que la vamos a aplicar (o un array de post types, si vamos a aplicarla a más de uno); y nuestro tercer parámetro (opcional) es una lista de argumentos con las características de la taxonomía. Entre ellos, si va a ser jerárquica o no, y una lista de textos que WordPress va a mostrar en diferentes partes del sitio web cuando se use la taxonomía. El detalle de todo lo que se puede usar dentro de estos parámetros se puede ver en la [documentación oficial](https://codex.wordpress.org/Function_Reference/register_taxonomy).

Vamos a agregar, entonces, dos taxonomías: una que se comporte de la misma manera que las categorías de posts, y otra que se comporte como las etiquetas.

```php
<?php
add_action( 'init', 'portfolio_register_category_taxonomy' );

function portfolio_register_category_taxonomy() {
	register_taxonomy( 'project_category', 'project' , array(
			'labels' => array(
				'name'          => __( 'Categories', 'portfolio' ),
				'singular_name' => __( 'Category', 'portfolio' ),
			),
            'hierarchical' => true,
		) 
	);
}

add_action( 'init', 'portfolio_register_tag_taxonomy' );

function portfolio_register_tag_taxonomy() {
    register_taxonomy( 'project_tag', 'project' , array(
            'labels' => array(
                'name'          => __( 'Tags', 'portfolio' ),
                'singular_name' => __( 'Tag', 'portfolio' ),
            ),
            'hierarchical' => false,
        ) 
    );
}

```

Al guardar el código y recargar la sección de administración, vamos a ver que bajo la sección *Projects* aparecen dos nuevos links: *Categories* y *Tags*. Accediendo a cada uno de ellos vamos a poder crear nuestras propias categorías y etiquetas, y asignárselas a nuestros proyectos. Estos nuevos datos que ingresamos como categorías y etiquetas son manejados por WordPress internamente como términos, o *terms*, y podemos ver la lista completa de funciones relacionadas con términos en la [documentación de la función `get_term()`](https://codex.wordpress.org/Function_Reference/get_term).

Con esto ya podemos asignar categorías y etiquetas a nuestros proyectos, pero todavía no podemos verlas en el front-end del sitio. Para resolver esto, una posible solución es filtrar la información del evento `the_content`.

```php
<?php
add_filter( 'the_content', 'portfolio_project_categories', 20 );

function portfolio_project_categories( $content ) {
	if ( taxonomy_exists( 'project_category' ) ) {
		$taxonomy = get_taxonomy( 'project_category' );
		$terms    = get_the_terms( get_the_ID(), 'project_category' );

		if ( is_array( $terms ) ) {
			$category_links = array();
 
			 foreach ( $terms as $term ) {
				 $category_links[] = '<a href="' . esc_url( get_term_link( $term ) ) . '">' . $term->name . '</a>';
			 }

			$categories = join( ', ', $category_links );

			$content .= '<div class="portfolio-project-taxonomy"><strong>' . $taxonomy['labels']['name'] . '</strong> ' . $categories . '</div>';
		}
	}

	return $content;
}

add_filter( 'the_content', 'portfolio_project_categories', 30 );

function portfolio_project_tags( $content ) {
	if ( taxonomy_exists( 'project_tag' ) ) {
		$taxonomy = get_taxonomy( 'project_tag' );
		$terms    = get_the_terms( get_the_ID(), 'project_tag' );

		if ( is_array( $terms ) ) {
			$tag_links = array();
 
			 foreach ( $terms as $term ) {
				 $tag_links[] = '<a href="' . esc_url( get_term_link( $term ) ) . '">' . $term->name . '</a>';
			 }

			$tags = join( ', ', $tag_links );

			$content .= '<div class="portfolio-project-taxonomy"><strong>' . $taxonomy['labels']['name'] . '</strong> ' . $tags . '</div>';
		}
	}

	return $content;
}
```

Lo que hacemos en ambas funciones es chequear si la taxonomía ya existe. En caso afirmativo, obtenemos sus datos y la lista de términos que hayan sido creados bajo dicha taxonomía. A continuación construimos el código HTML que se va a agregar al contenido del proyecto, incluyendo el nombre de la taxonomía (*Category* y *Tag*) y una lista con los nombres de todos los términos creados para la misma, cada uno enlazando a su respectivas página de archivo. Una vez armada la pieza de código HTML, la agregamos al final del contenido original del post.

# Internacionalización y Localización

Una posibilidad que WordPress ofrece al hacer la instalación es elegir el lenguaje en el que se va a mostrar nuestro sitio. Gracias a esta opción, si se selecciona un lenguaje que no sea inglés, WordPress va a traducir automáticamente todos los textos que estén preparados para ser mostrados en el idioma elegido.

Una recomendación típica de extensibilidad es que todos los textos escritos en el código de plugins y themes sean traducibles, es decir que deben ser internacionalizados. De esta manera, alguien que quiera traducir un plugin internacionalizado a su propio idioma va a poder hacerlo simplemente siguiendo unos pocos pasos.

Para que un plugin pueda ser traducido hacen falta dos cosas: en primer lugar, contar con una palabra clave para que los textos que introducidos puedan ser reconocidos como propios del plugin; en segundo lugar que los textos sean invocados con las funciones de localización que provee WordPress. Adicionalmente se puede ofrecer una plantilla para traducciones. Esto no es obligatorio, pero resulta muy útil para los traductores.

El requisito de la palabra clave es el más sencillo de cumplir: basta con agregarla como dato en el encabezado del plugin, bajo el nombre "Text Domain". Por convención, el nombre del dominio suele ser igual al del plugin, pero en minúsculas y con los espacios separados por guiones. Este valor también suele ser igual al nombre de la carpeta del plugin.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 */
```

Opcionalmente, se puede especificar, en el valor "Domain Path", la ruta relativa a la carpeta donde se encuentren los archivos de traducción que podamos ofrecer por defecto.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 * Domain Path: languages/
 */
```

Una vez que la tenemos, debemos usar la función `load_textdomain()` para que se carguen automáticamente todas las posibles traducciones existentes de nuestro plugin.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 * Domain Path: languages/
 */

load_plugin_textdomain( 'my-plugin' );
```

Las funciones de localización son varias, aunque las más comunes son `__()` y `_e()`. `__()` devuelve la traducción del texto que se le pasa como primer parámetro, mientras que `_e()` la imprime. Cada vez que se las llame se debe especificar, como segundo parámetro, el dominio al cual pertenece el texto a traducir. Teniendo eso en cuenta, puede retomarse alguno de los ejemplos de código anteriores e internacionalizarlo.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 * Domain Path: languages/
 */

load_plugin_textdomain( 'my-plugin' );

add_action( 'my_action', 'my_action_callback', 20, 1 );

function my_action_callback() {
	_e( 'Hello world!', 'my-plugin' );
}

add_filter( 'my_filter', 'my_filter_callback', 20, 1 );

function my_filter_callback( $value ) {
	return __( 'some other value', 'my-plugin' );
}

$my_value = apply_filters( 'my_filter', 'some value' );

do_action( 'my_action' );
```

Un problema con el que es común encontrarse es que parte del texto a traducir dependa de valores variables. Esto es justamente un problema porque `gettext`, la librería que usa WordPress para gestionar sus traducciones, no puede leer información dinámica dentro de las cadenas de texto a traducir. Por lo tanto, algo así no puede ser traducido:

```php
<?php
_e( 'Hello World! The current year is ' . date( 'Y' ), 'my-plugin' );
```

Para resolver este problema, podemos combinar nuestros textos a traducir con las funciones nativas de PHP `printf()` y `sprintf()`. El ejemplo anterior se puede replantear de manera correcta de esta forma:

```php
<?php
printf( __( 'Hello World! The current year is %s', 'my-plugin' ), date( 'Y' ) );
```

Por último, se puede ofrecer una plantilla con textos localizables. La plantilla se trata de nada más que un archivo de texto con el nombre del dominio y con extensión POT (*Portable Object Template*), y un formato como el siguiente:

```
# my-plugin.pot
msgid "Hello world!"
msgstr ""

msgid "some other value"
msgstr ""
```

La propiedad `msgid` corresponde al texto original a traducir, y `msgstr` es la traducción propiamente dicha. En nuestro template este segundo valor va a estar en blanco, porque todavía no es una traducción, sino una herramienta para desarrollar nuevas traducciones.

Muchas veces podemos contar con un número bastante grande de textos a traducir, por lo cual no es muy viable incluir todos manualmente en el archivo POT. Para esto tenemos programas que pueden generar el archivo POT de manera automática, como por ejemplo [Poedit](https://poedit.net/).

Estos son simplemente los conceptos básicos de internacionalización y localización, y es todo lo que hace falta saber al momento de crear plugins que puedan ser traducidos.
