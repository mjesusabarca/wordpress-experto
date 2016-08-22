# Links Útiles para Clase 6
 
* [Activation / Deactivation Hooks](https://developer.wordpress.org/plugins/the-basics/activation-deactivation-hooks/)
* [Transientes API](https://codex.wordpress.org/Transients_API)
* [Widgets API](https://codex.wordpress.org/WordPress_Widgets)

# Activación y Desactivación de Plugins
WordPress permite a los desarrolladores programar procesos que solamente deban ejecutarse una vez, durante el momento de activación o desactivación de un plugin. Esto es una herramienta muy útil tanto para la creación como para la limpieza de datos que un plugin puede llegar a generar, ya que con mucha frecuencia se necesita generar nuevas opciones en la base de datos de WordPress, o incluso crear nuevas tablas, y cabe la posibilidad de que esa información ya no se necesite una vez desactivado el plugin.

Los procesos de activación y desactivación se ejecutan a través de eventos. Podemos asignar procesos a esos eventos por medio de ciertas funciones similares a `add_action()`, pero que solamente van a ejecutarse cuando se opte por desactivar o desactivar un plugin.

## Proceso de Activación

Para asignar procesos al evento de activación usamos la función `register_activation_hook()`. Esta función va a recibir como primer parámetro la ruta absoluta al archivo principal del plugin (es decir aquel en el cual declaramos sus datos) en el servidor, y como segundo parámtro un callback, es decir el nombre de la función o método que se quiera ejecutar.

**Ejemplo:**

```
<?php
if ( ! function_exists( 'portfolio_activation' ) ) :
register_activation_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_activation' );
/**
 * Process plugin activation.
 *
 * @since 1.0
 */
function portfolio_activation() {
	add_option( 'portfolio_settings', array(
		'portfolio_page'         => null,
		'show_logged_only'       => true,
		'remove_on_deactivation' => false,
	) );
}
endif;
```

En el ejemplo se crea una nueva opción con la configuración por defecto del plugin. Nótese que, si el llamado a `register_activation_hook()` se encuentra en el archivo principal del plugin, el primer parámetro puede ser sustituido por `__FILE__`. En el ejemplo se presupone que se definió anteriormente una constante llamada `PORTFOLIO_PLUGIN_MAIN_FILE`, a la cual se le asignó `__FILE__` como valor.

## Proceso de Desactivación

Para asignar procesos de desactivación, la función indicada es `register_deactivation_hook()`. Se usa de la misma manera que la función de activación: el primer parámetro es la ruta del archivo principal del plugin, y el segundo parámetro es el callback que se desea ejecutar al desativar el plugin.

**Ejemplo:**

```
<?php
if ( ! function_exists( 'portfolio_deactivation' ) ) :
register_deactivation_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_deactivation' );
/**
 * Process plugin deactivation.
 *
 * @since 1.0
 */
function portfolio_deactivation() {
	$settings = get_option( 'portfolio_settings' );
	if ( $settings['remove_on_deactivation'] ) {
		delete_option( 'portfolio_settings' );
	}
}
endif;
```

En este ejemplo se eliminan los valores de configuración del plugin si la opción `remove_on_deactivation` está activada.

## Extra: Proceso de desinstalación:

Adicionalmente, también contamos con una función que permite ejecutar procesos al momento de desinstalar un plugin, es decir cuando elegimos borrar sus archivos a través de WordPress.  Dicha función es `register_uninstall_hook()`, y se usa de la misma manera que las funciones de activación y desactivación.

**Ejemplo:**

```
<?php
if ( ! function_exists( 'portfolio_uninstall' ) ) :
register_uninstall_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_uninstall' );
/**
 * Process plugin uninstall.
 *
 * @since 1.0
 */
function portfolio_uninstall() {
	delete_option( 'portfolio_settings' );
}
endif;
```

En el ejemplo forzamos la eliminación de las opciones del plugin al desinstalarlo.

Sin embargo, este proceso de desinstalación acarrea un posible problema: si contamos con un hook de desinstalación, para que el proceso se ejecute, WordPress necesita ejecutar el archivo principal del plugin una vez más, lo cual puede derivar en la ejecución completa del plugin, y llegar a provocar resultados no deseados después de haberlo desactivado. Por esto, WordPress cuenta con un nombre de archivo reservado, `uninstall.php`, que podemos incluir en la carpeta de nuestro plugin, y que, en caso de existir, se ejecutará en lugar del hook de desinstalación. En este archivo ya no hace falta incluir nuestro proceso de desintalación dentro de una función, sino que podemos ejecutarlo de manera directa.

**Ejemplo de `uninstall.php`:**

```
<?php
/**
 * Process plugin uninstall.
 *
 * @since 1.0
 */
delete_option( 'portfolio_settings' );
```

# Transients API
## Persistencia de datos (Transients)

Cada vez que nuestra web necesita datos tiene que hacer una llamada a su base de datos para conseguirlos. Por ejemplo, imaginad que necesitamos todas aquellas entradas personalizadas del tipo “producto” cuyo campo personalizado “color” no sea “azul”:

Esto supone una llamada a la base de datos, con el consumo de recursos y de tiempo correspondiente. Y si tenemos muchos visitantes tenemos que hacer esta llamada una y otra vez. Esto afecta al rendimiento de nuestra página, y si la llamada a la base de datos es muy compleja puede hasta tirarnos abajo la web si tenemos un tráfico de visitantes muy grande para el que no estamos preparado:

```
$args = array(
	'post_type' => 'producto',
	'meta_query' => array(
		array(
			'key' => 'color',
			'value' => 'azul',
			'compare' => 'NOT LIKE'
		)
	)
 );
$productos = new WP_Query( $args );
```

El Transients API nos permite almacenar información para usarla posteriormente, mejorando el rendimiento de tu página.

¿Dónde se guardan los datos cuando usamos el Transients API? En principio se guardan en la misma base de datos, en la tabla wp_options. Estaréis pensando que vaya un sistema de caché que para evitar llamadas a la base de datos guarda los resultados otra vez en la base de datos… Pues sí, es cierto. Aunque hay mejora, ya que guarda el resultado, por lo que la llamada a la base de datos será ahora más sencilla.

Pero para aprovechar realmente el Transients API debemos utilizar algún plugin como W3 Total Cache. ¿Por qué? porque en este caso los resultados ya no se almacenan en la base de datos, sino en una memoria rápida. Por esta razón los desarrolladores de WordPress nos recomiendan que usemos transients para almacenar información que va a caducar con el tiempo. También debemos recordar en este caso que los datos del transients no se van a volver a almacenar en la base de datos, por lo que no debemos intentar obtenerlos de la misma.

También podemos utilizar el Transients API para almacenar información externa a nuestra web, ya que hay veces que lo que demora la carga de la misma puede ser el conectar con un servidor externo. Por ejemplo, podemos almacenar usando el Transients API información procedente de Facebook, de Twitter, etc..
<br>
### Funciones del Transients API

¿Cómo usamos el Transients API? Pues la gran ventaja es que es muy facil de usar. Hay que usar dos funciones: set_transient() para crear el transient y get_transient() para obtenerlo. Para WordPress multiusuario tenemos también set_site_transient(), que crea el transient para toda la red, y get_site_transient(), que te permite obtenerlo en cualquier parte de la red.

Hay otra función importante: delete_transient() o delete_site_transient(), que permite borrar el transient. Esto es útil cuando queremos que la información se refresque porque hay nuevas entradas. Borramos el transient y así hacemos que se regenere con la nueva información.
<br>
#### Ejemplo:

Vamos a seguir con el ejemplo de más arriba. Supongamos que queremos utilizar el Transients API para almacenar la información resultante de esa llamada a la base de datos. Habría que modificar un poco el código, como veréis a continuación:

```
// Obtenemos la copia del transient data
if ( false === ( $productos = get_transient( "productos" ) ) ) {

    // Si no estaba lo regeneramos
    $args = array(
	'post_type' => 'producto',
	'meta_query' => array(
	    array(
	       'key' => 'color',
	       'value' => 'azul',
	       'compare' => 'NOT LIKE'
	    )
	)
    );
    $productos = new WP_Query( $args );
    set_transient( "productos", $productos, 60*10 ); // Lo mantenemos 10 minutos
}                

// Comenzamos el loop
while ( $productos->have_posts() ) : $productos->the_post();
```
<br>
**Vamos a ver línea a línea:**

* En la línea 2 usamos la función get_transient() para obtener el transient llamado “productos”. En el caso de que no exista, el resultado que nos va a dar será “FALSE”, por eso ponemos el condicional. Si es FALSE (porque hemos borrado el transient o porque el mismo ha caducado) tenemos que volver a generarlo. Es decir, una vez caducado o borrado el transient, al entrar un nuevo usuario en la web se hará otra vez la llamada a la base de datos y se creará de nuevo el transient.

* En las líneas 4 a 15 generamos el query tal y como lo haríamos normalmente.

* En la línea 16 usamos la función set_transient() para generar el nuevo transient. Como veis, esta función necesita de varios parámetros: En primer lugar el nombre que le vamos a dar a este transient concreto. El nombre debe tener menos de 45 caracteres. En segundo lugar, el valor que se da a dicho transient, que en nuestro caso es el resultado del query. En tercer lugar (opcional), el tiempo que queremos que dure el transient en segundos. Por ejemplo para un día sería: ( 60 * 60 * 24 ). En nuestro ejemplo lo mantenemos 10 minutos.

* En la línea 20 comenzamos el loop como lo haríamos normalmente.
<br>

WordPress 3.5 ha añadido nuevas variables para facilitarnos las cosas:
* MINUTE_IN_SECONDS = 60 (segundos)
* HOUR_IN_SECONDS = 60 * MINUTE_IN_SECONDS
* DAY_IN_SECONDS = 24 * HOUR_IN_SECONDS
* WEEK_IN_SECONDS = 7 * DAY_IN_SECONDS
* YEAR_IN_SECONDS = 365 * DAY_IN_SECONDS


### Borrado del Transient

¿Qué sucede si la información se renueva (por ejemplo porque hay una nueva entrada que mostrar) y el transient está aún activo? Pues que esa nueva entrada no se mostrará hasta que 1) Caduque el transient o 2) Borremos el transient para generar uno nuevo que incluya esa nueva entrada. Para borrar el transient usamos la función delete_transient(‘nombre-del-transient’).

WordPress nos ofrece este ejemplo, en el que añadiremos una función a la acción ‘edit_term’, que se ejecutará cada vez que se edite una categoría o una etiqueta (asumimos que dicha actualización invalida nuestros datos por lo que queremos eliminar la información que hay en caché):


```
<?php
// Create a simple function to delete our transient
function edit_term_delete_transient() {
     delete_transient( 'special_query_results' );
}
// Add the function to the edit_term hook so it runs when categories/tags are edited
add_action( 'edit_term', 'edit_term_delete_transient' );
?>
```


Otro ejemplo donde podemos poner la función delete_transient. Supongamos que en functions.php hemos creado un campo personalizado. Por ejemplo, un metabox repetible para incluir imágenes. Si veis el ejemplo de ese artículo, podemos añadir la función delete_metabox() cada vez que se grabe un nuevo valor, es decir, dentro de la función save_custom_meta():


```
// Grabar los datos
    function save_custom_meta($post_id) {

    // todo el código. Al final del mismo ponemos:
    delete_transient('imagenes');
    }
    add_action('save_post', 'save_custom_meta');
```


Y a la hora de obtener las imágenes haríamos ahora:


```
if ( false === ( $attachID = get_transient( 'imagenes' ) ) ) {
    $attachID = (get_post_meta( $post->ID, 'custom_imagenrepetible', true ));
    set_transient( 'imagenes', $attachID, 60*60*3 ); // Lo mantenemos 3 horas. Se borra al subir uno nuevo mediante delete_transient('imagenes')
}
```

# Uso de Templates en Plugins
Al desarrollar themes contamos con la función `get_template_part()` para cargar templates parciales, los cuales pueden ser buscados por WordPress en varias ubicaciones diferentes hasta dar con el archivo indicado. Sin embargo, al trabajar con plugins no contamos con ninguna funcionalidad nativa que nos permita hacer algo así. De hecho, en plugins ni siquiera contamos con una manera nativa de incluir templates para que WordPress los reconozca y cargue de forma automática.

Esto es un problema, ya que muchas veces vamos a contar con ciertas funcionalidades que requieran un template específico. Lo mejor que podemos hacer para estos casos es crear nuevas funciones específicas para nuestros plugins que tomen como punto de partida algunas funciones nativas de WordPress.

Suponiendo que nuestro plugin tiene la opción de seleccionar una página concreta para mostrar una lista de posts creados a partir de un Custom Post Type (similar a lo que hace WooCommerce con Shop), y que al ver esa página se cargue un template llamado `portfolio-page.php`, necesitamos poder indicarle a WordPress bajo qué condiciones usar dicho template y no el template por defecto para páginas incluido en el theme. Para poder hacer eso necesitamos usar el filtro `template_include`.

**Ejemplo:**

```
<?php
if ( ! function_exists( 'portfolio_page_template' ) ) :
add_filter( 'template_include', 'portfolio_page_template', 99 );
/**
 * Obtain template path for portfolio page.
 *
 * @param  string $template
 *
 * @return string
 */
function portfolio_page_template( $template ) {
	global $post;
	$settings = get_option( 'portfolio_settings' );
	if ( $post->ID === $settings['portfolio_page'] ) {
		$new_template = portfolio_get_template( 'portfolio-page.php', false );
		if ( '' !== $new_template ) {
			return $new_template ;
		}
	}
	return $template;
}
endif;
```

En el ejemplo usamos `template_include` para comparar el ID de la página actual con el de la página seleccionada para mostrar la lista de posts. Si el ID coincide, se usará la función `portfolio_get_template()` para obtener la ubicación del template dentro del plugin.

La función `portfolio_get_template()` se encarga de buscar el template en varias ubicaciones, en un cierto orden de prioridades: en primer lugar va a buscarlo en la carpeta `portfolio` dentro del Child Theme. Si no existe, lo busca en la carpeta `portfolio` del theme padre, y por último en la carpeta `templates` de nuestro plugin. Es decir que un implementador podría sobreescribir nuestro template con solo incluir uno propio en una ruta específica de su theme o Child Theme.

```
<?php
if ( ! function_exists( 'portfolio_get_template' ) ) :
/**
 * Search for a template and load it if required.
 *
 * @see   locate_template()
 *
 * @param mixed|string|array $template_names
 * @param bool               $load
 * @param bool               $require_once
 *
 * @return string
 */
function portfolio_get_template( $template_names, $load = false, $require_once = true ) {
	$located = '';
	foreach ( (array) $template_names as $template_name ) {
		if ( ! $template_name ) {
			continue;
		}
		if ( file_exists( STYLESHEETPATH . '/portfolio' . $template_name ) ) {
			$located = STYLESHEETPATH . '/' . $template_name;
			break;
		} elseif ( file_exists( TEMPLATEPATH . '/portfolio' . $template_name ) ) {
			$located = TEMPLATEPATH . '/' . $template_name;
			break;
		} elseif ( file_exists( plugin_dir_path( PORTFOLIO_PLUGIN_MAIN_FILE ) . '/templates/' . $template_name ) ) {
			$located = plugin_dir_path( PORTFOLIO_PLUGIN_MAIN_FILE ) . '/templates/' . $template_name;
			break;
		}
	}
	if ( $load && $located ) {
		load_template( $located, $require_once );
	}
	return $located;
}
endif;
```

Esta función toma como base `locate_template()`, la cual realiza un proceso muy parecido dentro del theme. Nótese que a partir del segundo parámetro que recibe la función (`$load`), puede cargarse el template de manera directa o solo devolver su ubicación. Esto significa que puede cargarse un template parcial directamente haciendo algo como esto:

```
<div class="post-content">
  <?php portfolio_get_template( 'content.php', true ); ?>
</div>
```

# Widgets

Para crear un widget, solo se necesita extender la clase estándar **WP_Widget** y algunos de sus métodos.

La clase base contiene información sobre la función que debe ser extendida para obtener un widget funcional.

**Uso por defecto**

```
class My_Widget extends WP_Widget {
	function My_Widget() {
		// procesos efectivos, reales del widget
	}

	function form($instance) {
		// saca el formulario de opciones en admin
	}

	function update($new_instance, $old_instance) {
		// procesa las opciones del widget que se guardarán
	}

	function widget($args, $instance) {
		// saca el contenido del widget
	}

}
register_widget('My_Widget');
```

**Ejemplo**

Este ejemplo crea un widget llamado **FooWidget** que tiene un formulario de configuraciones para cambiar el título.

```
/**
 * FooWidget Class
 */
class FooWidget extends WP_Widget {
    /** constructor */
    function FooWidget() {
        parent::WP_Widget(false, $name = 'FooWidget');	
    }

    /** @see WP_Widget::widget */
    function widget($args, $instance) {		
        extract( $args );
        $title = apply_filters('widget_title', $instance['title']);
        ?>
              <?php echo $before_widget; ?>
                  <?php if ( $title )
                        echo $before_title . $title . $after_title; ?>
                  Hello, World!
              <?php echo $after_widget; ?>
        <?php
    }

    /** @see WP_Widget::update */
    function update($new_instance, $old_instance) {				
	$instance = $old_instance;
	$instance['title'] = strip_tags($new_instance['title']);
        return $instance;
    }

    /** @see WP_Widget::form */
    function form($instance) {				
        $title = esc_attr($instance['title']);
        ?>
            <p><label for="<?php echo $this->get_field_id('title'); ?>"><?php _e('Title:'); ?> <input class="widefat" id="<?php echo $this->get_field_id('title'); ?>" name="<?php echo $this->get_field_name('title'); ?>" type="text" value="<?php echo $title; ?>" /></label></p>
        <?php 
    }

} // clase FooWidget
```

Este widget puede registrarse en el gancho **widgets_init**:

```
// registrar el widget FooWidget
add_action('widgets_init', create_function('', 'return register_widget("FooWidget");'));
```
