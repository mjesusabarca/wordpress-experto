# Clase 6

[comment]: # (Faltantes:)
[comment]: # ([The Loop])

## Programación Orientada a Objetos

La programación orientada a objetos es un paradigma de programación basado en la idea de que un programa está definido por ciertas interacciones entre, al menos, un usuario (del programa) y una o más entidades. Dichas entidades se llaman _objetos_ y cuentan, por un lado, con _atributos_, y por otro lado, con _métodos_.

Los atributos (también llamados _propiedades_), son diferentes datos que definen el estado actual o la forma de un objeto. Los métodos son acciones que un objeto puede realizar, y que tienen la capacidad de modificar el estado del objeto al que pertenecen. En PHP, los métodos son similares a las funciones.

Los objetos son tipos de datos complejos, y solamente pueden construirse a partir de un tipo de definición llamado _clase_. En PHP, un objeto puede contruirse de la siguiente manera:

```php
<?php
class Futbolista {
	var $nombre;
	var $apellido;
	var $energia;
}

$jugador = new Futbolista;
```

En PHP, las variables internas de una clase son sus propiedades. Al crear un nuevo objeto a partir de una clase, podemos modificar las propiedades internas del objeto:

```php
<?php
class Futbolista {
	var $nombre;
	var $apellido;
	var $energia;
}

$jugador = new Futbolista;

$jugador->nombre = 'Lionel';
$jugador->apellido = 'Messi';
$jugador->energia = 100;
```

Al introducir métodos en una clase, estos tienen la posibilidad de modificar las propiedades internas del objeto. La variable que se usa para referirse al objeto mismo en el contexto de un método es `$this`.

```php
<?php
class Futbolista {
	var $nombre;
	var $apellido;
	var $energia;

	/**
	 * Descuenta un punto de energía por cada 100 metros corridos.
	 */
	function correr( $metros ) {
		if ( $metros ) {
			$this->energia = $this->energia - ( $metros / 100 );
		}
	}
}

$jugador = new Futbolista;

$jugador->nombre = 'Lionel';
$jugador->apellido = 'Messi';
$jugador->energia = 100; // $jugador->energia vale 100.

$jugador->correr( 100 ); // $jugador->energia vale 99.
$jugador->correr( 100 ); // $jugador->energia vale 98.
```

También es posible asignar valores predeterminados a un objeto durante la declaración de la clase, con lo cual puede evitarse declarar el valor de la propiedad después de haber creado el objeto.

```php
<?php
class Futbolista {
	var $nombre;
	var $apellido;
	var $energia = 100; // Valor predeterminado.

	/**
	 * Descuenta un punto de energía por cada 100 metros corridos.
	 */
	function correr( $metros ) {
		if ( $metros ) {
			$this->energia = $this->energia - ( $metros / 100 );
		}
	}
}

$jugador = new Futbolista; // $jugador->energia vale 100.

$jugador->nombre = 'Lionel';
$jugador->apellido = 'Messi';

$jugador->correr( 100 ); // $jugador->energia vale 99.
$jugador->correr( 100 ); // $jugador->energia vale 98.
```

Las clases admiten la creación de un método llamado `__construct()`, el cual se ejecuta automáticamente al crear un nuevo objeto. Este método suele usarse para asignar propiedades al objeto dependiendo de ciertas condiciones.

```php
<?php
class Futbolista {
	var $nombre;
	var $apellido;
	var $energia = 100; // Valor predeterminado.

	function __construct( $nombre, $apellido ) {
		$this->nombre = $nombre;
		$this->apellido = $apellido;
	}

	/**
	 * Descuenta un punto de energía por cada 100 metros corridos.
	 */
	function correr( $metros ) {
		if ( $metros ) {
			$this->energia = $this->energia - ( $metros / 100 );
		}
	}
}

$jugador = new Futbolista( 'Lionel', 'Messi' );
// $jugador->nombre es "Lionel".
// $jugador->apellido es "Messi".
// $jugador->energia es 100.

$jugador->correr( 100 ); // $jugador->energia vale 99.
$jugador->correr( 100 ); // $jugador->energia vale 98.
```

## Widgets

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

Este ejemplo crea un widget llamado `Foo_Widget` que tiene un formulario para cambiar el título. Lo que vemos en el método `form` es el HTML del formulario de edición del widget en el panel de administración, mientras que `update` es el proceso de actualización de datos que se ejecuta al guardar el widget, y el método `widget` procesa el HTML que se va a mostrar en la parte pública del sitio.

```php
<?php
/**
 * Foo_Widget Class
 */
class Foo_Widget extends WP_Widget {
	/** constructor */
	public function __construct() {
		$args = array( 
			'classname' => 'my_widget',
			'description' => 'My Widget is awesome',
		);
		parent::__construct( 'foo-widget', 'Foo Widget', $args );
	}

	/** @see WP_Widget::widget */
	function widget( $args, $instance ) {
		$title = apply_filters( 'widget_title', $instance['title'] );

		echo $args['before_widget'];

		if ( $title ) {
			echo '<h3>' . $args['before_title'] . $title . $args['after_title'] . '</h3>';
		}

		echo '<p>Lorem ipsum dolor sit amet.</p>';

		echo $args['after_widget'];
	}

	/** @see WP_Widget::update */
	function update( $new_instance, $old_instance ) {
		$instance = $old_instance;
		$instance['title'] = strip_tags($new_instance['title']);
		return $instance;
	}

	/** @see WP_Widget::form */
	function form( $instance ) {
		$title = esc_attr( $instance['title'] );
		?>
		    <p><label for="<?php echo $this->get_field_id('title'); ?>"><?php _e('Title:'); ?> <input class="widefat" id="<?php echo $this->get_field_id('title'); ?>" name="<?php echo $this->get_field_name('title'); ?>" type="text" value="<?php echo $title; ?>" /></label></p>
		<?php
	}
}
```

Este widget puede registrarse en el hook `widgets_init`:

```php
<?php
add_action( 'widgets_init', 'my_plugin_register_widgets' );

function my_plugin_register_widgets() {
	register_widget( 'Foo_Widget' );
}
```
