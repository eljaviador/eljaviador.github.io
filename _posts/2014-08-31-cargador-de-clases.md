---
layout: post
title:  "Cargador de clases"
date:   2014-08-31 18:10:04
published: true
categories: [conceptos, javase]
tags: [principios, conceptos, arquitectura, javase]
comments: true
shortinfo: Un tema sumamente importante para entender como funciona la JVM y poder resolver varios problemas asociados al uso de clases.
---

Este tema siempre me ha parecido interesante y es de las cosas que muchos programadores no conocen, pero una vez lo entiendes, empiezas a comprender como funcionan muchas cosas en la plataforma Java. Entre ellas el sistema de carga de servidores Java EE, OSGI y otras.

Empecemos desde el comienzo para entender todo esto. La siguiente imagen muestra en una idea general de como funciona la plataforma:

![plataforma_java](/images/plataforma_java.jpg)

&nbsp;

Como ya sabes, un mismo archivo .class pueda ser ejecutado en diferentes sistemas operativos. Este archivo debe ser cargado por cada JVM y ejecutado. La JVM usa un mecanismo llamado carga de clase, que permite registrar en su área de datos los archivos `.class` y los asocia a un tipo **Class**. La JVM usa los cargadores de clases para este proceso.

![jvm_architecture](/images/jvm_architecture.jpg)

&nbsp;

Supongamos que tienes un archivo `HolaMundo.java`, lo compilas y ejecutas.

{% highlight bash %}
java HolaMundo
{% endhighlight %}

&nbsp;

Que proceso ocurre cuando lo ejecutas? Que hace la JVM con las referencias a otras clases usadas dentro de `HolaMundo.java`?.

La JVM carga el tipo **HolaMundo** en su memoria y si dentro de `HolaMundo.java` referencias otro tipo como por ejemplo `MiClaseA.java` esta también sera cargada en demanda por la JVM. Si `MiClaseA.java` referencia a una clase `MiClaseB.java` esta sera también cargada en demanda por la JVM y así sucesivamente. Esto es igual si referencias una clase del lenguaje, por ejemplo String.

El subsistema de carga de clases posee una jerarquía de funcionamiento. Existen 3 tipo de cargadores:

1.  **Bootstrap ClassLoader :** Carga las clases de `/lib` del JRE
2.  **Extensions ClassLoader :** Carga las clases de `/lib/ext` de JRE
3.  **System ClassLoader :** Carga el classpath. También llamado Application ClassLoader

![classloader_hierarchy](/images/classloader_hierarchy.jpg)

&nbsp;

La JVM al cargar en su memoria un `.class`, básicamente crea un objeto de tipo **Class** que tiene todas las características de dicha clase. Este objeto tipo **Class** lo podemos obtener ya que el lenguaje me proporciona un método llamado `getClass()` que esta presente en cada clase que creamos. Veamos un ejemplo:

{% highlight java linenos %}
public class HolaMundo {

   public static void main(String[] args) {

      HolaMundo h = new HolaMundo();
      Class clazz = h.getClass();

      String clazzName = clazz.getName();
      System.out.println(clazzName);

   }
}
{% endhighlight %}

&nbsp;

La clase **Class** posee mas métodos interesantes que me dicen las características de la clase `HolaMundo`. Uno de estos es `getClassLoader()` que me dice cual fue el **ClassLoader** que cargo dicha clase. De hecho una misma clase puede ser cargada al mismo tiempo por diferentes ClassLoaders y en esencia aunque sea la misma clase a juicio de la JVM son diferentes.

Obtengamos la jerarquía de cargadores de clases que mencionamos.

{% highlight java linenos %}
public class HolaMundo {

   public static void main(String[] args) {

      HolaMundo h = new HolaMundo();
      Class clazz = h.getClass();

      ClassLoader system = clazz.getClassLoader();
      System.out.println(system);

      ClassLoader extensions = system.getParent();
      System.out.println(extensions);

      ClassLoader bootstrap = extensions.getParent();
      System.out.println(bootstrap);

   }
}
{% endhighlight %}

&nbsp;

El ultimo me imprime `null`, esto es porque el cargador bootstrap es nativo y no esta escrito en java. Veamos algunos conceptos asociados a los cargadores de clases.

&nbsp;

### Principio de Delegación

Este principio dicta que un **ClassLoader** pide a su padre una clase, si este no la consigue  delega en su padre y así sucesivamente hasta llegar al bootstrap y bajar de nuevo al **ClassLoader** que inicio la petición el cual termina cargando la clase.

### Principio de Visibilidad

Las clases cargadas por un **ClassLoader** son visibles a sus hijos y no viceversa, es decir las clases cargadas por los hijos no son visibles para sus padres.

### Principio de Unicidad

Cuando un **ClassLoader** carga una clase sus hijos nunca la re-cargaran.

&nbsp;

Veamos un ejemplo mas complejo con un paquete EAR instalado en un servidor Java EE. Un archivo EAR generalmente contiene lo siguientes:

1.  Un o varios módulos **EJB**
2.  Uno o varios módulos **WAR**
3.  Dependencias **JAR** compartidas por los EJB y los WAR
4.  Un archivo descriptor `application.xml`

Supongamos que tenemos dos EAR, `MyEar1.ear` y `MyEar2.ear`. La estructura de ClassLoaders es parecida esta:

![application_server_classloader](/images/application_server_classloader.jpg)

- Cada EAR tendrá su propio ClassLoader.
- Todos los EJB comparten un mismo ClassLoader.
- Cada WAR tiene su propio ClassLoader.
- Los servidores y contenedores de Java EE pueden aplicar diferentes estrategias de carga de clases.

&nbsp;

### Padre primero, padre ultimo (Parent First, Parent Last)

Algunos cargadores delegan las peticiones a su padre de forma inmediata sin antes buscar en sus directorios los archivos `.class`. Esta forma de operar se le denomina _**Parent First**_.

Si un cargador busca primero el .class en sus directorios y solo después que no la encuentra hace la petición al padre a esto se le denomina _**Parent Last**_.

Algunos servidores y contenedores por defecto están configurados para usar el enfoque de Parent Last, esto carga primero las librerías del WAR para después llegar al ClassLoader del servidor. Esto permite a las aplicaciones usar sus propias librerías y no las que están instaladas en los servidores.