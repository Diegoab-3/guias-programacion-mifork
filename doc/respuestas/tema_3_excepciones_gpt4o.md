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
1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función raiz ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.
Respuesta

En C, como no existen excepciones, el error debe indicarse mediante el valor de retorno o a través de parámetros adicionales. Una primera opción es devolver un valor especial que indique error, por ejemplo -1 o NAN, y que el código llamador compruebe ese valor.

#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial para indicar error
    }
    return sqrtf(x);
}

int main() {
    float r = raiz(-9);
    if (r < 0) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }
    return 0;
}

Una segunda opción es devolver un código de error y pasar el resultado por referencia mediante un puntero. Así se separa claramente el estado del resultado.

#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0;  // 0 indica error
    }
    *resultado = sqrtf(x);
    return 1;  // 1 indica éxito
}

int main() {
    float r;
    if (!raiz(-9, &r)) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }
    return 0;
}
2. Brevemente ¿Qué es una "excepción"? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?
Respuesta

Una excepción es un mecanismo que permite indicar que se ha producido una situación anómala o error durante la ejecución de un programa. En lugar de devolver códigos de error manualmente, el flujo normal del programa se interrumpe y se transfiere el control a un manejador especializado.

Un programador usa excepciones para separar la lógica principal del código del tratamiento de errores. Cuando implementa funciones, puede lanzar una excepción si ocurre un problema; cuando las llama, puede capturarla para gestionarla adecuadamente sin mezclar el código de control de errores con la lógica principal.

3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase Calculadora y llama a dicho método desde el método main, mostrando cómo se puede controlar desde fuera.
Respuesta

En Java podemos usar excepciones para indicar el error. Por ejemplo, lanzando IllegalArgumentException si el número es negativo.

public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = Calculadora.raiz(-9);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

Aquí el método raiz lanza la excepción y el método main la controla desde fuera con un bloque try-catch.

4. ¿Qué es "lanzar" una excepción? ¿Qué es "controlar" o "capturar" una excepción? ¿Qué es que se "propague" una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.
Respuesta

"Lanzar" una excepción significa crear un objeto excepción y transferir el control fuera del flujo normal mediante la palabra clave throw. En el ejemplo, raiz lanza una IllegalArgumentException cuando el argumento es negativo.

"Capturar" o "controlar" una excepción significa interceptarla mediante un bloque catch para tratar el error. Si main tiene un try-catch, puede mostrar un mensaje sin que el programa termine abruptamente.

Cuando una excepción no es capturada en el método donde se produce, se "propaga" hacia arriba en la pila de llamadas. Cada método que no la captura termina inmediatamente su ejecución y devuelve el control al método que lo llamó. Las funciones intermedias no se reanudan después: su ejecución queda interrumpida definitivamente.

5. ¿Qué ventajas tiene frente a C, la "propagación natural" de las excepciones a través de la pila (stack) de llamadas?
Respuesta

La propagación natural evita tener que comprobar manualmente códigos de error en cada llamada intermedia. En C, cada función debe revisar y reenviar el error explícitamente, lo que complica el código.

En cambio, con excepciones, si un método no sabe cómo manejar el error, simplemente no lo captura y este se transmite automáticamente hasta encontrar un manejador adecuado. Esto hace el código más limpio, legible y menos propenso a errores de comprobación olvidada.

6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?
Respuesta

Sí, en orientación a objetos las excepciones son objetos que encapsulan información sobre el error ocurrido. Contienen datos como el mensaje descriptivo y la traza de la pila.

Esto permite encapsular información relevante del fallo dentro de un objeto coherente. Además, podemos crear excepciones personalizadas heredando de Exception o RuntimeException, adaptándolas a las necesidades específicas de nuestra aplicación.

7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué información esencial lleva cualquier objeto excepción que es muy útil tener cuando se llega a un manejador?
Respuesta

Un objeto excepción en Java contiene información esencial como el mensaje descriptivo del error y la traza de la pila (stack trace), que indica en qué métodos y líneas se produjo el problema.

Esta información es muy útil en el manejador porque permite saber no solo que hubo un error, sino exactamente dónde y cómo ocurrió, algo que en C normalmente habría que gestionar manualmente.

8. En Java, sobre el bloque "try-catch", ¿se pueden tener más de un bloque catch? ¿cuántos bloques catch se ejecutan?
Respuesta

Sí, en Java se pueden tener varios bloques catch asociados a un mismo try, cada uno para un tipo distinto de excepción.

Sin embargo, solo se ejecuta un bloque catch: el primero cuyo tipo de excepción sea compatible con la excepción lanzada. Una vez ejecutado ese bloque, los demás se ignoran.

9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con finally, tanto con catch como sin él.
Respuesta

Para garantizar la ejecución de código crítico como liberar recursos, se utiliza el bloque finally, que se ejecuta siempre haya o no excepción.

Con catch:

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Liberando recursos...");
}

Sin catch:

try {
    double r = Calculadora.raiz(-9);
} finally {
    System.out.println("Liberando recursos...");
}

En ambos casos, el bloque finally se ejecuta siempre.

10. En Java, el bloque finally puede ir sin catch? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un return en medio del try?
Respuesta

Sí, el bloque finally puede ir sin catch, siempre que exista un try. Se ejecuta tanto si ocurre una excepción como si no ocurre ninguna.

Incluso si hay un return dentro del bloque try, el bloque finally se ejecuta antes de que el método retorne definitivamente.

11. En Java, qué son las excepciones "controladas" y las "no controladas"? ¿Qué papel juega RuntimeException? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.
Respuesta

Las excepciones controladas (checked) son aquellas que el compilador obliga a capturar o declarar con throws, como IOException. Las no controladas (unchecked) heredan de RuntimeException y no requieren declaración obligatoria.

RuntimeException es la clase base de las excepciones no controladas y suele utilizarse para errores de programación.

Situaciones típicas para excepciones controladas:

Error al abrir un fichero.

Fallo en conexión de red.

Lectura incorrecta de datos externos.

Acceso a base de datos.

Situaciones típicas para excepciones no controladas:

Argumento inválido (IllegalArgumentException).

Acceso fuera de rango (IndexOutOfBoundsException).

Uso de objeto nulo (NullPointerException).

Error lógico interno de programación.

12. ¿Qué es y para qué se usa throws? ¿Por qué es alternativa a capturar una excepción controlada?
Respuesta

throws se usa en la firma de un método para declarar que puede lanzar una excepción controlada. Indica que el método no la maneja internamente.

Es alternativa a capturarla porque en vez de gestionarla localmente con try-catch, se delega su tratamiento al método llamador, permitiendo que la excepción se propague.

13. Pon un ejemplo en Java de firma de método que incluya throws, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del finally.
Respuesta
import java.io.*;

public static void leerFichero(String nombre) throws IOException {
    FileReader fr = null;
    try {
        fr = new FileReader(nombre);
        // leer fichero
    } finally {
        if (fr != null) {
            fr.close();
        }
    }
}

Aquí el método declara throws IOException, dejando que el llamador la controle.

14. ¿Podemos poner en throws excepciones no controladas, como RuntimeException? ¿Debería el método llamador entonces poner try-catch en ese caso? ¿Qué sentido tendría?
Respuesta

Sí, se pueden declarar excepciones no controladas en throws, aunque no es obligatorio.

El método llamador no está obligado a capturarlas. Solo tendría sentido hacerlo si quiere tratarlas específicamente, pero normalmente las excepciones no controladas indican errores de programación que deberían corregirse y no capturarse sistemáticamente.

15. ¿Cuándo se recomienda usar excepciones controladas, como IOException, y cuándo no controladas como IllegalArgumentException? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?
Respuesta

Se recomienda usar excepciones controladas cuando el error puede ocurrir por causas externas y el programa puede recuperarse, como fallos de entrada/salida. Las no controladas se usan cuando el error indica un fallo de programación o uso incorrecto de la API.

No todos los lenguajes distinguen entre ambas. En muchos lenguajes modernos solo existen excepciones similares a las no controladas, siendo esta la opción más habitual.

16. ¿Tiene sentido lanzar excepciones dentro del catch? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.
Respuesta

Sí, tiene sentido lanzar excepciones dentro de un catch cuando queremos transformar una excepción de bajo nivel en otra más significativa para capas superiores.

try {
    leerFichero("datos.txt");
} catch (IOException e) {
    throw new RuntimeException("Error al procesar el fichero", e);
}

También se puede relanzar la misma excepción:

try {
    leerFichero("datos.txt");
} catch (IOException e) {
    System.out.println("Registrando error...");
    throw e;
}

Esto tiene sentido cuando queremos hacer un registro (log) pero dejar que el error siga propagándose.

17. ¿En qué consiste que una excepción sea la "causa" de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?
Respuesta

Que una excepción sea la "causa" de otra significa que una excepción original se encapsula dentro de otra nueva, manteniendo la referencia a la excepción inicial. Esto permite no perder información del error original.

class ErrorCalculoException extends Exception {
    public ErrorCalculoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    throw new ErrorCalculoException("Fallo en cálculo avanzado", e);
}

Cuando una excepción con causa se muestra por pantalla, la traza incluye tanto la excepción principal como la causa original, indicando claramente la cadena de errores.




