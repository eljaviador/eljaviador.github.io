---
layout: post
title:  "Patrones de diseño - Solucionando problemas"
date:   2011-05-31  13:28:00
categories: arquitectura patrones
---

Cuando construimos software nos encontramos a menudo con todo tipo de situaciones y problemas que debemos solucionar creando un 
diseño adecuado con nuestro modelo de clases. 

Para afrontar estos retos indudablemente se debe tener una base muy solida de 
conocimientos del paradigma orientado a objetos.Poco a poco desde que se popularizo la POO se han ido catalogando y publicando 
una serie de guías de diseño para dar solución a varios problemas presente en el desarrollo orientado a objetos. 

Personas detrás del desarrollo de lenguajes como SmallTalk, Eiffel o C++ han sido los artífices de esto. Aunque sin duda alguna la 
publicación mas conocida en este ambito es **_Design patterns: elements of reusable object-oriented software_**, escrito por 
Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides.

[![](/images/gof_book.jpg "Libro en Amazon"){: .imgH300}](http://goo.gl/3vtZb5)<br/>

La idea fundamental detrás de un patrón de diseño es identificar una 
situación o problema y describir una estructura o modelo que recree la solución de este, así uno de sus objetivos es probar y 
documentar esto para ser reutilizado.

Un patrón de diseño debe describir los elementos involucrados en un problema e identificar **_el rol_**, **_la relación_** y
**_la responsabilidad_** de cada uno.

Saber identificar cuando es realmente necesario el uso adecuado de patrones de diseño es una habilidad que se aprende con el tiempo, 
no pretendamos que al terminar de leer un libro sobre desarrollo de software nos convertiremos en unos gurus del diseño porque 
esto puede llevar a que sobrecarguemos nuestra aplicación con diseños innecesarios, dándole a esta un grado de complejidad mayor.

Los patrones de diseño están catalogados en tres categorías:

1.  **Patrones de creación :** Nos permiten definir la mejor forma de crear objetos según determinadas situaciones. 
Existe un sin numero de casos en que podemos crear instancias de nuestras clases.
2.  **Patrones de comportamiento :** Estos patrones especifican la forma como se comunican nuestros objetos, es decir como 
interactuan entre ellos.
3.  **Patrones estructurales :** Estos definen la forma como podemos **_organizar_** y **_combinar_** nuestras clases y objetos 
para crear un modelo que satisface nuestros requerimientos en una determinada situación.

En cada una de estas categorías encontramos varios tipos de situaciones que iremos describiendo en las próximas entregas.