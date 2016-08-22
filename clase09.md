# Virtualización

-----------

## Ventajas y desventajas de los ambientes virtualizados

### Ventajas

Permiten que todos los miembros del equipo tengan configurado un mismo entorno de trabajo en sus computadoras, que proporcionan un único flujo de trabajo consistente, para ayudar a maximizar la productividad y flexibilidad de un equipo de desarrolladores.

Otras de las ventajas que podemos destacar es que se trabaja en entornos de desarrollo más parecido a los de producción. Ademas le permite al desarrollador aislar el ambiente de desarrollo de sus herramientas como pueden ser editores de texto, debuggers, explorador web, etc.

Tambien tiene la ventaja de que solo una persona del equipo es necesaria para crear ese entorno, se lo pasa al resto del equipo, cada uno de los cuales "levanta" el entorno previamente configurado y ya lo tiene listo para usar.



### Desventajas 

La principal desventaja de la virtualización, es que lógicamente el sistema principal que soportara las máquinas virtuales, debe disponer de una mayor cantidad y potencia de recursos a mayor número de sistemas  que queramos tener virtualizados en él. Los componentes principales que determinarán el número de máquinas virtuales que se podrán soportar sobre un hardware y el rendimiento de cada una de ellas son: la cantidad y velocidad de memoria RAM, la potencia del procesador y la velocidad de lectura, acceso y transferencia del disco duro, aunque hay más factores que determinarán el rendimiento final del sistema.

Otra de las desventajas es que en ocasiones aparecen problemas en la compatibilidad con el hardware virtualizado, aunque en las útlimas versiones de los programas de virtualización estos problemas no se presentan casi nunca.

También podríamos contar como desventaja que todavía nos es difícil configurar ciertos recursos en máquinas virtualizadas, en ocasiones por entendimiento y otras por limitaciones del propio software de virtualización. A veces al ser un método relativamente nuevo, nos es difícil pensar que en un mismo sistema físico podamos tener varios sistemas independientes funcionando.

-----------

## Herramientas de virtualización: VirtualBox, Vagrant y VVV

### VirtualBox

Oracle VM VirtualBox es un software de virtualización, creado originalmente por la empresa alemana innotek GmbH. Actualmente es desarrollado por Oracle Corporation. Por medio de esta aplicación es posible instalar sistemas operativos adicionales, conocidos como "sistemas invitados" dentro de otro sistema operativo "anfitrión".

Resulta una aplicación extremadamente útil si estamos casi siempre trasteando con sistemas operativos nuevos o versiones Betas inestables. Un punto importante que tenemos que tener presente es que la experiencia de uso del sistema no será igual a la de tener un sistema operativo instalado limpiamente en tu ordenador.

Entre los sistemas operativos soportados (en modo anfitrión) se encuentran GNU/Linux, Mac OS X, OS/2 Warp , Microsoft Windows, y Solaris/OpenSolaris, y dentro de ellos es posible virtualizar los sistemas operativos FreeBSD, GNU/Linux, OpenBSD, OS/2 Warp, Windows, Solaris, MS-DOS y muchos otros.

La aplicación fue inicialmente ofrecida bajo una licencia de software privativo, pero en enero de 2007, después de años de desarrollo, surgió VirtualBox OSE (Open Source Edition) bajo la licencia GPL 2. Actualmente existe la versión privativa Oracle VM VirtualBox, que es gratuita únicamente bajo uso personal o de evaluación, y esta sujeta a la licencia de "Uso Personal y de Evaluación VirtualBox" (VirtualBox Personal Use and Evaluation License o PUEL) y la versión Open Source, VirtualBox OSE, que es software libre, sujeta a la licencia GPL.

### Vagrant 

Vagrant es una herramienta gratuita de línea de comandos, disponible para Windows, MacOS X y GNU/Linux, que permite generar entornos de desarrollo reproducibles y compartibles de forma muy sencilla. Para ello, Vagrant crea y configura máquinas virtuales a partir de simples ficheros de configuración.

Basta con compartir el fichero de configuración de Vagrant (llamado “Vagrantfile”) con otro desarrollador para que, con un simple comando, pueda reproducir el mismo entorno de desarrollo. Esto es especialmente útil en equipos formados por varias personas, ya que asegura que todos los desarrolladores tienen el mismo entorno, con las mismas dependencias y configuración. Con Vagrant, compartir pesadas máquinas virtuales o el ya mítico “en mi ordenador funciona” (causado generalmente por diferentes configuraciones o versiones de software) son cosas del pasado.

Además, dado que la configuración de la máquina virtual es un simple fichero de texto plano, podemos incluir este fichero en nuestro repositorio en el control de versiones, junto con el resto del código del proyecto. De esta manera, un nuevo desarrollador que se incorpore al equipo simplemente tendrá que clonar el repositorio del proyecto y ejecutar Vagrant para tener el entorno de desarrollo montado y funcionando en cuestión de minutos.

Por defecto, Vagrant utiliza VirtualBox como motor de máquinas virtuales, aunque existe la opción de utilizar VMWare Workstation (Windows) o VMWare Fusion (MacOS X) con un plugin de pago.

### VVV

VVV o Varying Vagrant Vagrants permite configurar un servidor Ubuntu con todo lo necesario listo para desarrollar un plugin o tema para WordPress utilizando Vagrant claro. También te da facilidades si quieres desarrollar funcionalidades para el core de WordPress.

--------------

## Construcción de sitios en ambientes virtualizados

Para comenzar a realizar la construcción de sitios en ambientes virtualizados es necesario seguir una serie de pasos comenzando por su instalación:

**1.**  Instalar tanto VirtualBox como Vagrant.
**2.** Te recomendaba instalar vagrant-hostmanager, pues bien, después de haberlo probado te recomiendo vagrant-hostupdater que es el que recomienda VVV en su lugar. Las funciones cumplidas por los dos plugins son similares. Lo puedes instalar con: 

```
 vagrant plugin install vagrant-hostupdater
```

**3.** Instalar vagrant-triggers. Este plugin es muy interesante ya que permite ejecutar scripts antes/después de ejecutar ciertas acciones en tu máquina tales como “vagrant halt” o “vagrant destroy”. En VVV, se utiliza para hacer un dump de la base de datos, lo cual viene muy bien ya que aunque hagas “vagrant destroy” tu máquina y proyecto volverá a estar en el mismo estado cuando vuelvas a hacer “vagrant up”. Además es una forma de hacer backups automáticos muy cómoda. Para instalarlo: 

```
 vagrant plugin install vagrant-triggers
```

**4.** Clonar el proyecto de github en una carpeta local: 

```
 git clone git://github.com/Varying-Vagrant-Vagrants/VVV.git vagrant-local 
```

**5.** Sitúate dentro de la carpeta y ejecuta “vagrant up”: 

```
cd vagrant-local
vagrant up
```

**6.** Y ahora puedes acceder a cualquiera de las siguientes urls: 

* http://local.wordpress.dev/: Contiene una copia estable de WordPress.

* http://local.wordpress-trunk.dev/: Contiene una copia de WordPress trunk.

* http://src.wordpress.develop.dev/: Contiene una copia de WordPress trunk con archivos de desarrollo.

* http://build.wordpress-develop.dev/: Contiene una version de esos archivos de desarrollo empaquetados con Grunt.

* http://vvv.dev/: Un panel para acceder a varias herramientas útiles: 

    * Un enlace al repositorio en github de VVV
    * Un enlace a un phpMyAdmin
    * Un enlace a un phpMemcachedAdmin
    * Un enlace para ver ver el estado de Opcache
    * Un enlace a Mailcatcher
    * Un enlace a Webgrind
    * Un enlace a un phpinfo
    * Y los enlaces a cada versión de WordPress instalada.

Si no te gusta el panel por defecto, puedes implementar tu mismo uno o descargar alguno que ya está disponible en github. Uno de los más recomendados es el de topdown: VVV-Dashboard.

Para acceder a cualquiera de los backends de wordpress, las credenciales son admin/password. Para acceder a la base de datos, las credenciales son root/root. Para acceder por SSH a vuestra máquina virtual, como siempre “vagrant ssh”.

Además, esto es todo lo que se instala para tu uso:

* Ubuntu 14.04 LTS.
* WordPress Develop.
* WordPress Stable.
* WP-CLI: Algún día caerá un artículo sobre esta herramienta que es muy útil y deberías conocer si desarrollas con WordPress.
* nginx.
* mysql 5.5.x
* php-fpm 5.5.x
* memcached
* Extensión memcache php.
* Extensión xdebug php.
* Extensión imagick php.
* PHPUnit.
* ack-grep.
* git.
* subversion: Sigue siendo el control de versiones de wordpress.
* ngrep.
* dos2unix.
* composer.
* phpMemcachedAdmin: Para monitorear y hacer debug de memcached.
* phpMyAdmin.
* Opcache Status.
* Webgrind.
* nodejs.
* grunt-cli.
* Mailcatcher

**¿Cómo añadir un sitio nuevo?**

Hay dos maneras de hacerlo. La primera es sencilla y la segunda es muy sencilla… La primera te la explico a continuación:

**1.** Vete a la carpeta vagrant-local/config/nginx-config/sites/ y copia el archivo local-nginx-example.conf-sample en esa misma carpeta llamando a esa copia mi-dominio.com.conf.

**2.** Abre ese archivo y edita lo siguiente:
    * server_name: Debe ser el nombre de tu servidor por ejemplo mi-dominio.com.
    * root: Debe ser donde vas a poner tus archivos. Por ejemplo vagrant-local/www/mi-dominio.

**3.** En tu carpeta mi-dominio, crea un archivo llamado vvv-hosts y ahí debes escribir el nombre de sitio mi-dominio.com.

**4.** Después, por línea de comandos, estando en tu directorio vagrant-local, ejecuta “vagrant provision” y espera.

**5.** Una vez terminado, ya puedes acceder a tu sitio.

**¿Y si quiero base de datos nueva e instalar wordpress desde cero?**

**1.** Copiar el archivo vagrant-local/database/init-custom.sql.sample y renombrarlo a init-custom.sql.

**2.** En ese archivo nuevo, configura los datos de tu base de datos, con el nombre, usuario y contraseña que quieres utilizar.

**3.** “vagrant provision”.

La base de datos está creada, ya puedes instalar wordpress utilizándola accediendo a tu nuevo sitio e instalando wordpress como siempre.

**¿Y si quiero reutilizar un dump de una base de datos de wordpress que ya tengo?**

**1.** Copia el dump sql en vagrant-local/database/backups/nombre-base-de-datos-nueva.sql. Es importante que el nombre del archivo sea el nombre de la base de datos nueva que quieres crear.

**2.** “vagrant provision”.

La segunda manera es todavía más sencilla y para eso debemos instalar Variable VVV que es una especie de instalador mágico (wizard). 

Si tienes un Mac y homebrew, la instalación es tan sencilla como:

```
 brew install bradp/vv/vv 
```

Sino puedes clonar el proyecto de github donde quieras y seguir las instrucciones de instalación.

Para crear un nuevo sitio:

```
 vv create 
```

Responderás a unas preguntas relativas a tu nuevo sitio y este script hará lo siguiente:

* “vagrant halt”.
* Crear un directorio para el sitio en www con 3 archivos: 
   * vvv-init.sh: Le dice a Vagrant que debe crear una base de datos si no existe e instalar la última versión de WordPress con WP-CLI.
   * wp-cli.yml: Le dice a WP-CLI que WordPress debe estar en la carpeta htdocs.
   * vvv-hosts: Contiene la entrada del archivo hosts con el dominio configurador en el wizard.
* Crea un archivo en nginx-config para la configuración de tu sitio nuevo.
* Reinicia vagrant con “vagrant up -—provision” 

De nuevo, tu sitio está listo para ser utilizado.

# Deploy avanzado y diferenciación de ambientes

----------

## Diferencias entre ambiente local, de pruebas y de producción

Parte del dimensionamiento de un proyecto de software de gestión se basa en la elección adecuada de los necesarios **ambientes o entornos de desarrollo**, así como de las metodologías de desarrollo usadas.

En cuanto a la elección de los ambientes de desarrollo, podemos diferenciar tres:

### Ambiente local

Para quienes son profesionales en pruebas de softwares, acostumbrados a trabajar en la automatización de ellas, seguramente tiene en su máquina la base de códigos de la aplicación o software ejecutado localmente, de forma independiente.

Los beneficios de usar el ambiente local es una garantía de que el código de la aplicación esté siempre actualizado, de manera que cuando el desarrollador envía algún cambio, pueda ser aplicada y garantizar que se sigue trabajando en la versión más reciente.

El ambiente local de pruebas permite también mayor movilidad y variedad de test. Por ejemplo, se puede corregir errores o la configuración o administración del ambiente. Y si decides cambiar o alterar lo que deseas (como las configuraciones y el banco de datos del software para ver el resultado), puedes hacerlo sin tener que preocuparte con otros tests, puesto que solo estarás usando el ambiente de prueba local o aislado.

### Ambiente de pruebas

En cuanto al ambiente de prueba integrado, administrado centralmente y conectado a otros sistema, es ideal para verificar como tu aplicación se comporta en un ambiente dedicado, plataformas utilizadas por otras aplicaciones o softwares, y ampliar tu análisis a influencias externas, que no aparecerían en el ambiente de ensayos local.

La prueba integrada permite un entendimiento más profundo de la aplicación al utilizar una dirección externa de IP en lugar del localhost, permitiendo ampliar el mapeo del comportamiento a problemas de conectividad, por ejemplo. Además, tendrás un retrato más fiel del funcionamiento del software, que será probado en el ambiente real, y no limitándote al funcionamiento de la máquina del desarrollador.

### Ambiente de producción

Una vez que se hayan resuelto todos los problemas hallados durante la actualización de prueba, ya estará listo para empezar la actualización completa en el entorno de producción. En el plan de actualización encontrará información sobre cada uno de los pasos de la actualización completa.

El entorno de producción es el imprescindible por una cuestión obvia, es aquel donde se realizan todas las transacciones reales de la organización. El resto de entornos cobran mayor relevancia cuanto más desarrollo requiere el proyecto o cuanto más exigente o necesidad de garantías requiere.

----------

## Ventajas y desventajas del uso de FTP

Usar un servidor FTP ofrece **ventajas** propias. Por un lado, un usuario puede utilizar un programa para realizar una carga masiva a un servidor, sin tener que preocuparse por volver a examinar archivos y cargarlos nuevamente usando un formulario. Las descargas también pueden realizarse en masa. Asimismo:

* Son muy rápidos y directos en el acceso de conexión.
* Nos permiten renaudar la descarga previa de archivos.
* Permite transferir archivos de un sistema operativo a otro, transferir archivos en una sola conexión y transferir archivos en ambos sentidos. 

Por desgracia, un servidor FTP todavía requiere un cliente FTP para ser usado, y el uso de uno podría ser particularmente incómodo para aquellos que solo desean cargar una imagen o dos. Otras desventajas pueden ser:

* Es una tecnología vieja y aunque ha tenido ciertos avances a través del tiempo es insuficiente para negocios que requieren seguridad y auditorias.
* La mayoría de los servidores FTP tienen muy pocas capacidades para encriptar los archivos de principio a fin.
* Dado que el puerto FTP es tradicionalmente inestable, es necesario monitorear que las transmisiones se completan con éxito, esto lo hace muy ineficiente especialmente si los archivos son de gran tamaño.

----------

## Herramientas de deployment: Capistrano y deploy.io

### Capistrano
<div align="center">
![](http://i.imgur.com/WlImask.png)
</div>
<br>
Capistrano es un framework basado en **Ruby** que permite ejecutar procesos entre servidores de manera automática por medio de **SSH**. Hoy en día es la **herramienta más utilizada por los desarrolladores de WordPress** que optan por sistemas de **deployment automatizados**. Hay otras opciones muy buenas, algunas de ellas pagas, pero mi nuestra recomendación es usar esta, ya que es la más documentada tanto de manera oficial como por la gran cantidad de desarrolladores de primera línea que la usan.

Para poder instalar Capistrano, necesitás tener instalado previamente **Ruby Gems** en los equipos desde los cuales lo vayas a usar. Te recomendamos que lo uses sólo en sistemas con Linux (cualquier versión más o menos reciente de Ubuntu o CentOS debería bastarte), o a lo sumo en un OSX. En Windows también debería funcionar bien, pero los sistemas basados en Unix son el estándar sobre el que trabaja la mayoría de los desarrolladores profesionales, y casi toda la documentación no oficial está basada en ellos. 

Si ya tenés Windows y cambiar de sistema operativo es un problema, hay alternativas de virtualización muy sencillas de instalar y usar, de entre las cuales preferimos **Vagrant**. Es muy recomendable también, si mantenés un control de versiones de tu sitio (y si no lo hacés, deberías), tener instalado el programa que uses (Git, Mercurial, SVN, etc.) en todos los equipos involucrados en el proceso (tu máquina local y tus servidores de prueba y producción).

<div align="center">
![](http://i.imgur.com/yXs6FDo.png)
</div>
<br>
Es importante notar que Capistrano asume que trabajás de una manera determinada, es decir que previamente ya adoptaste ciertas técnicas que sus desarrolladores consideran buenas prácticas, como acceder remotamente a todos los equipos por medio de SSH, y que para llevar a cabo este acceso usás **llaves públicas** que ya están cargadas en los mencionados servidores.

Por sí mismo, Capistrano no funciona *out of the box* para hacer un *deploy*, sino que requiere que creemos y modifiquemos una serie de archivos en pos de lograr que nuestro sitio se suba automáticamente a un servidor. **No es estrictamente necesario saber algo de programación en Ruby, pero si se sabe algo, va a aportar muchísimo**.

Sin embargo, si uno prefiere no hacer todo el trabajo desde cero, existen varias herramientas que, básicamente, consisten en librerías de preconfiguraciones orientadas al deployment de sitios basados en WordPress.

----------

## Cómo preparar una instalación de WordPress para procesos de deployment avanzados

<div align="center">
![](http://i.imgur.com/GaqWq0O.png)
</div>
<br>
Una de las librerías más populares basadas en **Capistrano** y orientadas puntualmente a *deployment* de sitios web hechos con WordPress es **WP-Stack**, obra de Mark Jaquith, uno de los *lead developers* del core de WordPress. 

**WP-Stack utiliza Capistrano para realizar deploys** a ambientes de producción y/o testing, y funciona perfectamente haciendo una **mínima personalización de archivos** que no requiere ningún conocimiento específico de Ruby. También provee una serie de *MU plugins* capaces de mejorar tu trabajo y el rendimiento de tu sitio significativamente.

De todas maneras, hay tres limitaciones en WP-Stack que creo que vas a necesitar tener en cuenta:

1. **No está pensado para hacer deploys desde tu equipo local**: Es necesario que instales **WP-Stack** en tu servidor de producción y ejecutes tus *deployments* desde ahí. No debería ser un gran problema, pero siempre será preferible hacer el deploy desde el mismo sistema en el que se esté trabajando.
2. **No provee un método sencillo para sincronizar bases de datos y archivos estáticos entre diferentes equipos**: Es posible traer archivos y base de datos desde producción hacia *staging*, pero no desde *staging* o producción al equipo local.
3. **Tiene poca documentación, especialmente sobre su configuración**: No es exactamente algo de lo que debamos quejarnos, **WP-Stack** fue desarrollado inicialmente sólo para satisfacer necesidades individuales. Algunos desarrolladores que trabajan en su propio *fork* de Github hicieron intentos de mejorar la documentación, así que si te sentís un poco confundido por la original, quizás prefieras echarle un vistazo a alguno de esos forks.

<div align="center">
![](http://i.imgur.com/4V9QuJz.jpg)
</div>
<br>
Si tenés algún conocimiento previo de Ruby y necesitás más funcionalidades, podés empezar a aprender cómo funciona Capistrano y escribir tus propias tareas. 

Otra opción que podrías considerar es **Stage WP**. *Stage* está pensado para **trabajar localmente**, y se le puede añadir cualquier stage adicional que haga falta. 

Tanto WP-Stack como Stage WP pueden ser utilizados con **cualquier tipo de instalación de WordPress**, aunque su rendimiento mejora notablemente si se los usa en conjunto con una instalación de WordPress basada en un repositorio de **Git**, como es el caso de **WordPress Skeleton para WP-Stack** y **WordPress Bareboner para Stage WP**. Estos proyectos proveen una manera alternativa de instalar WordPress bien organizada.

Claro que **hay muchas otras librerías basadas en Capistrano para deployment de WordPress**, tal como **Bedrock**, la cual también parece ser una muy buena solución.

# PHP Avanzado

---------

## Debugging: xDebug y métodos tradicionales

Muchos desarrolladores, aun con amplia experiencia en el lenguaje, se limitan a hacer depuración con las típicas funciones de PHP como var_dump(), print_r() o simples sentencias echo, con las que volcar el contenido de las variables en la salida de la página, hacia el navegador.

Si bien este método te pueda sacar de apuros llega un momento que se queda corto. Los que más echan en falta los debuggers complejos son los desarrolladores que vienen de otros lenguajes de programación, donde las esas herramientas son más tradicionalmente usadas. Si te acostumbras a usarlas, seguro que te pasará lo mismo.

### xDebug

xDebug es una extensión de **PHP** para hacer *debug* con herramientas de depuración tradicionales, desde el editor, tal como se hace en lenguajes de programación clásicos.

Podemos encontrar esta extensión en su sitio web: http://xdebug.org/ pero lamentablemente si entras verás que en **xDebug** no son especialistas en la comunicación y aunque se trate de una gran herramienta uno se siente perdido para saber por dónde comenzar a usarla.

<div align="center">
![](http://i.imgur.com/21fk9Y9.png)
</div>
<br>
Xdebug te permitirá no solo analizar el contenido de las variables, sino también realizar el seguimiento del flujo de ejecución, para saber **qué es lo que realmente está ocurriendo cuando algo no funciona como se esperaba**.

Tienes disponible información sobre todas las variables creadas en un momento dado de la ejecución de tu aplicación y puedes ver su contenido de manera expandida. Te ofrece también la posibilidad de marcar puntos de ruptura de tu código, donde el flujo del programa se detendrá para que puedas ver el estado de tus variables, o la traza de ejecución. Si lo deseas puedes ser tan minucioso como acompañar la ejecución del programa línea a línea.
<br>
### Instalar Xdebug

Es posible que tu instalación de PHP ya tenga xDebug instalado. Lo mejor es comprobarlo primero ejecutando el típico **phpinfo()**. Entre la salida de esta función podrás encontrar el dato que necesitas, pero además puedes pedir que un asistente te lo revise automáticamente.

Entrando en la página **http://xdebug.org/wizard.php** encontrarás un sistema que te ofrece instrucciones a medida para instalar xDebug, focalizadas en tu sistema operativo y la configuración de PHP. Podrás pegar en un textarea el contenido completo devuelto por la función phpinfo() y que la web te informe **si está o no instalado y qué acciones tienes que realizar para actualizarlo o bien para instalarlo desde cero**.

Pero sin duda lo mejor es hacerse con algún entorno donde ya tengas xDebug configurado, pues te ahorrará el tener que meterle mano a tu sistema.

Por ejemplo el paquete "Mamp" para Mac (también tiene versión para Windows) tiene una opción para activar xDebug.

Si instalas **tu entorno de desarrollo con Vagrant y PuPHPet** podrás activar tu mismo la configuración de xDebug en la máquina virtual que uses como servidor de desarrollo.

----------

## Diferencias entre versiones de PHP: nuevas características y fallbacks

<div align="center">
![](http://i.imgur.com/2gbswjC.jpg)
</div>
<br>
### PHP 3

Una de las mejores características de PHP 3.0 era su gran **extensibilidad**. Cuando hablamos de extensibilidad nos referimos a la **existencia de un núcleo o core al que se le pueden ir añadiendo fragmentos de código** que permiten hacer más cosas. Se trataría de algo parecido a tener una base sobre la que podemos ir añadiendo piezas.

Además de facilitar el trabajo con muchas bases de datos, protocolos y sistemas informáticos, las características de extensibilidad de PHP 3.0 atrajeron a muchos programadores a unirse al proyecto y enviar nuevos módulos de extensión. Esta fue la clave del enorme éxito de PHP 3.0: **fue un proyecto abierto al que se unieron muchos programadores formando una comunidad**.
<br>
### PHP 4

**La principal novedad de PHP 4 está en el intérprete del lenguaje PHP**. Con la nueva versión se lograron aumentos de entre 5 y 10 veces en la velocidad de ejecución de páginas PHP respecto a la versión anterior, lo cual fue un salto muy importante.

Además del aumento en rendimiento, se introdujeron novedades relacionadas con el **soporte de objetos** en PHP. La programación orientada a objetos es una forma avanzada de programación y gracias a estas características PHP se encuentra entre los lenguajes de vanguardia.

**Otra nueva característica de PHP 4 fue que podía soportar hilos**, cosa que en PHP 3 no era posible. 

Para los programadores, **PHP 4 incluía un depurador**. Un depurador viene siendo un asistente que nos ayuda a detectar dónde se encuentran los errores en los programas o código de desarrollo. Esto ayuda mucho al programador y le evita quebraderos de cabeza por no poder localizar errores. Gracias al depurador, se podían seguir con mayor profundidad esos errores “misteriosos” que hacían perder mucho tiempo.
<br>
### PHP 5

La principal característica de PHP 5 con respecto a PHP 4 es la **mejora en la programación orientada a objetos (POO)**. Aunque este tipo de programación ya se había introducido en el lenguaje desde PHP 3, las versiones anteriores a PHP 5 tenían bastantes limitaciones para el uso de este tipo de programación.

El principal objetivo de PHP 5 ha sido mejorar los mecanismos de programación orientada a objetos para solucionar las carencias de las anteriores versiones. El resultado ha sido un lenguaje **más potente, que cada vez es más popular y que cada vez se usa más, tanto a nivel profesional en empresas como a nivel de usuarios** que quieren construir pequeñas páginas o portales web.

Algunas personas dicen que PHP se parece cada vez más a Java, que es quizás el lenguaje de programación más usado en el mundo, y que también es un lenguaje de programación orientado a objetos.
<br>
### PHP 7

El pasado jueves 3 de diciembre (2015) fue liberada la versión estable 7.0.0 del popular lenguaje de programación, que intenta ser un salto de calidad y que viene con interesantes novedades con respecto a sus predecesores.

<div align="center">
![](http://i.imgur.com/XMyyXx8.png)
</div>
<br>
Estas novedades y mejoras se afirma desde el equipo de core del proyecto que consiguen un rendimiento en PHP 7 que es el **doble de rápido que PHP 5.6** y entre ellas destacar:

* Nueva versión de Zend Engine (de nombre en clave PHP#NG).
* Importante reducción del uso de memoria.
* Árbol sintáctico abstracto.
* Soporte consistente a los 64-bits.
* Un buen número de errores fatales convertidos en excepciones.
* Mejora en la jerarquía de estas excepciones.
* Generador de números aleatorio más seguro.
* Operador de coalescencia con *null*.
* Clases anónimas.
* Compatibilidad con la recién salida última versión de OpenSSL (1.0.2e).

Además se han eliminado todas las extensiones y funcionalidades viejas y a las que ya no se daba soporte y se han arreglado decenas de bugs, como se puede comprobar en un *changelog* que es realmente impresionante en cuanto a volumen y longitud.

----------

## Uso de autoloaders y aplicación en plugins y themes

Imaginemos que organizamos nuestro código en diferentes archivos, cada uno con una clase y con el nombre de la clase como nombre del archivo. Por ejemplo, **UnaClase.php** y **OtraClase.php** contendrán las definiciones de *class UnaClase* y *class OtraClase* repectivamente. Para inicializar estas clases desde otro script podríamos hacer algo así:

```
// incluir los archivos que contienen las clases
include_once 'UnaClase.php';
include_once 'OtraClase.php';

// inicializar los objetos
$objeto1 = new UnaClase();
$objeto2 = new OtraClase();
```

Todo perfecto. Sigues trabajando en la aplicación y vas definiendo nuevas clases en cada vez más archivos que tienes acordarte de incluir en tu script:

```
// incluir los archivos que contienen las clases
include_once 'UnaClase.php';
include_once 'OtraClase.php';
include_once 'MasClase.php';
include_once 'CuartaClase.php';
include_once  'YtodaviamasClase.php';

// inicializar los objetos
$objeto1 = new UnaClase();
$objeto2 = new OtraClase();
$objeto3 = new MasClase();
//....
```

No hay nada de malo en el código anterior, funcionará perfectamente. Asegúrate de incluir todos los archivos y no tendrás ningún problema. Pero, ¿y si no siempre los necesito todos?. Da igual, cárgalos todos por si acaso o haz condicionales para su carga. ¿Y si tengo 50 archivos? Pues tendrás que hacer 50 *include*. Vaya, qué incomodidad; y si se me olvida algún archivo…. Fatal ERROR.
<br>
### Autoloaders

En lugar de tener que cargar (en inglés *load*) cada una de las clases de nuestra aplicación manualmente, podemos registrar un **autoloader**. Como su nombre indica, el autoloader está destinado a **cargar de forma automática las clases utilizadas**. Cada vez que se intenta inicializar una clase y la clase no existe, el nombre de esta clase se pasa al autoloader y este es ejecutado. En el autoloader podremos automatizar el proceso de carga sin tener que incluir manualmente cada archivo y además nos permite hacer el código más rápido, pues sólo se cargarán las clases que efectivamente se utilicen.

Para registrar un autoloader se utiliza la función **spl_autoload_register()** pasando como primer parámetro la función que hará de autoloader:

```
//Registramos mi_autoloader() como callback que se ejecutará
//al intentar inicializar una clase que no existe
spl_autoload_register('mi_autoloader');

//implementamos mi_autoloader(), que recibe el nombre de la clase
//que se ha intentado inicializar
function mi_autoloader( $NombreClase ) {
    include_once $NombreClase . '.php';
}
```

Desde PHP 5.3.0 podemos utilizar funciones anónimas:

```
spl_autoload_register( funcion( $NombreClase ) {
    include_once $NombreClase . '.php';
} );

// inicializar los objetos
$objeto1 = new UnaClase();
$objeto2 = new OtraClase();

if( $alguna_condicion ) {
    // Ganamos en performance. MasClase.php solo se cargará
    // si se verifica $alguna_condicion y llegamos hasta este punto
    $objeto3 = new MasClase();
}
```

Cómo ves, es necesario conocer los nombres de los archivos que se necesitan cargar. Por eso es muy imporante seguir unas **reglas fijas de nomenclatura de clases y archivos**. El ejemplo anterior de utilizar una sóla clase por cada archivo y dar al archivo el mismo nombre que la clase sería una regla aceptable. Puedes seguir tus propias reglas aunque es recomendable utilizar reglas estandarizadas y de amplio uso, por ejemplo el **estándar PSR-4**.
<br>
### Olvídate de __autoload

Si no defines tu propio autoloader como hemos hecho anteriormente, puedes utilizar el **método mágico __autoload()**:

```
function __autoload( $nombreclass ) {
    //incluir el archivo de la clase
    include_one $nombreclass . '.php';
}

// inicializar los objetos
$objeto1 = new UnaClase();
$objeto2 = new OtraClase();
```

Se puede decir que *_ _autoload* sería el autoloader por defecto. Sin embargo, tal y como se menciona en la documentación de PHP sobre autocarga de clases, se recomienda **spl_autoload_register** por su mayor flexibilidad, pero además advierte que *_ _autoload* **puede ser marcado como obsoleto o eliminarse en el futuro**, así que olvídate ya de este método y utiliza siempre **spl_autoload_register**.
