---
layout: post
title:  "Comprender Manven [02]"
date:   2013-05-27  13:57:00
categories: frameworks herramientas
---

El **_Project Object Model_** es un archivo xml(`pom.xml`) que cada proyecto posee y el cual describe la configuración de los plugins, las dependencias o librerías de terceros, los perfiles y más. 

Este xml es usado por maven para realizar la construcción(build) del proyecto cada vez que ejecutamos algún goal o una fase del ciclo de vida.

Contiene un elemento padre llamado `<project>...</project>` y este a su vez otros subelementos. De todos los elementos el POM al menos debe tener las coordenadas que identifican al proyecto. Que son las coordenadas?


## Coordenadas Maven

Los elementos **groupId**, **artifactId** y **version** son identificadores que forman lo que se denomina coordenadas de un proyecto. Estas coordenadas son únicas y no existen dos proyectos con los mismos valores en groupId, artifactId y version.

Se le denomina coordenadas porque se asemeja a tener una dirección o ubicación única para un proyecto dentro del espacio de proyectos Maven. Esta es la forma en que maven relaciona proyectos entre sí. Los repositorios Maven están organizados usando estos identificadores.

### El elemento groupId

Representa un grupo, empresa, equipo u organización. Generalmente se nombra empezando con un dominio y la organización que crea el proyecto ej: **org.miempresa**.

### El elemento artifactId

Representa de manera única el nombre ya sea del del proyecto completo o de un módulo de este.

### El elemento version

Representa la versión de este proyecto o de este módulo. Bajo desarrollo suele usarse un número más la palabra **SNAPSHOT**.

### El elemento packaging

Representa la forma en que se va a distribuir la aplicación. Si no se coloca este elemento por defecto es **JAR**. También existe **WAR**, **EJB**, **POM** y otros. Aunque packaging es un elemento de las coordenadas, no es parte de los identificadores únicos de un proyecto.

Los siguientes son los elementos que puede contener un pom.xml

{% highlight xml linenos %}
<project ...></span>
  <modelVersion>4.0.0</modelVersion>

<!-- Básicos -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>

<!-- Configuración para construcción(build) -->
  <build>...</build>
  <reporting>...</reporting>

<!-- Otras información del proyecto -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>

<!-- Configurcion de entorno -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>
{% endhighlight %}

&nbsp;

Mientras vayamos haciendo ejemplos veremos la mayoría de estos elementos. Este es un pom básico generado por maven:

{% highlight xml linenos %}
<project... >

  <modelVersion>4.0.0</modelVersion>
  <groupId>net.ejemplos.ejemplomaven</groupId>
  <artifactId>ejemplo-maven</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>

  <name>ejemplo-maven</name>
  <url>http://maven.apache.org</url>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

</project>
{% endhighlight %}

&nbsp;

Ahora hagamos un ejemplo. Vamos a crear un pequeño proyecto y compilarlo. Usaremos dos plugins:

1. **archetype:generate** para generar el proyecto.
2. **compiler:compile** para compilarlo.

&nbsp;

## Arquetipos

En Maven un arquetipo básicamente es un modelo. Como una especie de molde para crear los proyectos. Los arquetipos crean un proyecto agregando características específicas como: que directorios hacen parte de la estructura o que elementos son incluidos en el pom. Vamos a usar el arquetipo **_maven-archetype-quickstart_** que crea un proyecto muy básico.

### Cremos el Proyecto

{% highlight bash %}
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false -DgroupId=org.empresa.proyecto -DartifactId=proyecto1
{% endhighlight %}

Esto crea una estructura como esta:
<pre>
proyecto1
 ├─ pom.xml
 └─ src
     ├─ main
     │   └─ java
     │       └─ org
     │           └─ empresa
     │               └─ proyecto
     │                   └─ App.java
     └─ test
         └─ java
             └─ org
                 └─ empresa
                     └─ proyecto
                         └─ AppTest.java
</pre>

&nbsp;

### Compilamos el Proyecto

{% highlight bash %}
mvn compiler:compile
{% endhighlight %}

Esto crea un nuevo directorio para el proyecto generado llamado `target` y un subdirectorio en `target` llamado `classes`.

&nbsp;

### Ejecutamos el Proyecto

{% highlight bash %}
java -cp target/classes org.empresa.proyecto.App
{% endhighlight %}

No te gustan esos nombres de directorio? Bueno hagamos nuestra primera configuracion en el `pom.xml` generado.

&nbsp;

### Limpiar el proyecto generado:

{% highlight bash %}
mvn clean
{% endhighlight %}

&nbsp;

### Agregamos al pom.xml:

{% highlight xml linenos %}
<build>
  <directory>build</directory>
</build>
{% endhighlight %}

&nbsp;

### Compilamos de nuevo:

{% highlight bash %}
mvn compiler:compile
{% endhighlight %}

Ahora ya no vemos que genera el directorio target si no build. Así configuramos muchas cosas en el `pom.xml` para cambiar el comportamiento de los plugins.

&nbsp;

## Usando el Ciclo de Vida

En vez de usar plugins directamente para compilar usemos el ciclo de vida. Quitamos la configuración el elemento que colocamos en el `pom.xml` y ejecutamos:

{% highlight bash %}
mvn install
{% endhighlight %}

Esto crea el directorio target con varios subdirectorios y el nombre del empaquetado del proyecto, en este caso:
`proyecto1-1.0-SNAPSHOT.jar`