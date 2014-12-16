---
layout: post
title:  "Contenedores e Inyección de Dependencias [02]"
date:   2014-07-11  09:24:00
categories: arquitectura conceptos
---

En la entrada anterior veíamos un poco el paradigma de _Inyección de Dependencias_ que promueve la **modularidad** y la **reutilización** de objetos, además los contenedores nos facilitan su uso. 

Vamos a crear un contenedor propio, claro solo con fines académicos para entrar mas en detalle y asimilar mejor estos conceptos.

Supongamos que en una oficina **X** necesitan un software para impresión de mensajes o documentos. En esta oficina hay una impresora Laser.

1.  Creamos una clase `Office` y una clase `LaserPrinter`.
2.  Creamos una clase `Main` que inicia nuestro programa.

## Versión cero

{% highlight java linenos %}
public class Office {

   private LaserPrinter printer = new LaserPrinter();
   private String message;

   public void receiveMessage(String msg){
      this.message = msg;
   }

   public void print(){
      printer.print(message);
   }

}

public class LaserPrinter{

   public void print(String message) {
      //Codigo para imprimir en laser
      System.out.println("Imprimiendo en Laser el mensaje :" + message);
   }

}

public class Main {

   public static void main(String[] args) {
      Office office = new Office();
      office.receiveMessage("Hola mundo");
      office.print();
   }

}
{% endhighlight %}

&nbsp;

## Primera Corrección

Existe un principio de dice que debemos **_programar contra interfaces y no contra clases concretas_**. Nuestro primer cambio entonces va sobre la propiedad `printer` en la clase `Office`.
Creamos una interface `Printer` para que sea implementada por `LaserPrinter` y reemplazamos en la clase `Office` la propiedad.

{% highlight java linenos %}
public interface Printer {
   void print(String message);
}

public class LaserPrinter implements Printer{

   public void print(String message) {
      //Codigo para imprimir en laser
      System.out.println("Imprimiendo en Laser el mensaje :" + message);
   }
}

public class Office {

   private Printer printer = new LaserPrinter();
   private String message;

   public void receiveMessage(String msg){
      this.message = msg;
   }

   public void print(){
      printer.print(message);
   }

}

public class Main {

   public static void main(String[] args) {
      Office office = new Office();
      office.receiveMessage("Hola mundo");
      office.print();
   }
}
{% endhighlight %}

&nbsp;

Ya cambiamos el tipo de la variable `printer` a una interface.

Surge una nueva oficina **Z** que tiene una impresora de puntos. Que hacemos? Pues fácil, creamos una implementación nueva para la interface `Printer` que trabaja con impresoras de punto. La llamaremos `DotMatrixPrinter`.

Ahora la interrogante si en el código de `Office` hemos instanciado directamente nuestra `LaserPrinter`:

{% highlight java linenos %}
private Printer printer = new LaserPrinter();
{% endhighlight %}

Que hacemos?

&nbsp;

## Segunda Corrección

Aplicamos el principio de _Inyección de Dependencias_, es decir, le quitamos a la clase `Office` la responsabilidad de hacer la instanciación de `LaserPrinter`. La clase `Office` debe recibir desde afuera la implementación indicada para la interface `Printer`. Podemos usar por ejemplo el constructor para pasar la implementación adecuada.

{% highlight java linenos %}
public interface Printer {
   void print(String message);
}

public class LaserPrinter implements Printer{

   public void print(String message) {
      //Codigo para imprimir en laser
      System.out.println("Imprimiendo en Laser el mensaje :" + message);
   }
}

public class DotMatrixPrinter implements Printer{

   public void print(String message) {
      //Codigo para imprimir en matrix de punto
      System.out.println("Imprimiendo en matrix de punto mensaje :" + message);
   }
}

public class Office {

   private Printer printer;
   private String message;

   public Office(Printer p){
      this.printer = p;
   }

   public void receiveMessage(String msg){
      this.message = msg;
   }

   public void print(){
      printer.print(message);
   }

}

public class Main {

   public static void main(String[] args) {
      DotMatrixPrinter dmp = new DotMatrixPrinter();
      Office office = new Office(dmp);
      office.receiveMessage("Hola mundo");
      office.print();
   }
}
{% endhighlight %}

&nbsp;

En este caso estamos inyectando desde afuera una impresora especifica. Esto es un caso sencillo, pero todos sabemos que en el mundo real no es así. Tu sistema tendrá cientos de clases que interactuan unas con otras.

Aqui es donde los contenedores me ayudan con toda esa cantidad de objetos y las conexiones entre ellos.

## Manos a la obra

Vamos a crear nuestro pequeño contenedor(le haremos competencia a _Google Guice_ y _Spring framework_). Explico el flujo básico que tienen estos contenedores conocidos.

1.  Se debe indicar nuestro punto de arranque, generalmente lo que llamamos un lanzador.
2.  Este lanzador carga o inicia el contenedor.
3.  El contenedor lee la configuración que hemos indicado de nuestros objetos y dependencias.
4.  Le pedimos al contenedor que inicie o nos proporcione nuestra aplicación.

Lo nuevo aquí es crear una clase que actúe como nuestro contenedor y que llamaremos `Container`. Ya tenemos nuestro lanzador que es la clase `Main`.

{% highlight java linenos %}
public class Container {

   private Map<class, Object> objects = new HashMap<>();

   public Container(){

      LaserPrinter lp = new LaserPrinter();
      objects.put(LaserPrinter.class, lp);

      Office office = new Office(lp);
      objects.put(Office.class, office);

   }

   public Object getObject(Class id){
      return objects.get(id);
   }

}

public class Main {

   public static void main(String[] args) {

      Container container = new Container();
      Office office = (Office) container.getObject(Office.class);
      office.receiveMessage("Hola mundo");
      office.print();
   }

}
{% endhighlight %}

&nbsp;

## Notas Finales

Vamos a puntualizar algunos aspectos aquí. Nuestro contenedor en el constructor tiene el código de "_configuración_" de los objetos. Esto lo podemos hacer de forma externa al contenedor, es decir, podemos pasar un archivo de configuración en el constructor con nuestros objetos como lo hace Spring. También podemos crear la configuración por código y que el contenedor la lea como hace Guice y  Spring.