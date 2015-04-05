---
layout: post
title:  "Guía de instalación de Maven"
date:   2013-05-21  18:08:00
published: true
categories: [guias]
tags: [maven, guias]
shortinfo: Guía de instalación de la herramienta de construcción de proyectos Maven.
---
Maven es una herramienta Java y necesitamos el JDK(no solo el JRE) instalado y configurado. La instalación de Maven es 
sencilla. Descargar un `.zip`, descomprimirlo en algún directorio y apuntar las variables a ese directorio. 
En Linux incluso podemos usar el comando `apt-get`.

## En Windows

#### 1. Descarga
Obtenemos la distribución más actual(3.0.5) de la web oficial de [maven](http://maven.apache.org "Web de Maven"), en este 
caso `apache-maven-3.0.5-bin.zip`. La descomprimimos en C y nos quedaría una ruta como `C:\apache-maven-3.0.5`.

#### 2. Establecer variable de entorno `M2_HOME`
En las propiedades del sistema encontramos las variables de entorno de Windows. Accedemos a través de las teclas `Windows + Pausa`.

![Variable M2_HOME](/images/m2_home_windows.png)

#### 3. Agregar el directorio `bin` de Maven al Path
Para ejecutar los comandos de maven necesitamos el directorio `\bin` en el Path. Editamos la variable de entorno Path y 
separando con un punto y coma (;) adicionamos al final `%M2_HOME%\bin`.

#### 4. Probar la instalación
Ejecutar el comando:

{% highlight bash linenos %}
mvn -version

Apache Maven 3.0.5 (r01de14724cdef164cd155faf9602da; 2013-02-19 09:21:28-0430)
Maven home: C:\apache-maven-3.0.5
Java version: 1.6.0_20, vendor: Sun Microsystems Inc.
Java home: C:\Program Files (x86)\Java\jdk1.6.0_20\jre
Default locale: es_ES, platform encoding: Cp1252
OS name: "windows 7", version: "6.1", arch: "x86", family: "windows"
{% endhighlight %}<br/>

## En Linux

### 1. Descarga

Descargar la distribución más actual de la web oficial de [maven](http://maven.apache.org), en este caso 
`apache-maven-3.0.5-bin.tar.gz` o por medio del comando `wget`.

{% highlight bash linenos %}
$ wget apache.tradebit.com/pub/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz
{% endhighlight %}<br/>

Desde el directorio de la descarga lo descomprimimos por ejemplo en `/usr/local/maven`.

{% highlight bash linenos %}
$ tar -xvf apache-maven-3.0.5-bin.tar.gz -C /usr/local/maven
{% endhighlight %}<br/>

y nos quedaría una ruta como `/usr/local/maven/apache-maven-3.0.5`.<br/><br/>

#### 2. Establecer variable de entorno `M2_HOME`

{% highlight bash linenos %}
$ export M2_HOME=/usr/local/maven/apache-maven-3.0.5
{% endhighlight %}<br/>

#### 3. Agregar el directorio `/bin` de Maven al Path
A fin de ejecutar los comandos de maven desde cualquier parte adicionamos al Path separando con dos puntos(:).

{% highlight bash linenos %}
$ export PATH=$PATH:$M2_HOME/bin
{% endhighlight %}<br/>

#### 4. Probar la instalación

Ejecutar el comando:

{% highlight bash linenos %}
$ mvn -version

Apache Maven 3.0.5 (r01de14724cdef164cd33c7c8c2fe155faf9602da; 2013-02-19 09:21:28-0430)
Maven home: /usr/local/maven/apache-maven-3.0.5
Java version: 1.6.0_27, vendor: Sun Microsystems Inc.
Java home: /usr/lib/jvm/java-6-openjdk-amd64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.5.0-21-generic", arch: "amd64", family: "unix"
{% endhighlight %}