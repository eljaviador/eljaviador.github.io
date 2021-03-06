---
layout: post
title:  "Microservicios"
date:   2015-10-05 09:30:00
published: true
categories: [conceptos y principios]
tags: [microservicios, javaee, arquitectura, patrones]
comments: true
shortinfo: Introdución al concepto de microservicios. Que son y porque usarlos?
---

Desde hace tiempo tratamos de dividir los grandes sistemas en pequeñas partes. Los _paquetes_, _clases_ y _métodos_ son un 
ejemplo práctico de ello. La _**modularidad**_ la implementamos por lo general dentro de nuestra gran aplicación. 

Esto lo conocemos como una aplicación _**monolítica**_ y de esa manera funciona para nosotros. Toda nuestra lógica de negocio 
en un solo distribuible ya sea para desplegar en un server o para correr como standalone.


## Microservicios
Recientemente, quiero decir quizás de 2013 y 2014 en adelante un estilo de arquitectura denominado _**microservicios**_ ha empezado 
a emerger y a popularizarse. Los microservicios es un estilo de arquitectura que involucra construir grandes sistemas 
como un conjunto de _**servicios pequeños**_, _**granulares**_, _**desplegados independientemente**_ y que _**colaboran entre sí**_.

Ejemplos de microservicios pueden ser un _catálogo de productos_, un _carrito de compra_, _registro de usuarios_, _facturas_ entre otros. 
Hay quienes pudieran llegar al punto de ver los microservicios como tablas siendo desde mi punto de vista un poco extremista.

La idea de este tipo de arquitectura no es nueva pero se ha popularizado mucho ahora a raíz de las tendencias como **Cloud**, 
**Continuos delivering** y **DevOps** que permiten trabajar este tipo de enfoque. 

<br/>

#### Porque si?
Los microservicios fomentan buenas prácticas de ingeniería, uso de APIs e _**interfaces limpias**_, _**bajo acoplamiento**_, _**alta cohesión**_, 
_**escalabilidad independiente**_, entre otras. Los microservicios permiten independencia en equipos de trabajo y facilitan el 
Continous delivering de forma muy puntual mientras el resto del sistema se mantiene estable y disponible.

<br/>

#### Porque no?
Los microservicios no son adecuados para todos. Una taza de té en varias copitas no hace el té sea más apetecible, de la misma 
forma los microservicios no siempre son la solución adecuada para poner tu código en orden. Lo ideal es que tu aplicación se desarrolle 
de forma _**monolítica**_ y muy bien _**modularizada**_. El tiempo te dira si es muy compleja para ser manejada de esta forma y debes ir a microservicios.

Hay un esfuerzo extra para llegar a los microservicios. Características como _**data común**_, _**sincronización**_ de datos y _**transaccionalidad**_. 
Adicionalmente puede haber un sobrecarga en la comunicación en cuanto a serialización/deserialización y el tipo de protocolo.


## Lo importante
Uno de los puntos de debate es la _**transición hacia los microservicios**_, debe ser en proyectos nuevos o en proyectos existentes, 
de ser de forma gradual o de un solo tajo.
Otro punto importante es la _**orquestación de los procesos**_ de negocio a través de ellos. Por lo general en los procesos de comunicación 
comúnmente encontramos productos y enfoques que hacen la comunicación inteligente, por ejemplo un **ESB**(Enterprise Service Bus) que 
ofrece mecanismos sofisticados como ruteo de mensajes, transformación y aplicación de reglas de negocio.

Los microservicios favorecen un enfoque diferente: _**Endpoints inteligentes**_ y _**tuberías tontas**_. Las aplicaciones apuntan a ser tan 
desacopladas y cohesivas como se pueda. Generalmente usando protocolos simples al estilo **REST**. Los más usados son HTTP y un Bus de 
mensajería liviano.

<br/>

#### Que pasa con Java?
En el mundo Java hay varios proyectos que ya están habilitados para los microservicios. Por ejemplo los servidores de aplicaciones 
[**WildFly**](http://www.wildfly.org "Web de WildFly") y [**Payara**](http://www.payara.fish "Web de Payara")
tienen los subproyectos _**WildFly Swarm**_ y _**Payara Micro**_ destinados a trabajar con microservicios y aplicaciones **Java EE**.

Otro producto que ha acaparado mucho la atención es [**Spring Boot**](http://projects.spring.io/spring-boot/ "Web de Spring Boot") que 
permite crear aplicaciones standalone basadas en **Spring** con un contenedor embebido como Tomcat o Jetty. Aunque también es posible 
construir una aplicación con **Spring Boot** y correrla con **Payara Micro** o **WildFly Swarm**.

En todo caso tendriamos nuestros servicios como aplicaciones independientes que arrancamos como un standalone y como distribuibles 
listos para usar en cualquier servicio Cloud. 
