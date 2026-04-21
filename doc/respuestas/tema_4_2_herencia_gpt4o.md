<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

Aquí tienes las respuestas desarrolladas para cada una de las preguntas, manteniendo los enunciados originales y siguiendo la estructura solicitada.

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
La herencia es un mecanismo fundamental de la POO que permite crear nuevas clases a partir de otras ya existentes, estableciendo una relación de jerarquía. La relación **"A es-un B"** indica que la subclase (A) es una especialización de la superclase (B); por tanto, cualquier instancia de la subclase posee todas las características de la clase base, además de las suyas propias. 

Las dos implicaciones principales son: (1) la **herencia de estado y comportamiento**, donde la subclase adquiere los atributos (estado) y métodos (comportamiento) de la superclase, evitando duplicar código; y (2) la **compatibilidad de tipos**, que permite que un objeto de la subclase sea tratado legalmente como un objeto de la superclase en cualquier parte del código, facilitando el polimorfismo.

```java
class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Soy el soldado " + nombre); }
}

class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) { super(nombre); this.cohetes = cohetes; }
    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { super(nombre); this.minas = minas; }
    public int getMinas() { return minas; }
}

// Uso de compatibilidad de tipos
Soldado[] ejercito = { new Artillero("Ramiro", 5), new Zapador("Lucas", 10) };
for (Soldado s : ejercito) { s.saludar(); }
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al instanciar una subclase, se ejecutan tantos constructores como niveles haya en la jerarquía de herencia. El orden de ejecución es **de arriba hacia abajo**: primero se ejecuta el constructor de la superclase (la base) y, una vez finalizado este, se ejecuta el de la subclase. Esto garantiza que la "parte" del objeto que pertenece a la clase padre esté correctamente inicializada antes de que la clase hija añada su propia lógica.

La palabra clave `super` dentro de un constructor se utiliza para invocar explícitamente al constructor de la superclase. Es una herramienta para pasar parámetros hacia arriba en la jerarquía. Si la clase base no dispone de un constructor sin parámetros (o este no es visible), es **obligatorio** llamar a `super` manualmente en la primera línea del constructor de la hija, proporcionando los argumentos que la clase padre requiera.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase **forman parte de la instancia** de la subclase en memoria. Cuando creas un objeto `Artillero`, el espacio de memoria reservado contiene tanto el campo `nombre` (definido en `Soldado`) como el campo `cohetes` (definido en `Artillero`). El objeto es un bloque íntegro que engloba toda la jerarquía de datos.

Sin embargo, que existan en memoria no implica que sean accesibles directamente. Debido a las reglas de encapsulación, la subclase **no puede acceder directamente** a los atributos privados de su padre. Por ejemplo, un `Artillero` no puede hacer `this.nombre = "X"`; debe interactuar con ese dato a través de métodos públicos o protegidos de la clase `Soldado` (como el constructor o un setter), respetando así la privacidad definida en la base.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La compatibilidad de tipos permite que el código sea altamente **extensible** mediante el principio de "abierto/cerrado". Esto significa que podemos añadir nuevas funcionalidades o tipos de datos al sistema sin tener que modificar el código que ya funciona. El sistema está programado para interactuar con la abstracción (la clase base), por lo que cualquier nuevo subtipo "encajará" automáticamente.



Si decidimos añadir un `Francotirador`, no necesitamos tocar el bucle que recorre el array de soldados. El código permanece intacto y el nuevo tipo se integra de forma transparente:

```java
class Francotirador extends Soldado {
    public Francotirador(String nombre) { super(nombre); }
}

// El código de saludo sigue siendo el mismo, aunque ahora incluyamos Francotirador
Soldado[] ejercito = { new Artillero("A", 1), new Zapador("B", 1), new Francotirador("C") };
for (Soldado s : ejercito) { s.saludar(); } // No se cambia ni una línea
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Es totalmente posible tener una referencia de supertipo (ej. `Soldado s`) apuntando a un objeto de subtipo (ej. `new Artillero()`). Sin embargo, a través de esa referencia **solo se pueden invocar métodos definidos en el supertipo**. El compilador solo "ve" lo que la referencia permite, por lo que no podrías llamar a `getCohetes()` directamente usando una referencia de tipo `Soldado`.

El **upcasting** es la conversión hacia arriba en la jerarquía (de hijo a padre) y es automático. El **downcasting** es la conversión hacia abajo (de padre a hijo) y requiere un "cast" explícito, ya que es peligroso. Para hacerlo seguro, usamos `instanceof`, que comprueba si un objeto es de un tipo determinado antes de intentar la conversión.

```java
for (Soldado s : ejercito) {
    s.saludar(); 
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso **protegido (`protected`)** es un nivel de visibilidad intermedio. Permite que un atributo o método sea accesible para la propia clase, para sus subclases (aunque estén en paquetes distintos) y para otras clases dentro del mismo paquete. Es una forma de "abrir la mano" a los herederos sin hacer el dato totalmente público al resto del mundo.

En Java se implementa usando la palabra clave `protected`. En el caso del `Zapador`, si el atributo de `Soldado` es protegido, el `Zapador` puede concatenar el nombre directamente en sus propios métodos de lógica específica sin necesidad de usar un getter público.

```java
class Soldado {
    protected String nombre; // Acceso protegido
    public Soldado(String nombre) { this.nombre = nombre; }
}

class Zapador extends Soldado {
    public Zapador(String nombre) { super(nombre); }
    public void ponerMina() {
        // Acceso directo a 'nombre' por ser protegido
        System.out.println(nombre + " está colocando una mina.");
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
No todos los lenguajes orientados a objetos imponen una raíz única, pero es una característica muy común en los lenguajes modernos. En lenguajes como C++, no existe una clase base universal obligatoria; puedes crear jerarquías totalmente independientes unas de otras.

En **Java**, sin embargo, existe una jerarquía única y absoluta: la clase **`Object`**. Todas las clases en Java, sin excepción, heredan de `java.lang.Object` de forma implícita si no se especifica otra cosa. Esto garantiza que todos los objetos compartan comportamientos básicos, como los métodos `toString()`, `equals()` o `hashCode()`.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es la capacidad de una clase de heredar estado y comportamiento de más de una superclase simultáneamente (por ejemplo, que una clase `CocheAnfibio` herede de `Coche` y de `Barco`). Aunque parece útil, genera problemas complejos como el "Diamante de la Muerte", donde hay ambigüedad sobre qué método ejecutar si ambas clases padre implementan lo mismo.

En **Java no existe la herencia múltiple de clases**. Una clase solo puede extender (`extends`) a una única superclase. Java soluciona la necesidad de compartir comportamientos múltiples mediante el uso de **interfaces**, que permiten a una clase cumplir con múltiples contratos (tipos) sin los problemas de colisión de estado de la herencia múltiple tradicional.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
Para crear una excepción **no controlada**, debemos heredar de `RuntimeException`. Al ser un objeto, podemos añadirle campos adicionales (como un objeto `Usuario`) para que, cuando se capture la excepción, se disponga de más contexto sobre el error ocurrido.

```java
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Sobrecarga para permitir añadir una causa (Throwable)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() { return usuario; }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
La herencia es el vínculo más fuerte que puede existir entre dos clases. Si heredas solo para reutilizar un par de métodos, estás forzando a tu clase a ser un subtipo de la otra para siempre. Esto crea un **acoplamiento rígido**: si la clase base cambia, la subclase se ve afectada obligatoriamente, incluso si esa relación no tiene sentido semántico.

La herencia debe usarse cuando existe una relación conceptual de identidad ("es-un"). Si solo quieres usar una funcionalidad de otra clase, es mejor la composición: tener una instancia de esa clase dentro de la tuya. Así, obtienes el código que necesitas sin heredar todas las restricciones y responsabilidades de la jerarquía del padre.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se favorece la composición porque es mucho más **flexible y dinámica**. Mientras que la herencia se define en tiempo de compilación y no se puede cambiar (un objeto no puede dejar de ser hijo de su padre), la composición permite cambiar el comportamiento en tiempo de ejecución simplemente sustituyendo el objeto contenido por otro.



Además, la composición reduce el acoplamiento. Al usar composición, solo dependes de la interfaz pública del objeto que contiene, mientras que en la herencia dependes a menudo de detalles internos de la implementación de la superclase. Esto hace que el sistema sea más fácil de mantener y probar.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Esta frase se refiere a que la herencia expone a la subclase a los detalles de implementación de la superclase. En muchos casos, para que una subclase funcione correctamente, el programador debe conocer cómo están implementados internamente los métodos del padre (por ejemplo, si un método llama internamente a otro que el hijo ha sobrescrito). 

Esto crea una dependencia frágil: un cambio interno en la superclase que no debería afectar a nadie (porque su interfaz pública sigue igual) puede romper inesperadamente todas sus subclases. En cambio, con la composición, la encapsulación es total, ya que la clase contenedora solo interactúa con el objeto contenido a través de su contrato externo.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
En la opción de **herencia**, definimos una jerarquía donde el estudiante y el trabajador "son" personas. En la de **composición**, el estudiante y el trabajador "tienen" datos personales, lo cual separa la identidad de la entidad de sus atributos descriptivos.

```java
// OPCIÓN 1: Herencia
class Persona {
    String dni, nombre;
    public Persona(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) { super(dni, nombre); }
}

// OPCIÓN 2: Composición
class DatosPersonales {
    String dni, nombre;
    public DatosPersonales(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
class Trabajador {
    private DatosPersonales datos; // Composición
    public Trabajador(DatosPersonales datos) { this.datos = datos; }
}
```