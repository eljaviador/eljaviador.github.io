---
layout: post
title:  "Inyectando Colecciones con Spring [04]"
date:   2014-11-15 13:10:00
published: true
categories: [frameworks]
tags: [conceptos, arquitectura, spring]
comments: true
shortinfo: Cuarta entrega para comprender el popular framework y contenedor Spring.
---

Antes de leer este post, te recomiendo los anteriores para un mejor entendimiento:

* [_**Contenedores e Inyección de Dependencias 01**_]({% post_url 2014-07-08-contenedores-e-inyeccion-de-dependencias-parte-1 %})
* [_**Contenedores e Inyección de Dependencias 02**_]({% post_url 2014-07-11-contenedores-e-inyeccion-de-dependencias-parte-2 %})
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})
* [_**Contenedor Spring 02**_]({% post_url 2014-09-17-contenedor-spring-parte-2 %})
* [_**Contenedor Spring 03**_]({% post_url 2014-10-25-contenedor-spring-parte-3 %})

## Inyeccion de Colecciones de Valores
Hemos visto en los otros artículos la inyección por constructor y setter. Vimos como inyectar valores y referencias a otras
clases. Ahora veamos como es la inyección de colecciones, por ejemplo listas, mapas, etc. Spring proporciona algunos tags 
para esto como `<list>`, `<set>`, `<map>` y `<props>`.

* El tag `<list>` trabaja con `java.util.Collection`, `java.util.List` y `java.util.Set`.
* El tag `<map>` trabaja con `java.util.Map`.
* El tag `<prop>` trabaja con `java.util.Properties`<br/><br/>

#### Listas
El tag `<list>` permite dentro de el otros como `<value>`, `<bean>`, `<ref>` y `<null/>`.

{% highlight xml linenos %}
<bean class="com.ejemplo1.Foo">
    <property name="lista">
        <list>
            <value>valorUno</value>
            <value>valorDos</value>
            <null/>
        </list>
    </property>
</bean>

<bean class="com.ejemplo1.Bar">
    <property name="lista">
        <list>
            <ref bean="beanUno"/>
            <bean class="examples.BeanDos" />
        </list>
    </property>
</bean>
{% endhighlight %}

El tag `<set>` es básicamente parecido a `<list>` y son intercambiables.<br/><br/>

#### Mapas
Permite internamente `<entry>`. El tag `<entry>` posee los atributos `key`, `value`, `value-type`, `key-ref` y `value-ref`.

{% highlight xml linenos %}
<bean class="com.ejemplo1.Foo">
   <property name="map">
      <map>
         <entry key="keyUno" value="1"  value-type="int"/>
         <entry key="keyDos" value="2"  value-type="int"/>
      </map>
   </property>
</bean>
{% endhighlight %}<br/>

#### Properties
Este tag permite internamente `<prop key=”...”>valor</prop>`.

{% highlight xml linenos %}
<bean class="com.ejemplo1.Foo">
   <property name="propiedades">
      <props>
         <prop key="keyUno">valorUno</prop>
         <prop key="keyDos">valorDos</prop>
      </props>
   </property>
</bean>
{% endhighlight %}

Alternativamente podríamos hacer:
{% highlight xml linenos %}
<bean class="com.ejemplo1.Foo">
   <property name="propiedades">
      <value>
         keyUno=valor1
         keyDos=valor2
      </value>
   </property>
</bean>
{% endhighlight %}
