---
layout: post
title:  "Contenedores e Inyección de Dependencias [01]"
date:   2014-07-08  09:24:00
categories: arquitectura conceptos
---

Voy a abordar este tema de una forma sencilla, puntual y fácil ya que próximamente hablaremos de _**Java EE**_ y _**Spring framework**_ y los conceptos que toquemos deben quedar bastante claros.

Cuando desarrollamos una aplicación esta consta de muchos objetos. Estos colaboran entre si, un objeto usa otro para realizar algún trabajo. Aquí tenemos lo que llamamos _dependencia_. Una dependencia es un objeto necesitado por otro. Ahora una dependencia como la obtiene quien la necesita?. Es instanciada directamente por quien la necesita?

{% highlight java linenos %}
public interface Persister{
   public void save(Libro libro)
}

public class DBPersister implements Persister{

   public void save(Libro libro){
      //Codigo para guardar en DB..
   }

}

public class Libro{
   private String ISBN;
   private String title;
   private int year;
   private Persister p = new DBPersister();
   // mas datos...

   public void guardar(){
      p.save(this);
   }
}

public class Prueba{

   public static void main(String[] args){
      Libro l = new Libro(); //Datos del libro
      l.guardar();
   }

}
{% endhighlight %}

&nbsp;

**Noooo!**. Al instanciar un objeto dentro de otro(`new DBPersister()`) estamos creando _acoplamiento_. Así lo que tenemos es un sistema rígido. Aunque durante mucho tiempo la gente lo hizo así, esto llevó a que surgieran patrones y técnicas que mejoran la conexión entre objetos. Uno de los patrones que surgieron es el que conocemos como **_Inyección de Dependencias_**, esta promueve la modularidad y el reuso de componentes.

&nbsp;

## Inyección de Dependencias

Esta emerge de un principio denominado **_Inversion del Control_** o **_IoC_**. Que en resumen lo que permite es indicar quien controla a quien. Y porque la Inyección de Dependencias emerge de el? Pues porque el objeto que necesita la dependencia ya no es quien la instancia(controla), esa tarea la lleva a cabo alguien mas. Ahi se esta invirtiendo el control. La _**IoC**_ abarca otras areas también que no mencionaremos ahora.
El otro enfoque para manejo de las dependencias es el conocido _**Service Locator**_, del cual no hablaremos aquí.

La Inyección de Dependencias mejora mucho nuestra arquitectura y toma los conceptos de la **_Inversion de Dependencias_**(principio **S.O.L.I.D**) que dice que _NO debemos depender de clases concretas_ si no de abstracciones que nos ayudan a desacoplar nuestros objetos.

&nbsp;

## Formas de inyectar dependencias

Básicamente encontramos 2 formas, una a través del _**constructor**_ y otra a través de un _**método**_(mayormente un _setter_). Usando el mismo ejemplo lo que vamos a cambiar es crear la inyección por el constructor o por el setter en la clase `Libro`.

{% highlight java linenos %}
public class Libro{
   private String ISBN;
   private String title;
   private int year;
   private Persister p;
   // mas datos...

   // Con este setter pasamos el Persister concreto
   public void setPersister(Persister persister){
      this.persister = persister;
   }

   public void guardar(){
      p.save(this);
   }
}

public class Prueba{

   public static void main(String[] args){
      Libro l = new Libro(); //Datos del libro
      DBPersister p = new DBPersister();
      l.setPersister(p);
      l.guardar();
   }

}
{% endhighlight %}

&nbsp;

Aquí quitamos el `new DBPersister()` y cremos un metodo setter para pasar el `Persister` a la clase `Libro`.

Cuando hablábamos de la inversion del control, decíamos que alguien mas controlaba la dependencia. Quien crea la instancia y la inyecta?. En el ejemplo anterior lo hicimos manualmente nosotros y es aquí donde aparecen los _**contenedores**_.

&nbsp;

## Contenedor

Un contenedor básicamente lo podemos ver como un almacén de objetos. También nos permite manejar el _**ciclo de vida**_ de estos, es decir, la creación, organización, recuperación y destrucción de los objetos. Los contenedores simplifican y hacen mas cómodo el trabajo con las dependencias ya que se encargan de crear todas la conexiones que indiquemos entre los diferentes objetos. De hecho nosotros podemos crear un pequeño contenedor para gestionar nuestros objetos, aunque claro es un trabajo que necesita un poco de dedicación.

Hay contenedores como **Google Guice** que se enfoca principalmente en gestionar dependencias y otros que nos ofrecen mucho mas, como el caso de **Spring** que posee un stack de tecnologías que nos facilitan el desarrollo de aplicaciones por la gran cantidad de APIs que nos ofrece. Esto permite que nos concentremos mas en nuestra lógica de negocio.

Cada contenedor se configura y se usa de forma particular, pero eso es tema para otra entrada.