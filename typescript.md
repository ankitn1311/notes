# Notes for typescript

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
