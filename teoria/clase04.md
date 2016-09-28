# Clase 4

[comment]: # (Intro a MySQL)

## PHP: Manipulación de datos

Una de las características principales de PHP es la de poder procesar información ingresada por un usuario de un sitio web. Normalmente, esta información se envía al servidor web por medio de un formulario de contacto o por determinados parámetros añadidos a la URL de un sitio. Este tipo de envío de información es lo que en desarrollo web se llama **solicitud** o **_request_**.

Existen dos tipos de solicitudes, a los que llamamos **métodos**: _POST_ y _GET_. La diferencia entre ambos consiste en que, usando _post_, la información es enviada de forma directa al servidor web, de manera invisible al usuario. Usando _get_, la información se agrega a la URL de la página que procesa la solicitud, lo cual el usuario puede ver en su navegador.

### GET

Una manera de entender cómo funciona el método GET es por medio del análisis de una URL como la siguiente:

`http://localhost/misitio/form.php?nombre=Roque&apellido=Molina`

Al visitar esa URL estaríamos enviando los datos `nombre` y `apellido` al archivo `form.php` ubicado en nuestro sitio. Es importante notar cómo se escriben los datos, con el formato `clave=valor`. El primer dato a enviar estará precedido del caracter `?`, y a partir del segundo dato se usa `&`.

Al recibir los datos en PHP, los mismos quedan guardados en la variable superglobal `$_GET` como un array. De esta forma, los datos enviados en el ejemplo serían equivalentes a esto:

```php
<?php
$_GET = array(
    'nombre'   => 'Roque',
    'apellido' => 'Molina',
);
?>
```

Y podríamos obtenerlos de esta manera:

```php
<?php
echo '¡Hola ' . $_GET['nombre'] . ' ' . $_GET['apellido'] . '!';
// Se imprime "¡Hola Roque Molina!".
?>
```
Las URLs con datos adjuntos tambien pueden generarse automáticamente a partir de un formulario HTML.

```php
<form method="GET" action="form.php">
    <label for="nombre">Nombre</label>
    <input name="nombre" type="text" /><br />
    <label for="apellido">Apellido</label>
    <input name="apellido" type="text" /><br />
    <input type="submit" value="Enviar" />
</form>
```

Al completar los datos y enviar el formulario, el navegador se redirige a la página especificada en el atributo `action` de la etiqueta `form`. Si el valor especificado en el atributo `method` es `GET`, a la URL de redirección se le adjuntan como parámetros todos los valores ingresados en el formulario, y los nombres de sus claves se corresponden con los valores del atributo `name` de cada campo.

Por ejemplo, si en el campo "Nombre", generado con el código HTML `<input name="nombre" type="text" />`, se ingresa el valor "Roque", dado que el valor del atributo `name` del campo es `nombre`, se adjunta a la URL la combinación `nombre=Roque`.

Al completar y enviar el formulario, el navegador nos redirige a una URL similar a la del primer ejemplo:

`http://localhost/misitio/form.php?nombre=Roque&apellido=Molina`

### POST

El método POST evita que la información enviada al servidor web sea visible al usuario, inyectándola al navegador de forma directa. Normalmente se lo usa en combinación con formularios HTML, especificando `POST` en el atributo `method` de la etiqueta `form`.

```php
<form method="POST" action="form.php">
    <label for="nombre">Nombre</label>
    <input name="nombre" type="text" /><br />

    <label for="apellido">Apellido</label>
    <input name="apellido" type="text" /><br />

    <input type="submit" value="Enviar" />
</form>
```

Al igual que con GET, al completar el formulario el navegador redirige al usuario a la página especificada en `action`, pero con la diferencia de que no se agrega información a la URL. Esa información es accesible desde PHP usando la variable superglobal `$_POST`, que también almacena los datos recibidos como un array. De esta manera, si hubiéramos ingresado "Roque" para el nombre y "Molina" para el apellido, el valor de `$_POST` sería este:

```php
<?php
$_POST = array(
    'nombre'   => 'Roque',
    'apellido' => 'Molina',
);
?>
```

Y podríamos obtener la información de la misma manera que con GET:

```php
<?php
echo '¡Hola ' . $_POST['nombre'] . ' ' . $_POST['apellido'] . '!';
// Se imprime "¡Hola Roque Molina!".
?>
```

### REQUEST

En PHP también existe la variable superglobal `$_REQUEST`, que es una combinación de los valores recibidos por POST y por GET, es decir de cualquier tipo de solicitud. Por eso podemos usarla para manipular datos indistintamente del método con el que los hayamos recibido:

```php
<?php
echo '¡Hola ' . $_REQUEST['nombre'] . ' ' . $_REQUEST['apellido'] . '!';
// Se imprime "¡Hola Roque Molina!".
?>
```

##  Manipulación de Base de Datos en WordPress

WordPress define una clase llamada `wpdb`, la cual contiene un conjunto de métodos que se utilizan para interactuar con una base de datos. Su objetivo principal es facilitar la interacción con la base de datos de WordPress, pero se puede utilizar para comunicarse con cualquier otra base de datos.

La conexión con la base de datos de una instalación de WordPress se establece a partir de la información ingresada en el archivo `wp-config.php`. Al establecer esta conexión, WordPress crea, a partir de la clase `wpdb` una variable global llamada `$wpdb`. Esta variable permite interactuar de manera directa con la base de datos a través de los métodos definidos en la clase `wpdb`.

### Selección de una variable

El método `wpdb::get_var()` devuelve un valor único por medio de una consulta a la base de datos. Se usa en casos en los que se consulta por un único valor.

**Ejemplo:**

```php
<?php
$user_count = $wpdb->get_var( "SELECT COUNT(*) FROM $wpdb->users" );
echo '<p>User count is ' . $user_count . '</p>';
```

### Selección de una fila

Para recuperar una fila completa de una consulta se utiliza `wpdb::get_row()`. El método puede devolver la fila como un objeto, un array asociativo, o un array de índice numérico.

**Ejemplo:**

```php
<?php
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", OBJECT ); // Devuelve un objeto.
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", ARRAY_A ); // Devuelve un array asociativo.
$row = $wpdb->get_row( "SELECT * FROM $wpdb->links WHERE link_id = 10", ARRAY_N ); // Devuelve un array de índice numérico.
?>
```

Los valores de la variable `$row` son los nombres de las columnas del resultado de la consulta SQL (en este ejemplo, todas las columnas de la tabla `wp_links`, pero también se podría consultar por columnas específicas).

De esta manera, si variable es un objeto, podríamos obtener el valor de la columna `link_id` de esta manera:

```php
<?php echo $row->link_id; // Imprime "10" ?>
```

En caso de que la variable sea un array asociativo `link_id` se obtendría así:

```php
<?php echo $row['link_id']; // Imprime "10" ?>
```

Y para un array de índice numérico:

```php
<?php echo $row[1]; // Imprime "10" ?>
```

### Selección de una columna

Para seleccionar una columna, contamos con el método `wpdb::get_col()`. Este método devuelve un array de índice numérico con la cantidad de resultados especificados en la consulta (o todos, si no se especifica un límite). En el siguiente ejemplo se seleccionan 10 IDs de la tabla `wp_posts`:

```
<?php
$col = $wpdb->get_col( "SELECT ID FROM $wpdb->posts LIMIT 0, 10" );
?>
```

### Inserción de una fila

Para insertar una nueva fila en una tabla se usa el método `wpdb::insert()`.

Suponiendo una tabla con el nombre `table` y con dos columnas llamadas `column1` y `column2`, podemos asignar nuevos datos para esas columnas de la siguiente manera:

```php
<?php
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
?>
```

### Eliminación de una fila

El método `wpdb::delete()` se puede utilizar para borrar registros de una tabla. Su uso es muy similar a `wpdb::insert()`. Devuelve el número de filas actualizadas o `false` en caso de error.

**Ejemplo:**

```php
<?php $wpdb->delete ( 'table', array ( 'ID' => 1 ), array ( '%d') ); ?>
```
