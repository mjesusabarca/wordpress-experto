# Links Útiles para Clase 5
## PHP

* [Variables globales](http://php.net/manual/en/reserved.variables.globals.php)
* [Alcance de variables](http://php.net/manual/en/language.variables.scope.php)
* [Funciones anónimas](http://php.net/manual/en/functions.anonymous.php)
* [Programación Orientada a Objetos](http://php.net/manual/en/language.oop5.php)

## WordPress

* Tiempos de ejecución: [*WordPress for the adventurous: Loading*](https://carlalexander.ca/wordpress-adventurous-loading/).
* Base de datos: [$wpdb](https://codex.wordpress.org/Class_Reference/wpdb)
* [Shortcodes](https://codex.wordpress.org/Shortcode_API)
* Creación de loops: [WP_Query](https://codex.wordpress.org/Class_Reference/WP_Query) y [get_posts()](https://codex.wordpress.org/Template_Tags/get_posts)
* Manejo de posts: [WP_Post](https://codex.wordpress.org/Class_Reference/WP_Post)
* Personalización de themes:
  * [Theme Features](https://codex.wordpress.org/Theme_Features)
  * [Theme Modification API](https://codex.wordpress.org/Theme_Modification_API)
  * [Theme Customizer API](https://developer.wordpress.org/themes/advanced-topics/customizer-api/)
  
  #  Manipulación de Base de Datos

WordPress define una clase llamada `wpdb`, la cual contiene un conjunto de métodos que se utilizan para interactuar con una base de datos. Su objetivo principal es facilitar la interacción con la base de datos de WordPress, pero se puede utilizar para comunicarse con cualquier otra base de datos.

La conexión con la base de datos de una instalación de WordPress se establece a partir de la información ingresada en el archivo `wp-config.php`. Al establecer esta conexión, WordPress crea, a partir de la clase `wpdb` una variable global llamada `$wpdb`. Esta variable permite interactuar de manera directa con la base de datos a través de los métodos definidos en la clase `wpdb`.

### Selección de una variable

El método `wpdb::get_var()` devuelve un valor único por medio de una consulta a la base de datos. Se usa en casos en los que se consulta por un único valor.

**Ejemplo:**

```
<?php
$user_count = $wpdb->get_var( "SELECT COUNT(*) FROM $wpdb->users" );
echo '<p>User count is ' . $user_count . '</p>';
```

### Selección de una fila

Para recuperar una fila completa de una consulta se utiliza `wpdb::get_row()`. El método puede devolver la fila como un objeto, un array asociativo, o un array de índice numérico.

**Ejemplo:**

```
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", OBJECT ); // Devuelve un objeto.
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", ARRAY_A ); // Devuelve un array asociativo.
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", ARRAY_N ); // Devuelve un array de índice numérico.
```

Los valores de la variable `$row` son los nombres de las columnas del resultado de la consulta SQL (en este ejemplo, todas las columnas de la tabla `wp_links`, pero también se podría consultar por columnas específicas).

De esta manera, si variable es un objeto, podríamos obtener el valor de la columna `link_id` de esta manera:

```
echo $row->link_id; // Imprime "10"
```

En caso de que la variable sea un array asociativo `link_id` se obtendría así:

```
echo $row['link_id']; // Imprime "10"
```

Y para un array de índice numérico:

```
echo $row[1]; // Imprime "10"
```

### Selección de una columna

Para seleccionar una columna, contamos con el método `wpdb::get_col()`. Este método devuelve un array de índice numérico con la cantidad de resultados especificados en la consulta (o todos, si no se especifica un límite). En el siguiente ejemplo se seleccionan 10 IDs de la tabla `wp_posts`:

```
<?php
$col = $wpdb->get_col( "SELECT ID FROM $wpdb->posts LIMIT 0, 10" );
```

### Inserción de una fila

Para insertar una nueva fila en una tabla se usa el método `wpdb::insert()`.

Suponiendo una tabla con el nombre `table` y con dos columnas llamadas `column1` y `column2`, podemos asignar nuevos datos para esas columnas de la siguiente manera:

```
$wpdb->insert( 
	'table', 
	array( 
		'column1' => 'value1', 
		'column2' => 123 
	), 
	array( 
		'%s', 
		'%d' 
	) 
);
```

### Eliminación de una fila

El método `wpdb::delete()` se puede utilizar para borrar registros de una tabla. Su uso es muy similar a `wpdb::insert()`. Devuelve el número de filas actualizadas o `false` en caso de error.

**Ejemplo:**

```
$wpdb->delete ( 'table', array ( 'ID' => 1 ), array ( '%d') );
```

# Shortcodes


Los shortcodes de WordPress son pequeños códigos que puedes añadir en el editor de WordPress. Se usan para añadir funciones al contenido de tus entradas y páginas sin tener que escribir un script cada vez que necesites hacer esa tarea.

Son como tags HTML que usan corchetes ([ ]) en vez de los símbolos de “mayor y menor qué” (< >). Vamos, algo así:

```
[shortcode]
```

Lo primero es elegir el archivo PHP que “acogerá” las funciones de los shortcodes. La elección más habitual es el fichero functions.php del tema activo, que casi todos incluyen, y sino lo creas manualmente. También puedes crear tu propio plugin de funciones, una opción habitualmente más limpia y controlable.

Los dos pasos básicos son estos:

Crea la función primaria PHP

Crea el conector add_shortcode, cuya función es conectar la función primaria PHP a WordPress (creas el shortcode para la función PHP y luego lo insertas en el editor de WordPress para que active esa función)
Como he apuntado, una vez insertes el [shortcode] en tu editor, WordPress se conectará con el archivo functions.php o plugin de funciones y reemplazará de manera automática el shortcode con la función que creaste.

Una vez hayas creado las funciones, sean pocas o muchas, todo lo que tienes que hacer es insertar el shortcode único asociado a cada función cada vez que lo quieras usar en tu editor.

1.  Abrir el archivo de funciones (functions.php) de tu tema, normalmente en /wp-content/themes/tutema/functions.php, o el plugin de funciones y crear la función.

Un ejemplo sencillo sería algo así:

```
function shortcode_gracias() {
	return '<p>¡Gracias por todo!</p>';
}
add_shortcode('gracias', 'shortcode_gracias');
```

Guarda los cambios del archivo de funciones, ya estás listo para usar tu nuevo shortcode.

2. Pon el nuevo shortcode [gracias] en el lugar que desees de tu editor de WordPress.

Una vez guardes los cambios en tu página se vería el resultado de la función “llamada” por el shortcode, viéndose algo así:

```
¡Gracias por todo!
```

Ahora veremos unos ejemplos de uso de Shorcodes:

**Shorcodes en widgets de texto**

Lo primero es añadir este código al fichero functions.php de tu tema activo:

```
add_filter( 'widget_text', 'shortcode_unautop');
add_filter( 'widget_text', 'do_shortcode');
```

La segunda línea es la que hace que funcionen los shortcodes en el widget de texto. Además, hay que tener en cuenta que los widgets de texto tienen una casilla para “añadir saltos de párrafo automáticamente”, pues bien, la segunda linea inhabilita el código autop que podría, en caso de estar marcada la casilla, meter el shortcode introducido en un párrafo o incluso romper las tags.

**Shortcodes en el tema**

También se pueden usar shortcodes en el tema que uses. Para ello utilizamos la función do_shortcode() en la que el argumento será una cadena que contendrá el shortcode.

Por ejemplo, para mostrar el shortcode [publicidad] en tu tema pondrías algo así donde quieras que aparezca el resultado esperado:

```
<?php do_shortcode('[publicidad]'); ?>
```

La función do_shortcode() acepta cualquier tipo de texto. Si la cadena contiene un shortcode procesará ese código. De este modo, por ejemplo, podrías mostrar manualmente contenido de tus entradas de este modo:

```
<?php
$content = get_the_content();
echo do_shortcode($content);
?>
```

**Shortcodes en comentarios**

De nuevo recurrimos al fichero functions.php para añadirle este código que permite shortcodes en los campos de comentarios:

```
add_filter( 'comment_text', 'shortcode_unautop');
add_filter( 'comment_text', 'do_shortcode' );
```

**Shortcodes en extractos**

Ahora, en functions.php añadiremos esto para poder insertar shortcodes en los extractos de entrada:

```
add_filter( 'the_excerpt', 'shortcode_unautop');
add_filter( 'the_excerpt', 'do_shortcode');
```

**Shortcodes en la descripción de usuario**

Para poder meter shortcodes en la descripción del usuario hay que pasar la cadena de descripción por la función do_shortcode(). Para ello tendrás que añadir esto a tu tema activo:

```
<?php
// $user_id = 3;
$userdata = get_userdata($user_id);
echo do_shortcode($userdata->description);
?>
```
Solo tendrás que cambiar el ID de usuario, en este caso el 3.

**Shortcodes en descripciones de etiquetas, categorías y taxonomías**

También puedes filtrar estas descripciones. Para ello recurrimos de nuevo al fichero functions.php:

```
add_filter( 'term_description', 'shortcode_unautop');
add_filter( 'term_description', 'do_shortcode' );
```

# Personalización de Theme

## Theme mods (Get-Remove-Set)

### `get_theme_mod()`

Es posible  recuperar un ajuste de modificacion para el theme actual. Junto con `set_theme_mod()` esta función a veces ofrece a los desarrolladores una alternativa más simple a la Options API cuando existe la necesidad de manejar ajustes básicos sobre themes concretos.

#### Uso

```
<?php get_theme_mod( $name, $default ); ?>
```

#### Ejemplo

```
.footer {
     border-top: solid 1px #<?php echo get_theme_mod( 'background_color', 'red' ); ?>;
}

```

##  `get_theme_mods()`


Esta función permite recuperar todos los valores de modificaciones para el theme actual.

#### Uso

```
<? php get_theme_mods();  ?>

```

#### Ejemplo

```
$mods = get_theme_mods();
var_dump($mods);
// output example:
//    array(2) { ["header_textcolor"]=> string(3) "333" ["header_image"]=> string(20) "random-default-image" }
var_dump($mods['header_textcolor']);
// output example:
//    string(3) "333"
```

##  `remove_theme_mod()`

Remueve el nombre de la modificación del tema desde la lista del tema actual.

Si la remoción del nombre remueve todos los elementos, toda la opción por defecto será eliminada

#### Uso

```
<?php remove_theme_mod( $name ) ?>
```

##  `remove_theme_mods()`

Elimina todas las opciones de modificación para el theme actual.

#### Uso

```
<?php remove_theme_mods() ?>
```

##  `set_theme_mod()`

Crea o actualiza una opción de modificación para el theme actual.

#### Uso

```
<? php set_theme_mod (  $nombre,  $valor  );  ?>
```

---

## Theme Customizer

El Personalizador de temas es un marco para la vista previa y en vivo de cualquier cambio realizado a WordPress. Este proporciona una interfaz simple y consistente para que los usuarios puedan personalizar diversos aspectos de su tema y su sitio, tales como los colores y diseños para los widgets, menús y mucho más. También es posible agregar opciones personalizadas de themes y plugins al personalizador. En resumen, el personalizador es una herramienta sencilla que nos permite añadir opciones a nuestro tema.  

Una de las opciones del personalizador es conceder a los usuarios diferentes capacidades de manejo del mismo, en donde la mayoría de las opciones sólo son visibles para los administradores en forma predeterminada, mientras que otros usuarios pueden acceder a ciertas opciones si el administrador considera y cree que son capaces de hacerlo.

Para añadir sus propias opciones para el personalizador, es necesario utilizar un mínimo de 2 ganchos (hooks):

**customize_register**
Este gancho permite definir nuevos paneles para el personalizador: secciones, los ajustes y controles.

**wp_head** 
Este gancho  permite que la salida de CSS generada  se muestre correctamente en el sitio en línea.


### Añadir el Personalizador de temas al menú de administración

**Nota :** El primer paso ya no es necesario con las nuevas versiones de WordPress . El personalizador se añade automáticamente al menú incluso si el tema no lo usa . Por lo tanto saltamos al siguiente paso.

Primero abriremos el archivo **functions.php** del tema y agregamos el código siguiente . Realmente no importa en qué parte del archivo **functions.php** colocamos el código con tal de que no está dentro de otra función .

```
function example_customizer_menu() {
    add_theme_page( 'Customize', 'Customize', 'edit_theme_options', 'customize.php' );
}
add_action( 'admin_menu', 'example_customizer_menu' );
```

Aquí creamos un nuevo título  de función, " **example_customizer_menu** " y lo adjuntamos a  " **admin_menu** " de acción 


Esta función tiene sólo una línea , y se llama a la función **add_theme_page ( )** para añadir páginas a la zona de administración de WordPress . Esta función tiene cuatro parámetros requeridos .


**Function**: add_theme_page( $page_title, $menu_title, $capability, $menu_slug, $function );

**$page_title	- string	**

Este será el texto del título de la nueva página . Sin embargo , la página del personalizador no tiene un título, así que supongo que esto no importa demasiado.

**$menu_title	- string	**

Este será el texto del nuevo elemento de menú en el menú " Apariencia " .

**$capability	- string	**

Esto hace que el usuario tenga los permisos adecuados para acceder al personalizador

**$menu_slug -	string	**

El único slug del menú único para esta página.

### Añadir una nueva sección al personalizador

Para crear nuestra propia configuración de la sección , vamos a colocar el siguiente fragmento de código en el archivo **functions.php** justo debajo del código que agregó anteriormente ( una vez más , la colocación no importa mucho , es simplemente más fácil añadir el código en orden).

```
function example_customizer( $wp_customize ) {
    $wp_customize->add_section(
        'example_section_one',
        array(
            'title' => 'Example Settings',
            'description' => 'This is a settings section.',
            'priority' => 35,
        )
    );
}
add_action( 'customize_register', 'example_customizer' );
```

**Tenga en cuenta :** Usted no será capaz de ver la configuración de la sección hasta que contenga al menos un ajuste .


Ahora bien, hemos unido nuestra función a la acción " **customize_register** " para garantizar que se ejecute en el momento adecuado.

El objeto **$wp_customize** se pasa a nuestra función y podemos utilizar sus métodos para agregar secciones, ajustes y controles para el personalizador de temas .

Vamos a empezar por la adición de una sección utilizando el método **add_section () .** Este método acepta dos parámetros.


Método: add_section( $id, $args );

**$id -	string**

El ID único de esta sección

**$args	- array**

Conjunto de argumentos pasados a la función **add_section ( ) .**

El array **$args** nos permite especificar una serie de detalles acerca de la configuración de la sección que estamos creando .

Array: $args

**title -	string**	

Título de la sección de configuración .

**description	- string**	

Opcional. Descripción de la sección de configuración . Esto sólo se muestra como un consejo cuando se pasa el ratón sobre el título de la sección .

**priority	- integer**

Opcional. La prioridad determina el orden en el que se mostrarán las secciones . Los números más bajos son los primeros.

**capability	- string**

Opcional. Muestra u oculta la sección sobre la base de los niveles de permisos del usuario.

**theme_supports	- string**

Opcional. Muestra u oculta la sección en función de si el tema es compatible con una característica en particular .
