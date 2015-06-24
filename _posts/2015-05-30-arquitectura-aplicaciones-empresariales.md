---
layout: post
title:  "Arquitectura de Aplicaciones Empresariales"
date:   2015-05-30 08:00:00
published: true
categories: [javaee]
tags: [javaee, ejb, cdi, arquitectura]
comments: true
shortinfo: Un tema base para empezar el desarrollo de aplicaciones enterprise Java EE.
---

Antes de leer este artículo te recomiendo que le des una mirada a :

* [_**Introducción a Java EE**_]({% post_url 2015-02-10-introduccion-a-javaee %})


Vamos a detallar la arquitectura de una aplicación enterprise(empresarial) para despues empezar a crear ejemplos tanto con **Java EE** 
como con **Spring Framework**.

En el artículo anterior hicimos una introducción de Java EE y sus conceptos más relevantes. En este vimos la división de capas físicas :

1. **Capa cliente:** A la cual pertenece la máquina del cliente.
2. **Capa middleware:** A la que pertenece Java EE y sus servidores de aplicaciones.
3. **Capa EIS:** A la que pertenece el servidor de Base de Datos.

![Java EE](/images/javaee-capas-fisicas-01.png)

<br/>

Hoy nos vamos a centrar en la capa middleware o Java EE. Vimos el concepto de _**componente**_ y como estos estan desplegados o instalados 
en un servidor de aplicaciones. Un servidor está compuesto por varios _**contenedores**_ de componentes.

![Java EE](/images/javaee-contenedores-01.png)


### Flujo Request/Response
Comprender el flujo de una petición(_**Request**_) y respuesta(_**Response**_) entre cliente y servidor es de vital importancia para poder 
empezar con este tipo de aplicaciones. Este flujo se hace sobre el protocolo **HTTP** que permite realizar conversaciones simples 
de petición y respuesta. Esta peticiones, desde un cliente son hechas con un navegador o con otra herramienta como `CURL`.

Tanto el request como el response se componen de dos partes, _**un head**_ y _**un body**_.

![Java EE](/images/request-response-partes.png)


Por ahora no vamos a entrar en mucho detalle pero tengamos en cuenta lo mínimo. Para realizar un request necesitamos una **URL** o direccion 
y además un **método** de petición. Los métodos más usados son _GET_ y _POST_. Por ahora vamos a dejarlo con esos dos conceptos: 

1. URL
2. Método(GET, POST)


### Mapeo
Cuando hacemos una petición usando una URL, el servidor debe saber que está pidiendo el cliente. La URL le indica 
que hacer o que componente llamar. Esto es un mapeo que debemos configurar en el servidor para que nuestros componentes puedan ejecutarse
con la petición del cliente.

### Contenedores y Capas
Un servidor está compuesto por varios _**contenedores**_ y estos a la vez son los que alojan nuestros _**componentes**_. En el artículo de 
introducción a Java EE vimos que el server esta divido en dos capas logicas:

1. **Capa web :** Aquí se alojan los componentes que procesan las peticiones del cliente.
2. **Capa de negocio :**  Aquí se alojan los componentes que contienen la lógica de negocio de nuestra aplicacion.

<br/>

![Java EE](/images/javaee-server-capas-01.png)

<br/>

Los contenedores hacen partes de estas capas. Actualmente(Java EE 7) en un middleware(servidor Java EE) podemos encontrar 3 tipos de 
contenedores como lo vemos en la imagen:

![Java EE](/images/javaee-contenedores-02.png)

### Tipo de Componentes
Los nombres como los define el estándar Java EE para nuestro componentes son los siguientes:

* **Servlet :** Componente de capa web
* **JSP :** Componente de capa web
* **JSF :** Componente de capa web
* **Bean EJB :**  Componente de capa de negocio
* **Bean CDI :** Componente de capa web y capa de negocio

Con estos componentes armamos la arquitectura de nuestra aplicación la cual puede ser tan variada según el tipo de componente que usamos. 
Un arquitectura tradicional por lo general está formada por componentes web y componentes de negocio, aunque esto no es una regla rígida 
ya que según el requerimiento se puede usar solo un tipo de componente.

Veamos algunos tipos:

## Arquitectura Ejemplo No. 1
En esta vemos un componente web Servlet que invoca(a través de inyección de dependencias) un componente de negocio EJB y retorna la respuesta al cliente.

![Java EE](/images/javaee-arquitectura-01.png)

<br/>

## Arquitectura Ejemplo No. 2
En este vemos un componente web Servlet que invoca(a través de inyección de dependencias) un componente de negocio EJB y retorna un 
componente JSP como respuesta al cliente.

![Java EE](/images/javaee-arquitectura-02.png) 

<br/>

## Arquitectura Ejemplo No. 3
En esta vemos un componente web XHTML JSF(una página) la cual esta conectada con un componente web Bean JSF(también llamado Backing Bean), 
este Backing Bean invoca(a través de inyección de dependencias) un componente de negocio EJB y retorna la respuesta al cliente a través del mismo XHTML.

![Java EE](/images/javaee-arquitectura-03.png)

<br/>

## Arquitectura Ejemplo No. 4
En esta otra vemos una arquitectura parecida a la No.3 pero usando componentes CDI. Lo especial aqui es que el contenedor CDI puede tener
componentes tanto en la capa web como en la capa de negocio.

![Java EE](/images/javaee-arquitectura-04.png) 

