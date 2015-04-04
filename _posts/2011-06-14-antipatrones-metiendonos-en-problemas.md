---
layout: post
title:  "Antipatrones. Metiendonos en problemas [01]"
date:   2011-06-14  13:28:00
categories: arquitectura
---

Así como existen buenas practicas, también podemos encontramos con malas practicas que en teoría no nos ofrecen soluciones 
optimas y nos llevan a crear malos diseños.

Un antipatrón se presenta como una posible solución, algo que nos parece bueno aplicar para resolver un problema, pero realmente 
lo que hace es crear complejidad, malos hábitos y conducirnos por el camino mas largo y espinoso. Conocerlos y evitarlos es una 
practica muy común en los desarrolladores experimentados, y aun mas identificarlos en el código existente debe ser vital.

Al igual que con los patrones, con los antipatrones podemos mencionar uno de los libros mas conocidos en este tema, 
_**AntiPatterns: Refactoring Software, Architectures, and Projects in Crisis**_, sus autores son William Brown, Raphael Malveau, 
Skip McCormick, y Tom Mowbray. 

[![](/images/book_antipatterns.jpg "Libro en Amazon"){: .imgH300}](http://goo.gl/u9pyLw)<br/>


Los antipatrones los podemos encontrar documentados en varias áreas, no solo en desarrollo de software, aunque este libro se 
enfoca en tres categorías, **_desarrollo de software_**, **_arquitectura de software_** y **_gestión de proyectos de software_**.

Aprender a refactorizar y escribir buen código deben ser metas importantes para ti. Estas son habilidades necesarias a la hora 
de enfrentarse con la construcción y con código lleno de antipatrones y malas practicas de programación no lo olvide. 
Una bibliografía recomendada por mi seria :

*   [**_Refactoring: Improving the Design of Existing Code_**](http://goo.gl/5RkeFn) de Martin Fowler.
*   [**_Clean Code: A Handbook of Agile Software Craftsmanship_**](http://goo.gl/zvdjrG) de Robert Martin.

[![](/images/book_refactoring.jpg "Libro en Amazon"){: .imgH300}](http://goo.gl/5RkeFn)<br/>

[![](/images/book_cleancode.jpg "Libro en Amazon"){: .imgH300}](http://goo.gl/zvdjrG)<br/>

Si no lo ha hecho, es tiempo que empiece a considerar los antipatrones como una parte integral en la construcción de su software 
junto a los patrones de diseño, indiscutiblemente ambos lo llevaran a una buena solución de su problemática.

Veamos la lista de antipatrones:<br/><br/>

## Desarrollo de Software

#### **La clase Dios (The blob)** 
Conocido como el objeto todopoderoso. Esta es la típica clases que monopoliza los procesos, tiene y hace de todo. La solución es 
dividirla y aplicar el principio de simple responsabilidad.<br/><br/>

#### **Lava seca (Lava flow):**
Conocido como código muerto debido a que en el proceso de desarrollo se va adicionando código que al final no se usa. Un 
porcentaje de desarrolladores prefiere crear código en vez de modificar el que existe ya que no fue hecho por ellos. La solución 
es completar el proceso de arquitectura antes de poner en producción el código.<br/><br/>

#### **Descomposición funcional (Functional decomposition):**
Se conoce como antipatrón no orientado de objetos. Ocurre al tratar de implementar un sistema de programación estructurada a POO. 
Desarrolladores acostumbrados a esta forma de programación tratan de adaptar la POO a su sistema. La solución es rediseñar el 
sistema totalmente orientado a objetos.<br/><br/>

#### **Poltergeists:**
Se conoce como la gitana, fantasmas que aparece y desaparece misteriosamente. Clases que no tienen una responsabilidad completa 
solo pedazos y su uso es temporal debido a su corta vida, como esas que usamos solo para llamar un método de otra clase y pasar 
información hacia otro lado. La solución es cazar estos fantasmas y crear un buen diseño.<br/><br/>

#### **Martillo de oro (Golden hammer):**
Conocido como el amigo fiel. Es una aplicación o herramienta de software que se cree la mejor solución para todas las empresa, y 
que es aplicable universalmente. Yo soy el martillo y los demás son clavos (como el refran). La solución es un cambio de 
mentalidad, desarrollar mi herramienta con limites definidos y posibilidades de comunicación con otros componentes o herramientas 
de software.<br/><br/>

#### **Código espagueti (Spaghetti code):**
Aparece en un software que contiene poca estructura o esta es muy pobre y demasiado complicada(enredada). No tiene claridad y se 
entiende muy poco por la conexión sin sentido entre todos los componentes. Por ejemplo el uso indebido de la archiconocida 
sentencia `GOTO` en muchos lenguajes :(. La solución evidentemente es refactorizar y limpiar dicho código.<br/><br/>

#### **Programación copiar y pegar (Cut and paste programming):**
Conocida como codificación portapapeles. Es cuando se copia y se pega código de un lado a otro desde soluciones existentes, se 
identifica por la existencia de segmentos de código similares. La solución es crear algo genérico y que se use en todos los 
sitios donde sea requerido.Quedamos pendientes con las otras dos categorías de las cuales hablaremos próximamente.<br/><br/>