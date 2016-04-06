---
layout: post
title:  "WildFly Swarm y Microservicios"
date:   2015-11-03 13:50:00
published: false
categories: [framewokrs]
tags: [spring, arquitectura, patrones]
comments: true
shortinfo: Uso de Spring Boot y los microservicios.
---

Antes de comenzar quizas te interese leer :

* [_**Microservicios**_]({% post_url 2015-10-05-microservicios %})


Unos de los puntos a criticar de **Spring** es todo el trabajo de configuración que se necesita antes de poner en marcha un proyecto.
Spring Boot nace como una respuesta a todas esas quejas de los usuarios con respecto a este tema. Este es un proyecto que te permite 
crear desde cero aplicaciones basadas en Spring de forma rápida y con poco esfuerzo de configuración.  

En su web encontramos lo siguiente:

Spring Boot toma un conjunto de principios establecidos(dogma) en la construcción de aplicaciones basadas en Spring. Favoreciendo 
la _**convencion sobre configuracion**_ y diseñado para que construyas y ejecutes lo mas rápido posible.

Bueno, vamos entrando en materia. Como el ejemplo anterior que hicimos con **WildFly Swarm** haremos uno parecido con **Spring Boot**.
En este caso tendremos un contenedor **Tomcat** embebido corriendo nuesta aplicación Spring y que ejecutaremos como una simple
aplicación standalone.

Para crear una aplicación con Spring Boot tenemos 3 alternativas:
 
1. Usando la herramienta web `start.spring.io`.
2. Usando la herramiente CLI que debemos descargar e instalar.
3. De forma manual creando un proyecto y modificando las dependencias.

En este caso vamos a usar la número 1 que es la mas rapida y facil.

![Spring Boot](/images/spring-boot-01.png)
 
<br/>



#### Que es WildFly Swarm
En la web de WildFly Swarm encontramos lo siguiente:

Swarm ofrece un enfoque innovador para construir y ejecutar aplicaciones JavaEE empaquetandolas con justo lo necesario de la plataforma para solo hacer "java -jar tuAplicacion". Sin embargo, es mucho mas genial que eso...

Ahora vamos a nuestro ejemplo.

NOTA: Es recomendable tener la version **`3.2.5`** o superior de `Maven`.

## Pasos
1. Crear un proyecto WEB con Maven
2. Agregar las dependencias y plugins de Swarm al `pom.xml`
4. Ejecutar la aplicacion

<br/>

#### Crear un proyecto con Maven
Teniendo Maven(3.2.5 o superior) instalado vamos a la linea de comandos y copiamos:

{% highlight bash %}
$ mvn archetype:generate -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=webapp-javaee7 -DarchetypeVersion=1.1 -DgroupId=org.acme -DartifactId=sample-swarm -Dpackage=org.acme.sample --batch-mode
{% endhighlight %}

Esto nos crea un proyecto Web Java EE 7.

<br/>

#### Agregar dependencias y plugins de Swarm
La dependencia necesaria por ahora:

{% highlight xml linenos %}
<dependency>
    <groupId>org.wildfly.swarm</groupId>
    <artifactId>undertow</artifactId>
    <version>1.0.0.Beta2</version>
</dependency>
{% endhighlight %}<br/>

Y el plugin:
{% highlight xml linenos %}
  <build>
    <plugins>
      ... otros plugins
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
    </plugins>
  </build>
{% endhighlight %}<br/>


#### Ejecutar la aplicacion
Ahora solo compilamos, empaquetamos y corremos la aplicacion:

{% highlight bash %}
$ mvn clean package
$ mvn wildfly-swarm:run
{% endhighlight %}

Ahora tenemos nuestra aplicacion corriendo con un servidor embebido WildFly.

Ya podemos navegar a la URL `localhost:8080`

NOTA : Tambien podemos ejecutar la aplicacion dentro del directorio `target` con el comando:

{% highlight bash %}
$ java -jar sample-swarm-1.0-SNAPSHOT-swarm.jar
{% endhighlight %}

## Notas Finales
Con esto podemos empezar de una forma sencilla y rapida nuestra aplicaciones. Usaremos esta mecanica de ahora en adelante mientras vayamos explorando toda la plataforma **Java EE**.

En un proximo articulo veremos como crear aplicaciones con **`Spring Boot`** que tiene una filosofia parecida a **`WildFly Swarm`**.