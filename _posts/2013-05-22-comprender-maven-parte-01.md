---
layout: post
title:  "Comprender Manven [01]"
date:   2013-05-22  13:41:00
published: true
categories: [herramientas]
tags: [maven, conceptos, arquitectura, proyectos]
comments: true
shortinfo: Enfoque comprensivo sobre esta herramienta de construcción de proyectos.
---

Comúnmente creamos nuestros proyectos con el soporte Maven que traen IDEs como _Eclipse_, _IntelliJ IDEA_ o _Netbeans_ para facilitar las cosas sin ir a los detalles.
La idea aquí es ir mas allá, a esos detalles y usarlo de forma manual para aprenderlo mejor.

[Aquí]({% post_url 2013-05-22-comprender-maven-parte-01 %}) tienes una guía de instalación antes de comenzar.

## Tareas en el desarrollo

Las tareas mas usadas de forma general son: crear el directorio del proyecto para nuestros archivos(fuentes, imágenes, xml, etc.) y las librerías de terceros,
compilar y generar los binarios(`.class`) en un directorio diferente a los fuentes, ejecutar los test, empaquetar `.jar`, `.war` o `.ear` y desplegar el proyecto si es necesario.

Como nos ayuda Maven con estas tareas?.

## Estructura de Directorios

Tan sencillo como crear un directorio para todos nuestros proyectos y de ahí en adelante se encarga maven. Maven establece una estructura de directorios
estándar para organizar los archivos del proyecto. Cada proyecto puede contener una estructura como esta:

<table class="tbl1">
 <tr>
  <th>Ruta</th>
  <th>Proposito</th>
 </tr>
 <tr>
  <td>src/main/java</td>
  <td>Achivos fuentes</td>
 </tr>
 <tr class="even">
  <td>src/main/resources</td>
  <td>Recursos</td>
 </tr>
 <tr>
  <td>src/main/filters</td>
  <td>Filtros de recursos</td>
 </tr>
 <tr class="even">
  <td>src/main/assembly</td>
  <td>Descriptores de ensamble</td>
 </tr>
 <tr>
  <td>src/main/config</td>
  <td>Archivos de configuración</td>
 </tr>
 <tr class="even">
  <td>src/main/scripts</td>
  <td>Scripts de aplicación</td>
 </tr>
 <tr>
  <td>src/main/webapp</td>
  <td>Contenido de aplicación Web</td>
 </tr>
 <tr class="even">
  <td>src/test/java</td>
  <td>Fuentes para Test</td>
 </tr>
 <tr>
  <td>src/test/resources</td>
  <td>Recursos para Test</td>
 </tr>
 <tr class="even">
  <td>src/test/filters</td>
  <td>Filtros de recursos para Test</td>
 </tr>
 <tr>
  <td>src/site</td>
  <td>Sitio</td>
 </tr>
 <tr class="even">
  <td>target/</td>
  <td>Proyecto generado</td>
 </tr>
 <tr>
  <td>pom.xml</td>
  <td>Descriptor del proyecto</td>
 </tr>
 <tr class="even">
  <td>LICENSE.txt</td>
  <td>Licencia del Proyecto</td>
 </tr>
 <tr>
  <td>NOTICE.txt</td>
  <td>Notas y atribuciones requeridas por librerías de terceros</td>
 </tr>
 <tr class="even">
  <td>README.txt</td>
  <td>Archivo readme de proyecto</td>
 </tr>
</table><br/>

Aunque no todos son necesarios en muchos casos.

## Crear un Proyecto

Bien vamos a crear un proyecto directamente con los comandos de maven. Por ahora no entraremos en detalle sobre el comando. En la consola y ubicado sobre tu
directorio de proyectos, ejecuta el siguiente comando:

{% highlight bash %}
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=net.ejemplos.ejemplomaven -DartifactId=ejemplo-maven -DinteractiveMode=false
{% endhighlight %}<br/>

Esto crea un directorio para tu nuevo proyecto con el nombre puesto en el parámetro **artifactId**, en este caso se llama **_ejemplo-maven_**.
Al final obtienes un resumen con varios valores y un mensaje de BUILD SUCCESS.
En la estructura generada no están todos los directorios mencionados anteriormente. Algunos son opcionales según tu proyecto.

## Plugins y Goals

El comando principal de maven es mvn. Este es el encargado de ejecutar las órdenes que le pasemos.

{% highlight bash %}
mvn orden_a_ejecutar
{% endhighlight %}<br/>

La **_orden a ejecutar_** puede ser un _plugin_ o una _fase_ del ciclo de vida.
Cuando la orden es un plugin, especificamos también un goal de ese plugin. Un goal es una tarea a realizar y se puede personalizar para modificar su comportamiento.
Un plugin puede tener varios goal y la sintaxis de ejecución así:

{% highlight bash %}
mvn plugin:goal [-Dnombre1=valor1 -Dnombre2=valor2]
{% endhighlight %}<br/>

Maven esta basado en plugins. Con los plugins es que se realizan las tareas que necesitamos. Crear un archivo `jar`, crear un archivo `war`, compilar el código, etc.
Esto es lo que permite que sean reutilizados en varios proyectos para ejecutar las tareas comunes. Por ejemplo para compilar ejecutemos la siguiente orden en la raíz del directorio del proyecto ejemplo-maven:

{% highlight bash %}
mvn compiler:compile
{% endhighlight %}<br/>

Hemos ejecutado el goal _**compile**_ del plugin _**compiler**_. Este plugin tiene 2 goal, `compile` y `testCompile`. Esto realiza la tarea de compilar las fuentes creando
un nuevo directorio llamado target. Maven posee muchos plugins que puedes ver [aquí](http://maven.apache.org/plugins/index.html), ahí se detalla los goal que posee cada
uno y los parámetros de configuración para cada goal.

Cuando creamos el proyecto usamos el plugin **_archetype_** con el goal _**generate**_. También le pasamos unos parámetros de la forma **_-DnombreParametro=valor_**.

Luego veremos como configurar el comportamiento de los plugins mas en detalle.

## Ciclo de Vida

La construcción o generación de un proyecto pasa por varias fases. Tener las librerias de terceros en el classpath, compilar el código fuente, hacer test, empaquetar la
aplicación generada, desplegarla en un server y mas. Hacer todo esto paso por paso con plugins y goal no tiene nada de productivo.

Ahí es donde entra en acción el ciclo de vida. Este posee una serie de fases que están predefinidas o se pueden personalizar para realizar todas estas tareas de una vez.
Cada fase esta formada por uno o varios goal para lograr un objetivo.

Hay 3 tipos de ciclos de vidas incorporados en maven: **default**, **clean** y **site**.

**Default** maneja toda la generación y despliegue del proyecto.<br/>
**Clean** limpia el proyecto generado.<br/>
**Site** crea la documentación del proyecto.<br/><br/>

#### **Fases de Clean**
**pre-clean :** Operaciones antes de la limpieza.<br/>
**clean :** Remueve todos los archivos generados de la construcción anterior.<br/>
**post-clean :** Procesos necesarios para finalizar la limpieza.<br/><br/>

#### **Fases de Site**
**pre-site :** Procesos antes de la generación de documentación.<br/>
**site :** Generación de la documentación del proyecto.<br/>
**post-site :** Procesos después de la generación de documentación y preparación del despliegue de esta.<br/>
**site-deploy :** Despliega la documentación en el servidor web especificado.<br/><br/>

#### **Fases de Default**
**validate :** Valida que el proyecto está correcto y esté toda la información necesaria.<br/>
**initialize :** Establece variables y crea directorios.<br/>
**generate-sources :** Genera el código fuente necesario para incluir en el compile.<br/>
**process-sources :** Procesa el código fuente filtrando valores.<br/>
**generate-resources :** Genera recursos a incluir en el empaquetado.<br/>
**process-resources :** Copia y procesa los recursos en el directorio destino.<br/>
**compile :** Compila el código fuente del proyecto.<br/>
**process-classes :** Post-procesamiento a la compilación (ej: manipular el bytecode).<br/>
**generate-test-sources :** Genera el código fuente de testing a incluir en el test-compile.<br/>
**process-test-sources :** Procesa el código fuente de testing filtrando valores.<br/>
**generate-test-resources :** Crea los recursos para test.<br/>
**process-test-resources :** Copia y procesa los recursos en el directorio test de destino.<br/>
**test-compile :** Compila el código fuente de test en el directorio test de destino.<br/>
**process-test-classes :** Post-procesamiento a la compilación de test (ej: manipular el bytecode).<br/>
**test :** Testea el código compilado con el framework especificado.<br/>
**prepare-package :** Operaciones antes de empaquetar.<br/>
**package :** Empaqueta el código compilado en el formato de distribución indicado(jar, war, ear).<br/>
**pre-integration-test :** Operaciones antes de integrar (ej: configurar entorno).<br/>
**integration-test :** Procesa y despliega el paquete si es necesario en un ambiente de integración.<br/>
**post-integration-test :** Operaciones después de la integración (ej: limpiar entorno).<br/>
**verify :** Verifica que el paquete es válido y cumple con criterios de calidad.<br/>
**install :** Instala el paquete en el repositorio local para ser usado como dependencia.<br/>
**deploy :** Integra o despliega en un entorno, copia el paquete a repo remoto para otros desarrolladores.<br/><br/>

Al ejecutar una fase se ejecutan también todas las fases previas a esa. Si ejecutamos **_deploy_**, se ejecutará todo el ciclo de vida default desde **_validate_**.
Cada fase del ciclo de vida tiene uno o varios goal asociados a ella. Estos se ejecutan en el orden que va pasando el ciclo de vida. Por ejemplo el goal `jar:jar`
se adjunta a la fase **_package_**.

Algunos plugins ejecutados en las fases de ciclo de vida por defecto.

<table class="tbl1">
 <tr>
  <th>Plugin:Goal</th>
  <th>Fase</th>
 </tr>
 <tr>
  <td>resources:resources</td>
  <td>process-resources</td>
 </tr>
 <tr class="even">
  <td>compiler:compile</td>
  <td>compile</td>
 </tr>
 <tr>
  <td>resources:testResources</td>
  <td>process-test-resources</td>
 </tr>
 <tr class="even">
  <td>surfire:test</td>
  <td>test (este goal termina el ciclo si falla)</td>
 </tr>
 <tr>
  <td>jar:jar</td>
  <td>package</td>
 </tr>
</table><br/>

Lo mismo pasa si obviamos las fases y especificamos los goals directamente uno tras otro:

{% highlight bash %}
mvn resources:resources compiler:compile resources:testResources surfire:test jar:jar
{% endhighlight %}<br/>

También puede ejecutarse los tres tipos ciclos de vida así:

{% highlight bash %}
mvn clean deploy site
{% endhighlight %}<br/>

Maven posee dos formas de manipular las tareas en las fases y como estas se ejecutan.

**1. Por el tipo de empaquetado.**
Para hacer esto es necesario el archivo de configuración del proyecto `pom.xml` que veremos mas adelante. Por ahora solo tendremos en cuenta que los tipos de
empaquetado pueden ser varios. Ej: jar, war, ejb, ear, pom y otros. Si no se especifica el tipo, **_por defecto es jar_**. Cada uno de estos tipos de empaquetado
especifican las fases que se ejecutan. Por ejemplo para el tipo pom se ejecutan solo install y deploy.

**2. Por configuracion de plugins.**
Los plugins también se configuran en el pom.xml. Ahí se especifican cosas como en cual fase se ejecuta el plugin y determinado goal. Estos se suman a los que se
ejecutan de forma predeterminada y el orden puede ser configurado igualmente.

Bien, por ahora lo dejamos aquí.