# Documentación Esencial de JavaScript

A continuación se presenta la documentación de los temas base de JavaScript para dar soporte a los estudiantes.

## 1. Datos Primitivos

En JavaScript, un dato primitivo es un dato que no es un objeto y no tiene métodos. Hay 7 tipos de datos primitivos:

*   **`string`**: Una secuencia de caracteres. Se utilizan para representar texto.
    ```javascript
    let nombre = "Juan";
    ```
*   **`number`**: Un número. JavaScript no distingue entre enteros y números de punto flotante.
    ```javascript
    let edad = 30;
    let precio = 19.99;
    ```
*   **`boolean`**: Un valor lógico que solo puede ser `true` o `false`.
    ```javascript
    let esEstudiante = true;
    ```
*   **`null`**: Un valor que representa la ausencia intencional de cualquier valor de objeto.
    ```javascript
    let direccion = null;
    ```
*   **`undefined`**: Un valor que se le asigna automáticamente a las variables que solo han sido declaradas, o a los argumentos de función para los que no se proporcionan valores.
    ```javascript
    let telefono;
    console.log(telefono); // undefined
    ```
*   **`symbol`**: Un tipo de dato único e inmutable, a menudo utilizado como clave para las propiedades de los objetos.
    ```javascript
    let id = Symbol("id");
    ```
*   **`bigint`**: Un tipo de dato numérico que puede representar enteros con una precisión arbitraria.
    ```javascript
    const numeroGrande = 1234567890123456789012345678901234567890n;
    ```

## 2. Scope (var, let y const)

El "scope" o ámbito en JavaScript se refiere a la visibilidad y accesibilidad de las variables. JavaScript tiene tres tipos de scope: global, de función y de bloque.

*   **`var`**: Las variables declaradas con `var` tienen un ámbito de función o global. No tienen ámbito de bloque. Esto significa que si una variable `var` se declara dentro de un bucle `for` o una sentencia `if`, sigue siendo accesible fuera de ese bloque.

    ```javascript
    function ejemploVar() {
        if (true) {
            var x = 10;
        }
        console.log(x); // 10
    }
    ```

*   **`let`**: Las variables declaradas con `let` tienen un ámbito de bloque. Esto significa que solo son accesibles dentro del bloque en el que se declaran (por ejemplo, dentro de un bucle `for` o una sentencia `if`).

    ```javascript
    function ejemploLet() {
        if (true) {
            let y = 20;
            console.log(y); // 20
        }
        // console.log(y); // ReferenceError: y is not defined
    }
    ```

*   **`const`**: Las variables declaradas con `const` también tienen un ámbito de bloque, como `let`. La principal diferencia es que las variables `const` no se pueden reasignar. Deben ser inicializadas en el momento de la declaración.

    ```javascript
    function ejemploConst() {
        const z = 30;
        // z = 40; // TypeError: Assignment to constant variable.

        const obj = { a: 1 };
        obj.a = 2; // Esto es válido, porque no se está reasignando el objeto en sí.
    }
    ```

## 3. Hoisting

El "hoisting" es un comportamiento de JavaScript en el que las declaraciones de variables y funciones se mueven al principio de su ámbito (global o de función) antes de que se ejecute el código. Es importante destacar que solo las *declaraciones* se elevan, no las *inicializaciones*.

*   **Hoisting de `var`**: Las variables declaradas con `var` se elevan y se inicializan con `undefined`.

    ```javascript
    console.log(miVar); // undefined
    var miVar = 5;
    ```

*   **Hoisting de `let` y `const`**: Las variables `let` y `const` también se elevan, pero no se inicializan. Se encuentran en una "zona muerta temporal" (Temporal Dead Zone o TDZ) desde el inicio del bloque hasta que se procesa su declaración. Intentar acceder a ellas antes de la declaración resulta en un `ReferenceError`.

    ```javascript
    // console.log(miLet); // ReferenceError: Cannot access 'miLet' before initialization
    let miLet = 10;
    ```

*   **Hoisting de funciones**: Las declaraciones de funciones se elevan por completo, lo que significa que se puede llamar a una función antes de que se declare en el código.

    ```javascript
    miFuncion(); // "Hola"

    function miFuncion() {
        console.log("Hola");
    }
    ```

    Sin embargo, las expresiones de función (cuando se asigna una función a una variable) no se elevan de la misma manera. Si la variable se declara con `var`, se elevará y se le asignará `undefined`, lo que provocará un `TypeError` si se intenta llamar como una función. Si se declara con `let` o `const`, se producirá un `ReferenceError`.

    ```javascript
    // miExpresionDeFuncion(); // TypeError: miExpresionDeFuncion is not a function (con var)
    // O ReferenceError (con let o const)
    var miExpresionDeFuncion = function() {
        console.log("Hola desde una expresión");
    };
    ```

## 4. Event Loop: Cómo JavaScript interpreta las tareas síncronas y asíncronas

JavaScript es un lenguaje de programación de un solo hilo, lo que significa que solo puede ejecutar una tarea a la vez. El "Event Loop" (bucle de eventos) es el mecanismo que permite a JavaScript realizar operaciones asíncronas y no bloqueantes, a pesar de ser de un solo hilo.

El modelo de concurrencia de JavaScript se basa en estos componentes:

*   **Call Stack (Pila de llamadas)**: Es una estructura de datos que registra la posición del script. Si entramos en una función, la ponemos en la parte superior de la pila. Cuando volvemos de una función, la sacamos de la pila. Las tareas síncronas se ejecutan directamente en la pila de llamadas.

*   **Web APIs (APIs del navegador) / Node.js APIs**: Son APIs proporcionadas por el entorno de ejecución (navegador o Node.js) que manejan operaciones asíncronas como `setTimeout`, peticiones HTTP (`fetch`), o manipulación del DOM. Cuando se llama a una de estas APIs, la operación se inicia en segundo plano, fuera de la pila de llamadas de JavaScript.

*   **Callback Queue (Cola de callbacks) / Task Queue**: Es una cola de mensajes (tareas) que contiene los callbacks que están listos para ser ejecutados. Cuando una operación asíncrona finaliza (por ejemplo, un `setTimeout` expira o se recibe una respuesta de una petición `fetch`), su función de callback se añade a esta cola.

*   **Event Loop**: Es un proceso que se ejecuta continuamente y comprueba si la pila de llamadas está vacía. Si la pila de llamadas está vacía, el Event Loop toma el primer mensaje de la cola de callbacks y lo empuja a la pila de llamadas para que se ejecute.

### Flujo de ejecución:

1.  El código JavaScript se ejecuta línea por línea.
2.  Las llamadas a funciones síncronas se apilan en el Call Stack y se ejecutan inmediatamente.
3.  Cuando se encuentra una operación asíncrona (como `setTimeout`), se pasa a la API correspondiente del entorno. La ejecución de JavaScript no se detiene a esperar que termine.
4.  La API del entorno procesa la operación en segundo plano.
5.  Una vez que la operación asíncrona se completa, su función de callback se coloca en la Callback Queue.
6.  El Event Loop monitorea constantemente el Call Stack. Cuando el Call Stack está vacío (lo que significa que todo el código síncrono ha terminado de ejecutarse), el Event Loop toma el primer callback de la Callback Queue y lo mueve al Call Stack.
7.  El callback se ejecuta.

Este ciclo permite que JavaScript maneje eventos y ejecute código asíncrono sin bloquear el hilo principal, manteniendo la interfaz de usuario receptiva.

```javascript
console.log("Inicio del script"); // 1. Se ejecuta y se muestra primero

setTimeout(() => {
    console.log("Timeout de 0 segundos"); // 4. Se ejecuta después del script principal
}, 0);

console.log("Fin del script"); // 2. Se ejecuta y se muestra segundo

// Orden de salida:
// "Inicio del script"
// "Fin del script"
// "Timeout de 0 segundos"
```

## 5. JSON (JavaScript Object Notation)

JSON es un formato de texto ligero para el intercambio de datos. Es fácil de leer y escribir para los humanos, y fácil de analizar y generar para las máquinas. Aunque se deriva de JavaScript, es un formato de datos independiente del lenguaje.

La sintaxis de JSON es un subconjunto de la sintaxis de objetos de JavaScript:

*   Los datos están en pares de nombre/valor (clave/valor).
*   Los datos están separados por comas.
*   Las llaves `{}` contienen objetos.
*   Los corchetes `[]` contienen arrays.
*   Las claves (nombres) deben ser cadenas de texto entre comillas dobles.

### Ejemplo de JSON:

```json
{
    "nombre": "Juan Perez",
    "edad": 30,
    "esEstudiante": false,
    "cursos": [
        {
            "titulo": "Historia del Arte",
            "creditos": 3
        },
        {
            "titulo": "Programación Avanzada",
            "creditos": 4
        }
    ],
    "direccion": null
}
```

### Uso en JavaScript

JavaScript proporciona dos métodos principales para trabajar con JSON:

*   **`JSON.stringify()`**: Convierte un objeto de JavaScript en una cadena de texto JSON.

    ```javascript
    const usuario = {
        nombre: "Ana",
        edad: 25
    };

    const jsonString = JSON.stringify(usuario);
    console.log(jsonString); // '{"nombre":"Ana","edad":25}'
    ```

*   **`JSON.parse()`**: Analiza una cadena de texto JSON y la convierte en un objeto de JavaScript.

    ```javascript
    const jsonString = '{"nombre":"Ana","edad":25}';

    const usuario = JSON.parse(jsonString);
    console.log(usuario.nombre); // "Ana"
    ```

JSON es el formato más común para el intercambio de datos entre un cliente (como un navegador web) y un servidor en las aplicaciones web modernas.

## 6. Prototypes y herencia prototipal

En JavaScript, la herencia se basa en prototipos. Cada objeto en JavaScript tiene una propiedad interna oculta llamada `[[Prototype]]` (que se puede acceder a través de `Object.getPrototypeOf(obj)` o el accesor `__proto__`) que es una referencia a otro objeto, llamado el "prototipo" del objeto.

Cuando se intenta acceder a una propiedad de un objeto, si la propiedad no se encuentra en el propio objeto, el motor de JavaScript busca la propiedad en el prototipo del objeto. Si tampoco se encuentra allí, busca en el prototipo del prototipo, y así sucesivamente, hasta que encuentra la propiedad o llega al final de la cadena de prototipos (un objeto cuyo prototipo es `null`).

### ¿Cómo funciona?

*   **Constructores y `prototype`**: Todas las funciones en JavaScript tienen una propiedad especial llamada `prototype`. Cuando se crea un objeto utilizando una función como constructor (con la palabra clave `new`), el objeto resultante tendrá su `[[Prototype]]` establecido en el valor de la propiedad `prototype` de la función constructora.

    ```javascript
    function Persona(nombre) {
        this.nombre = nombre;
    }

    // Añadimos un método al prototipo de Persona
    Persona.prototype.saludar = function() {
        console.log("Hola, soy " + this.nombre);
    };

    const juan = new Persona("Juan");
    const ana = new Persona("Ana");

    juan.saludar(); // "Hola, soy Juan"
    ana.saludar(); // "Hola, soy Ana"

    // El método saludar no está en los objetos juan o ana directamente,
    // sino en su prototipo (Persona.prototype).
    ```

*   **Herencia prototipal**: Se puede crear una cadena de prototipos para que los objetos hereden propiedades y métodos de otros objetos.

    ```javascript
    function Estudiante(nombre, curso) {
        // Llama al constructor de Persona para establecer el nombre
        Persona.call(this, nombre);
        this.curso = curso;
    }

    // Establece la herencia: el prototipo de Estudiante será un objeto
    // que hereda de Persona.prototype
    Estudiante.prototype = Object.create(Persona.prototype);

    // Es buena práctica restaurar el constructor
    Estudiante.prototype.constructor = Estudiante;

    Estudiante.prototype.presentarCurso = function() {
        console.log("Estoy en el curso de " + this.curso);
    };

    const estudiante = new Estudiante("Carlos", "Matemáticas");

    estudiante.saludar(); // Heredado de Persona.prototype
    estudiante.presentarCurso(); // Propio de Estudiante.prototype
    ```

Aunque la sintaxis de clases (`class`), introducida en ES6, proporciona una forma más sencilla y familiar de trabajar con la herencia, es importante entender que en el fondo, JavaScript sigue utilizando la herencia prototipal. Las clases en JavaScript son principalmente "azúcar sintáctico" sobre el sistema de prototipos existente.

## 7. Arrow functions y Función convencional Javascript

ES6 introdujo las "arrow functions" (funciones flecha), que proporcionan una sintaxis más concisa para escribir funciones en JavaScript. Sin embargo, existen diferencias clave en su comportamiento en comparación con las funciones tradicionales.

### Función Convencional (Declaración de función o Expresión de función)

```javascript
// Declaración de función
function sumar(a, b) {
    return a + b;
}

// Expresión de función
const restar = function(a, b) {
    return a - b;
};
```

### Arrow Function

```javascript
// Arrow function
const multiplicar = (a, b) => {
    return a * b;
};

// Si solo hay una expresión de retorno, se puede simplificar
const dividir = (a, b) => a / b;

// Si solo hay un parámetro, los paréntesis son opcionales
const cuadrado = x => x * x;
```

### Diferencias Clave

1.  **Sintaxis**: Las arrow functions son más compactas.

2.  **El `this` léxico**: Esta es la diferencia más importante.
    *   En una **función convencional**, el valor de `this` se determina por *cómo se llama a la función*. Puede ser el objeto que llama, el objeto global (`window` en el navegador), `undefined` (en modo estricto), o un valor establecido explícitamente con `.call()`, `.apply()`, o `.bind()`.
    *   En una **arrow function**, `this` no tiene su propio contexto. En su lugar, hereda (o "captura") el valor de `this` del ámbito en el que fue creada (el contexto léxico). `this` en una arrow function siempre se refiere al `this` de la función o ámbito que la contiene.

    ```javascript
    function Temporizador() {
        this.segundos = 0;

        setInterval(function() {
            // En una función convencional, 'this' aquí es el objeto global (window)
            // o undefined en modo estricto, no el objeto Temporizador.
            // console.log(this.segundos++); // No funciona como se espera
        }, 1000);

        setInterval(() => {
            // En una arrow function, 'this' es el mismo que en el ámbito de Temporizador.
            console.log(this.segundos++); // Funciona correctamente
        }, 1000);
    }

    const miTemporizador = new Temporizador();
    ```

3.  **No tienen `arguments`**: Las arrow functions no tienen su propio objeto `arguments`. Si necesitas acceder a los argumentos, puedes usar parámetros rest (`...args`).

    ```javascript
    const miArrowFunction = (...args) => {
        console.log(args); // [1, 2, 3]
    };
    miArrowFunction(1, 2, 3);
    ```

4.  **No se pueden usar como constructores**: No se puede usar una arrow function con la palabra clave `new`. Intentarlo lanzará un `TypeError`.

5.  **No tienen la propiedad `prototype`**.

### ¿Cuándo usar cada una?

*   Usa **funciones convencionales** cuando necesites un `this` dinámico (por ejemplo, en métodos de objetos que serán llamados en diferentes contextos) o cuando necesites usar la función como un constructor.
*   Usa **arrow functions** en la mayoría de los demás casos, especialmente para callbacks o cuando quieras preservar el contexto de `this` de su ámbito original. Son ideales para métodos de clase que actúan como manejadores de eventos en componentes (por ejemplo, en React).

## 8. Callbacks, Promesas y Async/Await

JavaScript utiliza varias construcciones para manejar operaciones asíncronas, que son acciones que pueden tardar un tiempo en completarse (como leer un archivo, hacer una petición a una API, etc.).

### 1. Callbacks

Un "callback" es una función que se pasa como argumento a otra función, con la intención de que se ejecute ("called back") después de que la función contenedora haya completado su tarea.

*   **Ventaja**: Es el método más fundamental y funciona en todos los entornos de JavaScript.
*   **Desventaja**: Puede llevar al "Callback Hell" o "Pyramid of Doom", que es una situación donde múltiples callbacks anidados hacen que el código sea difícil de leer y mantener.

```javascript
function obtenerDatos(id, callback) {
    setTimeout(() => {
        console.log("Datos obtenidos");
        // Simulamos que obtenemos un objeto de usuario
        const usuario = { id: id, nombre: "Usuario " + id };
        callback(usuario);
    }, 1000);
}

// Callback Hell
obtenerDatos(1, (usuario1) => {
    console.log(usuario1);
    obtenerDatos(2, (usuario2) => {
        console.log(usuario2);
        obtenerDatos(3, (usuario3) => {
            console.log(usuario3);
        });
    });
});
```

### 2. Promesas (Promises)

Una `Promise` (promesa) es un objeto que representa la eventual finalización (o fallo) de una operación asíncrona y su valor resultante. Una promesa puede estar en uno de estos tres estados:

*   **Pending (pendiente)**: Estado inicial, ni cumplida ni rechazada.
*   **Fulfilled (cumplida)**: La operación se completó con éxito.
*   **Rejected (rechazada)**: La operación falló.

Las promesas permiten encadenar operaciones asíncronas de una manera más legible usando `.then()` para manejar el éxito y `.catch()` para manejar los errores.

```javascript
function obtenerDatosConPromesa(id) {
    return new Promise((resolve, reject) => {
        if (id < 0) {
            reject("ID no válido");
        }
        setTimeout(() => {
            console.log("Datos obtenidos con promesa");
            const usuario = { id: id, nombre: "Usuario " + id };
            resolve(usuario);
        }, 1000);
    });
}

obtenerDatosConPromesa(1)
    .then(usuario1 => {
        console.log(usuario1);
        return obtenerDatosConPromesa(2);
    })
    .then(usuario2 => {
        console.log(usuario2);
        return obtenerDatosConPromesa(3);
    })
    .then(usuario3 => {
        console.log(usuario3);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

### 3. Async/Await

`async/await` es una sintaxis más moderna (introducida en ES2017) que se construye sobre las promesas y permite escribir código asíncrono que se ve y se comporta más como código síncrono. Esto hace que el código sea aún más fácil de leer y entender.

*   **`async`**: La palabra clave `async` se usa para declarar una función asíncrona. Estas funciones siempre devuelven una promesa.
*   **`await`**: La palabra clave `await` solo se puede usar dentro de una función `async`. Pausa la ejecución de la función `async` y espera a que una promesa se resuelva o se rechace. Si la promesa se resuelve, `await` devuelve el valor resuelto. Si se rechaza, lanza una excepción.

Para manejar errores, se utiliza el bloque `try...catch`.

```javascript
// Reutilizamos la función obtenerDatosConPromesa

async function mostrarUsuarios() {
    try {
        const usuario1 = await obtenerDatosConPromesa(1);
        console.log(usuario1);

        const usuario2 = await obtenerDatosConPromesa(2);
        console.log(usuario2);

        const usuario3 = await obtenerDatosConPromesa(3);
        console.log(usuario3);
    } catch (error) {
        console.error("Error en la función async:", error);
    }
}

mostrarUsuarios();
```

En resumen, los **callbacks** son el mecanismo base, las **promesas** ofrecen una mejora para evitar el Callback Hell, y **`async/await`** proporciona una sintaxis superior y más legible para trabajar con promesas.

## 9. Instalar Node.js y NVM

### Node.js

**¿Qué es?**
Node.js es un entorno de ejecución de JavaScript de código abierto y multiplataforma que permite a los desarrolladores ejecutar código JavaScript fuera de un navegador web. Se utiliza comúnmente para construir aplicaciones de servidor (backend), pero también es fundamental para el desarrollo frontend moderno.

**¿Para qué sirve?**
*   **Desarrollo Backend**: Crear servidores web, APIs REST, y microservicios.
*   **Herramientas de Desarrollo**: Ecosistema de herramientas de línea de comandos (CLI) para automatizar tareas, empaquetar archivos, transpilar código, etc. (por ejemplo, Webpack, Babel, ESLint).
*   **Aplicaciones de Escritorio**: Con frameworks como Electron.
*   **Aplicaciones en Tiempo Real**: Como chats o juegos, utilizando librerías como Socket.io.

### NVM (Node Version Manager)

**¿Qué es?**
NVM es un gestor de versiones de Node.js. Es una herramienta de línea de comandos que permite instalar, desinstalar y cambiar entre múltiples versiones de Node.js en el mismo sistema.

**¿Por qué usarlo?**
Los proyectos a menudo requieren versiones específicas de Node.js. NVM soluciona el problema de tener que desinstalar y reinstalar Node.js manualmente para cambiar de proyecto. Permite tener varias versiones instaladas y seleccionar la que se necesita para un proyecto específico.

**Comandos básicos de NVM (en macOS/Linux):**
*   `nvm install <version>`: Instalar una versión específica de Node.js (ej. `nvm install 18.17.0`).
*   `nvm install node`: Instalar la última versión de Node.js.
*   `nvm ls`: Listar todas las versiones de Node.js instaladas.
*   `nvm use <version>`: Cambiar a una versión específica de Node.js en la sesión actual de la terminal (ej. `nvm use 18.17.0`).
*   `nvm alias default <version>`: Establecer una versión por defecto para nuevas terminales.
*   `.nvmrc`: Puedes crear un archivo llamado `.nvmrc` en la raíz de tu proyecto con una versión de Node.js (ej. `18.17.0`). Luego, al ejecutar `nvm use` en ese directorio, NVM usará automáticamente la versión especificada en el archivo.

## 10. NPM, package.json y package-lock.json

### NPM (Node Package Manager)

**¿Qué es?**
NPM es el gestor de paquetes por defecto para Node.js. Es una herramienta de línea de comandos que ayuda a los desarrolladores a descubrir, compartir e instalar paquetes de código reutilizable (llamados "módulos" o "dependencias").

**Funciones principales:**
*   **Instalar paquetes**: `npm install <nombre-paquete>` (ej. `npm install express`).
*   **Desinstalar paquetes**: `npm uninstall <nombre-paquete>`.
*   **Ejecutar scripts**: `npm run <nombre-script>` (definidos en `package.json`).
*   **Gestionar dependencias**: A través del archivo `package.json`.

### package.json

Es el corazón de cualquier proyecto de Node.js. Es un archivo JSON que contiene metadatos sobre el proyecto y gestiona sus dependencias.

**Contenido principal:**
*   `name`: El nombre del proyecto.
*   `version`: La versión actual del proyecto (sigue el versionado semántico).
*   `description`: Una breve descripción del proyecto.
*   `main`: El punto de entrada principal del proyecto (ej. `index.js`).
*   `scripts`: Un diccionario de scripts que se pueden ejecutar con `npm run`. Por ejemplo, `"start": "node index.js"`.
*   `dependencies`: Una lista de los paquetes que el proyecto necesita para funcionar en producción.
*   `devDependencies`: Una lista de los paquetes que solo se necesitan para el desarrollo y las pruebas (ej. `jest`, `eslint`).

### package-lock.json

**¿Qué es?**
Este archivo se genera automáticamente cada vez que `npm` modifica el `node_modules` o el `package.json`. Registra la versión exacta, la ubicación y la integridad (un hash de seguridad) de cada paquete y de todas sus dependencias.

**Propósito principal: Instalaciones deterministas.**
El `package.json` puede tener rangos de versiones (ej. `^4.17.1`). Esto significa que si alguien instala las dependencias de tu proyecto, podría obtener una versión ligeramente diferente a la que tú usaste, lo que podría introducir errores.

El `package-lock.json` soluciona esto. Cuando se ejecuta `npm install`, `npm` usa el `package-lock.json` para instalar las versiones *exactas* de los paquetes que se usaron la última vez, asegurando que todos los desarrolladores del equipo y los entornos de producción tengan exactamente el mismo árbol de dependencias.

**En resumen:**
*   **`package.json`**: Define las dependencias que *quieres* para tu proyecto (a menudo con rangos de versión).
*   **`package-lock.json`**: Describe el árbol de dependencias *exacto* que se generó en un momento dado.

**Importante**: Siempre debes subir el archivo `package-lock.json` a tu control de versiones (como Git).

## 11. Versionado Semántico (Semantic Versioning)

El versionado semántico (SemVer) es un estándar de numeración de versiones que tiene como objetivo comunicar el tipo de cambios que se han introducido en un paquete de software. El formato es `MAYOR.MENOR.PARCHE` (ej. `1.4.2`).

*   **`MAYOR` (Major)**: Se incrementa cuando se introducen cambios incompatibles con la API (breaking changes). Si un usuario actualiza a una nueva versión mayor, es posible que necesite hacer cambios en su propio código para que siga funcionando.

*   **`MENOR` (Minor)**: Se incrementa cuando se añade nueva funcionalidad de una manera que es compatible con versiones anteriores (backwards-compatible). Los usuarios pueden actualizar sin miedo a que su código existente se rompa.

*   **`PARCHE` (Patch)**: Se incrementa cuando se hacen correcciones de errores que son compatibles con versiones anteriores.

### Símbolos en `package.json`

En el `package.json`, a menudo verás símbolos junto a los números de versión:

*   **Caret (`^`)**: `^1.4.2` significa que se aceptan actualizaciones de parche y menores, pero no mayores. Es decir, `npm` puede instalar cualquier versión desde `1.4.2` hasta (pero sin incluir) `2.0.0`. Esta es la opción por defecto de `npm install <paquete> --save`.

*   **Tilde (`~`)**: `~1.4.2` significa que se aceptan solo actualizaciones de parche. Es decir, `npm` puede instalar cualquier versión desde `1.4.2` hasta (pero sin incluir) `1.5.0`.

*   **Sin símbolo**: `1.4.2` significa que se debe usar esa versión exacta.

El uso de SemVer y estos símbolos permite a los desarrolladores gestionar sus dependencias de forma segura, beneficiándose de las correcciones de errores y nuevas características sin introducir cambios que rompan su aplicación inesperadamente.

## 12. Spread Operator (`...`)

El "spread operator" o sintaxis de propagación (`...`) es una característica de ES6 que permite que un elemento iterable (como un array o un string) se expanda en lugares donde se esperan cero o más argumentos (para llamadas a funciones) o elementos (para literales de array), o que un objeto se expanda en lugares donde se esperan cero o más pares de clave-valor (para literales de objeto).

### Uso con Arrays

1.  **Copiar un array**: Permite crear una copia superficial de un array.

    ```javascript
    const original = [1, 2, 3];
    const copia = [...original];
    copia.push(4);

    console.log(original); // [1, 2, 3]
    console.log(copia);    // [1, 2, 3, 4]
    ```

2.  **Combinar arrays**:

    ```javascript
    const arr1 = [1, 2];
    const arr2 = [3, 4];
    const combinado = [...arr1, ...arr2];

    console.log(combinado); // [1, 2, 3, 4]
    ```

3.  **Pasar elementos como argumentos a una función**:

    ```javascript
    function sumar(a, b, c) {
        return a + b + c;
    }

    const numeros = [1, 2, 3];
    const resultado = sumar(...numeros);

    console.log(resultado); // 6
    ```

### Uso con Objetos

1.  **Copiar un objeto**: Permite crear una copia superficial de un objeto.

    ```javascript
    const original = { nombre: "Juan", edad: 30 };
    const copia = { ...original };
    copia.edad = 31;

    console.log(original); // { nombre: 'Juan', edad: 30 }
    console.log(copia);    // { nombre: 'Juan', edad: 31 }
    ```

2.  **Combinar objetos**:

    ```javascript
    const obj1 = { nombre: "Juan" };
    const obj2 = { edad: 30 };
    const combinado = { ...obj1, ...obj2 };

    console.log(combinado); // { nombre: 'Juan', edad: 30 }

    // Si hay propiedades con el mismo nombre, la última propiedad prevalece.
    const obj3 = { nombre: "Ana" };
    const combinado2 = { ...obj1, ...obj3 };
    console.log(combinado2); // { nombre: 'Ana' }
    ```

### Parámetros Rest

La sintaxis de `...` también se puede usar como "parámetros rest" en las declaraciones de funciones. En este caso, agrupa un número indefinido de argumentos en un array.

```javascript
function miFuncion(primerArg, ...elRestoDeArgs) {
    console.log(primerArg); // "uno"
    console.log(elRestoDeArgs); // ["dos", "tres", "cuatro"]
}

miFuncion("uno", "dos", "tres", "cuatro");
```

El spread operator es una herramienta muy versátil y de uso común en el JavaScript moderno para trabajar de manera inmutable y funcional.

## 13. Desestructuración (Destructuring)

La desestructuración es una sintaxis que permite desempacar valores de arrays o propiedades de objetos en distintas variables. Es como una forma inversa al spread operator.

### Desestructuración de Objetos

Permite extraer propiedades de un objeto y asignarlas a variables con el mismo nombre.

```javascript
const usuario = {
    nombre: "Ana",
    edad: 28,
    pais: "Colombia"
};

// Extraemos 'nombre' y 'edad' del objeto usuario
const { nombre, edad } = usuario;

console.log(nombre); // "Ana"
console.log(edad);   // 28
```

**Renombrar variables:**

```javascript
const { nombre: nombreCompleto, pais: nacion } = usuario;

console.log(nombreCompleto); // "Ana"
console.log(nacion);       // "Colombia"
```

**Valores por defecto:**

```javascript
const { profesion = "Desarrolladora" } = usuario;
console.log(profesion); // "Desarrolladora"
```

### Desestructuración de Arrays

Permite desempacar valores de un array en el orden en que aparecen.

```javascript
const colores = ["rojo", "verde", "azul"];

const [primerColor, segundoColor] = colores;

console.log(primerColor);  // "rojo"
console.log(segundoColor); // "verde"
```

**Saltar elementos:**

```javascript
const [,,tercerColor] = colores;
console.log(tercerColor); // "azul"
```

**Uso con parámetros rest:**

```javascript
const [colorPrincipal, ...otrosColores] = colores;

console.log(colorPrincipal); // "rojo"
console.log(otrosColores);   // ["verde", "azul"]
```

La desestructuración es extremadamente útil para escribir código más limpio y legible, especialmente al trabajar con objetos de configuración, respuestas de APIs o al pasar props en frameworks como React.

## 14. Template Literals (Plantillas de texto)

Los template literals son cadenas de texto que permiten expresiones incrustadas. Se delimitan con el acento grave (`` ` ``) en lugar de comillas simples o dobles.

### Características Principales

1.  **Interpolación de expresiones**: Permiten inyectar variables o cualquier expresión de JavaScript directamente en la cadena de texto usando la sintaxis `${expresion}`.

    ```javascript
    const nombre = "Mundo";
    const saludo = `Hola, ${nombre}!`;
    console.log(saludo); // "Hola, Mundo!"

    const a = 5;
    const b = 10;
    console.log(`El resultado de ${a} + ${b} es ${a + b}`); // "El resultado de 5 + 10 es 15"
    ```

2.  **Cadenas multilínea**: Permiten crear cadenas de texto que abarcan varias líneas sin necesidad de caracteres de escape como `\n`.

    ```javascript
    const mensaje = `Esta es una cadena
    que ocupa varias
    líneas.`;

    console.log(mensaje);
    // Salida:
    // Esta es una cadena
    // que ocupa varias
    // líneas.
    ```

Los template literals hacen que el manejo de cadenas de texto sea mucho más limpio, legible y potente en comparación con la concatenación tradicional con el operador `+`.

## 15. Módulos (import/export)

Los módulos de ES6 permiten dividir el código en archivos separados y reutilizables. Esto ayuda a organizar el código, mantenerlo limpio y evitar la contaminación del scope global.

*   **`export`**: Se usa para exponer funciones, objetos o valores primitivos desde un archivo (módulo) para que puedan ser utilizados por otros archivos.
*   **`import`**: Se usa para traer a nuestro archivo actual las funcionalidades que otro módulo ha exportado.

### Tipos de Exportación

1.  **Exportaciones nombradas (Named Exports)**: Permiten exportar múltiples valores desde un módulo. Al importar, se debe usar el mismo nombre (o un alias).

    ```javascript
    // 📁 lib.js
    export const PI = 3.1416;
    export function sumar(a, b) {
        return a + b;
    }
    ```

    ```javascript
    // 📁 main.js
    import { PI, sumar } from './lib.js';
    // Usando un alias
    import { PI as numeroPi } from './lib.js';

    console.log(PI); // 3.1416
    console.log(sumar(2, 3)); // 5
    ```

2.  **Exportación por defecto (Default Export)**: Permite exportar un solo valor como el valor por defecto del módulo. Solo puede haber una exportación por defecto por archivo. Al importar, se le puede dar cualquier nombre.

    ```javascript
    // 📁 miComponente.js
    export default function MiComponente() {
        // ...
    }
    ```

    ```javascript
    // 📁 app.js
    import ComponentePrincipal from './miComponente.js';
    ```

Para que los módulos funcionen en el navegador, es necesario agregar `type="module"` a la etiqueta `<script>` en el HTML:

```html
<script type="module" src="./app.js"></script>
```

Los módulos son la base de la arquitectura de las aplicaciones modernas de JavaScript.

## 16. El `this` en Profundidad

La palabra clave `this` es una de las características más potentes y a la vez más confusas de JavaScript. Su valor se determina por el **contexto de ejecución** (cómo se llama a la función).

### Reglas de `this`

1.  **Contexto Global**: Fuera de cualquier función, `this` se refiere al objeto global. En un navegador, es `window`.

    ```javascript
    console.log(this === window); // true
    ```

2.  **Método de un Objeto**: Cuando una función es llamada como un método de un objeto, `this` se refiere al objeto que contiene el método.

    ```javascript
    const persona = {
        nombre: "Carlos",
        saludar: function() {
            console.log(`Hola, soy ${this.nombre}`);
        }
    };

    persona.saludar(); // "Hola, soy Carlos" (this es 'persona')
    ```

3.  **Llamada de Función Simple**: Cuando se llama a una función de forma simple (sin ser un método de un objeto), `this` se refiere al objeto global (`window`) en modo no estricto, o `undefined` en modo estricto (`'use strict'`).

    ```javascript
    function quienSoy() {
        console.log(this); // window (o undefined en modo estricto)
    }

    quienSoy();
    ```

4.  **Arrow Functions**: Como se mencionó antes, las arrow functions no tienen su propio `this`. Heredan el `this` del contexto en el que fueron creadas. Esto las hace muy predecibles y útiles dentro de métodos de objetos o callbacks.

    ```javascript
    const coche = {
        marca: "Ford",
        obtenerMarca: function() {
            setTimeout(() => {
                // 'this' aquí es el mismo que en obtenerMarca (el objeto 'coche')
                console.log(this.marca);
            }, 100);
        }
    };

    coche.obtenerMarca(); // "Ford"
    ```

5.  **Constructores**: Cuando una función se usa como constructor (con `new`), `this` se refiere al nuevo objeto que se está creando.

    ```javascript
    function Coche(marca) {
        this.marca = marca;
    }

    const miCoche = new Coche("Toyota");
    console.log(miCoche.marca); // "Toyota" (this es 'miCoche')
    ```

6.  **Métodos `call`, `apply` y `bind`**: Permiten establecer explícitamente el valor de `this` para una función.
    *   `.call(thisArg, arg1, arg2, ...)`: Llama a la función con un `this` específico y argumentos pasados individualmente.
    *   `.apply(thisArg, [arg1, arg2, ...])`: Similar a `call`, pero los argumentos se pasan como un array.
    *   `.bind(thisArg)`: Crea una *nueva* función que, cuando sea llamada, tendrá su `this` permanentemente establecido al valor proporcionado.

    ```javascript
    function presentarse(saludo, puntuacion) {
        console.log(`${saludo}, soy ${this.nombre} y mi puntuación es ${puntuacion}`);
    }

    const jugador = { nombre: "Mario" };

    presentarse.call(jugador, "Hola", 100); // "Hola, soy Mario y mi puntuación es 100"
    presentarse.apply(jugador, ["Qué tal", 99]); // "Qué tal, soy Mario y mi puntuación es 99"

    const presentarAJugador = presentarse.bind(jugador);
    presentarAJugador("Hey", 150); // "Hey, soy Mario y mi puntuación es 150"
    ```

## 17. Igualdad (`==` vs `===`)

JavaScript tiene dos operadores de igualdad, y entender su diferencia es crucial para evitar errores.

### `===` (Igualdad Estricta)

El operador de igualdad estricta, también conocido como "triple igual", compara dos valores para ver si son iguales. Se considera la forma más segura y predecible de comparar.

**Reglas:**
1.  Si los tipos de los dos operandos son diferentes, devuelve `false`.
2.  Si son del mismo tipo, compara sus valores.
    *   Para números, `NaN` no es igual a nada, ni siquiera a sí mismo.
    *   Para objetos, devuelve `true` solo si ambos operandos hacen referencia al *mismo objeto* en memoria.

```javascript
console.log(5 === 5);       // true
console.log('5' === 5);     // false (diferente tipo)
console.log(true === 1);    // false (diferente tipo)
console.log(null === null); // true
console.log(undefined === undefined); // true

const obj1 = { a: 1 };
const obj2 = { a: 1 };
const obj3 = obj1;

console.log(obj1 === obj2); // false (diferentes objetos en memoria)
console.log(obj1 === obj3); // true (misma referencia)
```

### `==` (Igualdad Débil o Abstracta)

El operador de igualdad débil, o "doble igual", compara dos valores después de realizar una **coerción de tipo** si los tipos son diferentes. Intenta convertir uno o ambos valores a un tipo común antes de hacer la comparación.

**Reglas (simplificadas):**
*   Si un operando es un string y el otro un número, convierte el string a número.
*   Si un operando es un booleano, lo convierte a `1` (para `true`) o `0` (para `false`).
*   Si un operando es un objeto y el otro es un primitivo, intenta convertir el objeto a un primitivo.

Este comportamiento puede llevar a resultados inesperados y es una fuente común de errores.

```javascript
console.log('5' == 5);       // true (string '5' se convierte a número 5)
console.log(true == 1);     // true (boolean true se convierte a número 1)
console.log('' == false);    // true (string vacío se convierte a 0, false se convierte a 0)
console.log(null == undefined); // true (una regla especial del lenguaje)
console.log(0 == false);      // true
```

### Conclusión

**Usa siempre `===` (igualdad estricta)** a menos que tengas una razón muy específica para querer la coerción de tipo que realiza `==`. El uso de `===` hace que tu código sea más predecible, más fácil de leer y menos propenso a errores sutiles.

---

## Fuentes y Recursos Adicionales

Aquí tienes una lista de recursos de alta calidad para profundizar en los temas que hemos cubierto y para practicar con otras APIs.

### Libros y Guías Fundamentales
*   [Eloquent JavaScript (en español)](https://www.eloquentjavascript.es/): Un libro fantástico y completo para dominar JavaScript, desde los fundamentos hasta temas avanzados.

### Documentación Oficial y Diseño de APIs
*   [Guía de inicio de Express.js](https://expressjs.com/es/starter/installing.html): La documentación oficial es el mejor lugar para aprender todas las capacidades de Express.
*   [Guías de Node.js](https://nodejs.org/es/docs/guides): Guías oficiales sobre temas clave de Node.js.
*   [Visión general de HTTP (MDN)](https://developer.mozilla.org/es/docs/Web/HTTP/Overview): Entender el protocolo HTTP es fundamental para cualquier desarrollador de APIs. La Mozilla Developer Network (MDN) es una referencia de máxima calidad.

---

## Conclusión

Este repositorio te ha mostrado las bases fundamentales de javascript para inciar con el desarrollo web moderno.