---
layout: post
title:  "Pensando en objetos [Encapsulación]"
date:   2011-01-29 18:10:04
published: true
categories: [conceptos y principios]
tags: [encapsulación, poo]
shortinfo: Comprendiendo el concepto de Encapsulación asociado a la programación orientada a objetos.
---

Anteriormente habíamos hablado del proceso de Abstracción, este me ayuda a definir lo que es inherente al modelado de mi problema. Por ejemplo si tratamos de modelar un objeto Televisor, podemos pensar en que características y funciones debe tener este:

1. Un botón de encender / apagar.
2. Un par de botones para subir / bajar volumen.
3. Un par de botones para cambiar  el canal (subir / bajar)

![Encapsulacion-01](/images/encapsulacion-01.gif) 

Dejémoslo así sencillo. <br/><br/>

Ahora después de este arduo proceso de abstracción, debo tomar todos esos conceptos que agrupe y meterlos en una bolsa o saco.

> _Encapsulación, tomar mi abstracción y ponerla dentro de una entidad_.

Esta entidad resultante debería tener características y funcionalidades que estén fuertemente relacionadas, que sea coherente con lo que representa. Esta entidad es mi molde de creación de objetos. Es lo que conocemos como _Clase_ y el objeto resultante formado de este molde es lo conocemos como _Instancia_. Por eso decimos que:

* **Una Clase** es una representación abstracta de un grupo de objetos.
* **Una Instancia** es una representación física real de este tipo de dato abstracto.<br/><br/>

Es importante no confundir la encapsulación con _**ocultamiento de información**_, que es un mecanismo que me brinda el lenguaje para definir que cosas son o no visibles al mundo exterior y en que nivel de visibilidad.

En la encapsulación podemos definir una _**interfaz pública**_ y manipular a través de ella las características de mis objetos para tener un mejor control en muchos aspectos.

Ahora el ocultamiento de información es algo que es fácil de entender por si misma. Podemos definir niveles de visibilidad para los atributos y métodos de un objeto, en Java conocemos 4 tipos de visibilidad:

1. **Publica :** Visible para todos.
2. **Protegida :** Visible para objetos hijos (concepto de herencia).
3. **Default :** Visibles para todos los objetos del mismo grupo (concepto de paquete en java)
4. **Privada :** Visible solo dentro del mismo objeto.
