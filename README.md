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

