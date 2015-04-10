---
layout: post
title:  "Hilos en Java [01]"
date:   2011-03-30 20:20:00
published: true
categories: [javase]
tags: [concurrencia, hilos, javase, thread]
comments: true
shortinfo: Primera entrega sobre concurrencia y programación de hilos en Java
---

Bueno esta es la primera de varias entregas sobre concurrencia o hilos en Java.  Vamos a tratar de ir de 0 a 100 para entender mejor todo este
mundo de la multitaréa con el fin de aplicar lo que aprendamos en nuestros diseños de una forma mas optima.

## Introducción a los Hilos
Para no alargar mucho la historia ya que pueden conseguir bastantes definiciones por ahí, Java nos permite la ejecución de varios procesos al
tiempo algo muy parecido a lo que hace el sistema operativo. Bueno realmente sabemos que con un solo núcleo o procesador solo se ejecuta un proceso
a la vez, pero dada la velocidad con que se realizan para nosotros son como procesos paralelos.

Con Java la **JVM** es nuestro pequeño sistema operativo en este caso y es la encargada con su sistema de scheduling (planificador) de manejar
nuestros hilos o procesos de forma concurrente.

Si tuviéramos un sistema operativo como Solaris, es posible que la JVM haga un mapeo de cada hilo que nosotros creamos directamente a procesos del mismo sistema operativo.

Cuando hablamos de un HILO en Java tenemos dos posibles conceptos según el contexto:

1. Un hilo es una instancia de la clase `Thread` ubicada en el paquete `java.lang`.
2. Un hilo es un proceso(Thread) de ejecución.

Una instancia de `Thread` es un objeto común y corriente, de los que usamos con un simple new:

{% highlight java linenos%}
Thread hilo = new Thread();
{% endhighlight %}<br/>

Aquí solo he creado un simple objeto de tipo `Thread` y este tendrá variables, métodos, vivirá y morirá en el **Heap**. No es un hilo de ejecución porque no lo he iniciado como tal. 

Un _Thread de ejecución_ es un proceso individual con su propia _**PILA DE LLAMADAS**_ (Call Stack).

## Pila de Llamadas
En Java siempre hay aunque sea un único hilo ejecutándose, este hilo se crea al llamar al método `main()`. Es decir, siempre tengo un hilo llamado _**main**_ que es el hilo inicial. Para este hilo main, se crea una pila de llamadas que no es mas que la secuencia en que los métodos se van invocando unos a otros.

![Hilos 01](/images/hilos-01.jpg)<br/><br/>

Desde el método `main` llamaremos al `método1`, desde el `método1` llamamos al `método2` y así sucesivamente.

Para cada nuevo hilo que iniciamos se creara una pila de llamadas totalmente independiente una de otra. Si lanzamos dos hilos y cada uno llama al mismo método de una misma instancia de una clase(osea el mismo objeto en el heap), este se copia en cada pila sin interferir uno con otro. Cada hilo tendrá su propia copia del contenido de dicho método, sip, _**incuso de las variables locales**_ (variables de método) y todo los declaremos dentro de dicho método. 

Ojo que no pasa lo mismo con las variables o atributos de la instancia, osea _**el estado de ese objeto es compartido**_ y si un hilo modifica un atributo el otro hilo vera ese cambio, pero ese tema lo vemos después con mas detalle, solo quería que entendieran el mecanismo de la pila de llamadas.

![Hilos 02](/images/hilos-02.jpg)<br/><br/>

Cada vez que lanzamos un nuevo hilo, este nuevo call stack o pila de llamadas empieza con el método `run()`. Evidentemente el hilo principal empieza su stack con main.

## Usando hilos
La forma de usar hilos en Java es a través de la clase `java.lang.Thread`. Esto se puede hacer de dos formas:

1. Crear una clase que extienda de `Thread` y sobrescribir el método `run()`.
2. Implementar la interface `Runnable` y pasarla a un objeto `Thread`.

En cualquiera de los casos hay que usar un objeto `Thread`.

{% highlight java linenos%}
public class HiloUno extends Thread{
    public void run() {
        // Aqui mi codigo de ejecucion
    }
}
 
public class Principal {
    public static void main(String[] args) {
        HiloUno h1 = new HiloUno();
        HiloUno h2 = new HiloUno();
        h1.start();  // Se crea pila de llamadas
        h2.start();  // Se crea pila de llamadas
    }
}
{% endhighlight %}<br/>

Esta es la forma de lanzar o iniciar un hilo de ejecución. Usando el método `start()` estoy convirtiendo un simple objeto `Thread` en una nueva pila de llamadas, en este caso estamos lanzando dos procesos mas aparte del _**main**_, osea tenemos tres hilos de ejecución. Pero ojo si llamáramos solo el método `run()` sin usar `start()` estaríamos ejecutando un simple método en un simple objeto y no crearía ninguna pila de llamadas, osea no crearíamos ningún proceso aparte. 

De la otra forma sería:

{% highlight java linenos%}
public class Ejecutor implements Runnable{
    public void run() {
        // Aqui mi codigo de ejecucion
    }
}
public class Principal {
    public static void main(String[] args) {
        Ejecutor e = new Ejecutor();
        Thread t = new Thread(e);
        t.start();
    }
}
{% endhighlight %}<br/>

Pasar en el constructor de `Thread` un objeto `Runnable` y de igual manera iniciar el hilo de ejecución con `start()`. De hecho `Thread` tiene varios constructores
y la misma clase `Thread` también implementa `Runnable` así que es posible pasar un `Thread` a otro a través de su constructor.

{% highlight java linenos%}
public class MiHilo extends Thread{
    public void run() {
        // Aqui mi codigo de ejecucion
    }
}
 
public class Principal {
    public static void main(String[] args) {
        MiHilo h1 = new MiHilo();
        Thread t = new Thread(h1);
        t.start();  // Se crea pila de llamadas
    }
}
{% endhighlight %}<br/>

Generalmente se recomienda usar el segundo enfoque implementando `Runnable` porque es mas flexible en cuanto a diseño. En ocasiones mi modelo puede que no permite extender de `Thread` porque ya extiende de otra clase. Aunque usar el primer enfoque no quiere decir que precisamente este mal, como dije es cuestión de diseño o gustos .

## Ciclo de Vida
Antes de pasar a trabajar más con los hilos conozcamos el ciclo de vida de estos. Cada _hilo de ejecución_ tiene un ciclo de vida determinado por sus posibles estados.

![Hilos 03](/images/hilos-03.jpg)

* **Nuevo :** Este es el estado al crear una instancia de la clase `Thread`, pero aun no es un hilo vivo, no lo hemos iniciado con `start()`.
* **Ejecutable :** Este estado aparece al llamar el método `start()` y aquí se convierte en un hilo vivo con su propia pila de llamadas.
* **Ejecutandose :** Este estado indica que el hilo esta ejecutando parte del código del método `run()`.
* **Esperando / Bloqueado / Dormido :** Estos estados aparecen por algún motivo, el propio hilo se fue a dormir, esta en espera de algún recurso, el planificador lo decidió, etc.
* **Muerto :** El hilo se considera muerto cuando terminó la ejecución del método `run()`.

Podemos obtener el estado de un hilo con el método `getState()` de `Thread`. Este retorna un enum que pertenece a la misma clase. 

El planificador posee una especie de bolsa donde echa todos los hilos en estado _**Ejecutable**_ para luego seleccionar uno y ponerlo en ejecución. La decisión de cual hilo seleccionar para ejecutar siempre pertenece al planificador, aunque bajo ciertas condiciones pudiéramos influenciar al planificador, por ejemplo especificando las prioridades de algunos hilos no significa que lo podemos controlar.

Hay varios escenarios por los cual el planificador puede sacar un hilo en ejecución y enviarlo a la bolsa de ejecutables o ponerlo en otro estado(esperando, bloqueado, dormido o muerto):

* Completo el método `run()`.
* Hay una llamada al método `wait()` – Lo veremos después.
* Un hilo no pudo obtener el _bloqueo(Lock)_ de un objeto - Lo veremos después.
* El planificador tomó la decisión según su criterio (porque le dio la gana ).

Unos puntos para finalizar esta primera parte de hilos:

1. Un hilo en estado muerto(finalizó el método `run()`) no puede volver a iniciarse con `start()` ya que la JVM nos lanza una `IllegalThreadStateException`.
2. Puedo encadenar hilos. Un hilo inicia solo cuando otro termine.
3. Un hilo puede ceder el paso a otro y se va automáticamente de Ejecutándose a Ejecutable.
4. Un hilo no puede bloquear a otro.

Hasta aquí esta primera parte, see you later .
