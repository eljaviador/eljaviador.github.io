---
layout: post
title:  "Antipatrones y Problemas del Desarrollo [04]"
date:   2011-11-12  13:28:00
published: true
categories: [patrones]
tags: [patrones, antipatrones, principios, poo, arquitectura]
comments: true
shortinfo: Cuarta entrega de problemas asociados al uso de patrones de diseño y desarrollo de software
---

Seguimos con los antipatrones de software, ahora con la tercera y ultima categoría de la que hablaremos, la gestión de 
proyectos de software.

## Gestión de proyectos de software. Parte 1

#### **Analysis paralysis (Paralisis de analisis)** 
Este es un antipatrón sobre diseño orientado a objetos. El proceso de abstracción en la orientación a objetos nos ayuda a 
descomponer un problema para darle solución. El punto aquí es: cual debe ser el nivel de detalle para dar solución al problema?. 
Quizás en este proceso lleguemos a un nivel de descomposición en que nuestro diseño resulte muy complejo y tendremos que 
rediseñar y rediseñar por los siglos de los siglos :). 

La solución recomendada es un proceso de desarrollo incremental en donde el conocimiento se va dando en cada fase del proyecto, 
análisis, diseño, codificación, testeo y validación.<br/><br/>

#### **Blowhard jamboree (Francachela de fanfarrones)** 
Los informes(en ocasiones sesgados e inexactos) que emiten aquellos que llaman _"expertos de la industria"_ son tomados en 
cuenta por gerentes y lideres de proyectos los cuales generan una gran cantidad de preguntas que los programadores deben contestar. 

Las soluciones pueden ser varias: contar con un experto en determinada tecnología, ya sea dentro de la empresa o como consultor, 
tomar cursos especializados por parte del personal e identificar y evitar ciertos tipos de informes ya que algunos son simple 
propaganda.<br/><br/>

#### **Death by planning (Muerte por planificación)** 
En muchas organizaciones la planificación es una actividad necesaria para la ejecución de cualquier proyecto. El problema surge 
cuando se planifica en exceso y se cree dicha planificación es la única responsable del éxito del proyecto. No arranca el proyecto 
hasta que este milimétricamente planificado. Un proyecto de software puede contener actividades caóticas y desconocidas al momento 
de planificar. 

La solución propuesta es que el plan del proyecto debe mostrar principalmente lo que se entrega: requerimientos de 
negocio, descripción técnica, criterios de aceptación medibles, escenarios de uso del producto, casos de usos de componentes.
Todo esto soportado con sus respectivas validaciones: diseño conceptual, de especificación, de implementacion y de test.<br/><br/>

#### **Corncob** 
Son las conocidas _"personas difíciles"_ dentro del desarrollo de los proyectos de software. Y el origen de esto puede deberse 
a la propia personalidad, falta de reconocimiento y hasta falta de incentivos monetarios. Esta puede transformar el entorno de 
trabajo en un ambiente estresado. Afecta al equipo de varias maneras: política, técnica o personal. La política es la mas común. 

La solución es controlar a quien apoya el comportamiento conflictivo, es decir los seguidores del Corncob. Cuando el Corncob 
pierde este apoyo prevalecerá el interés general del equipo.<br/><br/>

#### **Irrational managment (Gestion irracional)** 
Este es un antipatrón sobre la personalidad de quien ejecuta un proyecto. Esta persona se le dificulta enormemente tomar decisiones 
debido a algún defecto en la personalidad o por obsesionarse con los detalles. Cuando llega el momento la decisión puede ser un 
acto basado en el instinto en vez de ser una medida táctica o estratégica. 

Las soluciones pueden ser varias y tratare de resumir: 

* Admitir el problema y buscar ayuda 
* Conocer habilidades y personalidades de su equipo para delegar trabajo y algunas decisiones
* Proporcionar información clara y objetivos a corto plazo que el personal entiende como alcanzarlos
* Compartir el enfoque y asegurarse que el equipo va hacia la misma meta
* Buscar siempre la mejora de los procesos aunque sean pequeños ajustes
* Facilitar la comunicación cuando se debate sobre un tema
* Gestionar mecanismo de comunicación como correos y grupos de noticias
* Aplicar técnicas efectivas de decisión como las de Kepner-Tregoe