---
layout: post
title:  "Observando a otros [Patron observer]"
date:   2012-10-18  13:28:00
published: true
categories: [patrones]
tags: [patrones, principios, poo, arquitectura]
comments: true
shortinfo: Conoce como desarrollar una comunicación eficiente entre los componentes de tu sistema con este patron de diseño. 
---

Qué pasa si tienes uno o varios objetos que necesitan saber cuando cambia algún valor en otro objeto?. Consultar al objeto 
del cual necesito el valor cada determinado tiempo?

Un reloj tiene un display que muestra la temperatura actual basándose en un sensor interno de dicho reloj. Esto quiere decir, 
que el display del reloj debe estar consultando por ejemplo cada 30 segundos el valor actual del sensor para mostrarlo. 
Puede que funcione, pero no es lo adecuado.

Mejor es que el sensor notifique al display al haber un cambio de temperatura y este lo muestre.  Si no cambia la temperatura 
en 10 minutos no hay necesidad que el display consulte cada 30 segundos al sensor.

Pues la idea del patrón Observador es esta. Un `Subject`, en este caso el sensor, y uno o varios `Observers` como el 
display del reloj. Los observers deben registrarse con el subject, esto es como una especie suscripción. Así como cuando nos 
suscribimos a un servicio este le envía la notificaciones a todos sus suscriptores.

## Patrón Observer
Define una dependencia de uno-a-muchos entre objetos tal que cuando un objeto cambia su estado, todas sus dependencias son 
notificadas y actualizadas automáticamente.

Diagrama de clases:

![Patron Observer](/images/observer-pattern.jpg)

El `Subject` u objeto observable posee una lista de observers u objetos observadores. Esta lista se llena a medida que agregamos 
objetos `Observer`, es decir cada vez que suscribimos uno. También pueden ser removidos de esa lista.
Los Observadores en concreto necesitan implementar la interface `Observer` para que estos puedan ser agregados.

Cuando el objeto observable(subject) cambia de estado este debe llamar al `notifiyObservers()`, el cual lógicamente debe recorrer 
la lista que tiene de observadores e ir llamando el método `update()` de cada observador.
Este método `update()` debe recibir los datos actualizados del Subject con una referencia de algún tipo de datos.

Veamos un ejemplo:

{% highlight java linenos %}
public interface Observer {
  void update(Serializable value);
}

public interface Subject {

  void addObserver(Observer o);
  void removeObserver(Observer o);

  void notifyObservers();

}

public class Display implements Observer {

  public void update(Serializable value) {
    //Recibimos la notificacion y mostramos el valor
    showValue(value);
 }

  private void showValue(Serializable value){
    System.out.println("Valor =" + value);
  }
}

public class Sensor implements Subject {

  private List<Observer> observers = new ArrayList<Observer>();
  private Integer value;

  public void addObserver(Observer o) {
    observers.add(o);
  }

  public void removeObserver(Observer o) {
    // Codigo para remover Observer de la lista
  }

  public void notifyObservers() {
    for(Observer o : observers){
      o.update(this.value);
    }
  }

  public void setState(Integer value) {
    this.value = value;
    notifyObservers();
  }
}

public class Principal {

  public static void main(String[] args) {

     Display display = new Display();
     Sensor sensor = new Sensor();
     sensor.addObserver(display);

     sensor.setState(100);

  }
}
{% endhighlight %}

Estamos haciendo manual el cambio del valor para el sensor, pero el cambio de este valor puede ser automático. Lo importante 
es que se notifique a los observadores.

Que debemos notificar a los observadores?. Tienes varias opciones para hacer esto, podemos pasar un objeto `Serializable` como 
en el ejemplo o puedes pasar la referencia del Sensor y luego obtener el valor de esta referencia. Aunque prefiero el uso 
de `Serializable`.

## `java.util.Observer` y `java.util.Observable`
Dentro de Java tenemos soporte a esto, es decir el lenguaje nos brinda las interfaces y clases necesarias para trabajar con 
observadores y subjects. La interface `java.util.Observer` y la clase `java.util.Observable` nos ayudan en esta tarea. 
Notemos que `Observable` es una clase y debemos extender de ella para heredar varios métodos útiles.

{% highlight java linenos %}
public class Display implements java.util.Observer{

  public void update(Observable o, Object arg) {
    Integer value = (Integer) arg;
    showValue(value); // Mostramos el valor
  }

  private void showValue(int temp){
    System.out.println("Temperatura = " + temp);
  }
}

public class Sensor extends java.util.Observable{

  private int temperatura;

  public void setValue(int value){
    temperatura = value;
    notifyObservers(temperatura);
  }

}

public class Principal {

  public static void main(String[] args) {
    Display d = new Display();
    Sensor s = new Sensor();

    s.addObserver(d);

    s.setValue(50);

  }
}
{% endhighlight %}

Otro caso común lo podemos ver por ejemplo en la administración de eventos de _Swing_, donde a un Botón ( observado) se 
le registra un Listener(observador). Cuando algo pasa en el botón, por ejemplo un click, este le notifica a sus 
observadores(listeners), y estos harán lo suyo.