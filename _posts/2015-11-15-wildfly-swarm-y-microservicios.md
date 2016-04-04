---
layout: post
title:  "WildFly Swarm y Microservicios"
date:   2015-11-15 08:02:00
published: false
categories: [javaee]
tags: [microservicios, javaee, arquitectura, patrones]
comments: true
shortinfo: Uso de WildFly Swarm y los microservicios.
---

Siguiendo con la parte practica vamos a trabajar con proyectos JavaEE y Spring. Sabemos que para correr una aplicacion JavaEE es necesarios tener un servidor de aplicaciones instalado y corriendo para luego desplegar nuestro proyecto JavaEE en el. Pero vamos a hacer sencillo usando WildFly Swarm y depues Spring Boot.

<br/>

#### Que es WildFly Swarm
En la web de WildFly Swarm encontramos lo siguiente:

Swarm ofrece un enfoque innovador para construir y ejecutar aplicaciones JavaEE empaquetandolas con justo lo necesario de la plataforma para solo hacer "java -jar tuAplicacion". Sin embargo, es mucho mas genial que eso...

Ahora vamos a nuestro ejemplo.

## Pasos
1. Crear un proyecto con Maven
2. Agregar las dependencias y plugins de Swarm al `pom.xml`
3. Crear un `Servlet`
4. Ejecutar la aplicacion

<br/>

#### Crear un proyecto con Maven
Teniendo Maven instalado vamos a la linea de comandos y copiamos:

{% highlight bash %}
$ mvn archetype:generate -DgroupId=org.acme -DartifactId=sample-swarm -Dpackage=org.acme.sample --batch-mode
{% endhighlight %}

Esto nos crea un proyecto sencillo de maven, o lo que conocemos como un quick-start.

<br/>

#### Agregar dependencias y plugins de Swarm
La dependencia necesaria por ahora:

{% highlight xml linenos %}
<dependency>
    <groupId>org.wildfly.swarm</groupId>
    <artifactId>undertow</artifactId>
    <version>1.0.0.Beta4</version>
</dependency>
{% endhighlight %}<br/>

Y el plugin:
{% highlight xml linenos %}
  <build>
    <plugins>
      <plugin>
        <groupId>org.wildfly.swarm</groupId>
        <artifactId>wildfly-swarm-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>package</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
{% endhighlight %}<br/>

#### Crear un Servlet
Ahora creamos un `Servlet` en el paquete `org.acme.sample` :

{% highlight java linenos %}
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.write("Hola Mundo!!");
    }
}
{% endhighlight %}<br/>

#### Ejecutar la aplicacion
Ahora solo compilamos, empaquetamos y corremos la aplicacion:

{% highlight bash %}
$ mvn clean package
$ mvn wildfly-swarm:run
{% endhighlight %}

Ahora tenemos nuestra aplicacion corriendo con un servidor embebido WildFly.

Ya podemos navegar a la URL `localhost:8080/hello`

## Notas Finales
Con esto podemos empezar de una forma sencilla y rapida nuestra aplicaciones. Usaremos esta mecanica de ahora en adelante mientras vayamos explorando toda la plataforma **Java EE**.

En un proximo articulo veremos como crear aplicaciones con **`Spring Boot`** que tiene una filosofia parecida a **`WildFly Swarm`**.