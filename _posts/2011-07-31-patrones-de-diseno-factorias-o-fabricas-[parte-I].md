---
layout: post
title:  "Patrones de Diseño - Factorias o Fabricas [01]"
date:   2011-07-31  13:28:00
categories: arquitectura
---

Uno de los patrones de diseño creacionales que mas abunda en la POO son los denominados fábricas o factorías. Se asemejan 
mucho a tener una fabrica de cosas, en este caso de objetos, de allí su nombre.

Al investigar un poco te darás cuenta que existen casos en donde la terminología para este patrón puede ser usada con 
imprecisiones llevando así a crear confusión en su uso.

Aunque el libro de [GoF](http://goo.gl/3vtZb5) documenta **Abstract Factory** y **Factory Method**, podríamos encontrar 
otras formas de crear objetos que al final serian fácilmente identificables como factorías pero no como un patrón de diseño 
como tal. El objetivo es común siempre, crear cosas u objetos.

Veamos una forma simple y común denominada **_static factory_**. Esta es una simple clase(podría ser abstracta o tener el 
constructor privado para no instanciarla) con un método static que podria o no recibir parámetros. Teniendo una interface 
retornamos una determinada implementación, pero pediremos cual implementación retornar a través de un parámetro.

{% highlight java linenos %} 
public interface Printer{ 
   void print(String texto); 
}

public class DotMatrixPrinter implements Printer{ 
   public void print(String texto) { 
      // Impresion de puntos 
   } 
}

public class InkjetPrinter implements Printer{ 
   public void print(String texto) { 
      // Impresion de tinta 
   } 
}

public class LaserPrinter implements Printer{ 
   public void print(String texto) { 
      // Impresion de laser 
   } 
}

public enum PrinterType{ 
   DOT_MATRIX, INKJET, LASER; 
}

public class PrinterFactory{
   public static Printer getPrinter(PrinterType type){ 
      Printer p = null; 
      switch(type){ 
         case DOT_MATRIX: 
            p = new DotMatrixPrinter(); 
            break; 
         case INKJET: 
            p = new InkjetPrinter(); 
            break; 
         case LASER: 
            p = new LaserPrinter(); 
            break; 
      } 
      return p; 
   } 
}

public class Client{ 
   public static void main(String[] args) { 
      Printer printer = PrinterFactory.getPrinter(PrinterType.LASER); 
      printer.print("Texto a imprimir"); 
   } 
} 
{% endhighlight %} 

Con un método, en este caso estático obtenemos un `Printer`. El detalle aquí es que el código que crea las implementaciones 
esta en la misma clase que las retorna, en este caso `PrinterFactory`.

En otra entrega veremos los dos patrones factory detallados en el libro de GoF.

[![](/images/book_gof.jpg "Libro en Amazon"){: .imgH300}](http://goo.gl/3vtZb5)<br/>