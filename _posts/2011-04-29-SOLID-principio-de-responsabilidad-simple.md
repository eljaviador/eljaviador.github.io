---
layout: post
title:  "S.O.L.I.D Principio de Responsabilidad Simple"
date:   2011-04-29  13:28:00
categories: arquitectura conceptos principios
---

Este es el primero de varios posts sobre los principios [_**S.O.L.I.D**_]({% post_url 2011-03-14-principios-solid %}).

El principio de responsabilidad simple nos dice:

> _There should never be more than one reason for a class to change. - No debe existir mas que una sola razón para que una clase cambie_.

Robert Martin se basó en el concepto de **_Cohesión_** descrito por Tom DeMarco en _Structured Analysis and Systems Specification_, el cual
estaba mas dirigido a los paquetes pero que aplicado a clases mantiene una definición similar.

El punto es que todos los atributos y todas las operaciones de una clase deben estar intrínsicamente relacionados con un solo propósito.
Si encuentro que al modificar una clase tiene dos responsabilidades las debo separar en dos clases respectivamente. Una clase que hace muchas cosas,
es muy grande o muy complicada de seguro tendrá mas de una sola responsabilidad.

En general creo que es un principio aplicable a todas estas cosas como paquetes, clases y métodos.

Veamos un ejemplo teniendo una clase Cuenta:

![Account 1](/images/account1.jpg)<br/>

Aunque tenga sentido que podamos transferir dinero desde una cuenta a otra lo que vemos aquí es una clara violación de este principio al darle a la clase
`Cuenta` la realización de la operación de transferencia. Esta operación debe ser sacada de la clase y llevada a otra que podríamos  llamar por ejemplo
`TransferService`, así nuestra clase cuenta solo tendría operaciones relacionadas a ella misma.

![Account 2](/images/account2.jpg)<br/>

Así vemos como podemos tener nuestro esquema de clases y con sus responsabilidades separadas como debe ser.

Muchas veces por distintas razones tenemos la tendencia de permitir este tipos situaciones y tener muchas cosas que parecen relacionadas en una misma clase,
así lo que logramos con el tiempo es tener un sistema demasiado acoplado llegando a la rigidez y fragilidad de las que habla el Tío Bob.
