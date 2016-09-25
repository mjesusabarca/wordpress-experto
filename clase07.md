# Clase 7

[comment]: # (Faltantes:)
[comment]: # ([Workshop - consigna])
[comment]: # ([Generación de traducciones])
[comment]: # ([Intro a JavaScript])

## Internacionalización y Localización

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
 ?>
```

Opcionalmente, se puede especificar, en el valor "Domain Path", la ruta relativa a la carpeta donde se encuentren los archivos de traducción que podamos ofrecer por defecto.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 * Domain Path: languages/
 */
 ?>
```

Una vez que la tenemos, debemos usar la función `load_textdomain()` para que se carguen automáticamente todas las posibles traducciones existentes de nuestro plugin.

```php
<?php
/*
 * Plugin Name: My Plugin
 * Text Domain: my-plugin
 * Domain Path: languages/
 ?>
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
?>
```

Un problema con el que es común encontrarse es que parte del texto a traducir dependa de valores variables. Esto es justamente un problema porque `gettext`, la librería que usa WordPress para gestionar sus traducciones, no puede leer información dinámica dentro de las cadenas de texto a traducir. Por lo tanto, algo así no puede ser traducido:

```php
<?php
_e( 'Hello World! The current year is ' . date( 'Y' ), 'my-plugin' );
?>
```

Para resolver este problema, podemos combinar nuestros textos a traducir con las funciones nativas de PHP `printf()` y `sprintf()`. El ejemplo anterior se puede replantear de manera correcta de esta forma:

```php
<?php
printf( __( 'Hello World! The current year is %s', 'my-plugin' ), date( 'Y' ) );
?>
```

Por último, se puede ofrecer una plantilla con textos localizables. La plantilla se trata de nada más que un archivo de texto con el nombre del dominio y con extensión POT (*Portable Object Template*), y un formato como el siguiente:

```
## my-plugin.pot
msgid "Hello world!"
msgstr ""

msgid "some other value"
msgstr ""
```

La propiedad `msgid` corresponde al texto original a traducir, y `msgstr` es la traducción propiamente dicha. En nuestro template este segundo valor va a estar en blanco, porque todavía no es una traducción, sino una herramienta para desarrollar nuevas traducciones.

Muchas veces podemos contar con un número bastante grande de textos a traducir, por lo cual no es muy viable incluir todos manualmente en el archivo POT. Para esto tenemos programas que pueden generar el archivo POT de manera automática, como por ejemplo [Poedit](https://poedit.net/).

Estos son simplemente los conceptos básicos de internacionalización y localización, y es todo lo que hace falta saber al momento de crear plugins que puedan ser traducidos.
