<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### 

Un **puntero a una función** en C es una variable que puede almacenar la **dirección de memoria de una función**, permitiendo invocarla de forma indirecta. Esto hace posible tratar a las funciones como datos, pasarlas como parámetros, devolverlas desde otras funciones o almacenarlas en estructuras. Desde el punto de vista conceptual, un puntero a función describe *qué tipo de función se puede llamar* (tipo de retorno y parámetros), no el código concreto que se ejecutará.

Este mecanismo es uno de los antecedentes directos de los aspectos funcionales en lenguajes modernos. Aunque C no es un lenguaje funcional, los punteros a función permiten desacoplar el **qué se hace** del **cuándo o cómo se llama**, facilitando diseños más flexibles. Por ejemplo, un mismo algoritmo puede recibir distintas funciones para aplicar comportamientos diferentes, algo muy habitual en bibliotecas de C y sistemas embebidos.

En el ejemplo propuesto, se define una función que recibe una cadena de caracteres y devuelve una nueva cadena con el contenido en mayúsculas. Posteriormente, se declara un puntero a función con la misma firma, se le asigna la función definida y se invoca a través del puntero. La llamada indirecta demuestra que el uso del puntero es transparente para el código que ejecuta la función.

Este tipo de construcción muestra cómo, incluso en C, se pueden expresar ideas cercanas a la programación funcional, que más adelante aparecerán de forma más estructurada en lenguajes como Java mediante lambdas e interfaces funcionales.

***

## ✅ **Ejemplo en C: puntero a función**

```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

/* Función que convierte una cadena a mayúsculas */
char* aMayusculasFuncion(const char* texto) {
    size_t len = strlen(texto);
    char* resultado = malloc(len + 1);

    for (size_t i = 0; i < len; i++) {
        resultado[i] = toupper((unsigned char) texto[i]);
    }
    resultado[len] = '\0';
    return resultado;
}

int main() {
    /* Puntero a función */
    char* (*aMayusculas)(const char*);

    /* Asignación de la función al puntero */
    aMayusculas = aMayusculasFuncion;

    /* Invocación mediante el puntero */
    char* texto = aMayusculas("Aspectos funcionales en C");

    printf("%s\n", texto);

    free(texto);
    return 0;
}
```

***

✅ **Idea clave**  
Un puntero a función permite invocar una función de forma indirecta, habilitando patrones flexibles que anticipan muchos conceptos de la programación funcional moderna. Si quieres, en la siguiente pregunta se puede ver cómo esta idea evoluciona en Java con **interfaces funcionales y expresiones lambda**.



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### 

Una **función lambda** es una función **anónima**, es decir, una función sin nombre, que puede definirse de forma compacta y tratarse como un valor. Este tipo de funciones permite asignar comportamiento a variables, pasarlas como parámetros o devolverlas como resultado, sin necesidad de definir una función completa con nombre. Conceptualmente, las funciones lambda representan la evolución natural de los punteros a función en C hacia un modelo más seguro y expresivo.

En lenguajes como **JavaScript**, las funciones son ciudadanos de primera clase, por lo que las funciones lambda forman parte del lenguaje desde su base. Una función lambda puede asignarse directamente a una variable y utilizarse como cualquier otra función. El código resulta más conciso y expresivo que con funciones tradicionales, manteniendo la misma funcionalidad que un puntero a función en C, pero con una sintaxis más sencilla.

En **Java**, las funciones lambda se introducen a partir de Java 8 y se apoyan en el concepto de **interfaces funcionales**, es decir, interfaces con un único método abstracto. Una función lambda no existe de forma aislada, sino que siempre se asigna a una referencia de un tipo funcional, como `Function<T, R>`. Esto permite integrar el estilo funcional dentro del modelo orientado a objetos de Java, manteniendo la seguridad de tipos y el chequeo en tiempo de compilación.

En ambos lenguajes, el uso de una variable local llamada `aMayusculas` para apuntar a la función lambda muestra cómo el comportamiento puede almacenarse y ejecutarse indirectamente. La principal diferencia es que JavaScript ofrece esta capacidad de forma nativa, mientras que Java la introduce de manera controlada a través de su sistema de tipos.

***

## ✅ **Ejemplo en JavaScript**

```javascript
// Función lambda que convierte un texto a mayúsculas
let aMayusculas = (texto) => texto.toUpperCase();

// Invocación de la función
console.log(aMayusculas("Aspectos funcionales en JavaScript"));
```

***

## ✅ **Ejemplo en Java usando lambda y Function\<String, String>**

```java
import java.util.function.Function;

public class EjemploLambdaJava {
    public static void main(String[] args) {

        // Función lambda asignada a una variable local
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();

        // Invocación de la función
        String resultado = aMayusculas.apply("Aspectos funcionales en Java");
        System.out.println(resultado);
    }
}
```

***

✅ **Idea clave**  
Las funciones lambda permiten tratar el comportamiento como un valor. En JavaScript forman parte del núcleo del lenguaje, mientras que en Java se integran mediante interfaces funcionales, ofreciendo una versión tipada y segura de los punteros a función vistos previamente en C.



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### 

El **paradigma funcional** es un estilo de programación en el que el programa se construye principalmente mediante **funciones** que transforman datos, evitando en lo posible el uso de estado mutable y de efectos secundarios. En este paradigma, el énfasis no está en describir *cómo* se realizan los pasos (como ocurre en el estilo imperativo), sino en describir *qué transformación* se aplica a los datos de entrada para obtener un resultado. Las funciones se tratan como valores y el cálculo se entiende como la evaluación de expresiones.

Se denomina a lenguajes como **Java (a partir de Java 8)** lenguajes **multi‑paradigma** porque permiten combinar distintos estilos de programación dentro del mismo lenguaje. Java sigue siendo fundamentalmente orientado a objetos, pero incorpora elementos del paradigma funcional, como funciones lambda, interfaces funcionales y operaciones sobre colecciones de tipo funcional. Esto permite elegir el enfoque más adecuado según el problema, sin abandonar el lenguaje ni su modelo de tipos.

Decir que las funciones son **“ciudadanos de primera clase”** significa que las funciones pueden tratarse como cualquier otro valor del lenguaje. Esto implica que pueden asignarse a variables, pasarse como argumentos a otras funciones y devolverse como resultado. En C esto se logra mediante punteros a función; en Java moderno se consigue mediante expresiones lambda y referencias a métodos; y en lenguajes como JavaScript es una característica central del lenguaje. Esta capacidad es uno de los pilares del paradigma funcional.

En resumen, el paradigma funcional introduce una forma distinta de estructurar programas basada en funciones y transformaciones de datos. Java es considerado multi‑paradigma porque integra estas ideas sin abandonar la orientación a objetos, y la noción de funciones como ciudadanos de primera clase es lo que permite expresar comportamiento como datos, facilitando diseños más expresivos y flexibles.



## 4. Explica la sintaxis básica de una función lambda en Java.

### 

La **sintaxis básica de una función lambda en Java** sirve para definir una implementación de un método de forma compacta, sin necesidad de crear una clase completa. Una expresión lambda representa el **cuerpo de un método**, pero sin nombre, y siempre se asocia a una **interfaz funcional**, es decir, una interfaz que define un único método abstracto. La sintaxis general consta de tres partes: la lista de parámetros, el operador `->` y el cuerpo de la función.

La forma general es `(parámetros) -> expresión` o `(parámetros) -> { bloque }`. Si el método recibe un solo parámetro, pueden omitirse los paréntesis; si el cuerpo consiste en una única expresión, pueden omitirse las llaves y la palabra clave `return`. Cuando el cuerpo contiene varias instrucciones, deben usarse llaves y un `return` explícito si el método devuelve un valor. El tipo de los parámetros suele inferirse automáticamente, por lo que no es necesario indicarlo.

El operador `->` se denomina **operador lambda** y separa los parámetros del comportamiento que se ejecutará. Conceptualmente, la lambda define *qué hacer* cuando se invoque el método abstracto de la interfaz funcional asociada. Desde el punto de vista del programador que viene de C, esta sintaxis puede entenderse como una forma más segura y estructurada de expresar un puntero a función.

En resumen, una lambda en Java es una forma concisa de expresar una función anónima, donde el compilador se encarga de enlazarla con la interfaz funcional adecuada. Esta sintaxis reduce el código repetitivo, mejora la legibilidad y facilita el uso de estilos funcionales dentro del lenguaje orientado a objetos.

***

## ✅ **Ejemplos de sintaxis**

```java
// Un parámetro, una expresión
x -> x * 2
```

```java
// Varios parámetros
(a, b) -> a + b
```

```java
// Bloque de instrucciones
(texto) -> {
    String r = texto.toUpperCase();
    return r;
}
```



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### 

Recibir una **función como parámetro** permite separar claramente el **algoritmo general** de la **operación concreta** que se desea aplicar. En el paradigma funcional, esta técnica se utiliza para escribir métodos más reutilizables y flexibles, ya que el comportamiento específico se delega en una función externa. Conceptualmente, esta idea ya aparece en C con punteros a función, pero en lenguajes modernos se expresa de forma más segura y legible mediante funciones lambda.

En **JavaScript**, las funciones son ciudadanos de primera clase, por lo que pueden pasarse directamente como argumentos a otros métodos. El método `transformar` puede recibir un `String` y una función transformadora, e invocarla internamente sin restricciones adicionales. Este estilo es natural en JavaScript y encaja bien con el enfoque funcional del lenguaje.

En **Java**, esta idea se implementa mediante **interfaces funcionales**. Un método puede recibir como parámetro una referencia a una función lambda cuyo tipo sea, por ejemplo, `Function<String, String>`. De este modo, el compilador garantiza que la función recibida acepta un `String` y devuelve un `String`, manteniendo la seguridad de tipos. El método `transformar` se limita a invocar la función recibida, sin conocer su implementación concreta.

Este patrón demuestra cómo el enfoque funcional permite escribir código más genérico y desacoplado, donde el flujo del programa no depende de condicionales ni de herencia, sino de funciones intercambiables que encapsulan el comportamiento.

***

## ✅ **Ejemplo en JavaScript**

```javascript
// Función lambda transformadora
let aMayusculas = texto => texto.toUpperCase();

// Método que recibe una función como parámetro
function transformar(texto, transformador) {
    return transformador(texto);
}

// Uso
console.log(transformar("aspectos funcionales", aMayusculas));
```

***

## ✅ **Ejemplo en Java**

```java
import java.util.function.Function;

public class EjemploFuncionesComoParametro {

    // Método que recibe una función lambda
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {

        // Función lambda transformadora
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // Uso del método
        String resultado = transformar("aspectos funcionales", aMayusculas);
        System.out.println(resultado);
    }
}
```

***

✅ **Idea clave**  
Pasar funciones como parámetros permite abstraer el comportamiento y reutilizar el mismo método con distintas transformaciones. JavaScript lo hace de forma nativa, mientras que Java lo integra mediante interfaces funcionales y lambdas, ofreciendo una versión tipada y segura del mismo concepto.



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### 

Definir una **función lambda directamente en la llamada** a un método permite expresar el comportamiento de forma **local y puntual**, sin necesidad de declarar una variable previa ni una función con nombre. Este estilo es característico del paradigma funcional y resulta especialmente útil cuando la transformación se usa una sola vez. El código gana concisión y deja claro que la función es un detalle del uso concreto, no una parte reutilizable del diseño.

En **JavaScript**, este enfoque es natural porque las funciones son ciudadanos de primera clase. Una lambda puede escribirse inline al pasarla como argumento, y el método receptor simplemente la invoca. El lector del código ve inmediatamente qué transformación se aplica, reduciendo el contexto necesario para entender el flujo.

En **Java**, el mismo patrón se logra pasando una **expresión lambda** que implementa una **interfaz funcional** (por ejemplo, `Function<String, String>`). Aunque Java es un lenguaje tipado estáticamente, el compilador infiere los tipos de la lambda y garantiza que la firma sea compatible. De este modo, se mantiene la seguridad de tipos y se obtiene una sintaxis compacta comparable a la de lenguajes funcionales.

Este uso de lambdas inline refuerza la idea de **pasar comportamiento como dato**, permitiendo escribir métodos genéricos y reutilizables que delegan la lógica específica en funciones proporcionadas por el llamador.

***

## ✅ **Ejemplo en JavaScript (lambda inline)**

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

// Invocación con lambda que invierte la cadena
console.log(
    transformar("aspectos funcionales", s => s.split("").reverse().join(""))
);
```

***

## ✅ **Ejemplo en Java (lambda inline)**

```java
import java.util.function.Function;

public class EjemploInlineLambda {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Invocación con lambda definida en la llamada
        String resultado = transformar(
            "aspectos funcionales",
            s -> new StringBuilder(s).reverse().toString()
        );

        System.out.println(resultado);
    }
}
```

***

✅ **Idea clave**  
Definir la lambda directamente en la llamada hace el código más expresivo y localiza el comportamiento donde se usa, manteniendo flexibilidad y seguridad de tipos tanto en JavaScript como en Java.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### 

Un **cierre** o **closure** es una función que, además de su propio código, **captura y recuerda variables del contexto donde fue definida**, incluso cuando se ejecuta fuera de ese contexto. Es decir, la función no solo depende de sus parámetros, sino también de variables externas que estaban disponibles en el momento de su creación. Este concepto es fundamental en el paradigma funcional y permite escribir funciones más expresivas y adaptables.

En Java, las funciones lambda **sí son closures**, ya que pueden acceder a variables locales del método donde se definen. Sin embargo, existe una restricción importante: dichas variables deben ser **finales o efectivamente finales**, lo que significa que no pueden modificarse después de su inicialización. Esta restricción garantiza la seguridad del modelo de ejecución y evita problemas de concurrencia o inconsistencias de estado.

El uso de closures permite personalizar el comportamiento de una función sin necesidad de crear nuevas clases ni de pasar explícitamente todos los valores como parámetros. En el ejemplo solicitado, se define una función lambda que transforma una cadena concatenándole otra cadena definida fuera de la lambda. Aunque esa variable no forma parte de los parámetros, la lambda puede acceder a ella gracias al mecanismo de cierre.

Este comportamiento muestra cómo Java integra ideas del paradigma funcional de forma controlada dentro de un lenguaje orientado a objetos. Las lambdas no solo representan código ejecutable, sino que también encapsulan parte del contexto, lo que permite construir comportamientos más ricos y reutilizables.

***

## ✅ **Ejemplo en Java: closure con lambda**

```java
import java.util.function.Function;

public class EjemploClosure {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {

        // Variable local capturada por la lambda (efectivamente final)
        String sufijo = " <-- procesado";

        // Lambda que usa una variable externa (closure)
        Function<String, String> concatenar = 
            s -> s + sufijo;

        String resultado = transformar("Aspectos funcionales", concatenar);
        System.out.println(resultado);

        // También puede definirse directamente en la llamada
        String resultado2 = transformar(
            "Java",
            s -> s + " es funcional"
        );
        System.out.println(resultado2);
    }
}
```

***

✅ **Idea clave**  
Una lambda en Java puede acceder a variables locales del contexto donde se define, formando un **closure**, siempre que dichas variables no se modifiquen posteriormente. Esto permite combinar datos y comportamiento de forma segura y expresiva dentro del estilo funcional.



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### 

La principal diferencia entre una **función lambda** y un **puntero a función en C** es que una lambda no solo representa código ejecutable, sino que también puede **capturar y mantener acceso a variables del contexto donde fue definida**, formando un **closure**. Un puntero a función en C se limita a almacenar la dirección de una función, sin ningún tipo de información adicional sobre el entorno en el que fue creada. Por tanto, el comportamiento de una función en C depende exclusivamente de sus parámetros explícitos.

Otra diferencia importante es la **seguridad de tipos y el modelo de abstracción**. En C, los punteros a función requieren declarar explícitamente la firma completa y el compilador no ofrece mecanismos avanzados para garantizar un uso seguro más allá de esa firma. En Java, las funciones lambda están integradas en el sistema de tipos mediante **interfaces funcionales**, lo que permite un chequeo más estricto y una mejor integración con el resto del lenguaje. La lambda no existe de forma aislada, sino siempre asociada a un contrato bien definido.

Además, las funciones lambda en Java pueden acceder a **variables locales efectivamente finales**, mientras que en C cualquier dato adicional debe pasarse explícitamente como argumento o gestionarse mediante variables globales o estructuras auxiliares. Esto hace que el código funcional en Java sea más expresivo y menos propenso a errores derivados de estados globales compartidos.

En resumen, aunque los punteros a función en C y las funciones lambda persiguen un objetivo similar —tratar funciones como valores—, las lambdas ofrecen un modelo más potente y seguro. Incorporan el contexto mediante closures, están respaldadas por el sistema de tipos del lenguaje y permiten un estilo de programación más declarativo, mientras que los punteros a función en C representan una solución más básica y manual.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

###

Devolver **funciones como resultado** es una consecuencia directa de tratar las funciones como ciudadanos de primera clase. En el paradigma funcional, una función no solo puede recibirse como parámetro, sino también **crearse dinámicamente y devolverse** como resultado de otra función. Esto permite construir comportamientos personalizados a partir de datos, en este caso, crear distintas funciones de descuento a partir de un porcentaje dado.

La función `crearDescuento(porcentaje)` devuelve una función lambda que aplica ese descuento a una cantidad. La clave del diseño es que la función devuelta **recuerda el valor del porcentaje** con el que fue creada, aunque ese valor ya no esté en el ámbito de ejecución cuando la función se use más tarde. Este comportamiento es posible gracias al mecanismo de **closure**, que encapsula tanto el código de la función como el contexto donde se definió.

En Java, este patrón se implementa mediante **interfaces funcionales**, como `Function<Double, Double>`. La lambda devuelta captura la variable local `porcentaje`, que debe ser final o efectivamente final. Cada vez que se llama a `crearDescuento`, se genera una nueva función con su propio porcentaje asociado, permitiendo crear descuentos distintos sin duplicar código ni usar herencia.

Este ejemplo muestra claramente cómo los closures permiten construir funciones especializadas a partir de parámetros, separando la lógica general (crear descuentos) de la aplicación concreta (usar un descuento específico). Se trata de una de las aplicaciones más representativas del estilo funcional en Java moderno.

***

## ✅ **Ejemplo en Java: devolver funciones con closure**

```java
import java.util.function.Function;

public class Descuentos {

    // Función que crea funciones de descuento
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {

        // Crear dos funciones de descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        double precio = 100.0;

        System.out.println("Precio original: " + precio);
        System.out.println("Con 10% descuento: " + descuento10.apply(precio));
        System.out.println("Con 25% descuento: " + descuento25.apply(precio));
    }
}
```

***

✅ **Explicación del closure**  
La lambda devuelta por `crearDescuento` **captura la variable `porcentaje`** del contexto donde fue creada. Cada función conserva su propio valor de porcentaje incluso después de que `crearDescuento` haya terminado de ejecutarse. Este encapsulamiento de datos y comportamiento es precisamente lo que define un **closure** en programación funcional.



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### 

En Java, una **interfaz funcional** es un tipo especial de interfaz que representa el **tipo de una función lambda**. Dado que Java es un lenguaje con comprobación estática de tipos, toda expresión lambda debe tener un tipo bien definido en tiempo de compilación. Ese tipo no es la lambda en sí, sino la **interfaz funcional** a la que se asigna. Conceptualmente, una interfaz funcional define *qué firma debe tener la función*, es decir, qué parámetros recibe y qué devuelve.

El requisito fundamental de una interfaz funcional es que contenga **exactamente un único método abstracto**. Este método abstracto es el que la función lambda implementa. Puede contener, además, métodos `default` o `static`, pero estos no cuentan como métodos abstractos a efectos de la definición. Si una interfaz tuviera más de un método abstracto, el compilador no podría saber a cuál corresponde la lambda, y por tanto no sería válida como tipo funcional.

Las interfaces funcionales permiten integrar el paradigma funcional dentro del modelo orientado a objetos de Java. En lugar de usar punteros a función, Java utiliza interfaces como `Function`, `Predicate`, `Consumer` o `Supplier`, que definen distintos tipos de funciones comunes. Gracias a ellas, el compilador puede verificar que una lambda es compatible con el contexto donde se usa, garantizando seguridad de tipos y evitando errores en tiempo de ejecución.

De forma opcional, Java ofrece la anotación `@FunctionalInterface`, que permite declarar explícitamente la intención de que una interfaz sea funcional. Esta anotación no es obligatoria, pero es recomendable porque el compilador comprobará que la interfaz cumple el requisito de tener un único método abstracto. Así, una interfaz funcional actúa como el **puente tipado** entre las funciones lambda y el resto del lenguaje Java.

***

✅ **Idea clave**  
Una interfaz funcional es el **tipo** de una función lambda en Java, definida por un único método abstracto, y es el mecanismo que permite usar funciones de forma segura en un lenguaje con tipado estático.



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### 

Crear una **interfaz funcional a mano** consiste en definir explícitamente el **tipo de una función lambda**, en lugar de usar una de las interfaces funcionales estándar que proporciona Java. Dado que Java es un lenguaje con tipado estático, toda función lambda debe asociarse a una interfaz que describa su firma. Definir una interfaz funcional propia permite expresar con claridad la intención del código y usar nombres de dominio más significativos que los genéricos como `Function`.

Una interfaz funcional debe cumplir un requisito fundamental: **contener exactamente un método abstracto**. Ese método define la firma que deberán respetar todas las funciones lambda que se asignen a dicha interfaz. Puede contener métodos `default` o `static`, pero estos no cuentan como métodos abstractos. Para reforzar esta condición y evitar errores accidentales, es recomendable usar la anotación `@FunctionalInterface`.

En este caso, la interfaz `Transformador` representa el concepto de una función que **recibe una cadena de texto y devuelve otra cadena de texto**. Este tipo funcional encaja perfectamente con los ejemplos anteriores de transformación de cadenas (mayúsculas, inversión, concatenación, etc.), y permite expresar el comportamiento de forma clara y tipada, sin necesidad de conversiones ni ambigüedades.

Definir interfaces funcionales propias es una práctica habitual cuando se quiere construir una API expresiva y alineada con el dominio del problema, integrando el estilo funcional dentro del modelo orientado a objetos de Java.

***

## ✅ **Interfaz funcional `Transformador`**

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

***

## ✅ **Ejemplo de uso con una función lambda**

```java
public class EjemploTransformador {

    public static String transformar(String texto, Transformador t) {
        return t.transformar(texto);
    }

    public static void main(String[] args) {

        Transformador aMayusculas = s -> s.toUpperCase();
        Transformador invertir = s -> new StringBuilder(s).reverse().toString();

        System.out.println(transformar("Aspectos funcionales", aMayusculas));
        System.out.println(transformar("Java", invertir));
    }
}
```

***

✅ **Idea clave**  
Una interfaz funcional define el **tipo de una función lambda**. Crear una propia, como `Transformador`, permite expresar con claridad el comportamiento esperado, manteniendo seguridad de tipos y un diseño más legible y alineado con el dominio.



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### 

Hacer una **interfaz funcional más genérica mediante generics** permite desacoplar completamente el tipo de entrada del tipo de salida de la función. En lugar de limitarse a transformar siempre un `String` en otro `String`, se puede definir un transformador que convierta **un tipo cualquiera en otro tipo cualquiera**, manteniendo el chequeo de tipos en tiempo de compilación. Esto generaliza el concepto de transformación y lo hace reutilizable en muchos contextos distintos.

Mediante el uso de **parámetros de tipo**, la interfaz funcional expresa explícitamente qué tipo recibe y qué tipo devuelve la función. El compilador garantiza que cualquier lambda asignada a dicha interfaz respete esa firma, evitando conversiones explícitas y errores en tiempo de ejecución. Esta técnica combina programación genérica y funcional, dos de los mecanismos más potentes del Java moderno.

Un ejemplo típico consiste en transformar un `Double` en un `Integer`, por ejemplo redondeando un número real. Gracias a la interfaz funcional genérica, el tipo de entrada y el tipo de salida quedan perfectamente definidos. El código que utiliza el transformador conoce exactamente qué tipo recibe y qué tipo obtiene, sin necesidad de *downcasting* ni comprobaciones adicionales.

Este enfoque demuestra cómo las interfaces funcionales genéricas permiten modelar transformaciones tipadas de forma clara y segura, reforzando el sistema de tipos y facilitando la reutilización del código en distintos escenarios.

***

## ✅ **Interfaz funcional genérica `Transformador`**

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

***

## ✅ **Ejemplo: transformar `Double` en `Integer`**

```java
public class EjemploTransformadorGenerico {

    public static void main(String[] args) {

        // Lambda que redondea un Double y devuelve un Integer
        Transformador<Double, Integer> redondear =
            d -> (int) Math.round(d);

        Double valor = 3.7;
        Integer resultado = redondear.transformar(valor);

        System.out.println("Valor original: " + valor);
        System.out.println("Valor redondeado: " + resultado);
    }
}
```

***

✅ **Idea clave**  
Una interfaz funcional genérica permite definir transformaciones **entre tipos distintos** con seguridad de tipos en compilación. En este ejemplo, el compilador garantiza que el transformador recibe un `Double` y devuelve un `Integer`, integrando de forma natural programación funcional y genericidad en Java.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### 

La interfaz funcional genérica `Transformador<T, R>` es conceptualmente equivalente a una interfaz funcional que **ya existe en la API estándar de Java**, llamada `Function<T, R>`. Esto no es casual: Java proporciona un conjunto de **interfaces funcionales predefinidas** pensadas para cubrir los casos más habituales de uso de funciones, evitando que el programador tenga que definirlas manualmente una y otra vez. Estas interfaces forman parte del paquete `java.util.function`.

Las interfaces funcionales predefinidas se clasifican según **qué reciben y qué devuelven**. Algunas representan funciones que transforman un valor en otro (`Function`), otras que reciben un valor y no devuelven nada (`Consumer`), otras que devuelven un valor sin recibir parámetros (`Supplier`), y otras que devuelven un valor booleano (`Predicate`). Esta clasificación cubre la mayoría de los patrones funcionales comunes en programación diaria.

Además, existen versiones especializadas para **tipos primitivos** (`int`, `double`, `long`), como `IntFunction`, `DoubleConsumer` o `IntPredicate`, que evitan el *boxing* y mejoran el rendimiento. Todas estas interfaces siguen el mismo principio: definir un único método abstracto que describe la firma de la función, permitiendo que se implementen fácilmente mediante expresiones lambda.

En resumen, Java proporciona un **ecosistema completo de interfaces funcionales estándar** que permiten escribir código funcional sin necesidad de crear interfaces propias en la mayoría de los casos. Definir una interfaz funcional personalizada sigue siendo útil cuando se quiere expresar un concepto de dominio concreto, pero para transformaciones generales, las interfaces estándar son la opción recomendada.

***

## ✅ **Principales interfaces funcionales predefinidas en Java**

```java
// Transforma un valor en otro
Function<T, R>

// Consume un valor y no devuelve nada
Consumer<T>

// Consume dos valores
BiConsumer<T, U>

// Proporciona un valor sin recibir parámetros
Supplier<T>

// Evalúa una condición
Predicate<T>

// Evalúa una condición con dos parámetros
BiPredicate<T, U>
```

***

## ✅ **Versiones especializadas para tipos primitivos**

```java
IntFunction<R>
DoubleFunction<R>

IntConsumer
DoubleConsumer

IntPredicate
DoublePredicate

IntSupplier
DoubleSupplier
```

***

✅ **Idea clave**  
`Function<T, R>` y el resto de interfaces funcionales de `java.util.function` constituyen el **soporte estándar del paradigma funcional en Java**, evitando la redefinición de tipos funcionales genéricos y proporcionando una base segura, expresiva y reutilizable para trabajar con funciones lambda.



## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### 

El método **`forEach`** de `List` representa una forma funcional de recorrer colecciones en Java. En lugar de describir explícitamente un bucle con índices o variables de control, se indica **qué operación debe aplicarse a cada elemento** de la lista. Este enfoque es coherente con el paradigma funcional, donde se prioriza la expresión del comportamiento frente al control detallado del flujo de ejecución.

Desde el punto de vista conceptual, `forEach` recibe una **función** que se ejecuta una vez por cada elemento de la colección. Esa función suele expresarse mediante una **lambda**, que define qué hacer con cada elemento. El recorrido de la colección queda encapsulado dentro de la propia estructura de datos, lo que reduce código repetitivo y mejora la legibilidad. Para alguien con experiencia previa en C, puede verse como una generalización del bucle `for`, donde el cuerpo del bucle se pasa como parámetro.

El uso de `forEach` no cambia lo que el programa hace, sino **cómo se expresa**. El código deja de centrarse en “cómo iterar” y pasa a centrarse en “qué hacer con cada elemento”. Este estilo resulta especialmente adecuado cuando se trabaja con colecciones y transformaciones simples, y sirve como puerta de entrada al uso de lambdas y programación funcional en Java.

En el ejemplo siguiente, se recorre una lista de enteros y se muestra un mensaje únicamente cuando el número es positivo, utilizando `forEach` como sustituto funcional del bucle tradicional.

***

## ✅ **Ejemplo en Java usando `List.forEach`**

```java
import java.util.List;
import java.util.Arrays;

public class EjemploForEach {

    public static void main(String[] args) {

        List<Integer> numeros = Arrays.asList(-3, 0, 5, -1, 8, 2);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}
```

***

✅ **Idea clave**  
`List.forEach` permite recorrer colecciones pasando el comportamiento como una función, ofreciendo una alternativa funcional al bucle `for` tradicional que mejora la expresividad del código y encaja de forma natural con el uso de lambdas en Java.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### 

La firma de `List.forEach` utiliza **`Consumer<? super T>`** y no simplemente `Consumer<T>` para **permitir mayor flexibilidad manteniendo la seguridad de tipos**. El método `forEach` aplica una operación a cada elemento de la lista, es decir, la función **consume** elementos de tipo `T`. Si se exigiera exactamente `Consumer<T>`, se limitaría innecesariamente el uso, impidiendo pasar consumidores que acepten un supertipo de `T` (por ejemplo, un `Consumer<Object>` para una `List<String>`), aunque conceptualmente eso sea correcto.

Este criterio se resume en la regla **PECS**, que significa **Producer Extends, Consumer Super**. Según esta regla, cuando una estructura **produce** valores de tipo `T` (se leen elementos), debe usarse `? extends T`; cuando una estructura **consume** valores de tipo `T` (se escriben o procesan), debe usarse `? super T`. En el caso de `forEach`, la lista produce elementos `T`, pero la función **consume** esos elementos, por lo que el tipo correcto para la función es `Consumer<? super T>`.

Aplicando esta idea al método `transformar`, se puede mejorar su definición siguiendo PECS. Si el método recibe una función transformadora que **consume** un `String` y devuelve otro valor, conviene permitir que dicha función acepte no solo `String`, sino también cualquier supertipo de `String` (como `Object`). Esto se logra usando `? super String` en el parámetro de entrada de la función. De este modo, el método resulta más general sin perder seguridad de tipos.

En resumen, `Consumer<? super T>` se utiliza para permitir **contravarianza controlada** en funciones consumidoras, y PECS proporciona una guía práctica para decidir cuándo usar `extends` o `super`. Esta regla es clave para diseñar APIs genéricas flexibles y seguras, como ocurre en `forEach` y en métodos funcionales similares.

***

## ✅ **Firma simplificada de `forEach`**

```java
void forEach(Consumer<? super T> action)
```

***

## ✅ **Mejora del método `transformar` aplicando PECS**

```java
import java.util.function.Function;

public static <R> R transformar(
        String texto,
        Function<? super String, ? extends R> transformador) {

    return transformador.apply(texto);
}
```

✅ Ventajas:

*   La función puede **consumir `String` o cualquier supertipo**.
*   El resultado puede ser **`R` o cualquier subtipo de `R`**.
*   Se gana flexibilidad sin perder chequeo de tipos.

***

✅ **Idea clave final**  
PECS es una regla práctica para decidir cómo usar `extends` y `super` en genéricos:

*   **Producer → `extends`**
*   **Consumer → `super`**

`forEach` y `transformar` son ejemplos claros de cómo esta regla permite diseñar APIs más expresivas y seguras.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### 

Las **referencias a métodos** permiten obtener una referencia directa a un método existente, ya sea de un objeto concreto o de una clase, y tratarla como una función que puede almacenarse en una variable o pasarse como parámetro. Conceptualmente, es una forma más explícita y legible de reutilizar un método ya definido, sin necesidad de escribir una nueva función lambda que simplemente delegue en ese método. Este mecanismo encaja de forma natural con el paradigma funcional, donde el comportamiento puede manejarse como un valor.

En **JavaScript**, las funciones son ciudadanos de primera clase y los métodos de los objetos son, en esencia, funciones asociadas a un contexto (`this`). Al obtener una referencia a un método, es importante tener en cuenta dicho contexto, ya que la llamada debe mantener la asociación con el objeto original. Aun así, es posible guardar una referencia al método y ejecutarlo posteriormente como si se tratara de una función independiente.

En **Java**, las referencias a métodos se introducen como una sintaxis especial relacionada con las funciones lambda. En lugar de escribir una lambda que invoque un método, se puede usar la notación `objeto::metodo` para obtener una referencia directa. Esta referencia se asigna a una **interfaz funcional** compatible, y el compilador se encarga de enlazar correctamente la llamada. Esto mejora la legibilidad y refuerza la idea de reutilización de comportamiento existente.

En ambos lenguajes, las referencias a métodos permiten separar la definición del comportamiento de su ejecución, facilitando un estilo más declarativo y funcional, especialmente cuando se combinan con funciones como parámetros o estructuras de control de alto nivel.

***

## ✅ **Ejemplo en JavaScript**

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

// Crear objeto
const p = new Persona("Ana");

// Obtener referencia al método (asegurando el contexto)
const saludarRef = p.saludar.bind(p);

// Invocar el método mediante la referencia
saludarRef();
```

***

## ✅ **Ejemplo en Java**

```java
import java.util.function.Consumer;

public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona p = new Persona("Ana");

        // Referencia al método de instancia
        Consumer<Void> saludarRef = v -> p.saludar();
        saludarRef.accept(null);

        // Alternativa más directa usando referencia a método
        Runnable saludarRef2 = p::saludar;
        saludarRef2.run();
    }
}
```

***

✅ **Idea clave**  
Una referencia a método permite reutilizar directamente un método existente como función. En JavaScript se gestiona como una función con contexto, y en Java se integra mediante referencias a métodos y interfaces funcionales, ofreciendo una forma más clara y expresiva de trabajar con comportamiento reutilizable.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### 

En Java existen **cuatro tipos principales de referencias a métodos**, que permiten reutilizar métodos existentes como si fueran funciones, integrándose con interfaces funcionales. Estas referencias son una forma más concisa y expresiva de escribir lambdas que simplemente delegan en un método ya definido. Todas ellas usan el operador `::` y se diferencian según **qué método se referencia y en qué contexto se aplica**.

El primer tipo es la **referencia a un método estático**, que apunta a un método de clase sin necesidad de una instancia. El segundo es la **referencia a un método de instancia de una instancia concreta**, donde el método se invoca siempre sobre un objeto específico. Ambos casos son directos y suelen usarse cuando ya existe el método y se quiere reutilizar sin crear una lambda explícita.

El tercer tipo es la **referencia a un método de instancia sobre cualquier instancia de una clase**, donde el objeto sobre el que se ejecuta el método se convierte implícitamente en el primer parámetro de la función. Este tipo es muy común al trabajar con colecciones y operaciones funcionales. Por último, existe la **referencia a un constructor**, que permite tratar la creación de objetos como una función, algo especialmente útil en combinación con genéricos y streams.

En conjunto, estas referencias permiten escribir código funcional más legible, evitando lambdas innecesarias y reforzando la reutilización de comportamiento ya existente dentro del modelo tipado de Java.

***

## ✅ **Ejemplos de los tipos de referencias a método**

### 1️⃣ Referencia a **método estático**

```java
import java.util.function.Function;

public class Util {
    public static String aMayusculas(String s) {
        return s.toUpperCase();
    }

    public static void main(String[] args) {
        Function<String, String> f = Util::aMayusculas;
        System.out.println(f.apply("java"));
    }
}
```

***

### 2️⃣ Referencia a **método de instancia de una instancia concreta**

```java
import java.util.function.Supplier;

public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }

    public static void main(String[] args) {
        Persona p = new Persona("Ana");

        Supplier<String> saludo = p::saludar;
        System.out.println(saludo.get());
    }
}
```

***

### 3️⃣ Referencia a **método de instancia sobre cualquier instancia**

```java
import java.util.function.Function;

public class EjemploCualquiera {
    public static void main(String[] args) {

        Function<String, Integer> longitud = String::length;
        System.out.println(longitud.apply("funcional"));
    }
}
```

✅ Aquí, el objeto `String` sobre el que se llama `length()` se pasa implícitamente.

***

### 4️⃣ Referencia a **constructor**

```java
import java.util.function.Function;

public class EjemploConstructor {

    public static void main(String[] args) {

        Function<String, Persona> crearPersona = Persona::new;
        Persona p = crearPersona.apply("Carlos");

        System.out.println(p.saludar());
    }
}
```

***

✅ **Resumen final**

| Tipo de referencia           | Ejemplo                  |
| ---------------------------- | ------------------------ |
| Método estático              | `Clase::metodoEstatico`  |
| Método de instancia concreta | `objeto::metodo`         |
| Método de instancia genérico | `Clase::metodoInstancia` |
| Constructor                  | `Clase::new`             |

Estas cuatro formas cubren los casos habituales de uso de referencias a método en Java y constituyen una pieza clave del estilo funcional integrado en el lenguaje.



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### 

El uso de **expresiones lambda** para ordenar colecciones en Java es uno de los ejemplos más claros de integración del paradigma funcional en un lenguaje orientado a objetos. Tradicionalmente, ordenar una lista requería implementar explícitamente la interfaz `Comparator` en una clase separada o mediante una clase anónima. Con lambdas, la lógica de comparación puede expresarse **directamente en el punto de uso**, haciendo el código más conciso y legible.

En una primera versión, la función de comparación puede implementarse **manualmente**, definiendo paso a paso cómo comparar dos objetos `Persona`: primero por edad y, si las edades coinciden, por orden alfabético del nombre. Esta versión es explícita y fácil de entender para quien viene de estilos imperativos, ya que reproduce de forma directa la lógica de comparación tradicional.

Java también ofrece utilidades en la clase **`Comparator`** que permiten construir comparadores de forma más declarativa. Métodos como `comparingInt` y `thenComparing` encapsulan patrones comunes de comparación y reducen el código repetitivo. Esta segunda versión expresa **qué criterio se usa para ordenar**, más que cómo se realiza la comparación, lo que encaja mejor con el estilo funcional.

Ambas soluciones producen el mismo resultado, pero la versión basada en `Comparator` es más reutilizable, menos propensa a errores y más expresiva. Este ejemplo muestra cómo las lambdas permiten evolucionar desde un estilo imperativo clásico hacia un estilo más declarativo y funcional sin perder claridad.

***

## ✅ **Clase `Persona`**

```java
public class Persona {
    private final String nombre;
    private final int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}
```

***

## ✅ **Versión 1: comparación manual con lambda**

```java
import java.util.*;

public class EjemploOrdenacionManual {
    public static void main(String[] args) {

        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Carlos", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(personas, (p1, p2) -> {
            int cmpEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (cmpEdad != 0) {
                return cmpEdad;
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });

        personas.forEach(System.out::println);
    }
}
```

***

## ✅ **Versión 2: usando `Comparator`**

```java
import java.util.*;

public class EjemploOrdenacionComparator {
    public static void main(String[] args) {

        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Carlos", 30),
            new Persona("Bea", 25)
        );

        Collections.sort(
            personas,
            Comparator
                .comparingInt(Persona::getEdad)
                .thenComparing(Persona::getNombre)
        );

        personas.forEach(System.out::println);
    }
}
```

***

✅ **Idea clave**  
Las expresiones lambda permiten definir comparaciones de forma directa y expresiva. La versión manual muestra el control explícito del algoritmo, mientras que `Comparator` permite una formulación declarativa más alineada con el estilo funcional moderno de Java.

