---
layout: post
title:  "Java Tip #2 - Eliminar elementos mientras iteramos colecciones"
date:   2016-03-06  11:40:00
published: true
categories: [javase]
tags: [conceptos, date, poo]
comments: true
shortinfo: Como eliminamos adecuadamente elementos de una lista mientras la recorremos.
---

Una operación no tan común como la de nuestro anterior [_**Tip #1**_]({% post_url 2016-01-12-java-tip-1-colecciones-y-separaciones-por-comas %}) pero igual de necesaria en algunas ocasiones. Como eliminamos un elemento mientras recorremos una lista?

En alguna ocasión el instinto de novato nos ha llevado a hacer algo como esto:

```java?start_inline=1
List<Integer> lista = new ArrayList<Integer>();

//LLenamos la lista para el ejemplo
for(int index = 0; index < 10; index++){
    lista.add(index);
}

//Ahora tratamos de eliminar el elemento 5
for (Integer item : lista) {
    if(item.equals(5)){
        lista.remove(item);
    }
}
```

<br/>

Y bingo nos da una excepción de concurrencia como esta `Exception in thread "main" java.util.ConcurrentModificationException`.

<br/>

Aquí es donde aparecen los iteradores. La interface `Iterator` es nuestra amiga para esta operación, de hecho es la única forma segúra de remover o modificar una colección mientras la recorremos. Toda clase e interface que implemente o extienda de `Iterable<T>` retorna un `Iterator`.

La interface `Collection` extiende de `Iterable` y por ende, todas sus subinterfaces `List`, `Set`, `Queue` tiene acceso a un `Iterator`.

#### Eliminar un elemento

```java
List<Integer> lista = new ArrayList<Integer>();

//LLenamos la lista para el ejemplo
for(int index = 0; index < 10; index++){
    lista.add(index);
}

//Ahora eliminamos el elemento 5
Iterator<Integer> iterator = lista.iterator();
while (iterator.hasNext()){
    Integer item = iterator.next();
    if(item.equals(5)){
        iterator.remove();
    }
}
```

<br/>


#### Y que pasa con `Map`?

Podmemos sacar el `Iterator` del `Set` de entries del mapa:

```java
Map map = new HashMap<>();

//LLenamos el mapa para el ejemplo
for(int index = 0; index < 10; index++){
    map.put(index, index);
}

//Ahora eliminamos el elemento 5
Set entrySet = map.entrySet();
Iterator iterator = entrySet.iterator();
while (iterator.hasNext()){
    Map.Entry item = (Map.Entry) iterator.next();
    if(item.getKey().equals(5)){
        iterator.remove();
    }
}
```

<br/>


