# devtalles-principios-solid-clean-code

## Sección 1. Introducción

### Description

- https://github.com/ryanmcdermott/clean-code-javascript
- https://github.com/devictoribero/clean-code-javascript

### Referencias

- [Clean Javascript - Miguel A. Gómez](https://github.com/patchamama/clean-javascript-book), [libro](https://cleanjavascript.es/)
- [Refactoring Guru: Khmelnitske shosse](https://refactoring.guru/)

## Sección 2. Clean Code y Deuda técnica

_Deuda técnica (technical debt): la falta de calidad en el código, que genera una deuda que repercutirá en costos futuros._ La deuda técnica se paga con **REFACTORIZACIÓN**.

Refactorización: Es simplemente un proceso que tiene como objetivo mejorar el código sin alterar su comportamiento para que sea entendible y tolerante a cambios. *Para ello el código debe de tener una buena batería de tests automáticos*.

Sí no hay pruebas automáticas, muchas veces se genera el problema de "sí funciona, no lo toques" debido al miedo a hacer cambios y que se rompa el software. 

Costos económicos: **Tiempo** en realizar mantenimientos, refactorizar código, comprender el código y tiempo adicional en la transferencia del código

> "Código Limpio es aquel que se ha escrito con la intención de que otra persona (o tú mismo en el futuro) lo entienda". - Carlos Blé
> 
> "Nuestro código tiene que ser simple y directo. Debería de leerse con la misma facilidad que un texto bien escrito". - Grady Booch
>
> "Programar es el arte de decirle a otro humano lo que quieres que la computadora haga". - Donald Knuth

### Nombre pronunciables y expresivos

Los nombres de variables deben ser en inglés y expresivos con sentido semántico. Cada lenguaje tiene sus convenciones (camelCase, snake_case,...)

```js
// Mal
const n = 53;
const tx = 0.15;
const cat = 'T-Shirts';
const ddmmyyyy = new Date('August 15, 1965 00:00:00')

// Mejor
const numberOfUnits = 53;
const tax = 0.15;
const category = 'T-Shirts';
const birthDate = new Date('August 15, 1965 00:00:00')
```

Eliminar información técnica en los nombres de clases, tipos,.. pues ya está implícito en el tipo de dato:

```js
// Malos
class AbstractUser { };
class UserMixin { };
class UserImplementation { };
interface IUser { };

// Mejor
class User { };
interface User { };

//--------------------------------------

// Mal
// Archivos a evaluar - files to evaluate
const fs = [
    { id: 1, f: false },
    { id: 2, f: false },
    { id: 3, f: true },
    { id: 4, f: false },
    { id: 5, f: false },
    { id: 7, f: true },
]; 
// Archivos marcados para borrar - files to delete
const archivos = fs.map( f => f.f );

// Mejor
const filesToEvaluate = [
    { id: 1, flagged: false },
    { id: 2, flagged: false },
    { id: 3, flagged: true },
    { id: 4, flagged: false },
    { id: 5, flagged: false },
    { id: 7, flagged: true },
]; 
const filesToDelete = filesToEvaluate.map( file => file.flagged );
```

### Nombres según el tipo de dato

> Sí las definiciones de variables o constantes van a acompañadas de comentarios que explican la razón de la variable, entonces eso es un mal indicador pues no sería necesario esto y de por sí las variables deberían de explicarse por sí solas con su nombre, de la funcionalidad que tienen pro sí solas. 

```js
// Arreglos, usar la descripción en plural y describiendo que contiene el arreglo en la variable
// malo
const fruit=['manzana','platano','fresa']

//regular
const fruitList=['manzana','platano','fresa']

//bueno
const fruits=['manzana','platano','fresa']

//mejor (pues se sabe literalmente lo que hay que son nombres de frutas)
const fruitNames=['manzana','platano','fresa']

// ----------------------------
// Booleanos mejor en forma de pregunta para que tenga un mejor valor semántico
// mal
const open = true;
const write = true;
const fruit = true;
const active = false;
const noValues = true;
const notEmpty = false;

// mejor
const isOpen = true;
const canWrite = true;
const hasFruit = true;
const isActive = false;
const hasValues = false;
const isEmpty = true;

// ----------------------------
// Números
// mal
const fruits = 3;
const cars = 10;

// mejor
const maxFruits = 5;
const minFruits = 1;
const totalFruits = 3;
const totalOfCars = 5;

// ----------------------------
// funciones: deben de representar acciones que por lo general debe de ser un verbo seguido por un sustantivo, 
// el nombre de la función debe de ser descriptivo y conciso, debe de expresar lo que hace pero debe de abstenerse
// de todo lo que hace la función.
// mal
createUserIfNotExists(); 
updateUserIfNotEmpty();
sendEmailIfFieldsValid();

//mejor
createUser();
updateUser();
sendEmail();
```

### Consideraciones para las clases

Las clases deben de tener nombres formados por un sustantivo o frases de sustantivos. Debemos de evitar nombres genéricos porque pueden llevar a que las clases realizan más trabajo del que real deben de hacer. Se debe de usar UpperCamelCase. Más palabra !== mejor nombre.

3 preguntas para determinar saber sí una clase usa un buen nombre:
- ¿Qué exactamente hace la clase?
- ¿Cómo exactamente esta clase realiza cierta tarea?
- ¿Hay algo específico sobre su ubicación?

> Si algo no tiene sentido, remuévalo o refactoriza.

```js
// malos (son muy genéricos)
class Manager {};
class Data {};
class Info {};
class Individual {};
class Processor {};
class SpecialMonsterView {};
```

### Nombres de funciones, argumentos y parámetros

> Sabemos que estamos trabajando con código limpio sí la función hace exactamente lo que el nombre de la función indica.

> Se recomienda que se limite el número máximo de parámetros a 3, pues sino se hace más complicado de leer.  

```js
function sendEmail( toWhom: string, from: string, body: string, subject: string, apikey: string): boolean {}

//mejor usando una interfase con reestructuración y además se puede especificar el orden de los parámetros cómo uno lo desee
interface SendEmailOptions {
    toWhom: string; 
    from: string; 
    body: string; 
    subject: string; 
    apikey: string;
}
function sendEmail( {toWhom, from, body, subject, apikey} : SendEmailOptions): boolean {}
```

### Detalles adicionales sobre funciones

> Otras recomendaciones:
> - Simplicidad es fundamental
> - Funciones de tamaño reducido (de lo contrario hace más de lo que debía hacer)
> - Prioridad a funciones de una sola línea sin causar complejidad innecesaria
> - Recomendación de menos de 20 líneas de código
> - Evitar el uso del "else" a menos de que sea estrictamente necesario
> - Priorizar el uso de la condicional ternaria ( `condición ? result_true : result_false;`)


### Principio DRY (Don't Repeat Yourself)

> "Sí quieres ser un programador productivo, esfuérzate en escribir código legible". - Robert C. Martin

> DRY:
> - Simplemente es evitar tener duplicidad de código,
> - Simplifica las pruebas,
> - Ayuda a centralizar procesos,
> - Aplicar el principio DRY, usualmente lleva a refactorizar

```js
type Size = ''| 'S'|'M'|'XL';

class Product {
    constructor(
        public name: string = '',
        public price: number = 0,
        public size: Size = '',
    ){}

    isProductReady(): boolean {    
        for( const key in this ) {
            switch( typeof this[key] ) {
                case 'string':
                    if ( (<string><unknown>this[key]).length <= 0 ) throw Error(`${ key } is empty`);
                break;
                case 'number':
                    if ( (<number><unknown>this[key]) <= 0 ) throw Error(`${ key } is zero`);
                break;
                default:
                    throw Error(`${ key } with type ${ typeof this[key] } is not valid`);
            }
        }
        return true;
    }

    toString() {    
        //no con principio DRY
        // if (this.name.length <= 0) throw Error('name is empty')
        // if (this.price <= 0) throw Error('price is zero')
        // if (this.size.length <= 0) throw Error('name is empty')

        // con principio DRY
        if ( !this.isProductReady ) return;
        return `${ this.name } (${ this.price }), ${ this.size }`
    }
}

(()=> {
    const bluePants = new Product('Blue Pants', 10,'S');
    console.log(bluePants.toString());
})();
```

## Sección 3. Clean Code - Clases y comentarios

Los comentarios deben de explicar cosas como la razón de tomar algún camino y no otra solución, por qué se decide hacer una función de una forma y no de otra, es decir, cuando en sí el nombre de la función o variable no se autoexplicativa. 

### Breve introducción a las clases en TypeScript

```js
// Forma larga de definir una clase en TS
type Gender = 'M'|'F';

class Person {
    public name: string, 
    public gender: Gender, 
    public birthdate: Date
    constructor(name: string, gender: Gender, birthdate: Date){
        this.name = name, 
        this.gender = Gender, 
        this.birthdate = birthdate;
    }
}

const newPerson = new Person('Fernando', 'M', new Date('1985-10-21'))
console.log({ newPerson });
```

### Herencia - Problemática

En el siguiente ejemplo se genera una clase padre Person de la que heredan dos clases que a su vez al crearse una constante que llame a estas, deben de pasarse los parámetros de la clase hija y los de la clase padre a su vez pues está heredando ciertas propiedades y esto puede ser muy trabajoso, sobretodo sí se llama como en el ejemplo un `new UserSettings()` por todos los parámetros que hay que pasarle.

```js
// Forma corta de definir una clase en TS
type Gender = 'M'|'F';

class Person {
    constructor(
        public name: string, 
        public gender: Gender, 
        public birthdate: Date
    ){}
}

class User extends Person {
    public lastAccess: Date;
    constructor(
        public email: string,
        public role: string,
        name: string,
        gender: Gender,
        birthdate: Date,
    ) {
        super( name, gender, birthdate );  // llama al constructor de la clase padre
        this.lastAccess = new Date();
    }

    checkCredentials() {
        return true;  // revisaría sí está autenticado
    }
}

class UserSettings extends User {
    constructor(
        public workingDirectory: string,
        public lastOpenFolder  : string,
        email                  : string,
        role                   : string,
        name                   : string,
        gender                 : Gender,
        birthdate              : Date
    ) {
        super(email, role, name, gender, birthdate );
    }
}

const userSettings = new UserSettings(
    '/usr/home',
    '/home',
    'fernando@google.com',
    'Admin',
    'Fernando',
    'M',
    new Date('1985-10-21')
);

    console.log({ userSettings });
```

### Objetos como propiedades

Vamos a refactorizar y evitar tener que mandar siempre todos los valores de las propiedades al crearse un objeto declarando las propiedades dentro de este y además vamos a desestructurar y crear interfaces.

```js
// No aplicando el principio de responsabilidad única

type Gender = 'M'|'F';

interface PersonProps {
    birthdate : Date;
    gender    : Gender;
    name      : string;
}

class Person {
    public birthdate: Date;
    public gender   : Gender;
    public name     : string;

    constructor({ name, gender, birthdate }: PersonProps ){
        this.name      = name;
        this.gender    = gender;
        this.birthdate = birthdate;
    }
}


interface UserProps {
    birthdate : Date;
    email     : string;
    gender    : Gender;
    name      : string;
    role      : string;
}

class User extends Person {
    public email: string;
    public role : string;
    public lastAccess: Date;

    constructor({
        birthdate,
        email,
        gender,
        name,
        role,
    }: UserProps ) {
        super({ name, gender, birthdate });
        this.lastAccess = new Date();
        this.email = email;
        this.role  = role;
    }

    checkCredentials() {
        return true;
    }
}


interface UserSettingsProps {
    birthdate        : Date;
    email            : string;
    gender           : Gender;
    lastOpenFolder   : string;
    name             : string;
    role             : string;
    workingDirectory : string;
}

class UserSettings extends User {

    public workingDirectory: string;
    public lastOpenFolder  : string;

    constructor({
        workingDirectory,
        lastOpenFolder,
        email,
        role,
        name,
        gender,
        birthdate,
    }: UserSettingsProps ) {
        super({ email, role, name, gender, birthdate });
        this.workingDirectory = workingDirectory;
        this.lastOpenFolder   = lastOpenFolder;
    }
}


const userSettings = new UserSettings({
    birthdate: new Date('1985-10-21'),
    email: 'fernando@google.com',
    gender: 'M',
    lastOpenFolder: '/home',
    name: 'Fernando',
    role: 'Admin',
    workingDirectory: '/usr/home',
});

console.log({ userSettings });
```

### Principio de responsabilidad única

A menos de que sea necesario, hay que evitar las herencias (extends) **priorizando la composición frente a la herencia**. 

`UserSettings` es una clase composición que va a integrar a las demás clases sin heredarlas, o que es una composición de las otras clases.
Ahora cada una de las clases tienen un principio de responsabilidad única al ser instancias propias y es más fácil de entender/leer.

```js
// Aplicando el principio de responsabilidad única
// Priorizar la composición frente a la herencia!

type Gender = 'M'|'F';

interface PersonProps {
    birthdate : Date;
    gender    : Gender;
    name      : string;
}

class Person {
    public birthdate: Date;
    public gender   : Gender;
    public name     : string;

    constructor({ name, gender, birthdate }: PersonProps ){
        this.name      = name;
        this.gender    = gender;
        this.birthdate = birthdate;
    }
}


interface UserProps {
    email     : string;
    role      : string;
}

class User { // el extend anterior se eliminó
    public email      : string;
    public lastAccess : Date;
    public role       : string;

    constructor({
        email,
        role,
    }: UserProps ) {
        // no se llama al super() pues no hay herencia aquí
        this.lastAccess = new Date();
        this.email = email;
        this.role  = role;
    }

    checkCredentials() {
        return true;
    }
}


interface SettingsProps {
    lastOpenFolder   : string;
    workingDirectory : string;
}

class Settings {  // Se eliminó el extends pues no hace falta la herencia
    public workingDirectory: string;
    public lastOpenFolder  : string;

    constructor({
        lastOpenFolder,
        workingDirectory,
    }: SettingsProps ) {
        this.lastOpenFolder   = lastOpenFolder;
        this.workingDirectory = workingDirectory;
    }
}


interface UserSettingsProps {
    birthdate        : Date;
    email            : string;
    gender           : Gender;
    lastOpenFolder   : string;
    name             : string;
    role             : string;
    workingDirectory : string;
}

class UserSettings {
    public person  : Person;
    public user    : User;
    public settings: Settings;

    constructor({
        name, gender, birthdate,
        email, role,
        lastOpenFolder, workingDirectory,
    }: UserSettingsProps ){

        this.person = new Person({ name, gender, birthdate });
        this.user = new User({ email, role });
        this.settings = new Settings({ lastOpenFolder, workingDirectory })
    }
}


const userSettings = new UserSettings({
    birthdate: new Date('1985-10-21'),
    email: 'fernando@google.com',
    gender: 'M',
    lastOpenFolder: '/home',
    name: 'Fernando',
    role: 'Admin',
    workingDirectory: '/usr/home',
});

console.log({ userSettings });
```

### Estructura recomendada de una clase

> "El buen código parece estar escrito por alquien a quien le importa". - Michael Feathers

La estructura de las clases es un acuerdo que toma el equipo.

Primero se deben de definir las propiedades, así:
1. Propiedades estáticas (`public static variableName: type;`),
2. Propiedades públicas (`private variableName: type;`).
3. Propiedades privadas

Después los métodos:
1. Empezar por los constructores estáticos (`static nameFunction(parámetros) {}`)
2. Luego el constructor (`constructor (parámetros)`) y luego constructores privados de existir.
3. Seguidamente los métodos estáticos (`functionName (parámetros) {}`)
4. Métodos privados después.
5. Resto de métodos de instancia ordenados de mayor a menor importancia.
6. Getters y Setters al final (ejemplo: `get métodoName() : type {}`).

```js
class HtmlElement {
    public static domReady: boolean = false; //propiedad estática

    public isReadyOnly: boolean = false; // Propiedad pública

    private _id: string; // propiedades privadas
    private type: string;
    private updatedAt: number: 

    static createInput( id: string ) { // constructor estático (método estático que regresa una instancia de la clase)
        return new HtmlElement(id, 'input');
    }

    constructor( id: string, type: string ) { 
        this._id = id;
        this.type = type;
        this.updatedAt = Date.now();
    }

    setType( type: string ) { // métodos estáticos
        this.type = type;
        this.updateAt = Date.now();
    }

    get id(): string { // getter
        return this.id;
    }
}
```

### Comentarios en el código

Cuando necesites añadir comentarios a tu código, es porque no es lo suficientemente autoexplicativo, lo cual quiere decir que no estamos escogiendo nuevos nombres, por lo que seguramente se hace necesario refactorizar el código.
Cuando usamos librerías de terceros, APIS, frameworks, etc., nos encontramos ante situaciones en las que escribir un comentario será mejor que dejar una solución compleja o un hack sin explicación. 

**Los comentarios deberían de ser la excepción, no la regla**.

> "No comentes el código mal escrito, reescríbelo". - Brian W. Kernighan

Nuestro código debe de ser suficientemente auto explicativo. Sí fuera necesario comentar algo, debería de ser el ¿por qué? en lugar del ¿qué? o ¿cómo? pues esto último ya lo muestra el código (el cómo), pero por qué has decidido hacer algo de una determinada forma sí puede ser comentado.

### Uniformidad en el proyecto

> Problemas similares, soluciones similares.

Significa que se debe de usar la misma estructura de los directorios. Los frameworks brindan estándares para nombrar los `filesystem` y establecen cómo nombrar los archivos, en qué carpeta guardarlos, la estructura de los mismos. 
También es importante respetar la identación que elija el equipo de desarrollo (espacios), es decir, las convenciones pactadas para identar los bloques de códigos. Normalmente se usa un determinado autoformateado del equipo, seguir los estándares del lenguaje de desarrollo (python).

## Sección 4. Acrónimo - STUPID

Aquí se analizan los antipatrones, es decir, las cosas que no se deben de hacer. Aquí se incluye todo lo llamado `code smells`, es decir, código que huele mal.

### CodeSmells - STUPID

Acrónimo STUPID. El concepto está relacionado con la deuda ténica.

6 Code Smells que debemos de evitar:

- **S**ingleton: patrón singleton.
- **T**ight Coupling: alto acoplamiento (y baja cohesión).
- **U**ntestability: código no probable o testeable (unit test).
- **P**remature optimization: optimizaciones prematuras.
- **I**ndescriptive Naming: nombres poco descriptivos.
- **D**uplication: duplicidad de código, no aplicar el principio DRY.

**Patrón Singleton**

PROS:
- Garantiza una única instancia de la clase a lo largo de toda la aplicación. 

CONS: (¿por qué code smell?)
- Vive en el contexto global.
- Puede ser modificado por cualquiera y en cualquier momento.
- No es rasteable.
- Difícil de testear debido a su ubicación.

Ejemplo [aquí](./src/code-smells/01-singleton.js).


### Acoplamiento y cohesión

- **T**ight Coupling: alto acoplamiento y pésima cohesión (en realidad lo ideal es tener un bajo acomplamiento y una alta cohesión).

Desventajas del alto acoplamiento:
- Un cambio en un módulo por lo general provoca un efecto dominó de los cambios en otros módulos,
- El ensamblaje de módulos puede requerir más esfuerzo y/o tiempo debido a la mayor dependencia enre módulos,
- Un módulo en particular puede ser más difícil de reutilizar y/o probar porque se deben incluir módulos dependientes.

> "Queremos diseñar componentes que sean autocontenidos, autosuficientes e independientes. Con un objetivo y un propósito bien definido". - The pragmatic programer

**Cohesión**
Lo ideal es tener bajo acoplamiento y buena o alta cohesión.
- **La cohesión se refiere a lo que la clase (o módulo) puede hacer.
- La baja cohesión significaría que la clase realiza una gran variedad de acciones: es amplia, no se enfoca en lo que debe hacer.
- Alta cohesión significa que la clase se enfoca en lo que debería estar haciendo, es decir, solo métodos relacionados con la intención de la clase.

**Acoplamiento**
Lo ideal es tener bajo acomplamiento y buena cohesión.
- **Se refiere a cuán relacionadas o dependientes son dos clases o módulos entre sí** 
- *En bajo acoplamiento*, cambiar algo importante en una clase no debería afectar a la otra.
- *En alto acoplamiento*, dificultaría el cambio y el mantenimiento de su código; dado que las clases están muy unidas, hacer un cambio podría requerir una renovación completa del sistema,

> Un buen diseño de software tiene alta cohesión y bajo acoplamiento

En [este](./src/code-smells/02-high-coupling.ts) ejemplo sí se cambia la clase Person y se cambia por ejemplo, el nombre de la variable `lastName` por otro nombre, esto determina que hay que modificar las clases que heredan sus propiedades (pues tienen un alto acomplamiento) cambiando esta propiedad en las clases hijas.  

En [este](./src/code-smells/02-low-coupling.ts) archivo hay un acoplamiento menor, lo que determina que con menos cambios se alcanza lo que se desea, por ejemplo sí se desea agregar la propiedad `firsName` y renombrar la propiedad `lastName`.

### Code Smells adicionales

**Código no problable**
- Código difícilmente testeable,
- Código con alto acoplamiento,
- Código con muchas dependencias no inyectadas,
- Dependencias en el contexto global (Tipo Singleton)

> Debemos de tener en mente las pruebas desde la creación del código.

**Optimizaciones prematuras**
- Mantener abiertas las opciones retrasando la toma de decisiones nos permite darle mayor relevancia a lo que es más importante en una aplicación.
- No debemos anticiparnos a los requisitos y desarrollar abstracciones innecesarias que puedan añadir complejidad accidental.

Complejidad accidental: Cuando implementamos una solución compleja a la mínima indispensable
Complejidad esencial: La complejidad es inherente al problema.
Se debe de encontrar un equilibrio entre estas complejidades.

Nombres poco descriptivos:
- Nombres de variables mal nombradas,
- Nombres de clases genéricas,
- Nombres de funciones mal nombradas,
- Ser muy específico o demasiado genérico.

Duplicidad de código: No aplicar el principio DRY:
Duplicidad Real:
- Código es idéntico y cumple la misma función,
- Un cambio implicaría actualizar todo el código idéntico en varios lugares,
- Incrementa posibilidades de error humano al olvidar una parte para actualizar,
- Mayor cantidad de pruebas innecesarias.

Duplicidad accidental:
- Código luce similar pero cumple funciones distintas,
- Cuando hay un cambio, sólo hay que modificar un sólo lugar,
- Este tipo de duplicidad se puede trabajar con parámetros u optimizaciones.

### Otros olores honoríficos (extraídos sobretodo de refactoring.guru)

- Inflación: Cuando un método tiene muchas líneas de código (> 10 líneas) se debería de pensar en hacerlo un poco más pequeño. Usualmente lo que se hace es cortar este método en pequeños submétodos en dependencia a pequeñas tareas. También está el caso de las clases supergrandes, que aumentan a nivel que el programa sigue creciendo junto con las características requeridas que cumpla, en este caso se pudieran separar las mismas en pequeñas subclases o módulos en dependencia de las subtareas requeridas. 
- Obsesión primitiva: el uso de primitivos o constante en lugar de objetos o clases nuevos por problemas de comodidad. En este caso el tratamiento es que sí hay gran cantidad de variables de tipo primitivo seguramente se pudieran reunir en una clase u objeto que quizás pudiera permitir su reuso en otra parte del programa.
- Lista larga de parámetros: más de 3 parámetros en una función o método. Sí se usan muchos parámetros, quizás alguno no sea necesario o se le está dando demasiada funcionalidad a la función, clase. Comprobar que todos los parámetros son estrictamente necesarios. Sí algún método es el resultado de un retorno de una función, quizás se pudiera llamar al mismo dentro de la función. 

### Acopladores

- Feature Envy (envidia de una característica): Un método, clase, módulo o función accede más a los métodos de otra clase, módulo o función, más que a los suyos propios. Se puede tratar como regla básica, quizás cambiar ese método o función en el lugar al que se referencia.
- Intimidad inapropiada: Cuando una clase usa campos o métodos internos de otra clase. Las buenas clases deben de saber lo menos posible de otras clases. 
- Cadena de mensajes: Tenemos una función A, que llama a B, que de B llama a C. Esto es un problema porque el cliente depende de la navegación de clases, de módulos, estructura. La solución es ver sí se puede eliminar toda esa cadena de comunicación, reduciendo las dependencias entre clases o módulos y se reduce la cantidad de código, permitiendo en el caso anterior que A acceda directamente a C.
- The middle man (el hombre del medio): Sí una clase realiza solo una acción y la acción es delegar en otra clase (entonces por qué existe esta clase?), esto puede ser el resultado de la eliminación de cadena de mensajes (punto anterior) o una refactorización incompleta. 


## Sección 5. Principios SOLID

Son principios (son recomendaciones) y no reglas (esta última es algo que hay que cumplir). 

Los 5 principios S.O.L.I.D. de diseño de software son:
- S – Single Responsibility Principle (SRP)
- O – Open/Closed Principle (OCP)
- L – Liskov Substitution Principle (LSP)
- I – Interface Segregation Principle (ISP)
- D – Dependency Inversion Principle (DIP)

### Principios SOLID - SRP - Responsabilidad única

Los principios de SOLID nos indican cómo organizar nuestras funciones y estructuras de datos en componentes y cómo dichos componentes deben estar interconectados.

> "Nunca debería haber más de un motivo por el cual cambiar una clase o un módulo". - Robert C. Martin

Tener más de una responsabilidad de nuestras clases o módulos, hace que nuestro código sea más difícil de mantener, testear y leer, y hace que el código sea más rígido y menos tolerante al cambio. 

"tener una única responsabilidad" !== "hacer una única cosa", queremos que nuestras clases y módulos se encarguen de hacer un grupo de procesos que estén estrechamente relacionados entre sí. El principio SRP no se base en crear clases de un solo método sino en diseñar componentes que estén solo expuestos a una fuente de cambios.

### Detectar incumplimiento de SRP (principio de resposabilidad única)

- Nombres de clases y módulos demasiado genéricos (por lo que tiene demasiada responsabilidad, lo que permite detectar que quizás una propiedad o método no debería incluirse en estos)
- Cambio en el código suelen afectar la clase o módulo,
- La case involucra múltiples capas (almacenamiento, comunicación con la bds, ...),
- Número elevado de importaciones (sí una clase o módulo abstracto hace una tarea específica, no debe de tener muchas importaciones)
- Cantidad elevada de métodos públicos (demasiados métodos expuestos al mundo exterior, lo que determina que está haciendo demasiadas cosas),
- En la clase o módulo hay un excesivo número de líneas de código, lo que determina que pueda tener demasiada responsabilidad.

### OCP - Principio de abierto y cerrado

Es un principio que depende mucho del contexto y establece que las entidades de software (clases, módulos, métodos, etc.) deben estar abiertas para la extensión, pero cerradas para la modificación. Este principio también se puede lograr de muchas otras maneras, incluso mediante el uso de la herencia o mediante patrones de diseño de composición como **el patrón de estrategia**.

### Detectar violaciones de OPC - Principio de abierto y cerrado

- Cambios normalmente afectan nuestra clase o módulo,
- Cuando una clase o módulo afecta muchas capas (presentación, almacenamiento, etc.). Sí la clase o módulo tiene demasiada responsabilidad, seguramente viola el OPC.

### Principio de Substitución de Liskov

> "Las funciones que utilicen punteros o referencias a clases base, deben ser capaces de usar objetos de cases derivadas sin saberlo". - Robert C Martin

Substitución de Liskov: "Siendo U un subtipo de T, cualquier instancia de T debería poder ser sustituida por cualquier instancia de U sin alterar las propiedades del sistema". En otras palabras, cualquier clase A heredera de una clase B, entonces cualquier instancia de B puede ser sustituida ... 

### Principio de segregación de interfaz

> "Los clientes no deberían estar obligados a depender de interfaces que no utilicen". - Robert C. Martin

Las clases no deben de heredar métodos que no necesitan, y así el código no se hace vulnerable a cambios.

Detecar violaciones de este principio:
- Sí las interfaces que diseñamos nos obligan a violar los principios de responsabilidad única y substituticón de Liskov.


### Principio de inversión de dependencias

> "Los módulos de alto nivel no deben de depender de módulos de bajo nivel. Ambos deben depender de abstracciones. Las abstracciones no deben depender de concreciones. Los detalles deben depender de abstracciones". - Rober C. Martin

Explicación:
- Los módulos de alto nivel no deberían depender de módulos de bajo nivel. 
- Ambos deberían depender de abstracciones,
- Las abstracciones no deberían depender de detalles,
- Los detalles deberían de depender de las abstracciones

Los componentes más importanes son aquellos centrados en resolver el problema subyacente al negocio, es decir, la capa de dominio.

Los menos importantes son los que están próximos a la infraestructura, es decir, aquellos relacionados con la UI, la persistencia, la comunicación con API externas, etc. 


Ejemplo:

```js
// File 05-dependency-c
import localPosts from '../data/local-database.json';
import { Post } from './05-dependency-b';

export abstract class PostProvider {
    abstract getPosts(): Promise<Post[]>
}

export class LocalDataBaseService implements PostProvider {
    async getPosts() {
        return [
            {
                'userId': 1,
                'id': 1,
                'title': 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
                'body': 'quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto'
            },
            {
                'userId': 1,
                'id': 2,
                'title': 'qui est esse',
                'body': 'est rerum tempore vitae sequi sint nihil reprehenderit dolor beatae ea dolores neque fugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis qui aperiam non debitis possimus qui neque nisi nulla'
            }]
    }
}

export class JsonDataBaseService implements PostProvider {
    async getPosts() {
        return localPosts;
    }
}

export class WebApiPostService implements PostProvider {
    async getPosts(): Promise<Post[]> {
        const resp = await fetch('https://jsonplaceholder.typicode.com/posts');
        return await resp.json();
    }
}

// File 05-dependency-a
import { PostService } from './05-dependency-b';
import { JsonDataBaseService, LocalDataBaseService, WebApiPostService } from './05-dependency-c';

// Main
(async () => {
    // const provider = new JsonDataBaseService();
    // const provider = new LocalDataBaseService();
    const provider = new WebApiPostService();
    const postService = new PostService( provider );
    const posts = await postService.getPosts();
    console.log({ posts });
})();
```