# Clase 4

[comment]: # (Manejo de datos en PHP)
[comment]: # (Intro a MySQL)

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
