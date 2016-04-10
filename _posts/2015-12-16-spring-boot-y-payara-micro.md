---
layout: post
title:  "Spring Boot y Payara Micro"
date:   2015-12-16 07:30:00
published: false
categories: [frameworks]
tags: [spring, payara, arquitectura, patrones]
comments: true
shortinfo: Uso de Spring Boot y Payara Micro.
---

Antes de comenzar quizás te interese leer :

* [_**Microservicios**_]({% post_url 2015-10-05-microservicios %})
* [_**WildFly Swarm y Microservicios**_]({% post_url 2015-10-15-wildfly-swarm-y-microservicios %})
* [_**Payara Micro y Microservicios**_]({% post_url 2015-11-10-payara-micro-y-microservicios %})
* [_**Spring Boot Microservicios**_]({% post_url 2015-11-27-spring-boot-y-microservicios %})

Es turno para que usemos **Spring Boot** con una configración manual y ejecutemos nuestra aplicación con **Payara Micro** y no con un **Tomcat** embebido.

<br/>

## Pasos
1. Crear un proyecto Maven Web
2. Agregar la dependencia de Servlet 3.0
3. Agregar dependencias de Spring Boot
4. Configurar arranque de Spring Boot
5. Agregar un Controller
6. Ejecutar la aplicacion

<br/>

#### 1. Crear un proyecto Maven Web
{% highlight bash %}
$ mvn archetype:generate -DarchetypeArtifactId=maven-archetype-webapp -DgroupId=org.acme -DartifactId=spring-boot-payara -Dversion=1.0 --batch-mode
{% endhighlight %}

Esto nos crea un proyecto Web clásico. Es decir debemos hacer un pequeño cambio para colocar la versión servlet 3.0. Colocamos el siguiente contenido dentro del archivo `web.xml`.

{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

</web-app>
{% endhighlight %}<br/>


#### 2. Agregar la dependencia de Servlet 3.0
En nuestro `pom.xml` :

{% highlight xml linenos %}
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
    <scope>provided</scope>
</dependency>
{% endhighlight %}<br/>

#### 3. Agregar dependencias de Spring Boot
Spring Boot usa dependencias configuradas como parent. Pero no lo vamos hacer asi ya que nuestro proyecto puede depender de un parent de nosotros mismos. Usaremos el sistema de `<dependencyManagement>` de Maven para esto. En nuestro `pom.xml`:

{% highlight xml linenos %}
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.3.3.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
{% endhighlight %}<br/>

Agregamos el `starter` para web `spring-boot-starter-web`. Esta dependencia por defecto incluye `Tomcat`. Lo vamos excluir ya que no lo usaremos.

{% highlight xml linenos %}
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
{% endhighlight %}<br/>

#### 4. Configurar arranque de Spring Boot
Ahora debemos crear una clase para el arranque y configuración automática de **Spring Boot**. Esto es similar a la clase que Spring Boot agrega por defecto cuando creamos el proyecto con el mismo.

En la ruta `src/main/java/org/acme`:

{% highlight java linenos %}
package org.acme;

@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

}
{% endhighlight %}<br/>

#### 5. Agregar un Controller
Creamos una clase controller en el paquete `org.acme` :
 
{% highlight java linenos %}
package org.acme;

@Controller
public class HelloController {

    @RequestMapping("/")
    @ResponseBody
    public String hello(){
        return "Hola Mundo";
    }
}
{% endhighlight %}<br/>



#### 6. Ejecutar la aplicacion
Ahora solo compilamos, empaquetamos y corremos la aplicacion:

{% highlight bash %}
$ mvn spring-boot:run
{% endhighlight %}

Ahora tenemos nuestra aplicacion corriendo con un servidor embebido Tomcat.

Ya podemos navegar a la URL `localhost:8080/spring-boot-payara`

## Notas Finales
De esta manera podemos usar **Spring Boot** en cualquier servidor o contenedor de aplicaciones. El procedimiento es similar cuando quieras usar por ejemplo **WildFly Swarm**.
