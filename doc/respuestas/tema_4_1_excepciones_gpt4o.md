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


1. En C, podemos crear estructuras mayores componiendo unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando struct, de una línea de puntos, donde puntos tienen dos coordenadas (x e y), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.
Respuesta

En C podemos modelar la composición utilizando struct, donde una estructura contiene otras. En este caso, definimos una estructura Punto con dos coordenadas (x, y) y una estructura Linea que contiene dos puntos (p1 y p2). Esto refleja claramente la relación "una línea tiene dos puntos".

#include <stdio.h>
#include <math.h>

typedef struct {
    float x;
    float y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

float distancia(Punto a, Punto b) {
    return sqrt(pow(a.x - b.x, 2) + pow(a.y - b.y, 2));
}

float longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

La función distancia calcula la distancia euclídea entre dos puntos usando la fórmula matemática estándar. La función longitudLinea reutiliza esta función para calcular la longitud de la línea, demostrando cómo la composición permite reutilizar lógica de forma estructurada.

2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de composición en orientación a objetos. Crea una clase Punto, y una clase Linea. La clase Punto debe tener un método para calcular distancia a otro Punto y Linea debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.
Respuesta

En Java, la composición se implementa mediante clases que contienen otras clases como atributos. Para garantizar la inmutabilidad, declaramos los atributos como final y no proporcionamos métodos que los modifiquen. Así evitamos cambios una vez creados los objetos.

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(x - otro.x, 2) + Math.pow(y - otro.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

La encapsulación impide modificar los puntos desde fuera, lo que mejora la seguridad respecto a C. Además, la inmutabilidad hace que los objetos sean más fáciles de razonar y evita errores por cambios inesperados.

3. ¿Qué significa la multiplicidad en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre Linea y Punto? Indícalo expresando la multiplicidad en ambas direcciones, de Linea a Punto y de Punto a Linea.
Respuesta

La multiplicidad en composición indica cuántas instancias de una clase están relacionadas con otra. Es una forma de expresar las cardinalidades en una relación entre objetos, como uno a uno, uno a muchos, etc.

En el ejemplo de Linea y Punto, una Linea está compuesta exactamente por dos puntos. Por tanto, la multiplicidad de Linea a Punto es 2. Esto significa que cada instancia de Linea tiene exactamente dos instancias de Punto.

En sentido inverso, un Punto puede pertenecer a cero, una o varias líneas. Por tanto, la multiplicidad de Punto a Linea es 0..*, ya que un mismo punto podría ser compartido por muchas líneas o no pertenecer a ninguna.

4. ¿Qué significa composición fuerte y composición débil? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como "asociación o agregación" y a cuál como "composición" propiamente.
Respuesta

La composición fuerte implica que los objetos contenidos dependen completamente del objeto contenedor. Es decir, su ciclo de vida está ligado: si el objeto contenedor se destruye, los objetos contenidos también dejan de existir.

Por otro lado, la composición débil (también llamada agregación o asociación) implica que los objetos pueden existir de forma independiente. El contenedor simplemente mantiene una referencia, pero no controla su ciclo de vida.

En la práctica, solemos llamar composición a la relación fuerte, y agregación o asociación a la relación débil. Esta diferencia es importante en el diseño de software porque afecta a la gestión de memoria y a la responsabilidad de creación y destrucción de objetos.

5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer new dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de "dependencia"?
Respuesta

En estos casos hablamos de dependencia, no de composición. La dependencia se refiere a que una clase utiliza otra de forma puntual, pero no mantiene una relación estructural permanente.

Por ejemplo, si una clase recibe otra como parámetro o crea un objeto dentro de un método, solo está usando esa clase temporalmente. No la almacena como atributo ni forma parte de su estado interno.

La composición, en cambio, implica que una clase contiene a otra como parte de su estructura. Por eso, la dependencia es una relación más débil y menos duradera que la composición.

6. En el ejemplo anterior de línea y punto, programa la relación entre Linea y Punto de dos formas. Una como composición fuerte, donde el ciclo de vida de los puntos está ligado al de Linea y otra como composición débil, donde no.
Respuesta

En composición fuerte, la línea crea internamente los puntos, por lo que su ciclo de vida depende de ella:

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

En este caso, los puntos no existen fuera de la línea, ya que se crean dentro de ella.

En composición débil, los puntos se crean fuera y se pasan a la línea:

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}

Aquí los puntos pueden existir independientemente de la línea y ser compartidos con otras instancias.

7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que Linea destruya los Punto explícitamente, ¿Por qué?
Respuesta

En Java no existe destrucción explícita de objetos como en C o C++. En su lugar, se utiliza un recolector de basura (garbage collector) que libera la memoria automáticamente cuando un objeto ya no tiene referencias.

En composición fuerte, cuando el objeto contenedor deja de ser accesible, también lo hacen los objetos que contiene. Esto significa que los puntos quedarán sin referencias y el recolector de basura los eliminará.

Por eso no es necesario que Linea destruya explícitamente los Punto. La gestión de memoria está automatizada, lo que reduce errores pero también quita control directo al programador.

8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores...
Respuesta

En este caso, usamos una agregación: el departamento mantiene referencias a profesores que existen independientemente. Además, garantizamos que siempre haya un director y que pertenezca al conjunto de profesores.

public class Profesor {
    private String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }
}

public class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) throw new IllegalArgumentException();
        this.director = director;
        profesores[numProfesores++] = director;
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) throw new IllegalStateException();
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (profesores[pos] == director)
            throw new IllegalStateException("No se puede eliminar al director");

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public void setDirector(Profesor p) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == p) encontrado = true;
        }
        if (!encontrado) throw new IllegalArgumentException();
        director = p;
    }
}

Se mantiene la invariante de que el director siempre pertenece al departamento. Además, se protege la estructura interna evitando exponer directamente el array.

9. En Java, existen también List...
Respuesta

Usando List, el código se simplifica notablemente porque no tenemos que gestionar manualmente el tamaño ni los desplazamientos:

import java.util.*;

public class Departamento {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores.add(director);
    }
}

Nos ahorramos toda la lógica de gestión del array (límites, desplazamientos, contadores). Esto reduce errores y hace el código más limpio.

Si devolviéramos directamente la lista interna, romperíamos la encapsulación, ya que el cliente podría modificarla. Para evitarlo, podemos devolver una copia o una vista inmutable:

public List<Profesor> getProfesores() {
    return Collections.unmodifiableList(profesores);
}
10. Al igual que ocurre con las excepciones en Java...
Respuesta

Un ejemplo de composición recursiva es una clase Persona que contiene una referencia a su madre, que también es una Persona.

public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }
}

Ejemplo de uso:

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Maria", abuela);
        Persona hijo = new Persona("Juan", madre);
    }
}

Otros ejemplos clásicos incluyen árboles (como estructuras de directorios), listas enlazadas o expresiones matemáticas, donde cada elemento contiene otros del mismo tipo.

11. ¿Qué son las relaciones de composición "bidireccionales"?...
Respuesta

Las relaciones bidireccionales son aquellas en las que ambas clases mantienen referencias entre sí. Es decir, no solo el departamento conoce a sus profesores, sino que cada profesor también conoce a su departamento.

Esto implica que al modificar una relación en un lado, debemos actualizar el otro para mantener la coherencia. Por ejemplo, al añadir un profesor a un departamento, también debemos asignar ese departamento al profesor.

En el ejemplo, la clase Profesor tendría un atributo Departamento, y los métodos de Departamento deberían encargarse de mantener sincronizadas ambas referencias, evitando inconsistencias en el modelo.

MODO DESARROLLADOR


