# Clase 8

[comment]: # (Faltantes:)
[comment]: # ([Conceptos de asincronismo])
[comment]: # ([AJAX con jQuery])

## Workshop

Continuar con el desarrollo en clase del plugin o theme propio, implementando los conocimientos adquiridos hasta la clase 8, y siguiendo los lineamientos de calidad de código detallados de el [Plugin Developer Handbook](https://developer.wordpress.org/plugins/). El objetivo final es subir el plugin o theme al directorio oficial de WordPress.

## Tareas Asincrónicas

### Ejecución de procesos por medio de AJAX

AJAX es una tecnología asincrónica, en el sentido de que ciertos datos se solicitan al servidor y se cargan en segundo plano sin interferir con la visualización ni el comportamiento de la página, hasta el momento en que deban mostrarse.

#### AJAX en WordPress

En WordPress, una gran cantidad de trabajo puede hacerse más sencillo utilizando AJAX. El corazón de este es el archivo **wp-admin/admin-ajax.php**. La finalidad de este fichero es el manejo de las peticiones AJAX.

##### admin-ajax.php

Cuando el archivo `admin-ajax.php` recibe una solicitud, este se encarga de encender el motor de WordPress e inicializar todo lo necesario. Luego, inspecciona la solicitud para realizar la acción. Esto no es nada más que un valor de cadena arbitraria. Si ninguna acción es enviada en la solicitud, "morirá" con 0 , lo que indica un fallo. Para fines de ejemplo, vamos a suponer que la acción enviada en una solicitud es **get_comments**.

Una vez que WordPress se ha inicializado, se dispara una de las dos acciones dependiendo del estado de conexion. Si el usuario está conectado, WordPress lanzará **wp_ajax_get_comments**. Así, podrás "engancharte" a esta acción y seguir el proceso de solicitud y enviar una respuesta. Si el usuario no está conectado (es decir, no tiene privilegios) WordPress disparará **wp_ajax_nopriv_get_comments**.

Esta es una buena manera de dividir los comportamientos. Si una acción mediante AJAX sólo debe realizarse en la sección de administración, no utilice el **hook _nopriv_**. Esto ayudará a mantener todo un poco más seguro.

#### Objeto XMLHttpRequest

Lo primero que debemos hacer para que podamos realizar peticiones al servidor con AJAX es definirnos el objeto del siguiente modo:

```
<script>
 function AJAXCrearObjeto(){
  var obj;
  if (window.XMLHttpRequest) { // no es IE
   obj = new XMLHttpRequest();
  }
  else { // Es IE o no tiene el objeto
   try {
    obj = new ActiveXObject(“Microsoft.XMLHTTP”);
   }
   catch (e) {
    alert(‘El navegador utilizado no está soportado’);
   }
  }
  return obj;
 }
</script>
```

Existen diversas codificaciones del objeto. Aquí hemos puesto una de ellas como ejemplo.

#### AJAX asíncrono

La codificación resumida de una llamada asíncrona suele ser del siguiente modo:

```
<script>
 // Creamos la variable parametro
 parametro = ‘Datos pasados por POST’;
 oXML = AJAXCrearObjeto();
 // Preparamos la petición con parametros
 oXML.open(‘POST’,’pagina.php’, true);
 // Preparamos la recepción
 oXML.onreadystatechange = leerDatos;
 // Realizamos la petición
 oXML.send(‘parametro=’ + escape(parametro));

 function leerDatos() {
  if (oXML.readyState == 4) {
   alert (oXML.responseText);
  }
 }
</script>
```
