---
layout: post
title: TypeSctipt
date: 2023-03-14
categories: [Backend, TypeScript]
---
![](/assets/images/typescript.jpg)

## Implicit Typing and Explicit Typing

**Implicit Typing:**

TypeScript can infer the type of a variable based on its value, which is called implicit typing. For example, if a variable is initialized with a string value, TypeScript will infer that the variable has a string type. Implicit typing can be useful for writing code quickly and concisely, but it can also lead to errors if TypeScript infers the wrong type or if the type of the variable changes later in the code.

```ts
let message = 'Hello, TypeScript!';
```

**Explicit Typing:**

Explicit typing involves explicitly specifying the type of a variable using a type annotation. For example, you can specify that a variable has a string type like this:

```ts
let message: string = 'Hello, TypeScript!';
```

Explicit typing can be useful for ensuring that your code is type-safe, providing better documentation for your code, catching errors at compile-time, and making your code more maintainable and scalable.

## union Types

Union types in TypeScript allow a variable to have multiple possible types. `This is useful when a variable can have different types of values at different times.` 

For example, a variable that can hold `either a string or a number` value can be defined using a union type like this:

```ts
let variable: string | number;
```

This tells TypeScript that the `variable` can hold either a string or a number, but not any other type.

Union types can also be used with `function parameters` and `return types`. For example, a function that can return either a string or a number can be defined like this:

```ts
function getStringOrNumber(value: boolean): string | number {
  if (value) {
    return 'Hello';
  } else {
    return 42;
  }
}
```

In this example, the `getStringOrNumber` function takes a `boolean parameter and returns either a string or a number`, depending on the value of the parameter.

In TypeScript, `null` and `undefined` can be included in union types. This is useful when a variable can have a value of `null` or `undefined` in addition to other types.

For example, a variable that can hold either a string, a number, or `null` can be defined using a union type like this:

```ts
let variable: string | number | null;
```

This tells TypeScript that the `variable` can hold either a string, a number, or `null`.

Similarly, a variable that can hold either a string, a number, or `undefined` can be defined using a union type like this:

```ts
let variable: string | number | undefined;
```


Union types with `null` and `undefined` can also be used with function parameters and return types. For example, a function that can return either a string, a number, or `null` can be defined like this:

```ts
function getStringOrNumberOrNull(value: boolean): string | number | null {
  if (value) {
    return 'Hello';
  } else {
    return null;
  }
}
```

In this example, the `getStringOrNumberOrNull` function takes a boolean parameter and returns either a string, a number, or `null`, depending on the value of the parameter.


`void` is a type that represents the absence of a value. It's commonly used as a return type for functions that don't return anything, but instead perform some kind of side effect like logging to the console or updating the DOM. For example:

```ts
function greet(name: string): void {
  console.log(`Hello, ${name}!`);
}
```

In this example, the `greet` function has a `void` return type because it doesn't return a value that can be used elsewhere in the code. Instead, it logs a message to the console.

`never` is a type that represents a value that will never be returned. It's used as a return type for functions that don't have a normal return, such as functions that throw an error or go into an infinite loop. For example:

```ts
function throwError(message: string): never {
  throw new Error(message);
}
```

In this example, the `throwError` function has a `never` return type because it throws an error and doesn't return a value. The code execution will never reach the end of the function.

`any` is a type that represents any possible value. It's generally not recommended to use `any` because it weakens TypeScript's type checking, but it can be useful in situations where the type of a value is unknown or can't be determined. For example:

```ts
function doSomething(value: any): void {
  // do something with the value
}
```

In this example, the `doSomething` function has a parameter of type `any`, which means that it can accept any type of value. This can be useful in situations where the type of the value is not known ahead of time.

`unknown` is a type that represents a value whose type is not yet known. It's useful for situations where you need to work with a value that could be of any type, but you want to ensure that the type is checked before it's used in the code. For example:

```ts
function doSomething(value: unknown): void {
  if (typeof value === 'string') {
    console.log(`The value is a string: ${value}`);
  } else if (typeof value === 'number') {
    console.log(`The value is a number: ${value}`);
  } else {
    console.log(`The value is something else: ${value}`);
  }
}
```

In this example, the `doSomething` function has a parameter of type `unknown`, which means that the type of the value is not yet known. The function checks the type of the value using a conditional statement and performs different actions depending on the type. This ensures that the type of the value is checked before it's used in the code.

## Type assertions

Type assertions in TypeScript are `a way of telling the compiler that you know more about the type of a value than it does`. They are useful when you have a value of a certain type, but TypeScript can't infer the type from the code.

There are two ways to do type assertions in TypeScript: using the `as` keyword or using angle brackets (`<>`). Here's an example of using the `as` keyword:

```ts
let value: any = 'Hello, TypeScript!';
let length: number = (value as string).length;
```

In this example, we have a variable `value` that has an `any` type. We know that `value` is actually a string, so we use the `as` keyword to tell TypeScript to treat it as a string. We then assign the length of the string to a variable `length`.

Here's an example of using angle brackets:

```ts
let value: any = 'Hello, TypeScript!';
let length: number = (<string>value).length;
```

This is similar to the previous example, but we use angle brackets instead of the `as` keyword. The result is the same - TypeScript treats `value` as a string and assigns the length of the string to `length`.

It's important to note that type assertions can be risky if used improperly. If you assert a type that is not



## Enum

an enum is a way to define a set of named constants. Enums allow you to define a set of related values that can be used throughout your code, making it easier to write and maintain your code.

Here's an example of how to define and use an enum in TypeScript:

```ts
enum Color {
  Red,
  Green,
  Blue
}

let myColor: Color = Color.Red;
console.log(myColor); // Output: 0
```

In this example, we define an enum called `Color` with three values: `Red`, `Green`, and `Blue`. Each value is assigned a numeric value by default, starting with 0.

We then declare a variable `myColor` of type `Color` and assign it the value `Color.Red`. When we log `myColor` to the console, we see that it outputs `0`, which is the numeric value assigned to `Color.Red`.

Enums can also have custom numeric values assigned to their values, like this:

```ts
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4
}

let myColor: Color = Color.Red;
console.log(myColor); // Output: 1
```

In this example, we assign the values 1, 2, and 4 to `Red`, `Green`, and `Blue`, respectively. When we assign `Color.Red` to `myColor` and log it to the console, we see that it outputs `1`, which is the custom numeric value assigned to `Color.Red`.


## Array
To create an array in TypeScript using the `[]` syntax:
```ts
let arr: string[]; // only accepts strings
let arr2: (string | number)[]; // accepts strings or numbers
```
## Tuple
A tupple is an array with a fixed number of elements whose types are known.

You can create a tuple in TypeScript using the `[]` syntax and specifying the types of the elements inside the square brackets, like this:

```ts
let myTuple: [string, number, boolean] = ["hello", 123, true];
```

In this example, we're creating a tuple with three elements: a string, a number, and a boolean. We're using the `[string, number, boolean]` syntax to tell TypeScript the types of the elements in the tuple.

## Objects 

An object is a collection of key-value pairs, where the keys are strings and the values can be any type. You can create an object in TypeScript using curly braces `{}` and specifying the key-value pairs inside:

```ts
let person = {
  name: "Abdo",
  age: 21,
  isStudent: true
};
```

In this example, we're creating an object called `person` with three key-value pairs: `name` is a string, `age` is a number, and `isStudent` is a boolean.

You can access individual properties of an object using dot notation:

```
console.log(person.name); // Output: "Abdo"
console.log(person.age); // Output: 21
console.log(person.isStudent); // Output: true
```

You can also modify individual properties of an object by assigning a new value to their key:

```ts
person.age = 22;
console.log(person.age); // Output: 22
```
## Interfaces

An `interface` is a way to define the `shape and structure of an object in TypeScript`. You can use an interface to specify the names and types of the properties that an object should have. Here's an example:

```ts
interface Person {
  readonly name: string;
  age?: number;
  isStudent: boolean;
}
```

In this example, we're defining an interface called `Person` with three properties: `name` is a string and can not be change as it a `readonly`, `age?` is a number, and `?` means that is opetional `isStudent` is a boolean.

You can use an interface to define the structure of an object, and then create objects that match that structure:

```ts
let me: Person = {
  name: "Abdo",
  age: 21,
  isStudent: true
};
```
## Alias Types

To create an alias type, you can use the `type` keyword followed by the new name you want to give the type and the existing type you want to alias. Here's an example:

```ts
type Person = {
  name: string;
  age: Age;
};
```

In this example, we're creating an alias type called `Person` that combines a `name` property of type `string` and an `age` property of type `Age` (which we defined earlier as an alias for `number`).

Once you've defined the `Person` type alias, you can use it to create objects that conform to that type. Here's an example:

```ts
const me: Person = {
  name: 'Abdo',
  age: 21,
};
```

## the differnce between interfaces and types:

both interfaces and types are used to define custom types, but they have some differences in their syntax and usage.

An interface is a way to define a contract for an object. It specifies the names and types of the properties that an object should have. Interfaces can also be used to define function signatures, which specify the types of the parameters and the return value of a function.

Here's an example of an interface for a user object:

```typescript
interface User {
  name: string;
  age: number;
  email: string;
}
```

In this interface, we're specifying that a user object should have a `name` property that's a string, an `age` property that's a number, and an `email` property that's a string.

A type, on the other hand, is a way to define a custom type that can be used in place of existing types. Types can be used to define union types, intersection types, and other complex types that can't be defined with interfaces.

Here's an example of a type for a user object:

```typescript
type User = {
  name: string;
  age: number;
  email: string;
}
```

In this type, we're defining a user object in the same way as the interface. The syntax is slightly different, but the end result is the same.

So, the main difference between interfaces and types is that `interfaces are used to define contracts for objects and function signatures`, while `types are used to define custom types that can be used in place of existing types`. In general, it's a matter of personal preference which one you use, but `interfaces` are more commonly used for `object` contracts, while `types` are more commonly used for `complex types`.



## Classes 

classes are used to define object blueprints that encapsulate data and behavior. They can have properties, methods, and constructors, and can also inherit from other classes. 

`Access modifiers` are also available in TypeScript classes. The `public` modifier is the default and denotes that a property can be accessed outside of the class. The `private` modifier restricts access to the property to only within the class itself. The `protected` modifier `allows access to the property within the class and its child classes`.

Here's an example of a TypeScript class with access modifiers:

```ts
class Student {
  protected studentGrade: number;
  private studentId: number;

  constructor(grade: number, id: number) {
    this.studentGrade = grade;
    this.studentId = id;
  }

  id() {
    return this.studentId;
  }
}

class Graduate extends Student {
  studentMajor: string;

  constructor(grade: number, id: number, major: string) {
    super(grade, id);
    // this.studentId = id; // TypeScript Error: Property 'studentId' is private and only accessible within class 'Student'.
    this.studentGrade = grade;
    this.studentMajor = major;
  }
}

const myStudent = new Graduate(3, 1234, 'computer science');

console.log(myStudent.id()); // prints 1234
// myStudent.studentId = 1235; // TypeScript Error: Property 'studentId' is private and only accessible within class 'Student'.
console.log(myStudent.id()); // prints 1234
```

In this example, we're defining a `Student` class with `protected` and `private` properties, and a `Graduate` class that extends the `Student` class and has a `public` property. We're then creating a new `Graduate` object called `myStudent` and calling the `id` method on it.

## Factory Function
you can use Factory Functions to create objects with explicit typing. To do this, you first define an interface that describes the object's properties and methods. This interface serves as a blueprint for the object's structure and behavior.

Next, you create a factory function that takes in the necessary parameters and returns an object that conforms to the interface. This allows you to create objects with a specific structure and behavior that can be easily reused throughout your code.

```ts
interface Animal {
  name: string;
  age: number;
  speak(): void;
}

const animalFactory = (name: string, age: number): Animal => {
  const speak = (): void => console.log('Hello!');
  return { name, age, speak };
}

const myAnimal = animalFactory('Rex', 3);
```

In this example, we first define an interface called `Animal` that has properties for `name`, `age`, and a method called `speak()`.

Next, we define a `animalFactory` function that takes in `name` and `age` parameters and returns an object that conforms to the `Animal` interface. The function creates a `speak()` method that simply logs "Hello!" to the console.

Finally, we create a `myAnimal` object using the `animalFactory` function and passing in values for `name` and `age`. This creates an object with the `speak()` method that we can use throughout our code.

## Generics 

Generics are a powerful feature of TypeScript that allow you to `write code that can work with a variety of types`. They are similar to templates in other programming languages, but with some additional features specific to TypeScript.

To define a generic type or function, you use the syntax `<T>` to indicate that you want to use a type parameter named `T`. Here's an example of a simple generic function that takes an array of `T` values and returns the first element:

```typescript
function first<T>(arr: T[]): T {
  return arr[0];
}

const numbers = [1, 2, 3];
const firstNumber = first(numbers);
console.log(firstNumber); // 1

const strings = ["hello", "world"];
const firstString = first(strings);
console.log(firstString); // "hello"
```

In this example, we've defined a generic function called `first` that takes an array of `T` values and returns the first element. We then call this function with two different arrays, one of numbers and one of strings, and it works correctly in both cases.



### Generics and the Promise Return Type

```typescript
async function fetchJson<T>(url: string): Promise<T> {
  const response = await fetch(url);
  const data = await response.json();
  return data as T;
}

interface User {
  id: number;
  name: string;
  email: string;
}

async function main() {
  const user = await fetchJson<User>("https://jsonplaceholder.typicode.com/users/1");
  console.log(`User ${user.name} has email ${user.email}`);
}

main();
```

In this example, we have defined a function called `fetchJson` that takes a URL and returns a Promise that resolves with JSON data fetched from that URL. We have used a type parameter `T` to specify the type of the resolved value.

We then call the `fetchJson` function with a URL that returns JSON data representing a user object. We have specified the type of `T` as an interface called `User`, which has three properties: `id`, `name`, and `email`.

When the Promise resolves, we get a `User` object back, and we can access its properties using dot notation. In this case, we are logging the `name` and `email` of the user to the console.
