---
layout: post
title:  "Hilos en Java [Parte 4]"
date:   2012-02-13  13:28:00
categories: javase
shortinfo: Artículo número 4 sobre hilos en Java.
---

## Interacción entre Hilos 
Hay situaciones en que necesitamos que los hilos puedan comunicarse entre si. Un hilo puede necesitar información 
de otro para completar o seguir su proceso. La forma de hacerlo es con 3 metodos:

*   **_wait()_**
*   **_notify()_**
*   **_notifyAll()_**

Vamos a dar un ejemplo para entender un poco mejor esto: supongamos que tenemos un **_hilo A_** que realiza unos cálculos y 
otro _**hilo B**_ necesita esos resultados para continuar con su proceso. **_B_** debe esperar por _**A**_. La forma de hacerlo 
es obtener el bloqueo(lock) de A y luego desde A notificar que ya están listos los cálculos.

{% highlight java linenos %}
class TestClient {
   public static void main(String[] args) { 
      HiloA a = new HiloA(); 
      a.start(); 
      synchronized (a) { 
         try { 
            a.wait(); // esperamos por A 
         } catch (InterruptedException e) { 
            e.printStackTrace(); 
         } 
         System.out.println("A notifica que terminó... seguimos"); 
         System.out.println("Total="+a.getTotal()); 
      } 
   }
}

class HiloA extends Thread{
   private int total;
   public void run (){ 
      System.out.println("Iniciando A!"); 
      synchronized (this) { 
         for(int x = 1; x <= 5; x++){ 
            System.out.println("Procesando... X="+x); 
            total = total + x; 
            try { 
               Thread.sleep(1000); 
            } catch (InterruptedException e) { 
               e.printStackTrace(); 
            } 
         } 
         notify(); 
         System.out.println("Puede haber mas codigo..."); 
      } 
      System.out.println("Incluso fuera de synchronized"); 
   }

   public int getTotal(){ 
      return total; 
   }
} 
{% endhighlight %}<br/>

Aquí el hilo actual debe esperar hasta que _**A**_ le notifique para que este pueda mostrar el resultado. Colocamos 
un **_sleep_** para apreciar mejor el efecto.Ahora vamos a tener en cuenta un par de cosas:

*   Los metodos `wait()`, `notify()` y `notifyAll()` deben ser llamados dentro de un contexto sincronizado, es decir 
dentro de un método o bloque de código con `synchronized`.
*   Solo se pueden llamar estos métodos si se posee el bloqueo(Lock) sobre el objeto en el que se van a invocar.

Ahora como aplicamos esto a una situación real? pues bien el uso mas común podría ser con el clásico ejemplo de productor y 
consumidor. Un productor coloca datos en un sitio común y el consumidor obtiene los datos de ese sitio común. 

Ejemplos: Un pool de impresión. Se envían documentos al sitio común(en este caso el pool) y la impresora accesa a ese pool y consume. 
Otro ejemplo puede ser una bandeja de entrada de correos, un buffer de quemado de CD, una lista de pedidos, etc.

Es recomendable que ese buffer, pool o sitio común tenga unas reglas definidas para que productores y consumidores no 
causen estragos en el sistema. Ej: El buffer tendrá un limite maximo para que el productor no siga colocando cosas y 
también un limite mínimo para que el consumidor por ejemplo si dicho buffer esta en cero no tiene que consumir nada.

Veamos el codigo:

{% highlight java linenos %} 
import java.util.LinkedList; 
import java.util.Queue;

public class Buffer {
   private Queue q = new LinkedList();
   public synchronized void putData(long d){ 
      if(q.size() >= 5){ 
         //control del limite maximo 
         try { 
            wait(); 
         } catch (InterruptedException e) { 
            e.printStackTrace(); 
         } 
      } 
      q.add(d); 
      notify(); 
      //notificamos que se agrego data //asi el consumidor tratara de obtenerla 
   }
   
   public synchronized Long getData(){ 
      if(q.size() == 0){ 
         //si es 0 no hay que consumir 
         try { 
            wait(); 
            //esperamos entonces 
         } catch (InterruptedException e) { 
            e.printStackTrace(); 
         } 
      } 
      Long d = q.poll(); 
      notify(); 
      //notifico que se sacaron datos 
      //el productor seguira produciendo mas 
      return d; 
   }

   public synchronized int getSize(){ 
      return q.size(); 
   }
}

public class Producer extends Thread{
   private Buffer b; 
   public Producer(Buffer b){ 
      this.b=b; 
   }

   public void run(){ 
      long x = 1; 
      while(true){ 
         b.putData(x); 
         int s = b.getSize(); 
         System.out.println("putting.. " + x + " - BufferSize=" + s); 
         x++; 
         try { 
            Thread.sleep(1000); //producimos cada un segundo 
         } catch (InterruptedException e) { 
            e.printStackTrace(); 
         } 
      } 
   }
}

public class Consumer extends Thread{
   private Buffer b; 
   public Consumer(Buffer b){ 
      this.b = b; 
   }
   
   public void run(){ 
      while(true){ 
         Long d = b.getData(); 
         int s = b.getSize(); 
         System.out.println(" getting.."+d+ " - BufferSize=" + s); 
         try { 
            Thread.sleep(1500); //consumimos cada segundo y medio 
         } catch (InterruptedException e) { 
            e.printStackTrace(); 
         } 
      } 
   }
}

public class ClientTest {
   public static void main(String[] args) { 
      Buffer b = new Buffer(); 
      Producer p = new Producer(b); 
      Consumer c = new Consumer(b); 
      p.start(); 
      c.start(); 
   }
} 
{% endhighlight %}

Como vemos nuestro sitio común al cual accesan `Producer` y `Consumer` es el `Buffer` y este debe tener los métodos 
sincronizados para que no exista corrupción del estado del buffer. Cada vez que el productor intenta colocar un dato se 
comprueba el tamaño del buffer, si esta al limite se espera. Cada vez que el consumidor intenta sacar un dato se verifica 
que no este vacío el buffer.

* [Hilos en Java - Parte 1]({% post_url 2011-03-30-hilos-en-java-parte-1 %} "Hilos en Java – Parte 1")
* [Hilos en Java - Parte 2]({% post_url 2011-04-15-hilos-en-java-parte-2 %} "Hilos en Java – Parte 2")
* [Hilos en Java - Parte 3]({% post_url 2011-10-16-hilos-en-java-parte-3 %} "Hilos en Java – Parte 3")
    