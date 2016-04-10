---
layout: post
title:  "Spring Boot y Microservicios"
date:   2015-11-27 18:00:00
published: true
categories: [frameworks]
tags: [spring, arquitectura, patrones]
comments: true
shortinfo: Uso de Spring Boot y los microservicios.
---

Antes de comenzar quizás te interese leer :

* [_**Microservicios**_]({% post_url 2015-10-05-microservicios %})
* [_**WildFly Swarm y Microservicios**_]({% post_url 2015-10-15-wildfly-swarm-y-microservicios %})
* [_**Payara Micro y Microservicios**_]({% post_url 2015-11-10-payara-micro-y-microservicios %})


Unos de los puntos a criticar de **Spring** es todo el trabajo de configuración que se necesita antes de poner en marcha un proyecto.
_**Spring Boot nace**_ como una respuesta a todas esas quejas de los usuarios con respecto a este tema. Este es un proyecto que te permite 
crear desde cero aplicaciones basadas en Spring de forma rápida y con poco esfuerzo de configuración.  

En su web encontramos lo siguiente:

Spring Boot toma un conjunto de principios establecidos(dogma) en la construcción de aplicaciones basadas en Spring. Favoreciendo 
la [_**convención sobre configuración**_](https://es.wikipedia.org/wiki/Convenci%C3%B3n_sobre_Configuraci%C3%B3n) y diseñado para que construyas y ejecutes lo mas rápido posible.

Bueno, vamos entrando en materia. Como el ejemplo anterior que hicimos con **WildFly Swarm** y **Payara Micro**, ahora haremos uno parecido con **Spring Boot**.
En este caso tendremos un contenedor **Tomcat** embebido corriendo nuesta aplicación Spring y que ejecutaremos como una simple aplicación standalone.

Para crear una aplicación con Spring Boot tenemos 3 alternativas:
 
1. Usando la herramienta web [**start.spring.io**](http://start.spring.io "Iniciador de Spring Boot").
2. Usando la herramienta `CLI` que debemos descargar e instalar.
3. De forma manual creando un proyecto y modificando las dependencias.

En este caso vamos a usar la número 1 que es la mas rápida y fácil.
 
<br/>

## Pasos
1. Crear un proyecto Spring Boot
2. Agregar un Controller
3. Ejecutar la aplicacion

<br/>

#### 1. Crear un proyecto Spring Boot
Navegamos a la url [**start.spring.io**](http://start.spring.io "Iniciador de Spring Boot") y llenamos los datos básicos del proyecto como el `group` y el `artifact`. También debemos agregar las dependencias de Spring que vamos a usar en este caso solo la dependencia `web`. Ahora generamos el proyecto, descargamos y descomprimimos.

![Spring Boot](/images/spring-boot-01.png)

<br/>

#### 2. Agregar un Controller de pruebas
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


#### 3. Ejecutar la aplicacion
Ahora solo compilamos, empaquetamos y corremos la aplicacion:

{% highlight bash %}
$ mvn spring-boot:run
{% endhighlight %}

Ahora tenemos nuestra aplicacion corriendo con un servidor embebido Tomcat.

Ya podemos navegar a la URL `localhost:8080`

## Notas Finales
**Spring Boot** agrega por defecto muchas configuraciones. Si queremos cambiar o agregar configuraciones podemos usar el archivo `application.properties` que esta en el folder de `resources` del proyecto. En la web del proyecto esta toda la documentación para esto.

Espero en un futuro tener un poco de tiempo para hacer unos artículos específicos de **Spring Boot**.