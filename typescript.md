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

## Type assignment and type inference

- assignment
  `function add(n1: number, n2: number) { return n1 + n2; }`

- inference
  you don't need to specify type when you are assigning to a const variable because it will infer the type of value
  `const number = 5`
  automatically assign type number

## Union Type

- string | number

## Literal Types

- type : 'edit' | 'save'

## Type Aliases

- using _type_ keyword
  `type Combinable = string | name`

# Function return type and void

`function add(n1: number, n2: number) :number(automatic inference){ return n1 + n2; }`

- void type doesn't exist in javascript
- void exists in typescript

## Functions as types

- If only function then there is a type named : Funtion
  `const someFunction : Function;`
- If we have to specify arguments and return type then:
  `( _anyName_ : number, _anyName_ : number) => number`

# More Types

- Unknown Type
  `const a : unknown` - different from any because we can't assign it to another variable

- Never Type
  `const a : never` - return by function (when a function throws a error / infinte loop)
