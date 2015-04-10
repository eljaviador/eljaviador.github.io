---
layout: post
title:  "Reusando objetos"
date:   2013-04-10  12:30:00
published: true
categories: [patrones]
tags: [patrones, principios, poo, arquitectura]
comments: true
shortinfo: Optimiza tu sistema y el uso intensivo de objetos a través de este patron de diseño.
---

Al instanciar ciertos tipos de objetos estos realizan otras operaciones de inicialización que toman un poco más tiempo de 
lo habitual. Más aún, estos objetos son usados de forma periódica. Qué hacemos para evitar que esta situación degrade el 
rendimiento de nuestra aplicación?.

Creamos varios objetos y los colocamos en un sitio común de donde serán tomados para usarse y luego se devuelven una vez 
termine su uso. Como una biblioteca, tomas un libro, lo usas y lo regresas luego para ser usado por alguien más.

Estamos hablando de una piscina de objetos conocida como **_object pool_**.

Un object pool puede ser muy efectivo para ayudar al rendimiento, identificando aquellos objetos que sean como recursos, 
costosos de crear y usados intensivamente.

Este no es un patrón de GoF, pero por eso no deja de ser uno. Esta dentro de los patrones creacionales.

El diagrama de clases:

![Patron Object Pool](/images/patron-object-pool.png)

Entidades involucradas:

1. **Reusable :** Identifica los objetos o recursos costosos que son reusados.
2. **ReusablePool :** Administra los recurso usados por los clientes.
3. **Client :** Usa las instancias de Reusable.

El ReusablePool es el sitio común que administra el uso coherente de los recursos. Este sitio común es nuestro pool o 
piscina, y es un **_Singleton_**. Ya he hablado del Singleton [aquí]({% post_url 2013-03-31-el-objeto-unico-patron-singleton %}).

## Comportamiento
Los detalles de como funciona un pool pueden ser desde los más sencillos hasta los más complejos según las características 
que necesitemos. Límite mínimo y máximo para objetos Reusable, tiempo de expiración y limpieza periódica, listeners para 
seguimiento(tracking), etc. <br/><br/>

#### Detalles de implementación
**EAGER :** Se crean todos los Reusables al iniciar la aplicación.<br/>
**LAZY :** Los Reusables se crean con el primer pedido de un cliente.<br/>
**EAGER_LAZY :** Se crea un minimo al cargar la aplicación y después con peticiones se van creando más Reusables.<br/><br/>

#### Estrategia de adquisición
Cuando un cliente pide un Reusable varias situaciones se presentan.

1. Hay disponible y se le entrega al cliente.
2. No hay disponible:
2.1. Límite máximo no alcanzado, se crea un nuevo Reusable.
2.2. Límite máximo alcanzado, bloquea hasta que haya disponible.
2.3. Límite máximo alcanzado, lanza un error.

El `Client` es responsable de hacer el pedido y regresar el `Reusable`.

## Ejemplo
Haremos un ejemplo muy básico de un pool. Creamos una clase abstracta que sirva de base con el comportamiento genérico. 
Luego la extendemos y agregamos comportamiento específico según el tipo de pool.

#### Las características genéricas de esta clase serán
1. Manejar el almacenamiento de objetos genéricos.
2. Expiración de los objetos.
3. Podría tener Listeners para seguimiento(No implementada aquí)


#### Las características específicas de la clase que extiende del pool
1. Creación de los Reusables específicos.
2. Validación específica de los Reusables.
3. Destrucción de los Reusables.

{% highlight java linenos %}
public abstract class ObjectPool {

  private long expirationTime;
  private HashMap<T, Long> locked, unlocked;

  public ObjectPool() {
    expirationTime = 60000; // 60 segundos
    locked = new HashMap<T, Long>();
    unlocked = new HashMap<T, Long>();
  }

  public synchronized T acquire() {
    long now = System.currentTimeMillis();
    T t;
    if (unlocked.size() > 0) {
      Iterator it = unlocked.keySet().iterator();
      while (it.hasNext()) {
        t = it.next();
        //Verifcamos tiempo de expiracion
        if ((now - unlocked.get(t)) > expirationTime) {
          //Expiró, removemos
          unlocked.remove(t);
          expire(t); //Delegamos expiracion a una subclase
          t = null;
        } else {
          if (validate(t)) {
            unlocked.remove(t);
            locked.put(t, now);
            return (t);
          } else {
            // Si falla vaidacion, removemos y expiramos
            unlocked.remove(t);
            expire(t);
            t = null;
          }
        }
      }
    }
    // Si llega aqui es que no hay disponible y creamos uno.
    t = create();
    locked.put(t, now);
    return (t);
  }

  public synchronized void release(T t) {
    locked.remove(t);
    unlocked.put(t, System.currentTimeMillis());
  }

  //Implementacaión especifica por una Subclase
  protected abstract T create();
  public abstract boolean validate(T o);
  public abstract void expire(T o);

}

public class Reusable {

  public void doSomeWork(){
    //...
  }
}

public class ReusablePool extends ObjectPool {

  private static ReusablePool instance = new ReusablePool();

  private ReusablePool(){
    //Parametros de inicializacion
    System.out.println("Iniciando pool");
  }

  public static ReusablePool getInstance(){
    return instance;
  }

  protected Reusable create() {
    //Logica para crear objetos Reusable
    return new Reusable();
  }

  public boolean validate(Reusable o) {
    //Validaciones especificas sobre Reusable
    return true;
  }

  public void expire(Reusable o) {
    //Destruir el objeto reusable
    //Ej: cerrar si fuera una conexion
  }
}

public class Principal {

  public static void main(String[] args) {

    //Obtenemos el Reusable del pool
    Reusable reusable = ReusablePool.getInstance().acquire();

    //Se usa
    reusable.doSomeWork();

    //Se libera
    ReusablePool.getInstance().release(reusable);

  }
}
{% endhighlight %}<br/>

## Usos mas comunes
1. Conexiones a base de datos
2. Conexiones con Sockets
3. Aplicaciones de interfaz gráfica
4. Threads

Hoy dia ya no es tan necesario por ejemplo crear un pool de conexiones ya que existen varias librerías conocidas que te 
ayudan en eso. C3P0, apache DBCP, BoneCP. Incluso los servidores de aplicaciones traen sus propias implementaciones de pool jdbc.

A modo de práctica podrías tratar de crear uno siguiendo la estructura anterior.