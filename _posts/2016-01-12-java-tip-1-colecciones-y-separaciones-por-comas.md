---
layout: post
title:  "Java Tip #1 - Colecciones y Separaciones por Comas"
date:   2016-01-12  11:40:00
published: true
categories: [javase]
tags: [conceptos, collections, list]
comments: true
shortinfo: Cual es ma mejor forma de convertir una colección en una cadena de valores separados por coma.
---

Crear un `String` de elementos separados por comas son unas de las operaciones que nunca faltan en nuestro código. Por lo general tenemos como fuente una _lista_ o _array_ de valores para realizar esto. 

Aún veo código como el siguiente:

{% highlight java linenos %}
List<String> lista = new ArrayList<String>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

StringBuilder sb = new StringBuilder();
for (String s : lista) {
    sb.append(s).append(",");
}

sb.deleteCharAt(sb.length() -1);

System.out.println(sb.toString());
{% endhighlight %}<br/>

Nuestro resultado evidentemente cumple con el objetivo:

{% highlight bash %}
UNO,DOS,TRES,CUATRO
{% endhighlight %}<br/>

Aunque considero que esa última línea `sb.deleteCharAt(sb.length() -1)` esta demás si usamos otro enfoque que no use código fuera del loop.

<br/>

#### Opcion 1 : Antes de Java 8

{% highlight java linenos %}
List<String> lista = new ArrayList<>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

StringBuilder sb = new StringBuilder();
Iterator<String> it = lista.iterator();
while(it.hasNext()) {
    sb.append(it.next());
    if (it.hasNext()) sb.append(","); //la parte interesante
}

System.out.println(sb.toString());
{% endhighlight %}<br/>


#### Opcion 2 : Usando Java 8
       
Desde la versión 8 la clase `String` trae el método `join()`.
       
{% highlight java linenos %}
List<String> lista = new ArrayList<>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

//Puede recibir la misma lista si es de Strings
String result1 = String.join(",", lista);
System.out.println(result1);

//O puede recibir un array de String
String[] stringArray = lista.toArray(new String[]{});
String result2 = String.join(",", stringArray);
System.out.println(result2);

{% endhighlight %}<br/>
       

#### Opcion 3 : Usando Java 8
Usamos la clase `StringJoiner` para este trabajo.


{% highlight java linenos %}
List<String> lista = new ArrayList<>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

StringJoiner joiner = new StringJoiner(",");
for (String s : lista) {
    joiner.add(s);
}
System.out.println(joiner.toString());
{% endhighlight %}<br/>


#### Opcion 4 : Usando una librería
Existen multiples librerías que nos ayudan en esto. Una de esas es `common-lang` con la clase `org.apache.commons.lang.StringUtils`:


{% highlight java linenos %}
List<String> lista = new ArrayList<>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

String result = StringUtils.join(lista, ',');

System.out.println(result);
{% endhighlight %}<br/>

También **SpringFramework** nos proporciona la clase `org.springframework.util.StringUtils`:

{% highlight java linenos %}
List<String> lista = new ArrayList<>();
lista.add("UNO");
lista.add("DOS");
lista.add("TRES");
lista.add("CUATRO");

String result = StringUtils.collectionToCommaDelimitedString(lista);

System.out.println(result);
{% endhighlight %}<br/>


