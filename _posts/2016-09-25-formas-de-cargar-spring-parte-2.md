---
layout: post
title:  "Formas de cargar Spring [02]"
date:   2016-09-25  07:15:00
published: true
categories: [frameworks]
tags: [spring, javaee, arquitectura]
comments: true
shortinfo: Como cargar el contenedor(contexto) Spring en nuestras aplicaciones. Parte 2.
---

En el [_**post anterior**_]({% post_url 2016-09-20-formas-de-cargar-spring-parte-1 %}) vimos como cargar Spring en aplicaciones
**Standalone**. Ahora vamos con las aplicaciones Java EE, particularmente un empaquetado **`.war`**. Aquí igualmente nuestras
configuraciones pueden estar en **XML** o en clases `Java`.

En las aplicaciones web `.war` podemos configurar la carga del contenedor de Spring de dos formas:

1. Desde el archivo **`web.xml`**.
2. Desde código Java (disponible desde la versión 3 de Servlet).


## Cargar contenedor desde el web.xml (Configuración XML)
Esta había sido la forma mas tradicional de configurar Spring hasta la llegada de Servlet 3.0 que permite también la configuración
por código. Aquí usamos la clase `ContextLoaderListener`.


{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app ... >

    <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

</web-app>
{% endhighlight %}<br/>

`ContextLoaderListener` por defecto usa `XmlWebApplicationContext` para la carga del contexto. Este `XmlWebApplicationContext`
busca las configuraciones en `WEB-INF/applicationContext.xml`. Si nuestro archivo XML de configuraciones no esta en esa ruta
o no se llama igual, debemos especificar su nombre y ubicación de lo contrario Spring nos da un error.

A través del parámetro de contexto `contextConfigLocation` podemos especificar nuestro XML.

{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app ... >

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>WEB-INF/mis-beans.xml</param-value>
    </context-param>

    <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

</web-app>
{% endhighlight %}<br/>


## Cargar contenedor desde el web.xml (Configuración Java)
También podemos especificar que el origen de nuestras configuraciones sea una clase Java anotada con `@Configuration`.
Lo único es que tenemos que agregar un parámetro adicional al web.xml que especifica la clase del contexto. En este caso
usamos la clase `AnnotationConfigWebApplicationContext`.


{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app ... >

    <context-param>
        <param-name>contextClass</param-name>
        <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </context-param>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>com.acme.AppConfig</param-value>
    </context-param>

    <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

</web-app>
{% endhighlight %}<br/>

Aquí `AppConfig` es nuestra clase con las configuraciones de los beans.


## Cargar contenedor sin web.xml
Desde la versión Servlet 3.0 podemos tener un empaquetado `.war` sin el **web.xml**. La plataforma Java EE especifica que puedes
usar una implementación de `ServletContainerInitializer` para que registres todos tus filtros, listeners, servlets, etc de forma
programática.

Spring nos proporciona otra interface `WebApplicationInitializer` que debemos implementar para registrar la carga de nuestro
contexto. Debemos usar alguna implementación ya sea `XmlWebApplicationContext` o `AnnotationConfigWebApplicationContext` dependiendo
si nuestra configuración esta en un **XML** o en una clase **Java**.

{% highlight java linenos %}
public class IniciadorWeb implements WebApplicationInitializer {

    public void onStartup(ServletContext servletContext) throws ServletException {

        XmlWebApplicationContext context = new XmlWebApplicationContext();
        ContextLoaderListener loaderListener = new ContextLoaderListener(context);
        servletContext.addListener(loaderListener);

    }
}
{% endhighlight %}<br/>

Ya saben que `XmlWebApplicationContext` busca por defecto un xml en `WEB-INF/applicationContext.xml` y si es diferente el nombre
debemos aplicar la misma estrategia anterior y registrar los parámetros de contexto necesarios. Tambien podemos usar
`AnnotationConfigWebApplicationContext`.

{% highlight java linenos %}
public class IniciadorWeb implements WebApplicationInitializer {

    public void onStartup(ServletContext servletContext) throws ServletException {

        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class);
        ContextLoaderListener loaderListener = new ContextLoaderListener(context);
        servletContext.addListener(loaderListener);

    }
}
{% endhighlight %}<br/>


Espero que estas sencillas configuraciones ayuden a despejar algunas dudas que tenían sobre Spring y como integrarlo en nuestras aplicaciones.
Cabe resaltar que en este caso no he configurado Spring MVC para trabajar la parte web.

