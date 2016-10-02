---
layout: post
title:  "Formas de cargar Spring [01]"
date:   2016-09-20  20:11:00
published: true
categories: [frameworks]
tags: [spring, javaee, arquitectura]
comments: true
shortinfo: Como cargar el contenedor(contexto) Spring en nuestras aplicaciones. Parte 1.
---

Antes de detallar varias formas como podemos correr Spring en nuestras aplicaciones, es necesario que aclare un par de 
conceptos diferentes pero asociados entre si.

1. La carga del contenedor como tal.
2. El origen o fuente de la configuración de los **beans**, _`XML`_ o _`Java`_.

Te puedes imaginar el contenedor Spring como un gran saco donde están guardados nuestros _**beans**_ o _**componentes**_.
Spring maneja el concepto de _**contexto**_ que vendría siendo el lugar donde están registrados los componentes(beans).
Incluso podemos manejar una jerarquía de contextos que es una especie de árbol donde siempre hay un contexto padre.
Pero esto no lo vamos a detallar.

Una vez especifiquemos como se hará la carga del contenedor, este necesita leer el sitio donde tenemos configurados nuestros componentes(beans).
Esta configuración puede estar en un _**XML**_, en una clase _**Java**_ o puede ser un mix de los dos.

Otro punto a tener en cuenta es si nuestra aplicación es **Standalone** o es una aplicación **JavaEE**(_.war_, _.ear_).

## Aplicación Standalone con configuración XML
Aquí siempre tenemos un punto de entrada o de inicio de la aplicación, el conocido método estático **`main(Stirng[] args)`**.
Yo personalmente trabajo aquí con dos clases, la que llamo `Lanzador` o `Launcher` que es donde reside el método `main()`
y donde levanto el contenedor Spring, y otra que llamo `Main` o `Principal` que seria la clase que realmente
es mi aplicación.

{% highlight java linenos %}
// Cargo de un XML la configuración. Esta es la implementación mas recomendable ya que busca
// en el classpath. Si tenemos un proyecto Maven ese XML debería estar comúnmente en la carpeta  src/main/resources.
ApplicationContext context = new ClassPathXmlApplicationContext("mis-beans.xml");
{% endhighlight %}<br/>

Ahora nuestra clase `Main` o `Principal` debe ser un componente configurado de Spring. Luego solo pido el bean y lo inicio
dependiendo mi tipo de aplicación Standalone(Swing, bash, etc).

{% highlight java linenos %}
Main miMain = context.getBean(Main.clas);
miMain.start();  //Método donde esta la lógica para arrancar tu aplicación
{% endhighlight %}<br/>

De aquí en adelante puedo inyectar mis componentes(dependencias) unos en otros a través de setters y **`@Autowired`**.
El código completo sería algo así:

{% highlight java linenos %}
public class Launcher{
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("mis-beans.xml");
      Main miMain = context.getBean(Main.clas);
      miMain.start();
   }
}
{% endhighlight %}<br/>
 
 
## Aplicación Standalone con configuración Java
La única diferencia con el anterior es que no vamos a usar la implementación `ClassPathXmlApplicationContext` si no otra llamada
**`AnnotationConfigApplicationContext`**.

{% highlight java linenos %}
public class Launcher{
   public static void main(String[] args) {
      // Cargo de una clase que llamo AppConfig la configuración.
      ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
      Main miMain = context.getBean(Main.clas);
      miMain.start();
   }
}
{% endhighlight %}<br/>

En otro post revisaremos la carga de Spring en proyecto JavaEE ya que tiene mas variaciones.

Para finalizar vamos a ver las configuraciones con clases Java. Las configuraciones con XML aunque aun se usan debo decir que 
hoy día su uso ha bajado bastante. Si quieres ver algunos ejemplos revisa los post donde explico un poco de Spring.
 
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})

## Configuraciones con clases Java
En el fragmento de código anterior viste que en la clase `AnnotationConfigApplicationContext` paso como parámetro el argumento
`AppConfig.class`. Esta es una clase cualquiera Java donde colocas tus beans. Pero esta clase debe tener dos elementos importantes,
la anotación `@Configuration` y métodos anotados con `@Bean`.
 
{% highlight java linenos %}
@Configuration
public class AppConfig{
   
   @Bean
   public Main main() {
      Main mainBean = new MainBean();
      return mainBean;
   }
}
{% endhighlight %}

Mira que sencillo es declarar nuestros beans en una clase Java cualquiera.

<br/>

## Mix de configuración 1. XML dentro de Java
Para importar nuestras configuraciones XML dentro una clase Java usamos la anotación `@ImportResource`.


{% highlight java linenos %}
@Configuration
@ImportResource("classpath:ms-beans.xml")
public class AppConfig{

   //...

}
{% endhighlight %}<br/>

## Mix de configuración 2. Java dentro XML
Para importar nuestras configuraciones Java dentro del XML simplemente declaramos nuestra clase Java como un bean mas en el XML.
Adicionalmente debemos usar la instruccion `<context:annotation-config/>` en el XML.

{% highlight java linenos %}
@Configuration
public class AppConfig{

   //...

}
{% endhighlight %}

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

   <context:annotation-config/>

   <bean id="appConfig" class="com.acme.AppConfig" />

</beans>
{% endhighlight %}<br/>


## Recomendaciones finales
Una recomendación que hago aquí es que trates de tener tu configuración ya sea XML o por Java lo mas limpia posible. Es decir por lo general
yo en ves de declarar mis beans directamente en el **XML** o en una clase **`@Configuration`**, prefiero declarar directamente mis componentes(clases)
como beans. Usando `@Component` sobre la clase pero al mismo tiempo diciéndole a Spring que escanee los paquetes para que pueda encontrar
las clases con esa anotación `@Component`.

Hay dos formas de decirle a Spring que escanee, si es para XML o si es para Java.

{% highlight java linenos %}
@Configuration
@ComponentScan(basePackages = {"com.acme.paquete01", "com.acme.paquete02"})
public class AppConfig{
   

}

@Component  //Esta clase automáticamente sera un bean con esta anotación.
public class Main{

   public void start(){//...}
}
{% endhighlight %}<br/>


En el XML lo podemos hacer con `<context:component-scan base-package="com.acme.paquete01" />`.

Y para que quiero un clase `AppConfig` vacía?
Mayormente deberíamos usar la clase `AppConfig` o nuestro **XML** para declarar componentes de terceros los cuales no tenemos
acceso para usar `@Component` con ellos. Por ejemplo para declarar un `DataSource` o cosas así.






