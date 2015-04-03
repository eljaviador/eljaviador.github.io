---
layout: post
title:  "Principios S.O.L.I.D"
date:   2011-03-14 11:00:04
categories: conceptos principios
---

Los principios **S.O.L.I.D** son un conjunto de guías que nos ayudan a _**evitar malos diseños**_ cuando desarrollamos software orientado a objetos. Estos principios fueron reunidos y expuestos por _Robert Martin_, también conocido como “El tío bob” en su libro _“Agile Software Development: Principles, Patterns, and Practices”_.

El buen uso de estos principios nos permite construir software extensible y fácil de mantener,  también nos permiten detectar lo que conocemos como _**code smell**_ de los cuales hablaremos después. El Sr. Martin nos habla también de 3 características importantes que deben ser evitadas en la construcción de nuestro software:

1. **Rigidez :** Al cambiar algo en el sistema, es necesario cambiar otras partes por estar muy acopladas.
2. **Fragilidad :** Al hacer un cambio en el sistema, otras partes que no esperábamos del mismo se rompen.
3. **Inmovilidad :** Dificultad para reutilizar cosas en otro sistema por ser difícil de desenredar del actual.

Si nuestro objetivo es desarrollar buen software no podemos perdernos de su conocimiento que junto con los patrones de diseño nos van a hacer la vida más fácil.

Estos principios son:

* **S**ingle Responsability Principle
* **O**pen / Close Principle
* **L**iskov Substitution Principle
* **I**nterface Segregation Principle
* **D**ependency Inversion Principle

Veamos rápidamente la definición de cada uno. En las próximas entradas los veremos con más detalle.<br/><br/>

#### [Single Responsability Principle [Principio de Responsabilidad Simple]]({% post_url 2011-04-29-SOLID-principio-de-responsabilidad-simple %})
Una clase debe tener una sola razón para cambiar.<br/><br/>

#### [Open / Close Principle [Principio Abierto / Cerrado]]({% post_url 2011-05-16-SOLID-principio-abierto-cerrado %})
Clases, módulos y funciones deben ser abierta para extender y cerrada para modificar.<br/><br/>

#### Liskov Substitution Principle [Principio de Sustitución de Liskov]
Al usar una clase base los subtipos de esta pueden ser cambiados.<br/><br/>

#### Interface Segregation Principle [Principio de Segregación de Interfaces]
Un cliente no debe estar obligado implementar Interfaces que no usa.<br/><br/>

#### Dependency Inversion Principle [Principio de Inversión de Dependencias]
Módulos de alto nivel no deben depender de módulos de bajo nivel. Las dependencias entre estos deben ser sobre abstracciones no sobre clases concretas.<br/><br/>

La buena aplicación de estos principios al comienzo no suele ser tan sencillo, puede tomar cierto tiempo mientras se acumula experiencia y se conoce más el paradigma de objetos. Además de otras ventajas significativas como código fácil de mantener y de testear.
