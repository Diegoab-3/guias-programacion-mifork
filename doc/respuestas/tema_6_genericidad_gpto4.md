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

Aquí tienes las respuestas a tus preguntas, estructuradas y listas para copiar y pegar según el formato solicitado.

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
En Java, la clase raíz de toda la jerarquía de objetos es `Object`. Gracias al polimorfismo, una referencia de tipo `Object` puede apuntar a cualquier instancia de una clase. Para crear una estructura de datos "universal" usando un array primitivo (en el sentido de estructura básica de lenguaje), simplemente declararíamos un array de tipo `Object[]`. Al hacer esto, el array no guarda los datos directamente si son tipos primitivos (estos sufren *autoboxing* a sus clases envolventes como `Integer`), sino que guarda referencias a objetos de cualquier clase.

A continuación, un ejemplo sencillo de un contenedor que utiliza este principio:

```java
public class ContenedorUniversal {
    private Object[] elementos;
    private int capacidad;

    public ContenedorUniversal(int tam) {
        this.elementos = new Object[tam];
    }

    public void insertar(int index, Object dato) {
        elementos[index] = dato;
    }

    public Object obtener(int index) {
        return elementos[index];
    }
}
```

En este ejemplo, el array `elementos` puede alojar simultáneamente un `String`, un `Integer` o cualquier objeto personalizado. Sin embargo, el lenguaje pierde la noción del tipo específico de lo que hay dentro; el programador es quien debe recordar qué guardó en cada posición para recuperarlo correctamente.

---

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** es un paradigma de programación centrado en el diseño de algoritmos y estructuras de datos que pueden trabajar con diferentes tipos de datos sin conocerlos de antemano, manteniendo la seguridad de tipos (*type safety*). Su objetivo principal es la reutilización de código: escribir una lógica una sola vez (por ejemplo, un algoritmo de ordenación o una lista enlazada) y que funcione de forma eficiente tanto para números como para cadenas o tipos definidos por el usuario.

Respecto al ejemplo anterior con `Object[]`, **no se considera programación genérica pura**, sino más bien un uso de polimorfismo por herencia. Aunque permite almacenar cualquier dato, carece de la característica esencial de los genéricos: el chequeo en tiempo de compilación. En el ejemplo de `Object`, el compilador no sabe qué tipo de dato "específico" se espera recuperar, lo que obliga al programador a realizar conversiones manuales y arriesgarse a errores en tiempo de ejecución.

---

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El problema fundamental es la **pérdida de seguridad de tipos en tiempo de compilación**. Cuando usamos `Object` o `void*`, el compilador "se desentiende" de la naturaleza real del dato. Esto nos obliga a realizar un **downcasting** (conversión explícita a un tipo más específico) cada vez que recuperamos un elemento de la estructura. Si por error intentamos convertir un `Integer` que estaba guardado en el array a un `String`, el programa fallará con una `ClassCastException` en Java o un error de acceso a memoria en C, pero el compilador no nos habrá avisado antes.



Además, este enfoque ensucia el código y lo hace propenso a errores humanos. Al no haber restricciones, es posible insertar accidentalmente una `Coche` en una lista que se supone que solo debía contener `Persona`. La programación genérica moderna busca precisamente trasladar estos errores del **tiempo de ejecución** (cuando el usuario final usa la app) al **tiempo de compilación** (mientras el desarrollador escribe el código), haciendo que el software sea mucho más robusto.

---

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los **parámetros de tipo** son variables especiales que no representan valores (como un `int x = 5`), sino que representan **tipos de datos**. Se comportan como "huecos" o etiquetas que se colocan en la definición de una clase, interfaz o método. Por convención, se suelen representar con letras mayúsculas simples como `T` (Type), `E` (Element) o `K` (Key). Al definir una clase como `Lista<T>`, le estamos diciendo al compilador: "esta clase trabajará con algún tipo T que el programador decidirá más adelante".

Cuando el programador utiliza la clase, por ejemplo escribiendo `Lista<String>`, el parámetro de tipo `T` se sustituye (conceptualmente) por `String`. Esto permite que el compilador verifique que todos los elementos añadidos sean efectivamente cadenas y que, al extraerlos, el tipo de retorno sea `String` de forma automática, eliminando la necesidad de castings manuales y garantizando que la estructura sea homogénea y segura.

---

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En **Java**, utilizamos la clase `ArrayList<T>` de la biblioteca estándar. Al especificar el tipo entre diamantes `<String>`, el compilador bloquea cualquier intento de añadir algo que no sea una cadena. Al iterar, no necesitamos convertir el objeto:

```java
// Java
ArrayList<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");

for (String s : lista) { // s ya es String, no requiere cast
    System.out.println(s.toUpperCase()); 
}
```

En **C++**, utilizamos la clase `std::vector<T>` de la STL (Standard Template Library). El funcionamiento es similar: el compilador genera código específico para manejar cadenas, garantizando que el acceso sea seguro y directo:

```cpp
// C++
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> lista;
lista.push_back("Hola");
lista.push_back("Mundo");

for (const std::string& s : lista) {
    std::cout << s.length() << std::endl;
}
```

---

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
C++ y Java gestionan los genéricos de formas radicalmente opuestas. En **C++**, se utiliza la **instanciación de plantillas**: el compilador genera una copia real del código para cada tipo distinto que utilices. Si usas `Vector<int>` y `Vector<double>`, tendrás en tu ejecutable final dos versiones distintas de la clase. Esto es extremadamente eficiente en ejecución, pero aumenta el tamaño del binario.

En **Java**, se utiliza el **Type Erasure** (borrado de tipos). Para mantener la compatibilidad con versiones antiguas, el compilador de Java elimina toda la información de los genéricos tras comprobar la seguridad del código. Convierte todos los tipos `T` en `Object` (o en su límite superior) e inserta automáticamente los *casts* necesarios. Esto significa que, en tiempo de ejecución, una `List<String>` y una `List<Integer>` son exactamente la misma clase, lo cual ahorra memoria pero impone ciertas limitaciones técnicas.

---

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
La clase `Par` es un ejemplo clásico de cómo los genéricos permiten crear estructuras versátiles. Aquí definimos dos parámetros de tipo, `T` y `U`, para que los dos elementos puedan ser de tipos distintos o iguales:

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
```

A continuación, vemos cómo se usaría para devolver dos resultados de una función estadística. En este caso, ambos valores serán `Double`:

```java
public Par<Double, Double> calcularEstadisticas(double[] datos) {
    double media = 0; // ... lógica de cálculo
    double desv = 0;  // ... lógica de cálculo
    return new Par<>(media, desv);
}

// Uso:
Par<Double, Double> resultado = calcularEstadisticas(meuArray);
System.out.println("Media: " + resultado.getPrimero());
```

---

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
Si definimos el método usando `Object`, perdemos el control del tipo. Podríamos pasarle un `Coche` y un `Libro`, y el resultado sería un `Object` que tendríamos que convertir manualmente, asumiendo el riesgo de error. Con genéricos, el método se define así:

```java
public <T> T seleccionaUno(T a, T b) {
    return Math.random() > 0.5 ? a : b;
}
```

La diferencia es clave: (i) **Evitar downcasting**: Si llamamos a `String s = seleccionaUno("Hola", "Adiós")`, el compilador sabe que el retorno es `String`. No hay que escribir `(String)`. (ii) **Forzar mismo tipo**: Si intentamos hacer `seleccionaUno("Hola", 5)`, el compilador detectará que los tipos no coinciden (o buscará el ancestro común más cercano), impidiendo mezclar tipos de forma no intencionada, algo que con `Object` sería perfectamente válido.

---

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, es posible mediante las **cláusulas de acotación** (*bounded type parameters*). En Java se utiliza la palabra clave `extends`. 

**Solución 1 (Sin generics):** Usando la clase base `Number`. El problema es que `getX()` devuelve `Number`, y no sabemos si es un `Integer` o un `Double`.
```java
public class Punto {
    private Number x, y;
    public Number getX() { return x; }
    // ...
}
```

**Solución 2 (Con generics acotados):** Usamos `<T extends Number>`. Esto garantiza que `T` será `Number` o una subclase.
```java
public class Punto<T extends Number> {
    private T x, y;
    public T getX() { return x; }

    public double calcularDistanciaA(Punto<T> otro) {
        return Math.sqrt(Math.pow(x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(y.doubleValue() - otro.y.doubleValue(), 2));
    }
}
```

Respecto al **Type Erasure**, debido a que hemos puesto un límite con `extends Number`, el compilador no sustituye `T` por `Object`, sino por **`Number`**. Este es el tipo final que aparecerá en el *bytecode* generado.

---

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
En la solución **sin generics** (usando `Number` directamente), sí es posible crear un punto con una coordenada `Integer` y otra `Double`, ya que ambas son `Number`. Sin embargo, esto suele ser indeseable en cálculos precisos. En la solución **con generics** (`Punto<T>`), obligamos a que ambas coordenadas compartan el mismo tipo `T`. Si instanciamos un `Punto<Double>`, ambas coordenadas deben ser obligatoriamente `Double`.

En cuanto a los retornos: en la solución sin genéricos, `getX()` devuelve siempre una referencia de tipo `Number`. Si queremos usar métodos específicos de `Integer`, necesitamos un cast. En la solución con genéricos, si tenemos un `Punto<Integer>`, el método `getX()` devuelve directamente un `Integer`. El compilador ya conoce el tipo exacto, lo que hace que el código sea más limpio, seguro y eficiente para el desarrollador.

---

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

### Respuesta
Para lograr que un punto solo pueda calcular la distancia contra otro punto de su misma clase, utilizamos lo que se conoce como un **parámetro de tipo recursivo**. La interfaz debe recibir un tipo `P` que extienda de la propia interfaz.

```java
// Interfaz genérica que se refiere a sí misma
public interface Punto<P extends Punto<P>> { 
    public double distanciaA(P p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    public Punto2D(double x, double y) { this.x = x; this.y = y; } 

    @Override 
    public double distanciaA(Punto2D p2d) { // Recibe Punto2D directamente
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z;
    // ... constructor ...
    @Override 
    public double distanciaA(Punto3D p3d) { // Recibe Punto3D directamente
        return Math.sqrt(Math.pow(x - p3d.x, 2) + Math.pow(y - p3d.y, 2) + Math.pow(z - p3d.z, 2)); 
    } 
}
```

Con este diseño, si intentas pasar un `Punto3D` al método `distanciaA` de un `Punto2D`, el código simplemente no compilará. Hemos eliminado por completo el `instanceof` y el cast manual, dejando que el sistema de tipos garantice la corrección.

---

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
En Java, los genéricos son **Invariantes**, lo que significa que `List<String>` **no** es subtipo de `List<Object>`. No puedes asignar una a la otra. Esto es así para proteger la integridad de la lista: si pudiéramos tratar una `List<String>` como `List<Object>`, podríamos intentar insertar un `Integer` en ella, rompiendo la promesa de que la lista solo contiene cadenas.

Sin embargo, los arrays en Java son **Covariantes**: `String[]` **sí** es subtipo de `Object[]`. Esto permite que el código compile, pero genera un problema de seguridad: si asignamos un `String[]` a una referencia `Object[]` e intentamos guardar un `Integer` en la primera posición, el compilador no dirá nada, pero en tiempo de ejecución obtendremos una `ArrayStoreException`.

**Definiciones:**
*   **Covariante:** Si $A$ es subtipo de $B$, entonces $Contenedor<A>$ es subtipo de $Contenedor<B>$ (Ej: Arrays).
*   **Invariante:** No hay relación de subtipeado entre los contenedores, sin importar la relación entre sus elementos (Ej: Generics de Java).
*   **Contravariante:** La relación se invierte: $Contenedor<B>$ sería subtipo de $Contenedor<A>$.

---

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un **wildcard** representa un tipo desconocido. Se utiliza para dar flexibilidad a los métodos genéricos.
*   `<? extends T>` (**Covarianza/Lectura**): Permite cualquier tipo que sea `T` o hijo de `T`. Se usa cuando solo vas a **leer** datos de la estructura, ya que sabes que lo que salga será, como mínimo, un `T`.
*   `<? super T>` (**Contravarianza/Escritura**): Permite cualquier tipo que sea `T` o padre de `T`. Se usa cuando vas a **escribir** o añadir datos a la estructura, ya que es seguro meter un `T` en una lista de sus ancestros.



Aquí los ejemplos solicitados:

```java
// (i) Uso de ? extends para LECTURA (Suma de números)
public double sumarLista(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) { // Es seguro leer como Number
        total += n.doubleValue();
    }
    return total;
}

// (ii) Uso de ? super para ESCRITURA (Añadir enteros)
public void añadirEnteros(List<? super Integer> lista) {
    lista.add(10); // Es seguro añadir Integer
    lista.add(20);
}
```

Esta regla se conoce como **PECS** (*Producer Extends, Consumer Super*): usa `extends` cuando la estructura actúe como productora de datos y `super` cuando actúe como consumidora.</A></B></B></A></Object></String></Object></String></Object></String></Integer></Double></T></T></T></Integer></String></T></String></T></String></T>