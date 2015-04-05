---
layout: post
title:  "Pensando en objetos [Abstracción]"
date:   2011-01-14 18:10:04
published: true
categories: [conceptos y principios]
tags: [abstracción, poo]
shortinfo: Comprendiendo el concepto de Abstración asociado a la programación orientada a objetos.
---

Una de las características más importante en la programación orientada a objeto (POO en adelante) es que me permite expresar _**conceptos**_ del mundo real tal y como son.
Así como existe un vehículo en mi mundo real, tal cual lo puedo recrear en la POO.

En mi mundo real las cosas(objetos) tienen unas _**características**_ y también pueden tener una _**funcionalidad**_. 
Existen dos términos importantes que son:

* **Estado :** El estado del objeto está representado por sus características o rasgos particulares, también llamados atributos.
* **Comportamiento :** El comportamiento está representado por las operaciones o cosas que puede realizar este objeto, también llamados métodos.<br/><br/>

La base para empezar a recrear mi pequeño mundo orientado a objetos es saber identificar esto ya que el proceso de abstracción implica conocer el ecosistema y el problema que estamos tratando de modelar. La definición de abstracción puede tener cierta similitud en diferentes áreas.

1. **Filosofía :** Acto mental para aislar conceptualmente las propiedades de un objeto.
2. **Psicología :** Reducción de los componentes fundamentales de información de un fenómeno.
3. **Informática :** Agrupar características comunes y esenciales de un grupo de objetos.<br/><br/>

Entonces aquí tenemos uno de los principales conceptos en la POO :

> _Abstracción, agrupación de características esenciales para modelar mi problema_.

Con este proceso de abstracción en la POO puedo tener algunas ventajas significativas en el desarrollo de software:

1. **Concentrarse en pocas cosas :** Al tener un modelo muy bien definido puedo reducir la complejidad y puedo identificar con mayor facilidad lo que quiero.
2. **Representación real del mundo :** Me siento más cómodo al trabajar con mis componentes u objetos ya que me los imagino en la realidad de mi ecosistema como son y cómo interactúan entre si.
3. **Olvidar los detalles de implementación :** Es una consecuencia de concentrarme en **que es lo que debe hacer un objeto**, el como lo hace  queda en segundo plano.<br/><br/>

Es importante entender lo que quiero solucionar para lograr un diseño funcional adecuado a mis necesidades, si quiero diseñar un sistema de inventarios debemos saber que nos encontraremos con Productos, Clientes, Facturas, etc. Así que entienda primero lo que quiere hacer para que realice un buen proceso de abstracción

En las próximas entradas veremos el resto de los conceptos principales en la POO: encapsulación, herencia y polimorfismo.