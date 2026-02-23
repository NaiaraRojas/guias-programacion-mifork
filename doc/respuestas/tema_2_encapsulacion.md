<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### 
En Programación Orientada a Objetos, la **encapsulación** consiste en agrupar datos y las funciones que operan sobre esos datos dentro de una misma unidad llamada clase. A diferencia de C, donde se pueden tener `struct` y funciones separadas que operan sobre ellas, en POO se integran ambos elementos en un único bloque lógico. La **ocultación de información** es el mecanismo que impide el acceso directo a ciertos datos internos del objeto, permitiendo interactuar con ellos únicamente a través de métodos definidos.

El objetivo principal de ambos conceptos es proteger el estado interno del objeto y controlar cómo se utiliza. En lugar de permitir que cualquier parte del programa modifique directamente los atributos (como ocurriría con una `struct` pública en C), se establece una interfaz clara y controlada. De esta forma, se evita que el objeto quede en un estado inconsistente o inválido.

Algunas ventajas de la ocultación de información son:

* Mayor seguridad y control sobre los datos.
* Reducción de errores al evitar modificaciones indebidas.
* Facilita el mantenimiento y la modificación interna del código sin afectar a otras partes del programa.
* Mejora la claridad y organización del diseño del software.



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### 
Se entiende por **interfaz pública** de una clase el conjunto de métodos y miembros que pueden ser accedidos desde fuera de la clase. En Java, esto corresponde principalmente a los métodos declarados con el modificador `public`, que permiten interactuar con el objeto sin necesidad de conocer cómo está implementado internamente. Desde el punto de vista de quien usa la clase, la interfaz pública representa todo lo que “se puede hacer” con el objeto.

La interfaz pública actúa como un contrato: define qué operaciones están disponibles y cómo deben utilizarse, pero no expone los detalles internos, como los atributos privados o la lógica interna de los métodos. De esta forma, el usuario del objeto no necesita saber cómo se almacenan los datos ni cómo se realizan los cálculos, sino únicamente qué métodos puede invocar y qué resultados puede esperar.

La relación con la ocultación de información es directa, ya que esta última se consigue restringiendo el acceso a los datos internos (por ejemplo, declarándolos `private`) y permitiendo su manipulación solo a través de la interfaz pública. Así, se controla el acceso al estado interno del objeto y se evita que otras partes del programa dependan de detalles de implementación que podrían cambiar en el futuro.



## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### 
Se debe diseñar con cuidado la **interfaz pública** porque define cómo otras partes del programa interactúan con la clase. Una vez que otros módulos comienzan a utilizar esos métodos públicos, se genera una dependencia directa hacia esa forma de uso. Por ello, cualquier decisión en el diseño (nombres de métodos, parámetros, valores de retorno) influye en cómo se construye el resto del sistema.

No resulta sencillo cambiar la interfaz pública una vez que la clase está siendo utilizada. Modificar o eliminar métodos públicos puede obligar a cambiar todo el código que los invoca, generando errores de compilación y aumentando el coste de mantenimiento. Por esta razón, se considera que la interfaz pública forma parte del “contrato” estable de la clase y debe pensarse cuidadosamente desde el inicio.



## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### 
Las **invariantes de clase** son condiciones o propiedades que deben cumplirse siempre para que un objeto se considere en un estado válido. Se trata de reglas internas que relacionan los atributos de la clase y que deben mantenerse verdaderas después de construir el objeto y tras cada ejecución de un método público. Por ejemplo, si una clase representa una cuenta bancaria, una invariante podría ser que el saldo nunca sea negativo.

Estas invariantes garantizan la coherencia del objeto durante toda su vida. Si se permite modificar libremente los atributos desde fuera de la clase, como ocurriría con una `struct` pública en C, sería fácil romper esas condiciones sin control. Esto podría provocar comportamientos inesperados o errores difíciles de detectar.

La ocultación de información ayuda precisamente porque impide el acceso directo a los atributos internos, obligando a que cualquier modificación se realice a través de métodos definidos por la clase. De esta manera, dichos métodos pueden incluir comprobaciones y validaciones que aseguren que las invariantes se mantienen, protegiendo la consistencia del objeto.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### 
A continuación se muestra un ejemplo de una clase `Punto` que aplica ocultación de información. Las coordenadas se declaran como `private`, de modo que no puedan modificarse directamente desde fuera de la clase. El cálculo de la distancia al origen se realiza mediante un método público:

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

En este diseño, la ocultación de información se consigue al declarar los atributos `x` e `y` como `private`. Esto impide que otras clases accedan directamente a las coordenadas (por ejemplo, haciendo `p.x = 5;`). En su lugar, el acceso y uso de los datos se realiza únicamente a través de los métodos definidos en la clase, manteniendo el control sobre el estado interno del objeto.

La **interfaz pública** de la clase `Punto` está formada por el constructor `Punto(double x, double y)` y el método `calcularDistanciaAOrigen()`, ya que son los únicos elementos declarados como `public`. El modificador `public` indica que el miembro puede ser utilizado desde cualquier otra clase, mientras que `private` restringe el acceso exclusivamente al interior de la propia clase, reforzando así la encapsulación.



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### 
En Java, los modificadores `public` y `private` se pueden aplicar a distintos elementos del programa para controlar su nivel de acceso. Principalmente, pueden utilizarse en **clases, atributos (variables miembro), métodos y constructores**. De esta manera, se define desde dónde puede accederse a cada uno de esos elementos.

En el caso de las clases, solo pueden declararse como `public` o sin modificador (acceso por defecto), pero no como `private` si se trata de clases externas (top-level). Sin embargo, las clases internas (definidas dentro de otra clase) sí pueden declararse como `private`, ya que forman parte de la implementación interna de la clase contenedora.

En cuanto a atributos, métodos y constructores, pueden declararse tanto `public` como `private`. Si se declaran `public`, podrán utilizarse desde otras clases; si se declaran `private`, solo podrán accederse desde el interior de la misma clase. Esto permite aplicar la ocultación de información y controlar cuidadosamente la interfaz pública.



## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### 
En Programación Orientada a Objetos, además de la visibilidad **pública** y **privada**, suelen existir otros niveles de acceso que permiten un control más fino sobre quién puede utilizar ciertos miembros de una clase. Estos niveles intermedios se introducen para facilitar la reutilización y la extensión del código sin exponer completamente la implementación interna.

En Java, además de `public` y `private`, existen dos niveles adicionales: el acceso por defecto (también llamado *package-private*) y `protected`. El acceso por defecto permite que el miembro sea visible únicamente dentro del mismo paquete. Por su parte, `protected` permite el acceso desde el mismo paquete y también desde clases que hereden de la clase original, incluso si están en otro paquete. De esta forma, Java ofrece cuatro niveles de visibilidad: `public`, `protected`, acceso por defecto y `private`.

En otros lenguajes orientados a objetos también existen niveles similares, aunque pueden variar en nombre o comportamiento. Por ejemplo, en C++ existen `public`, `protected` y `private`, pero no existe el concepto de paquete como en Java. En lenguajes como C#, además de niveles similares, pueden encontrarse modificadores adicionales como `internal`, que limita el acceso al mismo ensamblado. En general, los distintos lenguajes incorporan estos mecanismos para equilibrar encapsulación, reutilización y control de acceso.



## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Los miembros de instancia declarados como `private` están ocultos para **otras clases**, pero no para otras instancias de la misma clase. Esto significa que una clase puede acceder a los atributos privados de cualquier objeto que pertenezca a esa misma clase. La restricción de `private` se aplica a nivel de clase, no a nivel de objeto individual.

Por ejemplo, si se amplía la clase `Punto` con un método que calcule la distancia a otro punto, se puede acceder directamente a los atributos privados del objeto recibido como parámetro:

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En el método `calcularDistanciaAPunto`, se accede directamente a `otro.x` y `otro.y`, aunque son atributos `private`. Esto es posible porque ambos objetos (`this` y `otro`) pertenecen a la misma clase `Punto`. Si otra clase distinta intentara acceder a esos atributos, el compilador produciría un error. Por tanto, la respuesta correcta es (a): están ocultos para otras clases, pero no para otras instancias de la misma clase.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### 
Los métodos **getter** y **setter** son métodos públicos que permiten acceder y modificar, respectivamente, los atributos privados de una clase. Se utilizan como mecanismo controlado de acceso cuando se aplica la ocultación de información. En lugar de permitir el acceso directo a una variable de instancia, se define un método que devuelve su valor (getter) o que lo modifica bajo ciertas condiciones (setter).

Un **getter** suele tener un nombre como `getX()` y simplemente devuelve el valor del atributo correspondiente. Un **setter** suele llamarse `setX(...)` y recibe un parámetro con el nuevo valor que se desea asignar. A diferencia del acceso directo, el setter puede incluir validaciones para asegurar que se mantienen las invariantes de la clase, evitando estados inválidos.

Este patrón es común en Java y otros lenguajes orientados a objetos porque permite mantener los atributos como `private` y, al mismo tiempo, ofrecer una interfaz pública controlada. De esta manera, se conserva la encapsulación y se facilita el mantenimiento futuro del código.



## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### 
Cuando se dice que la ocultación de información mejora la “seguridad” del programa, no se refiere a protección frente a ataques externos o *hackeos*. Se trata de seguridad **lógica y de integridad interna del programa**, es decir, proteger los datos del objeto frente a modificaciones incorrectas o inesperadas desde otras partes del código. En otras palabras, garantiza que los objetos se mantengan en un estado consistente durante toda su ejecución.

Al declarar atributos como `private` y controlar el acceso mediante métodos públicos, se evita que otras clases modifiquen directamente los datos internos de forma indebida. Esto previene errores de programación, mantiene las invariantes de la clase y facilita la detección de problemas, ya que cualquier cambio en los datos pasa por un punto controlado.

Por tanto, la “seguridad” en este contexto se entiende como **robustez y confiabilidad del diseño**, más que protección contra ataques externos. Es una forma de reducir fallos lógicos y mantener la coherencia del software, no de protegerlo frente a intrusiones maliciosas.



## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### 
Un **miembro de instancia** es un atributo o método que pertenece a cada objeto creado a partir de una clase. Cada instancia tiene su propia copia de estos miembros, por lo que los valores pueden diferir entre objetos. Por ejemplo, en una clase `Punto`, los atributos `x` e `y` son miembros de instancia: cada `Punto` tiene sus propias coordenadas independientes de otros puntos.

En cambio, un **miembro de clase** (declarado con `static` en Java) pertenece a la clase en sí y no a objetos individuales. Solo existe una copia compartida entre todas las instancias. Por ejemplo, un contador de objetos creados puede definirse como `private static int contador;`, de modo que todas las instancias acceden a la misma variable.

Sí, los miembros de clase también se pueden ocultar mediante `private`. Esto significa que, aunque pertenezcan a la clase y no a cada objeto, solo podrán ser accedidos o modificados desde métodos de la propia clase, preservando la encapsulación. Los métodos estáticos `public` pueden ofrecer acceso controlado a estos miembros de clase, de la misma manera que los getters y setters lo hacen para miembros de instancia.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### 
Sí, tiene sentido que los constructores sean privados en ciertos diseños. Un constructor `private` impide que otras clases creen instancias directamente, lo que permite controlar estrictamente cómo y cuándo se generan objetos. Esto se utiliza, por ejemplo, en patrones de diseño como **Singleton**, donde se desea que solo exista una instancia de la clase.

Además, un constructor privado puede combinarse con métodos públicos estáticos que devuelvan instancias controladas, aplicando validaciones o gestionando recursos compartidos. De esta forma, se mantiene la encapsulación y se asegura que los objetos se crean de manera segura y coherente según las reglas de la clase.



## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### 
En Java, los **miembros de clase** se indican utilizando el modificador `static`. Esto aplica tanto a atributos como a métodos, y significa que pertenecen a la clase en sí y no a objetos individuales. Todos los objetos comparten la misma copia de los miembros `static`, por lo que se utilizan para almacenar información global o acumulativa relacionada con la clase.

A continuación se muestra un ejemplo de cómo modificar la clase `Punto` para llevar un registro de los valores máximos de `x` e `y` entre todos los objetos creados:

```java
public class Punto {

    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```

En este ejemplo, `maxX` y `maxY` son miembros de clase (`static`) que registran los valores máximos de `x` e `y` de todos los puntos creados. Los métodos `getMaxX()` y `getMaxY()` también son `static` y forman parte de la **interfaz pública de clase**, permitiendo acceder a esta información sin necesidad de crear una instancia de `Punto`. Los atributos `x` e `y` siguen siendo `private` para mantener la encapsulación de cada objeto individual.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### 
```java
public static Punto crearPuntoRedondeado(double x, double y) {
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
```

Sí, se ha usado `static` porque un **método factoría** pertenece a la clase y no a ninguna instancia específica. Esto permite llamar al método sin necesidad de crear un objeto `Punto` previamente, por ejemplo: `Punto p = Punto.crearPuntoRedondeado(2.7, 3.4);`.



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### 
```java
public class Punto {

    private double[] coordenadas = new double[2];

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + coordenadas[1] * coordenadas[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }

    public static Punto crearPuntoRedondeado(double x, double y) {
        int xRedondeado = (int) Math.round(x);
        int yRedondeado = (int) Math.round(y);
        return new Punto(xRedondeado, yRedondeado);
    }
}
```

En esta versión, los atributos `x` e `y` se reemplazan por un **array de dos posiciones** llamado `coordenadas`. La **interfaz pública** se mantiene: los métodos `calcularDistanciaAOrigen()`, `calcularDistanciaAPunto(Punto otro)`, el constructor, los getters de máximos (`getMaxX()`, `getMaxY()`) y el método factoría `crearPuntoRedondeado()` siguen funcionando igual desde fuera de la clase.

El cambio solo afecta a la implementación interna de la clase, reforzando la encapsulación: ahora se puede modificar la representación interna sin afectar a los usuarios de la clase.



## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Aunque un atributo tenga un **getter** y un **setter** públicos, no es recomendable declararlo `public`. La razón es que los métodos proporcionan un **punto de control** sobre cómo se accede o modifica el valor, permitiendo validar entradas, mantener invariantes o realizar operaciones adicionales. Si el atributo fuera público, se perdería esa capacidad de control, y cualquier parte del programa podría cambiarlo directamente, rompiendo la coherencia del objeto.

La convención más habitual en Java y en la mayoría de lenguajes orientados a objetos es declarar **todos los atributos como `private`** y proporcionar acceso mediante métodos públicos cuando sea necesario. Esto asegura que la clase pueda evolucionar internamente sin afectar a su interfaz pública, manteniendo la encapsulación y protegiendo la integridad del objeto.

Esto está directamente relacionado con las **invariantes de clase**, que son condiciones que siempre deben cumplirse para que un objeto esté en estado válido. Al usar `private` y métodos de acceso controlado, se puede garantizar que cualquier cambio en los atributos cumpla estas invariantes, evitando estados inválidos que podrían surgir si los atributos fueran accesibles directamente desde fuera de la clase.



## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### 
Una clase se considera **inmutable** cuando, una vez creado un objeto, su estado interno no puede cambiar durante toda su vida. Es decir, todos sus atributos permanecen constantes y no existen métodos que puedan modificarlos. En Java, esto normalmente se logra declarando todos los atributos como `private` y `final`, y evitando la existencia de setters o cualquier método que altere los valores internos. Un ejemplo clásico son las clases de la API de Java como `String` o `Integer`.

Un **método modificador** es cualquier método que cambia el estado interno de un objeto, alterando uno o más de sus atributos. Aunque los setters son métodos modificadores típicos, no todos los métodos modificadores son setters. Por ejemplo, un método que incremente un contador interno (`incrementarContador()`) también es un método modificador, aunque no tenga la forma estándar de setter `setX(...)`.

Las clases inmutables presentan varias ventajas. Al no permitir cambios de estado, los objetos se vuelven **intrínsecamente seguros para uso concurrente**, porque no requieren sincronización. Además, facilitan la depuración y el mantenimiento, ya que se puede confiar en que un objeto no cambiará inesperadamente. También mejoran la consistencia y la fiabilidad del programa, ya que se asegura que las invariantes de clase se mantienen de manera automática durante toda la vida del objeto.



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### 
No es recomendable incluir métodos **setter** de forma automática ni como convención general. Su uso debe depender de si realmente se necesita que el atributo pueda modificarse después de crear el objeto. Agregar setters innecesarios rompe la encapsulación, expone el estado interno y aumenta el riesgo de que las invariantes de la clase se violen.

Como convención en Java, se suele declarar todos los atributos como **private** y solo se crean setters cuando existe un motivo claro para permitir modificaciones externas. Si un atributo no debe cambiar, no se proporciona setter y, en casos más estrictos, se puede hacer `final` para reforzar la inmutabilidad. Esto garantiza que los objetos mantengan un estado consistente y reduce errores derivados de cambios inesperados.



## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### 
En Java, la clase **`String`** es **inmutable**, lo que significa que una vez creado un objeto `String`, su contenido no puede cambiar. Cualquier operación que aparentemente modifica la cadena, como concatenar, no altera el objeto original, sino que crea uno nuevo con el resultado. Esto garantiza que los objetos `String` sean seguros y confiables, manteniendo invariantes internas y evitando efectos colaterales no deseados.

Cuando se concatenan dos cadenas, por ejemplo usando `+`, Java crea un **nuevo objeto `String`** que contiene la combinación de ambas, y el objeto original permanece intacto. Esto implica que operaciones repetidas de concatenación generan múltiples objetos temporales, lo que puede ser ineficiente en términos de memoria y rendimiento cuando se construyen cadenas largas de forma iterativa.

Si se va a realizar una operación que implique concatenar muchas veces para construir una cadena larga, es recomendable utilizar **`StringBuilder`** o **`StringBuffer`**. Estas clases son **mutables** y permiten modificar el contenido internamente sin crear nuevos objetos en cada operación. Al final, se puede obtener un `String` final con `toString()`. Este enfoque es más eficiente y evita la sobrecarga de memoria y procesamiento asociada a la concatenación repetida de objetos inmutables `String`.




## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### 
En POO, los objetos de una clase se pueden comparar de dos formas diferentes: **por identidad** o **por contenido**. Comparar por identidad significa verificar si ambos objetos ocupan la misma posición en memoria, es decir, si son exactamente el mismo objeto. Comparar por contenido implica evaluar si los atributos de los objetos son equivalentes según la lógica de la clase, independientemente de que sean instancias distintas.

En Java, el método **`equals`** se utiliza para comparar objetos por **contenido**, siguiendo la semántica definida por la clase. Por defecto, la implementación de `equals` en la clase `Object` compara los objetos por **identidad** (igual que `==`). Para realizar comparaciones por contenido, muchas clases sobrescriben `equals`, como `String`, `Integer` o cualquier clase personalizada, definiendo qué atributos deben considerarse para determinar la igualdad.

Para comparar dos cadenas en Java, nunca se debe usar `==`, ya que esto comprobaría si ambas referencias apuntan al mismo objeto. En su lugar, se utiliza `s1.equals(s2)`, lo que compara los contenidos de las cadenas carácter por carácter. Esto asegura que dos cadenas con los mismos caracteres sean consideradas iguales, incluso si son objetos distintos en memoria.




## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### 
En un lenguaje orientado a objetos, las **clases "wrapper"** son clases que envuelven tipos de datos primitivos para tratarlos como objetos. En Java, por ejemplo, `int` tiene su wrapper `Integer`, `double` tiene `Double` y así sucesivamente. Esto permite utilizar los valores primitivos en contextos donde se requieren objetos, como colecciones (`ArrayList`, `HashMap`) o métodos que esperan objetos en lugar de tipos primitivos.

En Java, la conversión entre un tipo primitivo y su wrapper puede hacerse **automáticamente** gracias al mecanismo llamado **autoboxing** (de primitivo a objeto) y **unboxing** (de objeto a primitivo). Por ejemplo:

```java
int numero = 5;
Integer objeto = numero; // autoboxing
int otro = objeto;        // unboxing
```

Las ventajas de los wrappers incluyen:

* Permitir que los tipos primitivos se utilicen en colecciones y estructuras que requieren objetos.
* Proporcionar métodos útiles asociados al tipo, como `Integer.parseInt()` o `Double.isNaN()`.
* Facilitar la interoperabilidad con APIs orientadas a objetos.

No todos los lenguajes orientados a objetos tienen tipos primitivos separados; por ejemplo, en Python o Ruby, todos los valores son objetos, por lo que no necesitan wrappers. Java y C# sí los utilizan porque mantienen una distinción histórica entre tipos primitivos eficientes y objetos, combinando rendimiento con la flexibilidad de la POO.



## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### En POO, un **tipo de dato enumerado** o *enum* es un tipo especial que permite definir un conjunto limitado y fijo de valores posibles para una variable. A diferencia de un entero o cadena que puede tomar cualquier valor, un enumerado restringe los valores a un conjunto predefinido, lo que reduce errores y mejora la claridad del código. Por ejemplo, los días de la semana o los estados de un semáforo se podrían representar mediante un enumerado.

En Java, un tipo enumerado es realmente una **clase especial**, derivada de `java.lang.Enum`. Cada valor del enumerado es en realidad una instancia estática y final de esa clase. Esto permite que los enumerados tengan atributos, métodos y constructores, ofreciendo más flexibilidad que un simple conjunto de constantes. Por ejemplo, un enumerado de `DiaSemana` podría incluir un método que indique si es día laboral o fin de semana.

Los enumerados en Java mejoran la **encapsulación** porque permiten agrupar los valores posibles y asociarles comportamiento sin exponer detalles internos. Los valores de un enum son `public static final` por defecto y no pueden crearse instancias adicionales desde fuera de la clase, lo que asegura que la integridad del conjunto se mantiene y evita que se usen valores inválidos. Además, cualquier lógica asociada al enumerado se mantiene dentro de su propia clase, reforzando la consistencia y la claridad del diseño orientado a objetos.




## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### 
```java id="m7kq3n"
public enum Mes {

    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int orden;

    private Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() {
        return dias;
    }

    public int getOrden() {
        return orden;
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }
}
```

En este enumerado `Mes`:

* Cada mes tiene **atributos privados** `dias` y `orden`, inicializados mediante un **constructor** privado.
* Los métodos `getDias()` y `getOrden()` forman parte de la **interfaz pública** para acceder a esa información sin exponer los atributos directamente.
* Los métodos de estación (`esDeInvierno`, `esDePrimavera`, `esDeVerano`, `esDeOtoño`) usan un parámetro booleano `esHemisferioNorte` para devolver `true` si el mes corresponde a esa estación en el hemisferio indicado.

Esto muestra cómo un **enum en Java se comporta como una clase completa**, con atributos, métodos y constructor encapsulados.

