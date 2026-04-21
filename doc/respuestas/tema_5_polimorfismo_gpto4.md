<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

Aquí tienes las respuestas completas, manteniendo los enunciados originales para facilitar tu trabajo de copia y pega.

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El **polimorfismo** es la capacidad que tienen diferentes objetos de responder al mismo mensaje (invocación de un método) de maneras distintas. Sirve para que un programador pueda enviar una orden genérica a un grupo de objetos sin preocuparse de qué tipo específico son, permitiendo que cada uno ejecute su propia lógica interna. Esto desacopla el código y facilita la creación de sistemas flexibles y escalables.

La **sobreescritura** (*overriding*) es la técnica que hace posible el polimorfismo. Ocurre cuando una subclase proporciona una implementación específica de un método que ya ha sido definido en su superclase. Al sobreescribir, la subclase "redefine" el comportamiento para adaptarlo a sus necesidades particulares, manteniendo la misma firma (nombre y parámetros) que el método original.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica** es el mecanismo por el cual el lenguaje decide qué implementación de un método ejecutar en tiempo de ejecución, en lugar de hacerlo en tiempo de compilación. Está íntimamente ligada al polimorfismo: cuando llamamos a un método sobre una referencia de tipo padre que apunta a un objeto hijo, la ligadura dinámica asegura que se ejecute la versión del método del hijo.

En **Java**, todos los métodos no estáticos y no finales usan ligadura dinámica por defecto; no hay que indicarlo explícitamente. En **C++**, por el contrario, el programador debe marcar el método con la palabra clave `virtual` en la clase base para habilitar este comportamiento; de lo contrario, se aplica "ligadura estática" y se ejecutaría siempre el método del tipo de la referencia. En **Python**, al ser un lenguaje puramente dinámico, todos los métodos se resuelven en tiempo de ejecución (enlace tardío) sin necesidad de ninguna palabra clave.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
En este ejemplo, aunque el array es de tipo `Soldado`, la ligadura dinámica permite que cada objeto ejecute su propia versión de `saludar()`.

```java
class Soldado {
    public void saludar() { System.out.println("Soldado presentándose."); }
}
class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("Zapador despejando el camino."); }
}
class Artillero extends Soldado {
    // Hereda el saludo por defecto del Soldado
}

// Demostración del polimorfismo
Soldado[] peloton = { new Zapador(), new Artillero() };
for (Soldado s : peloton) {
    s.saludar(); // Imprime mensajes distintos según el objeto real
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, es posible invocar el método de la clase base desde la subclase para reutilizar su lógica y luego extenderla. Esto es muy común cuando queremos añadir funcionalidad sin desechar el comportamiento original. Para lograr esto, se utiliza la palabra clave **`super`**, la cual permite acceder a miembros de la superclase inmediata.

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Invoca el comportamiento original de Soldado
        System.out.println("ZAPADOR A SUS ORDENES"); // Añade comportamiento específico
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Para una sobreescritura válida, la firma del método (nombre y tipos de parámetros) debe ser **idéntica**. El tipo de retorno debe ser el mismo o un subtipo del original (retorno covariante). La **sobrecarga** consiste en crear métodos con el mismo nombre pero con parámetros distintos en la misma clase; la **sobreescritura** consiste en cambiar el comportamiento de un método heredado en una subclase.

La anotación `@Override` le indica al compilador nuestra intención de sobreescribir un método. Es altamente recomendable usarla porque, si cometemos un error tipográfico en el nombre o en los parámetros, el compilador nos dará un error. Sin ella, el compilador podría interpretar el método erróneo como una simple carga nueva, provocando fallos lógicos difíciles de detectar en el polimorfismo.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Efectivamente, en Java el polimorfismo se utiliza desde los niveles más básicos. Debido a que todas las clases heredan de `Object`, cualquier clase que definamos ya participa en una jerarquía polimórfica. Cuando sobreescribimos `toString()` o `equals()`, estamos aplicando polimorfismo sobre los métodos fundamentales de la plataforma.

Al hacer esto, permitimos que otras partes del ecosistema Java (como las funciones de impresión `System.out.println()` o las colecciones de datos como `ArrayList`) interactúen con nuestros objetos de manera genérica. Cuando imprimes un objeto, Java llama polimórficamente al `toString()` de tu clase, demostrando que el polimorfismo es la base de cómo funciona todo el lenguaje.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una **clase abstracta** es una clase que no puede ser instanciada (no puedes hacer `new Soldado()`); sirve únicamente como base para otras clases. Un **método abstracto** es un método declarado sin implementación (sin cuerpo `{}`), que obliga a las subclases no abstractas a proporcionar su propia versión del mismo.

La palabra clave `abstract` debe colocarse tanto en la definición de la clase como en la firma de los métodos que no tengan cuerpo. Esto define un contrato: "todo soldado sabe atacar, pero hasta que no sepa qué tipo de soldado es, no puedo decirte cómo lo hace".

```java
abstract class Soldado {
    public void saludar() { System.out.println("Soldado listo."); }
    public abstract void atacar(); // Método sin cuerpo
}

class Artillero extends Soldado {
    @Override
    public void atacar() { System.out.println("Disparando cohetes!"); }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave `final` actúa como un limitador del polimorfismo. Si se aplica a una **clase**, impide que esta sea extendida (no puede tener hijos). Si se aplica a un **método**, impide que las subclases lo sobreescriban. Se utiliza por motivos de seguridad o diseño, cuando queremos garantizar que un comportamiento permanezca inalterable.

Un ejemplo clásico en la API de Java es la clase **`String`**. Es una clase `final`, lo que significa que nadie puede crear una subclase de `String` para alterar su comportamiento básico (que es inmutable). Esto es vital para la seguridad y el rendimiento del sistema, asegurando que las cadenas de texto se comporten siempre de la misma manera predecible.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Las **interfaces** son contratos de comportamiento que definen qué puede hacer una clase, pero no cómo lo hace. A diferencia de las clases abstractas, que pueden contener estado (atributos) y lógica, las interfaces tradicionalmente solo contenían constantes y firmas de métodos (aunque en versiones modernas de Java pueden tener métodos por defecto).

La gran diferencia y ventaja sobre las clases abstractas es que una clase en Java puede **implementar múltiples interfaces** simultáneamente, permitiendo a un objeto cumplir varios roles (por ejemplo, un `Soldado` podría implementar `Conductor` y `RadioOperador`). Esto compensa la falta de herencia múltiple de clases en Java, ofreciendo una forma limpia de polimorfismo múltiple.



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Este diseño muestra cómo la clase `Linea` puede trabajar con cualquier tipo de punto gracias al polimorfismo, mientras que los puntos individuales gestionan su propia lógica interna mediante el uso de tipos específicos.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) return -1;
        Punto2D p = (Punto2D) otro;
        return Math.sqrt(Math.pow(p.x - this.x, 2) + Math.pow(p.y - this.y, 2));
    }
}

class Linea {
    private Punto p1, p2;
    public Linea(Punto p1, Punto p2) { this.p1 = p1; this.p2 = p2; }
    public double getLongitud() { return p1.calcularDistanciaA(p2); }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces permite que una interfaz herede los métodos de otra, ampliando el contrato original. A diferencia de las clases, **sí existe la herencia múltiple de interfaces**; una interfaz puede extender a varias interfaces padres al mismo tiempo utilizando la palabra clave `extends` seguida de una lista separada por comas.

```java
interface Fichero {
    String leer();
}

// Herencia de interfaces
interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

// Una clase que implemente FicheroEscribible deberá implementar los 3 métodos.
```
Esto permite crear jerarquías de capacidades muy granulares y flexibles sin los problemas de ambigüedad de la herencia de clases.