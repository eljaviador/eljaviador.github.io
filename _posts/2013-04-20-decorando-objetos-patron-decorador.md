---
layout: post
title:  "Decorando objetos [Patrón decorador]"
date:   2013-04-20  08:29:00
published: true
categories: [patrones]
tags: [patrones, principios, poo, arquitectura]
comments: true
shortinfo: La herencia es un tema importante el la POO, aquí vemos como usarla con sabiduría para extender el software.
---

La herencia es una característica muy potente de la POO pero mal usada trae consigo problemas de flexibilidad, haiciendo 
que los diseños sean _**mas rígidos y menos mantenibles**_.

Existen un par de principios de diseño de software que dicen:

1.  **Composición sobre herencia.** (Composition over inheritance).
2.  **Abierto para extender cerrado para modificar.** (Open for extension, but closed for modification).

Estos dos principios son el corazón de un patrón de diseño estructural llamado **_Decorator_**. Algunas veces también 
conocido como _**Wrapper o envoltorio**_. Este patrón de diseño permite dar a los objetos nuevas responsabilidades sin 
cambiar el código de estos.

Veamos una posible situación para esto:

Tienes un componente `InputText` que captura el texto introducido por un usuario. Este componente es usado en muchas partes 
de tu aplicación. Un día deciden que es necesario que este componente capture solo letras y además permita convertirlas a mayúsculas.

Te arriesgas a modificar tu componente? Es realmente necesario?

La solución: **_decoralo_** o **_envuelvelo_** con otro.

La idea es que el comportamiento existente en `InputText` sea **_heredado por composición y no por herencia_**. La nueva 
funcionalidad es delegada en algún otro objeto en concreto. Esto reduce el riesgo de romper funcionalidades existentes o 
causar efectos secundarios no deseados.

## Decorator Pattern
Este es un patrón estructural detallado por GoF que dice:

> Agrega nuevas responsabilidades a un objeto dinámicamente. Los decoradores proporcionan una alternativa flexible a la 
herencia para extender funcionalidad.

## Estructura de clases

![Patron Decorador](/images/decorator-pattern.png)<br/>

#### Participantes

1. **Componente:** Supertipo que especifica la funcionalidad a decorar.
2. **ConponenteConcreto:** Objeto base a decorar. Al que se le adiciona responsabilidades.
3. **Decorador:** Mantiene una referencia de Componente y define una interface igual a este.
4. **DecoradorConcreto:** Decora una funcionalidad. Adiciona comportamiento.

_Decorador_ extiende de _Componente_ solo porque necesita ser **_el mismo tipo_** de lo que va a decorar. Esta herencia no 
es para obtener el comportamiento. El comportamiento lo obtenemos por composición ya que Decorador tiene un atributo que 
referencia a Componente.

## Aplicando decoradores a nuestro InputText

{% highlight java linenos %}
public interface InputText {
  String read();
}

public class BasicInputText implements InputText {

  private String text;

  public BasicInputText(String text) {
    this.text = text;
  }

  public String read() {
    return text;
  }
}

public abstract class InputTextDecorator implements InputText{

  private InputText inputText;

  protected InputTextDecorator(InputText inputText) {
    this.inputText = inputText;
  }

  public String read(){
    return inputText.read();
  }
}

public class AlphabeticTextDecorator extends InputTextDecorator {

  protected AlphabeticTextDecorator(InputText inputText) {
    super(inputText);
  }

  public String read() {
    String result = super.read();
    //Funcionalidad adicional
    char[] chars = new char[result.length()];
    for(int x = 0, y = 0; x < result.length(); x++){
      char character = result.charAt(x);
      if(Character.isLetter(character)){
        chars[y]=character;
        y++;
      }
    }
    result = String.valueOf(chars).trim();
    return result;
  }
}

public class UpperTextDecorator extends InputTextDecorator {

  protected UpperTextDecorator(InputText inputText) {
    super(inputText);
  }

  public String read() {
    String result = super.read();
    //Funcionalidad adicional
    return result.toUpperCase();
  }
}

public class Principal {
  public static void main(String[] args) {
    InputText text = new BasicInputText("AbCd-98L,.d&");
    text = new AlphabeticTextDecorator(text);
    text = new UpperTextDecorator(text);
    System.out.println(text.read());

    InputText text2 = new UpperTextDecorator(new AlphabeticTextDecorator(new BasicInputText("xYz7-/*erf.")));
    System.out.println(text2.read());
  }
}
{% endhighlight %}<br/>

Aqui la funcionalidad base para la entrada de datos la definimos en una interface `InputText` y la implementa la clase 
`BasicInputText`. Se crea el decorador padre `InputTextDecorator` que hereda de `InputText` para ser del mismo tipo. 

Se crean los decoradores que agregan las funcionalidades específicas `AlphabeticTextDecorator` y `UpperTextDecorator`. 
Estos extienden del decorador padre. Vemos que la operación se convierte en una composición o wrapping recursivo.

En java encontramos este tipo de diseño en el paquete `java.io`, los componentes de flujos(stream) de datos están hechos 
de esta forma.

1. **Componente ==&gt;**`InputStream`.
2. **ComponenteConcreto ==&gt;** `FileInputStream`.
3. **Decorador ==&gt;** `FilterInputStream`.
4. **DecoradorConcreto ==&gt;** `BufferedInputStream`, `LineInputStream`, `DataInputStream`.

{% highlight java linenos %}
public class Principal {
  public static void main(String[] args) throws FileNotFoundException {
    InputStream inputStream = new FileInputStream("mi_archivo.txt");
    inputStream = new BufferedInputStream(inputStream);
    inputStream = new LineInputStream(inputStream);
  }
}
{% endhighlight %}<br/>

En conclusión este patrón ayuda a que no prolifere la herencia como fundamento de tus diseños y no sobrecargar la clase 
base agregando comportamiento.

Hay que tener cuidado con la generación excesiva de decoradores. Elegir con cuidado las funcionalidades que merecen ser 
decoradas o envueltas.