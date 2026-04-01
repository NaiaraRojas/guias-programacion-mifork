<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

###
En el lenguaje C, la composición se expresa de manera natural mediante estructuras (struct) que contienen otras estructuras como campos internos. Este tipo de relación suele describirse como “A tiene‑un B”, y permite crear entidades más complejas a partir de entidades más sencillas. Un ejemplo típico consiste en definir un struct Punto que represente una coordenada en el plano y, a partir de él, construir un struct Linea que represente una línea compuesta por dos puntos. De esta forma, se establece una relación de composición donde la línea tiene dos puntos, sin necesidad de programación orientada a objetos.

El cálculo de la distancia entre dos puntos puede implementarse mediante la fórmula de la distancia euclidiana, que utiliza la raíz cuadrada de la suma de las diferencias cuadráticas entre sus coordenadas. Del mismo modo, la longitud de una línea puede calcularse reutilizando la función de distancia, ya que una línea formada por dos puntos es, esencialmente, el segmento definido por ellos. Este enfoque demuestra cómo la composición permite combinar estructuras y funciones simples para construir conceptos más complejos de manera modular.
A continuación se presenta un ejemplo funcional en C donde se definen las estructuras mencionadas y las funciones correspondientes. El código muestra claramente cómo un tipo compuesto puede apoyarse en tipos más básicos, ofreciendo así una manera estructurada de organizar datos y comportamientos relacionados.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

// Distancia entre dos puntos
double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

// Longitud de una línea
double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    Punto a = {0.0, 0.0};
    Punto b = {3.0, 4.0};
    Linea linea = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la línea: %.2f\n", longitudLinea(linea));

    return 0;
}


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### 
En orientación a objetos, la composición se expresa creando clases que contienen instancias de otras clases, igual que ocurría con struct en C, pero con mayores garantías gracias a la encapsulación. En este caso, una clase Punto representa un objeto simple con coordenadas x e y, mientras que una clase Linea representa una línea definida por dos puntos. La relación “una línea tiene dos puntos” refleja directamente el concepto de composición, donde un objeto complejo se construye a partir de otros más simples. A diferencia de C, esta combinación resulta más robusta porque la visibilidad de los atributos puede controlarse y el estado interno de los objetos puede protegerse de modificaciones externas.

Uno de los beneficios principales de la orientación a objetos es la ocultación de información. En lugar de exponer directamente los campos internos, las clases pueden declararlos como private y proporcionar únicamente los métodos necesarios. Gracias a esto, es posible garantizar que tanto los puntos como las líneas sean inmutables: una vez creados, sus valores no pueden modificarse. Esta inmutabilidad evita errores y estados inconsistentes, especialmente en programas donde los objetos se comparten entre distintos lugares del código. En Java, la inmutabilidad se logra declarando los atributos como private final y no proporcionando setters.

La clase Punto puede incluir un método que calcule la distancia a otro punto utilizando la fórmula euclidiana, de forma similar al ejemplo en C. La clase Linea no necesita conocer los detalles internos de los puntos; simplemente los almacena y delega en ellos el cálculo que corresponda. El método de la línea para obtener su longitud se basa en llamar al método de distancia entre los dos puntos que contiene. De esta manera, la composición facilita la reutilización de código y contribuye a un diseño más modular y mantenible.

A continuación se ofrece una implementación sencilla de estas dos clases en Java, reflejando los principios comentados:

package ejercicio1;

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### 
La multiplicidad en composición describe cuántos objetos de una clase participan en la relación respecto a otra. Se utiliza para expresar si un objeto contiene uno, varios o un número variable de objetos internos. En términos de diseño, la multiplicidad permite indicar restricciones estructurales, como “un objeto A contiene exactamente dos objetos B”, o “un objeto B puede pertenecer a cero, uno o varios objetos A”. Este concepto es fundamental para representar estructuras en orientación a objetos de forma precisa y evitar ambigüedades en las relaciones entre clases.

En el ejemplo de Linea y Punto, la multiplicidad indica cuántos puntos forman una línea y cuántas líneas pueden estar asociadas a un mismo punto. Una Linea está compuesta por exactamente dos puntos, lo que implica una multiplicidad 1 → 2 en la dirección “Linea tiene Puntos”. Dicho de otra forma, cada objeto Linea depende obligatoriamente de dos objetos Punto para existir, lo que es coherente con el concepto geométrico de un segmento.

Desde la perspectiva contraria, un mismo Punto puede aparecer en muchas líneas distintas o en ninguna. No existe un límite superior estricto, puesto que varios segmentos pueden compartir puntos extremos. Por tanto, la multiplicidad en la dirección “Punto pertenece a Linea” es 0.. → 1* (un punto puede no estar en ninguna línea, puede estar en una o en muchas). Esta diferencia explica cómo un punto es independiente, mientras que la línea depende estructuralmente de los puntos que la componen, reflejando así la esencia de la composición en orientación a objetos.



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### 
La composición fuerte y la composición débil describen el grado de dependencia entre los objetos que participan en una relación de composición. Ambas expresan una relación del tipo “A tiene‑un B”, pero difieren en cómo se gestionan la creación, existencia y destrucción de los objetos implicados. Esta distinción es especialmente relevante en orientación a objetos, donde el ciclo de vida de los objetos es un aspecto central del diseño.

En la composición fuerte, el objeto contenido no puede existir de manera independiente del objeto contenedor. El ciclo de vida del objeto “parte” está completamente ligado al del objeto “todo”: se crea cuando se crea el contenedor y se destruye cuando este desaparece. Esto implica una dependencia total, tanto conceptual como temporal. En UML y en diseño orientado a objetos, este tipo de relación es la que se denomina composición propiamente dicha, y se utiliza cuando la parte no tiene sentido fuera del todo.

Por el contrario, en la composición débil, el objeto contenido puede existir independientemente del objeto que lo referencia. El contenedor no es responsable del ciclo de vida completo de la parte, sino que simplemente mantiene una referencia a ella. Si el contenedor se destruye, la parte puede seguir existiendo y ser utilizada por otros objetos. Este tipo de relación es lo que habitualmente se conoce como agregación o asociación, ya que la dependencia es más flexible y menos restrictiva.

En resumen, la diferencia fundamental entre composición fuerte y débil reside en el ciclo de vida de los objetos. La composición fuerte implica dependencia total y destrucción conjunta, y se denomina composición en sentido estricto. La composición débil implica independencia parcial o total y se denomina agregación o asociación. Esta distinción permite modelar con precisión las relaciones entre objetos y elegir el tipo adecuado según el significado real de la relación en el problema que se desea representar.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### 
Cuando una clase utiliza a otra únicamente como parámetro de un método, como valor devuelto, como variable local o creando instancias dentro de un método mediante new, se considera que existe una dependencia entre ambas clases. Este tipo de relación es puntual y no implica que el objeto “contenedor” forme parte de la identidad o estructura interna del objeto que lo usa. La dependencia simplemente indica que, para realizar cierta operación, una clase necesita conocer o utilizar temporalmente otra, sin comprometer su diseño estructural.

La diferencia con la composición es que, en dependencia, los objetos utilizados no forman parte del estado permanente del objeto que los usa. Dichos objetos suelen crearse y destruirse dentro del ámbito del método o recibirse como argumentos sin que pasen a ser almacenados como atributos. Esta relación es más débil y temporal, apareciendo únicamente durante la ejecución del método en cuestión.

Por el contrario, la composición implica que un objeto contiene a otro como parte esencial de su estructura, lo que se refleja en la existencia de atributos que representan esa relación. En este caso, el objeto contenido se asocia firmemente al ciclo de vida del contenedor y forma parte de su identidad conceptual. Al desaparecer el objeto “todo”, también desaparecen sus partes, lo que no ocurre bajo un esquema de dependencia.

En resumen, cuando una clase emplea a otra únicamente para ejecutar operaciones puntuales, sin almacenarla como atributo ni formar parte de su estado, se está ante una dependencia, no una composición. En programación orientada a objetos, la dependencia se considera la forma más débil de relación entre clases, adecuada para interacciones momentáneas y no estructurales.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### 

En composición fuerte, los objetos que forman parte de otro objeto se consideran partes esenciales de su estructura y no tienen sentido independiente fuera de él. Su ciclo de vida queda estrictamente ligado: cuando el objeto contenedor deja de existir, sus partes también desaparecen. Aplicado al caso de una Linea, esto significa que los Punto que la componen se crean dentro de la propia clase Linea y no pueden ser reutilizados ni compartidos con otras líneas. Se fuerza así una relación de “propiedad total”, reflejando que la línea controla completamente la existencia de sus puntos extremos.

En composición débil, en cambio, el objeto contenedor utiliza otros objetos como parte de su estructura, pero dichos objetos pueden existir de manera independiente y ser compartidos por varios objetos. En este escenario, los puntos no se crean dentro de la línea: se reciben desde el exterior y pueden ser utilizados por varias líneas distintas. El ciclo de vida de los puntos no depende del de la línea, de modo que aunque una línea desaparezca, los puntos que usaba pueden seguir existiendo y ser empleados por otras entidades. Esta relación corresponde a lo que suele denominarse agregación.

Estas dos aproximaciones permiten modelar situaciones diferentes dentro del diseño orientado a objetos. La composición fuerte es apropiada cuando las partes no tienen sentido fuera del todo, mientras que la composición débil resulta útil cuando se desea que los objetos puedan ser compartidos o manipulados independientemente. El ejemplo de puntos y líneas ilustra ambas alternativas, permitiendo observar cómo el control del ciclo de vida define la fortaleza de la composición.

Composición fuerte:

package ejercicio1;

// Punto inmutable
public final class Punto {
    private final double x;
    private final double y;

    Punto(double x, double y) { // constructor package-private
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}

Composición débil:

package ejercicio1;

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1; // se guardan referencias externas
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### 
En Java, incluso en una composición fuerte, el contenedor no destruye explícitamente a los objetos que contiene. Esto se debe a que Java no utiliza destrucción manual como ocurre en C o C++. En lugar de liberar memoria con operaciones como free() o destructores, Java se basa en un sistema de recolector de basura (garbage collector). Este mecanismo automático se encarga de eliminar los objetos de la memoria cuando ya no existe ninguna referencia accesible hacia ellos. Por este motivo, aunque conceptualmente en una composición fuerte se dice que “el contenedor controla el ciclo de vida de las partes”, en Java este control no se expresa mediante destrucción explícita.

En una composición fuerte, el objeto contenedor es el único propietario lógico de los objetos que agrupa. En el ejemplo de la clase Linea, los puntos se crean dentro del constructor y no se exponen de manera modificable al exterior. Al desaparecer todas las referencias a la Linea, también desaparecen todas las referencias a esos Punto, lo que provoca que ambos queden disponibles para ser eliminados automáticamente por el recolector de basura. Por tanto, no es necesario que Linea invoque ninguna operación de “destrucción” manual, ya que la propia falta de referencias basta para que los objetos sean considerados inalcanzables.

Este comportamiento refleja una diferencia clave entre la composición fuerte en Java y en lenguajes como C++. En Java, el ciclo de vida se controla por alcance y por referencias, no mediante instrucciones explícitas del programador. Por ello, la composición fuerte se representa creando los objetos internos en el constructor y evitando que se compartan fuera de su contenedor, de forma que los elementos contenidos “mueren” cuando el contenedor deja de ser accesible. La destrucción ocurre, pero es responsabilidad del runtime y no del código del programador.

Si quieres, puedo preparar también la siguiente pregunta del cuestionario manteniendo este estilo.



## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### 
En una composición débil, el objeto contenedor mantiene referencias a otros objetos sin ser responsable de su ciclo de vida. Esto implica que dichos objetos pueden existir fuera del contenedor, ser compartidos y no quedar necesariamente ligados a él de forma estricta. En este contexto, el departamento mantiene un conjunto de profesores, pero no los crea ni los destruye directamente; únicamente los almacena y los utiliza mientras formen parte de su estructura. Esta relación permite flexibilidad, ya que los profesores pueden ser sustituidos, eliminados o estar asociados simultáneamente a otros contextos en el programa.

La existencia de un director introduce una segunda relación de composición débil, donde el departamento referencia a uno de los profesores como responsable principal. Este elemento forma parte del grupo general de profesores, y su presencia debe estar garantizada desde el inicio. La invariante que exige que el director siempre sea un miembro del profesorado contribuye a mantener la coherencia del modelo, ya que se impide que exista un director ajeno al departamento o que se elimine del listado sin actualizar adecuadamente el cargo.

El uso de un array primitivo para almacenar a los profesores refuerza la idea de estructura interna controlada, pero no debe exponerse al exterior para no romper la encapsulación. En su lugar, el acceso debe realizarse mediante métodos que indiquen cuántos profesores hay o devuelvan un profesor por posición válida. Con este enfoque, se evita que otras partes del programa modifiquen directamente la representación interna del departamento, garantizando la integridad de las invariantes asociadas al director y a la lista de profesores.

La composición débil permite modelar este escenario de forma natural, ya que ni el director ni los profesores dependen del departamento para existir. Sin embargo, sí existe una restricción conceptual: el director siempre debe estar presente en la lista de profesores. Esta invariante obliga a validar operaciones como el cambio de director o la eliminación de profesores, impidiendo que se deje al departamento en un estado inconsistente. El diseño resultante mantiene la flexibilidad de la agregación, pero con reglas adicionales para asegurar la coherencia interna.

 Código Java – Composición débil con invariantes

 package ejercicio1;

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

package ejercicio1;

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;

    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber un director desde el inicio.");
        }
        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;
        this.director = directorInicial;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int indice) {
        if (indice < 0 || indice >= numProfesores) {
            throw new IndexOutOfBoundsException("Índice fuera de rango.");
        }
        return profesores[indice];
    }

    public void agregarProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser null.");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No se pueden añadir más profesores.");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int indice) {
        if (indice < 0 || indice >= numProfesores) {
            throw new IndexOutOfBoundsException("Índice fuera de rango.");
        }
        Profesor aEliminar = profesores[indice];

        // Invariante: no se puede eliminar al director
        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        // Desplazar elementos a la izquierda
        for (int i = indice; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
        profesores[numProfesores] = null; // limpiar referencia
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null.");
        }

        // Invariante: el director debe ser profesor del departamento
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalStateException("El director debe estar en la lista de profesores.");
        }

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}
``




## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### 


Al sustituir un array primitivo por una colección de tipo `List`, gran parte de la gestión manual desaparece. Las operaciones de añadir y eliminar elementos ya no requieren desplazar elementos dentro del array ni controlar manualmente un contador de posiciones libres. El propio `List` se encarga de ajustar su tamaño dinámicamente y de mantener el orden de los elementos. Esto reduce la cantidad de código, elimina muchos errores frecuentes y mejora la legibilidad del diseño, sin romper la encapsulación mientras la estructura interna se mantenga privada.

El uso de `List` también simplifica los métodos del tipo “añadir profesor”, “eliminar profesor por posición” o “obtener profesor por posición”, ya que estas operaciones ya están soportadas mediante `add()`, `remove(int)` y `get(int)`. El diseño orientado a objetos se vuelve más expresivo y menos propenso a errores al no depender de un array fijo ni del control de un índice manual. Por tanto, la implementación con listas consigue el mismo resultado conceptual que con arrays, pero con un código más mantenible y directo.

No obstante, el empleo de `List` introduce un riesgo adicional si se expone directamente la lista interna al exterior. Si existiese un método que devolviese “todos los profesores” retornando la lista real, el usuario del objeto podría modificarla libremente desde fuera, rompiendo las invariantes del departamento, como la obligación de que el director forme parte del profesorado. En ese caso, se estaría filtrando la representación interna, rompiendo la encapsulación y permitiendo estados inconsistentes. La forma habitual de resolverlo consiste en devolver una **copia inmutable** o una **vista no modificable** de la lista interna, asegurando que el exterior pueda consultar pero no alterar su contenido.

Código usando `List` en lugar de arrays

Clase `Profesor` (igual que antes)

```java
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
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

 Clase `Departamento` con `List<Profesor>`

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class Departamento {

    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber un director desde el inicio.");
        }
        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public void agregarProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser null.");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor aEliminar = profesores.get(pos);

        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director.");
        }

        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null.");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe ser un profesor del departamento.");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    /**
     * Método seguro para devolver profesores sin romper encapsulación
     */
    public List<Profesor> getProfesoresSeguro() {
        return Collections.unmodifiableList(profesores);
    }
}
```


 ¿Qué parte del código original nos hemos ahorrado?

*   Ya **no es necesario** mantener un contador `numProfesores`.
*   No hace falta desplazar manualmente elementos al eliminar.
*   No existe límite fijo (como `MAX_PROFESORES`).
*   No se limpia manualmente referencias (`profesores[pos] = null`).
*   Se elimina la validación de espacio libre en el array.
*   Se simplifica mucho la lógica en `agregarProfesor` y `eliminarProfesor`.

En resumen, `List` ahorra toda la **gestión manual del almacenamiento**, dejando solo la lógica de negocio.

¿Por qué NO debe devolverse la lista interna? ¿Cómo solucionarlo?

Si existiera un método:

```java
public List<Profesor> getProfesores() {
    return profesores; // ❌ MAL
}
```

entonces otro código podría hacer:

```java
departamento.getProfesores().clear();        // Se borran todos los profesores
departamento.getProfesores().remove(0);      // Podría eliminar al director
departamento.getProfesores().add(new Profesor("x")); // Podría romper la invariante
```

Eso **rompe completamente la encapsulación** y permitiría violar la invariante de que el director debe siempre pertenecer al departamento.

✅ La solución correcta es devolver una **vista no modificable**:

```java
public List<Profesor> getProfesores() {
    return Collections.unmodifiableList(profesores);
}
```

o devolver una **copia defensiva**:

```java
public List<Profesor> getProfesores() {
    return new ArrayList<>(profesores);
}
```

En ambos casos, la representación interna queda protegida.





## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### 

Una composición recursiva aparece cuando un objeto contiene a otro objeto **del mismo tipo**, de forma que la estructura puede describirse como una cadena o árbol de elementos relacionados. En este tipo de diseño, cada instancia encierra otra del mismo tipo (como una persona que tiene una madre, que también es una persona). Este patrón resulta útil cuando los elementos siguen una jerarquía natural, donde cada nivel puede describirse en términos del mismo concepto. Tal estructura se encuentra habitualmente en árboles genealógicos, elementos gráficos, o estructuras organizativas.

La inmutabilidad en una composición recursiva implica que una vez creada la instancia, su relación con la instancia interna no puede modificarse. Esto proporciona coherencia y facilita el razonamiento, ya que cada objeto permanece en un estado consistente y estable. En Java, este enfoque se implementa mediante atributos privados y finales, evitando cualquier método que permita modificar el estado interno. De este modo se obtiene una estructura recursiva segura en la que cada instancia representa un nodo fijo dentro de la cadena.

Al igual que ocurre con las excepciones encadenadas en Java, donde una excepción puede envolver una causa que a su vez envuelve otra, las composiciones recursivas permiten establecer relaciones transitivas sin límite explícito. La estructura resultante facilita navegar hacia “arriba” o “atrás” siguiendo una referencia constante hacia el elemento previo. Esto da lugar a representaciones compactas y expresivas del dominio del problema, sin perder seguridad en el manejo de los objetos internos.

Este tipo de patrón es habitual en estructuras como carpetas y subcarpetas, nodos de árboles sintácticos, listas enlazadas, organizaciones jerárquicas e incluso comportamientos como decoradores en patrones de diseño. Todos ellos comparten la idea de que una instancia contiene otra del mismo tipo, permitiendo modelar relaciones cíclicas naturales o estructuras jerárquicas en profundidad.

Código Java — Persona inmutable con madre (composición recursiva)

```java
package ejercicio1;

public final class Persona {
    private final String nombre;
    private final Persona madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío.");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null si se desea permitir el inicio de la cadena
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return (madre == null)
                ? "Persona: " + nombre + " (sin madre registrada)"
                : "Persona: " + nombre + ", madre: " + madre.getNombre();
    }
}
```

Ejemplo de uso — familia (nieto → madre → abuela)

```java
package ejercicio1;

public class EjercicioPersonas {
    public static void main(String[] args) {
        Persona abuela = new Persona("María", null);
        Persona madre  = new Persona("Laura", abuela);
        Persona nieto  = new Persona("Carlos", madre);

        System.out.println(nieto);
        System.out.println(nieto.getMadre());
        System.out.println(nieto.getMadre().getMadre());
    }
}
```

Salida aproximada:

    Persona: Carlos, madre: Laura
    Persona: Laura, madre: María
    Persona: María (sin madre registrada)

Otros ejemplos típicos de composiciones recursivas

*   **Árbol genealógico:** cada persona tiene padre o madre, que son personas.
*   **Sistema de archivos:** una carpeta contiene carpetas, que contienen carpetas.
*   **Listas enlazadas:** cada nodo contiene una referencia al siguiente nodo.
*   **Árboles sintácticos o expresiones:** una expresión contiene subexpresiones.
*   **Árboles de decisión o estructuras jerárquicas:** cada nodo contiene hijos del mismo tipo.



## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### 
Una composición bidireccional aparece cuando dos clases mantienen referencias mutuas entre sí, formando una relación “de ida y vuelta”. En este tipo de diseño, cada objeto conoce al otro y puede navegar la relación en ambas direcciones. A diferencia de la composición unidireccional, donde solo el contenedor mantiene la referencia a sus partes, la composición bidireccional introduce una dependencia simétrica que obliga a mantener la coherencia entre ambas referencias. Esto implica que, si un objeto A contiene a un objeto B como parte, entonces B también mantiene una referencia de vuelta que indica que pertenece a A.

Aplicado al ejemplo de Departamento y Profesor, una relación bidireccional exigiría que cada profesor conociera a qué departamento pertenece, además de que el departamento tenga su lista de profesores. En el código original solo existía una dirección: el departamento almacenaba profesores, pero los profesores no sabían nada del departamento. Para convertir esta relación en bidireccional sería necesario añadir un atributo dentro de Profesor, por ejemplo private Departamento departamento, y asignarlo adecuadamente cada vez que un profesor fuese añadido al departamento. Del mismo modo, al eliminar un profesor, sería necesario borrar también su referencia al departamento para evitar inconsistencias.

Este tipo de composición requiere un cuidado especial para mantener las invariantes del sistema. Si el departamento cambia su director, habría que verificar que dicho profesor pertenece realmente al mismo departamento, incluyendo comprobar su referencia en la clase Profesor. Asimismo, cualquier modificación en la lista de profesores debería actualizar de manera inmediata la referencia recíproca, de modo que no existan profesores que “crean” pertenecer a un departamento sin que el departamento los tenga en su lista. La bidireccionalidad, aunque más expresiva, exige sincronizar ambas partes de la relación en cada operación.

Finalmente, este tipo de diseño se considera normalmente más complejo y propenso a errores si no se vigila la coherencia interna. En muchos casos, la composición bidireccional se evita a favor de relaciones unidireccionales más sencillas, salvo cuando es realmente necesaria la navegación en ambas direcciones. En el ejemplo de Profesor y Departamento, solo sería aconsejable introducirla si realmente se necesitara en el modelo que un profesor pueda preguntar a qué departamento pertenece de forma obligatoria.
