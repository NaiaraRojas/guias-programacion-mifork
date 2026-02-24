<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### 
En C, donde no existen excepciones, el control de errores suele realizarse mediante **valores de retorno especiales o mecanismos externos**. A diferencia de Java (donde se lanzaría una excepción), en C la función no interrumpe automáticamente el flujo de ejecución, sino que debe comunicar el error de forma explícita al código que la invoca. El control del error se realiza desde fuera de la función comprobando el resultado o algún indicador adicional.

Una primera opción consiste en **devolver un valor especial que indique error**. Por ejemplo, si la función debe recibir un número positivo, puede devolver un valor imposible o reservado (como `-1`) cuando el argumento sea negativo. El código que llama a la función debe comprobar ese valor antes de usar el resultado.

```c
#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // Valor especial indicando error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-9);

    if (resultado < 0) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

Una segunda opción consiste en **separar el resultado del estado de error**, utilizando por ejemplo un parámetro adicional (pasado por referencia mediante puntero) que indique si hubo error. Esta técnica es más robusta, ya que evita ambigüedades con valores válidos.

```c
#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0;  // 0 indica error
    }
    *resultado = sqrt(x);
    return 1;  // 1 indica éxito
}

int main() {
    float resultado;
    int ok = raiz(-9, &resultado);

    if (!ok) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

En ambos casos, el error no se gestiona dentro de la función, sino que se informa al exterior mediante un protocolo acordado (valor especial o código de estado). Este enfoque es típico en C y requiere que el programador recuerde siempre comprobar el resultado, ya que el lenguaje no obliga a hacerlo.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### 
Una **excepción** es un mecanismo del lenguaje que permite representar y gestionar situaciones anómalas que ocurren durante la ejecución de un programa. Se trata de un objeto que encapsula información sobre un error o condición inesperada, y que interrumpe el flujo normal de ejecución para transferir el control a un manejador específico. A diferencia del enfoque tradicional en C (basado en códigos de retorno), una excepción no requiere que cada llamada compruebe manualmente el resultado, ya que el propio lenguaje se encarga de propagar el error hasta que sea tratado.

El objetivo de las excepciones es **separar la lógica principal del programa del tratamiento de errores**, mejorando la claridad y mantenibilidad del código. Cuando un programador implementa una función, puede lanzar una excepción si detecta una situación que impide continuar correctamente (por ejemplo, un argumento inválido). Cuando otro programador llama a esa función, puede decidir capturar y tratar esa excepción, o dejar que se propague a un nivel superior.

En definitiva, las excepciones permiten diseñar programas más robustos y expresivos, donde los errores no se confunden con valores válidos de retorno y donde el control de situaciones excepcionales se realiza de forma estructurada y explícita.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### 
En Java, el control de errores puede realizarse mediante **excepciones**, lo que permite separar claramente la lógica del cálculo del tratamiento del error. En lugar de devolver un valor especial como en C, el método puede lanzar una excepción cuando detecta un argumento inválido. De esta forma, el error no se mezcla con los valores de retorno normales y el lenguaje obliga a gestionar la situación si se trata de una excepción comprobada.

En el siguiente ejemplo, el método `raiz` se define dentro de la clase `Calculadora`. Si se recibe un número negativo, se lanza una excepción del tipo `IllegalArgumentException`, que es una excepción no comprobada. El control del error se realiza desde fuera del método, en el `main`, utilizando un bloque `try-catch`.

```java
public class Calculadora {

    public double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo"
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();

        try {
            double resultado = calc.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

En este diseño, el método `raiz` se limita a detectar la situación incorrecta y lanzar la excepción, mientras que el método `main` decide cómo reaccionar ante el error. Este enfoque resulta más claro y seguro que el uso de valores especiales, ya que evita ambigüedades y permite que el flujo del programa se interrumpa automáticamente hasta que la excepción sea tratada.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### 
**Lanzar** una excepción significa crear un objeto que representa un error y transferir inmediatamente el control fuera del método actual mediante la instrucción `throw`. En el ejemplo anterior, cuando el método `raiz` detecta un número negativo, ejecuta `throw new IllegalArgumentException(...)`. En ese instante, el método deja de ejecutarse y no continúa con ninguna instrucción posterior. El flujo normal del programa se interrumpe y el sistema busca un lugar adecuado donde tratar esa situación excepcional.

**Capturar** (o controlar) una excepción significa interceptarla mediante un bloque `try-catch` para gestionar el error de forma explícita. En el `main`, la llamada a `calc.raiz(-9)` se encuentra dentro de un bloque `try`. Cuando se lanza la excepción, el control pasa automáticamente al bloque `catch`, donde se decide qué hacer (por ejemplo, mostrar un mensaje). Capturar una excepción implica, por tanto, asumir la responsabilidad de tratarla y evitar que el programa termine abruptamente.

Se dice que una excepción se **propaga** cuando, tras ser lanzada, no es capturada en el método donde se produjo y asciende por la pila de llamadas buscando un manejador adecuado. En este proceso, los métodos intermedios finalizan inmediatamente sin completar su ejecución normal. Es decir, cada función en la pila se va “desapilando” (terminando) hasta encontrar un `catch` compatible. Las funciones que no capturan la excepción no se reanudan después: su ejecución queda interrumpida definitivamente. Si ningún método la captura, la excepción llega al `main` y el programa termina mostrando un mensaje de error.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### 
La **propagación natural** de las excepciones permite que un error detectado en un método se transmita automáticamente a los niveles superiores de la pila de llamadas sin necesidad de comprobar manualmente códigos de retorno en cada función intermedia. En C, cada función debe revisar explícitamente el valor devuelto por la función que invoca y, si hay error, devolverlo a su vez. Este patrón repetitivo aumenta la complejidad y hace más probable que se olvide comprobar un error. En Java, en cambio, el lenguaje se encarga de esa propagación de forma automática.

Otra ventaja importante es la **separación clara entre la lógica principal y el tratamiento de errores**. En C, el código suele quedar “contaminado” con múltiples comprobaciones `if (error) return error;`, lo que dificulta la lectura. Con excepciones, el flujo principal se escribe de manera limpia, y el tratamiento del error se concentra en bloques `try-catch` situados donde realmente se puede tomar una decisión significativa. Esto mejora la claridad, mantenibilidad y modularidad del programa.

Además, la propagación automática reduce el riesgo de inconsistencias. Si una función intermedia no sabe cómo resolver un problema, simplemente no lo captura y permite que ascienda hasta un nivel donde sí pueda gestionarse adecuadamente. No es necesario definir manualmente protocolos de devolución de errores. Esto hace que los programas sean más robustos, ya que el lenguaje garantiza que la excepción no pase desapercibida y que el flujo de ejecución se interrumpa correctamente hasta encontrar un manejador o finalizar el programa.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### 
En Java, siguiendo el paradigma de orientación a objetos, las **excepciones son objetos**. Todas las excepciones heredan directa o indirectamente de la clase `Throwable`. Esto significa que una excepción no es simplemente un código numérico o un mensaje de texto, sino una instancia de una clase que puede contener estado (atributos) y comportamiento (métodos). Esta característica encaja naturalmente con los conceptos de clases y encapsulación previamente estudiados.

El hecho de que las excepciones sean objetos aporta ventajas claras en términos de **encapsulación**. Una excepción puede almacenar información relevante sobre el error (por ejemplo, un valor incorrecto recibido, un identificador, o un mensaje detallado) y ofrecer métodos para acceder a esos datos de forma controlada. En lugar de depender de variables globales o códigos ambiguos, toda la información relacionada con el problema queda encapsulada dentro del propio objeto excepción. Esto mejora la organización del código y evita que los detalles internos del error se dispersen por el programa.

Como consecuencia, es posible **crear excepciones personalizadas**, definiendo nuevas clases que hereden de `Exception` o de alguna de sus subclases. De esta manera, se pueden representar errores específicos del dominio de la aplicación. Por ejemplo, podría definirse una excepción `NumeroNegativoException` para el caso de la raíz cuadrada:

```java
public class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String mensaje) {
        super(mensaje);
    }
}
```

Esto permite diseñar sistemas más expresivos y coherentes, donde cada tipo de problema tiene su propia representación, manteniendo la filosofía de la programación orientada a objetos.



## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### 
En comparación con C, donde normalmente solo se dispone de un código de error o un valor especial, en Java cualquier **objeto excepción** transporta información mucho más rica cuando llega a un manejador (`catch`). Una de las informaciones esenciales es el **tipo concreto de la excepción**. El propio tipo (por ejemplo, `IllegalArgumentException`, `IOException`, etc.) ya clasifica el problema y permite diferenciar automáticamente entre distintos tipos de errores sin necesidad de analizar códigos numéricos.

Otra información fundamental es el **mensaje descriptivo** asociado a la excepción. Este mensaje, accesible mediante métodos como `getMessage()`, proporciona detalles específicos sobre lo ocurrido. A diferencia de C, donde el mensaje suele construirse manualmente en el punto de detección del error o consultarse en una variable global como `errno`, en Java el mensaje forma parte del propio objeto y viaja con él hasta el manejador.

Además, toda excepción incluye automáticamente la **traza de la pila de llamadas (stack trace)** en el momento en que fue lanzada. Esta información indica qué métodos estaban activos y en qué línea se produjo el error. Resulta extremadamente útil para depuración, ya que permite reconstruir el camino exacto seguido por el programa hasta el punto de fallo. Esta capacidad de transportar contexto detallado es una ventaja directa de la encapsulación y del hecho de que las excepciones sean objetos completos.



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### 
En Java es posible tener **más de un bloque `catch`** asociado a un mismo bloque `try`. Esto permite tratar de forma diferente distintos tipos de excepciones que puedan producirse dentro del mismo bloque. Cada `catch` especifica el tipo de excepción que es capaz de manejar, lo que permite diseñar respuestas específicas según la naturaleza del error.

Sin embargo, aunque puedan declararse varios bloques `catch`, **solo se ejecuta uno de ellos** cuando ocurre una excepción. El sistema compara el tipo de la excepción lanzada con los tipos declarados en los `catch`, en el orden en que aparecen, y ejecuta el primero que sea compatible. Una vez que se entra en un `catch`, los demás se ignoran y no se evalúan.

Por esta razón, el orden de los `catch` es importante. Si se coloca primero un `catch` que capture una clase más general (por ejemplo, `Exception`), los bloques posteriores para excepciones más específicas nunca se ejecutarán, ya que la excepción quedará capturada antes. Por tanto, deben colocarse primero los `catch` más específicos y después los más generales.



## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### 
En Java, para garantizar que se ejecute siempre un bloque de código sin importar si se produjo una excepción o no, se utiliza el bloque **`finally`**. Este bloque se coloca después de los bloques `try` y `catch` y se ejecuta siempre, ya sea que se haya capturado una excepción o que el bloque `try` se complete con normalidad. Es especialmente útil para liberar recursos como ficheros, conexiones de red o memoria asignada, asegurando que el programa no quede en un estado inconsistente.

Incluso si no se captura la excepción dentro de un `catch`, el bloque `finally` se ejecuta antes de que la excepción continúe propagándose. De esta manera, se puede garantizar que se realicen operaciones críticas de limpieza, independientemente de si el flujo normal se interrumpe. Esto evita problemas típicos en C, donde la liberación de recursos depende de tener que escribir manualmente comprobaciones y llamadas a `free` o `fclose` en cada posible retorno de error.

Ejemplo con `catch` y `finally`:

```java id="kz7v5b"
import java.io.FileReader;
import java.io.IOException;

public class EjemploFinally {

    public static void main(String[] args) {
        FileReader archivo = null;

        try {
            archivo = new FileReader("datos.txt");
            // Operaciones de lectura
            int c = archivo.read();
            System.out.println((char)c);
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        } finally {
            try {
                if (archivo != null) {
                    archivo.close();
                    System.out.println("Archivo cerrado en finally");
                }
            } catch (IOException e) {
                System.out.println("Error cerrando el archivo: " + e.getMessage());
            }
        }
    }
}
```

Ejemplo sin `catch` pero con `finally` (la excepción se propaga tras el `finally`):

```java id="u8nq1v"
import java.io.FileReader;
import java.io.IOException;

public class FinallySinCatch {

    public static void main(String[] args) throws IOException {
        FileReader archivo = null;

        try {
            archivo = new FileReader("datos.txt");
            int c = archivo.read();
            System.out.println((char)c);
        } finally {
            if (archivo != null) {
                archivo.close();
                System.out.println("Archivo cerrado en finally");
            }
        }
    }
}
```

En ambos casos, el bloque `finally` garantiza la **liberación de recursos**. La diferencia principal es que en el primer ejemplo la excepción se captura y se puede manejar, mientras que en el segundo la excepción se propaga al llamador después de ejecutar el `finally`. Esto demuestra cómo `finally` asegura la ejecución de tareas críticas aunque el flujo normal se interrumpa por una excepción.



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### 
Sí, en Java un bloque **`finally` puede ir sin `catch`**, es decir, puede aparecer después de un `try` solo. Esta construcción se utiliza cuando no se desea manejar la excepción en ese nivel, pero sí se quiere garantizar que se ejecuten ciertas acciones de limpieza o liberación de recursos antes de que la excepción se propague.

El bloque `finally` **siempre se ejecuta**, independientemente de que ocurra o no una excepción dentro del `try`. Esto asegura que operaciones críticas como cerrar ficheros, liberar memoria o cerrar conexiones de red se realicen aunque la ejecución del bloque `try` sea interrumpida por un error.

Incluso si hay un **`return` dentro del `try`**, el bloque `finally` se ejecuta antes de que el método realmente devuelva el valor. Esto significa que el `finally` tiene prioridad para completar su ejecución antes de que se complete el retorno, garantizando la limpieza de recursos incluso en rutas de salida tempranas del método. Por ejemplo:

```java id="v9b3yc"
public class FinallyReturn {

    public static int ejemplo() {
        try {
            System.out.println("Dentro del try");
            return 10;  // Se intenta retornar
        } finally {
            System.out.println("Finalmente siempre se ejecuta");
        }
    }

    public static void main(String[] args) {
        int valor = ejemplo();
        System.out.println("Valor retornado: " + valor);
    }
}
```

Salida del programa:

```
Dentro del try
Finalmente siempre se ejecuta
Valor retornado: 10
```

Esto demuestra que el bloque `finally` se ejecuta **antes de completar el retorno** y también **aunque ocurra una excepción no capturada**, garantizando que la limpieza o cierre de recursos se haga de manera confiable.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### 
En Java, las excepciones se clasifican en **controladas (checked)** y **no controladas (unchecked)** según cómo obliga el lenguaje a gestionarlas. Las **excepciones controladas** son aquellas que **el compilador exige capturar o declarar** mediante `throws`. Su objetivo es forzar al programador a prever errores que pueden ocurrir en condiciones externas previsibles, como problemas de entrada/salida o acceso a ficheros. Ejemplos típicos son `IOException` o `SQLException`. Este tipo de excepciones permite diseñar código más robusto frente a fallos esperables, obligando a documentar y manejar cada situación de riesgo.

Por el contrario, las **excepciones no controladas** heredan de `RuntimeException` y **no es obligatorio declararlas ni capturarlas**. Estas representan errores de programación o situaciones que normalmente no se espera que ocurran durante la ejecución normal, como errores lógicos o violaciones de contratos, por ejemplo `NullPointerException` o `ArrayIndexOutOfBoundsException`. Su ventaja es que no sobrecargan el código con capturas innecesarias cuando la recuperación no es posible o no tiene sentido, pero el riesgo es que si no se prevén, pueden interrumpir el programa abruptamente.

Algunos ejemplos que incluso se podrían definir nosotros mismos:

**Excepciones controladas (checked)**:

1. `ArchivoNoEncontradoException` – al intentar abrir un fichero que puede no existir.
2. `DatosInvalidosException` – al validar información introducida por un usuario.
3. `ConexionPerdidaException` – al interactuar con un servicio remoto.
4. `FormatoIncorrectoException` – al parsear datos que pueden no cumplir un formato esperado.

**Excepciones no controladas (unchecked)**:

1. `IndiceFueraDeRangoException` – al acceder a una posición inválida en un arreglo.
2. `ValorNuloException` – al intentar usar un objeto que es `null`.
3. `OperacionNoSoportadaException` – al invocar un método que no debe ejecutarse en determinado contexto.
4. `DivisionPorCeroException` – cuando ocurre una operación matemática inválida.

En términos de **preferencia de uso**:

* Se suele preferir **excepciones controladas** cuando:

  1. Se trabaja con recursos externos (archivos, bases de datos, red).
  2. Se procesan datos introducidos por el usuario.
  3. Se valida información que puede estar fuera de rango o malformada.
  4. Se interactúa con sistemas cuya respuesta puede fallar por condiciones externas.

* Se suele preferir **excepciones no controladas** cuando:

  1. Se detectan errores de programación que no deberían ocurrir en ejecución normal.
  2. Se violan invariantes internas de clases u objetos.
  3. Se accede a colecciones con índices fuera de rango.
  4. Se intenta operar sobre referencias nulas o datos mal inicializados.

Esta diferenciación permite que Java combine seguridad frente a errores previsibles con eficiencia y limpieza en casos de errores internos de programación.



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### 
En Java, la palabra clave **`throws`** se utiliza en la declaración de un método para indicar que este **puede generar una o varias excepciones controladas** durante su ejecución. Al declarar `



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### 
En Java, cuando un método puede generar una **excepción controlada** pero no desea gestionarla internamente, se puede declarar mediante `throws` para que la excepción **se propague al llamador**. Esto es útil cuando el método no tiene suficiente contexto para decidir cómo actuar ante un fallo, como al abrir un fichero que podría no existir. Al mismo tiempo, un bloque `finally` puede garantizar que se realicen acciones de limpieza, como cerrar recursos, independientemente de que ocurra la excepción.

Ejemplo de método que abre un fichero y propaga la excepción `FileNotFoundException`, usando `finally` para cerrar el recurso:

```java id="xz3r2q"
import java.io.FileReader;
import java.io.FileNotFoundException;
import java.io.IOException;

public class LecturaArchivo {

    public void abrirArchivo(String nombreArchivo) throws FileNotFoundException {
        FileReader archivo = null;
        try {
            archivo = new FileReader(nombreArchivo);
            System.out.println("Archivo abierto correctamente");
            // Aquí se podrían realizar operaciones de lectura
        } finally {
            if (archivo != null) {
                try {
                    archivo.close();
                    System.out.println("Archivo cerrado en finally");
                } catch (IOException e) {
                    System.out.println("Error cerrando el archivo: " + e.getMessage());
                }
            }
        }
    }

    public static void main(String[] args) {
        LecturaArchivo lector = new LecturaArchivo();
        try {
            lector.abrirArchivo("datos.txt");
        } catch (FileNotFoundException e) {
            System.out.println("El archivo no existe: " + e.getMessage());
        }
    }
}
```

En este ejemplo:

* El método `abrirArchivo` **declara** `throws FileNotFoundException`, por lo que no maneja directamente el caso de fichero inexistente.
* El bloque `finally` asegura que, si se abrió el archivo, se cierre correctamente antes de propagar cualquier excepción.
* El `main` captura la excepción lanzada y puede decidir cómo reaccionar, demostrando que `throws` permite delegar la responsabilidad de manejo al llamador mientras se mantiene la seguridad en la liberación de recursos.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### 
Sí, en Java es **posible declarar en `throws` excepciones no controladas** como `RuntimeException` o cualquiera de sus subclases. Sin embargo, el compilador **no obliga** al método llamador a capturarlas ni a declararlas, porque se consideran errores que normalmente no se esperan manejar durante la ejecución normal. Esto incluye problemas de programación como `NullPointerException` o `ArrayIndexOutOfBoundsException`.

Aunque se pueda declarar, **poner `throws RuntimeException` no cambia el comportamiento del lenguaje**, ya que el compilador no exige que se capture ni que se propague explícitamente. Por tanto, en la práctica, un bloque `try-catch` no es obligatorio para estas excepciones, y normalmente se deja que se propaguen si representan errores internos que deberían corregirse en el código. Capturarlas puede ser útil únicamente cuando se desea realizar alguna acción concreta antes de que el programa termine abruptamente, como registrar el error, liberar recursos o mostrar un mensaje amigable al usuario.

El sentido de incluir excepciones no controladas en `throws` es **documentativo o comunicativo**, para indicar a otros desarrolladores que este método podría provocar ciertos errores graves si se usan incorrectamente. Por ejemplo, un método podría declarar:

```java id="kq5z2d"
public void procesarDatos(Object dato) throws NullPointerException {
    System.out.println(dato.toString());
}
```

Aquí, `throws NullPointerException` **no obliga al llamador a capturarla**, pero deja claro que pasar un valor `null` causará un fallo. Esto puede ser útil en librerías para advertir sobre usos indebidos o precondiciones, aunque la propagación y manejo sigue siendo opcional.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### 
Se recomienda usar **excepciones controladas** (`checked`) como `IOException` cuando el error puede ser causado por condiciones **externas previsibles** y manejables, como fallos en ficheros, bases de datos o comunicación de red. Estas excepciones obligan al programador a prever y documentar cómo reaccionar ante fallos que no dependen del diseño interno del programa, aumentando la robustez y evitando que errores esperables pasen desapercibidos.

Por el contrario, las **excepciones no controladas** (`unchecked`) como `IllegalArgumentException` se usan para **errores de programación o violaciones de contratos internos**, como pasar un argumento inválido a un método o acceder a un índice fuera de rango. No se espera que estas situaciones ocurran en condiciones normales si el programa está bien escrito, por lo que no es necesario forzar al llamador a capturarlas. Su función es alertar de fallos lógicos y ayudar a detectar errores en tiempo de desarrollo.

No todos los lenguajes distinguen entre estas dos categorías. Por ejemplo, **C y C++ no tienen excepciones controladas**, solo códigos de retorno o, si se usa C++, excepciones similares a las no controladas de Java. En lenguajes como **Python**, todas las excepciones son básicamente no controladas: el programador puede capturarlas con `try-except`, pero no está obligado a declararlas ni a hacerlo.

En los lenguajes que sólo ofrecen una opción, lo habitual es **simular excepciones no controladas**: se lanza el error y el flujo se interrumpe hasta que alguien decida capturarlo, o termina el programa. Esto simplifica la sintaxis y evita la sobrecarga de declarar cada posible error, aunque requiere disciplina del programador para capturar y manejar los errores relevantes.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### 
Sí, en Java **tiene sentido lanzar excepciones dentro de un `catch`**. Esto se hace cuando, al capturar un error, el código detecta que no puede resolverlo completamente y necesita comunicarlo a un nivel superior de la aplicación, posiblemente con más información o transformándolo en un tipo de excepción más adecuado al contexto. Esto permite **encadenar y enriquecer la información del error** sin perder el flujo de propagación.

También es posible **relanzar la misma excepción capturada** usando `throw` dentro del bloque `catch`. Esto es útil cuando se quiere **realizar alguna acción adicional** (como registrar el error, liberar recursos o generar un log) pero, después, permitir que la excepción siga propagándose hacia arriba, para que un nivel superior la gestione o termine el programa de manera controlada.

**Ejemplo lanzando una nueva excepción dentro de `catch`:**

```java id="r5x2qk"
public void leerArchivo(String nombre) {
    try {
        // Código que puede generar FileNotFoundException
        FileReader archivo = new FileReader(nombre);
    } catch (FileNotFoundException e) {
        // Se captura pero se lanza una excepción más específica del dominio
        throw new RuntimeException("No se pudo abrir el archivo de configuración", e);
    }
}
```

**Ejemplo relanzando la misma excepción:**

```java id="y8p1kv"
public void procesarArchivo(String nombre) throws IOException {
    try {
        FileReader archivo = new FileReader(nombre);
    } catch (IOException e) {
        System.out.println("Error al abrir el archivo, registrando incidente...");
        // Hacemos limpieza o log, y luego dejamos que la excepción siga propagándose
        throw e;  
    }
}
```

En el primer ejemplo, se **transforma** la excepción capturada en otra que aporta contexto. En el segundo, se **relanza la misma excepción**, conservando su tipo y pila de llamadas, pero permitiendo realizar acciones adicionales antes de que el error llegue al llamador. Ambos patrones son útiles para mantener control del flujo de errores y al mismo tiempo enriquecer la información o realizar tareas críticas de limpieza.



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### 
Que una excepción sea la **"causa"** de otra significa que un error de bajo nivel, que provocó un fallo, se **encapsula dentro de otra excepción de más alto nivel** para aportar contexto adicional sin perder la información original. Esto permite al llamador comprender tanto el problema que ocurrió internamente como el contexto en que se produjo. En Java, esto se hace pasando la excepción original como argumento al constructor de la nueva excepción, utilizando `Throwable cause`.

Por ejemplo, supongamos que se intenta leer un archivo de configuración. Si ocurre un `FileNotFoundException` (error de bajo nivel), se puede capturar y lanzar una **excepción personalizada de alto nivel**, como `ConfiguracionException`, que informe que el fallo está relacionado con la carga de configuración:

```java id="xv3q9p"
import java.io.FileReader;
import java.io.FileNotFoundException;

class ConfiguracionException extends Exception {
    public ConfiguracionException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {
    public static void cargarConfiguracion(String archivo) throws ConfiguracionException {
        try {
            FileReader f = new FileReader(archivo);
            // Lectura del archivo...
        } catch (FileNotFoundException e) {
            throw new ConfiguracionException("No se pudo cargar la configuración", e);
        }
    }

    public static void main(String[] args) {
        try {
            cargarConfiguracion("config.txt");
        } catch (ConfiguracionException e) {
            e.printStackTrace();
        }
    }
}
```

Al ejecutarse, `e.printStackTrace()` mostrará la **traza de la excepción principal**, seguida de la información de la causa (`Caused by`) con su propio tipo y mensaje. Esto permite ver de manera completa **qué error ocurrió originalmente y en qué contexto**, lo que facilita la depuración y análisis de fallos en sistemas más complejos. Por ejemplo, la salida indicará algo como:

```
ConfiguracionException: No se pudo cargar la configuración
    at EjemploCausa.cargarConfiguracion(EjemploCausa.java:...)
Caused by: java.io.FileNotFoundException: config.txt (No such file or directory)
    at java.io.FileReader.<init>(FileReader.java:...)
```

Así, se ve claramente la relación de **causa y efecto** entre excepciones.


