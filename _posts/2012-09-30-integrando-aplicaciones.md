---
layout: post
title:  "Integrando aplicaciones"
date:   2012-09-30  13:28:00
categories: arquitectura
---

En una sola empresa puede haber multitud de sistemas(ERP, CRM...) para manejar el negocio, incluso puede tener no una, sino varias web. 

Todo esto puede estar hecho con diferentes lenguajes de programación estar en varios servidores con diferentes sistemas operativos y 
usar diferentes bases de datos. Toda esta enorme cantidad de plataformas y sistemas totalmente diferentes no se adquirieron de una sola 
vez, todo fue poco a poco y una cosa llevó a la otra. Recuerda que no existe el aplicativo único e ideal.

Estos sistemas tienen la obligación de compartir información por determinados motivos. Imagina el reto de hacer que todos estos 
sistemas que usan diferentes protocolos y formatos de datos se comuniquen entre sí y compartan información. Incluso podrían haber 
sistemas de otras empresas. Pues bien aquí es donde entra en juego lo que conocemos como integración.

Con el tiempo y la experiencia adquirida se han logrado identificar problemas recurrentes en estas integraciones y se han catalogado 
una serie de patrones que permiten realizar esta comunicación o integración entre diferentes sistemas.
Patrones?. Si patrones. Para ser más exactos, _patrones de integración empresarial_ o _EIP_.

Es decir que no solo los _**GoF**_ o patrones clásicos de desarrollo de software están documentados, estos también. Al ser algo 
compleja la integración estos patrones se convierten en una guía que establece posibles soluciones y especifican un lenguaje común para 
un mejor entendimiento.

Todo esto fue un trabajo realizado por Gregor Hohpe y Bobby Woolf en su 
libro [Enterprise Integration Patterns](http://www.enterpriseintegrationpatterns.com/).
Igualmente mantienen el sitio [http://www.eaipatterns.com](http://www.eaipatterns.com/).

## Estilos de Integracion
Existen varios enfoques para lograr la integración entre aplicaciones. Cada uno con sus pros y contras.

### Transferencia de Archivos
Cada aplicación produce archivos con datos que son consumidos por otras aplicaciones. Una exporta y la otra importa estos archivos. 
Esta información que se intercambia debería estar en alguno de los formatos más usados en la industria, ya sea XML y 
JSON.

![Transferencia de Archivos](/images/file-transfer.jpg)<br/>

### Base de Datos compartida
Cada aplicación almacena los datos a compartir en una base de datos común. De esta forma tendremos siempre un sistema transaccional 
con una integridad de datos segura.

![Base de Datos compartida](/images/shared-database.jpg)<br/>

### Invocación de procedimiento remoto
Cada aplicación pública métodos o procedimientos que las demás llaman. Esta es una comunicación directa ya que una aplicación llama a 
la otra. La integridad de los datos es manejada por cada aplicación dueña de dichos 
datos.

![Invocacion de procedimiento remoto](/images/remote-procedure.jpg)<br/>

### Mensajería
Cada aplicación se conecta a un sistema de mensajería común, intercambian datos e invocan comportamiento usando 
mensajes.

![Mensajería](/images/messaging.jpg)<br/>

Entre estos estilos, la mensajería asíncrona ha probado ser la mejor estrategia. Esto no significa que deba ser el único que 
siempre se debe usar. Cada integración tiene sus particularidades y de acuerdo a ellas se puede seleccionar un estilo que cubra 
las necesidades.

Al usar mensajería la integración se realiza con algo que denominamos middleware. Este middleware proporciona todos 
los enlaces entre los diferentes sistemas actuando como puente de comunicación que transporta los datos.

En otro artículo profundizaremos más en los patrones y conceptos involucrados en el estilo de mensajería así cuando tratemos 
de usar algún producto que implemente estos patrones la asimilación será más directa, sencilla y lograremos sacarle el mejor 
partido a estas herramientas.
