# Clase 5

[comment]: # (Paradigma funcional II)

## Custom Fields y Meta Boxes

La manera más rápida de agregar valores extra a los contenidos de un sitio es por medio del uso de campos personalizados, o *Custom Fields*. Estos campos personalizados se hacen visibles en la pantalla de creación o edición de un contenido cuando se chequea el campo correspondiente en *Opciones de Pantalla*. Esto va a hacer que WordPress muestre un menú en el que se puede agregar un nuevo campo con un nombre a elección y su correspondiente valor.

Puede obtenerse e imprimirse en pantalla ese valor desde un plugin de esta manera:

```php
<?php
add_filter( 'the_title', 'apply_title_color' );

function apply_title_color( $title ) {
	$color = get_post_meta( get_the_ID(), 'title_color', true );

	if ( $color ) {
		$title = '<span style="color: ' . $color . '">' . $title . '</span>';
	}

	return $title;
}
```

O mostrar todos los valores de campos personalizados previamente cargados de esta forma:

```php
<?php
add_filter( 'the_content', 'show_post_meta' );
function show_post_meta( $content ) {
	ob_start();

	the_meta();
	$content .= ob_get_contents();

	ob_end_clean();

	return $content;
}
```

La ventaja de esta forma de gestionar campos personalizados es que es rápida. Hace falta muy poco código para mostrar nuevos datos. Es muy útil para salir de un apuro o con fines de testing, pero para desarrollo de plugins dista bastante de ser óptima. En primer lugar, porque los campos tienen que ser creados manualmente, y se requiere que el usuario conozca el nombre de estos campos para poder ingresar información. Esto no es una muy buena práctica, porque parte de la lógica de la aplicación se manejaría como contenido, y no como código.

Por otra parte, este método también es limitante en cuanto a cómo se carga la información: no se puede modificar el formato de los campos personalizados. Su formato por defecto puede ser muy útil para ingresar texto, pero si se necesita manejar otro tipo de información, como un array, o elegir entre una cantidad limitada de opciones, los campos personalizados no alcanzan.

La solución es crear *meta boxes*. Las meta boxes son, básicamente, campos personalizados con esteroides. A la hora de guardar y obtener información funcionan casi de la misma manera que los campos personalizados, pero brindan control completo sobre el HTML que generan, por lo cual se puede crear cualquier tipo de campo para que los usuarios ingresen datos, en lugar de limitarnos a un input de texto. Además, manejan el nombre del campo internamente, por lo cual no es necesario que el usuario lo conozca a la hora de ingresar datos. También ofrecen control absoluto acerca de cómo se guarda esa información, por lo cual, si se necesita hacer algún tipo de validación de datos o correr algún tipo de proceso sobre la información ingresada antes de guardarla, puede hacerse sin problema.

Agregar meta boxes es muy sencillo, y para hacerlo es necesario asignar una función al evento `add_meta_boxes`, el cual corresponde al momento en el que WordPress registra este tipo de información.

```php
<?php
add_action( 'add_meta_boxes', 'register_meta_box' );

function register_meta_box() {
	add_meta_box(
		$id,       // Atributo "id" impreso en el contenedor HTML. String.
		$title,    // Título de meta box. String.
		$callback, // Función que imprime nuestro HTML. String o función anónima.
		$screen,   // Opcional. Vista o vistas donde va a aparecer la meta box. Nombres de post types u objeto WP_Screen.
	);
}
```

Teniendo en cuenta estos parámetros, la función se vería similar a esta:

```php
<?php
add_action( 'add_meta_boxes', 'register_meta_box' );

function register_meta_box() {
	add_meta_box(
		'my-meta-box'
		__( 'My meta box', 'my-plugin' ),
		'my_metabox_callback',
		'post', // También son válidos 'page', array( 'post', 'page' ) y current_screen().
	);
}

function my_metabox_callback() {
	echo 'Hello world!'
}
```

Aunque con este código todavía no es posible cargar nueva información, ya puede verse una nueva meta box en las pantallas de edición y creación de posts. La función `add_meta_box()` también acepta otros tres parámetros opcionales más: `$context`, para definir en qué sección de la pantalla va a aparecer la meta box; `$priority`, para indicar qué prioridad tiene dentro del contexto en el que se la registra; y `$callback_args`, para enviarle información adicional a nuestro callback dentro de un array.

El siguiente paso es imprimir, a través del callback, algún tipo de información que un usuario pueda cargar. Para eso simplemente puede crearse un campo de formulario dentro de la función e imprimirlo.

Con este código puede verse un campo de tipo select dentro de la meta box:

```php
<?php
add_action( 'add_meta_boxes', 'register_meta_box' );

function register_meta_box() {
	add_meta_box(
		'my-meta-box'
		__( 'My meta box', 'my-plugin' ),
		'my_metabox_callback',
		'post', // También son válidos 'page', array( 'post', 'page' ) y current_screen().
	);
}

function my_metabox_callback() { ?>
	<select name="_my_selectable_option">
		<option value="value-1"><?php _e( 'Value 1', 'my-plugin' ); ?></option>
		<option value="value-2"><?php _e( 'Value 2', 'my-plugin' ); ?></option>
	</select>
<?php
}
```

Con lo que logrado hasta ahora, un usuario puede seleccionar información, pero aún no puede guardarla. Para eso vamos a necesitar una nueva acción en el evento `save_post`, donde se chequee y se guarde la información que estamos enviando.

```php
<?php
add_action( 'add_meta_boxes', 'register_meta_box' );
function register_meta_box() {
	add_meta_box(
		'my-meta-box'
		__( 'My meta box', 'my-plugin' ),
		'my_metabox_callback',
		'post', // También son válidos 'page', array( 'post', 'page' ) y current_screen().
	);
}

function my_metabox_callback() { ?>
	<select name="_my_selectable_option">
		<option value="value-1"><?php _e( 'Value 1', 'my-plugin' ); ?></option>
		<option value="value-2"><?php _e( 'Value 2', 'my-plugin' ); ?></option>
	</select>
<?php
}

add_action( 'save_post', 'save_my_meta_box_data' );
function save_my_meta_box_data( $post_id ) {
	$data = get_post_meta( $post_id, '_my_selectable_option', true );

	if ( empty( $data ) ) {
		add_post_meta( $post_id, '_my_selectable_option', $_POST['_my_selectable_option'], true );
	} elseif ( $data != $_POST['_my_selectable_option'] ) {
		update_post_meta( $post_id, '_my_selectable_option', $_POST['_my_selectable_option'] );
	}
}
```

Este código sirve para guardar la información ingresada en la meta box al guardar un post. Lo que hace es chequear si ya existen datos para el post actual con el nombre del campo creado. Si no existen, los ingresa en la base de datos; si existen, y además son distintos de los que estamos enviando al guardar, los actualiza. Sin embargo, no se está haciendo ningún chequeo de seguridad, por lo cual un usuario malicioso podría ingresar cualquier tipo de información en ese campo, provocando problemas bastante graves. Es por eso que necesitamos ser más específicos en nuestros chequeos.

```php
<?php
/**
 * Necesitamos chequear:
 *
 * - Que la información que hace falta actualizar se esté enviando.
 * - Que la actualización se esté haciendo en el momento y lugar correctos.
 * - Que el usuario tenga permisos para modificar la información.
 * - Que la información a actualizar sea válida.
 */

add_action( 'add_meta_boxes', 'register_meta_box' );
function register_meta_box() {
	add_meta_box(
		'my-meta-box'
		__( 'My meta box', 'my-plugin' ),
		'my_metabox_callback',
		'post', // También son válidos 'page', array( 'post', 'page' ) y current_screen().
	);
}

function my_metabox_callback() {
	wp_nonce_field( '_my_selectable_option', '_my_selectable_option_nonce' );
?>
	<select name="_my_selectable_option">
		<option value="value-1"><?php _e( 'Value 1', 'my-plugin' ); ?></option>
		<option value="value-2"><?php _e( 'Value 2', 'my-plugin' ); ?></option>
	</select>
<?php
}

add_action( 'save_post', 'save_my_meta_box_data', 10, 2 );
function save_my_meta_box_data( $post_id, $post ) {
	// Si no se reciben datos, salir de la función.
	if ( ! isset( $_POST['_my_selectable_option'] ) ) {
		return;
	}

	// Si no se aprueba el chequeo de seguridad, salir de la función.
	if ( ! isset( $_POST['_my_selectable_option_nonce'] ) || ! wp_verify_nonce( $_POST['_my_selectable_option_nonce'], '_my_selectable_option' ) ) {
		return;
	}

	$post_type = get_post_type_object( $post->post_type );

	// Si el usuario actual no tiene permisos para modificar el post, salir de la función.
	if ( ! current_user_can( $post_type->cap->edit_post, $post_id ) ) {
		return;
	}

	// Convertimos los datos ingresados a un formato válido para nuestro campo.
	$valid_data = esc_html( $_POST['_my_selectable_option'] );

	$data = get_post_meta( $post_id, '_my_selectable_option', true );

	if ( empty( $data ) ) {
		add_post_meta( $post_id, '_my_selectable_option', $valid_data, true );
	} elseif ( $data != $_POST['_my_selectable_option'] ) {
		update_post_meta( $post_id, '_my_selectable_option', $valid_data );
	}
}
```

En el primer chequeo, en caso de que no se reciba la información esperada, simplemente se sale de la función sin guardar. El segundo chequeo es algo más complejo, pero sirve para introducir un concepto muy importante en cuestión de seguridad: los *nonces*.

Un *nonce* es, básicamente, un tipo de dato generado por WordPress que consiste en un valor numérico variable, y que permite chequear que un proceso dado se esté ejecutando en el momento y desde el lugar adecuados. Está pensado para cuando se necesita ejecutar operaciones delicadas, que requieren un cierto nivel de seguridad, como el almacenamiento de información en la base de datos.

Para poder chequear que un nonce sea válido antes de guardar datos, primero se lo debe crear.  Es por eso que dentro del callback que genera el HTML de nuestra meta box va a usarse la función `wp_nonce_field()` con dos parámetros: el primero es una palabra clave que permite identificar el contexto en el que se genera el nonce, y el segundo es el nombre del dato que va a contener el valor numérico del nonce y va a llegar por medio de la variable global `$_POST` cuando se guarde la entrada.

Al chequear un nonce debe confirmarse que el dato que lo contiene está creado, lo cual hacemos con `isset()`. También se necesita chequear con `wp_verify_nonce()` que el dato numérico corresponda al contexto definido. Si alguna de estas dos cosas no pasa, se sale de la función sin guardar información. Si se pasan ambos chequeos, se continúa.

Por último, debe confirmarse que los datos que se van a ingresar tengan el formato correcto. Por ejemplo, si lo que se desea guardar en la base de datos es un array, pero recibimos un string, se debe lo debe convertir en un array; si se espera texto plano pero se recibe HTML, se debe convertir a texto plano. Este proceso se llama *sanitización*, y permite asegurarse de que un usuario malicioso no va a poder ingresar algún tipo de información que dañe la base de datos. Además, garantiza que los datos introducidos en el sistema siempre van a ser los esperables.

En el ejemplo se usa la función `esc_html()` para asegurarse de que cualquier texto que pueda ser ingresado como HTML se convierta en texto plano. Hay una cantidad bastante grande de [funciones de sanitización](https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data) provistas por WordPress, las cuales se pueden chequear en la documentación oficial. Algunas de esas funciones son más recomendables para mostrar datos (lo cual se llama *escape*), y otras son preferibles a la hora de guardarlos (lo cual se llama *validación*).

## Menús de administración

Es bastante corriente que nuestros plugins necesiten algún tipo de configuración global, más allá de la información de podemos manipular por medio de post types y meta boxes. Para estos casos, generalmente necesitamos crear alguna pantalla de administración propia de nuestro plugin, donde el usuario pueda configurar las opciones que definan su funcionamiento.

Con este fin es que WordPress nos ofrece tres tipos de herramientas: **menús de administración**, ***Options API*** y ***Settings API***. Cada una consiste en un conjunto de funciones que nos permiten definir cómo se van a manejar las configuraciones internas de nuestro plugin. En esta clase vamos a ver la primera, los menús de administración.

Los menús de administración son esas secciones que vemos en la barra lateral de navegación en nuestro panel de administración. Cada uno de ellos tiene un link principal, y además pueden tener un segundo nivel de links, que vemos cuando estamos navegando un menú o cuando pasamos el mouse por encima del link principal. Estos links secundarios son lo que llamamos sub-menús.

Por último tenemos las páginas de opciones, que son aquellos links que vemos ubicados debajo del menú *Settings* (o *Configuración*).

Para agregar un menú usamos la función `add_menu_page()`, y la asignamos como acción al evento `admin_menu`. Con este ejemplo de código podemos crear un menú y asociarle una interfaz muy básica.

```php
<?php
add_action( 'admin_menu', 'portfolio_menu_page' );

function portfolio_add_menu_page() {
    add_menu_page(
        __( 'Portfolio Settings', 'portfolio' ), // Texto del título que aparece en la página de opciones.
        __( 'Portfolio', 'portfolio' ),          // Texto que aparece en el link principal del menú.
        'manage_options',                        // Permiso que debe tener el usuario para ver el menú.
        'portfolio-settings',                    // Slug, string que permite identificar internamente el menú.
        'portfolio_menu_page',                   // Nombre de la función que imprime el HTML de la página de opciones.
        'dashicons-art',                         // Icono del menú. Podemos usar Dashicons o una URL.
        10,                                      // Posición en la que aparece el menú en la barra de navegación.
    );
}

function portfolio_menu_page(){
    _e( 'Set Portfolio options here.', 'portfolio' );  
}
```

Cuando guardemos nuestro código y refresquemos el browser, vamos a ver que contamos con un nuevo menú de administración. Haciendo click en el link del menú, vamos a ver el texto que imprimimos dentro de nuestra función callback.

También podemos, en lugar de crear un nuevo menú para nuestra página de opciones, ubicar un link debajo de Settings. Esto es lo que hacen muchos desarrolladores de plugins para que la barra de navegación no se vea tan larga.

El procedimiento es muy similar a agregar un menú, pero usando la función `add_options_page()`. Simplemente reemplazamos el código anterior por este:

```php
<?php
add_action( 'admin_menu', 'portfolio_menu_page', 999 );

function portfolio_add_menu_page() {
    add_options_page(
        __( 'Portfolio Settings', 'portfolio' ), // Texto del título que aparece en la página de opciones.
        __( 'Portfolio', 'portfolio' ),          // Texto que aparece en el link principal del menú.
        'manage_options',                        // Permiso que debe tener el usuario para ver el menú.
        'portfolio-settings',                    // Slug, string que permite identificar internamente el menú.
        'portfolio_menu_page',                   // Nombre de la función que imprime el HTML de la página de opciones.
    );
}

function portfolio_menu_page(){
    _e( 'Set Portfolio options here.', 'portfolio' );  
}
```

Cuando refresquemos el browser después de guardar, la ubicación de nuestro link va a haber cambiado, pero vamos a seguir viendo la misma página. Lo único que no podemos desde definir desde `add_options_page()` es un ícono, ya que los sub-menús y links páginas de opciones no tienen ninguno, ni la posición en la que va a aparecer el link. Para la posición podemos jugar un poco con las prioridades de `add_action()` hasta que el link se vea donde pretendemos. En el ejemplo hay una prioridad alta (999) para que aparezca lo más cerca posible del final de la lista.

Una tercera alternativa, más ordenada, es ubicar el link a nuestra página de opciones dentro del menú que se creó automáticamente para nuestro post type. Como ya vimos al agregar un menú principal, todo menú cuenta con un slug o palabra clave para que pueda ser reconocido internamente, y para agregar un sub-menú necesitamos conocer el slug del menú principal.

En el caso de los post types, el nombre del slug es un poco más rebuscado que el de los que venimos definiendo, pero es fácilmente reconocible: `edit.php?post_type={nombre_del_post_type}`. En nuestro caso, como nuestro post type es `project`, el slug del menú va a ser `edit.php?post_type=project`. Teniendo esto en cuenta, la forma de registrar un sub-menú es prácticamente igual al ejemplo anterior, pero usando la función `add_submenu_page()`.

```php
<?php
add_action( 'admin_menu', 'portfolio_menu_page', 999 );

function portfolio_add_menu_page() {
    add_submenu_page(
        'edit.php?post_type=project',            // Slug del menú principal.
        __( 'Portfolio Settings', 'portfolio' ), // Texto del título que aparece en la página de opciones.
        __( 'Portfolio Settings', 'portfolio' ), // Texto que aparece en el link principal del menú.
        'manage_options',                        // Permiso que debe tener el usuario para ver el menú.
        'portfolio-settings',                    // Slug, string que permite identificar internamente el menú.
        'portfolio_menu_page',                   // Nombre de la función que imprime el HTML de la página de opciones.
    );
}

function portfolio_menu_page(){
    _e( 'Set Portfolio options here.', 'portfolio' );  
}
```

Al refrescar el browser van a ver que el link cambió nuevamente de ubicación, y ahora está en el último lugar del menú de *Projects*.

Algo importante a tener en cuenta es que, si estamos mirando la página de administración que acabamos de crear, al cambiar el link de lugar y refrescar la página, WordPress puede mostrar un error. Esto no es porque hayamos hecho algo mal, sino porque al cambiar la ubicación del link también puede cambiar la URL que se le asigna, pero con volver a la página principal del panel de administración vamos a poder ver el link en su nueva ubicación, y acceder a él desde su nueva URL sin problemas.

## Options API

Ya vimos cómo crear una página de administración para nuestro plugin, pero todavía no podemos guardar opciones a través de esa página. Para poder gestionar nuestras opciones, WordPress nos ofrece la Options API, un conjunto de funciones muy sencillas para manipular configuraciones internamente en nuestros plugins.

Para poder ingresar información desde nuestra página de opciones, primero tenemos que ofrecer algún campo con el que el usuario quiera interactuar. Por el momento vamos a agregar una única opción: la de mostrar proyectos solo a usuarios registrados. Para eso, vamos a retomar la función que usamos para mostrar la página de administración:

```php
<?php
function portfolio_menu_page() {
	$settings = get_option( 'portfolio_settings' );
	$checked  = isset( $settings['show_logged_only'] ) ? $settings['show_logged_only'] : false;

	wp_nonce_field( '_portfolio_settings', '_portfolio_settings_nonce' );

	?>
		<form method="post">
			<input id="portfolio-settings-show-logged-only" name="portfolio_settings[show_logged_only]" type="checkbox" value="true" <?php checked( $checked ); ?>/>
			<label for="portfolio-settings-show-logged-only"><?php _e( 'Only show projects to logged-in users', 'portfolio' ); ?></label>

			<input type="submit" class="button button-save" value="<?php _e( 'Save options', 'portfolio' ); ?>" />
		</form>
	<?php
}
```

Al refrescar la página vamos a ver aparecer la opción. Noten como, en las dos primeras líneas de esta función, chequeamos si nuestro plugin guardó datos previamente, y a partir de eso decidimos si el checkbox tiene que mostrarse tildado o no. También creamos el nonce que nos va a permitir validar los datos cuando se envíen.

Ahora tenemos que ver cómo se guardan esos datos. El proceso es muy parecido a lo que hacíamos con las meta boxes.

```php
<?php
add_action( 'admin_init', 'portfolio_save_settings' );

function portfolio_save_settings() {
	// Si no se aprueba el chequeo de seguridad, salir de la función.
	if ( ! isset( $_POST['_portfolio_settings_nonce'] ) || ! wp_verify_nonce( $_POST['_portfolio_settings_nonce'], '_portfolio_settings' ) ) {
		return;
	}

	// Si el usuario actual no tiene permisos para actualizar la configuración, salir de la función.
	if ( ! current_user_can( 'manage_options' ) ) {
		return;
	}

	// Chequeamos si el dato se envió para decidir si mostramos los proyectos a todos usuarios o no.
	if ( isset( $_POST['portfolio_settings']['show_logged_only'] ) ) {
		// Si el valor se envío, chequeamos si tiene un valor permitido. Si lo tiene, mostramos proyectos solo a usuarios registrados.
		$accepted_values = array( true, 'true' );
		$show_logged_only = in_array( $_POST['portfolio_settings']['show_logged_only'], $accepted_values );
	} else {
		// Si el valor no se envió, mostramos proyectos a todos los usuarios.
		$show_logged_only = false;
	}

	// Creamos un flag para evaluar si ya tenemos la configuración guardada en al base de datos.
	$settings_empty = false;

	// Obtenemos la configuración del plugin, si existe.
	$settings = get_option( 'portfolio_settings' );

	// Si no hay una configuración previa guardada en la base de datos, lo indicamos, y creamos un array vacío para contenerla.
	if ( empty( $settings ) ) {
		$settings_empty = true;
		$settings = array();
	}

	// Si el valor de la configuración que estamos modificando no existe o es diferente al que evaluamos, lo cambiamos por el nuevo valor.
	if ( ! isset( $settings['show_logged_only'] ) || $settings['show_logged_only'] != $show_logged_only ) {
		$settings['show_logged_only'] = $show_logged_only;
	}

	if ( $settings_empty ) {
		// Si la configuración está vacía, la creamos.
		add_option( 'portfolio_settings', $settings );
	} else {
		// Si la configuración no está vacía, la actualizamos.
		update_option( 'portfolio_settings', $settings );
	}
}
```

Con estos chequeos y el uso de `add_option()` y `update_option()` podemos crear y modificar la configuración de nuestro plugin. Es importante tener en cuenta que cada opción manejada por estas funciones genera una nueva entrada en la base de datos, y si para cada configuración que necesitemos en nuestro plugin creamos una entrada nueva, estamos complejizando la base de datos, lo cual, a la larga, puede llevar a problemas de rendimiento para sitios web con bases de datos grandes. Por eso lo que hacemos acá, y lo que cada vez se recomienda más, es usar una sola opción para guardar nuestra configuración, e incluir todos los datos que necesitamos dentro de un array.

Ahora bien, necesitamos ver cómo reflejar en el front-end estos cambios que introdujimos. Para eso vamos a volver a asignar una función a `the_content`, como en los ejemplos anteriores, pero en vez agregarle cosas al contenido, lo vamos a modificar por completo en caso de que el usuario no esté logueado, y que solo queramos mostrar proyectos a usuarios registrados. Para eso vamos a usar una prioridad con un número alto, y así asegurarnos de que la función se ejecute lo más tarde posible.

```php
<?php
add_filter( 'the_content', 'portfolio_project_show_logged_in_only', 999 );

function portfolio_project_show_logged_in_only( $content ) {
	$settings = get_option( 'portfolio_settings' );

	// Si se elige mostrar solo a usuarios registrados y el usuario actual no está logueado, reemplazamos el contenido por un mensaje.
	if ( ! empty( $settings['show_logged_in_only'] ) && ! is_user_logged_in() ) {
		$content = __( 'You don\'t have permissions to view this content.', 'portfolio' );
	}

	return $content;
}
```

De todas maneras, si bien la Options API es la herramienta que WordPress usa internamente y recomienda para manejar configuraciones propias, a la hora de manejar el guardado de datos y generación de campos en menús de administración, no es la mejor solución que tenemos a nuestro alcance, ya que tiene una gran cantidad de limitaciones. Es importante entender cómo funciona y cuáles son sus funciones principales, porque en desarrollo de plugins es casi inevitable usarla, pero para lo que acabamos de hacer, la mejor solución que nos ofrece WordPress es la ***Settings API***, que es el tema del próximo capítulo.

## Settings API

Ya vimos cómo se maneja la *Options API* para guardar la configuración de nuestro plugin, y recomendamos usar una única opción para guardar todos nuestros datos, construyendo un array. También vimos que para guardar nuestra configuración tenemos que hacer una serie de chequeos de seguridad que pueden llegar a ser tediosos, o pueden volver nuestras funciones muy largas y complicadas de mantener.

Todos estos problemas se pueden resolver usando la *Settings API*. Lo que nos permite esta API es sacarnos de encima gran parte de los procesos de seguridad y validación que necesitamos para guardar nuestros datos, y en consecuencia vuelve nuestro código más estable, más sencillo de mantener, y más extensible.

Una diferencia que vamos a tener con respecto a lo visto anteriormente es que no vamos a imprimir de manera directa el HTML que genera nuestros campos. En cambio, nuestros campos tienen que ser registrados en la *Settings API*, y asignados a una sección.

```php
<?php
add_action( 'admin_init', 'portfolio_settings_api_init' );

function portfolio_settings_api_init() {
 	// Registramos una sección para nuestro campo.
 	add_settings_section(
		'portfolio-general-settings-section',          // Texto del tag `id` de la sección.
		__( 'General Settings', 'portfolio' ),         // Título de la sección.
		'portfolio_general_settings_section_callback', // Nombre de la función que imprime el HTML de la sección.
		'portfolio-settings'                           // Slug del menú donde debe aparecer la sección.
	);

 	// Registramos un campo asociado a la sección.
 	add_settings_field(
		'portfolio-settings-show-logged-only-setting',              // Texto del tag `id` del campo.
		__( 'Only show projects to logged-in users', 'portfolio' ), // Título del campo.
		'portfolio_settings_show_logged_only_callback',             // Nombre de la función que imprime el HTML del campo.
		'portfolio-settings',                                       // Slug del menú donde debe aparecer el campo.
		'portfolio-general-settings-section'                        // ID de la sección a la que pertenece el campo.
	);

 	// Registramos nuestro campo como setting.
	// El primer parámetro es el nombre de la opción que estamos usando para guardar la configuración.
	// El segundo parámetro es el identificador de nuestra setting dentro del array de opciones.
	// El tercer parámetro es el nombre de la función que va a sanitizar los datos de la opción.
	// Esta función se ocupa de validar nuestros datos por medio de nonces.
 	register_setting( 'portfolio_settings', 'show_logged_only', 'portfolio_sanitize_show_logged_only' );
}

// Callback para la sección.
function portfolio_general_settings_section_callback() {
	_e( 'Configure the general plugin settings here' );
}

// Callback para el campo. Solamente imprimimos el input y su label, no el tag form ni el botón de confirmación.
function portfolio_settings_show_logged_only_callback() {
	$settings  = get_option( 'portfolio_settings' );
	$checked = isset( $settings['show_logged_only'] ) ? $settings['show_logged_only'] : false;

	?>
		<input id="portfolio-settings-show-logged-only" name="portfolio_settings[show_logged_only]" type="checkbox" value="true" <?php checked( $checked ); ?>/>
		<label for="portfolio-settings-show-logged-only"><?php _e( 'Only show projects to logged-in users', 'portfolio' ); ?></label>
	<?php
}

// Función de sanitización para nuestro campo.
function portfolio_sanitize_show_logged_only( $data ) {
	// Chequeamos si el valor ingresado está permitido. Si lo está, mostramos proyectos solo a usuarios registrados.
	$accepted_values = array( true, 'true' );
	$show_logged_only = in_array( $data, $accepted_values );

	return $show_logged_only;
}
```

Con este código ya podemos guardar nuestros datos automáticamente, sin necesidad de interactuar de manera directa con la *Options API* ni de chequear nonces. Sin embargo, todavía no podemos usarlo, porque lo que hicimos aún no se ve reflejado en la página de administración. Para eso necesitamos modificar nuestra función `portfolio_menu_page()` de la siguiente manera:

```php
<?php
function portfolio_menu_page() {
	?>
		<form action="options.php" method="POST">
			<?php settings_fields( 'portfolio_settings' ); // Imprime el HTML necesario para las validaciones. ?>
			<?php do_settings_sections( 'portfolio-general-settings-section' ); // Imprime nuestra sección y los campos asociados. ?>
			<?php submit_button(); // Imprime el botón de confirmación. ?>
		</form>
	<?php
}
```

Una vez que actualizamos a este código, pasan varias cosas: nuestra antigua función de guardado, `portfolio_save_settings()` ya no se usa más, ya que todas las validaciones son hechas por la Settings API, y podemos eliminarla tranquilos. También contamos con más funciones que antes, pero más chicas y más fáciles de mantener. Además, la sanitización de datos está desacoplada del proceso de guardado. Como toda la registración de secciones y de campos es manejada internamente por WordPress, con mirar un poco la documentación oficial de la API y sus otras funciones podemos ver que es muy fácil mostrar o no mostrar nuevas opciones condicionalmente, según el contexto en el que estemos, qué usuario está viendo la página, en qué menú estamos, etc. Es una mejora sustancial sobre el método anterior, con el que las cosas no se podían hacer de manera tan automática.
