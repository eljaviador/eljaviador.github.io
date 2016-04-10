---
layout: post
title:  "Monitoreando Spring Boot"
date:   2016-01-17 15:10:00
published: true
categories: [frameworks]
tags: [spring]
comments: true
shortinfo: Como monitorear nuestra aplicación Spring Boot.
---

Antes de comenzar quizás te interese leer :

* [_**Spring Boot Microservicios**_]({% post_url 2015-11-27-spring-boot-y-microservicios %})
* [_**Spring Boot y Payara Micro**_]({% post_url 2015-12-16-spring-boot-y-payara-micro %})

Una cuestión importante que agrega valor a un sistema es sin duda un sistema de monitoreo. **Spring Boot** tiene un `starter` especial para esta valiosa tarea. Se le llama _**Actuator**_. Sobre todo en ecosistemas basados en microservicios que cada uno permita un monitoreo y que informen del estado de salud en que se encuentran es realmente valioso.

Lo único que debemos hacer es agregar la dependenca necesaria y listo:

{% highlight xml linenos %}
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
{% endhighlight %}<br/>

La forma en que esto funciona es dando una lista de _EndPoints_ a consultar con sus respectivos mappings. En nuestro post anterior nuestra URL de ejemplo era algo así:

`localhost:8080/spring-boot-payara`

La forma de ver todos los mappings seria:

`localhost:8080/spring-boot-payara/mappings`

Esto nos da un `JSON` con una lista de mappings que podemos revisar. Algunos interesantes son:

* **/health**   : Estatus.
* **/env**      : Propiedades del entorno.
* **/metrics**  : Información de memoria, procesador, heap, hilos, etc.
* **/trace**    : Request ralizados.
* **/logfile**  : Disponible si tenemos la propiedad `logging.file` o `logging.path`.

Hay mas EndPoints disponibles que puedes consultar en la web del proyecto.
