---
layout: post
title:  "S.O.L.I.D Principio Abierto/Cerrado"
date:   2011-05-16  09:25:00
published: true
categories: [conceptos y principios]
tags: [principios, poo]
comments: true
shortinfo: Segunda entrega sobre los pricipios S.O.L.I.D para desarrollo de software.
---

Seguimos con el segundo de los principios [_**S.O.L.I.D**_]({% post_url 2011-03-14-principios-solid %}).
 
Este principio dice :

> _Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification. - Entidades 
de software (clases, módulos, funciones, etc) deben ser abiertas para extender, pero cerrada para modificación_.

Robert Martin cita a _Ivar Jacobson_:

> _Todo sistema cambia durante su ciclo de vida. Esto debe tenerse en cuenta cuando desarrollamos sistemas que esperamos que 
duren mas de la primera versión_.

Cuando un simple cambio ocasiona una cascada de cambios en módulos dependientes, el programa expone características indeseables 
que asociamos con el mal diseño.

Se pregunta entonces como lograr crear _**sistemas estables**_ de cara al cambio y que van a tener mas de una versión?.

Ahh, el Sr. Bertrand Meyer(creador de Eiffel y una eminencia en POO) acuño por allá en 1988 lo que hoy conocemos como 
principio abierto-cerrado. 

Debe existir la posibilidad de añadir nuevas entidades (clases, módulos o funciones) a un sistema sin que sea necesario hacer 
cambios en la estructura actual. Estas nuevas funcionalidades deben acoplarse en algún punto. Esto se logra gracias a la 
abstracción y el polimorfismo. Ahora vemos lo importante de entender bien estos dos conceptos de la POO.

Veamos un ejemplo de la aplicación de este principio:

{% highlight java linenos %} 
class User { 
   
   private String login; 
   private String password;
   
   User(String login, String password) { 
      this.login = login; this.password = password; 
   } 
   
   // Setters, getters y metodos de negocio 
   // ... 
}

class DataBaseUserProvider { 

   public List getUsers(){ 
      // Suponemos que viene de la Base de Datos 
      List lst = new ArrayList(); 
      lst.add(new User("admin", "passadmin")); 
      lst.add(new User("invitado", "passinvitado")); 
      return lst; 
   } 
}
{% endhighlight %} <br/>

Lo importante aquí a tener en cuenta es el proveedor de usuarios, este nos retorna una lista de usuarios que están en nuestra BD.

Ahora tenemos un autenticador que se encargara de buscar si existe el usuario en la lista que nos retorna el provider.

{% highlight java linenos %}
class Authenticator { 
   private DataBaseUserProvider provider;
   
   Authenticator(DataBaseUserProvider provider) { 
      this.provider = provider; 
   }
   
   public void autenticar(String login, String password){ 
      List<user> list = provider.getUsers(); 
      //No voy a escribir el codigo para no hacerlo largo, pero la idea es 
      // que aqui verificamos que ese login y password este en la lista. 
   } 
} 
{% endhighlight %}<br/>

Bueno aquí vemos como tenemos un mal diseño, porque? Que pasa si ahora los usuarios no vienen de la base de datos si no de un archivo 
xml por ejemplo. Ahhhhh :(. 

Bueno aplicando el principio de abstracción y el polimorfismo podemos hacer que nuestro autenticador dependa directamente de una 
clase abstracta o de una interface(contrato), ahora cuando agreguemos un nuevo provider por ejemplo que los usuarios vengan de un 
directorio `LDPA` no tengo que modificar nada, solo adicionar mi componente.

Si es una clase abstracta mi componente extenderá de esta o si es una interface o contrato lo implementara. 
Yo me inclino mas por diseños basados en interfaces.

{% highlight java linenos%} 
interface UserProvider { 
   List<user> getUsers(); 
}

class DataBaseProvider implements UserProvider { 
   public List getUsers() { 
      // Aqui nuestro codigo q busca usuarios en la Base de Datos 
      return new ArrayList(); 
   } 
}

class XmlProvider implements UserProvider{ 
   public List getUsers() { 
      //Aqui nuestro codigo q busca usuarios en un XML 
      return new ArrayList(); 
   } 
}

{% endhighlight %}<br/>

Creamos una interface `UserProvider` y con esta es que va a tratar el autenticador. No le interesa de donde vienen los usuarios, 
solo agregamos todos los provider que necesitemos y no tenemos necesidad de modificar nada.

{% highlight java linenos %} 
class Authenticator{ 
   private UserProvider provider;
   
   public Authenticator(UserProvider provider) { 
      this.provider = provider; 
   }
   
   public boolean autenticar(String login, String paswword){ 
      List<user> users = provider.getUsers(); 
      // Aqui nuestro codigo para verificar dentro la lista 
      return true; 
   } 
}
{% endhighlight %}<br/>

Otro punto por ejemplo es el uso inapropiado del operador **`instanceof`**. Si tenemos código que los usa intensivamente para 
preguntar un tipo de objeto y realizar determinado código, ojo que hay algo mal en nuestro diseño.