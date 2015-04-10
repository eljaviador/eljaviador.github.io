---
layout: post
title:  "Disfrazar objetos [Patrón adaptador]"
date:   2013-04-30  12:49:00
published: true
categories: [patrones]
tags: [patrones, principios, poo, arquitectura]
comments: true
shortinfo: Aprende como incorporar componentes incompatibles a tu sistema a través del patron Adaptador.
---

Sacas la tarjeta de memoria de tu celular, una microSD para ver su contenido en tu Laptop. Resulta que tu Laptop tiene 
una ranura para tarjetas SD. Que haces?. Pues consigues un adaptador de memorias y listo. Este ejemplo lo ves a diario 
con otras cosas como adaptadores de corriente de 3 pines a 2 pines.

En el software hay situaciones similares. Cuando decides incorporar algún componente nuevo pero tu aplicación no es compatible 
con la interface de ese nuevo componente. Entonces te preguntas qué hacer? Modificar el componente nuevo es imposible porque 
es una librería de tercero. Modificar tu aplicación para que trabaje con la interface del nuevo componente?. Es una opción 
pero no es lo adecuado.

Aquí es cuando aparece el patrón adaptador al rescate. Este patrón proporciona el puente entre estas dos interfaces 
incompatibles. Hace que el nuevo componente sea lo que tu aplicación espera. Es como un disfraz que se lo colocas a la 
nueva librería y asi tu software cree que es lo mismo que siempre el espera.

## Patrón adaptador
Es un **_patrón estructural_** descrito por GoF que dice así:

> “Convierte la interface de una clase en otra que los clientes esperan. Permite a las clases trabajar juntas que de 
otra forma no podrían por las interfaces incompatibles”.

##  Diagrama de Clases

![Patron Adaptador](/images/patron-adaptador.png)

## Participantes

1.  **Target :** Define la interface especifica que el cliente usa.
2.  **Adapter :** Adapta la interface `Adaptee` a la interface `Target`.
3.  **Adaptee :** La interface que necesita ser adaptada al `Cliente`.
4.  **Client :** Colabora para usar `Target`.

Vamos a ver un ejemplo para estar más claros.

Tienes un clase `Cliente` que recibe una lista para recorrer. Esta lista es una `Enumeration` y posee métodos 
particulares para recorrerla, estos son `hasMoreElements()` y `nextElement()`.

{% highlight java linenos %}
import java.util.Enumeration;

public class Client {

  public void imprimirLista(Enumeration target){
    while(target.hasMoreElements()){
      //Aqui recorremos la enumeracion
    }
  }
}
{% endhighlight %}<br/>

Pero resulta que en la nueva librería, la lista usa la interface `Iterator` que posee otro tipo de métodos para recorrerla, 
`hasNext()` y `next()`.

{% highlight java linenos %}
import java.util.Iterator;

public class NewList implements Iterator {

  public boolean hasNext() {
    return true;
  }

  public Object next() {
    return new Object();
  }

  public void remove() {
    //algun código
  }
}
{% endhighlight %}<br/>

Aqui el metodo `imprimirLista()` de tu clase cliente no permite `Iterator` si no `Enumeration`. Así que tenemos que crear 
el adaptador de `Iterator` en `Enumeration` que es la que espera tu clase cliente.

{% highlight java linenos %}
import java.util.Enumeration;
import java.util.Iterator;

public class IteratorEnumerationAdapter implements Enumeration{

  private Iterator iterator;

  public IteratorEnumerationAdapter(Iterator iterator) {
    this.iterator = iterator;
  }

  public boolean hasMoreElements() {
    return iterator.hasNext();
  }

  public Object nextElement() {
    return iterator.next();
  }
}
{% endhighlight %}<br/>

Ahora veamos como queda el uso de esta estructura de clases.

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {

    Client client = new Client();
    NewList adaptee = new NewList();
    IteratorEnumerationAdapter adapter = new IteratorEnumerationAdapter(adaptee);

    client.imprimirLista(adapter);
  }
}
{% endhighlight %}<br/>

El `Client` recibe felizmente el adaptador que usa internamente el nuevo tipo de lista.

## Conclusión

Este patrón es muy sencillo, usa principios como la delegación y la composición. Se usa el polimorfismo en el `Target` y 
el `Cliente` ni se entera que el objeto real es un Adaptador.