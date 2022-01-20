# Intro-to-TypeScript!

![typescriptlogo](https://user-images.githubusercontent.com/78386402/149967909-00f401ba-b16e-4e93-b308-2e59f8d21df7.png)

## What is TypeScript

TypeScript is a programming language that has been developed and maintained by Microsoft. It is considered a superset of JavaScript, which adds optional static typing to the language! 

## What is static typing? 

In programming, a language can be considered dynamically-typed (think JavaScript) or statically-typed (think Java, C++). Dynamic typed languages do not require you to declare a data type for a variable.

Here is how we may work with some basic data types in JavaScript

```js
   let num = 1;
   let name = "Tyler"
   let isAwesome = true;
```

Here is how we would work with those same data types in a static-typed language (c++ in this example)

```cpp
   int num = 1;
   string name = "Tyler"
   bool isAwesome = true;
```

## Why do Types Even Matter?

It is important as Software Engineers to avoid unexpected behavior. One of the toughest things for us to debug is when our code *doesn't* have an error. Here is a simple example of unexpected behavior.

```js
   let num1 = 1;
   let num2 = 5;
   let result = num1 + num2;
   
   console.log(result); // will print 6
```

This code looks great! But what if we accidentally assign `num2` as a string?

```js
   let num1 = 1;
   let num2 = "5";
   let result = num1 + num2;
   
   console.log(result); // what will print now?
```

This can give us some unexpected behavior! But JavaScript will *not* throw an error. Can you image if you have >100 lines of code? Debugging this error could get complex!

If we specified the data type for both num1 and num2, TypeScript would find the error during compilation.

## Basic Data Types in TypeScript

TypeScript includes all the primitive and non primitive data types of JavaScript, plus a couple extra ones, as well as custom data types (more on that later)

### Basic Types in TypeScript
* Boolean
* Number
* String
* Array
* Tuple
* Enum
* Unknown
* Any
* Void
* Null & Undefined
* Never
* Object

## Installing TypeScript

Since TypeScript is *NOT* JavaScript, the browser cannot read TypeScript files. In order for the browser to understand our code, we must compile it to JavaScript. In order to compile TypeScript, we need to install the compiler! We can do this using npm
```zsh
   npm install typescript -g
```

Lets try it out! Open up the `test.ts` file and take a look at the code. Looks familiar huh?

test.ts
```ts
   function printMessage() {
      console.log("Here is your message!")
   }
   
   printMessage();
```

If you try to run this file using node `node test.ts`, you will get an error. We must compile this to JavaScript. In order to do this, type the following in your terminal

```zsh
tsc test.ts
```

You should see a new file appear, called `test.js`.

* We can also tell TypeScript to watch for any changes to our `test.ts` file by adding a `-w` flag
```zsh
tsc test.ts -w
```

## *Optional Settings*

TypeScript can use an optional `tsconfig.json` file to change/set some options for TypeScript. Go ahead and `touch tsconfig.json` at the root this directory. Inside, add the following

```json
{
   "compilerOptions": {
      "rootDir": "./src",
      "outDir": "./dist"
   }
}
```

Now create a `dist` and `src` directory at the root and move the `test.ts` into the `src` directory. Now in your terminal type:
```zsh
tsc -w
```
Now the compiled JavaScript file will be generated in the `dist` directory. This is a way you could seperate the TypeScript from the JavaScript files. The `tsconfig.json` file can take multiple options. Read more about it [here](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

By default typescript will compile code to ES3 since its compatible with every web browser (note the use of var). We can change this by adding a `target` to the `tsconfig.json`

```json
{
    "compilerOptions": {
       "rootDir": "./src",
       "outDir": "./dist",
       "target": "ES6"
    }
 }
```

## Type Annotations

Lets use the number example from above, using TypeScript this time. Inside of `test.ts` lets declare a variable called `num1` and set it equal to 1. If you hover over `num1` you will see something called type inference in action! 

![Screen Shot 2022-01-18 at 12 45 53](https://user-images.githubusercontent.com/78386402/149990691-7301c607-5978-4747-956e-d81c22bf4ee0.png)

What is this weird colon syntax? That is how we declare types in typescript! If you don't explicitly tell TypeScript what type the variable should be, it will try to infer what it should be. We can explicitly tell TypeScript by writing it like this
```ts
   let num1: number = 1;
```

After the variable, we add a colon and then specify the type (note this is to the *left* of the = sign). 

Now lets create another variable called `num2` and accidentally set it to a string.

```ts
   let num1: number = 1;
   let num2: number = "5";
```

Notice what happens? TypeScript is telling us that type 'string' is not assignable to type 'number'! It catches the error before we do! If we don't tell `num2` to be a number, we can still catch the error when we try to add it

```ts
let num1: number = 1;
let num2 = "5";

let result: number = num1 + num2;
```

Here we get the same error as before!


## Arrays in TypeScript

Working with arrays in TypeScript is very straightforward. Here is how you might create an array in TypeScript

```ts

let fruits: string[] = ["Apple", "Orange", "Banana"];

// You could also see it written like this, but the syntax may look a little strange

let fruits: Array<string> = ["Apple", "Orange", "Banana"];

```

But what if I want my array to hold strings and numbers, for example? Here we can make use of a union! A union allows us to specify one type OR another

```ts
let values: (string | number)[] = [1, 2, "3", "4"];
```

You may see someone use the type `any` here if the array contains a lot of different types of data. However, it is strongly recommended that you avoid the use of `any` at all costs. It takes away all of the awesome TypeScript features!

```ts
// Avoid doing this
let values: any[] = [1, 2, "3", "4", true, []];
```


## Functions in TypeScript

Functions work the same as with variables, except now we can specify the types for the parameters, and the return type. Lets look at an example.

```ts
const addNums = (num1: number, num2: number): number => {
    return num1 + num2
}

addNums(1, 2) // Will return 3
addNums(1, "2") // Will throw error
```

### Return Void in Functions

Sometimes we want a function to do something, and then return us a value. In the example above, the function will return a number type. But what if we don't want our function to return anything? This is where we can use the void type!

```ts
const printNumber (num1: number, num2: number): void => {
    console.log(num1 + num2);
}
```

## Objects

Lets start by creating a basic object in our `test.ts` file.

```ts 
const student = {
    name: "Maria",
    age: 20,
    isEnrolled: true
}
```

We can hover our mouse over `student` and see that TypeScript was able to infer the type for us! But lets say we wanted to create a second student, and we wanted to ensure that the data was consistent. 

```ts
const student2 = {
    name: "John",
    age: "21",
    isEnrolled: true
}
```

This time `age` is now of type string. This is where being explicit with types is helpful! Here is how we can add a type annotation to an object

```ts
const student: {
    name: string;
    age: number;
    isEnrolled: boolean;
} = {
    name: "Maria",
    age: 20,
    isEnrolled: true
}
```

Repeating that over and over would get excessive quickly! This is where creating custom data types is helpful!

## Type Interface

TypeScript has two ways for us to declare a reusable type. For this lesson, we will use the Type Interface option. Lets create a Type Interface for our student object.

```ts
interface StudentInterface {
    name: string;
    age: number;
    isEnrolled: boolean;
}
```

Now we can replace our type annotations with StudentInterface!

```ts
const student: StudentInterface = {
    name: "Maria",
    age: 20,
    isEnrolled: true
}

const student2: StudentInterface = {
    name: "John",
    age: "21",
    isEnrolled: true
}
```

And now TypeScript is able to catch our error for the age property of `student2`!

## Classes

TypeScript adds a few new features to classes. Before we dive into those, let see how we can add type annotations to our classes in TypeScript. First lets start by declaring our properties and specifying their type

```ts
class Student {
    firstName: string;
    middleInitial: string;
    lastName: string;
    fullName: string;
    age: number;
}
```

Now we can add our constructor, just like we do in JavaScript

```ts
class Student {
    firstName: string;
    middleInitial: string;
    lastName: string;
    fullName: string;
    age: number;
    constructor(firstName: string, middleInitial: string, lastName: string, age: number) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.age = age;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}
```

Now we can try creating two instances of the `Student` class

```ts
const student1 = new Student("Tyler", "J", "Conti", 27);

const student2 = new Student("John", "J", "Doe", "30");
```

Thanks to typescript, we can catch our error early on!

### Public and Private

TypeScript gives us the ability to make certain properties in classes either public (visible anywhere) or private (only visible to the class). Lets say for example we wanted to add a grade property to our class

```ts
class Student {
    firstName: string;
    middleInitial: string;
    lastName: string;
    fullName: string;
    age: number;
    grade: number;
    constructor(firstName: string, middleInitial: string, lastName: string, age: number, grade: number) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.age = age;
        this.grade = grade;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

const student1 = new Student("Tyler", "J", "Conti", 27, 100);
```

Now if we try to access `student1.grade`, you will see that we can view, edit, or overwrite this value. What if we don't want anyone to change this value? Or we want to have more control over how this value is changed? We can make it a private property!

```ts
class Student {
    firstName: string;
    middleInitial: string;
    lastName: string;
    fullName: string;
    age: number;
    private grade: number;
    constructor(firstName: string, middleInitial: string, lastName: string, age: number, grade: number) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.age = age;
        this.grade = grade;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

const student1 = new Student("Tyler", "J", "Conti", 27, 100);
```

Now you will see that we can no longer access `student1.grade`. The only way to change this now would be through the use of getter and setter methods!


```ts
class Student {
    firstName: string;
    middleInitial: string;
    lastName: string;
    fullName: string;
    age: number;
    private grade: number;
    constructor(firstName: string, middleInitial: string, lastName: string, age: number, grade: number) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.age = age;
        this.grade = grade;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
    changeGrade(grade: number): void {
        this.grade += grade;
    }
    getGrade(): number {
        return this.grade;
    }
}
```

## Generics

Generics is a more complicated but important part of TypeScript. Generics allows us to create functions (or components in something like react) that can work with a variety of types rather than just one. Lets look at an example!

```ts
const insertAtBeginning = (array, value) => {
   const newArray = [value, ...array];
   return newArray;
}
```

Now if he hover our mouse over the function, you will see that the array can be `any` type, the value can be `any` type, and the function can return `any` type. 

![Screen Shot 2022-01-19 at 15 24 02](https://user-images.githubusercontent.com/78386402/150208101-5d66ca47-ea89-4adc-9a07-0715b83b0ffe.png)


Remember how we said we want to avoid the use of `any` at all costs? What do you think will happen if we keep our function like this?

```ts
const insertAtBeginning = (array, value) => {
   const newArray = [value, ...array];
   return newArray;
}

// What type do you think updatedArray will be?
const updatedArray = insertAtBeginning([1,2,3], 4);

// Will we get an error if we do this?
const updatedArray2 = insertAtBeginning([1,2,3], "4");
```

This is where generics comes in! Generics allows us to use a *placeholder* type. Lets see how this looks!

```ts
const insertAtBeginning = <T>(array: T[], value: T): T[] => {
   const newArray = [value, ...array];
   return newArray;
}
```

Woooah what are those weird `<>` and the `T`? This is how we use generics! You can think of the T as a *placeholder* for the type. Now we can replace the type with `T`, and the function will know which type we are talking about!
