---
layout: post
title:  "S.O.L.I.D Principio de Segregacion de Interfaces"
date:   2011-07-15  13:28:00
published: true
categories: [conceptos y principios]
tags: [principios, poo]
shortinfo: Cuarta entrega sobre los pricipios S.O.L.I.D para desarrollo de software.
---

Seguimos con el cuarto de los principios [_**S.O.L.I.D**_]({% post_url 2011-03-14-principios-solid %}).

Este principio introducido por el propio Robert Martin se define de la siguiente forma:

> Clients should not be forced to depend upon interfaces that they do not use. - Clientes no deben ser forzados a depender 
de interfaces que no usen.

Este principio nos habla de las interfaces pesadas o contaminadas, esas que tienen una gran cantidad de métodos. Si un 
cliente implementa esa interface pues tendrá que declarar métodos que no necesita y los cuales estarán evidentemente vacíos.

Cuando esto sucede es porque tenemos interfaces que no son cohesivas y lo mejor es dividirlas de manera funcional, es decir 
vamos a agrupar los métodos según su funcionalidad, así el cliente solo implementara la interface que necesite realmente. 
Pienso que es mejor hacer un esfuerzo adicional en nuestra fase de diseño a fin de tener un estructura mas **_funcional y 
flexible_** en nuestro software.

Pasemos a un ejemplo:

{% highlight java linenos %} 
public interface Worker{ 
   void trabajar(); 
   void descansar(); 
} 

public class Trabajador implements Worker {
   public void trabajar() { 
      // Codigo de trabajar 
   }
   
   public void descansar() { 
      // Codigo de descansar 
   } 
}
{% endhighlight %}<br/>

Aquí la empresa tiene trabajadores que harán operaciones como trabajar y descansar. 

Bien que pasaría si ahora la empresa tiene un robot que implementa esta interface `Worker`, entonces el robot esta obligado a 
tener un método `descansar()` que evidentemente no usara. Pues aquí lo que podemos hacer es aplicar este principio de segregar 
las interfaces. 

Usaremos una para trabajar y otra para descansar.

{% highlight java linenos %}
public interface Workable{ 
   void trabajar(); 
} 

public interface Pausable{ 
   void descansar(); 
} 

public class Trabajador implements Workable, Pausable{
   public void trabajar() { 
      // Codigo de trabajar 
   } 
   
   public void descansar() { 
      // Codigo de descansar 
   } 
}

public class Robot implements Workable{
   public void trabajar() { 
      // Codigo de trabajar 
   } 
} 
{% endhighlight %}<br/>

Como vemos hemos separado las interfaces, nuestro trabajador implementara las dos necesarias para el y el robot solo la 
que necesita en este caso la de `trabajar()`.