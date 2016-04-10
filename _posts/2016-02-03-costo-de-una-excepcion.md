---
layout: post
title:  "El Costo de una Excepción"
date:   2016-01-12  20:01:00
published: false
categories: [javase]
tags: [conceptos, exception, principios, poo]
comments: true
shortinfo: Que parte de lanzar una excepcion es costoso o afecta el rendimiento
---

http://stackoverflow.com/questions/36343209/which-part-of-throwing-an-exception-is-expensive


Desde la version 5, Java incluye una característica muy util para realizar algunas cosas interesantes como rangos de valores, 
constantes, singletones y mas. 

Hablo de los `enum` o _**enumerados**_. No confundamos con enumeraciones el cual es otro concepto diferente en Java.

Si tienes un metodo como este:

{% highlight java linenos %}
void metodo(int diaSemana){...}
{% endhighlight %}<br/>

Y unas constantes para los dias de la semana:

{% highlight java linenos %}
public interface DiaSemana{

  int LUNES = 1;
  int MARTES = 2;
  int MIERCOLES = 3;
  int JUEVES = 4;
  int VIERNES = 5;
  int SABADO = 6;
  int DOMINGO = 7;

}
{% endhighlight %}<br/>

Una de las desventajas de esto es que este metodo puede recibir cualquier valor valido para el rango de un `int`.

Una posible solucion que nos ayude a limitar los valores que recibe el metodo es crear nuestra propia clase con unas 
constantes dentro, veamos como hacerlo:

{% highlight java linenos %}
public class PizzaSize {

    public static final PizzaSize SMALL = new PizzaSize();
    public static final PizzaSize MEDIUM = new PizzaSize();
    public static final PizzaSize BIG = new PizzaSize();

    private PizzaSize(){}

}
{% endhighlight %}<br/>

Aqui creamos una clase `PizzaSize` que posee 3 constantes del mismo tipo `PizzaSize` y el constructor es privado para que 
no se instancie desde afuera. Ahora si tuvieramos un metodo:

{% highlight java linenos %}
void crearPizza(PizzaSize size){...}
{% endhighlight %}<br/>

Aqui estamos limitando con una clase propia(`PizzaSize`) los posibles valores que recibe el metodo, en este caso nuestras 
constantes `SMALL`, `MEDIUM` y `BIG`.

Esta forma sencilla de restringir valores podriamos mejorarla por ejemplo, que cada tamaño posea la cantidad de partes. Una pizza Small tiene 6 pedazos, la Medium tiene 8 pedazos y la Big tiene 10 pedazos.

{% highlight java linenos %}
public class PizzaSize {

    public static final PizzaSize SMALL = new PizzaSize(6);
    public static final PizzaSize MEDIUM = new PizzaSize(8);
    public static final PizzaSize BIG = new PizzaSize(10);

    private int parts;
    private PizzaSize(int parts){
        this.parts = parts;
    }

    public int getParts() { return parts; }
}

public class Principal {

    public static void main(String[] args) {

        PizzaSize p = PizzaSize.MEDIUM;
        Principal main = new Principal();
        main.metodo(p);
    }

    public void metodo(PizzaSize size){
        System.out.println("Partes =" + size.getParts());
    }

}
{% endhighlight %}<br/>

Agregamos un variable de instancia, modificamos el constructor y pasamos los valores en cada constante.

Incluso podriamos obtener una lista de nuestras constantes:

{% highlight java linenos %}
public class PizzaSize {

    public static Set<PizzaSize> values = new HashSet<PizzaSize>();
    public static final PizzaSize SMALL = new PizzaSize(6, values);
    public static final PizzaSize MEDIUM = new PizzaSize(8, values);
    public static final PizzaSize BIG = new PizzaSize(10, values);

    private int parts;
    private PizzaSize(int parts, Set<PizzaSize> values){
        this.parts = parts;
        values.add(this);
    }

    public int getParts() { return parts; }
}

public class Principal {

    public static void main(String[] args) {

        PizzaSize p = PizzaSize.MEDIUM;
        Set<PizzaSize> values = PizzaSize.values;

        for(PizzaSize obj : values){
            System.out.println("Equals =" + obj.equals(p));
            System.out.println("Igual ==" + (obj == p));
        }

    }
}
{% endhighlight %}<br/>

Aunque esto puede ser una solución algo creativa para nuestros problemillas de constantes, el lenguaje nos ayuda con los 
tipos enumerados o `enum`.

## Enumerados al rescate
Un enumerado es una declaracion especial que nos permite limitar el rango de valores para un tipo de datos.

{% highlight java linenos %}
public enum PizzaSize {
    SMALL, MEDIUM, BIG;
}
{% endhighlight %}<br/>

Caracteristicas:

*   Pueden tener uno o varios constructores (privados por defecto)
*   Poseen variables de instancia
*   Poseen metodos (incluso permiten metodos abstractos)
*   Poseen algo llamado _constant specific class body_

Un `enum` se puede declarar dentro de una clase o dentro de una interface y son static por defecto.

{% highlight java linenos %}
public class PizzaImpl {

  public enum PizzaSize {
    SMALL, MEDIUM, BIG;
  }
}

public interface Pizza {

  int B = 10;

  void metodo();

  enum PizzaSize {
    SMALL, MEDIUM, BIG;
  }
}

public class Principal {

  public static void main(String[] args) {

    PizzaImpl.PizzaSize smallSize = PizzaImpl.PizzaSize.SMALL;
    Pizza.PizzaSize mediumSize = Pizza.PizzaSize.MEDIUM;
  }
}
{% endhighlight %}<br/>

Una de las ventajas es que los podemos usar en los `switch` y que poseen la lista de las constantes:

{% highlight java linenos %}
PizzaSize p = PizzaSize.BIG;
switch (p){
  case BIG:
       //...
       break;
  case MEDIUM:
       //...
  break;
}
{% endhighlight %}<br/>

Una de las cosas mas intersantes es que puedes colocar codigo especifico para cada constante, lo que se llama 
_**constant specific class body**_.

Ahora aqui esta lo que puede contener un enum en su declaracion:

{% highlight java linenos %}
public enum PizzaSize implements Foo{
    SMALL(6){
        public void pizzaMetodo(){};
    }, MEDIUM(8){
        public void pizzaMetodo(){}
    }, BIG(10){
        public void pizzaMetodo(){}
        public void fooBar(){
            System.out.println("Metodo sobreescrito para BIG");
        }
    };

    private int parts;
    private PizzaSize(int parts) {
        System.out.println("Constructor");
        this.parts = parts;
    }

    public int getParts() {return parts;}

    public void fooBar(){
        System.out.println("SMALL y MEDIUM");
    }

    //Metodo implementado en cada cuerpo de constante
    public abstract void pizzaMetodo();

    public static final String TIPO = "enum";

    static {
        System.out.println("Bloque de clase, ejecutado una sola vez");
    }

    {
        System.out.println("Bloque de Instancia, ejecuta por instancia =" + this.name());
    }
}
{% endhighlight %}<br/>

Los `enum` tambien son singletones.