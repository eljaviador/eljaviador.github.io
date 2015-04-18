---
layout: post
title:  "Contenedor Spring [03]"
date:   2014-10-25 07:30:00
published: true
categories: [frameworks]
tags: [conceptos, arquitectura, spring]
comments: true
shortinfo: Tercera entrega sobre el popular framework y contenedor Spring.
---

Antes de leer este post, te recomiendo los anteriores para un mejor entendimiento:

* [_**Contenedores e Inyección de Dependencias 01**_]({% post_url 2014-07-08-contenedores-e-inyeccion-de-dependencias-parte-1 %})
* [_**Contenedores e Inyección de Dependencias 02**_]({% post_url 2014-07-11-contenedores-e-inyeccion-de-dependencias-parte-2 %})
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})
* [_**Contenedor Spring 02**_]({% post_url 2014-09-17-contenedor-spring-parte-2 %})

## Otras Configuraciones
Spring se adapta a nuestros objetos permitiendo configuraciones especificas.<br/><br/>


#### Instanciando con Static Factory Method
En algunos casos tenemos clases que tiene el constructor privado y usamos un metodo `static` para obtener la instancia de dicha
clase:

{% highlight java linenos %}
public class ClientService {
   
   private static ClientService clientService = new ClientService();
   
   //Constructor privado
   private ClientService() {}

   //Metodo static para instanciar la clase
   public static ClientService createInstance() {
      return clientService;
   }
}      
{% endhighlight %}

{% highlight xml linenos %}
<bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>
{% endhighlight %}<br/>

#### Instanciando con Instance Factory Method
Otro caso cuando tenemos un bean que instancia otros beans:

{% highlight java linenos %}
//Bean para instanciar otros beans
public class DefaultServiceLocator {
   
   private static ClientService clientService = new ClientServiceImpl();

   //Constructor privado
   private DefaultServiceLocator() {}

   //Metodo de instancia para instanciar
   public ClientService createClientService() {
      return clientService;
   }
}
      
{% endhighlight %}

{% highlight xml linenos %}
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
  <!-- cualqueir dependencia requirida -->
</bean>

<!-- El bean a crear via factory bean -->
<bean id="clientService" factory-bean="serviceLocator" factory-method="createClientService"/>
{% endhighlight %}<br/>

#### Factory Method con Argumentos
Aqui podemos pasar argumentos al método factory:

{% highlight java linenos %}
public class ExampleBean {

  private ExampleBean(...) { //... }

  public static ExampleBean createInstance (BeanUno beanUno, BeanDos beanDos, int i) {
      ExampleBean eb = new ExampleBean (...);
      // some other operations...
      return eb;
  }
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="beanUno"/>
    <constructor-arg ref="beanDos"/>
    <constructor-arg value="1"/>
</bean>

<bean id="beanUno" class="examples.BeanUno"/>
<bean id="beanDos" class="examples.BeanDos"/>
{% endhighlight %}<br/>

#### Dependencias Indirectas
Algunas veces necesitamos que un bean se instancie después de otro aunque estos no sean dependencias directas. Por alguna razón
queremos un orden específico de inicialización. Aquí usamos el atributo `depends-on`.

{% highlight xml linenos %}
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />

<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
{% endhighlight %}<br/>


#### Inicialización Lazy
Por defecto los bean se inician todos al levantar el contenedor. Es posible iniciar un bean solo cuando sea llamado. Aunque 
si un bean lazy es necesitado por uno eager(que se crea al levantar el contendor), el bean lazy será pre-inicializado.

{% highlight xml linenos %}
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
{% endhighlight %}<br/>



