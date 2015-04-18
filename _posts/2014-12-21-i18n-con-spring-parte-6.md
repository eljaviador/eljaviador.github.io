---
layout: post
title:  "Internacionalización con Spring [06]"
date:   2014-12-21 21:00:00
published: true
categories: [frameworks]
tags: [conceptos, arquitectura, spring]
comments: true
shortinfo: Sexta parte de la saga comprender Spring framework.
---

Antes de leer este post, te recomiendo los anteriores para un mejor entendimiento:

* [_**Contenedores e Inyección de Dependencias 01**_]({% post_url 2014-07-08-contenedores-e-inyeccion-de-dependencias-parte-1 %})
* [_**Contenedores e Inyección de Dependencias 02**_]({% post_url 2014-07-11-contenedores-e-inyeccion-de-dependencias-parte-2 %})
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})
* [_**Contenedor Spring 02**_]({% post_url 2014-09-17-contenedor-spring-parte-2 %})
* [_**Contenedor Spring 03**_]({% post_url 2014-10-25-contenedor-spring-parte-3 %})
* [_**Inyectar Colecciones en Spring 04**_]({% post_url 2014-11-15-inyectando-colecciones-con-spring-parte-4 %})
* [_**Ciclo de Vida con Spring 05**_]({% post_url 2014-12-06-ciclo-de-vida-con-spring-parte-5 %})

Habiamos comentado en el post #1 de Spring que este también ofrece otras características y servicios como transaccionalidad, 
AOP, eventos, soporte i18n, contextos y mas. Y que para esto Spring extiende el `BeanFactory` con la interface `ApplicationContext`.

Ahora todas estas características extras que ofrece `ApplicationContext` con respecto a `BeanFactory` que son y para que 
me sirven?. Veamos en detalles cada una. 


## Internacionalización (i18n)
La interface `ApplicationContext` extiende de `MessageSource` para proporcionar estas características. Es necesario definir
un bean del tipo `MessageSource` y el `ApplicationContext` delega en el la busqueda de mensajes con el método `getMessage()`.

Las implemantaciones de `MessageSource` son:

* `StaticMessageSource` implementación simple que permite registro programatico de mensajes.
* `ResourceBundleMessageSource` implementación que accesa recursos bundle con basename específicos.
* `ReloadableResourceBundleMessageSource` implementacion que puede recargar en caliente mensajes.

<br/>

#### ApplicationContext
Ahora que ya conocemos una de las características que proporciona `ApplicationContext` vamos a usarla.
La interface `ApplicationContext` tiene varias implementaciones:

* `ClassPathXmlApplicationContext`
* `FileSystemXmlApplicationContext`
* `GenericXmlApplicationContext`
* `AnnotationConfigApplicationContext`<br/>

Haremos los mismos pasos que con `DefaultListableBeanFactory`. En este momento usaremos `FileSystemXmlApplicationContext` 
y `ClassPathXmlApplicationContext`. La diferencia radica en que el `FileSystemXmlApplicationContext` le damos una ruta o 
ubicación donde esta nuestro archivo de configuración xml con los beans y con `ClassPathXmlApplicationContext` usamos el 
classpath para dar la ruta. 

{% highlight java linenos %}
//1. Punto de arranque
public class Main {

   public static void main(String[] args) {

      //2. y 3. Van juntos. 
      FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext("C:\TU_RUTA\myBeans.xml");

      //4.
      Office officeBean = context.getBean(Office.class);
      officeBean.setMessageToPrint("Hola mundo!!");
      officeBean.print();
   }
}
{% endhighlight %}<br/>

Como vemos es mas reducido el código que cuando usamos `BeanFactory`. <br/><br/>

#### Aplicando i18n
Tenemos un archivo `mensajes.properties` asi:

{% highlight xml linenos %}
saludo=Hola mundo!
{% endhighlight %}<br/>

Tenemos el mismo archivo para otros idiomas, por ejemplo para inglés es `mensajes_en.properties`.

{% highlight xml linenos %}
saludo=Hello world!
{% endhighlight %}<br/>

Configuramos un bean de tipo `MessageSource` en nuestro xml así:

{% highlight xml linenos %}
<bean id="msgSource" class="org.springframework.context.support.ResourceBundleMessageSource">
   <property name="basenames">
      <list>
         <value>mensajes</value>
      </list>
   </property>
</bean>
{% endhighlight %}<br/>

Para usarlo hacemos esto:


{% highlight java linenos %}
public class Main {

   public static void main(String[] args) {
 
      FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext("TU_RUTA/myBeans.xml");

      String message = context.getMessage("saludo", null, "Mensaje por defecto", Locale.ENGLISH);
      System.out.println(message);
   }
}
{% endhighlight %}<br/>

Como vemos usar las otras características de Spring no es tan complicado. 
