<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### 
Las cuatro características básicas de la programación orientada a objetos son **abstracción, encapsulación, herencia y polimorfismo**. Estas características permiten organizar el software de una forma más cercana a cómo se modelan los problemas del mundo real, facilitando el mantenimiento, la reutilización y la extensibilidad del código.

La **abstracción** consiste en identificar las características y comportamientos relevantes de una entidad, ignorando los detalles innecesarios. En programación orientada a objetos, esto se logra definiendo clases que representan conceptos concretos del problema, centrándose en *qué* hace un objeto y no en *cómo* lo hace internamente. Este enfoque ayuda a reducir la complejidad y a trabajar con modelos más claros.

La **encapsulación** permite ocultar los detalles internos de una clase y controlar el acceso a sus datos y métodos. En lugar de manipular directamente las variables, se interactúa con el objeto a través de métodos bien definidos. Esto protege la integridad de los datos y evita dependencias innecesarias entre distintas partes del programa.

La **herencia** posibilita crear nuevas clases a partir de otras existentes, reutilizando código y estableciendo relaciones jerárquicas. El **polimorfismo**, por su parte, permite que diferentes clases respondan de manera distinta a una misma operación, siempre que compartan una interfaz común. Juntas, estas dos características facilitan la extensión del sistema sin necesidad de modificar el código ya existente.




## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### 
Java, C++, Python y C#


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### 
La **programación estructurada** es un paradigma que propone organizar los programas como una secuencia clara de instrucciones, utilizando estructuras de control bien definidas como secuencias, condicionales y bucles. Su objetivo principal es mejorar la legibilidad y comprensión del código, evitando construcciones desordenadas como los saltos incontrolados. Este enfoque es característico de lenguajes como C, donde el programa se divide en funciones que se ejecutan de forma ordenada.

En este paradigma, los datos y las funciones suelen estar separados conceptualmente: las funciones operan sobre datos que, en muchos casos, son globales o se pasan como parámetros. Aunque esto permite crear programas eficientes y relativamente sencillos, a medida que el software crece resulta más difícil controlar qué partes del código modifican los datos, lo que puede provocar errores y dificultar el mantenimiento.

La **programación modular** surge como una evolución de la programación estructurada y propone dividir el programa en módulos independientes, cada uno con una responsabilidad concreta. Un módulo agrupa funciones relacionadas y, en ocasiones, los datos sobre los que operan, ofreciendo una interfaz clara al resto del programa. Esto facilita el desarrollo en equipo, la reutilización de código y la localización de errores.

A diferencia de la programación estructurada pura, la programación modular pone mayor énfasis en el aislamiento de responsabilidades y en la reducción de dependencias entre partes del sistema. Sin embargo, aunque mejora la organización del código, todavía no integra completamente datos y comportamiento en una única entidad, algo que sí se logra plenamente con la programación orientada a objetos.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### 
En programación orientada a objetos, un objeto se define a partir de **tres elementos fundamentales: identidad, estado y comportamiento**. Estos elementos permiten diferenciar a un objeto de otro y describir tanto la información que contiene como las acciones que puede realizar dentro de un programa.

La **identidad** es lo que distingue a un objeto de cualquier otro, incluso aunque tengan el mismo estado y comportamiento. Cada objeto es único y puede ser referenciado de forma independiente en memoria. Este concepto es importante porque permite tratar a cada objeto como una entidad concreta dentro del sistema, no solo como un conjunto de datos.

El **estado** de un objeto está formado por los valores de sus atributos en un momento dado. Estos atributos representan la información interna del objeto y pueden cambiar a lo largo del tiempo. El **comportamiento**, por su parte, se define mediante los métodos que el objeto ofrece, es decir, las operaciones que puede realizar y que pueden modificar su estado o interactuar con otros objetos.



## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### 
Una **clase** es una plantilla o modelo que define las características y comportamientos que tendrán los objetos de un determinado tipo. En una clase se especifican los atributos que describen el estado y los métodos que definen el comportamiento. Puede entenderse como una definición abstracta que no representa por sí misma una entidad concreta, sino la forma que tendrán los objetos que se creen a partir de ella.

Una clase **no es lo mismo que un objeto**. El objeto es una entidad concreta que existe en memoria y que se crea siguiendo la definición de una clase. Mientras la clase describe *qué* puede tener y hacer algo, el objeto representa *un caso particular* con valores específicos en sus atributos. Por tanto, la clase existe como definición, y los objetos como realizaciones de esa definición durante la ejecución del programa.

Una **instancia** es precisamente ese proceso y resultado de crear un objeto a partir de una clase. Al instanciar una clase, se reserva memoria y se inicializan sus atributos, dando lugar a un objeto concreto e independiente de otros objetos de la misma clase. Cada instancia mantiene su propio estado, aunque comparta estructura y comportamiento con las demás.

No todos los lenguajes orientados a objetos manejan obligatoriamente el concepto de clase. Algunos lenguajes, como los basados en prototipos, utilizan otros mecanismos para crear objetos sin definir clases de forma explícita. Aun así, el concepto de clase es el más común y el más utilizado en la mayoría de los lenguajes orientados a objetos tradicionales.




## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### 
En programación orientada a objetos, los **objetos se almacenan generalmente en la memoria dinámica**, también conocida como *heap*. Esta zona de memoria permite que los objetos existan más allá del ámbito de una función y que puedan ser compartidos mediante referencias. Las variables que se utilizan para acceder a los objetos suelen almacenarse en la pila, pero contienen únicamente una referencia al objeto, no el objeto en sí.

El modo en que se gestionan estos objetos **no es igual en todos los lenguajes**. En lenguajes como C++, el programador puede decidir si un objeto se almacena en la pila o en el heap y es responsable de liberar la memoria cuando deja de ser necesaria. En otros lenguajes, como Java, todos los objetos se crean en el heap y la gestión de la memoria se realiza de forma automática, sin intervención directa del programador.

La **recolección de basura** (*garbage collection*) es un mecanismo automático de gestión de memoria que se encarga de liberar los objetos que ya no son accesibles desde el programa. Es decir, cuando un objeto deja de tener referencias que lo apunten, el sistema lo considera inútil y puede liberar la memoria que ocupa. Esto evita errores comunes como fugas de memoria o accesos a memoria liberada.

Este sistema simplifica el desarrollo de software, ya que el programador no necesita preocuparse por destruir explícitamente los objetos. Sin embargo, la recolección de basura también implica un coste en tiempo de ejecución y un menor control directo sobre el uso de la memoria, lo que supone un compromiso entre comodidad y eficiencia.




## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### 
Un **método** es una función que forma parte de una clase y que define el comportamiento de los objetos creados a partir de ella. Los métodos permiten realizar operaciones sobre el estado interno del objeto y actuar como la única forma controlada de interactuar con sus datos. A diferencia de las funciones tradicionales, un método está asociado a una clase y se invoca normalmente sobre un objeto concreto.

Los métodos pueden acceder a los atributos del objeto al que pertenecen y modificar su estado. Esto refuerza el principio de encapsulación, ya que el código externo no necesita conocer cómo se almacenan los datos internamente, sino únicamente qué operaciones están disponibles. Desde el punto de vista conceptual, un método representa una acción que un objeto sabe realizar.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con diferentes listas de parámetros. El compilador distingue entre ellos según el número y el tipo de los argumentos utilizados en la llamada. Esto permite utilizar un mismo nombre para operaciones conceptualmente similares, mejorando la legibilidad del código.

Gracias a la sobrecarga, es posible ofrecer distintas formas de realizar una misma acción sin obligar a usar nombres distintos para cada caso. Este mecanismo no depende del valor de retorno del método, sino exclusivamente de la firma, es decir, del nombre y los parámetros definidos.



## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### 
A continuación se muestra un **ejemplo mínimo de clase en Java** que cumple las condiciones indicadas. La clase se llama `Punto`, tiene dos atributos `x` e `y` con **visibilidad por defecto** (sin modificador) y un método que calcula la distancia al origen `(0,0)`. La distancia se obtiene aplicando la fórmula matemática habitual usando el teorema de Pitágoras.

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

En este ejemplo, la clase define únicamente la estructura y el comportamiento común de los puntos. Los atributos `x` e `y` representan el estado del objeto, mientras que el método `calculaDistanciaAOrigen` define una operación que puede realizar cualquier objeto de tipo `Punto`. La clase por sí sola no ejecuta nada, únicamente describe cómo serán los objetos que se creen a partir de ella.

A continuación se muestra un **ejemplo de uso**, donde se crea una instancia de la clase y se invoca el método definido. En Java, la creación del objeto se realiza mediante la palabra clave `new`, y el acceso a los atributos y métodos se hace a través del operador punto.

```java
class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println(distancia);
    }
}
```

En este caso, se crea un objeto concreto `p` que es una instancia de la clase `Punto`. Se asignan valores a sus atributos y se utiliza el método para calcular la distancia al origen. El método opera sobre el estado interno del objeto, lo que ilustra cómo en programación orientada a objetos los datos y las operaciones relacionadas se agrupan en una misma entidad.




## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### 
El **punto de entrada** de un programa en Java es el método `main`. Este método es el primero que se ejecuta cuando se inicia la aplicación y debe tener una firma concreta para que la máquina virtual de Java pueda localizarlo. A través de `main` se inicia la ejecución del programa y, desde él, se crean objetos y se llaman a otros métodos según sea necesario.

La palabra clave **`static`** indica que un atributo o método pertenece a la clase y no a una instancia concreta. Esto significa que puede utilizarse sin crear previamente un objeto de esa clase. En el caso del método `main`, se declara como `static` porque Java necesita poder ejecutarlo sin haber creado todavía ningún objeto, ya que precisamente desde ahí comienza la ejecución del programa.

El modificador `static` **no se emplea únicamente en el método `main`**. Puede aplicarse también a atributos y a otros métodos cuando se desea que sean compartidos por todas las instancias de una clase o que representen un comportamiento general, no asociado a un objeto concreto. Por ejemplo, constantes, contadores globales o funciones auxiliares suelen declararse como `static`.

La combinación de **`static` con `final`** se utiliza habitualmente para definir constantes de clase. `static` permite que el valor sea único y compartido, mientras que `final` impide que dicho valor se modifique una vez inicializado. De este modo, se garantiza que el dato sea accesible sin crear objetos y que permanezca inmutable durante toda la ejecución del programa.



## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### 
Para ejecutar un programa en Java desde la línea de comandos, primero se escribe el código en un archivo con extensión `.java`, por ejemplo `PruebaPunto.java`. Luego se utiliza el comando `javac` para **compilar** el archivo. Este proceso traduce el código fuente a un formato intermedio llamado **byte-code**, que se guarda en un archivo con extensión `.class`. Por ejemplo:

```bash
javac PruebaPunto.java
```

Después de compilar, se puede ejecutar el programa usando el comando `java`, indicando el nombre de la clase que contiene el método `main` (sin la extensión `.class`). Por ejemplo:

```bash
java PruebaPunto
```

Esto iniciará la **Máquina Virtual de Java (JVM)**, que interpreta el byte-code contenido en el archivo `.class` y lo ejecuta en el sistema operativo. La JVM es responsable de traducir las instrucciones intermedias a instrucciones de la máquina física en tiempo de ejecución, gestionando además memoria, objetos y recolección de basura.

Java **es un lenguaje compilado e interpretado al mismo tiempo**. Se compila a byte-code que no depende de una plataforma específica, y la JVM lo interpreta o compila dinámicamente a código nativo según el sistema operativo. El **byte-code** es un conjunto de instrucciones intermedias que la JVM entiende, y los archivos `.class` contienen este byte-code listo para ejecutarse. Gracias a este mecanismo, un mismo archivo `.class` puede ejecutarse en cualquier sistema que tenga instalada la JVM, cumpliendo el principio de “escribir una vez, ejecutar en cualquier lugar”.



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### 
En Java, la palabra clave **`new`** se utiliza para crear una **nueva instancia** de una clase, es decir, para reservar memoria y construir un objeto a partir de su plantilla. Cuando se usa `new`, se llama automáticamente al **constructor** de la clase, que es un método especial encargado de inicializar el objeto recién creado.

Un **constructor** es un método cuyo nombre coincide con el de la clase y que no tiene tipo de retorno. Su función principal es inicializar los atributos del objeto en el momento de su creación, asegurando que el objeto comience con un estado válido. Aunque Java proporciona un constructor por defecto si no se define ninguno, es habitual crear constructores personalizados para asignar valores iniciales a los atributos según sea necesario.

A modo de ejemplo, se puede definir una clase `Empleado` con un constructor que reciba los datos básicos de un trabajador:

```java
class Empleado {
    String DNI;
    String nombre;
    String apellidos;

    // Constructor de la clase
    Empleado(String dni, String nombre, String apellidos) {
        this.DNI = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Con este constructor, al crear un objeto `Empleado` se pueden asignar directamente los valores de sus atributos:

```java
Empleado e = new Empleado("12345678A", "Ana", "García Pérez");
```

Aquí, `new Empleado(...)` crea una instancia y llama al constructor, inicializando `DNI`, `nombre` y `apellidos` del objeto `e`. Esto permite que cada objeto tenga su propio estado desde el momento de su creación, evitando dejar atributos sin definir o con valores incorrectos.





## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### 
En Java, la referencia **`this`** apunta al **objeto actual** sobre el que se está ejecutando un método o constructor. Permite distinguir entre los atributos del objeto y los parámetros locales que puedan tener el mismo nombre, así como pasar el objeto actual a otros métodos o devolverlo. Su uso refuerza la claridad del código y asegura que se está accediendo correctamente a los datos del objeto en curso.

No todos los lenguajes orientados a objetos utilizan exactamente el mismo nombre para esta referencia. Por ejemplo, en C++ también se usa `this`, mientras que en Python se utiliza `self` como primer parámetro explícito de los métodos. Aunque la función es equivalente —referirse al objeto que invoca el método—, el nombre y la forma de usarlo pueden variar según el lenguaje.

Un ejemplo en la clase `Punto` sería usar `this` dentro de un constructor para diferenciar los atributos de los parámetros:

```java
class Punto {
    double x;
    double y;

    // Constructor que usa 'this' para distinguir los atributos
    Punto(double x, double y) {
        this.x = x; // 'this.x' se refiere al atributo, 'x' al parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

// Uso del constructor
Punto p = new Punto(3, 4);
System.out.println(p.calculaDistanciaAOrigen());
```

En este ejemplo, `this.x` y `this.y` hacen explícito que se está asignando el valor recibido en los parámetros a los atributos del objeto actual. Además, dentro de `calculaDistanciaAOrigen`, se podría usar `this.x` y `this.y` aunque en este caso no es estrictamente necesario, ya que no hay ambigüedad. Esto ilustra cómo `this` ayuda a evitar confusiones y a referirse de forma clara al objeto que invoca los métodos o constructores.



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### 
Se puede añadir un nuevo método llamado `distanciaA` en la clase `Punto` que reciba otro objeto de tipo `Punto` como parámetro y calcule la distancia entre el objeto actual (`this`) y el punto proporcionado. Este método aplica nuevamente el teorema de Pitágoras, usando la diferencia de coordenadas entre ambos puntos.

```java
class Punto {
    double x;
    double y;

    // Constructor
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Calcula la distancia al origen
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Calcula la distancia a otro punto
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Un ejemplo de uso de este nuevo método sería:

```java
Punto p1 = new Punto(3, 4);
Punto p2 = new Punto(0, 0);

double distancia = p1.distanciaA(p2);
System.out.println(distancia);  // Muestra 5.0
```

En este ejemplo, `p1.distanciaA(p2)` calcula la distancia entre `p1` y `p2`. El uso de `this` permite que el método siempre se refiera al objeto que invoca el método (`p1` en este caso), mientras que el parámetro `otro` representa el punto al que se quiere medir la distancia. Esto ilustra cómo los objetos pueden interactuar entre sí mediante métodos, manteniendo encapsulados sus propios datos.



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### 
En Java, los objetos como `Punto` se pasan a los métodos **por valor de referencia**. Esto significa que **la referencia al objeto se copia**, no el objeto completo, pero ambas referencias apuntan al mismo objeto en memoria. Por ello, si se modifica algún atributo del objeto dentro del método, esos cambios **afectan al objeto original fuera del método**, porque ambos, el parámetro y la variable original, apuntan a la misma instancia.

Por ejemplo, si se añade un método que cambie las coordenadas de un `Punto` recibido como parámetro:

```java
void moverPunto(Punto p, double dx, double dy) {
    p.x += dx;
    p.y += dy;
}

Punto p1 = new Punto(3, 4);
moverPunto(p1, 1, 1);
System.out.println(p1.x + ", " + p1.y); // Muestra 4.0, 5.0
```

Se observa que los cambios realizados dentro del método afectan directamente al objeto `p1` original. Sin embargo, si dentro del método se reasignara el parámetro a un nuevo objeto, esta nueva referencia **no afectaría al objeto original**, ya que la copia de la referencia que recibe el método sería independiente de la original.

En cambio, los tipos primitivos como `int`, `double` o `boolean` se pasan **por valor**, es decir, se copia el valor y cualquier modificación dentro del método **no afecta al valor original** fuera del método. Por ejemplo:

```java
void incrementar(int n) {
    n = n + 1;
}

int x = 5;
incrementar(x);
System.out.println(x); // Muestra 5
```

Aquí, el entero `x` no se modifica porque el método trabaja sobre una **copia del valor**, no sobre la variable original. Esta diferencia es importante para entender cómo Java maneja la memoria y la interacción de objetos y valores primitivos dentro de los métodos.




## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### 
En Java, el método **`toString()`** es un método especial que devuelve una representación en forma de cadena (*String*) de un objeto. Su propósito principal es proporcionar una forma legible de visualizar el estado del objeto, por ejemplo cuando se imprime usando `System.out.println`. Por defecto, la implementación heredada de la clase `Object` devuelve un texto que incluye el nombre de la clase y un código hash, pero normalmente se **sobrescribe** para mostrar información más significativa.

El concepto de `toString()` existe en otros lenguajes con nombres o mecanismos similares, aunque no siempre exactamente igual. Por ejemplo, en C++ se puede sobrecargar el operador `<<` para imprimir objetos, y en Python se utilizan los métodos especiales `__str__()` y `__repr__()` para lograr un efecto equivalente. En todos los casos, la idea es generar una representación textual que describa el objeto de manera comprensible.

En la clase `Punto`, se puede sobrescribir `toString()` para mostrar las coordenadas de forma clara:

```java
class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Sobrescribir toString()
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

// Ejemplo de uso
Punto p = new Punto(3, 4);
System.out.println(p); // Muestra: (3.0, 4.0)
```

En este ejemplo, al imprimir `p` con `System.out.println`, se llama automáticamente a `toString()`, mostrando las coordenadas en lugar del texto genérico que daría la implementación por defecto. Esto mejora la legibilidad y permite que los objetos puedan representarse fácilmente como cadenas.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### 
Una **clase en Java** se parece en cierta medida a un **`struct` en C**, en el sentido de que ambos permiten agrupar datos relacionados bajo un mismo nombre. Por ejemplo, un `struct` puede contener varias variables de distintos tipos, al igual que una clase puede contener atributos que describen el estado de un objeto. Esta similitud es útil para conceptualizar cómo las clases organizan información, especialmente para alguien con experiencia en C.

Sin embargo, un `struct` en C **carece de varias capacidades clave de las clases**. No puede contener métodos que definan comportamiento, no tiene encapsulación para controlar el acceso a los datos y no permite herencia ni polimorfismo. Además, en C, las variables de tipo `struct` **no se consideran “objetos” con identidad propia**: no tienen un concepto de instancia en el sentido de la programación orientada a objetos, y su manejo de memoria es más manual y limitado.

Para que un `struct` fuera comparable a una clase y sus variables fueran instancias, debería poder:

1. **Agrupar datos y comportamiento** en la misma entidad (atributos y métodos).
2. **Mantener identidad propia**, de manera que cada variable pueda diferenciarse incluso si sus valores son iguales a los de otra.
3. **Gestionar el acceso a los datos** mediante mecanismos como encapsulación, getters y setters.
4. **Interactuar con otras estructuras de manera polimórfica**, es decir, poder ser tratadas mediante interfaces o herencia.

En resumen, un `struct` es solo un bloque de datos, mientras que una clase combina datos y comportamiento, define instancias con identidad y permite extender el sistema de manera segura y organizada. Por eso, la transición de C a Java implica no solo aprender sintaxis, sino adoptar una forma diferente de pensar sobre cómo modelar y organizar la información en el programa.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### 
Se puede “emular” una clase en C utilizando un **`struct` para los datos** y funciones externas que operen sobre ese `struct`. Como los `struct` no contienen métodos, la función que calcula la distancia al origen debe recibir explícitamente un puntero al `struct` para acceder a sus atributos. En este escenario, el puntero cumple un papel similar al de `this` en Java, pero no es implícito: hay que pasarlo de manera explícita en cada llamada.

Por ejemplo, la clase `Punto` de Java se podría emular en C así:

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

// Función que "pertenece" al struct
double calculaDistanciaAOrigen(Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

int main() {
    Punto p1 = {3.0, 4.0};
    double distancia = calculaDistanciaAOrigen(&p1);
    printf("%f\n", distancia);  // Muestra 5.0
    return 0;
}
```

En este ejemplo:

* El `struct Punto` **solo contiene datos**, no métodos.
* La función `calculaDistanciaAOrigen` recibe un puntero a `Punto`, lo que permite acceder a los atributos `x` e `y`.
* La referencia explícita `p` dentro de la función actúa como un **`this` manual**; no existe magia ni referencia implícita como en Java.

Esto evidencia la diferencia fundamental entre C y Java: en C, todo es más manual y separado —datos por un lado, funciones por otro—, mientras que en Java, los métodos están directamente asociados a los objetos y `this` permite referirse al objeto que los invoca de forma implícita.

