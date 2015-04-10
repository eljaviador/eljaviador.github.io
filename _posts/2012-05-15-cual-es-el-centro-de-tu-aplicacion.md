---
layout: post
title:  "Cual es el centro de tu aplicación"
date:   2012-05-15  13:28:00
published: true
categories: [general]
tags: [arquitectura, base de datos, aplicaciones]
comments: true
shortinfo: Artículo de Robert Martin sobre el uso de bases de datos en las apliacaciones.
---

Cual debe ser el centro de tu aplicacion? La base de datos o los casos de uso?

Una aplicación debería crearse a partir de casos de usos. Estos son los que le dan valor ya que dicen que es lo que tu 
sistema hace realmente. Pero no, lo que hacemos a menudo es crear nuestra aplicación e involucrar la base de datos desde 
el comienzo haciendo de esta el centro.

La tecnología de base de datos y los frameworks que usamos son detalles que se pueden seleccionar después. Cuando es el 
momento adecuado para crear tu base de datos? Cuando sepas como son tus entidades de datos, como se relacionan y como se usan. 

Esto los sabemos después de haber escrito y testeado nuestros **_casos de uso_** y **_reglas de negocio_**.

No dejes que una herramienta decida por ti, no dejes que te use. 

Extracto de un articulo de Robert Martin.

Puedes ver el articulo en ingles [aquí](http://goo.gl/R4jtnr "No DB").