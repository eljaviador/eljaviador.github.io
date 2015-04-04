---
layout: post
title:  "S.O.L.I.D Principio de Sustitucion de Liskov"
date:   2011-06-29  13:28:00
categories: arquitectura
---

Ahora conoceremos el tercer principio [_**S.O.L.I.D**_]({% post_url 2011-03-14-principios-solid %}).

Este principio introducido por Barbara Liskov en 1987 dice:

> Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it. 
- Funciones que usen punteros o referencias a clases base deben ser capaz de usar objetos de clases derivadas sin conocerlos.

Esto es básicamente una extensión del principio [Abierto-Cerrado]({% post_url 2011-05-16-SOLID-principio-abierto-cerrado %}). Lo que nos dice es que si tenemos una clase y varias subclases 
de esta al usar una referencia a la clase principal esta debe ser capaz de aceptar cualquier objeto de sus clases hijas.

{% highlight java linenos %}
// Tenemos una clase base 
abstract class Operation {
   protected double operator1; 
   protected double operator2;
   
   public void setOperator1(double operator1) { 
      this.operator1 = operator1; 
   } 
   
   public void setOperator2(double operator2) { 
      this.operator2 = operator2; 
   }
   
   public abstract double calcular(); 
   
}

// Una clase derivada de Operation 
class SumOperation extends Operation { 
   public double calcular() { 
      double result = this.operator1 + this.operator2; 
      return result; 
   } 
}

//Otra clase derivada de Operation 
class MultiOperation extends Operation { 
   public double calcular() { 
      double result = this.operator1 * this.operator2; 
      return result; 
   } 
}

class Calculator { 
   //Aqui pasamos una referencia de la clase base 
   public void calcular(Operation op) { 
      //Esa referencia la usamos aqui que es op 
      double result = op.calcular(); 
      System.out.println("Resultado="+result); 
   } 
}

class Main { 
   public static void main(String[] args) { 
      Calculator c = new Calculator();
      SumOperation s = new SumOperation(); 
      s.setOperator1(10); 
      s.setOperator2(4); 
      //El metodo calcular acepta cualquier objeto 
      //de una clase derivada de Operation 
      c.calcular(s);
      MultiOperation m = new MultiOperation(); 
      m.setOperator1(6); 
      m.setOperator2(5); 
      //El metodo calcular acepta cualquier objeto 
      //de una clase derivada de Operation c.calcular(m); 
   } 
} 
{% endhighlight  %}<br/>

Aquí vemos la aplicación de este principio en el método calcular. Este método usa una referencia a la clase base y nunca se 
entera cual es el objeto derivado(subtipo) que se esta usando.