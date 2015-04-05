---
layout: post
title:  "El objeto único [Patrón Singleton]"
date:   2013-03-31  14:24:00
categories: patrones
---

Existen muchas situaciones en el desarrollo de una aplicación, en la que se desea tener un punto de acceso único hacia 
algún tipo de recurso del sistema, un objeto que centralice la administración de dicho recurso que es compartido.

Entonces necesitamos tener una y sola una instancia de esta clase. No mas, solo un objeto que esté disponible para ser 
accesado desde cualquier parte de donde sea llamado.

## Singleton
La definición para este patrón de diseño dada por GoF es:

> Asegura que una clase tiene sólo una instancia y proporciona un punto global de acceso a esta.

Esta dentro de la categoría de los patrones de creación. Es uno de los más simples e involucra una sola clase que se 
instancia a sí misma proporcionado un punto de acceso único para tal objetivo.

El diagrama de clases es:

![Patron Singleton](/images/singleton.jpg)<br/>

En el diagrama vemos:

1. **Una variable de clase:** Referencia la instancia única de la clase.
2. **Un constructor privado:** No permite instanciar la clase con new desde afuera.
3. **Un método estático:** Acceso único hacia la variable(que debe ser static tambien).

El codigo seria el siguiente:

{% highlight java linenos %}
public class Singleton {

  private static Singleton instance;

  private Singleton() {}

  public static Singleton getInstance(){
    if(instance == null){
      instance = new Singleton();
    }
    return instance;
  }
}
{% endhighlight %}<br/>

Al usarlo:

{% highlight java linenos %}
Singleton s = Singleton.getInstance();
{% endhighlight %}<br/>

Aunque el código anterior funciona, tiene problemas en un ambiente multihilos. Que pasaria si dos o mas hilos usan el 
método `getInstance()`?. Se pueden crear varias instancias de la clase aunque solo una de estas instancias queda referenciada.

Una solución a esto es usar `synchronized`:

{% highlight java linenos %}
public class Singleton {

  private static Singleton instance;

  private Singleton() { }

  public static synchronized Singleton getInstance(){
    if(instance == null){
      instance = new Singleton();
    }
    return instance;
  }
}
{% endhighlight %}<br/>

Esto soluciona nuestro problema de concurrencia y funciona bien, pero y si la clase `Singleton` es altamente usada, 
`synchronized` es costosa y podría causar un cuello de botella. Que opciones tenemos?.

Otra opción es dejar a la máquina virtual que haga el trabajo, lo que hacemos es instanciar directamente la variable, 
también conocida como instanciación estática:

{% highlight java linenos %}
public class Singleton {

  private static Singleton instance = new Singleton();

  private Singleton() { }

  public static Singleton getInstance(){
    return instance;
  }
}
{% endhighlight %}<br/>

Esta opción tiene la característica de crear la instancia al momento de cargar la clase cuando la aplicación está iniciando, 
es decir una inicialización _**“eager”**_. Muchas veces es recomendable una inicialización _**"lazy"**_, es decir que solo 
se crea la instancia del `Singleton` cuando es usada por primera vez por la aplicacion.

Otra opción es **_“double-check locking”_** que se usan dos instrucciones `if` con un `synchronized` en medio:

{% highlight java linenos %}
public class Singleton {

  private static Singleton instance;

  private Singleton() { }

  public static synchronized Singleton getInstance(){
    if(instance == null){

      synchronized (Singleton.class){

        if(instance == null){
          instance = new Singleton();
        }
      }
    }
    return instance;
  }
}
{% endhighlight %}<br/>

Este patrón de diseño genera controversias. De hecho hay quienes dicen que no es patrón de diseño y que es usado fuera del 
contexto adecuado.

## Desventajas
**Dificultad para realizar testing:** Un `Singleton` es como una valor de variable global. Lo global y la orientación a objeto 
no son buenos amigos ya que introduce un “estado persistente”, es decir valores que se mantienen siempre dificultando el uso 
de objeto de reemplazo(mock) en test.

**Promociona el alto acoplamiento:** Si hay algo que debe ser alto en la orientación a objetos es la cohesión y no el acoplamiento. 
El `Singleton` es instanciado directamente desde su propia clase promocionando el uso de métodos privados y estáticos. Esto acopla 
la clase que los use además de impedir el uso adecuado de inyección de dependencias.

**Restricción de ejecuciones paralelas:** Aunque un objetivo del `Singleton` sea la gestión de un recurso compartido esto 
restringe operar de forma paralela a la aplicación y lo transforma en un cuello de botella de operaciones seriales que no 
es recomendable cuando la demanda es alta.

## Usos
Los usos de este patrón pueden ser varios:

**Logger :** Este es el ejemplo más adecuado de como usar un `Singleton`. Tienes un archivo de log(recurso compartido) que 
solo debe ser accesado por un proceso. Además de no permitir la creación de nuevos objetos cada vez que realiza una 
operación de logging.

**Configuración :** Permite tener un punto de acceso único a los parámetros de configuración de una aplicación manteniendo 
el estado de estos durante la ejecución, independientemente de donde se cargaron los valores al iniciar el sistema, si de 
una base de datos o un archivo.

**Pool y Factorías :** Aunque estos sean igualmente patrones de diseño, usan el `Singleton` como base ya que un pool o una 
factoría es un objeto único que tiene un estado que ofrece como servicio, permitiendo que la aplicación acceda a ese 
estado siempre que lo necesite.

## Conclusiones
El `Singleton` debe ser usado con sabiduría. Una forma de evitar el mal uso es no instanciarlo directamente dentro de la 
clase que lo necesite como dependencia. Para realizar esto lo recomendable es que el `Singleton` implemente una interface 
para hacer flexible el uso de inyección de dependencias y el testing.