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

### La abstracción consiste en centrarse solo en los aspectos importantes de un objeto, ignorando los detalles innecesarios. Permite trabajar con modelos simples de problemas complejos.

La encapsulación agrupa datos y métodos dentro de una misma entidad, protegiendo la información interna. Así se evita que los datos se modifiquen de forma incorrecta desde fuera.

La herencia permite crear nuevas clases a partir de otras existentes, reutilizando código. Esto facilita la organización y evita repetir funcionalidades.

El polimorfismo permite que un mismo método se comporte de forma diferente según el objeto que lo use. Gracias a esto, el código es más flexible.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Uno de los lenguajes más conocidos es Java, muy usado en aplicaciones empresariales y Android. Está basado completamente en la programación orientada a objetos.

C++ es otro lenguaje popular que combina programación estructurada y orientada a objetos. Se utiliza mucho cuando se necesita alto rendimiento.

Python también soporta la programación orientada a objetos y es muy usado por su facilidad de uso. Es común en ciencia de datos y desarrollo web.

Por último, C# es un lenguaje orientado a objetos muy usado en el entorno de Microsoft y en el desarrollo de videojuegos.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### La programación estructurada organiza el código usando estructuras como condicionales y bucles, evitando saltos desordenados. Su objetivo es que los programas sean más claros y fáciles de entender.

Este paradigma divide el programa en bloques que se ejecutan de forma ordenada. Lenguajes como C son un ejemplo de este tipo de programación.

La programación modular divide el programa en partes independientes llamadas módulos. Cada módulo tiene una función concreta y puede reutilizarse.

Gracias a esto, los programas son más fáciles de mantener y ampliar.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### El primer elemento es la identidad, que hace que cada objeto sea único dentro del sistema, aunque tenga los mismos datos que otro.

El segundo elemento son los atributos, que representan el estado del objeto. Guardan la información que describe al objeto.

El tercer elemento son los métodos, que definen lo que el objeto puede hacer. A través de ellos, el objeto realiza acciones.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Una clase es un molde que define cómo serán los objetos, indicando sus atributos y métodos. No representa algo concreto.

Un objeto es una entidad real creada a partir de una clase. Por tanto, una clase no es lo mismo que un objeto.

Una instancia es un objeto concreto creado desde una clase. Cada instancia puede tener valores diferentes.

No todos los lenguajes orientados a objetos usan clases de la misma forma; algunos usan otros modelos como prototipos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### En muchos lenguajes, los objetos se almacenan en el heap, una zona de memoria dinámica. Las referencias a esos objetos suelen estar en la pila.

No todos los lenguajes gestionan la memoria igual. Algunos, como Java, lo hacen automáticamente, mientras que otros dan más control al programador.

La recolección de basura es un mecanismo que libera la memoria de objetos que ya no se usan. Esto evita errores y fugas de memoria.

Gracias a ella, el programador no necesita liberar la memoria manualmente.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Un método es una función definida dentro de una clase que indica el comportamiento de un objeto. Permite trabajar con sus atributos.

La sobrecarga de métodos consiste en tener varios métodos con el mismo nombre pero con distintos parámetros. El lenguaje elige cuál usar según los argumentos.

Esto hace que el código sea más claro y fácil de usar. Además, permite reutilizar nombres de métodos para acciones similares.

Es una característica común en lenguajes como Java.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        System.out.println(p.calculaDistanciaAOrigen());
    }
}



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### El punto de entrada de un programa en Java es el método main. Es el primer método que se ejecuta cuando arranca el programa.

La palabra static indica que el método pertenece a la clase y no a un objeto. Por eso Java puede ejecutar main sin crear una instancia.

static también se usa en otros métodos o atributos que son compartidos por todos los objetos. No se usa solo en main.

Cuando se combina con final, se indica que el valor no puede cambiar, por ejemplo en constantes.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Para compilar un programa Java se usa el comando javac, que genera un archivo .class. Para ejecutarlo se usa el comando java.

Java no ejecuta directamente el código fuente, sino el archivo compilado. Por eso se dice que es un lenguaje compilado.

El código compilado se llama byte-code y no depende del sistema operativo. Esto permite que funcione en distintos sistemas.

La máquina virtual de Java (JVM) es la encargada de ejecutar ese byte-code en cada sistema.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### La palabra clave new se utiliza en Java para crear un nuevo objeto a partir de una clase. Al usar new, se reserva memoria y se inicializa el objeto para poder utilizarlo.

Un constructor es un método especial que se ejecuta automáticamente cuando se crea un objeto. Tiene el mismo nombre que la clase y sirve para dar valores iniciales a los atributos.

Un ejemplo de constructor en una clase Empleado sería el siguiente:
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### La referencia this apunta al objeto actual, es decir, a la instancia que está ejecutando el método en ese momento. Se usa para acceder a los atributos y métodos de ese objeto.

No se llama igual en todos los lenguajes. Por ejemplo, en Python se utiliza self, aunque cumple la misma función.

Ejemplo de uso de this en la clase Punto:
double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### El método distanciaA sirve para calcular la distancia entre el punto actual y otro punto recibido como parámetro. Para ello, se usan las coordenadas de ambos puntos.

El objeto actual se representa con this, mientras que el otro punto se recibe como argumento del método.

Un posible método sería el siguiente:
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### En Java, los objetos se pasan por copia de la referencia. Esto significa que si se modifican los atributos del objeto dentro del método, los cambios sí afectan al objeto original.

Sin embargo, los tipos primitivos como int se pasan por copia del valor. Si se modifica un int dentro del método, el valor original no cambia fuera.

Por tanto, los cambios en objetos se reflejan fuera del método, pero los cambios en tipos primitivos no.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### El método toString() devuelve una representación en texto de un objeto. Java lo usa automáticamente cuando se imprime un objeto por pantalla.

Existen métodos equivalentes en otros lenguajes, como __str__ en Python, aunque el nombre cambia.

Un ejemplo de toString() en la clase Punto sería:
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


###Una clase es parecida a un struct en C porque ambos agrupan datos relacionados en una misma estructura.

Sin embargo, al struct le faltan características importantes como métodos asociados, encapsulación, herencia y polimorfismo.

Por eso, un struct solo almacena datos, mientras que una clase representa datos y comportamiento.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### En C, se puede imitar la clase Punto usando un struct para los datos y una función externa para el comportamiento.

La función recibe el struct como parámetro, ya que en C no existe el concepto de métodos asociados a un tipo.
