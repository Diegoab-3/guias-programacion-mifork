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

1. En Programación Orientada a Objetos (POO), ¿Qué buscan la encapsulación y la ocultación de información? Enumera brevemente algunas ventajas de la ocultación de información.


La encapsulación en POO consiste en agrupar los datos (atributos) y los métodos que operan sobre esos datos dentro de una misma unidad llamada clase. Por su parte, la ocultación de información busca proteger los datos internos de una clase evitando que sean accesibles directamente desde fuera de la misma. Esto permite controlar cómo se modifican y se accede a los datos, evitando errores y usos indebidos.

Entre las ventajas de la ocultación de información se incluyen: primero, mayor seguridad, ya que los atributos internos no pueden ser alterados de forma arbitraria; segundo, facilidad de mantenimiento, porque los cambios internos no afectan directamente a otras partes del programa; tercero, claridad y simplicidad para el usuario de la clase, que solo necesita conocer la interfaz pública; y cuarto, ayuda a preservar las invariantes de la clase, asegurando que siempre se cumplan ciertas condiciones internas.

Además, la encapsulación y ocultación facilitan la reutilización de código y promueven un diseño más robusto y modular, donde cada clase es responsable de sus propios datos y comportamientos, reduciendo dependencias externas.

2. ¿Qué se entiende por la interfaz pública de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.


La interfaz pública de una clase es el conjunto de métodos y atributos accesibles desde fuera de la clase, que permiten a otros objetos interactuar con ella sin conocer los detalles internos de su implementación. Es la “cara visible” de la clase y define cómo se puede usar.

La relación con la ocultación de información es directa, ya que la interfaz pública permite exponer únicamente lo necesario para el usuario de la clase, mientras que los detalles internos permanecen privados. Esto asegura que los usuarios de la clase no dependan de su implementación interna, lo que facilita cambios futuros sin romper el código externo.

De esta manera, la interfaz pública actúa como un contrato: otros objetos saben qué pueden hacer con la clase, pero no cómo se hace internamente, protegiendo la integridad y consistencia de los datos.

3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la interfaz pública de una clase? ¿Es fácil cambiarla?

Diseñar la interfaz pública de una clase requiere cuidado porque representa el contrato que la clase ofrece a otros objetos. Una vez que la interfaz se usa en distintos lugares del programa, modificarla puede romper dependencias y generar errores en el código que la utiliza.

No es fácil cambiar la interfaz pública sin afectar a los clientes de la clase, especialmente en sistemas grandes o con muchos usuarios del código. Por ello, se recomienda definirla de forma clara, consistente y estable desde el inicio, exponiendo solo los métodos y atributos estrictamente necesarios.

Un buen diseño de la interfaz pública también ayuda a mantener la clase flexible y reusable, ya que facilita el mantenimiento y la evolución del código sin afectar a quienes dependen de ella.

4. ¿Qué son las invariantes de clase y por qué la ocultación de información nos ayuda?


Las invariantes de clase son condiciones o reglas que siempre deben cumplirse para que un objeto se considere válido. Por ejemplo, un atributo que representa una edad no puede ser negativo; esa restricción sería una invariante.

La ocultación de información ayuda a mantener estas invariantes porque impide que el estado interno del objeto sea modificado de forma arbitraria desde fuera. Solo los métodos de la clase pueden cambiar los atributos, y dentro de esos métodos se pueden implementar controles que aseguren que las invariantes se cumplan siempre.

Esto mejora la confiabilidad del código y reduce los errores, ya que los objetos no pueden entrar en estados inconsistentes, y facilita la comprensión y el mantenimiento del sistema.

5. Pon un ejemplo de una clase Punto en Java, con dos coordenadas, x e y, de tipo double, con un método calcularDistanciaAOrigen, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase Punto? ¿Qué significa public y private?

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x*x + y*y);
    }

    public double getX() {
        return x;
    }

    public void setX(double x) {
        this.x = x;
    }

    public double getY() {
        return y;
    }

    public void setY(double y) {
        this.y = y;
    }
}


La interfaz pública de la clase Punto incluye los métodos calcularDistanciaAOrigen(), getX(), setX(), getY() y setY(). Es decir, lo que otros objetos pueden usar para interactuar con un Punto.

El modificador public indica que un método o clase es accesible desde cualquier otra clase, mientras que private significa que solo puede ser accedido dentro de la propia clase, protegiendo así los datos internos.

6. En Java, ¿A quiénes se pueden aplicar los modificadores public o private?


En Java, los modificadores public y private se pueden aplicar a clases, métodos y atributos.

Cuando se aplica a una clase, public permite que sea accesible desde cualquier paquete, mientras que private no se puede aplicar directamente a la clase (solo anidada). Para los atributos y métodos, public hace que sean accesibles desde cualquier otra clase, y private los oculta, permitiendo que solo la propia clase los use.

Esto permite controlar la visibilidad de los componentes y mantener la encapsulación, garantizando que los detalles internos permanezcan protegidos mientras se expone únicamente la interfaz pública necesaria.

7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?


Sí, además de pública y privada, existen otros niveles de visibilidad. En Java, además de public y private, están protected (accesible dentro del mismo paquete y por subclases) y la visibilidad por defecto (package-private), accesible solo dentro del mismo paquete.

En otros lenguajes, existen variantes similares. Por ejemplo, en C++ se tiene public, private y protected, mientras que en Python la visibilidad se gestiona por convención usando guiones bajos para indicar atributos “protegidos” o “privados”, aunque no es estrictamente obligatorio.

Estos niveles adicionales permiten un control más fino sobre cómo se puede acceder a los miembros de una clase, protegiendo los datos y asegurando la integridad del objeto según el contexto.

8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método calcularDistanciaAPunto(Punto otro) y explica la respuesta.


Los miembros de instancia privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un objeto de la clase puede acceder a los atributos privados de otro objeto de la misma clase.

Ejemplo:

public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx*dx + dy*dy);
}


Aquí, aunque x e y son privados, podemos acceder a los de otro porque es otra instancia de la misma clase Punto. Esto demuestra que la privacidad se aplica a otras clases externas, no a otras instancias de la misma clase, permitiendo métodos que comparen objetos internamente.

9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?


Los métodos “getter” y “setter” son funciones que permiten acceder y modificar atributos privados de una clase de manera controlada. Un “getter” devuelve el valor de un atributo, mientras que un “setter” permite establecer un nuevo valor.

Su uso mantiene la encapsulación, ya que el acceso directo al atributo está restringido. Además, en los setters se pueden implementar validaciones antes de cambiar un valor, asegurando que las invariantes de la clase se mantengan.

Son la forma más habitual de exponer datos internos de manera segura, facilitando la evolución del código sin romper la interfaz pública.

10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?
Respuesta

No, la “seguridad” a la que nos referimos en POO no tiene que ver con ataques externos o hacking, sino con la protección de los datos internos del objeto frente a modificaciones accidentales o incorrectas.

Al ocultar información y controlar el acceso mediante métodos públicos, se asegura que los objetos permanezcan en estados válidos y coherentes. Esto evita errores lógicos, inconsistencias y efectos colaterales indeseados dentro del programa.

En resumen, la seguridad de la que hablamos es más sobre robustez y confiabilidad del software que sobre protección frente a ataques maliciosos.

11. ¿Qué diferencia hay entre miembro de instancia y miembro de clase? ¿Los miembros de clase también se pueden ocultar?
Respuesta

Los miembros de instancia son aquellos que pertenecen a un objeto en particular; cada instancia de la clase tiene sus propios valores de esos atributos. Por ejemplo, cada Punto tiene su x e y.

Los miembros de clase, por otro lado, son compartidos por todas las instancias de la clase. Se declaran con la palabra clave static en Java. Un ejemplo sería un contador de cuántos objetos de la clase se han creado.

Sí, los miembros de clase también se pueden ocultar usando private. Esto permite proteger los datos compartidos entre todas las instancias y controlar su acceso de manera segura mediante métodos public static.

12. Brevemente: ¿Tiene sentido que los constructores sean privados?
Respuesta

Sí, tiene sentido en ciertos casos, sobre todo cuando se quiere controlar la creación de objetos. Por ejemplo, en patrones de diseño como Singleton, donde solo debe existir una instancia de la clase, o en clases con métodos factoría.

Un constructor privado impide que otros objetos puedan crear instancias directamente, obligando a usar métodos específicos para obtenerlas. Esto permite mayor control sobre la inicialización y asegura que se cumplan invariantes de la clase.

Además, ayuda a encapsular la lógica de creación de objetos complejos, evitando que el usuario de la clase se preocupe por detalles internos de construcción.

13. ¿Cómo se indican los miembros de clase en Java? Pon un ejemplo, en la clase Punto definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores x e y máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.
Respuesta

En Java, los miembros de clase se declaran con la palabra clave static. Esto indica que pertenecen a la clase y no a instancias individuales.

Ejemplo en Punto:

public class Punto {
    private double x;
    private double y;
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}


Aquí, maxX y maxY son compartidos por todos los objetos y permiten consultar los valores máximos registrados sin necesidad de acceder a instancias concretas.

El uso de static y private garantiza que estos datos se gestionen de forma controlada, respetando la encapsulación.

14. Como sería un método factoría dentro de la clase Punto para construir un Punto a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado static?
Respuesta
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}


Sí, se usa static porque el método factoría pertenece a la clase, no a una instancia específica. Esto permite llamar al método sin necesidad de crear primero un objeto Punto.

El método se encarga de redondear las coordenadas y devuelve una nueva instancia, manteniendo la encapsulación al no exponer detalles internos de construcción directamente al usuario.

15. Cambia la implementación de Punto. En vez de dos double, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.
Respuesta
public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0]*coords[0] + coords[1]*coords[1]);
    }

    public double getX() { return coords[0]; }
    public void setX(double x) { coords[0] = x; }
    public double getY() { return coords[1]; }
    public void setY(double y) { coords[1] = y; }
}


Aquí, la interfaz pública se mantiene igual (getX, setX, getY, setY, calcularDistanciaAOrigen) aunque internamente usemos un array para almacenar las coordenadas.

Esto demuestra cómo se puede cambiar la implementación interna sin afectar a los usuarios de la clase, manteniendo la encapsulación y las invariantes de clase.

16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?
Respuesta

Aunque exista un getter y un setter público, no es recomendable declarar el atributo como público. Hacerlo rompería la encapsulación y permitiría cambios directos sin control, lo que puede comprometer las invariantes de la clase.

La convención habitual es declarar los atributos como private y exponerlos mediante métodos getter y setter que validen o controlen los cambios. Esto asegura que las invariantes se mantengan y que el estado interno del objeto sea consistente.

Por lo tanto, la protección de los atributos privados está directamente relacionada con mantener invariantes de clase, ya que evita modificaciones arbitrarias que puedan dejar al objeto en un estado inválido.

17. ¿Qué significa que una clase sea inmutable? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?
Respuesta

Una clase es inmutable si, una vez creada, su estado no puede cambiar. Todos sus atributos se establecen en el constructor y no hay métodos que puedan modificar su contenido.

Un método modificador es aquel que puede alterar el estado de un objeto. No todos los métodos modificadores son "setter"; un método que cambie varias propiedades internas también cuenta como modificador.

Las ventajas de la inmutabilidad incluyen simplificación de la concurrencia (no hay problemas de acceso simultáneo), mayor seguridad frente a errores y facilidad para razonar sobre el comportamiento del programa, ya que los objetos no cambian de forma inesperada.

18. ¿Es recomendable incluir métodos "setter" siempre y como convención?
Respuesta

No siempre es recomendable. Los setters deben incluirse únicamente si es necesario permitir la modificación de un atributo. Incluir setters innecesarios rompe la inmutabilidad y puede hacer que la clase sea más difícil de mantener.

En general, se aconseja limitar el acceso directo a los atributos y solo exponer setters cuando sea coherente con el diseño de la clase y las invariantes que se quieran mantener.

Esto ayuda a preservar la integridad del objeto y asegura que su estado no pueda alterarse de manera inadvertida desde fuera de la clase.

19. ¿La clase String en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?
Respuesta

La clase String en Java es inmutable. Esto significa que cualquier operación que modifique un String, como la concatenación, genera un nuevo objeto y no altera el original.

Al concatenar dos cadenas, se crea un nuevo String que contiene la combinación de ambas. El objeto original permanece intacto.

Si se van a hacer muchas concatenaciones, es más eficiente usar StringBuilder o StringBuffer, que son mutables y permiten modificar la cadena internamente sin crear un nuevo objeto en cada operación, mejorando el rendimiento.

20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?
Respuesta

Por defecto, los objetos se comparan por identidad, es decir, si son la misma instancia en memoria. Esto significa que obj1 == obj2 devuelve true solo si ambos apuntan al mismo objeto.

El método equals en Java permite definir la comparación por contenido. Por defecto, equals en la clase Object se comporta igual que ==, pero se puede sobrescribir para que compare atributos significativos de la clase.

Para comparar dos cadenas en Java por contenido, se debe usar str1.equals(str2) y no ==, ya que este último solo comprueba si las referencias apuntan al mismo objeto, no si las cadenas tienen el mismo texto.

21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?
Respuesta

Las clases wrapper son clases que envuelven tipos primitivos para tratarlos como objetos. Por ejemplo, en Java Integer envuelve int, y Double envuelve double.

En Java, se hace de forma automática mediante autoboxing (convertir un primitivo en objeto) y unboxing (convertir un objeto en primitivo).

Las ventajas incluyen poder usar tipos primitivos en colecciones genéricas, métodos que requieren objetos y acceso a métodos adicionales como toString() o compareTo(). No todos los lenguajes necesitan wrappers; algunos lenguajes orientados a objetos tratan todos los tipos como objetos, eliminando la distinción entre primitivos y objetos.

22. En POO qué es un tipo de dato enumerado? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?
Respuesta

Un tipo de dato enumerado (enum) define un conjunto limitado de valores posibles para una variable, lo que aumenta la seguridad y claridad del código.

En Java, un enum es efectivamente una clase especial que puede tener atributos, métodos y constructores privados, además de instancias fijas definidas en su declaración.

Las ventajas de encapsulación son que los valores posibles están controlados, evitando que se asignen valores inválidos, y se pueden añadir métodos que operen sobre cada instancia sin exponer detalles internos.

23. Crea un tipo enumerado en Java que se llame Mes, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.
Respuesta
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int orden;

    private Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() { return dias; }
    public int getOrden() { return orden; }
}


Aquí, los atributos dias y orden son privados y solo accesibles mediante métodos públicos, manteniendo la encapsulación. Cada instancia de Mes representa un mes concreto con información inmutable.

24. Añade a la clase Mes del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro enHemisferioNorte). Es decir: esDePrimavera(boolean esHemisferioNorte), esDeVerano(boolean esHemisferioNorte), esDeOtoño(boolean esHemisferioNorte), esDeInvierno(boolean esHemisferioNorte)
Respuesta
public boolean esDePrimavera(boolean enHemisferioNorte) {
    if (enHemisferioNorte) return this == MARZO || this == ABRIL || this == MAYO;
    else return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) return this == JUNIO || this == JULIO || this == AGOSTO;
    else return this == DICIEMBRE || this == ENERO || this == FEBRERO;
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    if (enHemisferioNorte) return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    else return this == MARZO || this == ABRIL || this == MAYO;
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    if (enHemisferioNorte) return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    else return this == JUNIO || this == JULIO || this == AGOSTO;
}


Estos métodos usan la enumeración y el parámetro del hemisferio para determinar la estación correspondiente, manteniendo la encapsulación y la claridad en el código.