---
layout: post
title:  "Formas de cargar Spring [01]"
date:   2016-09-20  20:11:00
published: false
categories: [frameworks]
tags: [spring, javaee, arquitectura]
comments: true
shortinfo: Como cargar el contenedor(contexto) Spring en nuestras aplicaciones. Parte 1.
----------------------------------------------------------------------------------------

Antes de detallar varias formas como podemos correr Spring en nuestras aplicaciones, es necesario que aclare un par de 
conceptos diferentes pero asociados entre si. El primero es la carga del contenedor en si mismo y la segunda es
el origen de la configuracion de nuestros beans o componentes.

Te puedes imaginar el contenedor Spring como un gran saco donde estan guardados nuestros beans. Spring maneja el concepto
de contexto que vendria a siendo el lugar donde estan registrados nuestros componentes(beans). Incluso podemos manejar
una jerarquia de contextos que es una especie de arbol donde siempre hay un contexto padre. Pero esto no lo 
vamos a detallar.

Este gran saco(contenedor) necesita leer el sitio donde tenemos configurados nuestros componentes(beans). Esta configuracion
puede estar en un XML, en una clase Java o puede ser un mix de los dos. Asi que empezemos.

Otro punto a tener en cuenta es si nuestra aplicacion es **Standalone** o es una aplicacion **JavaEE**(_.war_, _.ear_).

## Aplicacion Standalone con configuracion XML
Aqui siempre tenemos un punto de entrada o de inicio de la aplicacion, el conocido metodo estatico `main(Stirng[] args)`.
Yo personalmente trabajo aqui con dos clases, la que llamo `Lanzador` o `Launcher` que es donde reside el metodo `main()`
y el sitio donde levanto el contenedor Spring, y una otra que llamo `Main` o `Principal` que seria la clase que realmente
es mi aplicacion.

{% highlight java linenos %}
// Cargo de un XML la configuracion. Esta es la implementacion mas recomendable ya que busca
// en el classpath. Si tenemos un proyecto Maven ese XMl deberia estar comumente en la carpeta  `src/main/resources`.
ApplicationContext context = new ClassPathXmlApplicationContext("mis-beans.xml");
{% endhighlight %}<br/>

Ahora nuestra clase `Main` o `Principal` debe ser un componente configurado de Spring. Luego solo pido el bean y lo inicio
dependiendo mi tipo de aplicacion Standalone(Swing, bash, etc). 

{% highlight java linenos %}
Main miMain = context.getBean(Main.clas);
miMain.start();
{% endhighlight %}<br/>

De aqui en adelante puedo inyectar mis componentes(dependencias) unos en otros a traves de setters y @Autowired. El codigo completo
seria algo asi:

{% highlight java linenos %}
public class Launcher{
   public static void main(String[] args) {
      // Cargo de un XML la configuracion. Esta es la implementacion mas recomendable ya que busca
      // en el classpath. Si tenemos un proyecto Maven ese XMl deberia estar comumente en la carpeta  `src/main/resources`.
      ApplicationContext context = new ClassPathXmlApplicationContext("mis-beans.xml");
      Main miMain = context.getBean(Main.clas);
      miMain.start();
   }
}
{% endhighlight %}<br/>
 
 
## Aplicacion Standalone con configuracion Java
La unica diferencia con el anterior es que no vamos a usar la implementacion `ClassPathXmlApplicationContext` si no otra llamada

{% highlight java linenos %}
public class Launcher{
   public static void main(String[] args) {
      // Cargo de una clase Java la configuración.
      ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
      Main miMain = context.getBean(Main.clas);
      miMain.start();
   }
}
{% endhighlight %}<br/>

En otro post revisaremos la carga de Spring en proyecto JavaEE ya que tiene mas variaciones.

Para finalizar vamos a ver las configuraciones con clases Java. Las configuraciones con XML aunque aun se usan debo decir que 
muy poco hoy dia. Si quieres ver algunos ejemplos revisa los post donde explico un poco de Spring.
 
* [_**Contenedor Spring 01**_]({% post_url 2014-07-13-contenedor-spring %})

## Configuraciones con clases Java
En el fragmento de codigo anterior viste que en la clase `AnnotationConfigApplicationContext` paso como paremtro el argumento
`AppConfig.class`. Esta es una clase cualquiera Java donde colocas tus beans. Pero esta clase debe tener dos elementos importantes,
la anotaciones `@Configuration` y metodos anotados con `@Bean`.
 
{% highlight java linenos %}
@Configuration
public class AppConfig{
   
   @Bean
   public Main main() {
      Main mainBean = new MainBean();
      return mainBean;
   }
}
{% endhighlight %}<br/> 

Mira que sencillo es declarar nuestros beans en una clase Java cualquiera.

#### Recomendación
Una recomendacion que hago aqui es que trates de tener tu configuracion ya sea XML o por Java lo mas limpia posible. Es decir por lo general
yo en ves de declarar mis beans directamente en el XML o en una clase @Configuration, prefiero declarar directamente mis componentes(clases)
como beans. Usando @Component sobre la clase pero al mismo tiempo diciendole a Spring que escanee los paquetes para que pueda encontrar
las clases con esa anotacion @Component. Hay dos formas de decirle a Spring que escanee, si es para XML o si es para Java.

{% highlight java linenos %}
@Configuration
@ComponentScan(basePackages = {"com.acme.paquete01", "com.acme.paquete02"})
public class AppConfig{
   

}

@Component
public class Main{

   public void start(){//...}
}
{% endhighlight %}<br/>


En el XML lo podemos hacer con `<context:component-scan base-package="com.acme.paquete01" />`.

Mayormente deberiamos usar la clase `AppConfig` para componentes de terceros que no tenemos acceso para colocarles @Component. Por ejemplo
 declrar un daaSource  o cosas asi.






