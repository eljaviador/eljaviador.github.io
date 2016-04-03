---
layout: post
title:  "Microservicios"
date:   2015-10-05 09:30:00
published: false
categories: [conceptos y principios]
tags: [microservicios, javaee, arquitectura, patrones]
comments: true
shortinfo: Leve introdución al concepto de microservicios.
---

Desde hace tiempo tratamos de dividir los grandes sistemas en pequeñas partes. Los paquetes, clases y métodos son un 
ejemplo práctico de ello. La modularidad la implementamos por lo general dentro de nuestra gran aplicación. 

Esto lo conocemos como una aplicación monolítica y de esa manera funciona para nosotros. Toda nuestra lógica de negocio 
en un solo distribuible ya sea para desplegar en un server o para correr como standalone.


## Microservicios
Recienteme, quiero decir quizás de 2013 y 2014 en adelante un estilo de arquitectura denominado _**microservicios**_ ha empezado 
a emerger y a popularizarse. Los microservicios es un estilo de arquitectura que involucra construir grandes sistemas 
como un conjunto de _**servicios pequeños**_, _**granulares**_, _**desplegados independientemente**_ y que _**colaboran entre sí**_.

Ejemplos de microservicios pueden ser un catálogo de productos, un carrito de compra, registro de usuarios, facturas entre otros. 
Hay quienes pudieran llegar al punto de ver los microservicios como tablas siendo desde mi punto de vista un poco extremista.

La idea de este tipo de arquitectura no es nueva pero se ha popularizado mucho ahora a raíz de las tendencias como **Cloud**, 
**Continuos delivering** y **DevOps** que permiten trabajar este tipo de enfoque. 


## Porque si?
Los microservicios fomentan buenas practicas de ingenieria, uso de APIs e interfaces limpias, bajo acoplamiento, alta cohesión, 
escalabilidad independiente, entre otras. Los microservicios permite independencia en equipos de trabajo y facilitan el 
Continous delivering de forma muy puntual mientras el resto del sistema se mantiene estable y disponible.


## Porque no?
Los microservicios no son adecuados para todos. Una taza de te en varias copitas no hace el te sea más apetecible, de la misma 
forma los microservicios no siempre son la solución adecuada para poner tu codigo orden. Lo ideal es que tu aplicación se desarrolle 
de forma monolítica y muy bien modularizada. El tiempo te dira si es muy compleja para ser manejada de esta forma y debes ir a microservicios.

Hay un esfuerzo extra para llegar a los microservicios. Características como data común, sincronización de datos y transaccionalidad. 
Adicionalmente puede haber un sobrecarga en la comunicación en cuanto a serialización/deserialización y el tipo de protocolo.


## El debate
Uno de los puntos de debate es la transición hacia los microservicios, debe ser en proyectos nuevos o en proyectos existentes, 
de ser de forma gradual o de un solo tajo.
Otro punto importante es la orquestación de los procesos de negocio a través de ellos. Por lo general en los procesos de comunicación 
comúnmente encontramos productos y enfoques que hacen la comunicación inteligente, por ejemplo un ESB(Enterprise Service Bus) que 
ofrece mecanismos sofisticados como ruteo de mensajes, transformación y aplicación de reglas de negocio.

Los microservicios favorecen un enfoque diferente: Endpoints inteligentes y tuberías tontas. Las aplicaciones apuntan a ser tan 
desacoplada y cohesiva como se pueda. Generalmente usando protocolos simples al estilo REST. Los más usados son HTTP y un Bus de 
mensajería liviano.


## Que pasa con Java?
En el mundo Java hay varios proyectos que ya están habilitados para los microservicios. En especial los servidores de aplicaciones 
como _**WildFly Swarm**_ y _**WebSphere Liberty**_ para trabajar con aplicaciones **Java EE**.

Otro producto que ha acaparado mucho la atención es _**Spring Boot**_ que permite crear aplicaciones standalone basadas en **Spring** 
con un contenedor embebido como Tomcat o Jetty.

En todo caso tendriamos nuestros servicios como aplicaciones independientes que arrancamos como un standalone y como distribuibles 
listos para usar en cualquier servicio Cloud. 
