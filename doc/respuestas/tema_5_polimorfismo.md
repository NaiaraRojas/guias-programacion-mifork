<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### 

El **polimorfismo** en orientación a objetos es la capacidad de tratar objetos de distintas clases de forma uniforme a través de una **referencia común**, normalmente una superclase o una interfaz. Su utilidad principal consiste en permitir que el mismo código funcione con objetos diferentes sin necesidad de conocer su tipo concreto. Esto hace posible escribir programas más flexibles y extensibles, ya que el comportamiento concreto que se ejecuta depende del **tipo real del objeto en tiempo de ejecución**, no del tipo de la referencia. El polimorfismo se apoya directamente en la herencia y en la compatibilidad de tipos vista anteriormente.

Gracias al polimorfismo, un mismo mensaje (una llamada a un método) puede producir comportamientos distintos según el objeto que lo reciba. Por ejemplo, al invocar un método común definido en una superclase, cada subclase puede responder de manera diferente. Esto evita estructuras rígidas basadas en condicionales y permite que el código cliente permanezca inalterado aunque se añadan nuevos tipos. En términos prácticos, el polimorfismo es una de las bases para cumplir el principio de “abierto a extensión y cerrado a modificación”.

La **sobreescritura de métodos** es el mecanismo que permite implementar el polimorfismo en Java. Consiste en que una subclase proporcione su propia versión de un método que ya existe en la superclase, manteniendo la misma firma. Cuando ese método se invoca a través de una referencia del tipo base, Java decide en tiempo de ejecución qué versión ejecutar, en función del tipo real del objeto. Este proceso se denomina *enlace dinámico* y es el que hace posible el comportamiento polimórfico.

En resumen, el polimorfismo permite que distintos objetos se comporten de forma diferente ante el mismo mensaje, y la sobreescritura es la técnica que lo hace posible. Juntos, estos conceptos permiten desacoplar el código del uso de tipos concretos, facilitando la evolución del sistema y reduciendo la complejidad del diseño.



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### 

La **ligadura dinámica** o **enlace tardío** consiste en que la decisión sobre **qué implementación concreta de un método se ejecuta** no se toma en tiempo de compilación, sino en **tiempo de ejecución**, en función del tipo real del objeto al que apunta una referencia. Este mecanismo es esencial para el polimorfismo, ya que permite que una llamada a un método, realizada a través de una referencia del tipo base, invoque el comportamiento específico de la subclase correspondiente. Sin ligadura dinámica, el polimorfismo no sería posible, ya que el método a ejecutar quedaría fijado de forma anticipada.

La relación entre ligadura dinámica y polimorfismo es directa: el polimorfismo se apoya en el hecho de que el enlace del método se realiza tarde. Cuando se llama a un método sobrescrito mediante una referencia de la superclase, el sistema determina dinámicamente qué versión ejecutar según el objeto real. Esto permite escribir código genérico que no depende de tipos concretos y que puede extenderse sin modificarse, uno de los objetivos fundamentales de la programación orientada a objetos.

En **C++**, la ligadura dinámica **no es el comportamiento por defecto**. Para que un método se resuelva dinámicamente, debe declararse explícitamente como `virtual` en la clase base. Si no se hace, se utiliza enlace temprano, y el método que se ejecuta depende del tipo de la referencia, no del objeto real. Por tanto, en C++ el programador debe indicar de forma explícita cuándo desea comportamiento polimórfico. En **Java**, en cambio, la ligadura dinámica es el comportamiento por defecto para todos los métodos de instancia no marcados como `static`, `final` o `private`. El programador no necesita indicar nada especial para obtener polimorfismo, lo que simplifica el uso del modelo orientado a objetos.

En **Python**, la ligadura dinámica también es el comportamiento natural del lenguaje. Python es un lenguaje de tipado dinámico, y todas las llamadas a métodos se resuelven en tiempo de ejecución según el objeto real, sin necesidad de herencia explícita ni de interfaces formales. Esto hace que el polimorfismo sea muy flexible, aunque también menos estricto que en Java o C++, ya que no existe comprobación de tipos en tiempo de compilación. En resumen, mientras que C++ exige indicar explícitamente la ligadura dinámica, Java y Python la aplican de forma automática como parte central de su modelo de ejecución.



## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### 

En este ejemplo se ilustra el **polimorfismo** mediante la **sobreescritura completa** de un método en una subclase. La clase base `Soldado` define un método `saludar()`, que representa un comportamiento común. La subclase `Zapador` redefine ese método para cambiar totalmente su comportamiento, mientras que `Artillero` hereda el comportamiento original sin modificarlo. Esta redefinición permite que distintos tipos de objetos respondan de forma diferente al mismo mensaje.

El aspecto clave del polimorfismo es que la decisión sobre qué versión del método se ejecuta se toma en **tiempo de ejecución**, según el tipo real del objeto. Aunque se utilicen referencias del tipo `Soldado`, cuando se llama a `saludar()` Java invoca la implementación correspondiente a la clase concreta del objeto. Esto demuestra que el comportamiento no depende del tipo de la referencia, sino del tipo real del objeto almacenado en memoria.

El uso de un array de `Soldado` permite observar este comportamiento de forma clara. En dicho array se almacenan objetos de distintos subtipos, pero todos se recorren usando referencias del tipo base. Al invocar el mismo método sobre cada elemento, cada objeto responde de acuerdo con su propia implementación. Este mecanismo evita condicionales explícitos y facilita la extensibilidad del código.

El ejemplo demuestra cómo la sobreescritura es la base técnica del polimorfismo y cómo este permite escribir código genérico que funciona correctamente con distintos tipos concretos sin necesidad de conocerlos de antemano.

## ✅ **Ejemplo en Java**

### Clase base `Soldado`

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

### Subclase `Artillero` (no sobreescribe)

```java
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }
}
```

### Subclase `Zapador` (sobreescritura completa)

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        System.out.println("Soy el zapador " + nombre + " y coloco minas");
    }
}
```

### Uso polimórfico

```java
public class EjercicioPolimorfismo {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[2];

        soldados[0] = new Artillero("Carlos");
        soldados[1] = new Zapador("Ana");

        for (Soldado s : soldados) {
            s.saludar(); // se ejecuta el método según el tipo real
        }
    }
}
```




## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### 

Cuando un método se **sobreescribe** en una subclase, no se pierde la posibilidad de reutilizar el comportamiento definido en la clase base. Es posible invocar explícitamente el método original de la superclase y, a partir de su resultado o de su efecto, **añadir o modificar** el comportamiento. Esta técnica es muy habitual cuando se desea especializar ligeramente una operación sin reemplazarla por completo.

En Java, esta reutilización se realiza mediante la palabra clave **`super`**, que permite acceder a los métodos (y constructores) de la clase base. Al escribir `super.saludar()`, se está indicando explícitamente que se desea ejecutar la versión del método definida en la superclase, incluso aunque exista una versión sobreescrita en la subclase. Esta llamada debe hacerse desde dentro del método sobreescrito y permite construir el nuevo comportamiento de forma incremental.

Este enfoque resulta especialmente útil para mantener coherencia y evitar duplicación de código. En lugar de copiar el contenido del método base, la subclase delega en él y añade su propia lógica adicional. De este modo, si el comportamiento común cambia en la superclase, la subclase se beneficia automáticamente del cambio sin necesidad de modificaciones adicionales.

En el ejemplo siguiente, la clase `Zapador` no sustituye completamente la forma de saludar, sino que **reutiliza el saludo estándar del soldado** y añade un mensaje específico. La palabra clave utilizada para invocar el método de la clase base es **`super`**.

***

## ✅ **Ejemplo en Java**

### Clase base `Soldado`

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

### Subclase `Zapador` reutilizando el método base

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        super.saludar(); // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}
```




## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### 

Al **sobreescribir un método** en Java, existen varias restricciones que deben cumplirse para que la operación sea válida. La **firma del método** debe ser compatible con la del método de la superclase: el número y tipo de los parámetros deben ser exactamente los mismos. El **tipo de retorno** debe ser el mismo o un **subtipo** del retorno original (esto se denomina *retorno covariante*). Además, el método sobreescrito **no puede reducir la visibilidad** del método original, es decir, no se puede pasar de `public` a `protected` o `private`. En cuanto a excepciones, el método sobreescrito no puede lanzar excepciones más generales que las declaradas en el método base.

La **sobreescritura (*overriding*)** y la **sobrecarga (*overloading*)** son conceptos distintos aunque a menudo se confunden. La sobreescritura ocurre entre una superclase y una subclase, y permite cambiar el comportamiento de un método heredado manteniendo su misma firma, siendo la decisión del método a ejecutar dinámica (polimorfismo). La sobrecarga, en cambio, ocurre dentro de una misma clase (o entre superclase y subclase) cuando existen varios métodos con el mismo nombre pero **distintos parámetros**, y su resolución se realiza en **tiempo de compilación**, no está relacionada con el polimorfismo.

La anotación **`@Override`** se utiliza para indicar explícitamente que un método pretende sobreescribir un método de la superclase. Aunque no es obligatoria, su uso es altamente recomendable porque permite al compilador detectar errores comunes, como escribir mal el nombre del método o equivocarse en los parámetros. Si el método no está sobreescribiendo realmente a otro, el compilador emitirá un error, evitando fallos sutiles que de otro modo pasarían desapercibidos.

En resumen, la sobreescritura está sujeta a reglas estrictas de compatibilidad de firma y visibilidad, se diferencia claramente de la sobrecarga por su relación con el polimorfismo, y el uso sistemático de `@Override` mejora la seguridad y claridad del código. Por ello, se considera una buena práctica emplear siempre esta anotación cuando se redefine un método heredado.

***

## ✅ **Ejemplo ilustrativo**

```java
public class Soldado {
    public String saludar() {
        return "Soy un soldado";
    }
}

public class Zapador extends Soldado {

    @Override
    public String saludar() { // mismo método, mismo nombre y parámetros
        return super.saludar() + " y soy zapador";
    }
}
```

✅ Aquí:

*   Se mantiene la misma firma → **sobreescritura válida**
*   El tipo de retorno es el mismo
*   `@Override` garantiza que realmente se está sobreescribiendo




## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### 

En Java, el **polimorfismo se emplea desde las primeras etapas de aprendizaje**, aunque muchas veces se utiliza de forma implícita sin ser plenamente consciente de ello. Cuando se crea una clase y se **sobreescriben métodos como `toString()` o `equals()`**, ya se está utilizando polimorfismo. Estos métodos están definidos en la clase base `Object`, y al proporcionar una implementación propia en una clase concreta, se permite que el comportamiento cambie según el tipo real del objeto, incluso cuando se maneja mediante una referencia de tipo `Object`.

El polimorfismo aparece porque las llamadas a estos métodos se resuelven mediante **ligadura dinámica**. Por ejemplo, al imprimir un objeto con `System.out.println(objeto)`, internamente se invoca `toString()`. Si el objeto real es de una clase que ha sobreescrito `toString()`, se ejecutará esa versión específica, no la definida en `Object`. Esto ocurre aunque el código que realiza la llamada no conozca el tipo concreto del objeto, lo que es precisamente la esencia del polimorfismo.

De forma similar, al sobreescribir `equals()`, se redefine cómo se comparan dos objetos de una clase concreta. Cuando una estructura genérica, como una colección, utiliza `equals()` para comprobar igualdad, el método que se ejecuta depende del tipo real del objeto. Esto demuestra que el polimorfismo no es un concepto avanzado reservado para jerarquías complejas, sino una característica fundamental que se usa de manera habitual desde los primeros programas en Java.

En conclusión, al sobreescribir métodos heredados de `Object` ya se está haciendo uso del polimorfismo, aunque no se utilicen explícitamente arrays de supertipos o referencias polimórficas. Java está diseñado para que el polimorfismo forme parte natural del lenguaje desde el inicio, facilitando su adopción progresiva a medida que se profundiza en la orientación a objetos.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### 

Una **clase abstracta** es una clase que representa un concepto incompleto o genérico y que **no puede instanciarse directamente**. Su finalidad es servir como base para otras clases más concretas, proporcionando estado y comportamiento común, pero dejando ciertos aspectos sin definir. Una clase abstracta puede contener métodos implementados y también métodos sin implementar, que obligan a las subclases a proporcionar su propia versión. De este modo, se define una estructura común y se delega en las subclases el comportamiento específico.

Un **método abstracto** es un método que se declara sin implementación y que debe ser obligatoriamente implementado por las subclases concretas. Este tipo de método define *qué se puede hacer*, pero no *cómo se hace*. La existencia de métodos abstractos implica necesariamente que la clase sea abstracta, ya que no tendría sentido crear instancias de una clase que no puede ejecutar completamente su comportamiento. Por tanto, **no es posible crear objetos de una clase abstracta**.

Las clases abstractas están estrechamente relacionadas con el **polimorfismo**, ya que permiten trabajar con referencias del tipo abstracto y ejecutar comportamientos distintos según la subclase concreta. Al declarar un método abstracto en la clase base, se fuerza a que cada subtipo defina su propia acción, garantizando que el método exista y pueda invocarse de forma polimórfica. Esto resulta especialmente útil cuando todas las subclases deben responder a una misma operación, pero de formas distintas.

En Java, la palabra clave **`abstract`** debe colocarse tanto en la **clase** como en el **método** que no tiene implementación. El siguiente ejemplo redefine `Soldado` como clase abstracta y añade un método abstracto `atacar`, que cada tipo concreto de soldado implementa según su función.

***

## ✅ **Ejemplo en Java**

### Clase abstracta `Soldado`

```java
public abstract class Soldado {
    protected final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }

    // Método abstracto
    public abstract void atacar();
}
```

### Subclase `Artillero`

```java
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println("El artillero " + nombre + " dispara cohetes");
    }
}
```

### Subclase `Zapador`

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println("El zapador " + nombre + " coloca minas");
    }
}
```

### Uso polimórfico

```java
public class EjercicioAbstractos {
    public static void main(String[] args) {
        Soldado[] soldados = {
            new Artillero("Carlos"),
            new Zapador("Ana")
        };

        for (Soldado s : soldados) {
            s.saludar();
            s.atacar(); // comportamiento distinto según el tipo real
        }
    }
}
```

***

✅ **Respuesta directa a las preguntas**

*   Una clase abstracta **no puede instanciarse**.
*   Un método abstracto **no tiene implementación** y obliga a las subclases a definirla.
*   La palabra clave **`abstract`** se coloca en la **clase** y en el **método** abstracto.





## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### 

La palabra clave **`final`** en Java se utiliza para **restringir la herencia y la sobreescritura**, y puede aplicarse tanto a **clases** como a **métodos**. Cuando una **clase es `final`**, no puede ser heredada por ninguna otra clase. Esto significa que su comportamiento queda completamente cerrado a extensiones mediante herencia. Cuando un **método es `final`**, sí puede heredarse, pero **no puede ser sobreescrito** por las subclases. En ambos casos, `final` actúa como una forma explícita de impedir modificaciones estructurales en el diseño.

En relación con el **polimorfismo**, el uso de `final` **limita o anula** su aplicación. El polimorfismo se basa en la posibilidad de sobreescribir métodos y decidir en tiempo de ejecución qué implementación ejecutar. Si un método es `final`, no puede ser redefinido, por lo que no puede participar en polimorfismo dinámico mediante sobreescritura. Del mismo modo, si una clase es `final`, no puede tener subclases, lo que impide cualquier forma de polimorfismo basada en herencia para ese tipo.

El uso de `final` suele justificarse cuando se desea **garantizar un comportamiento fijo**, evitar errores de diseño o proteger invariantes internas. También puede emplearse por motivos de seguridad, claridad del diseño o incluso optimización, ya que el compilador puede realizar ciertas mejoras cuando sabe que un método no será sobreescrito. En este sentido, `final` expresa una decisión explícita de diseño: indicar que una clase o método no está pensada para ser extendida.

Un ejemplo clásico de **clase `final` en la API estándar de Java** es `String`. Esta clase no puede heredarse, lo que garantiza que su comportamiento sea inmutable y consistente en todo el sistema. Otras clases finales conocidas son `Integer`, `Double` o `Math`. En todos estos casos, Java prioriza la seguridad, la simplicidad y la fiabilidad frente a la extensibilidad mediante herencia, mostrando que el uso de `final` es una herramienta habitual y fundamental en el diseño del lenguaje.



## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### 

En Java, una **interfaz** es un tipo que define un **contrato**: especifica qué métodos debe ofrecer una clase, pero no cómo se implementan. Una interfaz describe capacidades o comportamientos que una clase puede tener, sin imponer una jerarquía de herencia de estado. Las interfaces permiten separar el *qué se hace* del *cómo se hace*, favoreciendo el diseño desacoplado y el polimorfismo, ya que el código puede trabajar con la interfaz sin conocer la clase concreta que la implementa.

Las interfaces se parecen a las **clases abstractas** en que ambas pueden declarar métodos sin implementación y no pueden instanciarse directamente. Sin embargo, existen diferencias importantes: una interfaz **no tiene estado** (no define atributos de instancia), y una clase **no hereda implementación** de una interfaz, solo se compromete a implementar sus métodos. En versiones modernas de Java, las interfaces pueden incluir métodos `default`, pero estos no convierten a la interfaz en una clase abstracta; siguen sin representar una relación “es‑un” con estado compartido.

Una diferencia clave es que **una clase puede implementar más de una interfaz**, mientras que solo puede heredar de una única clase. Esto permite resolver el problema de la herencia múltiple de forma segura, ya que se comparten contratos, no implementación ni estado. Gracias a ello, una clase puede combinar varios comportamientos (por ejemplo, “es comparable” y “es serializable”) sin los problemas de ambigüedad que tendría la herencia múltiple de clases.

En resumen, las interfaces no son exactamente como clases abstractas: ambas sirven para definir abstracciones, pero las interfaces se centran en **qué se ofrece** y permiten implementación múltiple, mientras que las clases abstractas combinan abstracción con **herencia de estado y comportamiento**. Por esta razón, las interfaces son una herramienta fundamental para el polimorfismo y el diseño flexible en Java.



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### 

En este ejemplo se combina el uso de **polimorfismo**, **clases abstractas** y **downcasting controlado** para trabajar con puntos de distintas dimensiones sin que el código cliente conozca los detalles concretos. La clase abstracta `Punto` define una operación común, `calcularDistanciaA`, pero deja su implementación a las subclases, ya que la forma de calcular la distancia depende del número de dimensiones. De este modo, se establece un contrato común que permite tratar todos los puntos de forma uniforme.

Las subclases `Punto2D` y `Punto3D` proporcionan implementaciones concretas del cálculo de la distancia. En cada implementación se comprueba que el punto recibido es del mismo subtipo usando `instanceof`, y se realiza un **downcasting** seguro para acceder a sus coordenadas específicas. Esto garantiza que la distancia solo se calcule entre puntos compatibles, evitando errores conceptuales como intentar medir la distancia entre un punto 2D y uno 3D.

Este diseño permite crear una clase `Linea` que trabaja exclusivamente con referencias de tipo `Punto`, sin conocer si los puntos son 2D o 3D. Gracias al polimorfismo, la línea puede calcular su longitud llamando al método `calcularDistanciaA`, delegando completamente en los puntos el cálculo concreto. Así, la clase `Linea` permanece desacoplada de las dimensiones de los puntos y puede reutilizarse sin modificaciones.

El ejemplo ilustra cómo el polimorfismo permite escribir código genérico y extensible, donde las decisiones específicas se resuelven en tiempo de ejecución. Aunque el uso de `instanceof` no siempre es la opción más elegante, resulta útil en este contexto didáctico para mostrar cómo verificar compatibilidad entre subtipos y realizar conversiones seguras cuando es necesario.

***

## ✅ **Ejemplo en Java**

### Clase abstracta `Punto`

```java
public abstract class Punto {

    public abstract double calcularDistanciaA(Punto otro);
}
```

***

### Implementación `Punto2D`

```java
public class Punto2D extends Punto {
    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible (no es 2D)");
        }
        Punto2D p = (Punto2D) otro;
        double dx = p.x - this.x;
        double dy = p.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

***

### Implementación `Punto3D`

```java
public class Punto3D extends Punto {
    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible (no es 3D)");
        }
        Punto3D p = (Punto3D) otro;
        double dx = p.x - this.x;
        double dy = p.y - this.y;
        double dz = p.z - this.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

***

### Clase `Linea` (independiente de la dimensión)

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.calcularDistanciaA(p2);
    }
}
```

***

### Uso del diseño

```java
public class EjercicioPuntos {
    public static void main(String[] args) {
        Punto a2 = new Punto2D(0, 0);
        Punto b2 = new Punto2D(3, 4);
        Linea l2 = new Linea(a2, b2);
        System.out.println(l2.longitud()); // 5.0

        Punto a3 = new Punto3D(0, 0, 0);
        Punto b3 = new Punto3D(1, 2, 2);
        Linea l3 = new Linea(a3, b3);
        System.out.println(l3.longitud()); // 3.0
    }
}
```





## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### 

La **herencia de interfaces** en Java consiste en que una interfaz puede **extender a otra interfaz**, heredando los métodos que esta declara. De este modo, se pueden definir contratos más específicos a partir de contratos más generales, sin introducir implementación ni estado. Esta herencia expresa una relación del tipo “A es‑un B” a nivel de capacidades, no de objetos concretos, y permite organizar jerárquicamente los comportamientos que una clase puede ofrecer.

En Java **sí existe herencia múltiple de interfaces**. Una interfaz puede extender a varias interfaces a la vez, y una clase puede implementar múltiples interfaces simultáneamente. Esto es posible porque las interfaces no heredan estado, solo definen métodos que deben implementarse. Gracias a esta característica, Java evita los problemas clásicos de la herencia múltiple de clases, como las ambigüedades de implementación, manteniendo al mismo tiempo una gran flexibilidad para el polimorfismo.

Aplicado al ejemplo de ficheros, se puede definir una interfaz `Fichero` que represente la capacidad básica de leer contenido. A partir de ella, otra interfaz más específica, `FicheroEscribible`, puede extenderla y añadir nuevas operaciones como escribir o eliminar. De este modo, cualquier clase que implemente `FicheroEscribible` estará obligada a cumplir también el contrato de `Fichero`, y podrá utilizarse polimórficamente como cualquiera de los dos tipos.

Este diseño muestra cómo la herencia de interfaces permite construir abstracciones graduales y reutilizables, separando claramente responsabilidades y favoreciendo la extensibilidad. Además, ilustra por qué las interfaces son el mecanismo elegido en Java para ofrecer herencia múltiple sin comprometer la simplicidad y la seguridad del lenguaje.

***

## ✅ **Ejemplo en Java**

### Interfaz base `Fichero`

```java
public interface Fichero {
    String leerContenido();
}
```

***

### Interfaz derivada `FicheroEscribible`

```java
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```

***

### Ejemplo de implementación

```java
public class FicheroTexto implements FicheroEscribible {

    private String contenido = "";

    @Override
    public String leerContenido() {
        return contenido;
    }

    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }

    @Override
    public void eliminar() {
        contenido = "";
    }
}
```

***

✅ **Resumen clave**

*   Las interfaces **sí pueden heredarse entre ellas**.
*   Java **sí permite herencia múltiple de interfaces**.
*   Las interfaces definen contratos, no implementación ni estado.



