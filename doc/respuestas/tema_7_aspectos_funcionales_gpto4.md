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

Aquí tienes las respuestas a este segundo bloque sobre programación funcional y lambdas, siguiendo el formato solicitado para copiar y pegar.

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un **puntero a función** en C es una variable que almacena la dirección de memoria donde reside el código ejecutable de una función. A diferencia de un puntero normal que apunta a datos, este permite invocar dinámicamente diferentes comportamientos en tiempo de ejecución. Es la base técnica para implementar *callbacks* y polimorfismo en lenguajes de bajo nivel.



```c
#include <stdio.h>
#include <ctype.h>

void a_mayus(char* s) {
    while (*s) {
        *s = toupper((unsigned char)*s);
        s++;
    }
}

int main() {
    char texto[] = "hola mundo";
    // Definición del puntero a función
    void (*aMayusculas)(char*) = a_mayus;
    
    // Invocación a través del puntero
    aMayusculas(texto);
    printf("%s\n", texto);
    return 0;
}
```

---

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una **función lambda** es una función anónima (sin nombre) que se define en el lugar donde se necesita. Se caracterizan por su sintaxis compacta y por poder ser tratadas como datos: se pueden asignar a variables, pasar como argumentos o devolver como resultados. A diferencia de las funciones tradicionales, suelen ser más eficientes de escribir para tareas breves.

En **JavaScript** (usando *arrow functions*):
```javascript
const aMayusculas = (str) => str.toUpperCase();
console.log(aMayusculas("hola"));
```

En **Java** (usando la interfaz `Function`):
```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));
```

---

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El **paradigma funcional** es un estilo de programación basado en el uso de funciones matemáticas puras, evitando el cambio de estado y los datos mutables. Se centra en el "qué hacer" (declarativo) más que en el "cómo hacerlo" paso a paso (imperativo). Java 8 se considera **multi-paradigma** porque, aunque mantiene su esencia de Objetos, incorporó herramientas funcionales (lambdas, streams), permitiendo al desarrollador elegir el mejor enfoque según el problema.

Decir que las funciones son **"ciudadanos de primera clase"** (*first-class citizens*) significa que el lenguaje las trata como a cualquier otra variable: pueden ser pasadas como parámetros a otras funciones, ser el valor de retorno de una función y ser asignadas a variables o almacenadas en estructuras de datos.

---

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis de una lambda en Java consta de tres partes principales: la **lista de parámetros**, el **operador flecha (`->`)** y el **cuerpo de la función**.

1.  **Parámetros**: Se ponen entre paréntesis `(a, b)`. Si solo hay uno, se pueden omitir los paréntesis `s ->`.
2.  **Flecha**: Actúa como separador.
3.  **Cuerpo**: Si es una sola instrucción, no necesita llaves ni `return`. Si tiene varias líneas, se encierra en `{}` y requiere `return` explícito si devuelve algo.

> Ejemplo: `(x, y) -> x + y` es equivalente a un método que recibe dos números y devuelve su suma.

---

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Este concepto se conoce como **función de orden superior**. El método `transformar` no sabe qué hará con la cadena; simplemente "confía" en la función que recibe para procesarla.

En **JavaScript**:
```javascript
function transformar(texto, funcionRef) {
    return funcionRef(texto);
}
const aMayusculas = s => s.toUpperCase();
console.log(transformar("hola", aMayusculas));
```

En **Java**:
```java
public static String transformar(String texto, Function<String, String> funcionRef) {
    return funcionRef.apply(texto);
}
// Uso:
Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(transformar("hola", aMayusculas));
```

---

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Esta es la forma más común de usar lambdas, ya que evitamos crear variables intermedias si la lógica solo se va a usar una vez.

En **JavaScript**:
```javascript
console.log(transformar("mundo", s => s.split('').reverse().join('')));
```

En **Java**:
```java
System.out.println(transformar("mundo", s -> new StringBuilder(s).reverse().toString()));
```

En ambos casos, la lógica de inversión se define "al vuelo" (*inline*) dentro de los paréntesis del método `transformar`.

---

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un **cierre** o *closure* es la capacidad de una función lambda de "recordar" y acceder a las variables que estaban presentes en su entorno cuando fue creada, incluso si ese entorno ya ha terminado de ejecutarse. En Java, estas variables externas deben ser **finales** o **efectivamente finales** (que no cambien de valor tras ser inicializadas).



```java
String saludo = "Sr/Sra "; // Variable local externa
// La lambda "captura" la variable 'saludo'
Function<String, String> concatenador = s -> saludo + s;

System.out.println(transformar("García", concatenador));
```
En este caso, la lambda lleva consigo el valor de `saludo` para usarlo cuando se invoque dentro del método `transformar`.

---

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia fundamental radica en el **estado**. Un puntero a función en C es puramente una dirección de memoria; no tiene "memoria" propia de los datos que lo rodeaban al definirse. Si necesitas usar una variable externa en C, debes pasarla explícitamente como un parámetro adicional.

Las lambdas, gracias a las **closures**, empaquetan tanto el código como el contexto de datos (las variables capturadas). Esto las hace mucho más potentes: una lambda es un objeto que contiene comportamiento y estado, mientras que un puntero a función es solo una instrucción de salto en la CPU. Además, las lambdas ofrecen seguridad de tipos, mientras que los punteros en C son propensos a errores de segmentación.

---

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Aquí la función `crearDescuento` actúa como una "fábrica" de funciones personalizadas:

```java
public static Function<Double, Double> crearDescuento(double porcentaje) {
    return cantidad -> cantidad - (cantidad * porcentaje / 100);
}

// Uso:
Function<Double, Double> diezPorCiento = crearDescuento(10);
Function<Double, Double> mitadPrecio = crearDescuento(50);

System.out.println(diezPorCiento.apply(100.0)); // Imprime 90.0
System.out.println(mitadPrecio.apply(100.0));   // Imprime 50.0
```

**Explicación de la closure:** Cuando llamamos a `crearDescuento(10)`, la variable `porcentaje` (valor 10) queda "atrapada" dentro de la lambda que se devuelve. Aunque la función `crearDescuento` termine y salga de la pila, la función `diezPorCiento` sigue teniendo acceso a ese `10` específico porque forma parte de su *closure*.

---

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una **interfaz funcional** es una interfaz que tiene **exactamente un método abstracto**. Es el "contrato" que define la firma de la lambda (qué recibe y qué devuelve). Aunque puede tener múltiples métodos `default` o `static`, solo puede tener un único método pendiente de implementar.

Para ayudar al compilador y a otros desarrolladores, se suele marcar con la anotación `@FunctionalInterface`. Esto asegura que si alguien intenta añadir un segundo método abstracto, el código no compilará, protegiendo así la compatibilidad con expresiones lambda.

---

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Definir nuestra propia interfaz nos permite dar nombres más significativos al dominio de nuestro problema en lugar de usar los genéricos de Java.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}

// Ejemplo de uso:
Transformador aMayus = s -> s.toUpperCase();
System.out.println(aMayus.transformar("hola"));
```

---

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Al añadir parámetros de tipo `<T, R>`, hacemos que la interfaz sea universal para cualquier conversión:

```java
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    R ejecutar(T entrada);
}

// Redondear Double a Integer:
TransformadorGenerico<Double, Integer> redondeador = d -> (int) Math.round(d);
Integer resultado = redondeador.ejecutar(9.99); // Devuelve 10
```

---

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java ofrece un paquete llamado `java.util.function` con las formas más comunes:

| Interfaz | Firma | Descripción |
| :--- | :--- | :--- |
| `Predicate<T>` | `T -> boolean` | Comprueba una condición. |
| `Function<T, R>` | `T -> R` | Transforma un dato en otro. |
| `Consumer<T>` | `T -> void` | Consume un dato (ej. imprimirlo). |
| `Supplier<T>` | `() -> T` | Provee un dato (sin entrada). |
| `UnaryOperator<T>`| `T -> T` | Transforma un dato en otro del mismo tipo. |
| `BiFunction<T, U, R>`| `(T, U) -> R` | Transforma dos datos en uno. |

---

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` acepta un `Consumer`. Es una forma declarativa de iterar, donde no manejamos índices ni el control del bucle, solo la acción a realizar por cada elemento.

```java
List<Integer> numeros = Arrays.asList(-2, 5, 0, 10, -8);

numeros.forEach(n -> {
    if (n > 0) {
        System.out.println(n + " es positivo");
    }
});
```

---

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
Se usa `? super T` para permitir la **flexibilidad**. Si tienes una lista de `String`, podrías querer usar un consumidor que sirva para cualquier `Object`. Con `Consumer<? super T>`, Java te lo permite.

**PECS** significa *Producer Extends, Consumer Super*:
*   Si una estructura "produce" datos (los lees), usa `<? extends T>`.
*   Si una estructura "consume" datos (le escribes), usa `<? super T>`.

En el método `transformar(T valor, Function<? super T, ? extends R> f)`, usamos `? super T` para la entrada de la función porque la función puede ser capaz de manejar tipos más generales que el dato concreto, y `? extends R` para la salida porque queremos que el resultado sea, como mínimo, el tipo esperado.

---

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos son "atajos" para escribir lambdas que simplemente llaman a un método existente.

En **JavaScript**:
```javascript
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log(`Hola, soy ${this.nombre}`); }
}
const p = new Persona("Ana");
const referencia = p.saludar.bind(p); // Necesita bind para no perder el 'this'
referencia();
```

En **Java**:
```java
class Persona {
    String nombre;
    Persona(String n) { this.nombre = n; }
    void saludar() { System.out.println("Hola, soy " + nombre); }
}
//...
Persona p = new Persona("Ana");
Runnable referencia = p::saludar; // Referencia a método de instancia
referencia.run();
```

---

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
Java permite cuatro tipos de referencias usando el operador `::`:

1.  **Estático**: `Math::abs` (equivale a `n -> Math.abs(n)`).
2.  **Constructor**: `ArrayList::new` (equivale a `() -> new ArrayList()`).
3.  **Instancia concreta**: `miObjeto::unMetodo` (visto en el ejemplo de Persona).
4.  **Instancia arbitraria de un tipo**: `String::toUpperCase`. Aquí la lambda recibe el objeto como primer parámetro: `(String s) -> s.toUpperCase()`.



---

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
**Versión 1: Manual (Lógica interna)**
```java
Collections.sort(personas, (p1, p2) -> {
    int res = Integer.compare(p1.getEdad(), p2.getEdad());
    if (res == 0) {
        res = p1.getNombre().compareTo(p2.getNombre());
    }
    return res;
});
```

**Versión 2: Usando `Comparator` (Más legible/funcional)**
```java
personas.sort(
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```
La segunda versión es preferible porque es mucho más declarativa ("ordena por edad, luego por nombre") y aprovecha las referencias a métodos para mayor claridad.</T></T,></T></T></T></T,></T></T,></T,></Double,></String,>