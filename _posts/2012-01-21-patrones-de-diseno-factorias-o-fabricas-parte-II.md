---
layout: post
title:  "Patrones de Diseño - Factorias o Fabricas [Parte II]"
date:   2012-01-21  13:28:00
categories: patrones
---

En el [artículo anterior]({% post_url 2011-07-31-patrones-de-diseno-factorias-o-fabricas-parte-I %}) hablábamos un poco 
sobre el concepto de las factorías. 


El porque `new` debe usarse con sabiduría ya que este crea una fuerte dependencia entre nuestros objetos. El código se acopla 
demasiado cuando no es necesario.

## Factory Method
Ahora veamos el patrón que se conoce como **_factory method_**. La definición técnica de este patrón es:

>Define una interface para crear objetos, pero deja que subclases decidan que clase crear.

Vamos a empezar un código sencillo para ir desglosando la utilidad del patrón.

{% highlight java linenos %}
interface Document { 
   public void addText(String text); 
}

class PdfDocument implements Document{ 
   public void addText(String text){ 
      System.out.println("Texto en pdf..." + text); 
   } 
}

class XmlDocument implements Document{ 
   public void addText(String text){ 
      System.out.println("Texto en xml..." + text); 
   } 
}

class DocumentCreator { 
   public Document getDocument(String type){ 
      Document doc = null; 
      if(type.equals("pdf")){ 
         doc = new PdfDocument(); 
      }else if (type.equals("xml")){ 
         doc = new XmlDocument(); 
      } return doc; 
   } 
}

class TestDocument{ 
   public static void main(String[] args) { 
      DocumentCreator docCreator = new DocumentCreator(); 
      Document doc = docCreator.getDocument("xml"); 
      doc.addText("Hola mundo"); 
   } 
} 
{% endhighlight %}<br/>

El ejemplo muestra un cliente `TestDocument` usando un `DocumentCreator` y usa el metodo `getDocument` con el tipo de documento a crear. 
Este código esta acoplado por el `if` que pregunta el tipo e instancia la clase adecuada. 
Si quisiera crear un nuevo tipo digamos `HtmlDocument` hay que modificar a `DocumentCreator`. 

Vamos a modificar el código para hacerlo mas flexible.

**1.** Eliminamos el `if` dentro de `DocumentCreator` y lo remplazamos(_**encapsulamos lo que varia**_) con otro método. 
En este caso `createDocument()`, que es abstracto. Hacemos la clase abstracta.

{% highlight java linenos %} 
abstract class DocumentCreator { 
   public Document getDocument(){ 
      Document doc = null; 
      doc = createDocument(); 
      //reemplazamos el if 
      return doc; 
   } 
   protected abstract Document createDocument(); 
} 
{% endhighlight %}<br/>

**2.** Ahora creamos **_clases concretas_** que extiendan de `DocumentCreator` **_para crear documentos concretos_**.

{% highlight java linenos %}
class PdfCreator extends DocumentCreator{ 
   protected Document createDocument() { 
      return new PdfDocument(); 
   } 
}

class XmlCreator extends DocumentCreator{
   protected Document createDocument() { 
      return new XmlDocument(); 
   } 
} 
{% endhighlight %}<br/>

**3.** Ya estamos listo a usar nuestra clase creadora concreta en el cliente.

{% highlight java linenos %}
class TestDocument{ 
   public static void main(String[] args) { 
      DocumentCreator docCreator = new XmlCreator(); 
      Document doc = docCreator.getDocument(); 
      doc.addText("Hola mundo"); 
   } 
} 
{% endhighlight %}<br/>

Así hemos desacoplado del `DocumentCreator` la creación de los documentos y se lo hemos dado a clases concretas: 
`PdfCreator` y `XmlCreator`. Este es el objetivo de este patrón que las subclases creadoras, instancien el producto concreto.

Por último dejo una imagen del diseño UML de este patrón y otra del ejemplo para hacer match conceptual.

![Factory Method 01](/images/factory-method-01.jpg)<br/><br/>
![Factory Method 02](/images/factory-method-02.jpg)<br/><br/>

* [Patrones de Diseño - Factorias (Parte 1)]({% post_url 2011-07-31-patrones-de-diseno-factorias-o-fabricas-parte-I %} "Factorias – Parte 1")
