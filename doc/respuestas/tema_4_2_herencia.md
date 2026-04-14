<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### 

**1. Herencia en orientación a objetos y relación “A es‑un B”**

En orientación a objetos, la **herencia** es un mecanismo que permite definir una nueva clase a partir de otra ya existente. La relación que se establece se describe como “A es‑un B”, lo que significa que un objeto de la subclase puede considerarse también un objeto de la superclase. Esto permite modelar jerarquías naturales, donde las clases más específicas reutilizan y especializan el comportamiento de clases más generales. A diferencia de la composición, la herencia expresa una relación de especialización, no de pertenencia.

La primera implicación fundamental de la herencia es la **compatibilidad de tipos**. Si una clase `Artillero` hereda de `Soldado`, entonces un objeto `Artillero` es compatible con el tipo `Soldado`. Esto permite tratar de forma uniforme a objetos de distintas subclases mediante referencias del tipo común, facilitando el uso de estructuras como arrays o métodos genéricos que operan sobre la superclase. Esta característica es clave para el polimorfismo, ya que permite escribir código que funciona con la abstracción general sin conocer el subtipo concreto.

La segunda implicación es la **herencia de estado y comportamiento**. Los atributos y métodos definidos en la superclase se heredan automáticamente por las subclases, sin necesidad de reescribirlos. De este modo, `Artillero` y `Zapador` heredan el atributo `nombre` y el método `saludar()` definidos en `Soldado`, y solo añaden su comportamiento específico. Esto evita duplicación de código y refuerza la coherencia del diseño, ya que los elementos comunes se definen en un único lugar.

El siguiente ejemplo muestra estas ideas en Java, incluyendo la compatibilidad de tipos mediante un array de `Soldado` que contiene objetos de distintos subtipos, todos ellos capaces de ejecutar el método común `saludar()`.


### Clase base `Soldado`

```java
public class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}
```

### Subclase `Artillero`

```java
public class Artillero extends Soldado {
    private final int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}
```

### Subclase `Zapador`

```java
public class Zapador extends Soldado {
    private final int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}
```

### Uso de compatibilidad de tipos

```java
public class EjercicioHerencia {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado("Luis");
        ejercito[1] = new Artillero("Carlos", 5);
        ejercito[2] = new Zapador("Ana", 3);

        for (Soldado s : ejercito) {
            s.saludar(); // todos son compatibles con Soldado
        }
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### 

Al crear un objeto de una clase concreta que hereda de otra, en Java **siempre se ejecutan varios constructores**, comenzando por el de la **clase base** y continuando hasta el de la clase más específica. Por ejemplo, al crear un `Artillero`, primero se ejecuta el constructor de `Soldado` y después el constructor de `Artillero`. Este orden es obligatorio porque la parte heredada del objeto debe inicializarse antes de que se inicialice la parte específica de la subclase. Conceptualmente, primero se construye el “Soldado” y luego se especializa como “Artillero”.

La palabra clave `super` dentro de un constructor se utiliza para **invocar explícitamente un constructor de la clase base**. Mediante `super(...)` se indican los parámetros necesarios para inicializar correctamente el estado heredado. Esta llamada debe ser siempre la **primera instrucción** del constructor, ya que Java exige que la inicialización de la superclase ocurra antes que cualquier operación propia de la subclase. Si no se escribe `super(...)` de forma explícita, el compilador intenta insertar automáticamente una llamada a `super()` sin parámetros.

Si la clase base **no tiene un constructor sin parámetros accesible**, entonces es obligatorio llamar explícitamente a `super(...)` con los argumentos adecuados. En caso contrario, el código no compila, ya que Java no sabe cómo inicializar la parte heredada del objeto. Por tanto, siempre que la superclase no disponga de un constructor vacío visible, la llamada a `super` debe realizarse explícitamente en todos los constructores de las subclases.

En resumen, al crear un objeto con herencia se ejecuta una cadena de constructores desde la superclase hasta la subclase, `super` sirve para inicializar correctamente la parte heredada, y su uso explícito es obligatorio cuando no existe un constructor por defecto accesible en la clase base. Esta mecánica garantiza que todos los objetos se construyan de forma coherente y completa.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### 


En Java, cuando se crea una instancia de una subclase, **los atributos privados definidos en la superclase sí forman parte físicamente del objeto en memoria**. Un objeto de una subclase contiene internamente toda la estructura del objeto de la superclase, además de sus propios atributos. Desde el punto de vista de la memoria, no existen “dos objetos separados”, sino un único objeto que incluye tanto la parte heredada como la parte específica. Por tanto, un `Artillero` contiene en memoria el atributo `nombre` definido en `Soldado`, aunque dicho atributo sea `private`.

Sin embargo, que un atributo privado exista en memoria **no implica que pueda ser accedido directamente desde el código de la subclase**. En Java, el modificador `private` restringe el acceso al **código de la propia clase**, no al objeto. Esto significa que, aunque el atributo `nombre` esté dentro del objeto `Artillero`, el código de la clase `Artillero` no puede acceder a él directamente mediante `nombre`. La privacidad actúa a nivel de clase, no a nivel de herencia ni de instancia.

Para poder utilizar ese estado heredado de forma segura, la superclase debe proporcionar **métodos de acceso**, como getters o métodos de comportamiento que usen internamente el atributo privado. En el ejemplo, `Soldado` ofrece el método `getNombre()` y el método `saludar()`, que permiten a las subclases usar indirectamente el atributo `nombre` sin violar la encapsulación. Así, la subclase hereda el estado, pero solo puede interactuar con él a través de la interfaz pública o protegida definida por la superclase.

En resumen, los atributos privados de la superclase **sí forman parte del objeto de la subclase en memoria**, pero **no son accesibles directamente desde el código de la subclase**. Esta separación entre existencia física y acceso lógico es una de las bases de la encapsulación en Java y permite diseñar jerarquías de herencia seguras y bien controladas.



```java
public class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

public class Artillero extends Soldado {
    private final int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
        // nombre = "X";   // ❌ NO permitido: es private en Soldado
    }

    public void informar() {
        // Uso indirecto del atributo heredado
        System.out.println("Artillero " + getNombre() + " con " + cohetes + " cohetes");
    }
}
```


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### 

La compatibilidad de tipos que introduce la herencia tiene una consecuencia directa y muy importante en términos de **extensibilidad del código**. Cuando varias clases comparten una superclase común, el código que trabaja con la superclase puede operar indistintamente con cualquiera de sus subclases, incluso con aquellas que todavía no existían cuando dicho código fue escrito. Esto permite ampliar el sistema añadiendo nuevos tipos sin necesidad de modificar el código ya existente, lo que reduce errores y facilita la evolución del programa.

Este principio se resume en la idea de que el código debe estar **abierto a extensión pero cerrado a modificación**. En el ejemplo de `Soldado`, el código que recorre un conjunto de soldados y les pide que saluden solo conoce el tipo `Soldado`, no los subtipos concretos. Gracias a la herencia y a la compatibilidad de tipos, dicho código sigue funcionando correctamente aunque se añadan nuevas clases como `Artillero`, `Zapador` o cualquier otro tipo futuro de soldado.

La herencia permite que el comportamiento común se defina una sola vez en la superclase, mientras que las subclases añaden o especializan características sin afectar al resto del sistema. Así, la extensibilidad se logra simplemente creando nuevas subclases que respetan el contrato definido por la superclase. El código cliente no necesita cambios porque sigue tratando con objetos del tipo general.

El siguiente ejemplo ilustra este concepto añadiendo un nuevo tipo de soldado sin modificar en absoluto el código que solicita el saludo a todos ellos. El bucle que recorre el array continúa siendo válido, demostrando cómo la compatibilidad de tipos favorece un diseño flexible y extensible.


### Nueva subclase añadida: `Medico`

```java
public class Medico extends Soldado {
    private final int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
```

### Código existente (NO se modifica)

```java
public class EjercicioHerencia {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Soldado("Luis");
        ejercito[1] = new Artillero("Carlos", 5);
        ejercito[2] = new Zapador("Ana", 3);
        ejercito[3] = new Medico("Sonia", 2); // nuevo tipo añadido

        for (Soldado s : ejercito) {
            s.saludar(); // el código no cambia
        }
    }
}
```




## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### 


En Java, es perfectamente posible que una **referencia del supertipo** apunte a un objeto real cuyo tipo concreto es un subtipo. Esto ocurre de manera natural cuando se trabaja con herencia y se conoce como **upcasting**. Por ejemplo, una referencia de tipo `Soldado` puede apuntar a un objeto `Artillero` o `Zapador`. Este mecanismo es seguro y automático, ya que todo objeto de una subclase es también, conceptualmente, un objeto de la superclase. Gracias a esto, se pueden tratar de forma uniforme objetos distintos que comparten un comportamiento común.

Sin embargo, cuando se utiliza una referencia del supertipo, **solo pueden invocarse los métodos definidos en dicho supertipo**, aunque el objeto real sea de un subtipo más específico. No es posible llamar directamente a métodos exclusivos del subtipo, como `getCohetes()` en un `Artillero`, usando una referencia `Soldado`. Para acceder a ese comportamiento específico es necesario realizar un **downcasting**, que consiste en convertir explícitamente la referencia del supertipo al subtipo concreto. Este proceso no es automático y puede provocar errores en tiempo de ejecución si el objeto real no es del tipo esperado.

Para evitar errores al hacer *downcasting*, Java proporciona el operador **`instanceof`**, que permite comprobar el tipo real del objeto antes de convertirlo. De este modo, se puede verificar si un objeto referenciado como `Soldado` es realmente un `Artillero` y, solo en ese caso, acceder de forma segura a sus métodos específicos. Esta combinación de *upcasting*, *downcasting* y `instanceof` permite escribir código flexible que aprovecha la herencia sin perder seguridad en tiempo de ejecución.

El siguiente ejemplo muestra un recorrido de un array de `Soldado` donde, aprovechando la compatibilidad de tipos, todos los soldados saludan, y además, si el objeto real es un `Artillero`, se imprime el número de cohetes que posee.


## ✅ **Ejemplo en Java**

```java
public class EjercicioCasting {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado("Luis");                 // upcasting implícito
        ejercito[1] = new Artillero("Carlos", 5);          // upcasting implícito
        ejercito[2] = new Zapador("Ana", 3);               // upcasting implícito

        for (Soldado s : ejercito) {
            s.saludar(); // método del supertipo

            // Comprobación del tipo real
            if (s instanceof Artillero) {
                Artillero a = (Artillero) s; // downcasting explícito
                System.out.println("  Cohetes: " + a.getCohetes());
            }
        }
    }
}
```




## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### 

En orientación a objetos, el acceso **protegido** forma parte de los mecanismos de ocultación de información y se sitúa entre el acceso `private` y el acceso `public`. Un atributo o método protegido **no es accesible desde cualquier clase**, pero **sí puede ser utilizado por las subclases**, incluso aunque estas se encuentren en otro paquete. De esta forma, se permite que las clases hijas reutilicen o extiendan el comportamiento de la clase base sin exponer completamente su estado interno al exterior.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. Un miembro declarado como `protected` es accesible desde la propia clase, desde cualquier clase del mismo paquete y desde las subclases, aunque estas estén en paquetes distintos. Esto resulta especialmente útil en herencia, cuando se desea que ciertos datos o comportamientos estén disponibles para las subclases, pero no formen parte de la interfaz pública de la clase base.

Aplicado al ejemplo de `Soldado`, declarar el atributo `nombre` como protegido permite que una subclase como `Zapador` pueda usar directamente dicho atributo en sus propios métodos, por ejemplo en un método específico para poner bombas. De este modo, se evita la necesidad de usar un getter únicamente para las subclases, manteniendo el atributo oculto frente a clases no relacionadas. El diseño sigue siendo encapsulado, pero más flexible para la extensión mediante herencia.

En resumen, el acceso protegido facilita la reutilización de estado y comportamiento en jerarquías de herencia, ofreciendo un equilibrio entre encapsulación y extensibilidad. Permite que las subclases accedan a detalles internos necesarios para su funcionamiento sin hacerlos visibles para todo el sistema.


### Clase base `Soldado` con atributo protegido

```java
public class Soldado {
    protected final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}
```

### Subclase `Zapador` usando el atributo protegido

```java
public class Zapador extends Soldado {
    private final int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }

    public void ponerBomba() {
        System.out.println("El zapador " + nombre + " pone una bomba");
    }
}
```

En este ejemplo, el atributo `nombre` **forma parte del objeto heredado** y, al ser `protected`, puede utilizarse directamente dentro del método `ponerBomba()` de la subclase `Zapador`, sin necesidad de exponerlo públicamente.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### 

En los lenguajes orientados a objetos, **no existe una regla universal** que obligue a que todas las clases hereden de una única clase base común. La existencia de una clase raíz depende del lenguaje concreto y de sus decisiones de diseño. Algunos lenguajes permiten jerarquías totalmente independientes, mientras que otros definen explícitamente una superclase común para todos los objetos. Por tanto, la presencia de una “clase base universal” no es una característica intrínseca de la orientación a objetos, sino una elección del lenguaje.

En lenguajes como C++, por ejemplo, **no existe una clase base común obligatoria**. Una clase puede no heredar de ninguna otra, y solo pasa a formar parte de una jerarquía cuando se declara explícitamente una herencia. Esto ofrece gran flexibilidad, pero también implica que no hay un conjunto mínimo de métodos garantizados para todos los objetos. En este contexto, la orientación a objetos es más explícita y menos uniforme que en otros lenguajes.

En Java, sin embargo, **todas las clases heredan directa o indirectamente de una clase base común llamada `Object`**. Incluso cuando no se indica ninguna herencia en el código, el compilador añade implícitamente `extends Object`. Esta clase proporciona un conjunto básico de métodos que todos los objetos poseen, como `toString()`, `equals()`, `hashCode()` o `getClass()`. Gracias a ello, Java garantiza que cualquier objeto puede tratarse como un `Object`, lo que aporta uniformidad y facilita el uso de mecanismos comunes como colecciones, reflexión o depuración.

En resumen, no todos los lenguajes orientados a objetos imponen una clase base común, pero Java sí lo hace de forma explícita. La existencia de `Object` como raíz de la jerarquía garantiza un comportamiento mínimo compartido por todos los objetos y simplifica el diseño del lenguaje, a costa de una menor libertad respecto a lenguajes como C++.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### 


La **herencia múltiple** es un mecanismo de la orientación a objetos mediante el cual una clase puede heredar directamente de **más de una clase base** al mismo tiempo. En este modelo, una clase hija obtiene el estado y el comportamiento de varias superclases, estableciendo relaciones del tipo “A es‑un B” y “A es‑un C” simultáneamente. Este enfoque permite reutilizar código de distintas jerarquías, pero introduce problemas de ambigüedad, especialmente cuando varias superclases definen métodos o atributos con el mismo nombre.

No todos los lenguajes orientados a objetos soportan herencia múltiple de clases. Lenguajes como C++ sí la permiten, lo que da lugar a situaciones complejas como el conocido *problema del diamante*, donde una clase hereda de dos clases que a su vez heredan de una misma base. Resolver estas ambigüedades exige reglas adicionales y un mayor cuidado por parte del programador, lo que incrementa la complejidad del diseño y del mantenimiento del código.

En **Java no existe herencia múltiple de clases**. Una clase solo puede heredar de una única clase base mediante la palabra clave `extends`. Esta decisión de diseño se tomó para simplificar el lenguaje y evitar los problemas de ambigüedad asociados a la herencia múltiple. Sin embargo, Java permite una forma alternativa de reutilización mediante **interfaces**, que permiten que una clase implemente múltiples contratos sin heredar estado, solo comportamiento abstracto (y, en versiones modernas, métodos por defecto con reglas claras).

En resumen, la herencia múltiple consiste en heredar de varias clases a la vez, pero **Java no la admite para clases** con el fin de mantener un modelo más simple y seguro. En su lugar, Java combina herencia simple de clases con implementación múltiple de interfaces, ofreciendo una solución más controlada para compartir comportamiento entre tipos sin introducir los conflictos típicos de la herencia múltiple clásica.



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### 

Las excepciones en los lenguajes orientados a objetos se modelan como **objetos**, lo que permite tratarlas como cualquier otra entidad del sistema. En Java, todas las excepciones heredan directa o indirectamente de la clase `Throwable`. Esto posibilita crear **excepciones personalizadas**, adaptadas al dominio del problema, que pueden contener información adicional relevante, como el objeto que causó el error. Este enfoque mejora la expresividad del código y facilita el diagnóstico de fallos.

Una excepción se considera **no controlada** cuando hereda de `RuntimeException`. Este tipo de excepciones no obliga al programador a declararlas ni a capturarlas explícitamente, y se utilizan normalmente para errores de programación o situaciones que no pueden resolverse localmente. Crear una excepción personalizada no controlada permite señalar problemas específicos del dominio sin sobrecargar el código con bloques `try-catch` innecesarios.

Además, las excepciones en Java pueden **encerrar una causa**, es decir, otra excepción subyacente que provocó el error original. Esta composición recursiva permite mantener toda la información del fallo a lo largo de la pila de llamadas. Para ello, se sobrecargan los constructores de la excepción y se utiliza el constructor de la superclase que recibe tanto un mensaje como una causa. De este modo, se conserva el encadenamiento de errores sin perder contexto.

El siguiente ejemplo muestra una excepción `UsuarioNoEncontradoException`, que es no controlada, contiene un objeto `Usuario` que causó el problema y permite opcionalmente incluir una causa subyacente.


### Clase `Usuario`

```java
public final class Usuario {
    private final String nombre;

    public Usuario(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

***

### Excepción personalizada `UsuarioNoEncontradoException`

```java
public class UsuarioNoEncontradoException extends RuntimeException {

    private final Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

***

### Ejemplo de uso

```java
public class ServicioUsuarios {

    public static void buscarUsuario(Usuario u) {
        // Simulación de error
        throw new UsuarioNoEncontradoException(
                u,
                new IllegalStateException("Base de datos no accesible")
        );
    }

    public static void main(String[] args) {
        Usuario usuario = new Usuario("Carlos");
        buscarUsuario(usuario);
    }
}
```

***

En este ejemplo, la excepción personalizada es un **objeto del dominio**, contiene al `Usuario` que causó el problema y además **encadena la causa original**, siguiendo el mismo modelo que utilizan las excepciones estándar de Java.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### 

En orientación a objetos se desaconseja utilizar la **herencia únicamente como mecanismo de reutilización de código** porque la herencia establece una relación semántica muy fuerte del tipo **“A es‑un B”**. Cuando se hereda de una clase, no solo se reutiliza su implementación, sino que también se asume su significado conceptual y su contrato. Si esa relación no es verdadera desde el punto de vista del dominio, el diseño resultante se vuelve confuso y difícil de mantener, ya que las subclases pasan a representar algo que realmente no “son”.

Otro problema importante es que la herencia genera un **acoplamiento fuerte** entre la subclase y la superclase. La subclase depende de los detalles internos de la superclase, y cualquier cambio en esta puede afectar de forma inesperada a todas las clases hijas. Esto provoca diseños frágiles, donde modificar una clase base para corregir o mejorar su comportamiento puede romper código en lugares lejanos. En cambio, la composición permite reutilizar funcionalidad sin quedar ligado de forma tan estricta a la implementación interna de otra clase.

Además, el uso incorrecto de la herencia puede violar el principio de **sustitución de Liskov**, que establece que un objeto de una subclase debe poder usarse allí donde se espera un objeto de la superclase sin alterar el comportamiento correcto del programa. Cuando se hereda solo por reutilizar código, es frecuente que la subclase no cumpla realmente las expectativas del tipo base, lo que conduce a errores lógicos y a la necesidad de comprobaciones de tipo adicionales.

Por estas razones, se recomienda considerar primero la **composición** cuando el objetivo principal es reutilizar código. La composición expresa relaciones del tipo “A tiene‑un B”, es más flexible, reduce el acoplamiento y facilita la evolución del sistema. La herencia debe reservarse para los casos en los que exista una relación clara de especialización y compatibilidad semántica entre las clases.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### 

Se recomienda **favorecer la composición frente a la herencia** porque la composición produce diseños **más flexibles y menos acoplados**. La herencia establece una relación muy fuerte entre clases, donde la subclase queda ligada a la implementación y al contrato de la superclase. Esto hace que cambios en la clase base puedan tener efectos colaterales inesperados en todas las subclases, dificultando la evolución del sistema. La composición, en cambio, permite reutilizar funcionalidad mediante delegación, sin asumir una relación semántica rígida del tipo “A es‑un B”.

Otro motivo importante es que la composición facilita el **cambio de comportamiento en tiempo de diseño** sin modificar jerarquías. Cuando se hereda, el comportamiento queda fijado por la jerarquía de clases; para cambiarlo, suele ser necesario crear nuevas subclases o reorganizar la herencia. Con composición, el comportamiento se obtiene a través de objetos internos intercambiables, lo que permite variar funcionalidades sin afectar a otras partes del sistema. Esto resulta especialmente ventajoso en sistemas que evolucionan o crecen con el tiempo.

Además, favorecer la composición ayuda a evitar **malos usos de la herencia**, como heredar solo para reutilizar código, lo que puede llevar a violar el principio de sustitución de Liskov. Con composición, la relación expresa claramente que un objeto **usa** otro objeto, no que **es** ese objeto. Esto mejora la claridad del modelo y reduce la necesidad de comprobaciones de tipo o conversiones inseguras en el código.

En conclusión, se favorece la composición frente a la herencia porque ofrece mayor flexibilidad, menor acoplamiento y un diseño más robusto ante cambios. La herencia sigue siendo útil cuando existe una relación clara de especialización, pero la composición debe considerarse la primera opción cuando el objetivo principal es reutilizar o combinar comportamiento de forma segura y mantenible.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### 

Cuando se afirma que la **herencia rompe la encapsulación**, se hace referencia a que una subclase puede quedar **acoplada a detalles internos** de la clase base, incluso a aquellos que no forman parte de su interfaz pública. Aunque la encapsulación pretende ocultar la representación interna de una clase, la herencia permite que las subclases conozcan y dependan de aspectos protegidos o del comportamiento implícito de la superclase. Esto provoca que cambios internos en la clase base puedan afectar directamente a las subclases, aunque la interfaz pública no haya cambiado.

Este problema se manifiesta especialmente cuando las subclases dependen de **atributos `protected`**, de métodos internos o del orden en que la superclase realiza ciertas operaciones. En esos casos, la subclase deja de apoyarse únicamente en el “qué hace” la superclase y empieza a depender del “cómo lo hace”. Como consecuencia, la superclase ya no puede modificar libremente su implementación interna sin riesgo de romper el funcionamiento de las subclases, lo que debilita la encapsulación original.

En contraste, la **composición preserva mejor la encapsulación**, ya que los objetos internos se usan a través de su interfaz pública y no se heredan detalles de implementación. El objeto que compone no necesita conocer cómo está implementado el objeto que utiliza, solo qué servicios ofrece. Esto permite cambiar la implementación interna de los objetos compuestos sin afectar al código que los usa, reduciendo el acoplamiento y mejorando la robustez del diseño.

En resumen, se dice que la herencia rompe la encapsulación porque expone a las subclases a los detalles internos de la superclase y crea dependencias implícitas difíciles de controlar. Por este motivo, se recomienda usar herencia solo cuando exista una relación clara de especialización y favorecer la composición cuando se desee mantener un mayor aislamiento entre las partes del sistema.



## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### 


## ✅ **13. Herencia vs. Composición: dos formas de modelar lo mismo**

Cuando varias clases comparten datos y comportamiento comunes, existen al menos dos alternativas de diseño: **herencia** y **composición**. Con herencia, se extraen los elementos comunes a una superclase, y las clases concretas heredan de ella. Esta opción expresa una relación semántica del tipo “A es‑una Persona” y permite reutilizar directamente atributos y métodos. Sin embargo, esta relación introduce un acoplamiento fuerte entre las clases y fija la estructura del modelo en una jerarquía rígida.

La composición ofrece una alternativa más flexible. En lugar de heredar, las clases contienen un objeto que representa los datos comunes. En este caso, `Estudiante` y `Trabajador` **tienen** unos `DatosPersonales`, en lugar de **ser** una `Persona`. Esta aproximación reduce el acoplamiento, evita jerarquías profundas y permite reutilizar los mismos datos personales en otros contextos sin forzar una relación de herencia. Además, el uso de composición deja más claro qué parte del objeto representa identidad y cuál representa un rol concreto.

Ambos modelos son válidos, pero transmiten intenciones distintas. La herencia es adecuada cuando existe una relación clara de especialización y se desea compatibilidad de tipos. La composición es preferible cuando se busca flexibilidad, reutilización sin acoplamiento fuerte y una mejor preservación de la encapsulación. Este ejemplo ilustra claramente cómo un mismo problema puede resolverse de dos maneras distintas según las prioridades del diseño.

***

## ✅ **Opción 1: Modelado por herencia**

### Superclase `Persona`

```java
public class Persona {
    private final String dni;
    private final String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

### Subclase `Estudiante`

```java
public class Estudiante extends Persona {

    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

### Subclase `Trabajador`

```java
public class Trabajador extends Persona {

    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

***

## ✅ **Opción 2: Modelado por composición**

### Clase `DatosPersonales`

```java
public final class DatosPersonales {
    private final String dni;
    private final String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

### Clase `Estudiante` (con composición)

```java
public class Estudiante {
    private final DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}
```

### Clase `Trabajador` (con composición)

```java
public class Trabajador {
    private final DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}
```


