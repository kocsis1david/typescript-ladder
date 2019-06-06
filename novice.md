class: center, middle

# Typescript Tech Ladder novice

http://www.techladder.io/?tech=typescript

---



## Playground

https://www.typescriptlang.org/play/index.html

Set all options to strict.

Allow implicit return:

~~~ts
function findIndex<T>(arr: T[], value: T): number | undefined {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === value) {
            return i;
        }
    }
}
~~~

---



## Type inference

### Function return type is inferred

~~~ts
function add(x: number, y: number) {
    return x + y;
}
~~~

Not the best idea, only omit the return type, if it's `void`

### Objects fields are also inferred

~~~ts
const obj = {
    a: "string",
    b: [1, 2],
};

const a = obj.a; // string
const b0 = obj.b[0]; // number
~~~

---



## Type annotations #1

### Arrays have the best common type of the elements

~~~ts
const arr = [1, "string"]; // (string | number)[]
const x = arr[0]; // string | number
~~~

This allows modifying `arr` later:

~~~ts
arr[0] = "something";
~~~

### Annotating `arr` with a tuple type

~~~ts
const arr: [number, string] = [1, "string"];
const x = arr[0]; // number
~~~

~~~ts
// error: string is not assignable to a number
arr[0] = "something";
~~~

---



## Type annotations #2

### In const context, it has a tuple type

~~~ts
const arr = [1, "string"] as const; // [1, "string"]
const x = arr[0]; // 1
~~~

Const context was introduced in TypeScript 3.4

Literal types are in the Advanced beginner section of the tech ladder.

---



## Primitive types #1

### Similar to ther languages

`boolean`, `number`, `string`, `void`, arrays, tuples, enums

### `null` and `undefined`

~~~ts
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
~~~

### `never`

~~~ts
function infiniteLoop(): never {
    while (true) {}
}
~~~

---



## Primitive types #2

### `any` - Assign anything to it

~~~ts
let x: any;

x = true;
x = "asd";
x = [4, 5];
~~~

### `object` - Only allows JS objects

~~~ts
let x: object = {
    something: true,
};

x = false; // error
x = "something"; // error

x = { // ok
    otherThing: false,
}; 
~~~

---



## Primitive types #3

### The `unknown` type

Safer than `any`.

~~~ts
let x: unknown = 4;
let y: string = x; // error
let z: string = x as string; // ok, but ...
~~~

With `any`, there is no compile error at all:

~~~ts
let x: any = 4;
let y: string = x; // ok
~~~

And it runs in the browser without error.

---



## Non-nullable types

Without scrict mode:

~~~ts
const x: string = null; // ok
const y: string = undefined; // ok
~~~

Strict mode needs to be turned on in `.tsconfig`

~~~json
{
    "compilerOptions": {
        "strict": true,
        ...
    }
}
~~~

And now there are good errors for nullability:

~~~ts
const x: string = null; // error
const y: string | null = null; // ok

const z: string | null = undefined; // error
const w: string | undefined = undefined; // ok
~~~

---



## Structural type system #1

### C# is a nominally-typed language

~~~cs
struct A
{
    public int x;
}

struct B
{
    public int x;
}

public static void Main(String[] args)
{
    var a = new A()
    {
        x = 10,
    };

    B b = a; // error
}
~~~

This is not allowed, becuase struct `A` and `B` have different names.

---



## Structural type system #2

### Typescript is structurally-typed

~~~ts
interface A {
    x: number;
}

interface B {
    x: number;
}

const a = {
    x: 10,
};

const b: B = a; // ok
~~~

In a structural type system, the name of the type doesn't matter.

When assigning a value to a variable, each member is checked recursively.

---



## Structural type system #3

~~~ts
interface HasLength {
    length: number;
}

printLength("asdasd"); // ok
printLength([3, 5]); // ok
printLength({ length: 3 }); // ok
printLength(3); // error

function printLength(obj: HasLength) {
    console.log(obj.length);
}
~~~

`printLength` accepts anything that has a `length`.

No need to implement `HasLength` explicity.

---



## Structural type system #4

### Ignored argument
~~~ts
function test1(func1: (x: number) => number) {
    test2(func1);   
}

function test2(func2: (x: number, y: number) => number) {
    func2(1, 2);
}

test1(x => x + 1);
~~~

`func1` is a subtype of `func2`, because you can assign `func1` to `func2`.

---



## Structural type system #5

### Each parameter and return type is checked recursively

~~~ts
function something(x: boolean | null): string {
    return x ? "asd" : "asdasd";
}

const f: (x: boolean, y: number[]) => string | number = something; // ok
~~~

Functions are covariant for the return type and contravariant for the parameters.

---



## Function declarations & function expressions #1

### Optional parameters

~~~ts
function test(x: number, y?: string) {
    const yy: string | undefined = y;

    console.log(yy.length); // error

    if (yy) {
        console.log(yy.length); // ok
    }
}

test(10);
~~~

### Default parameters

~~~ts
function test(x: number, y: string = "alma") {
    console.log(y.length); // ok
}

test(10);
~~~

---



## Function declarations & function expressions #3

### Rest & spread

~~~ts
function sum(...numbers: number[]): number {
    return numbers.reduce((x, y) => x + y, 0);
}

console.log(sum()); // 0
console.log(sum(2, 5, 1)); // 8
console.log(sum(...[2, 3, 4].map(x => x * x))); // 29
~~~

In C#, the `params` keyword has the same purpose.

---



## Function declarations & function expressions #2

### Generic arrow function

~~~ts
const chooseFromTwo = <T>(a: T, b: T) => {
    if (Math.random() < 0.5) {
        return a;
    } else {
        return b;
    }
};

const a = chooseFromTwo("alma", "körte");
const b = chooseFromTwo(2, 56);
~~~

---



## Tuples

Tuples are a subset of arrays with different types for each element.

~~~ts
const tuple: [number, string, boolean, null] = [1, "asd", true, null];
const x = tuple[2]; // boolean
~~~

Optional elements in the tuple:

~~~ts
let tuple: [string, boolean?];

tuple = ["asd"]; // ok
tuple = ["asd", false]; // ok
tuple = ["asd", false, "anything"]; // error
~~~

Rest elements:

~~~ts
let tuple2: [string, ...(number | null)[]];

tuple2 = ["asd"]; // ok
tuple2 = ["asd", 1]; // ok
tuple2 = ["asd", null, null, 1]; // ok
tuple2 = ["asd", "string"]; // error
~~~

---



## Interfaces #1

Interfaces lets you specify what properties are needed for the function:

~~~ts
interface Test {
    a: number,
    b: string,
    c?: boolean, // optional property
}

function func(obj: Test) {
    const c = obj.c; // boolean | undefined
    console.log(obj);
}

func({
    a: 1,
    b: "10",
    // c is omitted
});

func({
    a: 1,
    b: "10",
    c: Math.random() < 0.5,
});
~~~

---



## Interfaces #2

Interfaces can be extended to have stricter requirements on the object:

~~~ts
interface Test {
    a: number,
    b?: string,
}

interface Test2 extends Test {
    b: string,
    c: boolean,
}

const obj2: Test2 = {
    a: 1,
    b: "aasd",  // must be specified
    c: true
};
~~~

You gen an error if a property is less strict:

~~~ts
interface Test3 extends Test {
    a: number | null; // error
}
~~~

---



## Interfaces #3

Indexable, callable, etc interfaces

---



## Modules

---



## String templates & tagged templates