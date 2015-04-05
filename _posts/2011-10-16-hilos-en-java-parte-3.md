---
layout: post
title:  "Hilos en Java - [03]"
date:   2011-10-16  13:28:00
categories: javase
---

Tercera entrega sobre hilos en Java.

## Sincronizacion de Hilos

Cuando tenemos una Instancia de una clase, osea un objeto creado en el heap es posible que dos o mas hilos quieran acceder a 
ese mismo objeto o instancia al tiempo (o mas o menos al tiempo). Ahora bien el problemita aparece cuando se necesita trabajar 
con el estado(atributos) de esa instancia. 

Recordando un poco lo que hablábamos en la primera parte de hilos, estos tienen 
una copia propia de cada método pero comparten el estado del objeto, así que si un hilo modifica un atributo el otro vera el 
cambio y puede afectar el resultado que esperan cada uno de esos hilos.

Como prevenir este comportamiento?. Pues hacemos dos cosas:

1.  Colocar los atributos con visibilidad `private` y accederlos con un método.
2.  Sincronizar el método o fragmento de código que modifica los atributos.

La sincronización funciona con un bloqueo o **_lock_**. Cada objeto posee un bloqueo o lock. Cuando decimos _**cada objeto**_, 
nos referimos a que cada instancia particular de una clase tiene un lock y entra en acción cuando hay métodos sincronizados.

La sincronizacion significa que un hilo no puede entrar al método o fragmento de código sincronizado hasta que el otro hilo que 
esta usando ese código salga de el, osea hasta que suelte el bloqueo o lock. Cuando un hilo entra a un método sincronizado obtiene 
el lock de ese objeto y los otros hilos no pueden entrar a ningun metodo sincronizado hasta que el hilo que tiene el lock lo suelte. 

Así protegemos que un hilo particular modifique el estado y afecte el resultado de otro.

Tengamos en cuenta los siguientes puntos sobre sincronizacion:

*   Solo se sincroniza métodos o bloques de código, **_no variables ni clases_**.
*   Cada objeto tiene **_un solo lock_** o bloqueo.
*   No es necesario sincronizar todos los métodos.
*   Varios hilos pueden acceder a diferentes métodos **_no sincronizados_** en el mismo objeto.
*   Si un hilo se va a dormir **_no suelta el lock_**.
*   Se pueden **_obtener varios lock_** si métodos sincronizados llaman otros métodos sincronizados en otros objetos.

## Sincronizando métodos no estáticos también llamados métodos de instancia.
La sincronizacion se lleva a cabo con la palabra `synchronized`.

{% highlight java linenos %}
public synchronized void metodo1(){ 
   //codigo que manipula el estado 
} 
{% endhighlight %}<br/>

Es recomendable solo sincronizar el método que manipula el estado. De hecho es mas recomendable también solo sincronizar el 
bloque de código particular que manipula el estado aunque tiene el mismo efecto es mas flexible:

{% highlight java linenos %} 
public void metodo1(){ 
   //otro codigo 
   synchronized (this){ 
      //codigo que manipula el estado 
   } 
   // mas codigo 
}
{% endhighlight %}<br/>

Usando la palabra `this` obtenemos el lock del objeto actual y se lo pasamos a `synchronized`.

## Sincronizando métodos estáticos también llamados métodos de clase.
Se pueden tener muchas instancias u objetos de una clase y cada instancia tendrá su propio lock. Pero solo existe una sola 
instancia de la clase como tal en la JVM, es de decir existe una sola instancia de `java.lang.Class` por cada clase de mi 
programa cargada por la maquina virtual. Así como se obtiene el bloqueo del objeto actual usando `this` en métodos 
no estáticos, usamos la única instancia de la clase para obtener el bloqueo en métodos estáticos.

Veamos las formas de realizar esto:

{% highlight java linenos %}
public class Cliente {
   public static synchronized void metodo1(){ 
      //data a proteger... 
   }
   public static void metodo2(){ 
      //otro codigo 
      synchronized (Cliente.class){ 
         //data a proteger... 
      } 
      // mas codigo
   }
   
   public static void metodo3(){ 
      //otro codigo 
      try { 
         Class instancia = Class.forName("paquete.Cliente"); 
         synchronized (instancia) { 
            //data a proteger... 
         } 
      } catch (ClassNotFoundException ex) { 
         ex.printStackTrace(); 
      } 
      // mas codigo 
   }
} 
{% endhighlight %}<br/>

Ahí vimos las 3 formas de sincronizar métodos estáticos.

Por simplicidad en el diseño se recomienda que los atributos estáticos sean accedidos por métodos estáticos y los atributos 
no estáticos sean accedidos por métodos no estáticos. El bloqueo de clase(sobre métodos estáticos) es diferente al bloqueo de 
instancia(sobre métodos no estáticos), por ende un hilo puede acceder a un método estático y otro hilo a un método no estático 
y no bloquearse mutuamente.

* [Hilos en Java - Parte 1]({% post_url 2011-03-30-hilos-en-java-parte-1 %} "Hilos en Java – Parte 1")
* [Hilos en Java - Parte 2]({% post_url 2011-04-15-hilos-en-java-parte-2 %} "Hilos en Java – Parte 2")