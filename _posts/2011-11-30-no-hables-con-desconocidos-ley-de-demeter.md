---
layout: post
title:  "No hables con desconocidos [Ley de Demeter]"
date:   2011-11-30  13:28:00
published: true
categories: [conceptos y principios]
tags: [conceptos, principios, poo, arquitectura]
comments: trueAd
shortinfo: Principio para conocer el límite de comunicación entre componentes y optmizaar el bajo acoplamiento.
---

La cohesión y el acoplamiento son dos de los temas mas importante en el desarrollo de sistemas orientados a objeto. 
Estos han sido tratados desde los 60's cuando iniciaba la POO y como una parte vital de este mundo.

La ley de demeter apareció en el 87 y fue propuesta por _**Ian Holland**_ mientras trabajaba en un proyecto llamado **Demeter**, 
de ahí su nombre. 

El objetivo es el bajo acoplamiento teniendo un limite de conocimiento sobre otros componentes o unidades de código. 
_**Es decir solo tener contacto con los amigos mas cercanos.**_ Veamos cual es la propuesta.

Un método solo debe hablar:

1.  Con métodos de la misma clase.
2.  Con métodos de sus atributos.
3.  Con métodos de sus parámetros.
4.  Con métodos de objetos que el instancia.

![Ley de Demeter](/images/law_of_demeter.jpg)<br/><br/>

La idea es no hacer cadenas de llamadas como

{% highlight java linenos %}
a.getX().getY().getZ();
{% endhighlight %}<br/>

y si ocurren es porque algo esta donde no es. Es posible que Z debería estar en X y no en Y.

Aunque puedes encontrar debates sobre esto, en general pienso que es un buen principio de diseño que ayuda a mantener un 
código limpio y con bajo acoplamiento. En algún momento recuerdo haber visto una discusión sobre si una `Factura` te da 
el `Cliente`:

{% highlight java linenos %}
factura.getCliente();
{% endhighlight %}<br/>

y la vista desea mostrar el nombre del cliente tendría que hacer algo como:

{% highlight java linenos %}
factura.getCliente().getNombre();
{% endhighlight %}<br/>

Algunos decían que la factura debería entonces dar el nombre del cliente:

{% highlight java linenos %}
factura.getNombreCliente();
{% endhighlight %}<br/>

esto basándose en lo anterior, pero yo personalmente me inclino mas por que un método en la vista reciba el `Cliente` y obtenga 
el nombre.