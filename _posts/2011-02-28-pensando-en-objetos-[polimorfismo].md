---
layout: post
title:  "Pensando en objetos [Polimorfismo]"
date:   2011-02-28 18:10:04
categories: conceptos principios
---

Vayamos directo al grano y después entramos un poco más en detalle, veamos enseguida la definición de polimorfismo, y después la vamos a explicar paso a paso.

## Polimorfismo
Manipular o ejecutar el comportamiento particular de diferentes tipos de objetos _**a través de una interfaz común o uniforme**_, ya sea por medio de herencia o por contrato. Ya conocemos como es el mecanismo de herencia, antes de seguir entendamos que es eso de _**contrato**_.

Técnicamente cuando hablamos de un contrato, estamos hablando en términos de POO de una _**Interface**_. Cuando un objeto se compromete a cumplir una Interface (contrato), este debe implementar el comportamiento que dice ese contrato o Interface. Al cumplir ese contrato el objeto actual _**automáticamente puede ser tratado como un objeto del tipo de la Interface**_.

Por ejemplo si existe un contrato o Interface llamada **`Mamífero`** que especifica varias operaciones como `dormir()` y `comer()`, podríamos tener una clase llamada Perro la cual implementara la interface Mamífero y cualquier objeto tipo `Perro` que creemos automáticamente  es al mismo tiempo un tipo `Mamífero`; así donde se pida un tipo `Mamífero` yo podría pasar un objeto `Perro` y este es aceptado porque cumple con el contrato `Mamífero`.

Ahora veamos como es eso de una interfaz uniforme para tratar al polimorfismo?. Si tengo un método que pide una referencia de tipo `Mamífero` que es un contrato:

{% highlight java linenos %}
public void hacerAlgo(Mamifero m){
   m.comer();
}
{% endhighlight %}


Ahora al llamar a el método `hacerAlgo()` puedo pasar un objeto que cumpla con ese contrato `Mamífero`. Si tengo una clase `Perro` y otra clase `Ballena` y ambas implementan ese contrato entonces el método aceptará cualquiera de las dos.

{% highlight java %}
Perro p = new Perro();
hacerAlgo(p);
{% endhighlight %}

Igual si lo hacemos con un objeto `Ballena`:

{% highlight java %}
Ballena b = new Ballena();
hacerAlgo(b);
{% endhighlight %}

Al llamar a `m.comer()` dentro de `hacerAlgo()`, este se comportará según el objeto real que estamos pasando al método, sea un perro o una ballena, cada uno comerá a su manera y de eso se trata el polimorfismo, en este caso la interfaz común es el método `comer()` que estamos invocando dentro del método con la referencia _**m**_ de `Mamifero`.

Este mecanismo es aplicable tanto por contrato como en el ejemplo y también por herencia. El polimorfismo permite que las aplicaciones crezcan y que sean extensibles añadiendo nuevos componentes sin modificar lo que existe.

Podríamos crear nuevos objetos por ejemplo una clase `Caballo` y pasar una instancia de esta al método `hacerAlgo()` y el método no se modificaría, solo se comportaría diferente a la hora de `comer()`.
