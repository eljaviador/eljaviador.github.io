---
layout: post
title:  "Code Smell - Mal olor en tu codigo"
date:   2011-08-16  13:28:00
published: true
categories: [conceptos y principios]
tags: [antipatrones, patrones, principios, conceptos, poo, arquitectura]
comments: true
shortinfo: Conoce sobre este tema intersante y aprende a detectar problemas en el código.
---

Kent Beck(creador de [JUnit](http://www.junit.org/ "JUnit") y precursor de [Extreme Programming](http://goo.gl/M3PNEv "Programación extrema")) 
propuso el termino **Code smell** mientras ayudaba Martin Fowler a escribir su libro sobre refactoring, lo definen como un 
indicio superficial que generalmente esta asociado a un problema mas profundo en el sistema.

Son pequeños detalles en nuestro código que te dicen que algo esta como no debe ser. Esto no significa que tu sistema no funciona.
El termino podría tener una similitud con los [antipatrones de diseño]({% post_url 2011-06-14-antipatrones-metiendonos-en-problemas %}), 
pero es un poco mas superficial, solo un indicio y que no necesariamente representa un problema en tu software. 

Asi como cuando hueles algo... uhm este olor no me gusta... esta como raro :).Algunos Smells interesantes:

## Dentro de clases

**1. Método extenso** (Long method): La longitud del metodo hace mas dificil ver lo que hace.

**2. Lista de parametros extensa** (Long parameter list): Pasar extrictamente lo necesario.

**3. Código duplicado** (Ducplicate code): Obliga a hacer mantenimiento en varias partes.

**4. Clase extensa** (Large class): Clase que esta tratando de hacer demasiado.

**5. Tipo incorporado en nombre** (Type embedded in name): Redundancia en los nombres, clase.addListener(listener) por clase.add(listener).

**6. Nombre no comunicativo** (Uncommunicative name): Colocar nombre el correcto que indica lo que se hace.

**7. Código muerto** (Dead code): Variables, metodos, parametros, clases, fragmentos que no se usan en ninguna parte.

**8. Generalización especulativa** (Speculative generality): No generalizar el código intentando predecir necesidades futuras.

## Entre clases

**1. Obsesión primitiva** (Primitve obsession): Usar tipos primitivos para sustituir datos que pueden ser representados con una clase. Ej: Dinero (cantidad y moneda), teléfono (área y numero).

**2. Clase dato** (Data class): Clases con solo getters y setters de atributos y sin comportamiento(anemic model).

**3. Grupo de datos** (Data clumps): Grupos de atributos que siempre estan juntos en vez de agruparlos en una unica clase.

**4. Legado rechazado** (Refused bequest): Subclases que no quieren o no necesitan todo lo que heredan. Es necesario heredar entonces?

**5. Intimidad inapropiada** (Inappropriate intimacy): Dos clases se conocen demasiado.

**6. Clase perezosa** (Lazy class): Clase que no hace lo suficiente.

**7. Envidia de características** (Feature envy): Métodos que usan intensivamente otra clase que aquella a la que pertenecen.

**8. Cadena de mensajes** (Message chains): Secuencia de llamadas extensa de un método a otro `obj.getAlgo().getOtraCosa().getOtroMas().esto()`.

**9. Intermediario** (Middle man): Cuando una clase delega su trabajo haciendo llamadas a otras clases. Entonces para que existe?

**10. Cambio divergente** (Divergent change): Cambios hechos dentro de una clase que no tienen ninguna relación con las otras funcionalidades de la clase.

**11. Cirugía escopeta** (Shotgun surgery): Cuando se cambia una clase y se produce una cascada de cambios en otras clases.

**12. Jerarquía de herencia paralela** (Parallel Inheritance Hierarchies): Si al crear una subclase de la clase X es necesario hacer otra subclase de otra clase Y.<br/><br/>

Ahora puedes buscar en tu código, empieza a olerlo y a detectar aquello que... uhmm huele como raro :).