---
layout: post
title:  "WildFly Swarm y Microservicios"
date:   2015-11-10 08:02:00
published: true
categories: [javaee]
tags: [microservicios, javaee, wildfly, arquitectura, patrones]
comments: true
shortinfo: Uso de WildFly Swarm y los microservicios.
---

Antes de comenzar quizas te interese leer :

* [_**Microservicios**_]({% post_url 2015-10-05-microservicios %})


Siguiendo con la parte practica vamos a trabajar con proyectos **JavaEE** y **Spring**. Sabemos que para correr una aplicacion JavaEE es 
necesarios tener un servidor de aplicaciones instalado y corriendo para luego desplegar nuestro proyecto JavaEE en el. Pero vamos a hacer sencillo usando **WildFly Swarm** y después **Spring Boot**.

<br/>

#### Que es WildFly Swarm
En la web de WildFly Swarm encontramos lo siguiente:

_WildFly Swarm ofrece un enfoque innovador para construir y ejecutar aplicaciones JavaEE empaquetandolas con justo lo necesario de la plataforma para solo hacer "java -jar tuAplicacion". Sin embargo, es mucho mas genial que eso..._

Ahora vamos a nuestro ejemplo.

**NOTA:** Es recomendable tener la version **`3.2.5`** o superior de `Maven`.

## Pasos
1. Crear un proyecto WEB con Maven
2. Agregar las dependencias y plugins de Swarm al `pom.xml`
3. Agregar un Servlet de pruebas
4. Ejecutar la aplicacion

<br/>

#### 1. Crear un proyecto con Maven
Teniendo `Maven`(3.2.5 o superior) instalado vamos a la linea de comandos y copiamos:

{% highlight bash %}
$ mvn archetype:generate -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=webapp-javaee7 -DarchetypeVersion=1.1 -DgroupId=org.acme -DartifactId=sample-swarm -Dpackage=org.acme.sample --batch-mode
{% endhighlight %}

Esto nos crea un proyecto **Web Java EE 7**.

<br/>

#### 2. Agregar dependencias y plugins de Swarm
La dependencia necesaria por ahora:

{% highlight xml linenos %}
<dependency>
    <groupId>org.wildfly.swarm</groupId>
    <artifactId>undertow</artifactId>
    <version>1.0.0.Beta2</version>
</dependency>
{% endhighlight %}<br/>

Y el plugin en la sección `<build><plugins>...</plugins></build>`:
{% highlight xml linenos %}
<plugin>
    <groupId>org.wildfly.swarm</groupId>
    <artifactId>wildfly-swarm-plugin</artifactId>
    <version>1.0.0.Beta2</version>
    <executions>
      <execution>
        <goals>
          <goal>package</goal>
        </goals>
      </execution>
    </executions>
</plugin>
{% endhighlight %}<br/>


#### 3. Crear el Servlet de prueba
Creamos una clase servlet en el paquete `org.acme.sample` :
 
{% highlight java linenos %}
package org.acme.sample;

@WebServlet("/")
public class HelloServlet extends HttpServlet {

    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("Hola Mundo!!");
    }

}
{% endhighlight %}<br/>


#### 4. Ejecutar la aplicación
Ahora solo compilamos, empaquetamos y corremos la aplicacion:

{% highlight bash %}
$ mvn clean package
$ mvn wildfly-swarm:run
{% endhighlight %}

Ahora tenemos nuestra aplicación corriendo con un servidor embebido WildFly.

Ya podemos navegar a la URL `localhost:8080`

**NOTA :** También podemos ejecutar la aplicación dentro del directorio `target` con el comando:

{% highlight bash %}
$ java -jar sample-swarm-1.0-SNAPSHOT-swarm.jar
{% endhighlight %}<br/>

## Notas Finales
Con esto podemos empezar de una forma sencilla y rapida nuestra aplicaciones. Usaremos esta mecanica de ahora en adelante mientras 
vayamos explorando toda la plataforma **Java EE**.

En un proximo articulo veremos como crear aplicaciones con **`Spring Boot`** que tiene una filosofia parecida a **`WildFly Swarm`**.