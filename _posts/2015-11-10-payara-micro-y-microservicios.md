---
layout: post
title:  "Payara Micro y Microservicios"
date:   2015-11-10 20:45:00
published: true
categories: [javaee]
tags: [microservicios, javaee, payara, arquitectura, patrones]
comments: true
shortinfo: Uso de Payara Micro y los microservicios.
---

Antes de comenzar quizas te interese leer :

* [_**Microservicios**_]({% post_url 2015-10-05-microservicios %})
* [_**WildFly Swarm y Microservicios**_]({% post_url 2015-10-15-wildfly-swarm-y-microservicios %})


Siguiendo con la parte practica ahora le toca el turno a **Payara Micro**. Vamos a seguir los mismo pasos del artículo anterior pero esta vez no agregamos ningún plugin.

<br/>

#### Que es Payara Micro
En la web de Payara Micro encontramos lo siguiente:

_Pequeño e increiblemente fácil de usar te permite correr archivos `.war` desde la línea de comandos sin ningún servidor de aplicaciones instalado. Con Clustering automático esta diseñado para ejecutar aplicaciones en una infraestructura moderna de contenedores._

Ahora vamos a nuestro ejemplo.

## Pasos
1. Descargar **Payara Micro**
2. Crear un proyecto WEB con Maven
3. Agregar un Servlet de pruebas
4. Ejecutar la aplicacion

<br/>

#### 1. Descargar **Payara Micro**
Vamos a la url [_**descargas de Payara**_](http://www.payara.fish/downloads "Descargas Payara") y buscamos _**Payara Micro**_. El `.jar` debemos descargarlo en la misma carpeta donde vamos a generar nuestro `.war`, en este caso el directio `target` que genera **Maven**.

<br/>

#### 1. Crear un proyecto con Maven
En nuestra línea de comandos:

{% highlight bash %}
$ mvn archetype:generate -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=webapp-javaee7 -DarchetypeVersion=1.1 -DgroupId=org.acme -DartifactId=sample-payara -Dpackage=org.acme.sample --batch-mode
{% endhighlight %}

Esto nos crea un proyecto Web Java EE 7.

<br/>

#### 3. Agregar un Servlet de pruebas
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


#### 4. Ejecutar la aplicacion
Desde la carpeta raíz del proyecto, compilamos y empaquetamos:

{% highlight bash %}
$ mvn clean package
{% endhighlight %}

Desde el directorio `target` corremos la aplicación. Ten en cuenta que tu versión de Payara puede ser diferente:
{% highlight bash %}
$ java -jar payara-micro-4.1.1.161.1.jar --deploy sample-payara-1.0-SNAPSHOT.war
{% endhighlight %}

Ahora tenemos nuestra aplicación corriendo con un servidor Payara Micro.

Ya podemos navegar a la URL `localhost:8080/sample-payara-1.0-SNAPSHOT`

