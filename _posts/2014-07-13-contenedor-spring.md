---
layout: post
title:  "Contenedor Spring"
date:   2014-07-13 08:00:04
categories: arquitectura framework
---

No voy a entrar en detalles teóricos sobre **Spring** ya que en los dos últimos artículos hablamos sobre _Inyección de Dependencias_ _y Contenedores_. Mencionare los conceptos adicionales necesarios para conocer mejor Spring framework. 

Spring básicamente permite hacer uso del paradigma de _IoC_(Inversión del Control) que ya habíamos visto.
En el artículo anterior vimos una lista de pasos para usar un contenedor en nuestra aplicación. Son los siguientes:

1.  Indicamos un punto de arranque para nuestra aplicación, un lanzador.
2.  Este lanzador carga o inicia el contenedor.
3.  El contenedor carga la configuración de nuestros objetos y dependencias.
4.  Le pedimos al contenedor que nos proporcione nuestra aplicación.

Usaremos las mismas clases del artículo anterior, `Office`, `Printer`, `DotMatrixPrinter` y `Main`.

&nbsp;

## Usando el API de Spring

El core de Spring a través de la interface `BeanFactory` proporciona todas las características de _IoC e Inyección de Dependencias_. La clase principal que implementa esta interface es `DefaultListableBeanFactory`. Esta sera nuestro "_Container_".

Spring no solo es un simple contenedor para inyectar dependencias, también ofrece otras características y servicios como _transaccionalidad_, _AOP_, _eventos_, _soporte i18n_, _contextos_ y mas. Para esto Spring extiende el _BeanFactory_ con la interface _ApplicationContext_ de la que hablaremos pronto.

Spring framework posee muchos modulos, pero aqui solo vamos a usar dos. El modulo core y el modulo beans. Las dependencias maven necesarias solo son:

{% highlight xml linenos %}
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-beans</artifactId>
   <version>4.0.5.RELEASE</version>
</dependency>
{% endhighlight %}

&nbsp;

Manualmente se necesita agregar al proyecto los siguientes jars:

* **spring-core-4.0.5.RELEASE.jar**
* **spring-beans-4.0.5.RELEASE.jar**
* **commons-logging-1.1.3.jar**

&nbsp;

### Paso 1, punto de arranque

Nuestro punto de inicio o lanzador será un clase `Main` igual que el ejemplo del artículo anterior.

### Paso 2, el lanzador inicia el contenedor

Aquí usamos la clase de Spring que indicamos anteriormente, `DefaultListableBeanFactory`.

{% highlight java linenos %}
DefaultListableBeanFactory container = new DefaultListableBeanFactory();
{% endhighlight %}

### Paso 3, cargar la configuración al contenedor

Aquí tenemos varios sub-pasos, primero debemos especificar nuestra configuración. Spring permite esto a través de un archivo de configuración xml o a través de anotaciones. Usaremos el xml y lo llamaremos `my-beans.xml`. 
En Spring nuestros objetos se conocen como _Beans_.

{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="myDotMatrixPrinter" class="com.test.spc.DotMatrixPrinter" />

   <bean id="office" class="com.test.spc.Office">
      <constructor-arg ref="myDotMatrixPrinter" />
   </bean>

</beans>
{% endhighlight %}

&nbsp;

El elemento `<bean ...>` permite definir mis objetos.
Ahora cargamos nuestro xml y se lo pasamos al contenedor. Usaremos clases de Spring para ayudarnos. Primero usamos un cargador de recursos con la interface `ResourceLoader`. Luego un lector de xml con `XmlBeanDefinitionReader`.

{% highlight java linenos %}
ResourceLoader resourceLoader = new DefaultResourceLoader();
Resource resource = resourceLoader.getResource("classpath:my-beans.xml");
XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(container);
beanDefinitionReader.loadBeanDefinitions(resource);
{% endhighlight %}

### Paso 4, pedimos nuestra aplicación(bean) al contenedor

{% highlight java linenos %}
Office officeBean = container.getBean(Office.class);
officeBean.setMessageToPrint("Hola mundo!!");
officeBean.print();
{% endhighlight %}

&nbsp;

## Uniendo todo

{% highlight java linenos %}
//1.
public class Main {

   public static void main(String[] args) {

      //2. 
      DefaultListableBeanFactory container = new DefaultListableBeanFactory();

      //3.
      ResourceLoader resourceLoader = new DefaultResourceLoader();
      Resource resource = resourceLoader.getResource("classpath:my-beans.xml");

      XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(container);
      beanDefinitionReader.loadBeanDefinitions(resource);

      //4.
      Office officeBean = container.getBean(Office.class);
      Office officeBean2 = container.getBean(Office.class);
      officeBean.setMessageToPrint("Hola mundo!!");
      officeBean.print();
   }
}
{% endhighlight %}

&nbsp;

## Detalles de configuración del xml

Los atributos importantes en el elemento `<bean>` son `class` que permite especificar la clase y el `id` que se usa como identificador y permite referenciar el bean en todo el xml. En esta configuración usamos la _inyección por constructor_ con el elemento `<constructor-arg>` y con el atributo `ref` definimos cual id de bean es la dependencia.
También podemos inyectar dependencias por un método setter. De esta manera debemos usar el atributo `<property>` así:

{% highlight xml linenos %}
<property name="printer" ref="myDotMatrix" />
{% endhighlight %}

&nbsp;

Nuestra clase `Office` debe definir un método setter:

{% highlight java linenos %}
public class Office {

   private Printer printer;

   public void setPrinter(Printer printer) {
      this.printer = printer;
   }

   //mas codigo...
}
{% endhighlight %}

&nbsp;

## Scope

Otro punto importante para entender es que Spring cada vez que le pides un bean te retorna el mismo objeto ya que por defecto crea una sola instancia de cada bean, es lo que se conoce como scope _**singleton**_. Si queremos una instancia diferente cada vez que hacemos `getBean()` definimos el bean como scope _**prototype**_:

{% highlight xml linenos %}
<bean id="office" class="com.test.spc.Office" scope="prototype">...
{% endhighlight %}

&nbsp;

## Historia de Spring

Spring fue lanzado en 2004 y nació como una alternativa a las primeras versiones de _**Enterprise JavaBeans**_(EJB). Se define como un contenedor _liviano no intrusivo_ ya que nuestras clases no necesitan extender ni implementar nada del framework. Cosa que si era necesaria con EJB. Y el nombre de los beans de Spring viene de Enterprise JavaBeans.