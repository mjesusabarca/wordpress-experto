# Tareas Asincrónicas

---------

## Ejecución de procesos por medio de AJAX

Ajax es una tecnología asíncrona, en el sentido de que los datos adicionales se solicitan al servidor y se cargan en segundo plano sin interferir con la visualización ni el comportamiento de la página, aunque existe la posibilidad de configurar las peticiones como síncronas de tal forma que la interactividad de la página se detiene hasta la espera de la respuesta por parte del servidor.

### AJAX en WordPress

Si desarrollas en WordPress, una gran cantidad de trabajo puede hacerse más sencillo utilizando Ajax. El corazón de este es el archivo **wp-admin/admin-ajax.php**. La finalidad de este fichero es el manejo de las peticiones AJAX.

#### admin-ajax.php

Cuando el archivo admin Ajax recibe una solicitud, este se encarga de encender el motor de WordPress e inicializar todo lo necesario. Luego, inspecciona la solicitud para realizar la acción. Esto no es nada más que un valor de cadena arbitraria. Si ninguna acción es enviada en la solicitud, "morirá" con 0 , lo que indica un fallo. Para fines de ejemplo, vamos a suponer que la acción enviada en una solicitud es **get_comments**.

Una vez que WordPress se ha inicializado, se dispara una de las dos acciones dependiendo del estado de conexion. Si el usuario está conectado, WordPress lanzará **wp_ajax_get_comments**. Así, podrás "engancharte" a esta acción y seguir el proceso de solicitud y enviar una respuesta. Si el usuario no está conectado (es decir, no tiene privilegios) WordPress disparará **wp_ajax_nopriv_get_comments**.

Esta es una buena manera de dividir los comportamientos. Si una acción mediante Ajax sólo debe realizarse en la sección de administración, no utilice el **hook _nopriv_**. Esto ayudará a mantener todo un poco más seguro.

### Objeto XMLHttpRequest

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

### AJAX asíncrono

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

----------

## Programación de tareas por medio de WP-Cron

WordPress tiene un sistema de automatización de eventos cuyas funciones del núcleo las puedes encontrar en **wp-cron.php** y en **cron.php** dentro de la carpeta **wp-includes** de nuestra instalación.

Para hablar de **cron** nos tenemos que ir por un momento a Unix donde en este sistema cron es un administrador de procesos en segundo plano, es decir, es un programa que se ejecuta en segundo plano y que se dedica a lanzar uno o varios procesos cada determinado tiempo. Según la Wikipedia y para aquellos usuarios de Windows es algo parecido a las Tareas Programadas.

WordPress ha intentado emular este administrador de procesos en su núcleo de forma que se puedan ejecutar acciones cada cierto tiempo, pero como vamos a ver en WordPress hay una serie de limitaciones, es decir, no funciona exactamente igual que el cron de Unix.

Mientras que en Unix los procesos programados se ejecutan puntualmente cada tiempo indicado en WordPress la cosa no es así, sino que primero se programa la acción cada cierto tiempo, posteriormente se espera una visita al sitio web y por último se comprueba que ese tiempo se haya superado para ejecutar el proceso.

### Limitaciones de WP-Cron

Este pseudo-cron tiene una serie de limitaciones, la primera que se puede deducir a partir del último párrafo es que, ¿quiere decir que si no tengo visitas el evento no se dispara? Exactamente. Por ejemplo, si hemos programado un evento para que suceda diariamente, digamos que a las 12 de la noche, si durante las 11 horas siguientes nadie visita nuestra página web este evento no se lanzará hasta después de esas 11 horas.

Otro problema que nos puede dar es la carga del servidor por lo que todas las acciones programadas deberán ser simples para que no consuma muchos recursos del servidor. Imaginemos un sitio en WordPress con 100 tareas programadas para cada 5 minutos en las cuales se haga una copia de seguridad de toda la base de datos, de los ficheros, que se envíen 300 emails y 400 castelleres hagan un castillo. Sin duda en ejemplo un poco exagerado pero sirve para ilustrar lo que queremos decir. Procesos pesados y programados harán lenta la carga del sitio.

### Un paseo por las funciones

```
wp_schedule_event($timestamp, $recurrence, $hook, $args)
```

Esta es la principal función, o al menos a la que más uso se le dado hasta ahora pues es la que programa un evento. Soporta los siguientes parámetros:

* $timestamp: Tiene que ser en formato UNIX, para ello podemos usar la función time() de PHP.
* $recurrence: Es el periodo de tiempo en el que se ejecutará, por defecto soporta ‘hourly’, ‘daily’ y ‘twicedaily’ aunque se pueden generar más periodos como veremos.
* $hook: Gancho de la acción que se ejecutará.
* $args: Es un array opcional de argumentos para la función asociada a la acción definida por el parámetro anterior.

Por poner un ejemplo pensemos en un plugin que tiene que programar un evento.

```
register_activation_hook( __FILE__, 'funcion_activacion' ); //Función a ejecutar cuando se activa el plugin
register_deactivation_hook( __FILE__, 'funcion_desactivacion' ); //Función a ejecutar cuando se desactiva el plugin

function funcion_activacion() {
wp_schedule_event( time(), 'daily', 'envio_mails_hook' );
}

function funcion_desactivacion() {
wp_clear_scheduled_hook( 'envio_mails_hook' );
}

add_action( 'envio_mails_hook', 'funcion_envio_mails' );
function funcion_envio_mails() {
// Código para el envío de emails
}
```

En el ejemplo anterior hemos introducido otra función.

```
wp_clear_scheduled_hook( $hook, $args );
```

Esta función se encarga de desprogramar todos los eventos asociados a una acción, como se puede ver en el ejemplo anterior. Los parámetros que acepta son:

* $hook: El nombre del gancho al que se le asocia la función.
* $args: Array de argumentos para la función asociada al gancho.

Otra función muy parecida es:

```
wp_unschedule_event( $timestamp, $hook, $args );
```

En lugar de desprogramar todos los eventos asociados a un hook solamente desprograma aquel evento indicado por $timestamp, el resto de argumentos son los mismos que en **wp_clear_scheduled_hook**.

```
wp_next_scheduled( $hook, $args );
```

Devuelve un **timestamp** en formato UNIX del siguiente evento, por ejemplo si hemos programado un evento cada hora, nos devolverá el timestamp de la próxima vez que se deberá ejecutar la acción programada.

Nos puede servir junto con la función anterior para desprogramar un evento.

```
$timestamp = wp_next_scheduled( 'envio_mails_hook' );
wp_unschedule_event( $timestamp, 'envio_mails_hook' );
```

La última función que consideramos muy importante es:

```
wp_get_schedules();
```

Esta función devuelve un array con todos los periodos de tiempos definidos. Así, la función en sí no es lo que se considera muy importante, sino la posibilidad de crear nuevos periodos de tiempo:

```
add_filter( 'cron_schedules', 'anyadir_semana' );

function anyadir_semana( $schedules ) {
$schedules['semanal'] = array(
'interval' => 604800, // En segundos: 7 * 24 * 60 * 60
'display' => __( 'Semanalmente' )
);
return $schedules;
}
```

WordPress nos ofrece un sistema para programar eventos que aunque imperfecto sí es útil para tareas sencillas y no demasiado pesadas. El núcleo de WordPress nos ofrece una serie de funciones muy útiles para programar nuestros propios eventos, por ejemplo podemos crear un tema que reciba actualizaciones o un plugin para que haga una copia de la base de datos automáticamente y la envíe por email a un correo electrónico.

# Manejo de Usuarios

---------------

## Herramientas de administración de usuarios

Uno de los temores de cualquier administrador de un blog es la configuración de usuarios o, en su defecto, tener que administrar y “alimentar” el mismo toda el blog con contenidos nuevos generados por otros en su ordenador, y todo lo que esto conlleva de trabajo posterior de ajustes de código, etc. Con WordPress esto es asunto del pasado.

Con el sistema de perfiles y permisos se puede dar acceso al panel de administración a cuantos usuarios se desee y configurar exactamente a qué funciones podrán acceder.


### Crear un nuevo usuario

Haz click en la opción **Todos los usuarios** ubicado en la sección **Usuarios** en el menú del escritorio.

<div align="center">
![](http://i.imgur.com/D2QQSh4.png)
</div>
<br>
En la parte superior haz click en la opción **Agregar usuario** o **Add new**.

<div align="center">
![](http://i.imgur.com/Al0lGTt.png)
</div>
<br>
Rellena el formulario y define el rol del nuevo usuario. Recuerda guardar. La invitación le llegará por el email que colocaste en los datos.

<div align="center">
![](http://i.imgur.com/nEgHRKS.png)
</div>
<br>
El usuario nuevo o cualquier usuario pueden modificar la contraseña de su perfil haciendo click en **Tu perfil** en **Usuarios**.

<div align="center">
![](http://i.imgur.com/uWTFp2C.png)
</div>

### Plugins para la Administración de Usuarios


#### New User Approve

Determinado número de blogs de WordPress suelen permitir que sus visitantes se registren mediante una cuenta gratuita; ello tiene el único objetivo de tratar de ganar más seguidores a cada una de las noticias seleccionadas o por lo menos, **a las que se considera más importantes dentro de sitio web**. Cuando un visitante hace uso de este registro gratuito obligatoriamente tendrá que crear su nombre de usuario acompañado de la respectiva contraseña.

<div align="center">
![](http://i.imgur.com/D7uhhOm.jpg)
</div>


El plugin que estamos mencionando en este momento ofrece al administrador (que puedes ser tú) la posibilidad de **permitir o bloquear el registro**. Tomando en cuenta que hoy en día existe una gran cantidad de procesos automatizados (como robots) que pueden llegar a crear registros de cuentas falsas, el administrador será el que deberá denegar dicha solicitud. 

El plugin actúa de una forma interesante, pues cada vez que llegue un visitante hacia el blog de WordPress y solicite una cuenta de suscripción, en ese momento el administrador del blog de WordPress **recibirá una notificación mediante su correo electrónico**. Posteriormente solo tiene que dirigirse hacia la configuración de su sitio web y revisar el perfil de quien lo está solicitando, pudiendo aprobar o denegar su solicitud desde este mismo lugar.

#### Members

Este es otro interesante plugin que tiene similares características a las que mencionamos un poco más arriba. Con el mismo, el administrador del blog de WordPress tiene la posibilidad de aumentar responsabilidades a algunos de sus colaboradores, pudiendo ser éstas un autor, el editor o el suscriptor de su blog de WordPress.

<div align="center">
![](http://i.imgur.com/GOl11Q2.jpg)
</div>


El mejoramiento de las responsabilidades de los roles de estos colaboradores va desde el control de los plugins instalados hasta la misma edición de los diferentes posts; ello quiere decir, que si este plugin ha aumentado roles de trabajo a un suscriptor (sólo por dar un ejemplo extremo), el mismo tendrá la posibilidad de **revisar el contenido de un artículo y realizar determinado número de modificaciones** si así lo considera necesario. La captura que hemos propuesto en la parte superior nos muestra un ligero ejemplo de ello. 

La interfaz que nos ofrece este plugin a la hora de facilitar roles a un colaborador específico es fácil de ser manejada y en donde, se puede admirar a diferente número de columnas que nos indican lo que se ha otorgado a cada uno de ellos. **La última columna mostrará la cantidad de funciones** que se ha ofrecido a un colaborador específico; de manera concreta, el administrador es el que tiene mayor número de funciones para utilizar dentro de un blog de WordPress, estando en menor grado quien forma parte del blog de WordPress como un suscriptor.

#### Multisite User Management

Suponiendo que un administrador tenga a su cargo una gran cantidad de blogs (algunos los definen como multisitios), quizá sea de su interés hacer **que uno de sus redactores participe en todos ellos**. Con este plugin se tendrá la posibilidad de programar dicha tarea.

<div align="center">
![](http://i.imgur.com/XN6sPyK.jpg)
</div>


Ello quiere decir, que con el plugin se puede llegar a elegir a un redactor específico para **asignarle nuevas funciones a todos o a unos cuantos blogs** en los que deberá participar en adelante. Además de ello, con el plugin también se puede llegar a hacer que este colaborador actúe únicamente como redactor, autor, suscriptor o cualquier otra designación que el administrador de la red de blogs considere necesario.


---------------

## Manipulación de roles y capacidades

WordPress usa el concepto de **Roles**, y ofrece algunos prediseñados para dar a los administradores del blog la capacidad de controlar los poderes de lo que puede hacerse en el sitio. Un administrador puede otorgar acceso a las funciones de **escribir y editar entradas, crear páginas, crear enlaces, crear categorías, moderar comentarios, administrar plugins, administrar temas, y administrar usuarios**. Una forma lógica y sencilla de asignar estas capacidades, es mediante la asignación de roles a los usuarios.

WordPress tiene seis Roles prediseñados: **Super Administrador, Administrador, Editor, Autor, Colaborador y Suscriptor**. A cada rol se le permite realizar una serie de tareas llamadas **Capacidades**. Hay muchas capacidades incluyendo **publish_posts** (publicar entradas), **moderate_comments** (moderar comentarios), y **edit_users** (editar usuarios). Hay un conjunto de capacidades preasignadas a cada Rol en este diseño.

Quienes tienen el Rol de **Administrador** se les otorga **todas las capacidades**. Cada uno de los otros Roles tienen un número decreciente de capacidades asignadas. Por ejemplo, el Rol de **Suscriptor** tan solo tiene la capacidad de **lectura**. Un Rol en particular no debería ser considerado como superior a otro Rol. Es mejor, considerar que los Roles definen las responsabilidades de los usuarios dentro del blog.

La **API de Plugins de WordPress** permite crear nuevos Roles y Capacidades, así como cambiar las Capacidades preexistentes en los Roles que WordPress proporciona de manera predeterminada.

### Sumario de roles

* **Super Admin** - Quien cuenta con acceso a la característica de administración de la red de blogs completa.
* **Administrador** - Quien tiene acceso a todas las características de administración de un sitio en particular.
* **Editor** - Quien puede publicar y editar entradas, propias y de otros usuarios.
* **Autor** - Quien puede publicar y editar sus propias entradas.
* **Colaborador** - Quien puede escribir y editar sus propias entradas pero no publicarlas.
* **Subscriptor** - Quien solamente puede editar su perfil.

Al momento de instalar WordPress, una cuenta de Administrador con todas las Capacidades es creada automáticamente.

El rol preasignado a las nuevas usuarias del sitio, puede definir en el panel Ajustes generales.

---------------

## Creación y modificación de datos adicionales

Lo habitual cuando instalamos WordPress es que el primer usuario/contraseña que utilizamos durante el proceso de instalación pase a ser automáticamente el administrador de la instalación de WordPress.

La gran mayoría de instalaciones con este usuario tendrán suficiente para administrar su web desde el dashboard y no será necesario que se planteen la creación de nuevos usuarios con el mismo rol u otros diferentes personalizados.

Si por el contrario tu sitio web es administrado por varias personas y necesitas **gestionar roles** y principalmente crear nuevos usuarios que puedan atender determinadas tareas de gestión o administración del dashboard, te planteamos a continuación una opción sencilla y potente para que puedas **crear usuarios con diferentes roles en WordPress**.

Vamos a servirnos del plugin **WPFront User Role Editor** escrito por Syam Mohan y disponible a través del Directorio oficial de Plugins de **WordPress.org**.

Este plugin nos va a permitir reasignar roles a usuarios existentes, o crear nuevos roles, con funciones específicas o limitadas, para posteriormente asignar a uno o varios usuarios a este nuevo perfil o rol.

### Añade nuevos usuarios en WordPress

1. Accede al dashboard, Usuarios.
2. Click en "Añadir nuevo".
3. Rellena el formulario para el nuevo usuario (principalmente campos requeridos).
4. Si deseas que reciba la contraseña de acceso marca la casilla ¿Enviar Contraseña?.
5. Asignale un perfil (rol).

<div align="center">
![](http://i.imgur.com/MB9Lxda.jpg)
</div>

### Crea un rol o perfil personalizado

1. Accede al dashboard, Roles.
2. Click en "Add New "Añadir Nuevo" rol.
3. En Display Name y Rol Name coloca el nombre deseado (ej. Gestor).
4. En Capabilities (Capacidades) tendrás que administrar los permisos.
5. Finalizado el proceso, click en Add New Role para guardar cambios.

<div align="center">
![](http://i.imgur.com/JxMWPfH.jpg)
</div>

<div align="center">
![](http://i.imgur.com/pHUq3T6.jpg)
</div>

<div align="center">
![](http://i.imgur.com/3hrd5vW.jpg)
</div>
<br>
Una vez creado el nuevo Rol personalizado desde Roles, All Roles podrás ver en el listado añadido el nuevo rol.

<div align="center">
![](http://i.imgur.com/UNK0Itt.jpg)
</div>
<br>
Desde All Roles podrás Editar o Eliminar perfiles de usuario creados para adaptarlos a tus necesidades. La recomendación es no eliminar los que vienen por defect y en todo caso hacer una copia del perfil o rol deseado y adaptarla a tu nuevo "grupo" de usuarios o rol ya sea para un solo usuario o para un grupo de estos que vayan a operar bajo dicho perfil.
