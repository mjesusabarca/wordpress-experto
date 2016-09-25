# Clase 3

[comment]: # (Material completo)

## Carga de archivos

PHP nos permite separar nuestro código en diferentes archivos para poder mantenerlo de forma más ordenada. Para esto contamos con las construcciones `require`, `require_once`, `include` e `include_once`.

Por ejemplo, si estamos trabajando en un archivo llamado `contacto.php`, y queremos reutilizar el código contenido en `encabezado.php`, debemos usar una de las cuatro construcciones para incluir el archivo:

```php
# contacto.php
<?php
require 'encabezado.php';
require_once 'encabezado.php';
include 'encabezado.php';
include_once 'encabezado.php';
?>
```

Una diferencia entre estas construcciones es que aquellas con el sufijo `_once` solamente incluyen el archivo si no fue incluido previamente en alguna parte de la aplicación, lo cual evita la replicación de código. Sin embargo, a veces es deseable que el código se replique, y en estos casos se debe preferir usar `include` o `require`.

Otra diferencia es que `require` y `require_once` hacen una inclusión estricta, es decir que PHP va a arrojar un error fatal y cortar la ejecución de la aplicación en caso de que el archivo no se encuentre. En el caso de `include` e `include_once` no hay error fatal ni se detiene la ejecución, pero sí se imprime una advertencia.

## Paradigma estructurado

El paradigma de programación estructurada trabaja sobre la idea de que todo programa o aplicación puede escribirse usando tres tipos de instrucciones:

* Secuencias (bloques de declaraciones y ejecuciones)
* Condiciones (`if`, `switch`)
* Bucles (`while`, `for`, `foreach`)

En PHP, un ejemplo simple de programación estructurada sería el siguiente:

```php
<?php
// Secuencia (declaración)
$lista = array( 'Pan', 'Leche', 'Azúcar' );

// Condición
if ( is_array( $lista ) && ! empty( $lista ) ) {
  // Bucle
  foreach ( $lista as $item ) {
    // Secuencia (ejecución)
    echo $item; // Imprime cada elemento de la lista.
  }
}
?>
```

Este paradigma es el que se usa principalmente en PHP y, aunque puede ser combinado con otros, tales como el funcional, el orientado a objetos y el orientado a eventos, estos se usan en combinación con el estructurado.

Una característica del paradigma estructurado es que su tiempo de ejecución es secuencial, inmediato y continuo. Esto significa que cada bloque de código se va a ejecutar en el mismo orden en el que fue ubicado por el programador. Este puede no ocurrir al trabajar con otros paradigmas.

## Paradigma funcional

El paradigma de programación funcional propone que todo programa o aplicación debe consistir únicamente en definiciones de funciones que se utilizan mutuamente para obtener valores o ejecutar procesos. En sus implementaciones más puras no existen las variables, y las funciones consisten en procedimientos similares a operaciones matemáticas. Prácticamente no es utilizado en su forma más estricta en desarrollo web, pero muchos lenguajes de programación que no son estrictamente funcionales, como JavaScript, Python y PHP, incorporan ciertos conceptos de la programación funcional para facilitar el trabajo del programador.

Las funciones son bloques de código reutilizable, que se definen una única vez y pueden ejecutarse múltiples veces, produciendo siempre un resultado similar. Pueden recibir una serie de parámetros, cuyo valor puede modificar el resultado de una función.

En PHP, las funciones se declaran usando la palabra `function` seguida del nombre que vamos a asignar a nuestra función. El nombre puede escribirse de la misma forma que las variables, pero sin el caracter `$`. A continuación del nombre se usan paréntesis, entre los cuales se colocan los nombres de los parámetros que puede recibir la función.

```php
<?php
function imprimir_lista( $lista ) {
  // Condición
  if ( is_array( $lista ) && ! empty( $lista ) ) {
    // Bucle
    foreach ( $lista as $item ) {
      // Secuencia (ejecución)
      echo $item . "\n" ; // Imprime cada elemento de la lista.
    }
  }
}
?>
```

Al programar utilizando funciones podemos seguir utilizando secuencias, condiciones y bucles. Podemos entender, entonces, que la programación funcional en PHP presupone el uso de la programación estructurada.

Para ejecutar el código contenido en una función, simplemente escribimos su nombre y, entre paréntesis, los valores de los parámetros que queremos enviarle.

```php
<?php
function imprimir_lista( $lista ) {
  if ( is_array( $lista ) && ! empty( $lista ) ) {
    foreach ( $lista as $item ) {
      echo $item . "\n" ; // Imprime cada elemento de la lista.
    }
  }
}

// Se define una variable:
$lista = array( 'Pan', 'Leche', 'Azúcar' );

// Se ejecuta la función:
imprimir_lista( $lista );

// Se imprime:
// Pan
// Leche
// Azúcar
?>
```

Una función también puede recibir más de un parámetro:

```php
<?php
function imprimir_lista( $lista, $titulo ) {
  if ( is_array( $lista ) && ! empty( $lista ) ) {
    echo $titulo . "\n";

    foreach ( $lista as $item ) {
      echo $item . "\n" ; // Imprime cada elemento de la lista.
    }
  }
}

// Se ejecuta la función:
imprimir_lista( array( 'Pan', 'Leche', 'Azúcar' ), 'Lista de compras' );

// Se imprime:
// Lista de compras
// Pan
// Leche
// Azúcar
?>
```

Es importante notar que el llamado a la función debe hacerse una vez que la función ya haya sido declarada. De lo contrario, PHP imprime un error fatal con el mensaje "Call to undefined function".

Una función también puede ser usada para obtener datos. El siguiente ejemplo devuelve una cadena de texto a partir del contenido de un array:

```php
<?php
function lista_en_texto( $lista ) {
  $texto = '';

  if ( is_array( $lista ) && ! empty( $lista ) ) {
    foreach ( $lista as $item ) {
      $texto .= $item . "\n" ;
    }
  }

  return $texto;
}

// Se ejecuta la función:
$lista = lista_en_texto( array( 'Pan', 'Leche', 'Azúcar' ) );

// Valor de $lista:
// Pan
// Leche
// Azúcar
?>
```

La construcción `return` arroja un resultado que puede ser asignado a una variable o constante. Es importante notar que, una vez ejecutada, cierra la ejecución de la función que la contiene.

```php
<?php
function lista_en_texto( $lista ) {
  $texto = '';

  if ( is_array( $lista ) && ! empty( $lista ) ) {
    foreach ( $lista as $item ) {
      $texto .= $item . "\n" ;
    }
  }

  return $texto;

  echo $texto; // No se ejecuta, porque se usó "return" más arriba.
}
?>
```

### Redeclaración de funciones

Al contrario de las variables, las funciones no pueden ser redefinidas. Es decir que, si intentáramos escribir una nueva función con el mismo nombre de una ya existente, PHP arrojaría un error fatal.

```php
<?php
function mi_funcion() {
  echo 'Hello World!';
}

function mi_funcion() {
  echo 'Hola Mundo!';
}

// Error fatal.
?>
```

Sin embargo, existe la posibilidad de que el nombre de una función pueda ser reutilizado, reemplazando a la función original. Para eso, la función original debe haber sido declarada dentro de un bloque condicional que chequea si ya existe.

```php
<?php
// Si no existe una función con el nombre "mi_funcion", se la declara.
if ( ! function_exists( 'mi_funcion' ) ) {
  function mi_funcion() {
    echo 'Hello World!';
  }
}
?>
```

De esta manera, si antes del condicional se declara otra función con el mismo nombre, aquella que se encuentra dentro del condicional no va a ser declarada.

```php
<?php
function mi_funcion() {
  echo 'Hola Mundo!';
}

// Si no existe una función con el nombre "mi_funcion", se la declara.
if ( ! function_exists( 'mi_funcion' ) ) {
  function mi_funcion() {
    echo 'Hello World!';
  }
}

mi_funcion(); // Imprime "Hola Mundo!".
?>
```

#### Pluggable functions

Una característica que nos ofrece WordPress es la de reservar ciertos nombres de funciones para que podamos utilizarlos para reemplazar funcionalidad nativa por la nuestra. A estas funciones, en la terminología propia de WordPress, nos referimos como ***[Pluggable Functions](https://codex.wordpress.org/Pluggable_Functions)***.

Estas funciones tienen la característica de ser definidas recién después de que todos los plugins se hayan cargado, y chequean si la función ya se declaró antes, usando `function_exists()`. Lo que hacen es algo como esto:

```php
<?php
if ( ! function_exists( 'wp_mail' ) ) :
function wp_mail( $args... ) {
	// Code here…
}
endif;
```

Esto permite que podamos definir nuestras propias funciones con ese nombre antes de que WordPress lo haga.

```php
<?php
// En nuestro plugin…

function wp_mail( $args... ) {
	// Code here…
}

// En WordPress…

if ( ! function_exists( 'wp_mail' ) ) :
function wp_mail( $args... ) {
	// Code here…
}
endif;
```

Si hacemos esto, cada vez que se llame a la función `wp_mail()` se va a ejecutar nuestro código para esa función en lugar del código de WordPress.

Esta posibilidad de redefinir funciones nativas es extremadamente útil para plugins que necesitan llevar a cabo sus propios procesos de envío de mails, validación de usuarios, generación de contraseñas, etc.

En nuestros plugins también podemos crear nuestras propias *Pluggable Functions*, para permitir a otros desarrolladores que redefinan funcionalidad de nuestros plugins cuando lo necesiten.

```php
<?php
// En un plugin de terceros...
function my_function() {
	_e( 'Hello John!', 'third-party-plugin' );
}

// En nuestro plugin...
if ( ! function_exists( 'my_function' ) ) :
function my_function() {
	_e( 'Hello world!', 'my-plugin' );
}
endif;
```

Hay que tener en cuenta que, si alguien quiere redefinir nuestras funciones, tiene que encargarse de declarar las suyas antes de que se declaren las nuestras. Como no podemos estar seguros del orden en el que WordPress va a cargar los archivos de plugins que vaya encontrando, lo que se puede hacer es definir nuestras *Pluggable Functions* dentro de un evento, para que otro desarrollador pueda usar ese mismo evento e insertar sus propias funciones especificando una prioridad más alta.

```php
<?php
#my-plugin.php

add_action( 'plugins_loaded', 'load_pluggable_functions', 10 );

function load_pluggable_functions() {
	require( dir( __FILE__ ) . '/my-pluggable-functions.php' );
}

#my-pluggable-functions.php

if ( ! function_exists( 'my_function' ) ) :
function my_function() {
	_e( 'Hello world!', 'my-plugin' );
}
endif;

#third-party-plugin.php

add_action( 'plugins_loaded', 'load_third_party_functions', 1 );

function load_third_party_functions() {
	require( dir( __FILE__ ) . '/third-party-functions.php' );
}

#third-party-functions.php

function my_function() {
	_e( 'Hello world!', 'my-plugin' );
}
```

De esta forma, otro desarrollador puede usar el mismo evento que usamos nosotros para cargar sus funciones, pero al cargarse su archivo de funciones con una prioridad más alta que el nuestro, las suyas se van a declarar primero, y las nuestras van a ser ignoradas, al no pasar el chequeo de `if( ! function_exists() )`.

Si bien esta es una práctica muy útil a la hora de hacer nuestros plugins extensibles, tampoco tenemos que abusar de ella. Va a haber muchas ocasiones en las que no queremos que nuestras funciones puedan ser redeclaradas en plugins de terceros, y solamente deberíamos permitirlo con aquellas que sepamos que alguien puede llegar a necesitar personalizar.

### Variables globales

En PHP, al crear una función también se crea un nuevo contexto cerrado para el bloque de código en el que se encuentra. Esto tiene la consecuencia de que las variables que se encuentran por fuera de la función no sean accesibles.

```php
<?php
$foo = 'Hola mundo';

function saludar() {
  $bar = 'Hello world';

  echo $bar; // Imprime "Hello world".

  echo $foo; // No imprime nada.
}

saludar();
?>
```

Para poder acceder a variables que se encuentran fuera de una función, dichas variables deben ser declaradas como **globales**. Esto puede hacerse usando la palabra clave `global` al momento de inicializar o asignar un valor a la variable.

```php
<?php
global $foo = 'Hola mundo'; // Declaración de global en asignación.
?>
```

Otra forma válida de hacerlo es la siguiente:

```php
<?php
global $foo; // Declaración de global en inicialización.

$foo = 'Hola mundo';
?>
```

Para poder recuperar el valor de una variable global dentro de una función, necesitamos inicializar la variable como global. De esta manera, PHP reconoce que se trata de la misma variable previamente declarada.

```php
<?php
global $foo = 'Hola mundo';

function saludar() {
  global $foo;

  $bar = 'Hello world';

  echo $bar; // Imprime "Hello world".

  echo $foo; // Imprime "Hola mundo".
}

saludar();
?>
```

Las variables globales se guardan dentro de la supervariable `$_GLOBALS`, que contiene un array cuyas claves son los nombres de todas las variables globales. Sabiendo esto, también podríamos acceder a la variable global usando `$_GLOBALS`.

```php
<?php
global $foo = 'Hola mundo';

function saludar() {
  $bar = 'Hello world';
  $foo = $_GLOBALS['foo'];

  echo $bar; // Imprime "Hello world".

  echo $foo; // Imprime "Hola mundo".
}

saludar();
?>
```

---

## Programación Orientada a Eventos

Un primer paso muy recomendable para empezar a entender cómo funciona WordPress a nivel código, independientemente de si trabajamos con plugins o themes, es empezar a entender el paradigma de programación orientada a eventos, ya que en él se basan la mayoría de las posibilidades de extensión que tenemos disponibles. Es importantísimo conocer este paradigma a la hora de interactuar con el código de WordPress, ya que desconocerlo puede llevar a muchísimo trabajo innecesario, o a mucho tiempo perdido en mantenimiento, mejoras y arreglo de bugs.

#### ¿Qué es la programación orientada a eventos?

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

#### Acciones

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
// Se imprime "comiendo ..."tax
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

#### Filtros

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

#### Remover eventos del registro

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

#### Conclusiones

Un buen manejo y comprensión de la Plugin API permiten agregarle a WordPress cualquier tipo de funcionalidad que pretendamos. Pero más allá del trabajo que podemos hacer con WordPress, dominar el paradigma de programación orientada a eventos puede abrirnos muchas puertas en el mundo del desarrollo de software en general, y en particular en el desarrollo web, donde se demuestra cada vez más útil, siendo adoptado por muchos otros proyectos open source, como jQuery, Drupal o Symfony.

---

## Sidebars

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

## Menús de Navegación
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

## Qué es un plugin y para qué sirve

En un sentido conceptual, podríamos decir que todo sitio o aplicación web se divide en tres partes: **contenido**, **presentación** y **funcionalidad**. **El contenido es aquella información variable que nuestro sitio o aplicación le muestra al usuario final**, y que alguien con los permisos adecuados puede agregar, modificar o eliminar. En la mayoría de las aplicaciones modernas es provisto de manera externa por un usuario, sin necesidad de modificar de manera manual los archivos que constituyen a la aplicación, y normalmente queda guardado en algún tipo de base de datos. **La presentación es la forma en la que esa información se le muestra al usuario**, y tiene que ver con la implementación de algún tipo de diseño gráfico sobre una interfaz; es básicamente cómo se ve nuestro proyecto. **La funcionalidad tiene que ver con todos los procesos internos que manejan el contenido** (por ejemplo la carga, edición y eliminación) y lo dejan preparado para ser presentado al usuario.

WordPress, como todo CMS (por *Content Management System*) o sistema de gestión de contenidos, se encarga por su cuenta de gran parte de las cuestiones técnicas relacionadas con la manipulación de información. Sin embargo, muchas veces vamos a encontrarnos con que necesitamos poder manejar algún tipo de información que no está disponible desde la instalación, o que se ejecuten ciertos procesos internos invisibles al usuario, o que necesitamos mostrar algo de una manera que no estaba prevista por las opciones de presentación que tenemos a nuestro alcance. Ese es el momento en el cual entran en acción los plugins y themes.

**Lo que se dice más típicamente en el ámbito de los desarrolladores que trabajan con WordPress es que, mientras los themes están pensados para manejar cuestiones de presentación, los plugins apuntan exclusivamente a agregar nueva funcionalidad.** Sin embargo, en este libro vamos a ponernos un poco más específicos con estas definiciones, y a decir que eso no siempre es tan así. Eso pasa porque la funcionalidad y la presentación no siempre son dos conceptos inseparables, sino que a veces están muy entrelazados. Y si bien siempre es una excelente práctica intentar separarlos cuanto sea posible, a veces se presentan algunas situaciones problemáticas en las que es complicado distinguirlos. Por eso nos vamos a encontrar muy típicamente con themes que manejan algunas cuestiones de funcionalidad, o con plugins que ofrecen algún tipo de presentación. Incluso hay cosas que bien podrían ser incluidas tanto en plugins como en themes, y en ese punto nos vemos obligados a decidir dónde es mejor colocarlas.

Teniendo en cuenta todo esto, podemos decir que **los themes, más que tener que ver específicamente con la presentación, en realidad tienen el foco puesto en ella, sin dejar completamente de lado la funcionalidad**. Son aquello que, a grandes rasgos, va a definir de qué manera se va a ver nuestro sitio, pero pueden ofrecer ciertas características de comportamiento. **Lo mismo se aplica a los plugins, pero a la inversa: tienen el foco puesto en agregar nueva funcionalidad a nuestro sitio, pero pueden ofrecer nuevas formas de visualización**.

A partir de tener presentes cuáles son estas diferencias y similitudes conceptuales básicas entre plugins y themes, podemos empezar a construir nuestros propios plugins.

## Cómo crear un plugin básico

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

## Shortcodes

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

## Tipos de Contenido
### Post Types predefinidos

Al construir una aplicación o un sitio web con cierto nivel de complejidad, es muy común que en algún momento se necesite algún tipo de contenido diferente de los que ya tenemos, alguna nueva entidad para la que sea posible crear, editar y modificar registros.

Si nuestra aplicación está basada en PHP y MySQL, normalmente se tiende a diseñar e implementar nuevas tablas en la base de datos para que contengan la información de las nuevas entidades. A continuación se escribe el código que gestione la carga, edición y eliminación de contenidos, el acceso de los usuarios de la aplicación a esos contenidos, y las relaciones e interacciones con otras entidades.

Lo que suele pasar con este enfoque es que, a medida que la aplicación se vuelve más grande y se necesitan más tipos de contenido, también se vuelve más complejo el mantenimiento de nuestras entidades, ya que sus datos y funcionalidad pueden variar mucho de una a otra, por tener diferentes características.

Sin embargo, si se trabaja con un framework, a menudo este suele proveer una serie de herramientas para crear nuevos tipos de contenido de una manera previamente definida y estandarizada, de forma que se fuerce una cierta coherencia interna para lo que vayamos a crear.

En el caso de WordPress, esta serie de herramientas esta cubierta por la API de ***[Post Types](https://codex.wordpress.org/Post_Types)***, la cual permite interactuar con tipos de contenidos nuevos o ya existentes siguiendo un conjunto de reglas y entendiendo ciertos conceptos fundamentales.

Por defecto, con su instalación básica, WordPress presenta cinco tipos de contenido: posts, páginas, revisiones, archivos y menús de navegación. Todos tienen diferencias entre ellos, las cuales se pueden notar al cargar o editar información, y todos están construidos usando la API de Post Types. Esta API permite manejar diferentes tipos de datos para diferentes tipos de contenidos de una forma estandarizada.

### Custom Post Types

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

## Taxonomías

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
