---
layout: post
title:  "Estructura de Proyectos Empresariales"
date:   2015-06-10 20:30:00
published: true
categories: [javaee]
tags: [javaee, war, jar, ear]
comments: true
shortinfo: Conociendo la forma como se organiza un proyecto empresarial para ser instalado en un servidor o contenedor.
---

En el [artículo anterior]({% post_url 2015-05-30-arquitectura-aplicaciones-empresariales %}) vimos un poco sobre arquitecturas 
de un proyecto con Java EE. En este vamos a detallar la estructura para distribuir nuestras aplicaciones y que puedan ser instaladas
en un servidor Java EE.

**NOTA:** En el mundo Java EE existen servidores de aplicaciones, los cuales tienen dentro de si los contenedores y donde alojamos 
nuestros componentes. Tambien existen de forma individual _**Contenedores**_ como el caso de **Apache Tomcat** 
o **Jetty** en los cuales solo podemos trabajar con componentes web.
 
 
## Tipos de Empaquetados
La plataforma Java define varios tipos de paquetes para distribuir nuestras aplicaciones. Según el tipo de aplicacion podemos usar
uno u otro. No voy a definirlos ya que en internet se puede encontrar la definición detallada de cada uno. Solo vamos a enfocarnos
en la estructura interna de archivos **EAR**, **WAR** y **JAR**.

&nbsp; 
 
#### Archivo EAR
Un archivo .ear puede contener la siguiente estructura:

![Java EE](/images/paquete-ear-01.png)

&nbsp;

Un archivo .ear contiene un application.xml que define las aplicaciones web(.war) y los modulos ejb(.jar) que contiene.
Opcionalmente podemos tener librerias de terceros que son compartidas por todos los modulos.

&nbsp;

#### Archivo WAR
Un archivo .war puede contener la siguiente estructura:

![Java EE](/images/paquete-war-01.png)

&nbsp;

#### Archivo JAR
Un archivo .jar puede tener varios usos, uno de los cuales puede ser para empaquetar una aplicación stand-alone y simplemente ejecutando
una línea de comando como `java -jar archivo.jar com.acme.ClaseMain` nuestra clase princinpal `ClaseMain` arrancará. 

También es usado para empaquetar modulos EJB el cual puede tener la siguiente estructura:

![Java EE](/images/paquete-jar-01.png)

&nbsp;

Te recomiendo leer el artículo relacionado con cargadores de clases:

[Cargado de Clases]({% post_url 2014-08-31-cargador-de-clases %})