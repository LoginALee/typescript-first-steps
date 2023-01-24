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

## Objects

When we create an object in Javascript, Typescript will infer his types, for example:

```javascript
const Book = {
  title: 'The little prince'
  pages: 92
}
// Typescript knows that the typeof Book is
{ title: string, pages: number }

// So, if we try to access a property that does not
// exists in Book, TS will complain:

Book.finished
// Error: Property 'finished' does not
// exist on type ' { title: string, pages: number } '
```

### Object Types

Like we see before; it's common to use Type aliases to assign them to our objects

```javascript
type Car = {
  name: string
  model: number
  color: string
}

const Toyota: Car
const Mazda: Car
```

When we tell Typescript what type an object is, it will check that it has all the required properties that his type has. If any required property is missing, Typescript will throw an Error.

```javascript
type Car = {
  name: string
  model: number
  color: string
}

const Toyota: Car = {
  name: 'Corolla',
  color: 'Red'
}
// Property 'model' is missing in type '{ name: string, color: string } '
// but required in type 'Car'
```

Another important note is that their properties types must match

```javascript
type Car = {
  model: number,
}

const Toyota: Car = {
  model: '2020',
}
// Error: Type 'string' is not assignable to type 'number'
```

#### Excess Property Checking

What if our object has all the properties of the type we are telling Typescript out object is, but also has more that are not specified in that type? Typescript will not be happy

```javascript
type Car = {
  model: number,
}

const Toyota: Car = {
  model: '2020',
  maxPassengers: 4,
}
//Error type '{ model: string, maxPassengers: number }' is not assignable to type 'Car'.
// Object literal may only specify known properties.
// and 'maxPassengers' does not exist on type 'Car'
```

Excess of properties are a code smell, and they often mean mistyped property names or unused code.

### Nested Object types

We can declare Objects inside object types and make them as complex as we need:

```javascript
type School = {
  name: string
  location: {
    street: string
    externalNumber: number
    neighborhood: string
  }
}
```

It's good practice to define our nested object types into their own type to improve readability and clean Typescript error messages:

```javascript
type Location = {
  street: string
  externalNumber: number
  neighborhood: string
}

type School = {
  name: string
  location: Location
}
```

### Optional properties

When defining object types we can say that a property can be present or not:

```javascript
type Laptop = {
  brand: string
  model: string
  hasStickers?: boolean
}

const myLaptop: Laptop = {
  brand: 'MSI',
  model: '3FN2',
  hasStickers: true
}

const workLaptop: Laptop = {
  brand: 'Dell',
  model: '29NDU29'
}
```

Note: optional properties are not the same as undefined values; optional properties can exist or not, but undefined values need to be there, otherwise TS will complain

```javascript
type Laptop = {
  brand: string
  model: string
  hasStickers: boolean | undefined
}

const myLaptop: Laptop = {
  brand: 'MSI',
  model: '3FN2',
  hasStickers: true
}

const workLaptop: Laptop = {
  brand: 'Dell',
  model: '29NDU29'
  hasStickers: undefined
}

const gamingLaptop: Laptop = {
  brand: 'Razer',
  model: '29NIA920H'
}
// Error: property 'hasStickers' is missing in
// type { brand: string, model: string }' but
// required in type 'Laptop'
```

### Discriminated Unions

There is a popular pattern inside Object types called discriminated unions, which is to have a property on the object to indicate what shape the object is.

This helps TS to perform type narrowing in type guards, for example:

```javascript
type racingCar = {
  model: string
  hasTurbo: boolean
  type: 'RACING'
}

type familyCar = {
  model: string
  hasTVScreen: boolean
  type: 'FAMILY'
}

type Car = racingCar | familyCar

const firstCar: Car

if(firstCar.type === 'FAMILY'){
  firstCar.hasTVScreen // OK
} else{
  firstCar.hasTurbo // OK
}
```

### Intersection Types

Typescript allows to represent a type that is multiple types all in one; using the intersection type symbol: _&_

```javascript
type Electronic = {
  needsElectricity: true
}

type Portable = {
  hasBattery: true
}

type CellPhone: Electronic & Portable
```

This is the same as:

```javascript
type CellPhone = {
  hasBattery: true
  needsElectricity: true
}
```

## Functions

### Parameters

Inside our JavaScript functions we can tell Typescript which type are our parameters, for example:

```javascript
const isEven = (number: number) => number % 2 === 0
```

here we expect only one parameter of type number, if our function receives more or less than one parameter and a different type than number, Typescript will throw an error:

```javascript
isEven(29) // Ok

isEven(10, 'extra param')
// Error: Expected 1 argument, but got 2
```

But what if we want an optional parameter? we can do that, we just need to add it the _?_ symbol and put it at the end of our parameters

```javascript
// Here we gave a default value for our optional parameter
const isEven = (number: number, outputResult?: boolean = false) => {
  const result = number % 2 === 0

  if (outputResult) {
    console.log(result)
  }

  return result
}

isEven(29) // Ok

isEven(2, true) // Ok

isEven(10, true, true)
// Error: Expected 2 arguments, but got 3
```

We can also use rest parameters in our functions using the spread operator with this syntax:

```javascript
const areEven = (...numbers: number[]) =>
  numbers.every((number) => number % 2 === 0)
```

### Return types

Typescript is smart enough to infer which type our areEven function type, which is boolean, now we can make this more clear adding an implicit return type (which is not mandatory):

```javascript
const areEven = (...numbers: number[]): boolean =>
  numbers.every((number) => number % 2 === 0)
```

### Function types

We can declare a Function type for our functions like this:

```javascript
type AreEven = (...numbers: number[]) => boolean
// Here we are telling his parameters and return types

const areEven: AreEven = (...numbers) =>
  numbers.every((number) => number % 2 === 0)

areEven(2, 2, 2) // Ok
```

#### Void type

Sometimes our functions not return anything, and Typescript uses the _void_ keyword for those cases:

```javascript
type Log: (text: string) => void

const log: Log = (text) => console.log(text)

Log('Heeey!') // Ok
```

Note: Every JavaScript function returns undefined if we don't specify otherwise, so void means that we will ignore the function return value; void is not a value that can be returned.

#### Never type

Functions also can not return nothing at all, and that is where the never type comes in. This is not the same as void though; with never, our function does not return, and void returns nothing.

```javascript
const errorHandler = (errorMessage) => throw new Error(errorMessage)
// Here our function type is never
```

## Arrays

When we declare an array, Typescript will infer his type and won't allow us to push other type of items to it

```javascript
const musicCategories = ['Pop', 'Classic', 'Rock']

musicCategories.push('Jazz') // Ok

musicCategories.push(19)
// Argument of type 'number' is not assignable to parameter of type 'string'
```

if we don't want to give an initial value to our array it's cool, we just need to tell TS which types to accept:

```javascript
let musicCategories: string[]

musicCategories.push('Jazz') // Ok

musicCategories.push(19)
// Argument of type 'number' is not assignable to parameter of type 'string'
```

We can have arrays with multiple types inside

```javascript
// Here, we have an array which holds functions that returns numbers
const arrayOfFunctions: (() => number)[]

// Here, we have an array which can hold multiple types
const arrayOfMultipleTypes: (string | number | boolean)[]
```

If we do not tell Typescript which types will store our array, it will assume that it can hold any type:

```javascript
let foods = []
// Type: any[]

foods.push('pineapple') // Ok
foods.push(13) // Ok
```

However, this defeats the purpose of using Typescript; which is have our variables, functions, etc. well defined.

### Multidimensional Arrays

When typing a multidimensional array, the same concepts apply, and it depends in how many dimensions does our array has:

```javascript
let arrayOfArrays: number[][]
// Here, we have a 2 dimensions array

let arrayOfArrayOfArrays: number[][][]
// Here, we have a 3 dimensions array

// And so on and so forth...
```

### Array members

When we extract a member from an array and assign it to a variable, TS will use the Array type to know what type our variable will be:

```javascript
const foods: string[] = ['apple', 'watermelon', 'pineapple']

const myFood = foods[2]
// myFood will be of type 'string'
```

But, what happens if we access an index which is not defined? Typescript by default won't yell at us:

```javascript
const foods: string[] = ['apple']

const myFood = foods[300]
// TS won't report any error
```

### Using the spread operator

When using the spread operator with arrays, TS will infer the types from the spreaded arrays:

```javascript
const foods: string[] = ['apple']
const grades: number[] = [10, 10, 9, 5, 8]

const myJoinArray = [...foods, ...grades]
// myJoinArray type will be a join from foods and grades types:
// (string | number)[]
```

### Tuples

Tuples are fixed size arrays, which have a specific type for each index:

```javascript
let ownerAndPet: [string, string] = ['Ale', 'Mr. Moustache']

ownerAndPet = [10, 'Picles']
// Error: Type 'number' is not assignable to type 'string'

ownerAndPet = ['Rodolfo']
// Error: Type '[string]' is not assignable to type '[string, string]'
```

We can be more strict with our tuples if we want them to be more explicit and read-only, using the _as const_ keywords:

```javascript
const ownerAndPet: [string, string] = ['Ale', 'Mr. Moustache'] as const

ownerAndPet[0] = 'George'
//Error: cannot assign to '0'
// because it is a read-only property
```

---

## Documentation

- Josh Goldberg's Learning Typescript Book from O'Reilly (https://www.oreilly.com/library/view/learning-typescript/9781098110321/)

- Typescript documentation (https://www.typescriptlang.org/docs/)
