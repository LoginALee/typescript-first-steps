## Typescript first steps :footprints:

### Let's start with your day to day using JavaScript...

You are building some kind of counter feature to know how many students have long names, and write something like this:

```javascript
const students = ['Miguel', 'Rodrigo', 'Arturo'];
let counter = 0;

for (const name of students) {
  if (name.length() > 3) {
    counter++;
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

## Getting started

To run Typescript you need Node.js installed and install it with npm

```javascript
npm i -g typescript
```

Then, go to the folder where you want to use it and create a new tsconfig.json:

```javascript
tsc --init
```

This file declares the settings that Typescript uses to analyze our code, now we can compile any file we want with

```javascript
tsc theFileWeWantToCompile.ts
```

## WIP...

## Documentation

- Josh Goldberg's Learning Typescript Book from O'Reilly (https://www.oreilly.com/library/view/learning-typescript/9781098110321/)
