---
layout: post
title:  "El Costo de una Excepción"
date:   2016-03-27  20:01:00
published: true
categories: [javase]
tags: [conceptos, exception, principios, poo]
comments: true
shortinfo: Que parte de lanzar una excepcion es costoso o afecta el rendimiento
---

Cuando creamos objetos `Exception` o alguna subclase de esta, estamos usando el mismo costo que al crear cualquier otro objeto. Ahora bien, la mayoria de los constructores de la clase `Throwable`:

* `public Throwable()`
* `public Throwable(String message)`
* `public Throwable(String message, Throwable cause)`
* `public Throwable(Throwable cause)`

Hacen un llamado implicito al método `fillInStackTrace()`. Si revisamos el código fuente de la clase `Throwable` vemos la llamada a este metodo en cada unos de los construtores anteriores. La firma de este método es el siguiente:

{% highlight java %}
private native Throwable fillInStackTrace(int dummy);
{% endhighlight %}

Ahí esta el detalle. Es un método nativo y todo el costo de crear una _excepción_ esta asociado a el ya que tiene que recorrer toda la pila de llamada para construir el _**stack trace**_: _clases_, _métodos_, _líneas_, etc.

<br/>

#### Como evitar este costo?
Existe un constructor adicional que recibe un `boolean` con el que podemos controlar la llamada al método `fillInStackTrace()`.

{% highlight java %}
protected Throwable(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace)
{% endhighlight %}

Como vemos es un método `protected` y para usarlo debemos crear una clase(excepción personalizada) que extienda de `Exception` o preferiblemente de `RuntimeException`. Todo lo anterior tiene que ver con la creación del objeto.

### Lanzando la excepción
Hay otro tema y es cuando lanzamos la execpción, o mas bien cuando la capturamos. Esta es quizas la parte mas complicada. Mayormente después que creamos la excepción la lanzamos y dependiendo en toda la pila de llamada donde se encuentre el `catch`. Si este esta en el mismo método o contexto(_method inlining_) no tiene practicamente ningún costo. Pero si esta en otro nivel la JVM necesita desevolver toda la pila de llamada tomando esto algo de tiempo. 

Cuando hay métodos `synchronized` de por medio puede tomar incluso un poco mas ya que es necesario liberar los monitores que estan siendo usados en todos los noveles de la pila de llamada.


## Conclusión
Las excepciones estan ahí por algo. Son situaciones excepcionales de nuestra aplicación y que debemos usar con mesura. No hagamos de las execpciones el control de flujo de nuestro sistema. Si podemos salvar un poco de tiempo usando el constructor adecuado no siempre podemos hacer algo al respecto cuando son lanzadas.

