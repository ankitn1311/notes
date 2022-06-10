# Notes for typescript

## IMPORTANT LINKS
- https://www.youtube.com/watch?v=hBk4nV7q6-w

What is typescript

Typescript is superset for javascript.
Language building up on javascript.
Adds new features with advantages on javascript.
But browser can't execute it.

We have to compile to javascript.

# Installation

`npm install -g typescript`

# Run

tsc filename.ts

# Features

- Types
- Next gen javascript features
- Interfaces and generics (non javascript features)
- Meta programming features like : decorators
- Highly Configurable

# Types

- number
- string
- boolean
- object eg: {age : number}
- Array eg: string[]
- tuple eg: [string, age] (one bug is that we can push to this array even if we want only two elements in this)
- enum enum Role { NEW, OLD} starting from 0
- any

### Type assignment and type inference

- assignment

  ```
  function add(n1: number, n2: number) {
    return n1 + n2;
  }
  ```

- inference
  you don't need to specify type when you are assigning to a const variable because it will infer the type of value
  `const number = 5`
  automatically assign type number

### Union Type

- string | number

### Literal Types

- type : 'edit' | 'save'

### Type Aliases

- using _type_ keyword
  `type Combinable = string | name`

## Function return type and void

```
function add(n1: number, n2: number) :number{           // (automatic type inference here for return type)
  return n1 + n2;
}
```

- void type doesn't exist in javascript
- void exists in typescript

### Functions as types

- If only function then there is a type named : Funtion
  `const someFunction : Function;`
- If we have to specify arguments and return type then:
  `( _anyName_ : number, _anyName_ : number) => number`

## More Types

- Unknown Type
  `const a : unknown` - different from any because we can't assign it to another variable

- Never Type
  `const a : never` - return by function (when a function throws a error / infinte loop)

---

# Typescript Compiler

To watch for file change in a file

`tsc filename.ts --watch or -w`

To watch whole project

`tsc --init`

> creates tsconfig.json file

then `tsc --watch`

## tsconfig.json

### Exculde files

```
  {
    "exclude": [
      "filename.ts",         // "**/*.dev.ts" to exculde any file ending with .dev.ts
      "node_modules"
    ]
  }
```

### Include files

> This will then not include any other file.

```
  {
    "include": [
      "filename.ts",         // "**/*.dev.ts" to exculde any file ending with .dev.ts
    ]
  }
```

## Compiler Options

- target
  - what version of js file the compiler will compile to - es3, es5, es6 ....
- lib
  - ["dom", "dom.iterable", "es6", "scripthost"] // default value
- allowJs
- checkJs
- declartion > .d.ts files
- _sourceMap_
  > very important file, this will create filename.ts.map files that will help us in source map in browsers devtools
  > we can see typescript files as well in source tab
- outDir -> compiled javascript file location (mostly in dist folder)
- removeComments
- noEmitOnError: true

# Typescript & Modern Javascript

- const, let
- arrow functions
- default functions parameters
- spread operator
- rest parameters
- object and array destructuring

# Classes and Interfaces

### Syntax

```
class Department {
  name: string;
  constructor(n: string) {
    this.name = n
  }
  printDept(this: Department) {
    console.log("Department", this.name)
  }
}

const dept = new Department('accounting');
```

### Public Private

> introduced in typescript

```
public name;
private employee: [];
```

### Shorthand property

`constructor(private id: string, public name: string) {}`

### readonly modifier

> introduced in typescript

`constructor(private readonly id: string) {}`

## Inheritance

```
class ITDepartment extends Department {

}
```

> This will inherit everything what department has including constructor

If want to create your own constructor then:

```
constructor(id: string) {
  super(id, 'IT');  // Mandatory
}
```

### Override

```
printDept(this: Department) {
  // somecode
  this.id // can't access if this is private in parent class
}
```

### Protected

Private not accessable on child classes
We have to use protected in this case

### Getters and Setters

```
property: string;

set setProperty (val: string) {
  this.property = val;
}

get getProperty() {
  return this.property;
}
```

Now we can use like this:

```
obj.setProperty = 'Some value';
console.log(obj.getProperty);
```

### Static properties and methods

global variables

`static fiscalYear = 2022`

```
static getEmployeeObject (name: string) {
  return {name: name}
}
```

To access static variables in methods we have to use _ClassName.method/propertyName_ like syntax.

## Abstract Classes

This is when you want to enforce child class to override certain methods

Mark the method you want the child to override with abstract keyword and remove body of the method.
`abstract methodName(param: string): void;`

and also add abstract keyword with class

> We cannot make objects of abstract class

## _Singletons and Private constructors_

**Singleton Pattern** - This means you only have one instance of that class

To do this we have to make constructor of that class private

`private constructor() {}`

By doing this we won't be able to call **new ClassName()** on this
and then we have to add a static method which returns the instance
of that class only if the instance is not created before

```
static private instance: ClassName;

static getInstance() {
  if(ClassName.instance) {
    return this.instance;
  }
  this.instance = new ClassName();
  return this.instance;
}
```

# Interfaces

Only in typescript

```
interface Person {
  name : string;
  age: number;
  greet(name: string, age: number): void
};

let user : Person;
user = {
  name: "ankit",
  age: 30,
  greet(name: string) : {
    console.log("hello from ", name);
  }
}
```

**Difference between interfaces and type**
Interfaces are only can be used to describe the structure of an object
But in type you can also store other things such as union type

Interfaces can be used with classes

```
interface Greetable {
  name: string;
  greet(phrase: string): void;
}

class Person implements Greetable, AnotherInterface {

}
```

### Readonly with interface

Can't be used with type and only interface

interface One extends Two, Three // Two, Three are interfaces

## Use ? or ! to make any memeber of interface optional

`name?: string;`

---

# Advance Types

## Intersection Types (&)

```
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  startDate: Date;
}

type ElevatedEmployee = Admin & Employee;
```

> This concept is same as Interface inheritance

```
type Combinable= string | number;

type Numeric = number | boolean;

type Universal = Combinable & Number;
```

Universal here will be number

## Type Guards

Example: Checking with typeof operator

```
const n1 : number | string;

if(typeof n1 === 'string') {
  // Then do something with string
} else {
  // Do something with number
}
```

> This works for basic types like number string but doesn't work for Custom Types

`type UnknownEmployee = Employee | Admin`

Here we can't check with the typeof operator

There is a **_in_** keyword

`if('propertyName' in variable)`

If types are class based then we can use **instanceof** operator

### Discrimiated Union

```

interface Bird {
  type: 'bird';       // literal type
  flyingSpeed: number;
}
interface Horse {
  type: 'horse';     // literal type
  runningSpeed: number;
}

type Animal= Bird | Horse:

function moveAnimal(animal: Animal) {
  switch(animal.type) {
    case 'bird':        // autocomplete available because of typescript
      // do something
      break;
    case 'horse';
      // do something again
      break;
    default:
      // do something
  }
}
```

## Type Casting

If we get html element in ts

`document.querySelector('p')`

> In this typescript will automatically infer this as HTMLParagraphElement

`document.getElementById('class-name')` // This will return HTMLElment or null

To solve null we can add exclamation mark at the end.

> Here typescript will only infer it as HTMLElement

_To type cast to specific element like input we have to cast_
There are two methods to do this:

- `<HTMLInputElement>document.getElementById('input')!` // ! to prevent null

- `document.getElementById('input')! as HTMLInputElement`

- `(userInput as HTMLInputElement).value = 'Hi There'`

## Index Properties

```
interface ErrorContainer {
  id: string;     // This is allowed
  id: number;     // This is not allowed only string type is allowed if the index property type is also string;
  [error: string] : string;
}

const errors: ErrorContainer = {
  email: 'Not a valid email',
  name: 'Length must be greater than 3',
}
```

## Function Overloads

Function with same name but with different parameters

Before the main function

```

function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: number, b: string): string;
function add(a: string, b: number): string;
function add (a: Combinable, b : Combinable) {
  if(typeof a === 'string' || typeof b === 'string') {
    return a.toString()+ b.toString();
  }
  return a + b;
}
```

## Optional Chaining

```
const fetchedUser = {
  id: '123',
  name: 'Max',
  job: {
    title: 'CEO',
    desc: 'my own company'
  }
}

console.log(fetchedUser.job.title);
```

If there is no job property in the fetched user then there will be run time error

`console.log(fetchedUser?.job?.title)`

If we use _'?'_ job will not be fetched if fetchedUser is undefined

## Null coalescing

```
const someData = null;

const check = someData || 'Checked';
```

This will give back 'Checked' even if _somedata_ is empty string '' or 0

To avoid this we use:

`const check = someData ?? 'Checked'`

# Generics

Available in typescript but not in javascript

Inbuilt Generics Types Example:

Array, Promise

```
const a: Array<string> = [];
a.split('');


const promise: Promise<string> = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("some message")
  }, 2000);
})

promise.then((data) => {
  data.split('');
})
```

## Create a Generic

```
function merge<T, U>(a: T, b: U) {
  return Object.assign(a, b);
}
```

**Working with constraint**

```
function merge<T extends object, U extends object> (a: T, b: U) {
  return Object.assign(a,b);
}
```

## keyof constraint

```
function extractAndConvert<T extends object, U extends keyof T>(obj: T, key: U) {
  return obj[key];
}
```

# Generic Classes

```
class Data <T extends number | string | boolean> {
  ...
}
```

# Some utilities

## Partials

```
interface CourseGoal {
  name: string;
  desc: string;
  completeUntil: Date;
}

function createCourseGoal(name: string, desc: string, year: Date): CourseGoal {
  let courseGoal: Partial<CourseGoal> = {}
  courseGoal.name = name;
  courseGoal.desc = desc;
  courseGoal.completeUntil =  year;
  return courseGoal as CourseGoal;
}
```

## Readonly

```
const names : Readonly<string[]> = ['ankit', 'rahul'];
names.push('akash'); // should be error
```

---

# Decorators

**"experimentalDecorators": true** in _tsconfig.json_ file

Decorator is just a function

```
function Logger(constructor: Function) {      // DECORATOR
  console.log(constructor);
  console.log('logging');
}

@Logger
class Person {
  name = 'Ankit';
  constructor () {
    console.log("creating a person");
  }
}

```

- Number of arguments in a decorator depends on where you use them.
- Decorator runs when you define the class not when you instantiate the class with an object.

## Decorator Factory

Returns a decorator

```
function Logger(logString: string) {
  return function(constructor: Function) {
    console.log('Logging', logString);
  }
}

@Logger('something')       // Now this can accept arguments because of decorator factory
class Person {
  ...
}
```

Advantage of decorator factory is that we can now pass arguments to Decorator which helps in customizing it.

```
function Log(target: any, propertyName: string | Symbol) {
  ...
}

function Log2(target: any, name: string, descriptor: PropertyDescriptor) {
  ...
}

@Logger('Something')
class Person {
  @Log
  title: string;

  @Log2
  set
}
```

## More on Decorator

```
function Logger(logString: string) {
  console.log('LOGGER FACTORY');
  return function(constructor: Function) {
    console.log(logString);
    console.log(constructor);
  };
}

function WithTemplate(template: string, hookId: string) {
  console.log('TEMPLATE FACTORY');
  return function<T extends { new (...args: any[]): { name: string } }>(
    originalConstructor: T
  ) {
    return class extends originalConstructor {
      constructor(..._: any[]) {
        super();
        console.log('Rendering template');
        const hookEl = document.getElementById(hookId);
        if (hookEl) {
          hookEl.innerHTML = template;
          hookEl.querySelector('h1')!.textContent = this.name;
        }
      }
    };
  };
}

// @Logger('LOGGING - PERSON')
@Logger('LOGGING')
@WithTemplate('<h1>My Person Object</h1>', 'app')
class Person {
  name = 'Max';

  constructor() {
    console.log('Creating person object...');
  }
}

const pers = new Person();

console.log(pers);

// ---

function Log(target: any, propertyName: string | Symbol) {
  console.log('Property decorator!');
  console.log(target, propertyName);
}

function Log2(target: any, name: string, descriptor: PropertyDescriptor) {
  console.log('Accessor decorator!');
  console.log(target);
  console.log(name);
  console.log(descriptor);
}

function Log3(
  target: any,
  name: string | Symbol,
  descriptor: PropertyDescriptor
) {
  console.log('Method decorator!');
  console.log(target);
  console.log(name);
  console.log(descriptor);
}

function Log4(target: any, name: string | Symbol, position: number) {
  console.log('Parameter decorator!');
  console.log(target);
  console.log(name);
  console.log(position);
}

class Product {
  @Log
  title: string;
  private _price: number;

  @Log2
  set price(val: number) {
    if (val > 0) {
      this._price = val;
    } else {
      throw new Error('Invalid price - should be positive!');
    }
  }

  constructor(t: string, p: number) {
    this.title = t;
    this._price = p;
  }

  @Log3
  getPriceWithTax(@Log4 tax: number) {
    return this._price * (1 + tax);
  }
}

const p1 = new Product('Book', 19);
const p2 = new Product('Book 2', 29);

function Autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  const adjDescriptor: PropertyDescriptor = {
    configurable: true,
    enumerable: false,
    get() {
      const boundFn = originalMethod.bind(this);
      return boundFn;
    }
  };
  return adjDescriptor;
}

class Printer {
  message = 'This works!';

  @Autobind
  showMessage() {
    console.log(this.message);
  }
}

const p = new Printer();
p.showMessage();

const button = document.querySelector('button')!;
button.addEventListener('click', p.showMessage);

// ---

interface ValidatorConfig {
  [property: string]: {
    [validatableProp: string]: string[]; // ['required', 'positive']
  };
}

const registeredValidators: ValidatorConfig = {};

function Required(target: any, propName: string) {
  registeredValidators[target.constructor.name] = {
    ...registeredValidators[target.constructor.name],
    [propName]: ['required']
  };
}

function PositiveNumber(target: any, propName: string) {
  registeredValidators[target.constructor.name] = {
    ...registeredValidators[target.constructor.name],
    [propName]: ['positive']
  };
}

function validate(obj: any) {
  const objValidatorConfig = registeredValidators[obj.constructor.name];
  if (!objValidatorConfig) {
    return true;
  }
  let isValid = true;
  for (const prop in objValidatorConfig) {
    for (const validator of objValidatorConfig[prop]) {
      switch (validator) {
        case 'required':
          isValid = isValid && !!obj[prop];
          break;
        case 'positive':
          isValid = isValid && obj[prop] > 0;
          break;
      }
    }
  }
  return isValid;
}

class Course {
  @Required
  title: string;
  @PositiveNumber
  price: number;

  constructor(t: string, p: number) {
    this.title = t;
    this.price = p;
  }
}

const courseForm = document.querySelector('form')!;
courseForm.addEventListener('submit', event => {
  event.preventDefault();
  const titleEl = document.getElementById('title') as HTMLInputElement;
  const priceEl = document.getElementById('price') as HTMLInputElement;

  const title = titleEl.value;
  const price = +priceEl.value;

  const createdCourse = new Course(title, price);

  if (!validate(createdCourse)) {
    alert('Invalid input, please try again!');
    return;
  }
  console.log(createdCourse);
});
```
