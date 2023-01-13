## Typescript first steps :footprints:

### Let's start with your day to day using JavaScript...

You are building some kind of counter feature to know how many students have long names, and write something like this:

```javascript
const students = ['Miguel', 'Rodrigo', 'Arturo']
let counter = 0

for (const name of students) {
  if (name.length() > 3) {
    counter++
  }
}
```

If you read the code, everything seems right...doesn't it?
except that, the _length_ property of a string is a number, NOT a function

so if you where in vanilla JavaScript, you would know that until runtime, and that is where Typescript comes into play, to help you catch these bugs earlier in the development process, making the code less prone to errors and more resilient.

### What is TS anyway?

According to Josh Goldberg, Typescript is 4 things:

_A Programing language:_ that includes all the existing JavaScript syntax and the Typescript syntax used for using types.

_Type checker:_ a program that lets you know if all the constructs (variables, functions, etc) are set up correctly.

_Compiler:_ a program that runs the type checker, reports any issues and then outputs the equivalent JavaScript code.

_Language service:_ a program that uses the type checker and tells code editors how to provide helpful utilities to developers.

## Adding TypeScript to a project

To run Typescript you need Node.js installed and install it with npm

```javascript
npm i -g typescript
```

Then, go to the folder where you want to use it and create a new tsconfig.json:

```javascript
tsc --init
```

This file declares the settings that Typescript uses to analyze our code, you can check all the options available here: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html

Now we can compile any file we want with

```javascript
tsc theFileWeWantToCompile.ts
```

---

## The Type System

A Type is what properties and methods exist on a value, for example when we declare the variable

```javascript
const age = 20
```

Typescript knows that the age variable is of type number; The basic types in Typescript are the same as in JavaScript

- null
- undefined
- boolean
- string
- number
- bigint
- symbol

Typescript can also infer types from functions

```javascript
// inferred type: boolean
const isAdult = (age: number) => age >= 18
```

### Understanding type systems

These are a set of rules for how a programming language understands his types and constructs, The Typescript's system works by

- Reading the code with his already existing types and values
- Seeing the initial value and their type
- Seeing the way each value is used later in the code
- Help the developer to see if a value's usage doesn't match with its type

### Errors in Typescript

There are 2 kinds of errors while writing Typescript: Syntax errors and Type errors detected by the type checker.

Syntax error:

```javascript
const const word;
// Error: ',' expected
```

Type error:

```javascript
window.compileFast()
// Error: Property 'compileFast' foes not exist on type 'Window'.
```

### Assignability

Typescript reads initial values to know what type a variable will be, if a variable gets reassigned, it will check if the new value is the same type as the initial value.

```javascript
let isAdult = true
isAdult = 'yes'
// Error: Type 'string' is not assignable to type 'boolean'
```

### Type Annotations

If a variable doesn't have an initial value, Typescript won't try to figure out the initial type. It will default it to be of type _any_ .

```javascript
// inferred type: any
let age
```

Variables that don't have initial value go through an _evolving any_, which means that Typescript will try to infer the variable's type each time it gets reassigned

```javascript
// inferred type: any
let age

// inferred type: number
age = 14
age.length // => OK, no errors

// inferred type: () => void
age = true
age.toUpperCase() // => Error: 'toUpperCase' does not exist on type 'boolean'
```

In most cases, Typescript will infer correctly what type a variable is

```javascript
// This is not necessary, TS already know that it is a string
const beer: string = 'Corona'

// So you can omit that type annotation safety
const beer = 'Corona'
```

### Type Shapes

Typescript not only checks that the values assigned to variables match their original types. it also makes sure that if you attempt to access a property of a variable, it needs to exist on that variable's type

```javascript
const beer = 'Corona'
beer.toUpperCase() // => OK

beer.alert('hello')
// Error: Property 'alert' does not exist on type 'string'

const dog = {
  jump: () => 'I Just jumped!'
  run: () => 'I Just ran!'
  age: 3
}


dog.name
// Error: Property name does not exist on type
// '{ jump: () => string; run: () => string; age: number; }'
```

### Modules

A variable declared in one module with the same name as a variable declared in another file won't be consider a naming conflict unless one file import the other file's variable

```javascript
// file_1.ts
export const beer = 'Corona'

// file_2.ts
export const beer = 'Tecate'

// file_3.ts
import { beer } from 'file_2.ts'
// Error: Import declaration conflicts with local declaration of 'beer'

export const beer = 'Heineken'
// Error: Individual declarations in merged declaration 'beer'
// must be all exported or all local
```

## Unions and literals

In order for Typescript to make good inferences about our code variables, it uses Unions and litera

**Unions** is allowing a type to be two or more possible types, while **Narrowing** is _not_ allowing a type to be one or more possible types.

### Unions

Let's start from this code snippet:

```javascript
const currentYear = 2015

let bestStreamingPlatform = currentYear === 2015 ? 'Netflix' : undefined
```

Here, _bestStreamingPlatform_ can be either string or undefined. This is what is called a union; we don't know a variable exact type, but we do know that it's one of a n number of options, which here are string or undefined.

Unions are represented with the pipe operator: ( **|** ) between the possible values; in this example that would be

```javascript
string | undefined
```

we can declare an union type like this:

```javascript
const currentYear = 2015
let bestStreamingPlatform: string | undefined

if (currentYear === 2016) {
  bestStreamingPlatform = 'Amazon Prime'
  // Since our variable value is not a string
  // we can use string's methods
  bestStreamingPlatform.toUpperCase()
} else {
  bestStreamingPlatform.toLowerCase()
  // Here, our variable value is undefined
  // So a type error will pop up

  // Error: Property 'toLowerCase' does not exist
  // on type 'string | undefined'.
  // Property 'toLowerCase' does not exist on type
  // undefined.
}
```

### Narrowing

Narrowing is when Typescript infers that a value type is a more specific one than what it was when it was defined, declared or initially inferred.

This is commonly used in what is called a _type guard_

```javascript
let imLucky = Math.random() < 0.5 ? true : 'Not so much...'

// Here we use the typeof operator, which
// returns the type of a value in a form
// of a string
if (typeof imLucky === 'string') {
  imLucky.length() // Ok
}
```

### Literal types

Literal types are like the opposite of union types; more specific primitive types. For example:

```javascript
const currentYear = '2015'
```

We could say that currentYear's type is string, and yeah indeed it is a string, however it's not just a string, it's literally the string "2015".

If we declare a variable as a const and initialize it, Typescript infers the variable to be that value as his type.

When using Union types, we can use both Literal and Primitive types:

```javascript
let oneOfMyFavNumber: 1 | '2' | 3 | 'Four' | boolean

oneOfMyFavNumber = 1 // Ok

oneOfMyFavNumber = 300
// Error: Type '300' is not assignable to type
// '1 | "2" | 3 | "Four" | boolean'
```

### Truthiness Narrowing

In JavaScript, a value can be _truthy_ or _falsy_, therefore Typescript can also narrow a variable's type inside a truthiness check:

```javascript
const currentYear = 2015

let bestStreamingPlatform = currentYear === 2015 ? 'Netflix' : undefined

if (bestStreamingPlatform) {
  bestStreamingPlatform.toLowerCase() // Ok
}
```

And the same can be archived using logical operators like _&&_ and _?._

```javascript
const currentYear = 2015

let bestStreamingPlatform = currentYear === 2015 ? 'YouTube' : undefined

bestStreamingPlatform && bestStreamingPlatform.toLowerCase() // Ok

bestStreamingPlatform?.toLowerCase() // Ok
```

An important note is that this truthiness check does not work both ways; for example if a variable can be string | undefined, we can not know for sure if it's a string or undefined

```javascript
const currentYear = 2015

let bestStreamingPlatform = currentYear === 2015 ? 'HBO' : undefined

if (bestStreamingPlatform) {
  // type: string
} else {
  // type: string | undefined
}
```

### Type Aliases

Type aliases are an easy way for re-use a type; it starts with the type keyword, a name (commonly written using PascalCase), =, and then the type itself.

```javascript
type Color = 'red' | 'blue' | 'orange'
```

and then we can use it in our code multiple times:

```javascript
type Color = 'red' | 'blue' | 'orange'

let crayolaColor: Color
let pencilColor: Color
let brushColor: Color
```

Note: Type Aliases can include other primitives like arrays, functions, objects, etc:

```javascript
type Person = {
  name: string
  age: number
  talk: () => "I am talking!"
}

let student: Person
let students: Person[]
```

Now, Type Aliases are used only in the Typescript type system, we can not use them as values, for example:

```javascript
type Person = {
  name: string
  age: number
  talk: () => "I am talking!"
}

const ale = Person
// Error: 'Person' only refers to a type,
// but is being used as a value here.
```

We can also combine multiple types to make more complex ones:

```javascript
type Person = {
  name: string
  age: number
  talk: () => "I am talking!"
}

type Plant = {
  kind: string
  age: number
  size: number
  canWalk: false
}

type Dog = {
  race: string
  name: string
  age: number
}

type LivingBeing = Person | Dog | Plant
```

---

## Documentation

- Josh Goldberg's Learning Typescript Book from O'Reilly (https://www.oreilly.com/library/view/learning-typescript/9781098110321/)

- Typescript documentation (https://www.typescriptlang.org/docs/)
