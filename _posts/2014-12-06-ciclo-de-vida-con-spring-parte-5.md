---
layout: post
title:  "Ciclo de Vida con Spring [05]"
date:   2014-12-06 12:25:00
published: true
categories: [frameworks]
tags: [conceptos, arquitectura, spring]
comments: true
shortinfo: Quinta entrega para comprender el popular framework y contenedor Spring.
---

Antes de leer este post, te recomiendo los anteriores para un mejor entendimiento:

* [_**Contenedores e Inyección de Dependencias 01**_]({% post_url 2014-07-08-contenedores-e-inyeccion-de-dependencias-parte-1 %})
* [_**Contenedores e Inyección de Dependencias 02**_]({% post_url 2014-07-11-contenedores-e-inyeccion-de-dependencias-parte-2 %})
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})
* [_**Contenedor Spring 02**_]({% post_url 2014-09-17-contenedor-spring-parte-2 %})
* [_**Contenedor Spring 03**_]({% post_url 2014-10-25-contenedor-spring-parte-3 %})
* [_**Inyectar Colecciones en Spring 04**_]({% post_url 2014-11-15-inyectando-colecciones-con-spring-parte-4 %})

## Ciclo de Vida
No solo en Spring, también en otras tecnologías como **Java EE** nos vamos a encotrar con este concepto. Para resumir es el 
_**nacimiento**_, _**vida**_ y _**muerte**_ de tus objetos en el contendor. Esto es cuando el contenedor arranca, crea los objetos, luego tu 
los usas y por último al detener el contenedor este destruye tus objetos.

Existen situaciones en que necesitamos trabajar sobre ese ciclo de vida, por ejemplo si tenemos un bean de acceso a base de
datos podemos instanciar nuestras conexiones al momento de que se crea el bean y luego al detener el contenedor podemos 
liberar recursos. Usando métodos e indicandole a Spring cuales son para que los ejecute justo al cerar el bean y al destruirlo.

Aquí tenemos una clase con un método para inicializar cosas y otro para destruiro y liberar recursos.

#### Para Inicialización
{% highlight java linenos %}
public class ExampleBean {
  public void init() {
      // Código de inicialización
  }
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
{% endhighlight %}<br/>

#### Para Destrucción
{% highlight java linenos %}
public class ExampleBean {
  public void cleanup() {
      // Código de finzalizacion, ej: liberar recursos
  }
}
{% endhighlight %}

{% highlight xml linenos %}
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
{% endhighlight %}<br/>

Nota que en la declaración del bean en el XML especificamos los métodos con `init-method` y `destroy-method`.
También podemos usar unas interfaces de Spring aunque esta forma es mas intrusiva porque la usamos directamente en la
clase.


{% highlight java linenos %}

import org.springframework.beans.factory.InitializingBean;

public class AnotherExampleBean implements InitializingBean {
  public void afterPropertiesSet() {
      // Código de inicialización
  }
}


import org.springframework.beans.factory.DisposableBean;

public class AnotherExampleBean implements DisposableBean {
  public void destroy() {
      // Código de finzalizacion, ej: liberar recursos
  }
}

{% endhighlight %}

{% highlight xml linenos %}

<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>

<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>

{% endhighlight %}

En este caso no usamos los atributos `init-method` y `destroy-method` si no las interfaces `InitializingBean` y `DisposableBean`.

<br/>

#### Ciclo de Vida Global
Podemos especificar una configuración global para todos los beans y asi no indicar en cada bean cual es el método de inicialización
y destrucción. Esto nos obliga a estandarizar los métodos en cada bean. En el tag `<beans>` del XML usamos los atributos 
`default-destroy-method` y `default-init-method`:
 
{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"
 default-destroy-method="finish" default-init-method="init">

   <bean ... />

   <bean ... />

</beans>
{% endhighlight %}<br/>

En este caso si queremos usar el ciclo de vida de un bean este debe definir los métodos `finish` y `init`. 