# Programación Orientada a Objetos

---------

## Introducción a la creación de objetos y métodos

Se puede decir que la  POO es una técnica de programación que utiliza objetos e interacciones en el diseño de un sistema.  Un objeto es una entidad independiente con sus propios datos y programación. Las ventanas, menúes, carpetas de archivos pueden ser identificados como objetos; el motor de un auto también es considerado un objeto, en este caso, sus datos (atributos) describen sus características físicas y su programación (métodos) describen el funcionamiento interno y su interrelación con otras partes del automóvil (también objetos). Esta se compone ademas de los objetos, de clases, métodos, eventos, propiedades y atributos. Tiene como características principales la abstracción, encapsulamiento, modularidad, ocultación, polimorfismo y herencia.

### Clases en PHP

Éstas pueden ser:

* Instanciables (también pueden ser heredadas):

```
<?php
class MiClaseInstanciable {
}
?>
```

* Abstractas (no instanciables pero sí heredables):

```
<?php
abstract class MiClaseAbstracta {
}
?>
```

* Finales (instanciables no heredables):

```
<?php
final class MiClaseFinal {
}
?>
```

Heredadas (instanciables heredables, que a la vez heredan de una clase “madre”):

```
<?php
class MiClaseHija extends MiClaseMadre {
}
?>
```

### Propiedades de clases en PHP 

Éstas pueden ser:

* Públicas (accesibles desde cualquier parte de la aplicación):

```
<?php
class Ejemplo {
	public $propiedad = 'Valor';
}
?>
```

* Protegidas (solo accesibles desde la clase que las declara y clases que la heredan):

```
<?php
class Ejemplo {
	protected $propiedad = 'Valor';
}
?>
```

* Privadas (solo accesibles desde la clase que las declara):

```
<?php
class Ejemplo {
	private $propiedad = 'Valor';
}
?>
```

* Estáticas (aquellas que no pueden ser modificadas):

```
<?php
class Ejemplo {
	static $propiedad = 'Valor';
}
?>
```

Estas propiedades estáticas pueden tener cualquier visibilidad: pública, privada o protegida:

```
<?php
class Ejemplo {
	// no se modifica.
	// Se accede desde cualquier parte de la aplicación
	public static $public_static = 'Valor';

	// no se modifica.
	// Se accede desde la clase que la declara
     // y desde las que la heredan
	protected static $protected_static = 'Valor';

	// no se modifica.
	// Se accede desde la clase que la declara
	private static $private_static = 'Valor';
}
?>
```

### Constantes de Clase

Tienen visibilidad pública (como las propiedades públicas) y no pueden modificarse (al igual que las propiedades estáticas).

```
<?php
class Ejemplo {
	const MI_CONSTANTE = 'Valor';
}
?>
```

### Métodos de clase en PHP

Éstos, pueden, al igual que las propiedades, ser:

* Dinámicos : aquellos que efectúan cambios a la clase y/o acceden dinámicamente a sus propiedades.

* Estáticos : no efectúan cambios de ningún tipo y su función, en consecuencia, es estática.

* Públicos, privados o protegidos : donde aplica la misma visibilidad descrita en las propiedades.

```
<?php
class Ejemplo {

	public $propiedad = 'valor';
	static $variable = 'valor de variable';

	public function metodo_publico($valor) {
		$this->propiedad = $valor;
		// modifica el valor de la propiedad
	}

	static function metodo_estatico() {
		return 45;
		// siempre retorna el número 45
	}

	private function metodo_privado() {
		// a este método solo se puede acceder desde esta clase
	}
}
?>
```

### Creación de Objetos en PHP y llamada a las propiedades y métodos de una Clase

Crear un objeto para luego acceder a sus propiedades y métodos:

```
$nuevo_objeto = new NombreDeLaClase();
```

Acceder a las propiedades públicas NO estáticas:

```
$nuevo_objeto->propiedad_publica;
```

Acceder a los métodos públicos NO estáticos:

```
$nuevo_objeto->metodo_publico();
```

Acceder a propiedades estáticas públicas (no debe instanciarse el objeto):

```
NombreDeLaClase::$propiedad_publica_estatica;

NombreDeLaClase::CONSTANTE_DE_CLASE;
```

Acceder a métodos públicos y estáticos (no debe instanciarse el objeto):

```
NombreDeLaClase::metodo_publico_estatico();
```

#### Referencia a propiedades y métodos dentro de la misma clase:

```
<?php
class Ejemplo {
	static $propiedad_estatica = 1;
	const CONSTANTE = 15;
	public $propiedad_dinamica = 0;

	static function metodo_estatico() {
		return 45;
	}

	function metodo_dinamico() {
		// aceder a una propiedad dinámica y modificar su valor
		$this->propiedad_dinamica = 100;

		// acceder a un método estático
		$numero = self::metodo_estatico();

		// acceder a una propiedad estática o constante
		print self::$propiedad_estatica;
		print self::CONSTANTE;
	}

	function otro_metodo() {
		// acceder a un método dinámico
		$this->metodo_dinamico();
	}
}
?>
```

#### Referencia a propiedades y métodos dentro de una clase hija (heredada):

```
<?php
class ClaseHija extends Ejemplo {

	function metodo_hijo() {
		// aceder a una propiedad dinámica de la clase madre y modificar su valor
		$this->propiedad_dinamica = 100;

		// acceder a un método estático de la clase madre
		$numero = parent::metodo_estatico();

		// acceder a una propiedad estática o constante de la clase madre
		print parent::$propiedad_estatica;
		print parent::CONSTANTE;

		// acceder a un método dinámico de la clase madre
		$this->metodo_dinamico();
	}
}
?>
```

---------------

## Inicialización y uso de objetos mediante hooks

Existe diferentes formas de instanciar y usar objetos mediante hooks, que van desde Poner los hooks en el método __construct  de la clase, colocarlos todos en un método de clase por separado, hasta ponerlos fuera de la clase.

Poner los hooks en un método separado es una gran forma de utilizarlos en wordpress. Sólo tienes que reorganizar tu código un poco. Tienes que tener cuidado ya que  hay una manera equivocada para conseguirlo. Eso es lo que vamos a ver en primer lugar.

```
class MyPlugin
{
    public function __construct()
    {
        $this->init();
    }
 
    public function init()
    {
        add_action('wp_loaded', array($this, 'on_loaded'));
    }
 
    public function on_loaded()
    {
        // ...
    }
}
```

Se puede ver que el método  init  con todos nuestros hooks. Esa parte esta bien. Sin embargo el problema es que todavía se está llamando desde el constructor de la clase. Esto no ha resuelto el problema de acoplamiento en absoluto.

```
class MyPlugin
{
    public function init()
    {
        add_action('wp_loaded', array($this, 'on_loaded'));
    }
 
    public function on_loaded()
    {
        // ...
    }
}
 
$my_plugin = new MyPlugin();
$my_plugin->init();
```

Puede solucionar este problema mediante la eliminación del método init  del __construct. En lugar de llamarlo allí, se llama fuera de su clase. El resultado es el mismo, pero se retira el acoplamiento de su clase con el fin de adaptarse mejor a WordPress.



---------------

## JavaScript modular

En el mundo de javascript existe mucha facilidad para desarrollar con este lenguaje, el único detalle es que no debemos abusar de esa facilidad que nos ofrece. Al final uno obtiene variables y funciones repartidas a lo largo de todo el código y sin un orden, lo que termina siendo inmantenible. Una de las mejores practicas a la hora de escribir código es usar patrones de diseño y en esta sección  explicaremos el patron modular para Javascript.

En javascript el patrón modular emula el concepto de clases, de manera que somos capaces de incluir métodos públicos/privados y propiedades dentro de un único objeto, protegiendo las datos particulares del ámbito global, lo que ayuda a evitar la colisión de nombres de funciones y variables ya definidas a lo largo de nuestro proyecto, o API’s de terceros, a continuación unos conceptos previos para poder entender mejor el patrón modular.

EL patrón modular se basa en parte en los objetos literales por ende es importante entenderlo.
Un objeto literal es descrito como cero o más pares nombre/valor, separados por comas entre llaves. Los nombres dentro del objeto pueden ser cadenas o identificadores que son seguidas por 2 puntos, dichos objetos también pueden contener otros objetos y funciones.

```
var objetoLiteral = {
    /* los objetos literales pueden contener propiedades y métodos */
    saludo : "soy un objeto literal",
    miFuncion : function(){
      // código
    }
};
/* accediendo a una propiedad de nuestro objeto literal persona */
objetoLiteral.saludo
```

Un ejemplo de un módulo usando un objeto literal.


```
var persona = {
    /* definiendo propiedades */
    nombre : "adan",
    edad   : 33,
    /* método simple */
    comer  : function(){
        console.log(this.nombre + " esta comiendo.");
    }
};
/* accediendo al método comer de nuestro objeto literal persona */
persona.comer();
```

#### Módulo

Un módulo es una unidad independiente funcional que forma parte de la estructura de una aplicación. Podemos usar funciones y closures(cierres) para crear módulos.

```
var modulo = (function(){
    //- - -
});
```

Un ejemplo más completo:

```
var automovil = (function(colorDeAuto){
    var color = colorDeAuto;
    return{
        avanzar : function(){
            console.log("el auto "+ color +" esta avanzando");
        },

        retroceder : function(){
            console.log("el auto "+ color +" esta retrocediendo");
        }
    }
})("azul");
/* accediendo los metodos retroceder y avanzar de nuestro módulo */
automovil.retroceder();
automovil.avanzar();
```

### Función Anónima

Las funciónes anónimas son funciónes sin nombre, comúnmente asociados a una variable.

```
var miFuncionAnonima = function(){
    alert("Hola mundo!");
};
miFuncionAnonima();
```

### Función Auto-Ejecutables (IIFE)

Estas funciónes una vez declaradas se llaman a sí mismas para inicializarse, los primeros paréntesis encierran el contenido, los segundos paréntesis asumen que el interior de los primeros paréntesis es una función y la ejecuta inmediatamente.

```
/* 01 */
(function(){
    alert("Hola mundo!");
})();

/* 02 */
var miFuncionAnonima = function(){
    alert("Hola mundo!");
};

/* 03 */
var miFuncionAnonima = (function(mensaje){
    alert(mensaje);
})("Hola mundo");
// todo lo que le precede a los 2 últimos paréntesis se ejecuta inmediatamente
```

### Closure

Los clousures son funciones definidas dentro de otras funciones, así mismo dicha función interna tiene acceso al ámbito de la función contenedora.

```
function ejemploClousure(arg1, arg2){
    var localVar = 8;
    function multiplicador(interArg){
        return arg1 * arg2 * interArg * localVar;
    }
    /* retornar una referencia de la función interna definida como:
       multiplicador 
    */
    return multiplicador;
}

/* la función devuelve una función, por lo tanto necesita asignación */
var globalVar = ejemploClousure(2,4);
/* y luego llamar a */
globalVar(8);
```

### Métodos Privados

Los métodos privados son funciones que no pueden ser llamados desde fuera del ámbito donde han sido declarados, dichos métodos podrán ser invocados en nuestros métodos públicos.

```
var modulo = (function () {
    var privateMethod = function (message1) {
        console.log(message1);
    };
    var publicMethod = function (mensaje2) {
        privateMethod(mensaje2);
    };
    return {
        publicMethod: publicMethod
    };
})();

/* pasando datos a un método privado */
modulo.publicMethod("mi mensaje");
```

### Entendiendo el Retorno

Comúnmente los módulos retornan un objeto, la cual los métodos ligados a dicho objeto serán accesibles desde fuera del módulo.

```
var module = (function(){
    /* simple método privado */
    var privateMethod = function(){
        console.log("soy un método privado");
    };
  
    /* retornando un objeto literal */
    return{
        publicMethod : function(){
            privateMethod();
            console.log("soy un método publico");
        }
    }
})();
/* accediendo nuestro método publico */
module.publicMethod()
```

### Ventajas del patrón Modular

* Código limpio , separado y organizado.
* Soportan datos privados.
* Código Escalable.

Bien, ahora que ya tenemos los algunos conceptos definidos vamos a realizar un ejercicio la cual sera realizar tabs, con una estructura mas avanzada, vamos a realizarlo con jquery.

### 1. Definimos nuestra estructura HTML


```
        <ul class="tabs">
            <li class="tabs_item">
                <a class="tabs_item_target is_selected" data-tab="tab_one">JavaScript</a>
            </li>
            <li class="tabs_item">
                <a class="tabs_item_target" data-tab="tab_two">Html5</a>
            </li>
            <li class="tabs_item">
                <a class="tabs_item_target" data-tab="tab_three">Css</a>
            </li>
        </ul>
        <div class="tabs_content">
            <div id="tab_one" class="tabs_content_pane is_active">
                contenido JavaScript
            </div>
            <div id="tab_two" class="tabs_content_pane">
                contenido Html5
            </div>
            <div id="tab_three" class="tabs_content_pane">
                contenido Css
            </div>
        </div>
```

### 2. Definimos nuestra estructura javascript 

A continuación se muestra una estructura modular mas avanzada y explicaremos paso a paso las partes que la componen.

```
// Definimos la variable tabs la cual contendrá todo nuestro modulo.
var tabs = (function(){
    // Objeto la cual establecemos valores que vamos a usar mas adelante en este ámbito.
    var st = {
        //- - -
    };
   
    // Objeto vacío que guardará elementos que se manejan por HTML.
    var dom = {}

    // Función que llenará al objeto dom con los objetos HTML a través de jQuery ($).
    var catchDom = function(){
        //- - -
    };

    // Función donde establecemos los eventos que tendrán cada elemento.
    var suscribeEvents = function(){
        //- - -
    };

    /* Objeto que guarda métodos que se van a usar en cada evento definido 
      en la función suscribeEvents. */
    var events = {
        //- - -
    };
 
    // Función que inicializará los funciones decritas anteriormente.
    var initialize = function(){
        //- - -
    };

    /* Retorna un objeto literal con el método init haciendo referencia a la 
       función initialize. */
    return{
        init:initialize
    }
})();

// Ejecutando el método "init" del módulo tabs.
tabs.init();
```

### 3. Creación de nuestro Modulo

Finalmente luego de haber entendido nuestra estructura anterior, procedemos a la creación de nuestro modulo tabs.


```
var tabs = (function() {
  var st = {
    tabs: '.tabs',
    tabs_item: '.tabs_item',
    tabs_item_target: '.tabs_item_target'
  };

  var dom = {}

  var catchDom = function() {
    dom.tabs_item_target = $(st.tabs_item_target, st.tabs);
  };

  var suscribeEvents = function() {
    dom.tabs_item_target.on('click', events.eSelectedTab);
  };

  var events = {
    eSelectedTab: function(e) {
      self = $(this);
      self.addClass('is_selected');
      var brother = self.parent(st.tabs_item).siblings();
      $(st.tabs_item_target, brother).removeClass('is_selected');
      var pane = self.attr('data-tab');
      $("#" + pane).addClass('is_active');
      $("#" + pane).siblings().removeClass('is_active');
    }
  };

  var initialize = function() {
    catchDom();
    suscribeEvents();
  };

  return {
    init: initialize
  }
})();

tabs.init();
```

# Manejo de Dependencias

------------

## Manejo de librerías estáticas (CSS y JavaScript)

### Librerías CSS

Para quienes desean trabajar en **CSS3** y aprovechar todos sus recursos, fáciles de implementar, se enfrentarán con su gran limitante: la compatibilidad con los distintos navegadores y sus respectivas versiones, donde vemos que existen funciones que solo ciertos navegadores pueden aprovechar.

Como solución a ésto, los programadores han desarrollando diferentes **herramientas web y librerías** que nos ayudan a resolver estos problemas y así aprovechar todas las ventajas de CSS3 de una forma más simple y no tener la excusa de no trabajar con las nuevas tecnologías.

#### Bootstrap

Twitter Bootstrap es un framework o conjunto de herramientas de código abierto para diseño de sitios y aplicaciones web. Contiene plantillas de diseño con **tipografía, formularios, botones, cuadros, menús de navegación** y otros elementos de diseño basado en **HTML y CSS**, así como, extensiones de JavaScript opcionales adicionales.

<div align="center">
![](http://i.imgur.com/NEiV4CE.jpg)
</div>

Bootstrap tiene un soporte relativamente incompleto para HTML5 y CSS 3, pero es **compatible con la mayoría de los navegadores web**. La información básica de compatibilidad de sitios web o aplicaciones está disponible para todos los dispositivos y navegadores. Existe un concepto de compatibilidad parcial que hace disponible la información básica de un sitio web para todos los dispositivos y navegadores. Por ejemplo, las propiedades introducidas en CSS3 para las esquinas redondeadas, gradientes y sombras son usadas por Bootstrap a pesar de la falta de soporte de navegadores antiguos. Esto extiende la funcionalidad de la herramienta, pero no es requerida para su uso.

Desde la versión 2.0 también **soporta diseños sensibles**. Esto significa que el diseño gráfico de la página se **ajusta dinámicamente**, tomando en cuenta las características del dispositivo usado (computadoras, tabletas, teléfonos móviles).

Bootstrap es de **código abierto** y está disponible en **GitHub**. Los desarrolladores están motivados a participar en el proyecto y a hacer sus propias contribuciones a la plataforma.

El siguiente ejemplo ilustra como funciona. El código HTML define un simple formulario de búsqueda y una lista de resultados en un formulario tabular. La página consiste en elementos regulares y semánticos de HTML 5, y alguna información adicional de la clase de CSS de acuerdo con la documentación de Bootstrap.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Ejemplo de Twitter Bootstrap</title>
    <!-- Se incluyen las hojas de estilo de bootstrap -->

   <link href="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.no-icons.min.css" rel="stylesheet">

  </head>
 
  <body>
    <div class="container">
      <h1>Search</h1>
      <label>Ejemplo de un formulario sencillo de búsqueda.</label>

      <!-- Formulario de búsqueda con un campo de entrada (input) y un botón -->
      <form class="well form-search">
        <input type="text" class="input-medium search-query">
        <button type="submit" class="btn btn-primary">Buscar</button>
      </form>
 
      <h2>Results</h2>
 
      <!-- Tabla con celdas de color de fondo alternantes y con marco -->
      <table class="table table-striped table-bordered">
        <thead>
          <tr>
            <th>#</th>
            <th>Título</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1</td>
            <td>Lorem ipsum dolor sit amet</td>
          </tr>
          <tr>
            <td>2</td>
            <td>Consetetur sadipscing elitr</td>
          </tr>
          <tr>
            <td>3</td>
            <td>At vero eos et accusam</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- JavaScript se situa al final del documento para que la página cargue mas rápido -->
    <!-- Optional: Incluir la librería de jQuery -->
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>

    <!-- Optional: Incorporar los plug-ins de JavaScript de Bootstrap -->
    <script src="http://twitter.github.com/bootstrap/assets/js/bootstrap.js"></script>
  </body>
</html>
```


#### Normalize.css

En términos simples **Normalize.css** es un archivo .css que **pone en cero todas las etiquetas HTML**. Todo esto con el fin de que tu sitio web se vea igual en todos los navegadores.

<div align="center">
![](http://i.imgur.com/omW515d.jpg)
</div>

Normalize.css funciona como una **hoja de estilo** más en tu sitio web, que tiene todas las etiquetas básicas de HTML con el valor 0.

Al ser una hoja de estilo, debe ser tratado como tal, osea, con una extensión .css, y linkeado en el "head" de tu sitio web. **No tiene nada que ver con Javascript**, o cualquier otra sintaxis alejada de CSS o HTML. O sea, **no hay que instalar nada, configurar nada, solamente descargar el archivo e introducirlo**.

<div align="center">
![](http://i.imgur.com/aFbjRMS.jpg)
</div>

Hablar de instalar normalize.css está incorrecto, lo que realmente haremos es **introducir el archivo en nuestra web**. Lo cual es muy sencillo. Lo primero que haremos es “descargarnos” normalize.css desde su página oficial (http://necolas.github.io/normalize.css/2.0.1/normalize.css).

Luego de habernos descargado nuestro archivo, lo debemos guardar, y se recomienda (no es obligación) guardarlo con el nombre normalize.css. Lo guardaremos en la carpeta de nuestra web.

Ahora lo que haremos es **linkearlo a nuestro sitio web**, para que normalice nuestra web a todo navegador. Para eso lo linkeamos como una hoja de estilo más que es. Es decir, agregamos el siguente comando entre las etiquetas <head> </head>: 

```
<html>
<head>
   <title></title>
   <link rel="stylesheet" href="style.css" />
   <link rel="stylesheet" href="normalize.css" />
</head>
<body>
```

¡Y listo! Tenemos normalizada nuestra web.

#### Modernizer

<div align="center">
![](http://i.imgur.com/McbNEpe.png)
</div>

Modernizr es una **librería de código abierto** basado en javascript que nos ayuda con la detección del soporte de distintas capacidades de **HTML5 Y CSS3** del navegador. Es soportado por una gran cantidad de navegadores con sus respectivas versiones como lo son Firefox 3.5+ , Explorer 6+ , Opera 9.6+ , Safari 2+ y Chrome. Esta librería es **una de las más importantes** ya que consta con el apoyo y es usada por Twitter, Google, Microsoft entre otros.

**Modo de uso**: Descargar Modernizr, subir los archivos correspondientes a nuestro servidor y enlazarlo de la misma forma que cualquier script dentro de la etiqueta de nuestra página, además se recomienda colocar justo después de mandar a llamar nuestra hoja de estilos.

Para trabajar con Modernizr existen dos formas:

* **La primera sobre el CSS**: Modernizr genera distintas clases en CSS que son asignadas solamente cuando no son soportadas y permiten tener estilo alternativo.
* **La segunda como javascript**: Tiene distintas funciones en las cuales podemos generar scripts que permiten verificar si el navegador tiene la funcionalidad y este devuelve un “true” o un “false”.

```
if (Modernizr.video)
document.write("Si soporte video");
else
document.write("No soporta video");
```

Además vale destacar que se puede descargar de 2 formas diferentes una para developer y otra personalizada con solo las funciones que usaremos en nuestra página web.


#### Prefix-free

<div align="center">
![](http://i.imgur.com/jIZd1aD.jpg)
</div>


CSS3 es muy bonito de eso no hay duda, lo que resulta muy molesto son los prefijos. Existen varios que, si bien no son difíciles de aprender, son tediosos de escribir y quitan tiempo que podría invertirse en otras cosas. 

El uso de la librería no es difícil, sólo la agregamos a la cabecera de nuestro documento:

```
<script src="js/prefixfree.min.js"></script>
```

Y la magia sucede en el archivo CSS, donde:

```
.container {
   width: 960px;
   margin:10px 0;

   -webkit-box-shadow: 20px;
   -moz-box-shadow: 20px;
   box-shadow: 20px;

   -webkit-border-radius: 4px;
   -moz-border-radius: 4px;
   border-radius: 4px;

   background: -webkit-linear-gradient(top, #e3e3e3 10%, white);
   background: -moz-linear-gradient(top, #e3e3e3 10%, white);
   background: -o-linear-gradient(top, #e3e3e3 10%, white);
   background: -ms-linear-gradient(top, #e3e3e3 10%, white);
   background: linear-gradient(top, #e3e3e3 10%, white);
}
```

Se convierte en:

```
.container {
   width: 960px;
   margin:10px 0;
   box-shadow: 20px;
   border-radius: 4px;
   background: linear-gradient(top, #e3e3e3 10%, white);
}
```

**Se suprimieron todos los prefijos** dejando solo el “estándar”. Sí, así de fácil.



### Librerías JavaScript

Este lenguaje de programación se ha convertido en un must dentro del desarrollo, no solo por la parte del **front-end**, que era lo más habitual, también por el lado del **back-end** (marcos de desarrollo, bases de datos y entornos de desarrollo por la capa del servidor). **Sintaxis completa, con enormes oportunidades y de gran salida profesional**. 

Por el lado del front-end, diseñadores de todo el mundo utilizan infinidad de librerías JavaScript para implementar **funcionalidades y características dinámicas** a sus proyectos. Es en este lenguaje donde residen todas las potencionalidades que un desarrollador puede darle, por ejemplo, a una web. Algunas de estas librerías son de código abierto: acceso libre y gratuito. 

A continuación, te presentamos un listado con las librerías (y algunos frameworks) más utilizados por los desarrolladores. 

#### jQuery

jQuery es una librería JavaScript que permite la **manipulación de documentos HTML en el DOM y los estilos CSS, la gestión de eventos, efectos y animaciones, AJAX y una API que facilita mucho la labor de los desarrolladores**, compatible con todos los navegadores de última generación. El diseñador diseña, jQuery se encarga de que ese diseño se sirva sin problemas en todos los escenarios. 

<div align="center">
![](http://i.imgur.com/ScsROHv.png)
</div>

jQuery es una de las librerías más empleadas dentro del mundo del diseño para la creación de **proyectos digitales responsive** (con una variedad específica de jQuery llamada jQuery Mobile), un único diseño para todos los entornos (escritorio, móvil y tableta). Su versatilidad y capacidad para aumentar los recursos con plugins la convierten en una referencia real. Grandes compañías como **Google, Microsoft e IBM la usan en sus productos**.

jQuery tiene algunas otras ventajas que la hacen tan popular:

* **La programación de los estilos es muy similar al tradicional CSS**. Si el diseñador tiene conocimientos en CSS, su adaptación es sencilla.

* **Encadenamiento de enunciados**. Se pueden manipular distintos elementos a la vez con una única línea de código. Simplicidad.

* **Es realmente sencillo añadirla a cualquier página web**. Para incluir dentro del código debemos colocar el **script** dentro del **head** así:

```
<head>
<script src = "jquery-1.11.3.min.js"> </ script>
</ head>
```

Es recomendable colocar el archivo JavaScript descargado en el mismo directorio utilizado para alojar las páginas web donde usemos jQuery.

Dentro de esta librería de front-end existen dos versiones distintas, dependiendo del tipo de uso que se haga de la librería.

* **Versión de producción**: una versión de jQuery disponible para páginas web con el código ya compactado y comprimido.

* **Versión de desarrollo**: para pruebas (código sin comprimir).


#### AngularJS

AngularJS es un framework de JavaScript de **código abierto**, mantenido por Google, que se utiliza para **crear y mantener aplicaciones web de una sola página**. Su objetivo es aumentar las aplicaciones basadas en navegador con capacidad de **Modelo Vista Controlador (MVC)**, en un esfuerzo para hacer que el desarrollo y las pruebas sean más fáciles.

AngularJS está construido en torno a la creencia de que la **programación declarativa** es la que debe utilizarse para generar interfaces de usuario y enlazar componentes de software, mientras que la **programación imperativa** es excelente para expresar la lógica de negocio. Este framework adapta y amplía el HTML tradicional para servir mejor contenido dinámico a través de un data binding bidireccional que permite la sincronización automática de modelos y vistas. Como resultado, AngularJS pone **menos énfasis en la manipulación del DOM y mejora la testeabilidad y el rendimiento**.

<div align="center">
![](http://i.imgur.com/4Z7iLP0.png)
</div>

Los objetivos de diseño:

* Disociar la manipulación del DOM de la lógica de la aplicación. Esto mejora la capacidad de prueba del código.
* Considerar a las pruebas de la aplicación como iguales en importancia a la escritura de la aplicación. La dificultad de las pruebas se ve reducida drásticamente por la forma en que el código está estructurado.
* Disociar el lado del cliente de una aplicación del lado del servidor. Esto permite que el trabajo de desarrollo avance en paralelo, y permite la reutilización de ambos lados.
* Guiar a los desarrolladores a través de todo el proceso del desarrollo de una aplicación: desde el diseño de la interfaz de usuario, a través de la escritura de la lógica del negocio, hasta las pruebas.

#### ReactJS

React.js es una librería de JavaScript lanzada hace más de un año por **Facebook**, normalmente utilizada en el **Frontend** aunque puede ser utilizada en el **Backend**. Facebook la utiliza en producción para su red social en determinadas partes, como los comentarios y también en **Instagram**.

No se trata de un framework JS de cliente como pueden ser Angular, Backbone o Ember, si no de una **librería** que se encarga del renderizado de las vistas de una aplicación web. **Es la V del patrón MVC por así decirlo**.

<div align="center">
![](http://i.imgur.com/5eJO6GQ.jpg)
</div>

Una de las primeras cosas que no gustó mucho al principio de React era que parecía que mezclaba el HTML con código JavaScript, pero cuando lo pruebas ves que no es así, sino que usa un transformador de código JSX (Creado por Facebook) a JavaScript para facilitarnos la vida a la hora de programar, por ejemplo este código de React usando JSX:

```
var Item = React.createClass({  
    render: function() {
        return <div>Hello Item</div>
    }
});
```

Y sin usar la transformación JSX sería así:

```
var Item = React.createClass({  
    render: function() {
        return React.createElement('div', {}, 'Hello Item'); 
    }
});
```

Commo puedes ver, se ve de una forma más clara con JSX lo que estamos renderizando que usando ECMAScript 5/6 puro, y eso que este ejemplo es pequeño, si estamos usando más elementos HTML, con clases, eventos, etc... se puede hacer inmanejable.

Lo que ha hecho que React se vuelva trendy es su **velocidad de renderizado de vistas**. Eso es posible gracias a un **Virtual DOM** que genera React con cada componente que creamos y el algoritmo de Diff que básicamente lo que hace es marcar que elementos dentro de nuestro DOM Virtual tienen cambios para renderizar solo ellos y no tener que revisar y repintar el DOM entero de nuestra página. Dónde más tiempo se pierde en una aplicación web es en el renderizado y pintado del DOM. React evita eso y por eso es tan rápido.

------------

## Optimización de librerías estáticas propias

Si tenemos un blog con el CMS WordPress y queremos que este cargue rápido, tenemos que optimizar todos los codigos estaticos que tengamos básicamente los archivos CSS y javascript para que todos los usuarios que accedan puedan visualizar las publicaciones de manera rápida y sin que el sitio tarde en cargar, ya que muchas personas regresan a los resultados de búsqueda si no carga rápido.

<div align="center">
![](http://i.imgur.com/oSPnCY9.jpg)
</div>

Teniendo el sitio bien optimizado nos permite tener mejores resultados con Google y así hacer que tengamos mas visitas.

Desde hace tiempo que comprimo los archivo Javascrip y CSS de este sitio para que cargue mucho mas rápido cada articulo y eso lo podemos realizar fácilmente con paginas como CSS Drive que nos permite comprimir el CSS eliminando comentarios y optimizando el código, para Javasccript podemos utilizar Javascript compressor o Online Javascript Compression Tool, solo tenemos que buscar los archivos creado por nosotros y listo.

Estos es fácil de realizar si sabemos en que parte se encuentran los archivos CSS y Javascript, pero cuando queramos modificar el código, este no se vera igual que antes, para no tener ese tipo de problema podemos utilizar los siguientes plugins que facilitan todo.

<div align="center">
![](http://i.imgur.com/0UFLbOv.png)
</div>

Minify disponible en: code.google.com/p/minify/ permite optimizar nuestro blog o sitio de manera fácil, tenemos que activar el plugin y listo, este se encarga de comprimir CSS, HTML y Javascript para que cuando los archivos necesarios sean solicitados por el navegador estos se envíen como archivos de menos tamaño y serán mucho mas rápidos de ejecutar. Podemos ver en la captura de arriba, que redujo el ancho de banda en un 70%.

Para activarlo en nuestro WordPress, tenemos que ir a la sección Plugins y buscar “WP Minify” damos permisos 777 (escritura, lectura y ejecución) a la carpeta wp-content/plugins/wp-minify/cache y activamos el plugin para que empiece a optimizar nuestro blog, sin la necesidad de modificar los archivos de la plantilla.

En la pagina del plugin podemos obtener mas información sobre sus características.

Una alternativa es utilizar Better WordPress Minify, que cuenta con opciones muy parecidas.

Con los dos complementos podemos seleccionar los archivos para que no sean comprimidos, por si no funcionan adecuadamente.

Para verificar el cambio en nuestro sitio, podemos utilizar PageSpeed insights que nos proporciona un análisis de nuestro sitio con consejos para mejorarlos y tener una mejor puntuación.

------------

## Manejo de librerías con GIT (submódulos)

A menudo sucede que mientras estamos desarrollando un proyecto, es necesario utilizar otro proyecto dentro del mismo. Puede ser una librería de terceros o una solución que se está desarrollando separadamente que a la vez herede de otros. El problema nos surge cuando queremos tener dicho proyecto actualizado e integrado.

Aquí un ejemplo: supongamos que estás desarrollando una aplicación y la creación de un toolkit te ayuda para terminar tu desarrollo. Entonces decides tener proyectos separados. La aplicación se alimenta del toolkit, y decides que el toolkit sea de código abierto y lo publicas en Github. Este es un ejemplo perfecto para emplear los submódulos. **Puedes tener tu toolkit en colaboración mientras tu App puede actualizarse simplemente con un pull**.


### Añadir submódulos al proyecto

Para agregar un repositorio Git existente como un submódulo del repositorio que estamos trabajando, utilizamos **git submodule add** junto a la URL absoluta o relativa del proyecto.

```
git submodule add https://github.com/Coderhouse/Coderhouse.Toolkit.git
```

Por defecto, los submódulos agregarán el subproyecto con el mismo nombre que el repositorio, en este caso **Coderhouse.Toolkit**. Nótese que se ha añadido **.gitmodules** como archivo, el cual almacena la correspondencia entre la URL del proyecto y el subdirectorio local. Si tiene varios submódulos, podrás tener varias entradas en ese archivo. Es importante tener este archivo con control de versiones, de la misma manera que el **.gitignore**, ya que si alguien hace un **fork** del proyecto, git sabrá con este archivo de dónde descargarse el **submodule**.

### Clonar un proyecto con submódulos

Para clonar un proyecto con submódulos se introduce el siguiente comando:

```
git clone https://github.com/Coderhouse/Coderhouse.git
```

Tenemos que tener en cuenta que esto solo trae los directorios sin los ficheros. Para poder recuperar los ficheros y actualizarlos, debemos hacer lo siguiente:

Para recuperar los ficheros:

```
git submodule init
```

Para actualizarlos:

```
git submodule update
```

También podemos ahorrarnos las dos últimas líneas con –recursive:

```
git clone --recursive https://github.com/Coderhouse/Coderhouse.git
```

¡Y ya tenemos nuestro proyecto con submódulos listo para trabajar!

------------

## Manejo de librerías con Composer (dependencias)

Composer es un manejador de dependencias para PHP. Este proyecto inspirado en npm y Bundler sirve para **administrar librerías de terceros o propias en nuestros proyectos de PHP**.

Composer es capaz de de instalar las librerías que requieres para tu proyecto con las versiones que necesites y si esas librerías dependen de otras también es capaz de resolver las dependencias y descargar todo lo que necesites para que funcione, quitándonos el dolor de tener que hacer esto manualmente y mantenerlo todo actualizado.

### Instalar Composer

Composer está formado por dos partes, la primera es una aplicación de línea de comandos que permite encontrar, descargar, actualizar y compartir los paquetes o librerías y el otro es el repositorio donde se guardan dichos paquetes o librerías.

Para instalar Composer solo debes ejecutar en el terminal:

```
cd /ruta_a_tu_proyecto_php
curl -s https://getcomposer.org/installer | php
```

Si NO tienes curl instalado puedes ejecutar:

```
php -r "eval('?>'.file_get_contents('https://getcomposer.org/installer'));"
```

Lo que hace este instalador es chequear que tu php.ini esté configurado con lo necesario y de lo contrario te arrojará un error.

Luego para comprobar que se instaló correctamente puedes ejecutar:

```
php composer.phar
```

Si todo salió bien debe desplegar una lista de las opciones del comando.

Si quieres ejecutar Composer globalmente ejecutamos este comando adicional:

```
sudo mv composer.phar /usr/local/bin/composer
```

Con esto podremos ejecutar directamente el comando desde cualquier sitio o proyecto, por ejemplo:

```
composer about
```

### Utilizando Composer

Como mencionamos anteriormente Composer está basado en Bundler y npm por lo que su uso es muy parecido a estas herramientas.

Para indicar las dependencias de tu proyecto debes crear un archivo llamado composer.json que en su forma más básica debe verse así:

```
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

**Veamos sus partes:**

* **require**: esta palabra simplemente le dice a Composer que necesitamos la libreria que le indicamos dentro de los corchetes {}
* **Nombre del paquete o librería**: está separado en dos partes vendedor/proyecto, normalmente estos dos son iguales como en este caso o pueden ser distintos por ejemplo “illuminate/foundation”.
* **Versiones del paquete**: después de los dos puntos ( : ) indicamos la versión que queremos de este paquete. Tenemos varias opciones:
* **Versión exacta**: podemos decirle a Composer que queremos una versión específica como por ejemplo 1.0.1.
* **Rango**: Podemos definir rangos de versiones con los operadores **>, >=, <, <=, !=**. Por ejemplo si queremos una versión mayor a la 2 colocariamos >2. También podemos definir entre dos versiones separando con comas, por ejemplo **>=1.0, **<**2.0**.
* **Comodín**: podemos usar el comodín (o wildcard) para definir versiones, como ejemplo 1.0., esto quiere decir cualquier versión dentro de 1.0 ( 1.0.1, 1.0.2 … 1.0.8 etc.)
**Por último tenemos el operador ~**, es un poco dificil de explicar así que usaremos un ejemplo: tenemos una librería que tiene la versión 1.x y 2.x pero no son compatibles entre ellas y queremos la última de 1.x y no la de 2.x entonces utilizaríamos el operador de esta forma **~**1.2 que es equivalente **a >=1.2,** **<**2.0**, mientras si vamos a algo más específico como *~***1.3.1 es equivalente a >=1.3.1, <1.4.

Como vemos podemos ser tan libres o específicos como queramos, y me falto mencionarles que si no nos importa la versión simplemente podemos colocar “*”.


Una vez que tengas configurado tu archivo **composer.json** con todos los paquetes que requiere tu proyecto es hora de que Composer haga su trabajo y lo instale para esto ejecutamos:

```
php composer.phar install
```

O si seguiste nuestro consejo e instalaste Composer como un comando global puedes ejecutar:

```
composer install
```

Una vez que termina de descargar los paquetes los coloca dentro de una carpeta llamada vendor por defecto esto lo podemos cambiar en nuestro archivo composer.json agregando las siguientes líneas:

```
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
    "config": {
        "vendor-dir":"librerias"
    }
}
```

**Tip**: Si estás utilizando git para tu proyecto, probablemente quieras agregar vendor (o el directorio que hayas especificado) dentro de .gitignore. Con esto evitamos agregar el código adicional a nuestro repositorio sin necesidad.

Si queremos actualizar las librerias ejecutamos:

```
php composer.phar update
```

O si está como comando global:

```
composer update
```

### Autoloading

Ademas de descargar las librerias, Composer también prepara un archivo de **autoload** que es capaz de autocargar todas las clases en cualquiera de las librerías descargadas. Para utilizarlo, simplemente añadimos la siguiente línea en nuestro código:

```
require 'vendor/autoload.php';
```

Por supuesto que si cambiaste el directorio de descarga lo tienes que actualizar en esta línea.

------------

## Creación de librerías propias

Cuando quieras darle un estilo propio y un un comportamiento especifico a tu sitio web  conviene crear tus propias librerias, la complejidad de estas van a depender del nivel al que quieras llegar, unas vez las termines de hacer  toca cargar tus  ficheros css y js; para ello es importante saber donde cargar correctamente esta información, primero necesitamos el fichero funtions.php que encontrarás dentro de la carpeta de tu tema.

Código para cargar tu fichero css (figúrate que el fichero que quieres cargar esta dentro de la carpeta css de tu tema “tutema/css/style.css”).

```
<!--?php function add_mystyle() { wp_register_style( 'style', get_template_directory_uri() . '/css/style.css' ); wp_enqueue_style( 'style' ); } add_action( 'wp_enqueue_scripts', 'add_mystyle' ); ?-->
```

Copiando este código en tu fichero functions.php conseguirás cargar tu propio fichero css en el head de tu tema, además también es muy cómodo si quieres incluir LESS para que la maquetación de tu web sea mucho más rápida, ya que incluyes el fichero css que te genera la compilación de LESS.

Para incluir tu fichero javascript el código es bastante similar y también va en el fichero functions.php.

```
<!--?php function myjquery() { wp_enqueue_script( 'Myjquery', get_template_directory_uri() . '/js/Myjquery.js', array( 'jquery' ) ); } add_action( 'wp_enqueue_scripts', 'Myjquery' ); ?-->
```

Consiste en crear una función que registra y llama a los ficheros de tu plantilla para que esta llamada se ejecute es importante hacer una llamada a la función para poder ejecutarla.

# WP REST API

WP-API convierte a WordPress en una poderosa API de publicación, y administración de contenidos, muy fácil de usar.

WP-API permite crear, modificar y eliminar los siguientes tipos de datos en WordPress:

* Entradas
* Páginas
* Usuarios
* Multimedia
* Taxonomías
* Tipos de contenido personalizados (CPT)

De esta manera entonces se podría eliminar todo el código PHP desde el administrador y el cliente, construyendo todo por ejemplo en Javascript, Python, etc.

La seguridad en el plugin de WP-API siempre ha sido un factor importante, existen tres formas de autenticarse para interactuar con el API:

* Autenticación por cookies (cliente)
* Autenticación básica por HTTP
* Protocolo OAuth 1
----------

## Instalación, Obtención y envío de datos

Lo primero que vamos a hacer es descargarnos el plugin e instalarlo en una instalación que tengas de WordPress para hacer pruebas. 

Una vez instalado el plugin ya podemos activarlo. Para nuestros ejemplos vamos a suponer que nuestra instalación de WordPress está en el dominio wpruebas.dev. Ajusta los enlaces permanentes en wordpress para que salgan las URLs con el nombre del post, o de la forma que estes habituado a hacerlo.

Si simplemente escribimos en nuestro navegador:


```
http://wpruebas.dev/wp-json/
```

Obtendremos lo siguiente:

```
{
    "name": "Wordpress de Pruebas",
    "description": "Probando JSON REST API",
    "URL": "http:\/\/wpruebas.dev",
    "routes": {
        "\/": {
            "supports": [
                "HEAD",
                "GET"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/"
            }
        },
        "\/posts": {
            "supports": [
                "HEAD",
                "GET",
                "POST"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/posts"
            },
            "accepts_json": true
        },
        "\/posts\/<id>": {
            "supports": [
                "HEAD",
                "GET",
                "POST",
                "PUT",
                "PATCH",
                "DELETE"
            ],
            "accepts_json": true
        },
        "\/posts\/<id>\/revisions": {
            "supports": [
                "HEAD",
                "GET"
            ]
        },
        "\/posts\/<id>\/comments": {
            "supports": [
                "HEAD",
                "GET",
                "POST"
            ],
            "accepts_json": true
        },
        "\/posts\/<id>\/comments\/<comment>": {
            "supports": [
                "HEAD",
                "GET",
                "POST",
                "PUT",
                "PATCH",
                "DELETE"
            ],
            "accepts_json": true
        },
        "\/posts\/types": {
            "supports": [
                "HEAD",
                "GET"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/posts\/types"
            }
        },
        "\/posts\/types\/<type>": {
            "supports": [
                "HEAD",
                "GET"
            ]
        },
        "\/posts\/statuses": {
            "supports": [
                "HEAD",
                "GET"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/posts\/statuses"
            }
        },
        "\/taxonomies": {
            "supports": [
                "HEAD",
                "GET"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/taxonomies"
            }
        },
        "\/taxonomies\/<taxonomy>": {
            "supports": [
                "HEAD",
                "GET"
            ]
        },
        "\/taxonomies\/<taxonomy>\/terms": {
            "supports": [
                "HEAD",
                "GET"
            ]
        },
        "\/taxonomies\/<taxonomy>\/terms\/<term>": {
            "supports": [
                "HEAD",
                "GET"
            ]
        },
        "\/users": {
            "supports": [
                "HEAD",
                "GET",
                "POST"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/users"
            },
            "accepts_json": true
        },
        "\/users\/me": {
            "supports": [
                "HEAD",
                "GET",
                "POST",
                "PUT",
                "PATCH",
                "DELETE"
            ],
            "meta": {
                "self": "http:\/\/wpruebas.dev\/wp-json\/users\/me"
            }
        },
        "\/users\/<user>": {
            "supports": [
                "HEAD",
                "GET",
                "POST"
            ],
            "accepts_json": true
        }
    },
    "meta": {
        "links": {
            "help": "https:\/\/github.com\/WP-API\/WP-API",
            "profile": "https:\/\/raw.github.com\/rmccue\/WP-API\/master\/docs\/schema.json"
        }
    }
}
```

Ahí ves una serie de datos en formato JSON. Nos da el nombre de la web, su descripción, su url, y lo que es realmente importante, las rutas (routes), que son el prototipo de URLs a través de las cuales podemos acceder a información. Verás que todas ellas tienen un apartado del tipo < nombre >. Esto serán variables. Veras también cómo indica si la ruta soporta POST, GET, etc…

Te preguntaras cómo hacer llegar esta URL hasta nuestra instalación de WordPress, porque claro, yo estoy escribiendo la URL en el navegador, pero una hipotética aplicación de móvil no podrá hacer eso. Lo puedes hacer por ejemplo usando CURL. Vamos a ver ahora las diferentes “rutas” o urls que podemos utilizar para acceder, editar o grabar datos.

#### Obteniendo entradas (posts)

Vamos a avanzar con la primera ruta. Supongamos que queremos obtener los posts que hay en nuestra instalación de WordPress. Para eso haríamos:

```
http://wpruebas.dev/wp-json/posts
```

Como ves sería añadir a la URL “/wp-json/” y luego la parte que le indica al plugin lo que tiene que hacer. Y en este caso lo que tiene que hacer es devolver todos los posts en formato JSON:

```
[{
"ID":1,
"title":"\u00a1Hola mundo!",
"status":"publish",
"type":"post",
"author{
 "ID":2,
 "username":"Admin",
 "name":"Admin",
 "first_name":"Admin",
 "last_name":"",
 "nickname":"Admin",
 "slug":"admin",
 "URL":"",
 "avatar":"http:\/\/0.gravatar.com\/avatar\/0836445cbbdd3c1bc46ce5db446a899fs=96",
 "description":"",
 "registered":"2015-03-07T19:59:10+00:00",
 "meta":{
  "links":{
    "self":"http:\/\/wpruebas.dev\/wp-json\/users\/2",
    "archives":"http:\/\/wpruebas.dev\/wp-json\/users\/2\/posts"
   }
  }
 },
"content":"<p>Bienvenido a WordPress. Esta es tu primera entrada. Ed\u00edtala o b\u00f3rrala, \u00a1y comienza a publicar!.<\/p>\n",
"parent":0,
"link":"http:\/\/wpruebas.dev\/hola-mundo\/",
"date":"2015-02-16T12:59:58+00:00",
"modified":"2015-03-07T19:59:45+00:00",
"format":"standard",
"slug":"hola-mundo",
"guid":"http:\/\/wpruebas.dev\/?p=1",
"excerpt":"<p>Bienvenido a WordPress. Esta es tu primera entrada. Ed\u00edtala o b\u00f3rrala, \u00a1y comienza a publicar<\/p>\n",
"menu_order":0,
"comment_status":"open",
"ping_status":"open",
"sticky":false,
"date_tz":"UTC",
"date_gmt":"2015-02-16T12:59:58+00:00",
"modified_tz":"UTC",
"modified_gmt":"2015-03-07T19:59:45+00:00",
"meta":{
   "links":{
      "self":"http:\/\/wpruebas.dev\/wp-json\/posts\/1",
      "author":"http:\/\/wpruebas.dev\/wp-json\/users\/2",
      "collection":"http:\/\/wpruebas.dev\/wp-json\/posts",
      "replies":"http:\/\/wpruebas.dev\/wp-json\/posts\/1\/comments",
      "version-history":"http:\/\/wpruebas.dev\/wp-json\/posts\/1\/revisions"
    }
 },
"featured_image":null,
"terms":{
   "category":[{
      "ID":1,
      "name":"Sin categor\u00eda",
      "slug":"sin-categoria",
      "description":"",
      "parent":null,
      "count":1,
      "link":"http:\/\/wpruebas.dev\/category\/sin-categoria\/",
      "meta":{
        "links":{
        "collection":"http:\/\/wpruebas.dev\/wpjson\/taxonomies\/category\/terms",
        "self":"http:\/\/wpruebas.dev\/wp-json\/taxonomies\/category\/terms\/1"
        }
      }
    }]
  }
}]
```

Aquí sólo hay un sólo post, pero saldrían todos. La paginación, de existir, viene dada en el HTTP headers. Por ejemplo:


```
X-WP-Total: 492
X-WP-TotalPages: 50
Link: <http://wpruebas.dev/wp-json/posts?page=4>; rel="next",
 <http://wpruebas.dev/wp-json/posts?page=2>; rel="prev"
```

El X-WP-Total da el número total de posts disponibles, mientras que el X-WP-TotalPages header da el número de páginas. El Link da los enlaces para obtener las páginas anterior y posterior.

Si lo que queremos es obtener un post concreto, lo que haríamos sería indicar el ID del post al final de la URL (en este caso el post con ID = 1):
	
```
http://wpruebas.dev/wp-json/posts/1
```

Para obtener más datos podemos utilizar los filtros. Lo filtros son parámetros que controlan lo que queremos obtener de los posts. Puedes ver una lista completa en http://wp-api.org/#posts_retrieve-posts. Un ejemplo, para obtener los dos últimos posts:

```
http://wpruebas.dev/wp-json/posts?filter[posts_per_page]=2
```


Los filtros se pueden además concatenar:

```
http://wpruebas.dev/wp-json/posts?filter[posts_per_page]=2&filter[order]=ASC
```

Por otro lado, si queremos seleccionar posts de un tipo determinado de post, usaríamos el parámetro Type:

```
http://wpruebas.dev/wp-json/posts?type[]=post&type[]=page
```

Por defecto es post, y se pueden poner como cadena de texto o como array (como en el ejemplo).

O si queremos mostrar los datos de una búsqueda (por ejemplo, todos los posts en los que salga la palabra “WordPress”):

```
http://wpruebas.dev/wp-json/posts?filter[s]=Wordpress
```

Como ves el método para obtener datos es similar en terminología al usado en WP_Query en un loop normal de WordPress.

Si lo que queremos es editar o crear un post lo tenemos que hacer mediante autenticación. Esta se puede hacer de dos maneras: mediante cookies si estamos en la misma web o mediante OAuth si estamos conectando desde fuera (en este último caso hay que instalar de momento el plugin OAuth1, que se integrará en el API cuando pase a estar en el core de WordPress).

A diferencia de la obtención de datos (usando GET) para grabar datos lo haríamos mediante POST mandando los datos en formato JSON, la edición mediante PUT y el borrado mediante DELETE.

#### Obteniendo la información meta de un post

Si queremos obtener los datos meta de un post haríamos:

```
http://wpruebas.dev/wp-json/posts/<id>/meta/<mid>
```

donde es el ID del post y es el ID del meta que queremos obtener. Para obtener los meta de un post se requiere Autenticación.

#### Categorías y etiquetas

Para obtener los posts de una categoría o de una etiqueta podemos utilizar también los filtros. Si queremos obtener todos los posts de una categoría (por ejemplo, de la categoría con ID = 5) tendríamos que poner:

```
http://wpruebas.dev/wp-json/posts?filter[cat]=5
```

Y si queremos obtener todos los posts de la etiqueta con ID = 1:

```
http://wpruebas.dev/wp-json/posts?filter[tag_id]=1
```

Si queremos obtener todos los datos sobre la categoría o etiqueta o taxonomía personalizada:

```
http://wpruebas.dev//wp-json/taxonomies/
```

Para obtener las diferentes categorías existentes:

```
http://wpruebas.dev/wp-json/taxonomies/category/terms
```

Y una en concreto:

```
http://wpruebas.dev/wp-json/taxonomies/category/terms/6
```

#### Trabajando con imágenes

También podemos obtener una imagen a partir de su ID (en el ejemplo con el ID 46):

```
http://wpruebas.dev/wp-json/media/46
```

Y lo que se obtenido es:

```
{
  "ID": 48,
  "title": "loading3",
  "status": "inherit",
  "type": "attachment",
  "author": {
    "ID": 2,
    "username": "Admin",
    "name": "Admin",
    "first_name": "Admin",
    "last_name": "",
    "nickname": "Admin",
    "slug": "admin",
    "URL": "",
    "avatar": "http:\/\/0.gravatar.com\/avatar\/0836445cbbdd3c1bc46ce5db446a899f?s=96",
    "description": "",
    "registered": "2015-03-07T19:59:10+00:00",
    "meta": {
      "links": {
        "self": "http:\/\/wpruebas.dev\/wp-json\/users\/2",
        "archives": "http:\/\/wpruebas.dev\/wp-json\/users\/2\/posts"
      }
    }
  },
  "content": "<p class=\"attachment\"><a href='http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31.gif'><img width=\"300\" height=\"300\" src=\"http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31-300x300.gif\" class=\"attachment-medium\" alt=\"loading3\" \/><\/a><\/p>\n",
  "parent": 0,
  "link": "http:\/\/wpruebas.dev\/?attachment_id=48",
  "date": "2015-03-15T17:00:38+00:00",
  "modified": "2015-03-15T17:00:38+00:00",
  "format": "standard",
  "slug": "loading3-2",
  "guid": "http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31.gif",
  "excerpt": null,
  "menu_order": 0,
  "comment_status": "open",
  "ping_status": "open",
  "sticky": false,
  "date_tz": "UTC",
  "date_gmt": "2015-03-15T17:00:38+00:00",
  "modified_tz": "UTC",
  "modified_gmt": "2015-03-15T17:00:38+00:00",
  "meta": {
    "links": {
      "self": "http:\/\/wpruebas.dev\/wp-json\/media\/48",
      "author": "http:\/\/wpruebas.dev\/wp-json\/users\/2",
      "collection": "http:\/\/wpruebas.dev\/wp-json\/media",
      "replies": "http:\/\/wpruebas.dev\/wp-json\/media\/48\/comments",
      "version-history": "http:\/\/wpruebas.dev\/wp-json\/media\/48\/revisions"
    }
  },
  "terms": [
    
  ],
  "source": "http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31.gif",
  "is_image": true,
  "attachment_meta": {
    "width": 512,
    "height": 512,
    "file": "2015\/03\/loading31.gif",
    "sizes": {
      "thumbnail": {
        "file": "loading31-150x150.gif",
        "width": 150,
        "height": 150,
        "mime-type": "image\/gif",
        "url": "http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31-150x150.gif"
      },
      "medium": {
        "file": "loading31-300x300.gif",
        "width": 300,
        "height": 300,
        "mime-type": "image\/gif",
        "url": "http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31-300x300.gif"
      },
      "post-thumbnail": {
        "file": "loading31-512x510.gif",
        "width": 512,
        "height": 510,
        "mime-type": "image\/gif",
        "url": "http:\/\/wpruebas.dev\/wp-content\/uploads\/2015\/03\/loading31-512x510.gif"
      }
    },
    "image_meta": {
      "aperture": 0,
      "credit": "",
      "camera": "",
      "caption": "",
      "created_timestamp": 0,
      "copyright": "",
      "focal_length": 0,
      "iso": 0,
      "shutter_speed": 0,
      "title": "",
      "orientation": 0
    }
  }
}
```

#### Usuarios

Como puedes imaginar gran parte del apartado de usuarios está también sujeto a autenticación. De todas formas, si queremos obtener todos los posts de un autor en concreto, partiendo de su ID (en el ejemplo con ID = 2):

```
http://wpruebas.dev/wp-json/posts?filter[author]=2
```

#### Comentarios

Podemos obtener los comentarios de un post (en el ejemplo los comentarios del post con ID 6904):

```
http://wpruebas.dev/wp-json/posts/6904/comments
```

Y si queremos obtener un comentario en concreto a partir del ID del comentario (por ejemplo el comentario con ID 1620 perteneciente al post con ID 6904):

```
http://wpruebas.dev/wp-json/posts/6904/comments/1620
```

Con todo esto puedes tener una buena idea de las posibilidades que nos ofrece el WP API REST.

# Contribuciones

WordPress es un proyecto guiado por el usuario, pues todos los desarrollos y mejoras dependen de personas como tú. Si te gusta ayudar a otros, tal vez deberías considerar tu participación en los distintos proyectos y en la Comunidad de WordPress en una o más de las formas descritas a continuación. Las contribuciones de los usuarios mantienen el proyecto vivo y en el camino correcto.

No importa si eres un desarrollador novato, un diseñador perfeccionista, o simplemente alguien que se siente bien por brindar una ayuda, WordPress siempre está en la búsqueda de personas que contribuyan a convertirle en una plataforma mejor. 

-----------

## Cómo contribuir con WordPress Core

La manera más obvia y más conocida de contribuir a WordPress es mediante **WordPress Core**. Al ser un proyecto de código abierto, WordPress es desarrollado por un gran equipo de colaboradores voluntarios, los cuales se encargan de realizar todos los cambios y modificaciones de una versión a otra dependiendo de la disponibilidad y de las habilidades pertinentes.

Pero el lanzamiento de nuevas versiones de WordPress no sólo depende de los desarrolladores: ellos también necesitan de personas para sacar adelante los proyectos y testear cada versión que acabe de salir. Echemos un vistazo a estas tres maneras de contribuir.

### Contribuciones vía "Trac Tickets"

Antes de que los cambios sean incorporados en una nueva versión, es probable identificar algunos problemas que deberán ser planteados por medio de **"tickets"**. 

En caso de hallar un error en WordPress, este puede notificarse mediante una entrada utilizando el sistema **Trac**. Si el fallo no ha sido identificado por un ticket anteriormente, no espere a que alguien más aunque lo notifique -cualquier persona con una cuenta de **wordpress.org** puede hacerlo.

El proceso se describe en detalle en el manual de WordPress ( puedes acceder a él en esta dirección https://make.wordpress.org/core/handbook/tutorials/trac/opening-a-ticket/), pero los pasos principales son los siguientes:

1. Buscar en **Trac** para asegurarte de que alguien ya no haya invocado un ticket sobre el tema que hayas identificado.

2. Si no has encontrado ningún ticket sobre esto, **"emite"** uno por tu cuenta.

3. Así, próximamente serás notificado acerca de lo que ocurra con tu ticket.

La emisión de estos tickets representa una gran ayuda para todos, ya que garantiza que los errores se traten con la mayor rapidez posible.

<div align="center">
![](http://i.imgur.com/cm8p70Y.png)
</div>

### Contribuciones con Código

Una vez que un ticket es emitido o una vez que se identifiquen nuevas características o mejoras para futuras versiones, alguien tiene que escribir el código para **incorporar los cambios en el WordPress Core**.

Si eres un desarrollador con experiencia y te sientes cómodo con el código de WordPress, probablemente puedas llegar a ser parte de ese equipo en un futuro. 

Hay un montón de recursos disponibles para ayudarle a hacer esto, y el proceso general es el siguiente:

1. **Identifica un tickets en el que puedas trabajar** - Hay dos lugares para encontrar tickets: en el **#core channel** en la comunidad de **Making WordPress** en Slack (ver https://wordpress.slack.com/?redir=%2Fmessages%2Fcore%2Fdetails%2F), y en el sistema de tickets en  Trac. Si aún no tienes una cuenta en **Slack**, puedes creártela utilizando tus datos de acceso de wordpress.org. Slack puede ser un poco confuso al principio, pero se ha convertido en la forma preferida de los colaboradores para comunicarse entre sí; por lo que si has empezado a trabajar en base a un ticket de Trac, también sería relevante que realizaras comentarios al respecto en Slack.

2. Si no estás seguro por donde comenzar, a menudo podrás encontrar tickets que están marcados como **"buenos primeros errores"**, ideales para los contribuidores que recién se inician en esto.

3. Trabaja en un parche utilizando el procedimiento descrito en el **Manual del Colaborador Core** (https://make.wordpress.org/core/handbook/).

4. Presenta tu parche utilizando la línea de comandos.

### Testing

Todas las novedades necesitan ser probadas tanto por los usuarios como por los desarrolladores. Esto es algo en lo que cualquiera puede participar.

Para participar en los testeos, necesitarás crear un **sitio de pruebas** en el que no tengas problemas de instalar código potencialmente inestable (de modo que NO deberás hacerlo en un sitio que esté en pleno funcionamiento). Instala el plugin **WordPress beta tester** y configúralo de acuerdo con la versión que deseas trabajar.

Así, podrás probar la última versión beta semi-estable o bien trabajar con la última versión altamente desarrollada de WordPress.

Si encuentra errores, sólo tiene que levantar un boleto para informar sobre ellos utilizando el sistema Trac.

<div align="center">
![](http://i.imgur.com/w1sj2is.png)
</div>

-----------

## Envío y evaluación de plugins y themes al repositorio oficial

Si eres un desarrollador pero contribuir con el Core no es lo tuyo, otra manera de contribuir es creando plugins y themes gratuitos para el uso de la comunidad WordPress.

Echemos un vistazo a estos aspectos a continuación.


### Themes

Todas las personas que utilizan WordPress para su sitio necesitan un tema. Mucha gente probablemente va a seguir con el tema por defecto, que se activa automáticamente cuando se instala por primera WordPress, pero la mayoría de la gente va a **buscar un tema que se adapte a las necesidades de su sitio**.

Existe un mercado creciente para los temas de primera calidad y los marcos temáticos, pero todavía hay cientos de miles de personas en busca de temas libres en el directorio de temas de WordPress.

Si has desarrollado un tema que piensas que puede ser útil en una amplia variedad de entornos, **¿por qué no dejar que otros lo utilicen de forma gratuita?**

Si deseas enviar un tema, deberás asegurarse de que cumple con las **directrices para la evaluación de temas** (https://make.wordpress.org/themes/handbook/review/). Aunque esto será verificado al momento de enviar tu tema, es una buena práctica para ponerte a prueba por primera vez a ti mismo. 

<div align="center">
![](http://i.imgur.com/MhyBfQC.jpg)
</div>

Descarga la **información de las pruebas unitarias tema** (https://wpcom-themes.svn.automattic.com/demo/theme-unit-test-data.xml) y lleva a cabo las pruebas para ti mismo para comprobar que aprobará la evaluación.

Para cargar tu tema para su revisión, ve a la página de **carga tema en wordpress.org** (https://wordpress.org/themes/upload/), y carga un archivo .zip con tu tema completo.

Tu tema será entonces analizado por los colaboradores voluntarios y si se aprueba, será añadido al **repositorio de temas**. Si se detecta algún problema, se te notificará qué es lo que debes corregir mediante la emisión de un ticket. Ver las directrices del proceso de revisión de temas para más detalles (https://make.wordpress.org/themes/handbook/review/).


### Plugins

El repositorio de plugins de WordPress contiene miles de plugins gratuitos. Estos van desde pequeños plugins que realizan trabajos muy simples a complejos plugins como WooCommerce y BuddyPress. Algunos plugins son completamente **gratuitos** mientras que otros ofrecen **complementos de primera calidad o versiones pro**.

Si has desarrollado un plugin para atacar un problema que aun no ha sido resuelto por ningún otro plugin, o si crees que tu plugin es mejor que los que ya están disponibles, entonces **¿por qué no añadirlo como una "cesión temporal" y dejar que otras personas lo utilicen?**

Contribuir con un plugin es similar al proceso de contribuir con un tema:

1. Añade un archivo **readme.txt** en tu plugin para explicar lo que hace y cómo usarlo.
2. Comprueba que el plugin cumple con las **pautas de presentación de plugins** (https://wordpress.org/plugins/about/guidelines/) y que está codificado de acuerdo con los **estándares de codificación de WordPress** (https://make.wordpress.org/core/handbook/coding-standards/).
3. Finalmente, envíalo a través de la **página de carga de plugins** (https://wordpress.org/plugins/add/).

<div align="center">
![](http://i.imgur.com/K2GuMfX.png)
</div>

Si tu plugin es aceptado y publicado, entonces necesitarás mantenerlo actualizado. Para ello, puedes hacer uso de **Subversion**. Podrás encontrar más información sobre esto en el sitio de WordPress.

-----------

## Cómo contribuir con proyectos de terceros

Los mejores temas y plugins se actualizan periódicamente (o al menos, se prueban en las nuevas versiones de WordPress, sin importar si se requiere una actualización o no). Así, estos procesos son apoyados a través de los foros de soporte en internet.

Los **foros de soporte de WordPress** son distintos a los foros que conocemos de algunos softwares. Si bien hay algunas personas cuyo trabajo es estar  en los foros de soporte y **responder a las preguntas**, hay muchas otras que lo hacen de manera voluntaria.

Cualquiera puede ayudar en los foros de soporte. Todo lo que necesitas es un **inicio de sesión de WordPress**. Basta con encontrar un ticket para ayudar, y dar una respuesta. Notarás que existe un grupo de personas que puede proporcionar soporte acerca de temas muy generales, mientras que hay otro con expertos que están en la capacidad de ofrecer ayuda en aspectos mucho más específicos de WordPress.

Muchas de estas personas están **contribuyendo con su experiencia y su voluntad de ayudar** al equipo, y tú puedes ser uno de ellos.

**Atención**: Las personas que piden ayuda probablemente han pasado mucho tiempo tratando de encontrar una respuesta en el Codex o googleando. Tal vez estén cerca del límite de sus fuerzas y en busca de una respuesta bien redactada, con una sugerencia, pregunta o una solución que les ayude a estar más cerca de la superación de ese problema. Muchas veces, podrías pensar que una pregunta es tonta, pero ¡no lo digas! No es agradable.
