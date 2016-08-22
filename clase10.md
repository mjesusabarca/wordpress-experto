# Patrones de Diseño y Prácticas Recomendadas en PHP

-------------

## Variables globales: cuándo y cómo conviene usarlas

En PHP, todas las variables creadas en la página, fuera de funciones, son **variables globales a la página**. Por su parte, las variables creadas dentro de una función son variables locales a esa función.

Las variables globales se pueden acceder en cualquier lugar de la página, mientras que las variables locales sólo tienen validez dentro de la función donde han sido creadas. De modo que una variable global la podemos **acceder dentro de cualquier parte del código**, mientras que si intentamos acceder a una variable local fuera de la función donde fue creada, nos encontraremos con que esa variable no tiene contenido alguno.

Ahora bien, si intentamos acceder a una variable global dentro de una función, en principio también nos encontraremos con que no se tiene acceso a su valor. Esto es así en PHP por motivos de claridad del código, para evitar que se pueda prestar a confusión el hecho de usar dentro de una función una variable que no ha sido declarada por ningún sitio cercano.

Entonces, si queremos utilizar una variable global a la página dentro de una función, tenemos que **especificar de alguna manera que esa variable que vamos a utilizar es una global**. Existen en PHP un par de maneras de utilizar variables globales a la página dentro de una función. Son las siguientes:
<br>
### Matriz GLOBALS

Existe un array en PHP llamado **$GLOBALS**, que guarda una referencia a todas las variables creadas de manera global a la página. Es una matriz o array asociativo, de los que en lugar de índices numéricos utilizan índices de texto, donde cada índice es el nombre que hemos dado a la variable y cada valor es el contenido de cada variable.

Supongamos que tenemos esta declaración de variables globales a la página, es decir, fuera de cualquier función:

```
$mivariable = "pepe"; 
$otravariable = 1234;
```

Si queremos acceder a esas variables dentro de una función utilizando el array *$GLOBALS* tendríamos este código:

```
function mifuncion(){ 
//estoy dentro de la función, para aceder a las variables utilizo $GLOBALS 
echo $GLOBALS["mivariable"]; 
echo $GLOBALS["otravariable"]; 
}
```

Como se puede ver, se accede al contenido de las variables globales con el array $GLOBALS, utilizando como índices de la matriz los nombres de variables que deseamos mostrar.

Esto imprimiría por pantalla el texto "pepe1234", el valor de las dos variables uno detrás del otro.
<br>
### Declaración de uso de variables globales dentro de una función

Otra cosa que podemos hacer para acceder a variables globales dentro de una función es **especificar al comienzo de dicha función la lista de variables que vamos a utilizar dentro**. Para especificar esas variables utilizamos la palabra *global* seguida de la lista de variables que se van a utilizar del entorno global.

```
function mifuncion(){ 
global $mivariable, $otravariable 
//con esa línea dentro de la función, declaramos el uso de variables globales 
echo $mivariable; 
echo $otravariable; 
}
```

Como vemos, con *global* se especifica que vamos a utilizar unas variables que fueron declaradas como globales a la página. Una vez hecho esto, ya podemos acceder a esas variables globales como si estuvieran declaradas dentro de la función.

**Cualquier alteración que hagamos a las variables dentro de la función permanecerá cuando se haya salido de la función**, tanto si accedemos a través del array *$GLOBALS* o declarando con *global* el uso de esas variables.

-------------

## Patrones de diseño y tipos de clases: singletons, abstracts, interfaces y mediators

### Singleton

El **patrón Singleton** puede ser muy útil en PHP porque gracias a él con una única instancia del objeto podemos acceder a él las veces que necesitemos, eso significa que no haremos uso innecesario de recursos ya que no estaremos creando nuevos objetos cada vez que lo necesitemos, para entenderlo mejor pongámonos en situación:

Tenemos una clase llamada **Users** en la que tenemos varios métodos que nos devuelven datos, en este caso los mismos datos de los usuarios, veamos:

```
<?php
class Users 
{
    private $dbh;
    private $fila = array();

    /*preparamos la conexión con la base de datos en el constructor,
    *veamos que su ámbito es público, de esta forma
    podemos crear instancias de la clase Users*/
    public function __construct() {              
        $host = 'server';
        $db = 'database';
        $usuario = 'user';
        $password = password;
        $this->dbh = new PDO("mysql:host=$host; dbname=$db", $usuario, $password);
    }
    //función para evitar problemas de ñ y acentos con la base de datos
    private function set_names() {
        return $this->dbh->query("SET NAMES 'utf8'");
    }
    //método con el que obtenemos los usuarios 
    public function usuarios()
    {
        self::set_names();
        $consulta = $this->dbh->prepare("SELECT * FROM users");
        $consulta->execute();
        if($consulta->rowCount()>0):
            while($row=$consulta->fetch())
            {
                $this->fila[] = $row;
            }           
            return $this->fila;
        endif;        
    } 
}
?>
```

Vemos que tenemos una clase llamada **Users** en la que creamos una conexión con la extensión PDO en el constructor y éste es público, es importante ver el detalle, a continuación tenemos un método en el que obtenemos los usuarios. Pues si queremos hacer uso de esta clase en cualquier archivo deberíamos hacer lo siguiente.

```
<?php 
require_once 'users.php'; 
//creamos una instancia del objeto y dentro tenemos
//todos los métodos y funciones de nuestra clase.
$user = new Users();
//accedemos al método usuarios y mostramos los datos
$datos = $user->usuarios();
?>
```

**Esa es la forma normal de acceder a una clase desde fuera, creando una instancia de la misma**, pero claro, en cada archivo que necesitemos acceder a esa clase deberemos crear una nueva instancia y eso al final hace que nuestra aplicación consuma más recursos de lo habitual, pues **aquí es donde entra en juego el patrón SIngleton**, para poder trabajar con él necesitamos hacer varias cosas, a saber:

Lo primero es que el ámbito del constructor de la clase sea o bien *protected* o *private*, después debemos crear un contenedor donde guardar la instancia que creemos, un método estatico que será donde creemos la instancia de la clase siempre que no haya sido creada o seteada anteriormente, eso lo comprobamos con **if(!isset)**, y finalmente para evitar que nuestro objeto pueda ser clonado y crearse más instancias del mismo hacemos uso del método mágico __clone().

Con esta breve explicación podemos entender para que utilizar el patrón Singleton, ahora veamos como llevarlo a cabo.

```
<?php
class PatronSingleton{
	
    // Contenedor de la instancia del singleton
    private static $instancia;
	private $usuarios = array();
	private $dbh;

    // Un constructor privado evita la creación de un nuevo objeto
    private function __construct() {
		$this->dbh = new PDO("mysql:host=server;dbname=database", "user", "password");
    }
 
    // método singleton
    public static function singleton()
    {
        if (!isset(self::$instancia)) {
            $miclase = __CLASS__;
            self::$instancia = new $miclase;
        } 
        return self::$instancia;
    }
	
	public function usuarios()
	{
		$consulta = $this->dbh->prepare("SELECT * FROM users");
		$consulta->execute();
		if ($consulta->rowCount()>0) 
		{
            while($reg = $consulta->fetch())
            {
               $this->usuarios[]=$reg;
        	}
            return $this->usuarios;     
        }
	}
   
    // Evita que el objeto se pueda clonar
    public function __clone()
    {
        trigger_error('La clonación de este objeto no está permitida', E_USER_ERROR);
    }
}
?>
```

Como podemos ver en la clase **PatronSingleton** hacemos exactamente lo explicado anteriormente, así que ahora simplemente nos quedaría ver como hacer uso de esta clase fuera de la misma, como la instancia ya ha sido creada en el método publico y estático Singleton simplemente deberemos acceder a él y a través del mismo al método o función que necesitemos. Veamos como conseguirlo.

```
<?php
//hacemos un require_once del archivo que contiene nuestra clase
require_once 'patronsingleton.php';
//accedemos al método singleton que es quién crea la instancia
//de nuestra clase y así podemos acceder sin necesidad de 
//crear nuevas instancias, lo que ahorra consumo de recursos
$nuevoSingleton = PatronSingleton::singleton();
//accedemos al método usuarios y los mostramos
$usuario = $nuevoSingleton->usuarios();
?>
Mostramos a todos los usuarios<br />
<?php
foreach ($usuario as $fila):?>
    <?=$fila['name']?> || <?=$fila['email']?><br /> 
<?php
endforeach;
?>
```

Si intentamos hacer un **new PatronSingleton()** nos dará un error ya que como el constructor es de ámbito privado no se nos permite instanciar a la clase, si por ejemplo queremos clonar el objeto tampoco nos dejará y nos mandará otro error, esto lo podríamos hacer de la siguiente forma:

```
<?php
require_once 'patronsingleton.php';
//accedemos al método singleton que es quién crea la instancia
//de nuestra clase 
$nuevoSingleton = PatronSingleton::singleton();
//accedemos al método usuarios y los mostramos
$usuario = $nuevoSingleton->usuarios();
//clonamos a $nuevoSingleton y así en $nuevoSingleton2 tendremos
//lo todos los métodos y funciones que en $nuevoSingleton, pero gracias 
//al método __clone así tampoco podremos crear otra nueva instancia y 
//el patrón singleton trabajará como habíamos esperado
$nuevoSingleton2 = clone $nuevoSingleton;
$otro_usuario = $nuevoSingleton2->usuarios();
```
<br>
### Abstract

**PHP 5 introduce clases y métodos abstractos**. Las clases definidas como abstractas no se pueden instanciar y cualquier clase que contiene al menos un método abstracto debe ser definida como tal. Los métodos definidos como abstractos simplemente declaran la firma del método, pero no pueden definir la implementación.

Cuando se hereda de una clase abstracta, todos los métodos definidos como abstractos en la declaración de la clase madre deben ser definidos en la clase hija; además, estos métodos deben ser definidos con la misma (o con una menos restrictiva) visibilidad. 

Por ejemplo, si el método abstracto está definido como protegido, **la implementación de la función debe ser definida como protegida o pública**, pero nunca como privada. Por otra parte, las firmas de los métodos tienen que coincidir, es decir, la declaración de tipos y el número de argumentos requeridos deben ser los mismos. Por ejemplo, si la clase derivada define un argumento opcional y la firma del método abstracto no lo hace, no habría conflicto con la firma. Esto también se aplica a los constructores a partir de PHP 5.4. **Antes de PHP 5.4, las firmas del constructor podían ser diferentes**.
<br>
#### Ejemplo N°1 de clase abstracta

```
<?php
abstract class ClaseAbstracta
{
    // Forzar la extensión de clase para definir este método
    abstract protected function getValor();
    abstract protected function valorPrefijo($prefijo);

    // Método común
    public function imprimir() {
        print $this->getValor() . "\n";
    }
}

class ClaseConcreta1 extends ClaseAbstracta
{
    protected function getValor() {
        return "ClaseConcreta1";
    }

    public function valorPrefijo($prefijo) {
        return "{$prefijo}ClaseConcreta1";
    }
}

class ClaseConcreta2 extends ClaseAbstracta
{
    public function getValor() {
        return "ClaseConcreta2";
    }

    public function valorPrefijo($prefijo) {
        return "{$prefijo}ClaseConcreta2";
    }
}

$clase1 = new ClaseConcreta1;
$clase1->imprimir();
echo $clase1->valorPrefijo('FOO_') ."\n";

$clase2 = new ClaseConcreta2;
$clase2->imprimir();
echo $clase2->valorPrefijo('FOO_') ."\n";
?>
```

El resultado del ejemplo sería:

```
ClaseConcreta1
FOO_ClaseConcreta1
ClaseConcreta2
FOO_ClaseConcreta2
```
<br>
#### Ejemplo N°2 de clase abstracta

```
<?php
abstract class ClaseAbstracta
{
    // El método abstracto sólo necesita definir los argumentos requeridos
    abstract protected function nombrePrefijo($nombre);

}

class ClaseConcreta extends ClaseAbstracta
{

    // La clase derivada puede definir parámetros opcionales que no estén en la estructura del prototipo
    public function nombrePrefijo($nombre, $separador = ".") {
        if ($nombre == "Pacman") {
            $prefijo = "Mr";
        } elseif ($nombre == "Pacwoman") {
            $prefijo = "Mrs";
        } else {
            $prefijo = "";
        }
        return "{$prefijo}{$separador} {$nombre}";
    }
}

$clase = new ClaseConcreta;
echo $clase->nombrePrefijo("Pacman"), "\n";
echo $clase->nombrePrefijo("Pacwoman"), "\n";
?>
```

El resultado del ejemplo sería:

```
Mr. Pacman
Mrs. Pacwoman
```

El código antiguo que no tenga clases o funciones definidas por el usuario llamadas 'abstract' deberían ejecutarse sin modificaciones.
<br>
### Interfaces

Las interfaces de objetos permiten **crear código con el cual especificamos qué métodos deben ser implementados por una clase**, sin tener que definir cómo estos métodos son manipulados.

Las interfaces son definidas utilizando la palabra clave **interface**, de la misma forma que con clases estándar, pero sin métodos que tengan su contenido definido.

Todos los métodos declarados en una interfaz deben ser públicos, ya que ésta es la naturaleza de una interfaz.
<br>
#### Código para definir una interfaz

Veamos el código para realizar una interfaz. En concreto veremos el código de la interfaz encendible, que tienen que implementar todas las clases cuyos objetos se puedan encender y apagar.

```
interface encendible{ 
   	public function encender(); 
   	public function apagar(); 
}
```

Vemos que para definir una interfaz **se utiliza la palabra clave interface, seguida por el nombre de la interfaz y, entre llaves, el listado de métodos que tendrá**. Los métodos no se deben codificar, sino únicamente declararse.
<br>
#### Implementación de interfaces

Ahora veamos el código para implementar una interfaz en una clase.

```
class bombilla implements encendible{ 
   	public function encender(){ 
      	echo "<br>Y la luz se hizo..."; 
   	} 

   	public function apagar(){ 
      	echo "<br>Estamos a oscuras..."; 
   	} 
}
```

Para implementar una interfaz, en la declaración de la clase, se debe utilizar la palabra **implements**, seguida del nombre de la interfaz que se va a implementar. Se podrían implementar varias interfaces en la misma clase, en cuyo caso se indicarían todos los nombres de las interfaces separadas por comas.

**En el código de la clase estamos obligados a declarar y codificar todos los métodos de la interfaz**.

```
Nota: en concreto, PHP 5 entiende que si una clase implementa una interfaz, los métodos de esa interfaz estarán siempre en la clase, aunque no se declaren. De modo que si no los declaramos explícitamente, PHP 5 lo hará por nosotros. Esos métodos de la interfaz serán abstractos, así que la clase tendrá que definirse como abstracta.
```

Ahora veamos el código de la clase coche, que también implementa la interfaz encendible. Este código lo hemos complicado un poco más.

```
class coche implements encendible{ 
   	private $gasolina; 
   	private $bateria; 
   	private $estado = "apagado"; 

   	function __construct(){ 
      	$this->gasolina = 0; 
      	$this->bateria = 10; 
   	} 

   	public function encender(){ 
      	if ($this->estado == "apagado"){ 
         	if ($this->bateria > 0){ 
            	if ($this->gasolina > 0){ 
               	$this->estado = "encendido"; 
               	$this->bateria --; 
               	echo "<br><b>Enciendo...</b> estoy encendido!"; 
            	}else{ 
               	echo "<br>No tengo gasolina"; 
            	} 
         	}else{ 
            	echo "<br>No tengo batería"; 
         	} 
      	}else{ 
         	echo "<br>Ya estaba encendido"; 
      	} 
   	} 

   	public function apagar(){ 
      	if ($this->estado == "encendido"){ 
         	$this->estado = "apagado"; 
         	echo "<br><b>Apago...</b> estoy apagado!"; 
      	}else{ 
         	echo "<br>Ya estaba apagado"; 
      	} 
   	} 

   	public function cargar_gasolina($litros){ 
      	$this->gasolina += $litros; 
      	echo "<br>Cargados $litros litros"; 
   	} 
}
```

A la vista del anterior código, se puede comprobar que **no hay mucho en común entre las clases bombilla y coche**. El código para encender una bombilla era muy simple, pero para poner en marcha un coche tenemos que realizar otras tareas. Antes tenemos que ver si el coche estaba encendido previamente, si tiene gasolina y si tiene batería. Por su parte, el método apagar hace una única comprobación para ver si estaba o no el coche apagado previamente.

También hemos incorporado un constructor que inicializa los atributos del objeto. Cuando se construye un coche, la batería está llena, pero el depósito de gasolina está vacío. Para llenar el depósito simplemente se debe utilizar el método **cargar _ gasolina()**.
<br>
### Mediator

Este modelo proporciona una manera fácil de desacoplar muchos componentes que trabajan juntos. Es una buena alternativa a *Observer* si tiene una "central de inteligencia", como un controlador (pero no en el sentido del MVC).

Todos los componentes (llamados Colleague) sólo están acoplados a la *MediatorInterface*, algo que es positivo porque **en la programación orientada a objetos, un buen amigo es mejor que muchos**. Esta es la clave-principal de este patrón.

<div align="center">
![](http://i.imgur.com/UNP6Jdp.png)
</div>
<br>
#### Código

```
<?php
namespace DesignPatterns\Behavioral\Mediator;
/**
 * Mediator is the concrete Mediator for this design pattern.
 * In this example, I have made a "Hello World" with the Mediator Pattern.
 */
class Mediator implements MediatorInterface
{
    /**
     * @var Subsystem\Server
     */
    protected $server;
    /**
     * @var Subsystem\Database
     */
    protected $database;
    /**
     * @var Subsystem\Client
     */
    protected $client;
    /**
     * @param Subsystem\Database $db
     * @param Subsystem\Client   $cl
     * @param Subsystem\Server   $srv
     */
    public function setColleague(Subsystem\Database $db, Subsystem\Client $cl, Subsystem\Server $srv)
    {
        $this->database = $db;
        $this->server = $srv;
        $this->client = $cl;
    }
    /**
     * make request.
     */
    public function makeRequest()
    {
        $this->server->process();
    }
    /**
     * query db.
     *
     * @return mixed
     */
    public function queryDb()
    {
        return $this->database->getData();
    }
    /**
     * send response.
     *
     * @param string $content
     */
    public function sendResponse($content)
    {
        $this->client->output($content);
    }
}
```

---------

## Conceptos de OOP y prácticas recomendadas: modularización, encapsulado, polimorfismo y herencia
<br>
### Modularización

La modularidad es, en programación modular y más específicamente en programación orientada a objetos, la propiedad que permite subdividir una aplicación en partes más pequeñas (llamadas módulos), cada una de las cuales debe ser tan independiente como sea posible de la aplicación en sí y de las restantes partes.

Estos módulos que se puedan compilar por separado, pero que tienen conexiones con otros módulos. Al igual que la encapsulación, los lenguajes soportan la Modularidad de diversas formas. La modularidad debe seguir los conceptos de acoplamiento y cohesión.

Según Bertrand Meyer: «El acto de particionar un programa en componentes individuales para reducir su complejidad en algún grado… A pesar de particionar un programa es útil por esta razón, una justificación más poderosa para particionar un programa es que crea una serie de límites bien definidos y documentados en el programa. Estos límites, o interfaces, son muy valiosos en la comprensión del programa».
<br>
#### Diseño web modular

Los viejos frames funcionan bien pero google, yahoo y ningún buscador los indexa bien, así que perdemos ranking y será muy raro que aparezcamos en un buscador, además no nos permiten disfrutar de las ventajas del CSS.

Para reemplazar a los viejos frames, es mejor usar PHP y separar las secciones de nuestra página web en módulos, en castellano la vamos trozar como pollo, de tal forma que cada pedazo contenga la estructura de la página:

* **top_page.php**: Aqui vamos a poner todos los encabezados html.
* **header.php**: Aqui vamos a poner el encabezado de nuestra página, es decir el título y el menú.
* **footer.php**: Aqui vamos a poner la parte de abajo de nuestra página nuestro copyright, links, etc.

#### top_page.php

Primero, todo *header* del HTML, nuestro título, hoja de estilos:

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Mi pagina modular</title>
<link href="style.css" rel="stylesheet" type="text/css" />
</head>
<body>
<div id="wrapper">
<br>
```
#### header.php

Aquí vamos a poner nuestro encabezado y menú:

```
<div id="header">
</div>
<div id="menu">
    <a href="#">Homepage</a>&nbsp;&nbsp;|&nbsp;&nbsp;    
    <a href="#">About Us</a>&nbsp;&nbsp;|&nbsp;&nbsp;
    <a href="#">Services</a>&nbsp;&nbsp;|&nbsp;&nbsp;
    <a href="#">Contact Us</a>
</div>
```
<br>
#### footer.php

Aqui la parte final de nuestra página web:

```
<div id="footer">
    Visita <a href="http://www.miguelmanchego.com" target="_blank">el Blog</a> para aprender diseño
</div>
</div>
</body>
</html>
```
<br>
#### Uniendo todo

Usando **includes** vamos a pegar todas esas partes que se repiten en nuestra página web, y quedaría algo asi:

```
<?php include("includes/top_page.php"); ?>
<?php include("includes/header.php"); ?>
<div id="contenido"> 
  <p>Nuestro texto va aqui</p> 
</div>
<?php include("includes/footer.php"); ?>
```
<br>
### Encapsulado

Aunque este lenguaje de programación no es muy estricto en cuanto a POO, se pueden emular la mayoría de cosas que son necesarias en Objetos, en este caso Encapsulamiento. Para comenzar es bueno tener en cuenta la ocultación de información(encapsulamiento). Muchos dicen que PHP rompe el encapsulamiento debido a que nos permite acceder a cualquier atributo de una clase directamente, pero en si hay una forma de hacer que no se pueda y así generar el encapsulamiento. Para ello tenemos el concepto de visibilidad en php. 

El concepto de visibilidad nos dice que hay tres palabras claves/reservadas: public, protected y private que anteponiendolas a cualquier método o atributo/propiedad se podrán modificar o implementar su visibilidad, haciendo posible el encapsulamiento.

**Public**: Se podrá acceder a ella de cualquier forma, directamente o por métodos de la misma o de otras clases. Ejemplo:

```
class Persona{
public $nombre;
public $apellido;
public $edad; // Es public por lo que se accede desde 
cualquier parte
public function __construct($nom, $ape, $e){
       $this->nombre = $nom;
       $this->apellido= $ape;
       $this->edad = $e;
       }
public function imprimir(){
    return $this->edad;
}
}
$persona = new Persona('Pepito','Perez',24);
echo $persona->edad;
echo $persona->imprimir();
```

Como vemos es la misma clase persona que se viene trabajando. Creamos el objeto y hacemos echo a la edad directamente, lo que nos imprimirá un 24, despues por medio del metodo **imprimir()** tambien nos pondra en pantalla el mismo dato, teniendo en cuenta el *return* en la linea 12.

**Protected**: Si cambiamos el *public* por el *protected* y dejamos lo demas igual, nos saldrá un fatal error, ya que con *protected* no nos dejara acceder a la propiedad a menos que sea por métodos de la misma clase, herencia o con el *parent*.

```
class Persona{
     public $nombre;
     public $apellido;
     protected $edad; 
      public function __construct($nom, $ape, $e){
            $this->nombre = $nom;
            $this->apellido= $ape;
            $this->edad = $e;
      }
     public function imprimir(){
         return $this->edad;
     }
 }
 $persona = new Persona('Pepito','Perez',24);
 echo $persona->edad;
 echo $persona->imprimir();
```

Si comentamos la linea donde imprimimos directamente la propiedad, nos imprimirá el resultado del metodo **imprimir()**.

 ```
 //echo $persona->edad;
 echo $persona->imprimir();
```

**Private**: Al poner la propiedad edad en *private* solo nos dejará acceder desde la misma clase, entonces si hacemos el ejercicio anterior:

```
class Persona{
      public $nombre;
      public $apellido;
      private $edad; 
      public function __construct($nom, $ape, $e){
            $this->nombre = $nom;
            $this->apellido= $ape;
            $this->edad = $e;
      }
     public function imprimir(){
         return $this->edad;
     }
 }
 class trabajo extends Persona{
     public $trabajos;
     public $pruebas;
     public function trabajar($job){
         $this->trabajos = $job;
     }
     public function imprimir(){
         return 'Mi nombre es '.$this->nombre.' '.$this->
         apellido.' y tengo '.$this->edad.' Años y trabajo 
         en  '.$this->trabajos;
     }  
  }
 $persona = new Persona('Pepito','Perez',24);
echo $persona->imprimir();
```

Si corremos PHP con el código anterior, no nos dejará acceder a la propiedad edad de la clase Persona desde trabajo porque es privada, si fuera protected si nos dejaria pero solo por metodos o parent.
<br>
### Polimorfismo

El polimorfismo, junto con la encapsulacion y la herencia, forman parte de los pilares basicos de la programacion orientada a objetos.

El polimorfismo, como su nombre indica, sugiere multiples formas. En programacion cuando hablamos de polimorfismo nos referimos a la capacidad de acceder a multiples funciones a traves del mismo interfaz. Es decir que un mismo identificador, o funcion puede tener diferentes comportamientos en funcion del contexto en el que sea ejecutado.

Su implementacion varia en funcion del lenguaje de programacion. En algunos casos para establecer una relacion poliformica es necesario que cada uno de los objetos implicados compartan una misma raiz, siendo entonces necesario establecer una jerarquia de clases. Este es el caso de los lenguajes de programacion fuertemente tipados como Java.

Para el caso que nos ocupa vamos a estudiar el polimorfimo en PHP estableciendo una jerarquia de clases, ya que nos resultara mas facil portar el mismo ejemplo a otros lenguajes.
<br>
#### ¿Como se implementa el polimorfismo?

A continuacion veremos un ejemplo de como implementar una llamada polimorfica al metodo de un objeto.

Para ello vamos a crear una sencilla jerarquia de clases donde tendremos una clase base llamada "classPoligono" y sus respectivas clases extendidas : "classTriangulo" , "classCuadrado" , "classRectangulo" . Cada una de estas clases tendra un metodo en comun que se llamara "calculo" y cuya funcion sera la de mostrar la formula matematica para el calculo del area de la figura geometrica en cuestion.

Una vez que hemos definido nuestras clases crearemos la funcion que se encargara de hacer la llamada polimorfica al metodo "calculo" cuya ejecucion variara dependiendo del objeto que lo implementa.

```
<?php
/*
Empezaremos definiendo la jerarquia de clases
*/
class classPoligono
{
  function calculo()
  {
  echo 'El area depende del tipo de poligono';
  }
}

class classCuadrado extends classPoligono
{
  function calculo()
  {
  echo 'area de un cuadrado : a=l*l<br>';
  }
}

class classRectangulo extends classPoligono
{
  function calculo()
  {
  echo 'area de un rectangulo : a=b*h<br>';
  }
}

class classTriangulo extends classPoligono
{
  function calculo()
  {
  echo 'area de un triangulo : a=(b*h)/2<br>';
  }
}
/* fin definicion de la jerarquia de clases */

/* 
definicion de la funcion encargada de realizar las llamada 
polimorfica al metodo "calculo"
A destacar que en la definicion de la funcion definimos el tipo 
parametro que pasamos por referencia, esto no es obligatorio 
en PHP, pero nos ayuda a entender el concepto y asi poder aplicarlo 
en otros lenguajes mas estrictos.
 */
function area(classPoligono $obj)
{
  $obj->calculo();
}

/*
Creamos los objetos necesarios
*/
$cuadrado = new classCuadrado;
$rectangulo = new classRectangulo;
$triangulo = new classTriangulo;

/*
Ejecutamos la funcion encargada 
de realizar la llamada polimorfica
*/
area($cuadrado);
area($rectangulo);
area($triangulo);
?>
```

Al ejecutar el ejemplo anterior vemos que la funcion "area" nos muestra la formula correcta en cada una de sus ejecuciones para cada tipo de figura geometrica, pese a que en su definicion inicial hayamos especificado que el objeto es del tipo "classPoligono" , haciendo referencia a la clase base de cada objeto. Esto seria necesario en algunos lenguajes en los cuales nos hace falta un nexo comun a cada uno de los objetos, y la unica forma es de utilizar la clase base comun a cada uno de ellos.

En PHP esta definición no seria necesaria, ya que no es un lenguaje en el que sea obligatorio especificar el tipo de una variable en su definicion, por lo que en este aspecto es muchos menos estricto que otros lenguajes de programación.
<br>
### Herencia

La herencia es un principio de programación bien establecido y PHP hace uso de él en su modelado de objetos. Este principio afectará la manera en que muchas clases y objetos se relacionan unas con otras.

Por ejemplo, cuando se extiende una clase, la subclase hereda todos los métodos públicos y protegidos de la clase padre. A menos que una clase sobrescriba esos métodos, mantendrán su funcionalidad original.

Esto es útil para la definición y abstracción de la funcionalidad y permite la implementación de funcionalidad adicional en objetos similares sin la necesidad de reimplementar toda la funcionalidad compartida.

**NOTA:**
A menos que la carga automática sea utilizada, entonces las clases deben ser definidas antes de ser usadas. Si una clase se extiende a otra, entonces la clase padre debe ser declarada antes de la estructura de clase hija. Esta regla se aplica a las clases que heredan de otras clases e interfaces.
<br>
#### Ejemplo de herencia

```
<?php
class Foo
{
    public function printItem($string)
    {
        echo 'Foo: ' . $string . PHP_EOL;
    }
    
    public function printPHP()
    {
        echo 'PHP is great.' . PHP_EOL;
    }
}

class bar extends Foo
{
    public function printItem($string)
    {
        echo 'Bar: ' . $string . PHP_EOL;
    }
}

$foo = new Foo();
$bar = new Bar();
$foo->printItem('baz'); // Salida: 'Foo: baz'
$foo->printPHP();       // Salida: 'PHP is great' 
$bar->printItem('baz'); // Salida: 'Bar: baz'
$bar->printPHP();       // Salida: 'PHP is great'

?>
```

# Recomendaciones Finales y Posibles Problemas

-----------

## Breve repaso de contenidos

A lo largo del curso hemos brindado todas las herramientas para que usted pueda desarrollar sus propios productos/proyectos dentro de la plataforma **WordPress**, regidos por la utilización de las mejores prácticas.

Desde el inicio, hemos listado las herramientas más utilizadas en la actualidad para el desarrollo profesional orientado a WordPress, así como los lugares a los que se puede acudir en busca de recursos avanzados. De igual modo, se aprendió la estructura básica de themes, child themes y sus posibilidades de personalización.

También se ofreció información relacionada con la creación de funcionalidades extensibles, la prevención de vulnerabilidades comunes, las estructuras de los plugins, la sanitización de datos ingresados por el usuario, la construcción de widgets de manera eficiente y el manejo avanzado de templates.

Además, el curso fue propicio para estudiar todo lo relacionado con el manejo de roles y capacidades de usuarios y la obtención de datos de forma remota. Pudimos ahondar en los preceptos que supone el paradigma de OOP y en la utilización de JavaScript y CSS mediante funciones nativas de WordPress. Finalmente, se hizo alusión a las características de versiones de PHP más actuales, cómo aplicarlas, cómo lidiar con problemas de compatibilidad, y cómo *debuguear* nuestros códigos sin necesidad de escribir ningún código adicional.


------------

## Compatibilidad a través de distintas versiones de PHP y WordPress

Es importante conocer que versión de php es compatible con nuestra versión de wordpress y así para poder hacer una instalación exitosa en el servidor que hemos seleccionado. Por recomendación Apache y Linux son las plataformas mas robustas para correr WordPres. A continución se listan las versiones compatibles de wordpress y php:

Requerimientos del servidor para WordPress versión 2.0 o anterior:

* PHP versión 4.1 o posterior.

Requerimientos del servidor para WordPress versión 2.0 o posterior:

* PHP versión 4.2 o posterior.

Requerimientos del servidor para WordPress versión 3.2 o posterior:

* PHP versión 5.2.4 o posterior.



------------

## WordPress como solución global

Wordpress se ha covertido en los últimos años en el más popular gestor de contenidos, su éxito radica no sólo en que es un sistema de código abierto sino en que existe una enorme comunidad de creadores y diseñadores web que está continuamente trabajando para mejorar el sistema de gestión a través de plugins y temas de fondo.

Algunos de los usuarios más conocidos de WordPress son: Samsung, Ford, LinkedIn, Flickr, The Wall Street Journal, Blackberry, The Rolling Stones, Katy Perry, Tom Jones, Ryan Seacrest etc.

Instalar WordPress manualmente puede ser sencillo y solo llevarte unos minutos, aunque se requieren ciertos conocimientos técnicos sobre bases de datos, y la configuración manual puede no darte el resultado esperado . Para asegurar un trabajo bien hecho, hoy en día puedes contar con un freelance que configure tu sitio web y haga un diseño adaptado a tus necesidades. Una vez configurado puedes llevar la gestión de tu sitio web sin complicación alguna, hacerlo crecer y que sea un éxito dependerá solo de ti.

Su amplia variedad de opciones temáticas, ha sido razón para su elección. Tanto si optas por un tema libre, un tema premium o decides tener un diseñador de sitios web de WordPress para crear uno desde cero, tienes una larga lista de opciones para elegir. Las posibilidades de ampliar las funciones, aplicaciones, themes, plugins y demás de tu página son prácticamente infinitas.

Ademas Wordpress te permite aumentar  las funcionalidades  de tu sitio web mediante plugins, en donde hay literalmente miles de plugins gratuitos y de primera calidad que tu o tu diseñador pueden utilizar para mejorar tu sitio web. Hay plugins para todo tipo de cosas, desde plugins para crear un sitemap en cuestión de segundos hasta para enlazar tu blog con todas las redes sociales.

Por último y no menos importante, WordPress es un software de código abierto y de uso gratuito. No solo es gratis al principio , como hacen otros servidores de código abierto, que al llegar a determinado nivel empiezan a cobrarte por el servicio, siempre será gratis, y esta es la clave de su gran éxito.


-------------

## Documentación interna y externa

#### Documentación interna

Codex, es el manual en línea para WordPress y un repositorio viviente para la información y documentación de WordPress. En el encontraras cualquier tipo de información que van desde instalar una nueva versión en WordPress, conocer sus características o plugins, llevar a cabo una copia de seguridad, usar temas y plantillas, diseñar tu sitio en WordPress hasta temas más avanzados como los relacionados con servidores web, bases de datos o importación de contenidos. Este documento está integramente traducido al castellano para facilitar su aprovechamiento a toda la comunidad en español interesada en el auto aprendizaje. si quieres buscar información del Codex pueden acceder al mismo desde codex.wordpress.org.


#### Documentación externa

Si quieres aprender sobre WordPress mediante fuentes distintas a su página oficial y convertirte en todo un experto, te recomiendo que dediques unos minutos a visitar los Blogs de WordPress que se te predentan a continuación:

1. Ayuda WordPress disponible en http://ayudawordpress.com/
2. WP Directo disponible en http://wpdirecto.com/
3. +Que WordPress http://www.masquewordpress.com/
4. TodoWP disponible en http://www.todowp.org/
5. Blog Pocket disponilbe en www.blogpocket.com
6. Trazos Web disponible en http://www.trazos-web.com/
7. Mecus Blog disponible en http://mecus.es/blog/

-------------

## Foco en el usuario

Para un usuario es muy común cometer errores a la hora de utilizar wordpress por primera vez, principalmente por su falta de conocimiento y experiencia. Por esta razon es importante que consideres estos puntos cuando quieras empezar a trabajar con este gestor de contenidos: 	

* Buscar un Buen Hosting. El primer paso es buscar un buen Hosting, regularmente recomiendo empresas como Hostgator o Banahosting, esta última es espectacular, sin embargo hay en internet un sin fin de empresas de hospedaje web a buen precio, que te pueden sacar del apuro para levantar tu sitio web.

* Comprar el Dominio que deseas. El segundo paso es igual de simple, lo que deberás hacer es comprar tu propio dominio para tu sitio web, ya sea un dominio .com, .net, .org o el que tengas a la mano para el nombre que necesitas, toma en cuenta que con este nombre encontrarán tu sitio web.

* Instalar WordPress en tu Hosting. Posterior a tener estas dos cosas, vamos a proceder a instalar wordpress en nuestro host, si tienes dudas de como hacerlo, el mismo sitio de WordPress.org nos enseña como debemos hacerlo paso a paso para no fallar en nada.

* Configura tu WordPress y Listo. Por último ya que tenemos el sitio web montado, podemos acceder a el desde nuestro administrador, y lo primero que deberás hacer antes de empezar a crear contenido, es configurarlo, así que te vas a la sección de ajustes y empiezas a optimizar tu sitio de la mejor manera posible.

-------------

## Foco en otros programadores

Adentrarse en el mundo del desarrollo de Wordpress no es tarea fácil, sin embargo tampoco es algo imposible, una de las primeras recomendaciones que se le puede dar a alguien que apenas inicia es no tener miedo a experimentar;  Para conocer un programa o una nueva herramienta  la mejor forma para hacerlo es utilizarlo. Para ello no hay que tener miedo a que algo se estropee.

Muchas veces instalamos plugins o temas que provocan fallos en WordPress, ya sea por problemas con tu hospedaje o por el propio plugin o tema que no es compatible al 100% con la versión que utilices. Es por ello que es aconsejable tener tu propio entorno de desarrollo WordPress para testeo y lo uses para probar sin miedo a estropear tu blog en producción.

Otro consejo muy útil es leer bastante, principalmente todo lo que encuentres  sobre WordPress, Administración de sistemas Linux, Programación PHP, gestión de base de datos MySQL, seguridad informática, etc. La idea con esto es ir incrementando nuestro conocimiento y una vez manejemos al menos lo básico de WordPress y queramos personalizar nuestro blog, ya se tendría los conocimientos suficientes para leer directamente el código fuente de WordPress. El origen de la documentación de WordPress son los comentarios de su código. Leyendo el código fuente, veremos de primera mano cómo funciona internamente WordPress a través de los desarrolladores de la comunidad.


-------------

## El problema del rendimiento

El interior de WordPress dispone de un buen funcionamiento, pero una vez que comenzamos la instalación de plugins el rendimiento y la capacidad de respuesta de nuestro sitio web puede verse afectado. El consumo de recursos puede llegar a ser demasiado grande, dando lugar a una suspensión de nuestra cuenta de alojamiento, especialmente si utilizamos alojamiento compartido.

Una recomendación para mejorar esta situación es instslar un plugin de cache. Mostrar versiones en caché de todas nuestras páginas es una de las maneras más eficientes para la mejora de velocidad de un sitio y reducir el consumo de CPU y memoria del servidor. Si todavía no estamos utilizando un plugin de caché, recomiendo utilizar el plugin W3TC, ya que funciona de manera mas eficiente que los demás plugins de caché disponibles e incluye mas opciones de optimización.

Si tenemos plugins con funciones que pueden ser reemplazadas de otra forma, que no estamos utilizando o que sirven de poco, lo mejor es eliminar todos los que podamos y quitar peso del servidor. Es posible que podamos perder una funcionalidad o dos, pero a largo plazo esto va a mantener el sitio funcionando rápido y con seguridad.

Hay que tener en cuenta que es posible codificar las funciones que algunos plugins realizan. Por ejemplo, en vez de utilizar un plugin para generar una lista de “Entradas populares”, podemos crear la lista nosotros mismos con HTML puro. En vez de utilizar un plugin para el formulario de contacto, podemos escribir simplemente un párrafo en nuestra página de contacto, incluyendo nuestra dirección de correo electrónico en ese lugar, y así sucesivamente con todo los que podamos evitar.

Otra recomendacion para mejorar el rendimiento es optimizar las tablas de la bases de datos. Como pasa con cualquier sistema de gestión de contenido dinámico, gran parte del trabajo de nuestro sitio de WordPress se obtiene, almacena, elimina y actualiza la información en nuestra base de datos. Esa es la razón por lo que la optimización de las tablas en las bases de datos pueden afectar el rendimiento de nuestro sitio. Un buen complemento que podemos utilizar para este caso es WP-Optimize, con esta herramienta se puede eliminar las revisiones posteriores, limpiar los comentarios (por ejemplo, el spam y los enviados a la papelera), ejecutar consultas de optimización y mucho mas.

--------------

## Solidez y flexibilidad de workflow

Para realizar sitios web en wordpress siempre es recomendable contar con un flujo de trabajo que te establecerá los lineamientos y procedimientos a seguir para cumplir con todas las fases del proyecto, con el fin de entregar un trabajo de calidad en un tiempo relativamente rápido. Existen diversas maneras de encarar un proyecto realizado en wordpress y dependerá de la complejidad y el timepo que se tenga para la entrega del mismo.

Para comenzar un sitio web en wordpress simpre es importante identificar las necesidades y proponer soluciones del contenido que tendrá el sitio web, una idea que resulta eficaz es crear un mapa de contenidos, que te brindará la facilidad de relacionar y visualizar cuales de estos se complementan. Al finalizar es necesario hacer un esquema de la estructura de contenidos.

Realizar un boceto o wireframe, sin gráficos resulta muy conveniente cuando se quiere tener una idea rápida de cómo será el diseño de la web, además permite realizar cambios fácilmente. Cuando todo ya está claro que es lo que se quiere, se realiza un prototipo ya con gráficos, fotos ficticias etc., muy oportuno para mostrárselo al cliente. Normalmente se puede realizar con herramientas de diseño como Photoshop, Fireworks o Illustrator.

Una vez aprobado el prototipo por el cliente es el momento de decidir si utilizar una plantilla similar o crear una desde cero, todo ellos va a depender de las especificaciones dados por el cliente y el presupuesto que maneje.




-------------

## Espacio para preguntas y respuestas

Siempre es importante contar con espacios destinados para consuldar alguna incertidumbre o problemas que se nos puede presentar en el desarrollo con wordpress. Existen muchos foros que te brindan un espacios para tal fin, con respecto a wordpress desde su sitio web se encuentra su foro oficial en español disponible en https://es.forums.wordpress.com/  en el cualquier persona que tenga alguna inquietud puede hacerla de manera sencilla, una ventaja es lo activo de este foro considerandolo un punto importante. Otro foro muy popurar es el del sitio Ayuda Wordpress disponible en: http://ayudawp.com/foros/.
