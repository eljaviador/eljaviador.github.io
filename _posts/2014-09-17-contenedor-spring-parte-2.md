---
layout: post
title:  "Contenedor Spring [02]"
date:   2014-09-17 20:01:05
published: true
categories: [frameworks]
tags: [conceptos, arquitectura, spring]
comments: true
shortinfo: Segunda parte para comprender el popular framework Spring.
---

Antes de leer este post, te recomiendo los anteriores para un mejor entendimiento:

* [_**Contenedores e Inyección de Dependencias 01**_]({% post_url 2014-07-08-contenedores-e-inyeccion-de-dependencias-parte-1 %})
* [_**Contenedores e Inyección de Dependencias 02**_]({% post_url 2014-07-11-contenedores-e-inyeccion-de-dependencias-parte-2 %})
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})

En la [_**parte 1**_]({% post_url 2014-07-13-contenedor-spring %}) vimos como usar Spring siguiendo unos pasos lógicos.
Una de esos pasos era el número 3 donde definiamos nuestros objetos(beans) y luego cargabamos esas definiciones en el contenedor.
Esto lo hicimos con xml. Ahora veamos como hacer lo mismo sin usar xml, solo registrando nuestros objetos.

Como ya vimos si usas _Maven_ la unica dependencia a incluir en el `pom.xml` es `spring-beans` ya que por transitividad se
trae `spring-core` y `commons-logging`.

{% highlight xml linenos %}
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-beans</artifactId>
   <version>4.0.5.RELEASE</version>
</dependency>
{% endhighlight %}<br/>

Sis los haces manualmente se necesita agregar al proyecto los siguientes jars:

* **`spring-core-4.0.5.RELEASE.jar`**
* **`spring-beans-4.0.5.RELEASE.jar`**
* **`commons-logging-1.1.3.jar`**

## Moficando el paso 3
Aquí ya no necesitamos el xml y también tenemos que modificar el codigo Java. Tenemos que crear las mismas definiciones del 
xml pero con codigo Java. Veamos como nos queda este paso 3:

{% highlight java linenos %}
//Creamos dos definiciones de beans

//Definicion para DotMatrixPrinter
GenericBeanDefinition dotMatrixPrinterDef = new GenericBeanDefinition();
dotMatrixPrinterDef.setBeanClass(DotMatrixprinter.class);

//Definicion para Office
GenericBeanDefinition officeDef = new GenericBeanDefinition();
officeDef.setBeanClass(Office.class);

//Recordemos que Office recibe por constructor una referencia de Printer
//asi que definimos esta relacion con ConstructorArgumentValues
ConstructorArgumentValues officeConstructorValues = new ConstructorArgumentValues();
officeConstructorValues.addGenericArgumentValue(dotMatrixPrinterDef);

//Establecemos dicha relacion con la definición de Office
officeDef.setConstructorArgumentValues(officeConstructorValues);

//Registramos nuestras deficiones en el contenedor
container.registerBeanDefinition("dotMatrixPrinter", dotMatrixPrinterDef);
container.registerBeanDefinition("office", officeDef);
{% endhighlight %}<br/>

Ya estamos listos para usar nuestro contenedor.


## Uniendo todo

{% highlight java linenos %}
//1.
public class Main {

   public static void main(String[] args) {

      //2. 
      DefaultListableBeanFactory container = new DefaultListableBeanFactory();

      //3.
      GenericBeanDefinition dotMatrixPrinterDef = new GenericBeanDefinition();
      dotMatrixPrinterDef.setBeanClass(DotMatrixprinter.class);

      GenericBeanDefinition officeDef = new GenericBeanDefinition();
      officeDef.setBeanClass(Office.class);

      ConstructorArgumentValues officeConstructorValues = new ConstructorArgumentValues();
      officeConstructorValues.addGenericArgumentValue(dotMatrixPrinterDef);

      officeDef.setConstructorArgumentValues(officeConstructorValues);

      container.registerBeanDefinition("dotMatrixPrinter", dotMatrixPrinterDef);
      container.registerBeanDefinition("office", officeDef);

      //4.
      Office officeBean = container.getBean(Office.class);
      Office officeBean2 = container.getBean(Office.class);
      officeBean.setMessageToPrint("Hola mundo!!");
      officeBean.print();
   }
}
{% endhighlight %}<br/>

Esta es una de las tantas formas de usar Spring, en este caso sin XML solo con nuestras clases y código Java.

## Código Fuente
[Código fuente en gitHub](https://github.com/eljaviador/spring-02)
 
## Definir Dependencias por XML
Cuando tenemos dependencias entre nuestras clases y usamos xml, creamos estas relaciones dentro el tag `<bean>`. Veamos algunos
ejemplos.<br/><br/>

#### Inyección por referencia a través del constructor

{% highlight java linenos %}
package x.y;
public class Foo {
  public Foo(Bar bar, Baz baz) {//...}
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="foo" class="x.y.Foo">
    <constructor-arg ref="bar"/>
    <constructor-arg ref="baz"/>
</bean>

<bean id="bar" class="x.y.Bar"/>
<bean id="baz" class="x.y.Baz"/> 
{% endhighlight %}<br/>
 
#### Inyección por tipo a través del constructor

{% highlight java linenos %}
package examples;
public class ExampleBean {
  private int years;
  private String ultimateAnswer;

  public ExampleBean(int years, String ultimateAnswer) {
      this.years = years;
      this.ultimateAnswer = ultimateAnswer;
  }
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
{% endhighlight %}<br/>

#### Inyección por indice(posición del parámetro) a través del constructor

{% highlight xml linenos %}
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
{% endhighlight %}<br/>

#### Inyección por nombre de argumento a través del constructor

{% highlight xml linenos %}
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
{% endhighlight %}<br/>

Tambien podemos definir nuestras dependencias a través de atributos o setters de la clase. Para esto usamos el tag `<property>`
dentro de `<bean>`. Este permite las siguientes formas:

* `<property name=”propiedad” value=”...” />`
* `<property name=”propiedad” ref=”...” />`<br/><br/>

#### Inyección por setter

{% highlight java linenos %}
public class ExampleBean {

  private BeanUno beanOne;
  private BeanDos beanTwo;
  private int i;

  public void setBeanOne(BeanUno beanOne) {
      this.beanOne = beanOne;
  }

  public void setBeanTwo(BeanDos beanTwo) {
      this.beanTwo = beanTwo;
  }

  public void setIntegerProperty(int i) {
      this.i = i;
  }
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="exampleBean" class="examples.ExampleBean">
    <property name="beanOne" ref="beanUno"/>
    <property name="beanTwo" ref="beanDos"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="beanUno" class="examples.BeanUno"/>
<bean id="beanDos" class="examples.BeanDos"/>
{% endhighlight %}<br/>

## Definir dependencias por código
En la parte inicial vimos como usar la clase `ConstructorArgumentValues` para definir nuestras dependencias a través del 
constructor. También podemos definir dependencias por setter con la clase `MutablePropertyValues`. Imagenemos que nuestra
clase `Office` no recibe un `Printer` a través del constructor si no de un setter `setPrinter()`, nuestro código seria algo
asi:

{% highlight java linenos %}
MutablePropertyValues printerSetter = new MutablePropertyValues();
printerSetter.add("printer", dotMatrixPrinterDef);
officeDef.setPropertyValues(printerSetter);
{% endhighlight%}<br/>

Algo interesante con `MutablePropertyValues` es que podemos agregarle varias propiedades o setter por ejemplo:

{% highlight java linenos %}
MutablePropertyValues printerSetter = new MutablePropertyValues();
printerSetter.add("printer", dotMatrixPrinterDef);
printerSetter.add("setter1", algunValor);
printerSetter.add("setter2", otroValor);
officeDef.setPropertyValues(printerSetter);
{% endhighlight%}<br/>


Bueno espero este artículo haya ampliado tu comprensión sobre Spring, contenedores y dependencias. 


