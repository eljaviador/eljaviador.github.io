---
layout: post
title:  "Pensando en objetos [Herencia]"
date:   2011-02-13 18:10:04
published: true
categories: [conceptos y principios]
tags: [herencia, poo]
comments: true
shortinfo: Entender la Herencia y es su uso en la programación orientada a objetos.
---

Siguiendo con los conceptos importantes en la POO nos encontramos con la _**herencia**_.
La definición más sencilla y puntual que podemos darle a la herencia es: _**un mecanismo de relación jerárquica**_; así tal cual y no más.
Ahora bien esta relación es muy útil y la puedo aplicar en mi diseño o modelo de objetos para lograr varios objetivos:

1. **Reúso de código :** Al aplicar este mecanismo de relación puedo heredar código desde otra clase a la que se llama padre, así puedo usar código
del _**padre**_ en la clase hija tan solo escribiendo un par de líneas evitando el copiar, copiar y copiar de un lado a otro.
2. **Organización de codigo :** Puedo estructurar mi modelo de una forma más lógica al mundo real, así tendría árboles jerárquicos como
Animal – Mamífero – Gato, Animal – Reptil – Tortuga, etc…
3. **Crear subtipos :**  Puedo obtener otros tipos de objetos hijos que vienen siendo _**subtipos**_ del padre, aunque la definición de un subtipo
como tal este más ligada al _**polimorfismo**_ que a la herencia, podemos decir que esto es una consecuencia de aplicar este mecanismo de relación.<br/><br/>

Los términos a tener en cuenta cuando trabajamos con herencia son:

* Superclase o clase padre.
* Subclase o clase hija.
* Generalización
* Especialización

## Generalización

Supongamos que no tenemos ninguna relación jerárquica. Ahora bien, tengo una numero de clases que me modelan varios animales: Gato, Perro, Serpiente, etc. La generalización aparece cuando tomo este grupo de clases, identifico una serie de _**características y comportamientos comunes**_ y las llevo a otra clase la cual será una superclase de todas ellas, ahí en ese proceso de abstracción estoy generalizando.

## Especialización
Con la especialización ocurre lo contrario, al crear una nueva clase a partir de otra y _**agregar nuevas características y comportamiento**_ estoy especializando un tipo de objeto. Como lo que se hace al finalizar una profesión, por ejemplo el médico se especializa en un área más puntual digamos cardiología, ahora puede comportarse tanto como cardiólogo pero sigue siendo también un médico.
