---
layout: post
title:  "Antipatrones y Problemas del Desarrollo [03]"
date:   2011-09-30  13:28:00
categories: arquitectura
---

Aquí la segunda parte de los antipatrones de arquitectura de software.

## Arquitectura de Software. Parte 2

#### **Architecture by implication (Inferir arquitectura)** 
Sucede cuando por experiencia previa los responsables de un proyecto asumen que la documentación no es necesaria. Este exceso 
de confianza afecta el éxito del sistema. La solución ideal es definir y organizar la arquitectura del sistema con una buena 
planificación.<br/><br/>

#### **Design by Committee (Diseño de comite)** 
Imagina una sopa hecha por 10 cocineros. Una locura estoy de acuerdo. Bueno este antipatron se presenta cuando disponemos de un 
numero de posibles diseños pero la unificación de un criterio en cuanto a cual usar se convierte en un completo problema. 
Esto pasa muchas veces cuando se pretende definir estándares, el grupo de expertos puede divagar en una decisión por largo tiempo. 

Una solución posible es mejorar el proceso de reuniones, los horarios, tiempos de intervención, lugares de encuentro, la agenda u 
objetivos. Es posible también asignar roles dentro del grupo para así tener una separación de responsabilidades bien clara y definida.<br/><br/>

#### **Reinvent the Wheel (Reinventar la rueda)** 
Sucede cuando se desconoce la existencia de herramientas disponibles y hay poca transferencia de conocimiento entre proyectos de 
software. La construcción de software puede tener desarrollos muy específicos pero la mayoría de las veces mucho comportamiento 
es común o ya se ha usado antes en otro sitio. 

Aquí es cuando se decide crear una solución desde cero aunque dicha solución ya este 
creada. La solución posible es realizar una buena minería de arquitectura. La extracción de información valiosa permite crear 
diseños robustos, independientes, reusables y extensibles. Al final identificando los procesos es posible identificar la existencia 
de soluciones que se pueden aplicar a nuestra arquitectura.<br/><br/>

#### **Warm Bodies (Cuerpos tibios)** 
Recuerdo en la escuela cuando la maestra decía "vienen solo a calentar asiento". Exacto es eso mismo que estas pensando. 
Un proyecto de software pueden contener un staff de programadores de diferentes niveles. Según las estadísticas uno de cada 
20 programadores hace el trabajo que hacen los otros 20, juntos. Ese programador excepcional tiene talento, experiencia y es 
muy productivo. 

Así que en definitiva cuando este en un proyecto mire un grupo de 20 y deduzca si es excepcional o esta 
calentando asientos. Solucionamos esto dividiendo. El tamaño y la duración ideal de un proyecto de software es 4 programadores 
y 4 meses respectivamente. Si un equipo va mas allá de 5 personas eventualmente padecera problemas de coordinación de grupo. 

Equipos pequeños pueden mantener una visión común, tomar decisiones eficientes y producirán una solución mas exitosa a corto plazo.<br/><br/>

#### **Swiss Army Knife (Navaja suiza)** 
Cuando tenemos interfaces sobrecargadas que intentan proporcionar un numero de soluciones que satisfacen muchas necesidades. 
Se pierde el enfoque real de la clase o interface y no existe una abstracción adecuada. Tratamos de hacer productos que se 
apliquen a muchas situaciones. 

Esto acarrea problemas de depuración, documentación y mantenimiento. Una solución es crear 
convenciones en el uso de una tecnología que permitan manejar la complejidad de dicha arquitectura. Esta convención documentada 
es conocida como_Perfil_. Es como un plan que detalla la tecnología y su uso, secuencias de ejecución, llamadas de métodos y 
manejo de excepciones.<br/><br/>

#### **The Grand Old Duke of York (El viejo gran duke de York)** 
Hay programadores que echan código y hay programadores con instinto arquitectónico. Así que programar no es lo mismo que 
saber crear abstracciones. Saber manejar la complejidad es la clave del buen diseño. Así que el problema aparece cuando se 
relega o reduce al mínimo la importancia de los creadores de abstracciones. 

Esto se soluciona creando los roles necesarios 
en el desarrollo de software. Los arquitectos cumplen un papel fundamental ya que conocen la tecnología, facilitan la 
comunicación entre usuarios y desarrolladores, manejan y aseguran la adaptabilidad del sistema. La especialización de los 
roles en el desarrollo de software nos indica la existencia de múltiples disciplinas para construir sistemas.<br/><br/>