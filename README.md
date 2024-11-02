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

        // DRY
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

