---
title: TypeScript
description: Slides of my talk at VueConf China 2021
date: 2022-06-28T08:00:00.000+00:00
lang: en
type: note
recording: false
duration: 120min
---

## Preparations

[JavaScript](https://www.notion.so/JavaScript-477d09c67e5343b0b65380e757a779fe) 

## Intro

Features

- Static typing
- Code completion
- Refactoring
- Shorthand notations

> statically-typed  (C++, C#, Java);   Dynamically-typed (JavaScript,  Python, Ruby)
> 

## Dev environment

[Install Node environment first.](https://nodejs.org/en/)

Then you can install TypeScript through NPM.

`npm i -g typescript` 🪟 

Check the version

`tsc -V`  🪟

## First TS program

Compile TS files to JS files

`tsc index.ts`

## Configuring Complier

Initialize a TS project

`tsc --init`

Then you can uncomment and modify some useful configs in `tsconfig.json`

```json
"target": "es2016",   // depends on your target browser 
"rootDir": "./",      // TS files
"outDir": "./dist",   // target JS files
"removeComments": false,  // remove all comments when transforming to JS files
"noEmitOnError": true, // if a TS file have errors, omit that one
```

After set up these settings, you can compiler the TS files through:

`tsc`

## Debugging

Suppose you are using VSCode. In the side panel, click to `create a launch.json file`, choose Node.js and it will help you create and open a `launch.json` file. 

Add this line:

```json
"preLaunchTask": "tsc: build - tsconfig.json",
```

Then you can add some breakpoints and  start the debugging by clicking “Launch Program”:



## Basic Types

JavaScript (ES5)

- number
- string
- boolean
- null
- undefined
- object

TypeScript Supplement

- any
- unknow
- enum
- tuple

<aside>
💡 Avoid using `any` type as much as possible.

</aside>

Annotation with initiation phase is unnecessary:

```tsx
const sales = 1_234_567
const course = 'TypeScript'
const is_published = true
```

equals

```tsx
const sales = 1_234_567
const course = 'TypeScript'
const is_published = true
```

If you are clear about what you are going to do, use `any` annotation to declare a unassigned object:

```tsx
let a // a is equivalent to 'any` type
a = 123
a = '456'
function getResult(res: any) {
  console.log('res', res)
}
```

> Related config param in tsconfig.json : `NoImplicitAny`  `noUnusedLocals`
> 

### Arrays

Implicitly declare an array:

```tsx
const anyArray = [1, 'abc', true]
// equal to => let anyArray: any[] = [1, 'abc', true]
```

Explicitly declare an array: 

```tsx
const numArray: number[] = [1, 3, 5]
const strArray: string[] = ['Java', 'Go', 'Python']
```

### Tuples

Tuple is a fixed-length array where each element has a particular type.

> Tuple usually has two elements.
> 

```tsx
const user: [number, string] = [21, 'Mosh']
```

### Enums

Define an Enums type variable and you can assign value to other variables from a list of constants.

You can explicitly or implicitly define the: 

```tsx
enum Direction {
  Up,
  Down,
  Left,
  Right,
} //  Up = 0; Down = 1; Left = 2; Right = 3
enum UserResponse {
  No = 0,
  Yes = 1,
}
const ur: UserResponse = UserResponse.No
const mr: Direction = Direction.Down
console.log('ur', ur) // -> 0
console.log('mr', Mr) // -> 1
```

> Related config param in tsconfig.json : `NoImplicitAny`
> 

### Functions

Always properly annotate all your parameters and return types in your functions.

```tsx
function income(income: number): string {
  return `My income is ${income}`
}
function add(x: number, y: number): number {
  return x + y
}
```

You can add a optional parameter and give default value to a parameter( in this case you don’t need to annotate its type)

```tsx
function tax(income: number, taxYear?: number, formal = false): number {
  if ((taxYear || 2022) < 2022) {
    if (formal)
      return income

    else
      return income * 1.2 // this way

  }
  return income * 1.3
}

console.log(tax(1230, 2020)) // -> 1476
```

> Related config param in tsconfig.json : `noUnusedParameters`  `noImplicitReturns`
> 

### Objects

CODE

```tsx
const employ: {
  readonly id: number // readonly property cannot be re-assigned
  name: string // you must specify every property type
  retire: (date: Date) => void // function property
} = {
  id: 1,
  name: 'Tony Stark',
  retire: (date: Date) => {
    console.log(date)
  },
}
console.log('employ.id', employ.id)
console.log('employ.id', employ.name)
employ.retire(new Date())
console.log('employ', employ)
```

OUTPUT

```tsx
employ.id 1
employ.id Tony Stark
2022-07-04T10:23:37.018Z
employ { id: 1, name: 'Tony Stark', retire: [Function: retire] }
```

## Advanced  types

### Type Alias

Declare a type starts with keyword “type{}”, specify each property:

```tsx
interface Employ {
  readonly id: number
  name: string
  retire: (date: Date) => void
}

```

Then you can use this type to annoate a new variable;

```tsx
const emp1: Employ = {
  id: 2,
  name: 'Captain America ',
  retire: (date: Date) => {
    return console.log('date', date)
  },
}
```

### Union Type

Allow you can assign different types of a value to a new variable

```tsx
// for parameter weight, both number and string type is acceptable
function KgToLbs(weight: number | string): void {
  if (typeof weight == 'number')
    console.log(`weight:${weight}KG`)

  else
    console.log(`weight:${weight}`)

}
```

### Intersection Types

We can define two types

```tsx
interface Draggable {
  drag: () => void
}

interface Resizable {
  resize: () => void
}
type UIWidge = Draggable & Resizable
```

By using intersection type, you can make sure that the variable using this type must implement all the types needed.

```tsx
const textBox: UIWidge = {
  drag: () => {},
  resize: () => {},
}
```

### Literal Types

By explicitly limiting the literal values of a type. 

You can not assign other words than these options.

```tsx
const quantity: 8 | 18 | 50 = 50

// use Type Alias
type Quantity = 50 | 100 | 200
const q2: Quantity = 200

type Unit = 'C' | 'F'
const myUnit: Unit = 'C'
```

### Nullable values

```tsx
function greet(name: string | null) {
  if (name)
    console.log(name.toUpperCase())

  else
    console.log('Hola')

}

greet('Hello!') // -> HELLO
greet(null) // -> Hola
```

> To use `null` in TS ⇒  alter tsconfig.json ⇒  `strictNullChecks`
> 

### Optional Chaining

> use this to avoid errors
> 

Define a Customer type and try to access his/her birthday: 

```tsx
interface Customer {
  birthday: Date
}
function getCustomer(id: number): Customer | null | undefined {
  return id === 0 ? null : { birthday: new Date() }
}
const customer = getCustomer(0)
const customer2 = getCustomer(1)
```

Optional property access operator:

```jsx
console.log(customer?.birthday.getFullYear()); // -> undefined
console.log(customer2?.birthday?.getFullYear()); // -> 2022
```

Optional element access operator: 

```tsx
const langs = ['Java', 'C++', 'GO']
console.log(langs?.[0]) // Java
console.log(langs?.[3]) // undefined
```

Optional call ( a function)

```tsx
const log: any = (message: string) => {
  console.log('Hello ', message)
}
const log2: any = null
log?.('My Love') // -> Hello My Love
log2?.('My Love') //
```

## Record Type

[https://fjolt.com/article/typescript-record-type](https://fjolt.com/article/typescript-record-type)

## Type Assertion

Sometimes you will have information about the type of a value that TypeScript can’t know about.

For example, if you’re using `document.getElementById`, TypeScript only knows that this will return *some* kind of `HTMLElement`, but you might know that your page will always have an `HTMLCanvasElement` with a given ID.

```tsx
const myCanvas = document.getElementById('main_canvas') as HTMLCanvasElement
```

Like a type annotation, type assertions are removed by the compiler and won’t affect the runtime behavior of your code.

You can also use the angle-bracket syntax (except if the code is in a `.tsx` file), which is equivalent:

```tsx
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

## Class

```tsx
class Person {
  public id: number // public keyword equals to default
  name: string

  constructor(id: number, name: string) {
    this.id = id
    this.name = name
  }

  register() {
    return `${this.name} is now registered.`
  }
}

const brad = new Person(1, 'Brad New')
console.log(brad, mike) // -> Person {id:1, name: "Brad New" }    {id: 2, name: "Mike Jordan"}
console.log('brad.id', brad.id) // -> brad.id 1
```

When you set a property as `protected` , you can’t access it outside anymore.

```tsx
(property) [Person.id](http://person.id/): number
Property 'id' is protected and only accessible within class 'Person' and its subclasses
```

And to make the code more robust, you can define a `interface` and implements it with previous `class` .

```tsx
interface PersonInterface {
  id: number
  name: string
  register(): string
}

class Person implements PersonInterface {
  // ....
}
```

- Inheritence

```tsx
class Employee extends Person {
  position: string

  constructor(id: number, name: string, position: string) {
    super(id, name) // construct the ancestor object
    this.position = position
  }
}
```

## Generics

```tsx
function getArray(items: any[]): any[] {
  return [].concat(items)
}

const numArr: number[] = [1, 2, 3]
const strArr: string[] = ['Java', 'C', 'C++']
console.log(getArray(numArr)) // -> [1, 2, 3]
```

equals to 

```tsx
function getArray<T>(items: T[]): T[] {
  return [].concat(items)
}

const numArr = getArray<number>([1, 2, 3, 4])
const strArr = getArray<string>(['Java', 'C', 'C++'])
```

## [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

TypeScript provides several utility types to facilitate common type transformations. These utilities are available globally.****

### Pick<T, keys>

`Pick<T, Keys>`Is a utility type that returns an object type that contains only the specified key `T`from `Keys`the type.

```tsx
interface User {
  surname: string
  middleName?: string
  givenName: string
  age: number
  address?: string
  nationality: string
  createdAt: string
  updatedAt: string
}
type Person = Pick<User, 'surname' | 'middleName' | 'givenName'>
```

The above `Person`is the same as the following type.

```tsx
interface Person {
  surname: string
  middleName?: string
  givenName: string
}
```