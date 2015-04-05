---
layout: post
title:  "Dices o preguntas"
date:   2012-04-19  13:28:00
categories: arquitectura
shortinfo: Principio de programación de la POO.
---

### Conversación UNO:
*   Usuario : Tablero, tu posición 5,6 esta vacía?
*   Tablero : Porque preguntas?
*   Usuario : Es que quiero mover una ficha a esa posición.
*   Tablero : Mejor dime que mueva la ficha y yo veo si ella esta vacía.

### Conversación DOS:
*   Usuario : Factura cuantos artículos tienes?
*   Factura : Porque preguntas?
*   Usuario : Es que quiero aplicar un descuento si tienes mas de 10 artículos.
*   Factura : Mejor dime que aplique un descuento y yo verifico que tenga mas de 10 artículos;

En estas dos sencillas conversaciones tu cerebro identifica rápidamente que el tablero y la factura tienen razón. 
Pero también estarás de acuerdo conmigo que no siempre es tan intuitivo aplicar este simple principio.

# Tell, Don't ask (TDA)
Como identificamos cuando tenemos un código muy, muy procedural o un código mas orientado a objetos?. El codigo procedural 
tiende a obtener alguna información y de acuerdo a esa información tomar una decisión para hacer algo. A un objeto no se 
le debería preguntar por su estado para tomar una decisión y decirle luego que ejecute determinado método.El código orientado 
a objeto se le dice a este que haga algo. Si se ha de tomar alguna decisión en base al estado lo debe hacer el mismo.

Si necesitamos hacer algo de acuerdo al estado del objeto entonces esa responsabilidad debe estar dentro del objeto y que 
el mismo verifique su propio estado. Aquí estamos aplicando el concepto de **_encapsulacion_** y **_bajo acoplamiento_**
(otra vez estos términos, jum como que son importantes en la POO :)).

La orientación a objetos (POO) se trata de eso, _unificar datos y comportamiento relacionados entre si_.
El uso de getters para obtener información de los objetos esta bien, eso no es malo. El problema aparece cuando teniendo 
esa información desde afuera operas sobre ese mismo objeto. Esto lleva a duplicar el código. Seguir este principio evita 
tener reglas de negocio regadas por todos lados y asegura que las responsabilidades están puestas en los sitios adecuados.

Por ultimo te dejo aquí un pequeño fragmento para hacernos una idea de lo que no es adecuado hacer:

{% highlight java linenos %} 
Ficha f1 = new Ficha(); 
Ficha f2 = new Ficha(); 
Tablero tablero = new Tablero(f1, f2);
boolean vacia = tablero.estaVaciaPosicion(5, 6);
if(vacia){ 
   tablero.moverFicha(f2, 5, 6); 
}
{% endhighlight %}<br/>

Y tu, ¿dices o preguntas? :)