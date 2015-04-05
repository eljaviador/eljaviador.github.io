---
layout: post
title:  "S.O.L.I.D Principio de Inversion de Dependencias"
date:   2011-08-30  13:28:00
categories: arquitectura
---

Ahora veamos el quinto y último de los principios [_**S.O.L.I.D**_]({% post_url 2011-03-14-principios-solid %}).

Este principio dice :

> A. High level modules should not depend upon low level modules. Both should depend upon abstractions - Modulos de alto 
nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.

> B. Abstractions should not depend upon details. Details should depend upon abstractions. - Las abstracciones no deben 
depender de los detalles. Los detalles deben depender de las abstracciones.

Cuando tenemos asociaciones en nuestro sistema con el objetivo de realizar un tarea o de cumplir un objetivo, aparece la 
necesidad de usar partes dentro de partes para realizar esto, es aquí donde entra en acción la dependencia. Una cosa depende 
o se ayuda de otra para realizar el trabajo o parte de este.

Robert Martin dice que basándonos en el principio de [**Substitución de Liskov**]({% post_url 2011-06-29-SOLID-principio-de-sustitucion-de-liskov %}) 
y el principio [**Abierto / Cerrado**]({% post_url 2011-05-16-SOLID-principio-abierto-cerrado %}), 
la estructura resultante la podemos generalizar en un principio que llama **_Inversión de Dependencias_**.

Tenemos una entidad o clase que queremos persistir, ahora bien podemos usar por ejemplo una clase `Libro` :

{% highlight java linenos %}
public class Libro{ 
   private String ISBN; 
   private String title; 
   private int year; 
   
   // mas datos...
} 
{% endhighlight %}<br/>

Usaremos un clase persistidora(bajo nivel) que nos permita guardar un objeto libro en una base de datos:

{% highlight java linenos %}
public class DBPersister{
   public void save(Libro libro){ 
      //Codigo para guardar.. 
   }
}

public class Libro{ 
   private String ISBN; 
   private String title; 
   private int year; 
   private DBPersister p = new DBPersister(); 
   
   // mas datos...
   public void guardar(){ 
      p.save(this); 
   } 
}
{% endhighlight %}<br/>

La clase `Libro` usara este persistidor internamente para realizar el trabajo de enviar la información de un objeto libro a la bd. 
Pero aquí nos llega el inconveniente de si luego quiero guardar la información del libro en un archivo xml por ejemplo tengo 
una dependencia basada en una clase concreta y no en una abstracción.

## Aplicando la Inversion de Dependencia

Rediseñando esta pequeña estructura aplicaremos una abstracción ya sea con una Interface o clase abstracta. En este caso 
usaremos un Interface llamada `Pesister` que nos especifique los métodos necesarios para la abstracción y después dos clases 
que la implementen.

{% highlight java linenos %}
public interface Persister{ 
   public void save(Libro libro) 
}

public class DBPersister implements Persister{
   public void save(Libro libro){ 
      //Codigo para guardar en DB.. 
   }
}

public class XMLPersister implements Persister{
   public void save(Libro libro){ 
      //Codigo para guardar en XML.. 
   }
} 
{% endhighlight %}<br/>

Ahora en vez de una clase concreta usaremos una interface dentro de la clase `Libro` y luego desde afuera le pasamos al Libro 
la implementación necesaria, sea la de `DBPersister` o la `XMLPersister`.

{% highlight java linenos %}
public class Libro{ 
   private String ISBN; 
   private String title; 
   private int year; 
   private Persister persister; 
   
   // mas datos...
   // Con este setter pasamos el Persister concreto 
   public void setPersister(Persister persister){ 
      this.persister = persister; 
   } 
   
   public void guardar(){ 
      persister.save(this); 
   } 
}

public class Prueba { 
   public static void main(String[] args){ 
      Libro l = new Libro(); 
      //Datos del libro 
      XMLPersister p = new XMLPersister(); 
      l.setPersister(p); 
      l.guardar(); 
   }
}
{% endhighlight %}<br/>

Aquí `Libro` es nuestra clase de nivel alto y los persistidores nuestras clases de nivel bajo y basándonos en una abstracción 
llamada `Persister` aplicamos este principio.