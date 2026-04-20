<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### 

En lenguajes como **C**, donde no existe orientación a objetos ni genericidad, una forma habitual de permitir que una estructura de datos almacene **cualquier tipo de dato** consiste en utilizar punteros genéricos (`void*`). Un `void*` puede apuntar a cualquier tipo de dato, pero pierde toda la información sobre el tipo concreto, por lo que el programador debe recordar y gestionar manualmente el tipo real almacenado. Este enfoque ofrece flexibilidad, pero carece de seguridad de tipos y desplaza la responsabilidad de comprobar los tipos al tiempo de ejecución, lo que puede provocar errores difíciles de detectar.

Un enfoque similar existe en **Java** utilizando la clase base `Object`. Dado que todas las clases heredan de `Object`, un array de `Object` puede almacenar instancias de cualquier clase. Esto permite construir estructuras de datos aparentemente genéricas sin usar genéricos reales. Sin embargo, al extraer los elementos es necesario realizar *downcasting*, lo que introduce riesgos similares a los del uso de `void*`: errores en tiempo de ejecución si el tipo no es el esperado y pérdida de comprobación de tipos en tiempo de compilación.

Ambas soluciones permiten almacenar datos heterogéneos en un mismo contenedor, pero presentan el mismo problema fundamental: **la ausencia de seguridad de tipos estática**. Estos ejemplos son la base histórica sobre la que surge la genericidad, cuyo objetivo principal será precisamente mantener la flexibilidad evitando conversiones inseguras y errores en tiempo de ejecución. Entender estos enfoques previos ayuda a apreciar mejor la utilidad de los genéricos en Java.

***

## ✅ **Ejemplo en C usando `void*`**

```c
#include <stdio.h>

typedef struct {
    void* datos[10];
    int size;
} VectorGenerico;

void agregar(VectorGenerico* v, void* elemento) {
    v->datos[v->size++] = elemento;
}

int main() {
    VectorGenerico v = { .size = 0 };

    int x = 10;
    double y = 3.14;

    agregar(&v, &x);
    agregar(&v, &y);

    int* px = (int*) v.datos[0];
    double* py = (double*) v.datos[1];

    printf("%d\n", *px);
    printf("%f\n", *py);
}
```

***

## ✅ **Ejemplo en Java usando `Object`**

```java
public class VectorGenerico {
    private Object[] datos = new Object[10];
    private int size = 0;

    public void agregar(Object o) {
        datos[size++] = o;
    }

    public Object obtener(int i) {
        return datos[i];
    }

    public static void main(String[] args) {
        VectorGenerico v = new VectorGenerico();

        v.agregar(10);         // Integer (autoboxing)
        v.agregar("hola");     // String

        Integer x = (Integer) v.obtener(0);
        String s = (String) v.obtener(1);

        System.out.println(x);
        System.out.println(s);
    }
}
```



## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### 

La **programación genérica** es un paradigma que consiste en escribir código **independiente del tipo de datos concreto** con el que va a trabajar. En lugar de diseñar una estructura o algoritmo para un tipo específico (por ejemplo, `int` o `String`), se diseña para un **tipo parametrizable**, de forma que el mismo código pueda reutilizarse con distintos tipos manteniendo la seguridad y el comportamiento correcto. El objetivo principal es combinar **reutilización de código** con **seguridad de tipos**, evitando duplicaciones y errores en tiempo de ejecución.

Los ejemplos anteriores que emplean `void*` en C o `Object` en Java **no son auténtica programación genérica**, sino **simulaciones básicas** de esta idea. En dichos ejemplos, el código permite almacenar cualquier tipo de dato, pero pierde información sobre el tipo real, obligando a realizar conversiones explícitas (*casts*) al recuperar los elementos. Estas conversiones no se comprueban en tiempo de compilación, por lo que los errores aparecen únicamente en tiempo de ejecución si el tipo no coincide con lo esperado.

Por tanto, aunque estos ejemplos muestran la intención de programar de forma genérica, **no cumplen completamente los principios de la programación genérica**. Falta la verificación estática de tipos que garantice que los datos almacenados y recuperados son coherentes. La programación genérica real, como la que proporcionan los **genéricos en Java**, resuelve precisamente este problema, permitiendo escribir estructuras reutilizables que mantienen la seguridad de tipos sin necesidad de conversiones explícitas.

En resumen, el ejemplo anterior ilustra el problema que la programación genérica pretende solucionar, pero no constituye todavía un ejemplo pleno de genericidad. Se trata más bien de un paso previo histórico y conceptual que ayuda a comprender por qué los genéricos son una mejora fundamental respecto al uso de `Object` o `void*`.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### 

El principal problema del uso de `void*` en C o de `Object` en Java para crear estructuras de datos “genéricas” es la **pérdida del chequeo de tipos en tiempo de compilación**. Al almacenar cualquier tipo de dato sin información de su tipo concreto, el compilador deja de poder comprobar si las operaciones que se realizan sobre esos datos son correctas. En consecuencia, errores que podrían detectarse antes de ejecutar el programa se posponen al tiempo de ejecución, aumentando el riesgo de fallos inesperados.

Otro problema importante es la necesidad de realizar **conversiones explícitas (*casts*)** al recuperar los elementos almacenados. El programador debe recordar manualmente qué tipo se guardó en cada posición y convertirlo al tipo adecuado. Si la conversión es incorrecta, el programa puede provocar errores graves, como accesos inválidos a memoria en C o excepciones de tipo (`ClassCastException`) en Java. Este enfoque hace que el código sea más frágil y dependiente del uso correcto por parte del programador.

Además, este modelo dificulta el mantenimiento y la evolución del código. Al no existir una relación explícita entre la estructura de datos y los tipos que almacena, resulta complicado entender qué se espera de cada elemento sin consultar documentación externa o revisar el código que la utiliza. Esto incrementa el acoplamiento implícito y reduce la claridad del diseño, especialmente en programas de tamaño medio o grande.

En resumen, el uso de `void*` u `Object` permite flexibilidad, pero a costa de sacrificar el **chequeo estático de tipos**, la seguridad y la claridad del código. Precisamente estos problemas son los que la programación genérica moderna intenta resolver, permitiendo escribir estructuras reutilizables sin renunciar a la comprobación de tipos en tiempo de compilación.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### 

Los **parámetros de tipo** son el mecanismo fundamental de la **programación genérica**, y consisten en permitir que una clase, un método o una interfaz trabajen con **tipos que no se especifican de antemano**, sino que se indican cuando el código se utiliza. En lugar de fijar un tipo concreto, se introduce un “parámetro” que representa a un tipo cualquiera, normalmente expresado con letras como `T`, `E` o `K`. De este modo, el mismo código puede aplicarse a distintos tipos sin necesidad de duplicarlo.

La principal ventaja de los parámetros de tipo es que **preservan el chequeo de tipos en tiempo de compilación**. A diferencia de soluciones basadas en `void*` o `Object`, el compilador conoce qué tipo concreto se está usando en cada caso y puede verificar que las operaciones realizadas son correctas. Esto elimina la necesidad de conversiones explícitas y reduce drásticamente los errores en tiempo de ejecución relacionados con tipos incorrectos.

En Java, los parámetros de tipo se introducen entre signos angulares (`< >`) y se utilizan para definir clases o métodos genéricos. Por ejemplo, una estructura de datos puede declararse con un parámetro de tipo `T`, y posteriormente decidir si se usa con `Integer`, `String` o cualquier otra clase. El código de la estructura no cambia, pero el compilador garantiza que solo se almacenan y recuperan valores del tipo indicado.

En resumen, los parámetros de tipo permiten escribir código reutilizable, seguro y expresivo, solucionando los problemas clásicos de la programación “genérica” tradicional. Constituyen la base de la genericidad moderna en Java y representan una mejora clara frente al uso indiscriminado de `Object`, al combinar flexibilidad con seguridad de tipos.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### 

La programación genérica permite definir estructuras y algoritmos independientes del tipo concreto con el que trabajan, manteniendo al mismo tiempo la **seguridad de tipos**. Tanto Java como C++ proporcionan mecanismos propios para ello: **genéricos** en Java y **templates** en C++. En ambos casos, el objetivo es evitar estructuras basadas en `Object` o `void*`, eliminando conversiones inseguras y desplazando los errores de tipo al tiempo de compilación en lugar de al tiempo de ejecución.

En **Java**, los genéricos permiten declarar colecciones que solo admiten un tipo concreto, como `String`. El compilador garantiza que únicamente se puedan introducir elementos del tipo indicado y que, al recuperarlos, no sea necesario realizar *casts*. Esto hace que el recorrido de la colección sea seguro y claro, ya que cada elemento recuperado es directamente del tipo esperado.

En **C++**, los *templates* permiten un efecto similar, pero con una diferencia importante: la genericidad se resuelve completamente en **tiempo de compilación**. Cuando se instancia un `std::vector<std::string>`, el compilador genera una versión específica del contenedor para ese tipo. Al igual que en Java, no se requieren conversiones y cada elemento del vector es tratado directamente como `std::string`, con total seguridad de tipos.

Estos ejemplos muestran cómo tanto genéricos como templates resuelven los problemas clásicos de las estructuras “genéricas” basadas en `Object` o `void*`, proporcionando reutilización de código sin perder comprobación de tipos ni claridad en el uso.

***

## ✅ **Ejemplo en Java con generics**

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploGenericsJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");

        for (String s : lista) {
            // s es String con seguridad, no hay cast
            System.out.println(s.toUpperCase());
        }
    }
}
```

✅ La lista **solo admite `String`** y el tipo se conoce con seguridad en el recorrido.

***

## ✅ **Ejemplo en C++ con templates**

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");

    for (const std::string& s : lista) {
        // s es std::string con seguridad
        std::cout << s << std::endl;
    }

    return 0;
}
```

✅ El vector está **especializado en `std::string`** y no permite otros tipos.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### 

Cuando se instancia una clase o estructura **genérica**, el compilador debe decidir cómo manejar los **parámetros de tipo**. La idea general es que el código genérico se escribe una sola vez, pero debe funcionar correctamente con distintos tipos concretos. Sin embargo, **Java y C++ adoptan estrategias muy distintas** para conseguirlo, lo que tiene consecuencias importantes a nivel de rendimiento, información de tipos disponible y compatibilidad con versiones anteriores.

En **Java**, el compilador aplica un mecanismo llamado **type erasure** (borrado de tipos). Esto significa que los parámetros de tipo **se eliminan en tiempo de compilación** y el código generado utiliza tipos no genéricos, normalmente `Object`, junto con conversiones internas automáticas. En tiempo de ejecución **no existe información sobre los tipos genéricos**, por lo que `List<String>` y `List<Integer>` son en realidad el mismo tipo. Este enfoque se eligió para mantener compatibilidad con versiones antiguas de Java previas a los genéricos, pero implica ciertas limitaciones, como no poder crear instancias de tipos genéricos ni comprobar el tipo parametrizado en tiempo de ejecución.

En **C++**, en cambio, se utiliza la **instanciación de plantillas** (*template instantiation*). El compilador genera una **versión específica del código para cada tipo concreto** con el que se instancia la plantilla. Por ejemplo, `std::vector<int>` y `std::vector<std::string>` producen códigos distintos. Esto conserva toda la información de tipos en compilación y ejecución, permite mayor optimización y evita conversiones implícitas, pero a cambio genera más código y puede aumentar el tamaño del binario.

En resumen, **Java utiliza type erasure**, eliminando los tipos genéricos tras la compilación, mientras que **C++ genera código especializado para cada tipo** mediante instanciación de plantillas. Ambos mecanismos permiten programación genérica, pero con filosofías distintas: Java prioriza compatibilidad y simplicidad, mientras que C++ prioriza rendimiento y expresividad del sistema de tipos.



## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### 

Una clase con **parámetros de tipo** permite abstraer no solo sobre un tipo concreto, sino sobre varios a la vez. En Java, es posible definir una clase genérica con **más de un parámetro de tipo**, lo que resulta útil cuando se desea agrupar valores relacionados pero de tipos distintos. Este enfoque evita la creación de clases específicas para cada combinación de tipos y mantiene la seguridad de tipos en tiempo de compilación, sin recurrir a `Object` ni a conversiones explícitas.

La clase `Par` es un ejemplo clásico de este concepto. Mediante dos parámetros de tipo, normalmente llamados `A` y `B`, se define una estructura que puede almacenar dos valores heterogéneos de forma segura. El compilador garantiza que cada elemento del par se utiliza siempre con su tipo correcto, lo que simplifica el código y elimina posibles errores en tiempo de ejecución. Este tipo de estructuras es especialmente útil para devolver múltiples resultados de una función sin necesidad de crear clases específicas para cada caso.

Un uso habitual de este patrón consiste en devolver varias magnitudes relacionadas calculadas a partir de un conjunto de datos. Por ejemplo, al analizar un array de valores numéricos, puede resultar conveniente devolver tanto la **media** como la **desviación típica** en una sola llamada. Utilizando un `Par<Double, Double>`, se pueden agrupar ambos resultados de forma clara y segura, manteniendo un diseño limpio y expresivo.

En resumen, las clases genéricas con varios parámetros de tipo permiten modelar relaciones simples entre datos heterogéneos, favoreciendo la reutilización del código y evitando soluciones menos seguras basadas en tipos genéricos débiles. Este mecanismo es una extensión natural de la programación genérica en Java y una herramienta muy habitual en bibliotecas y APIs reales.

***

## ✅ **Ejemplo en Java**

### Clase genérica `Par`

```java
public class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```

***

### Función que devuelve media y desviación típica

```java
public class Estadisticas {

    public static Par<Double, Double> calcularMediaYDesviacion(double[] valores) {
        double suma = 0.0;
        for (double v : valores) {
            suma += v;
        }
        double media = suma / valores.length;

        double sumaCuadrados = 0.0;
        for (double v : valores) {
            double diff = v - media;
            sumaCuadrados += diff * diff;
        }
        double desviacion = Math.sqrt(sumaCuadrados / valores.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] datos = { 2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0 };

        Par<Double, Double> resultado = calcularMediaYDesviacion(datos);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}
```

***

✅ En este ejemplo:

*   `Par<Double, Double>` expresa claramente el **tipo de ambos resultados**.
*   No se realizan conversiones explícitas.
*   El compilador garantiza la corrección de los tipos en todo el uso del código.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### 

En Java, los **parámetros de tipo** no solo pueden declararse a nivel de clase, sino también **a nivel de método**. Un método genérico permite introducir sus propios parámetros de tipo, de modo que la genericidad se limita al propio método y no afecta a toda la clase. Esto resulta especialmente útil para utilidades estáticas o para operaciones puntuales donde se desea trabajar de forma genérica sin necesidad de parametrizar la clase completa.

Si un método se define utilizando parámetros de tipo genéricos, el compilador puede **inferir y comprobar los tipos en tiempo de compilación**, garantizando que los argumentos tienen el mismo tipo y que el valor devuelto también lo es. En cambio, cuando el método se define empleando `Object`, se pierde esa información: el compilador permite pasar objetos de tipos distintos y obliga a realizar *downcasting* al recibir el resultado, desplazando los errores al tiempo de ejecución.

El uso de parámetros de tipo en métodos genéricos mejora dos aspectos clave. Primero, **evita el downcasting**, ya que el tipo concreto del valor devuelto es conocido por el compilador. Segundo, **fuerza que los argumentos sean del mismo tipo**, impidiendo combinaciones incorrectas como pasar un `String` y un `Integer`. Estas garantías no existen cuando se utiliza `Object`, lo que hace al código menos seguro y más propenso a errores.

En resumen, los métodos genéricos permiten escribir código reutilizable y flexible sin renunciar a la seguridad de tipos, demostrando que la genericidad no se limita a las clases, sino que puede aplicarse de manera más fina y controlada a operaciones concretas.

***

## ✅ **Versión sin genéricos (usando `Object`)**

```java
import java.util.Random;

public class UtilidadesObject {

    public static Object seleccionaUno(Object a, Object b) {
        return new Random().nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        Object resultado = seleccionaUno("Hola", "Adiós");

        // Es necesario hacer downcasting
        String texto = (String) resultado;
        System.out.println(texto);
    }
}
```

❌ Problemas:

*   Se requiere **downcasting**.
*   El compilador permite llamadas incorrectas:
    ```java
    seleccionaUno("Hola", 10); // permitido, pero conceptualmente erróneo
    ```
*   Error posible en tiempo de ejecución.

***

## ✅ **Versión con método genérico**

```java
import java.util.Random;

public class UtilidadesGenericas {

    public static <T> T seleccionaUno(T a, T b) {
        return new Random().nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String texto = seleccionaUno("Hola", "Adiós");
        System.out.println(texto);

        Integer numero = seleccionaUno(3, 7);
        System.out.println(numero);
    }
}
```

✅ Ventajas:

*   No hay **downcasting**.
*   El compilador **fuerza que ambos argumentos sean del mismo tipo**.
*   Cualquier intento incorrecto se detecta en compilación:
    ```java
    seleccionaUno("Hola", 10); // ❌ error de compilación
    ```

***

✅ **Conclusión clave**  
Los métodos genéricos aportan seguridad de tipos, claridad y prevención de errores sin sacrificar reutilización, superando claramente a las soluciones basadas en `Object` y representando una de las aplicaciones más potentes de la programación genérica en Java.



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

###

En Java, **sí es posible establecer restricciones en los parámetros de tipo**, lo que se denomina *acotación* (*bounds*). Estas restricciones permiten indicar que un parámetro genérico debe ser, como mínimo, una subclase de un tipo concreto o implementar una interfaz determinada. Por ejemplo, al declarar un parámetro genérico `<T extends Number>`, se fuerza que cualquier tipo usado con ese genérico sea algún tipo numérico (`Integer`, `Double`, etc.). Esto permite tratar los valores genéricos como números de forma segura, sin perder el chequeo de tipos en tiempo de compilación.

Una primera solución sencilla para modelar un `Punto` con coordenadas numéricas consiste en usar directamente el tipo `Number` para las coordenadas. Esta aproximación permite almacenar cualquier número, pero pierde información sobre el tipo concreto, ya que ambos valores quedan reducidos a `Number`. Como consecuencia, las operaciones deben realizarse convirtiendo a tipos primitivos (`doubleValue()`) y no se puede saber si el punto trabaja originalmente con `Integer`, `Double` u otro subtipo. Aunque es funcional, esta solución debilita el sistema de tipos.

Una solución más robusta consiste en utilizar **parámetros de tipo acotados**, por ejemplo `<T extends Number>`. De este modo, el tipo concreto de las coordenadas queda fijado al crear el punto, y el compilador garantiza que ambas coordenadas y los puntos utilizados en el cálculo son compatibles entre sí. Esta aproximación refuerza el chequeo de tipos y evita combinaciones incorrectas, como mezclar puntos con coordenadas de distinto tipo numérico. Desde el punto de vista del diseño, esta solución expresa mejor la intención del código.

Respecto al **type erasure**, en ambos casos el compilador de Java elimina la información de los parámetros genéricos tras la compilación. En la versión genérica, el tipo final en el bytecode será `Number`, ya que es el límite superior del parámetro de tipo. Es decir, aunque en el código fuente se trabaje con `<T extends Number>`, en tiempo de ejecución Java no distingue entre `Punto<Integer>` y `Punto<Double>`. La ventaja de los genéricos reside en el chequeo de tipos en compilación, no en la existencia de tipos distintos en ejecución.

***

## ✅ **Solución 1: Coordenadas usando `Number`**

```java
public class Punto {
    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = otro.x.doubleValue() - this.x.doubleValue();
        double dy = otro.y.doubleValue() - this.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

***

## ✅ **Solución 2: Usando generics con restricción (`extends Number`)**

```java
public class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = otro.x.doubleValue() - this.x.doubleValue();
        double dy = otro.y.doubleValue() - this.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

***

✅ **Conclusión clave**

*   Las restricciones (`extends`) permiten tratar genéricos como tipos concretos.
*   Usar `Number` es flexible pero menos seguro.
*   Usar `<T extends Number>` refuerza el chequeo de tipos.
*   Tras la compilación, **el tipo final es `Number`** debido al *type erasure*.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### 

Ambas soluciones planteadas permiten trabajar con distintos tipos de números sin duplicar la clase `Punto`, pero difieren claramente en el **grado de refuerzo del chequeo de tipos** que ofrecen. En la solución que utiliza directamente el tipo `Number` para las coordenadas, se permite una gran flexibilidad, ya que cada coordenada puede ser de un subtipo distinto de `Number`. Esto implica que es posible crear un punto con una coordenada entera y otra real sin que el compilador lo impida. Aunque esta opción es funcional, desplaza parte de la responsabilidad al programador, ya que el sistema de tipos no garantiza homogeneidad entre las coordenadas.

En cambio, la solución basada en genéricos con una restricción `<T extends Number>` refuerza el chequeo de tipos al **forzar que ambas coordenadas sean del mismo tipo concreto**. Al crear un `Punto<Integer>` o un `Punto<Double>`, el compilador garantiza que tanto `x` como `y` pertenecen exactamente a ese tipo. Como consecuencia, no es posible mezclar una coordenada entera con una real dentro del mismo punto, y cualquier intento de hacerlo se detecta en tiempo de compilación. Este diseño expresa de forma más precisa la intención del modelo y evita combinaciones incorrectas desde el punto de vista conceptual.

Respecto a los métodos de acceso, en la solución sin genéricos el método `getX` devuelve siempre un objeto de tipo `Number`. Esto obliga a tratar el valor de forma genérica y, si se necesita un tipo concreto, a realizar conversiones posteriores. En la solución con genéricos, en cambio, `getX` devuelve el tipo `T`, es decir, el tipo concreto con el que se creó el punto. Esto proporciona mayor claridad y seguridad, ya que el tipo devuelto es conocido estáticamente por el compilador y no requiere conversiones explícitas.

En resumen, ambas soluciones evitan la duplicación de código y permiten trabajar con distintos tipos numéricos, pero la versión con genéricos ofrece un **chequeo de tipos más estricto y expresivo**. Aunque internamente Java aplica *type erasure* y el tipo final en tiempo de ejecución sigue siendo `Number`, la ventaja de los genéricos reside en el control que se ejerce en tiempo de compilación, reduciendo errores y mejorando la calidad del diseño.



## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### 

En el diseño original, el método `distanciaA(Punto p)` acepta cualquier implementación de `Punto`, lo que obliga a comprobar en tiempo de ejecución si el punto recibido es del tipo correcto mediante `instanceof` y a realizar *downcasting*. Este enfoque funciona, pero **debilita el chequeo de tipos**, ya que permite escribir código incorrecto que solo falla en tiempo de ejecución. El problema de fondo es que el contrato de la interfaz no expresa que la distancia solo tiene sentido entre puntos del **mismo tipo concreto**.

La **programación genérica** permite reforzar este contrato haciendo explícito, en el propio tipo, que un punto solo puede calcular distancias respecto a otros puntos de su misma clase. Esto se consigue parametrizando la interfaz con un **parámetro de tipo recursivo**, de forma que el método `distanciaA` reciba exactamente ese mismo tipo. Con ello, el compilador garantiza que no se pueden mezclar puntos 2D y 3D, eliminando por completo la necesidad de comprobaciones dinámicas y conversiones inseguras.

Este patrón, conocido como *self types* o *F-bounded polymorphism*, combina genericidad y polimorfismo para expresar relaciones más precisas entre tipos. Gracias a él, cada implementación concreta decide con qué tipo es compatible, y cualquier intento de usar puntos incompatibles se detecta en **tiempo de compilación**, no en ejecución. El diseño resultante es más seguro, más claro y mejor alineado con los principios de la programación genérica.

Además, aunque Java aplique **type erasure** tras la compilación, el beneficio permanece: todo el refuerzo del chequeo de tipos ocurre antes de ejecutar el programa. En tiempo de ejecución, el tipo genérico se borra, pero el código generado ya es correcto y no necesita defensas adicionales.

***

## ✅ **Versión mejorada con generics (sin `instanceof`)**

### Interfaz genérica `Punto<T>`

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

***

### Implementación `Punto2D`

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

***

### Implementación `Punto3D`

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        double dz = z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

***

## ✅ **Uso seguro (el compilador impide errores)**

```java
Punto2D a2 = new Punto2D(0, 0);
Punto2D b2 = new Punto2D(3, 4);
System.out.println(a2.distanciaA(b2)); // OK

Punto3D a3 = new Punto3D(0, 0, 0);
Punto3D b3 = new Punto3D(1, 2, 2);
System.out.println(a3.distanciaA(b3)); // OK

// a2.distanciaA(a3); // ❌ ERROR de compilación
```

***

## ✅ **Conclusión clave**

*   La versión original permite mezclar tipos y requiere `instanceof`.
*   La versión con genéricos **expresa el contrato correctamente en el tipo**.
*   Se elimina *downcasting* y errores en ejecución.
*   El compilador garantiza coherencia dimensional.
*   Tras el *type erasure*, el diseño sigue siendo seguro porque los errores ya no pueden compilar.




## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### 

Aunque `String` sea subtipo de `Object`, **no es cierto que `List<String>` sea subtipo de `List<Object>`** en Java. Los **tipos genéricos en Java son invariantes** respecto a su parámetro de tipo. Esto significa que, aunque exista una relación de herencia entre los parámetros (`String` → `Object`), dicha relación **no se propaga** al tipo genérico completo. La razón es la seguridad de tipos: si se permitiera tratar una `List<String>` como una `List<Object>`, se podrían insertar objetos que no sean `String`, rompiendo la garantía de que la lista contiene solo cadenas.

En cambio, **los arrays en Java sí son covariantes**, por lo que `String[]` **sí es subtipo** de `Object[]`. Esto permite, por ejemplo, asignar un array de `String` a una referencia de tipo `Object[]`. Sin embargo, esta decisión tiene un coste: **los arrays realizan comprobaciones de tipo en tiempo de ejecución**. Si se intenta insertar un objeto que no sea realmente un `String` en ese array, se produce una excepción en tiempo de ejecución (`ArrayStoreException`). Es decir, el compilador lo permite, pero el error aparece al ejecutar el programa, lo que debilita la seguridad.

Estos dos comportamientos distintos reflejan decisiones de diseño diferentes. En los genéricos, Java prioriza la **seguridad en tiempo de compilación**, incluso a costa de ser más restrictivo (invariancia). En los arrays, Java prioriza la compatibilidad histórica, aceptando la covariancia y desplazando los errores al tiempo de ejecución. Por ello, mientras los genéricos evitan directamente combinaciones peligrosas, los arrays permiten escribir código potencialmente incorrecto que solo falla al ejecutarse.

A partir de estos ejemplos, se pueden definir los conceptos de **variancia**: un tipo genérico es **covariante** si mantiene la relación de herencia de su parámetro (`A` subtipo de `B` ⇒ `G<A>` subtipo de `G<B>`), como ocurre con los arrays. Es **contravariante** si la relación se invierte. Y es **invariante** si no existe relación de subtipo en ningún sentido entre `G<A>` y `G<B>`, aunque `A` y `B` estén relacionados, que es el caso de los genéricos en Java. Esta invariancia es precisamente la que garantiza que estructuras genéricas sean seguras y libres de errores de tipo en tiempo de ejecución.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### 

Un **wildcard** (`?`) en Java representa un **tipo desconocido** dentro de un tipo genérico. Se utiliza cuando no interesa fijar exactamente el tipo parametrizado, sino **establecer una relación de compatibilidad**. Los wildcards permiten recuperar de forma controlada la covarianza y contravarianza que los genéricos normales no admiten, evitando los problemas de seguridad de tipos que aparecen con los arrays. En lugar de decir “esta lista es exactamente de `T`”, se puede expresar “esta lista contiene *algún* tipo relacionado con `T`”.

La expresión **`List<? extends T>`** indica una lista que contiene elementos de **algún subtipo de `T`**, aunque no se conoce cuál exactamente. Esta forma es **covariante** y resulta útil cuando la lista se va a **leer**, pero no a modificar. El compilador permite tratar los elementos como `T`, pero **impide añadir nuevos elementos**, ya que el tipo concreto almacenado es desconocido. Esta regla se resume habitualmente como *“Producer Extends”*: cuando una estructura produce datos, se usa `extends`.

Por el contrario, **`List<? super T>`** indica una lista que contiene elementos de **algún supertipo de `T`**. Esta forma es **contravariante** y se emplea cuando la lista se va a **modificar**, es decir, cuando se van a insertar elementos de tipo `T`. En este caso, el compilador garantiza que cualquier `T` es compatible con la lista, pero al leer los elementos solo se puede asumir que son objetos de tipo `Object`. Esta regla se recuerda como *“Consumer Super”*: cuando la estructura consume datos, se usa `super`.

En conjunto, los wildcards permiten expresar con precisión cómo se usan las estructuras genéricas: si se leen, si se escriben o ambas cosas. De este modo, Java combina flexibilidad con seguridad de tipos, evitando errores tanto en compilación como en ejecución.

***

## ✅ **Ejemplo (i): sumar números usando `? extends`**

```java
import java.util.List;

public class UtilNumeros {

    public static double sumar(List<? extends Number> numeros) {
        double suma = 0.0;
        for (Number n : numeros) {
            suma += n.doubleValue();
        }
        return suma;
    }
}
```

✅ Se acepta `List<Integer>`, `List<Double>`, etc.  
❌ No se pueden añadir elementos a la lista dentro del método.

***

## ✅ **Ejemplo (ii): añadir enteros usando `? super`**

```java
import java.util.List;

public class Inserciones {

    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
        lista.add(30);
    }
}
```

✅ Se puede insertar `Integer` en `List<Integer>`, `List<Number>` o `List<Object>`.  
❌ Al leer, los elementos solo pueden tratarse como `Object`.

***

## ✅ **Resumen**

*   `?` → tipo desconocido
*   `? extends T` → **covariante**, solo lectura
*   `? super T` → **contravariante**, solo escritura
*   Genéricos sin wildcards → **invariantes**

