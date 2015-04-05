---
layout: post
title:  "Hilos en Java [02]"
date:   2011-04-15  17:57:00
published: true
categories: [javase]
tags: [concurrencia, hilos, javase, thread]
shortinfo: Segunda entrega sobre concurrencia y programacion de hilos en Java
---

En esta segunda entrega de hilos vamos a trabajar un poco mas con ellos teniendo en cuenta los conceptos que vimos en 
la [_primera entrega_]({% post_url 2011-03-30-hilos-en-java-parte-1 %}).

## Trabajando con Hilos
Conozcamos un poco el API de **Thread** para ver que podemos hacer con ella.

Podemos obtener una referencia al hilo actual:  Obteniendo la referencia del hilo actual podemos manipularlo según lo que necesitemos.

{% highlight java linenos%}
Thread hiloActual = Thread.currentThread();
{% endhighlight %}<br/>

Revisando un poco el API de Thread podemos encontrar algunos métodos interesantes:<br/>

**Conocer el id y el nombre de un hilo.**

{% highlight java linenos %}
Thread hiloActual = Thread.currentThread();
long id = hiloActual.getId();
String nombre hiloActual.getName();
{% endhighlight %}<br/>


**Saber la prioridad.**

La clase `Thread` tiene 3 constantes enteras(`int`) que especifican los limites y el punto de medio para la prioridad de un hilo y que según la implementación de la JVM podría ser diferentes.

*   Prioridad mínima: **`Thread.MIN_PRIORITY`**
*   Prioridad máxima: **`Thread.MAX_PRIORITY`**
*   Prioridad normal: **`Thread.NORM_PRIORITY`**

Trabajamos la prioridad con su `get` y `set` correspondiente, **`getPriority()`** y **`setPriority(int)`**.

## Cediendo el paso
Es posible que según el caso tomemos la decisión de que el hilo que se esta ejecutando actualmente le ceda el paso a otro de la piscina de hilos.
Pero no le decimos al planificador cual hilo seleccionar para ejecutarse, solo le decimos "oye me voy a la piscina envía a otro a ejecutarse", pero es
posible que incluso decida que el hilo actual que cedió el paso regrese a ejecutarse.  Si recordamos el ciclo de vida de un hilo vemos que se puede
ir de **ejecutandose** a **ejecutable** y esto se consigue cediendo el paso con el método static **`yield()`**.

{% highlight java linenos%}
Thread hiloActual = Thread.currentThread();
Thread.yield(); // Metodo static
hiloActual.yield(); // produce el mismo efecto
{% endhighlight %}<br/>


## Hilos dormilones
Podemos poner a dormir un hilo con el método static `sleep()`, cuando este despierta regresa a ejecutable de ahí el planificador decidirá cuando enviarlo
a ejecutar de nuevo. Cual es la utilidad de esto?, bueno podemos tener diferentes razones importantes una podría ser que la operación que realiza consume mucho
ancho de banda por ejemplo. Es cuestión de criterios en nuestro diseño.

{% highlight java linenos%}
Thread hiloActual = Thread.currentThread();
try {
  // 5 Segundos (5000 milis)
  Thread.sleep(5000); // Metodo static
  hiloActual.sleep(5000); // produce el mismo efecto
} catch (InterruptedException ex) {
  ex.printStackTrace();
}
{% endhighlight %}<br/>

Note que debemos usar un try ya que es posible que se lanze una excepcion si este hilo es interrumpido de su estado actual.

## Encadenando hilos
Es posible que encadenemos un hilo a otro, es decir que un hilo no empieza a ejecutarse hasta que otro termine. Que su estado sea terminado o muerto. El método no
estático `join()` permite hacer esto.

{% highlight java linenos%}
public class HiloUno extends Thread{
    private String s;

    public HiloUno(String name) {
        this.s = name;
    }

    @Override
    public void run() {
        for(int x = 0; x <4; x++){
            System.out.println(this.s);
        }
    }
}

public class Main {

    public static void main(String[] args) throws InterruptedException{

        HiloUno t1 = new HiloUno("A");
        t1.start();
        t1.join(); // Encadenamos el actual a t1
        System.out.println("FIN MAIN");

    }
}
{% endhighlight %}<br/>

En este ejemplo estamos encadenando el hilo actual(main en este caso) a t1, la impresión de "FIN MAIN" solo aparecerá hasta que se imprima cuatro veces la
letra "A" que pasamos en el constructor de t1.

Ahora como encadenamos dos hilos diferentes que no sea el actual main?. Recordemos que `join()` toma el hilo actual que se esta ejecutando y lo encadena a otro.
Si queremos encadenar t2 a t1 podriamos hacerlo al iniciar t2 teniendo una referencia de t1.

Veamos el código.

{% highlight java linenos%}
public class HiloUno extends Thread{

    private String s;
    private Thread padre;

    public HiloUno(String name) {
        this.s = name;
    }

    public void setPadre(Thread padre) throws InterruptedException{
        this.padre = padre;
    }

    @Override
    public void run(){
        try {
            if(padre != null) {
                padre.join(); // Encadenamos
            }
            for(int x = 0; x <4; x++){
                System.out.println(this.s);
                Thread.sleep(2000); // Que duerma 2 segundos
            }
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
    }
}

public class Main {

    public static void main(String[] args) throws InterruptedException{

        HiloUno t1 = new HiloUno("A");
        HiloUno t2 = new HiloUno("B");

        t2.setPadre(t1);
        t1.start();
        t2.start();

        System.out.println("FIN MAIN");

    }
}
{% endhighlight %}<br/>

Bueno ahí lo que hicimos fue ver en run() si t2 tiene un padre para encadenarse. Así t2 debe esperar a que su padre(t1) termine. Usamos sleep para
ayudarnos a ver el proceso lentamente cuando ejecutemos este programa.

[Hilos en Java - Parte 1]({% post_url 2011-3-30-hilos-en-java-parte-1 %})
