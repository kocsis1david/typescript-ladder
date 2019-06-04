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

But it's better to write out the return type if it's not void

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

## Type annotations

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
// ERROR: string is not assignable to a number
arr[0] = "something";
~~~

---

### In const context, it has a tuple type

~~~ts
const arr = [1, "string"] as const; // [1, "string"]
const x = arr[0]; // 1
~~~

Const context was introduced in TypeScript 3.4

Literal types are in the Advanced beginner section of the tech ladder.

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

And now there are errors for nullability:

~~~ts
const x: string = null; // error
const y: string | null = null; // ok

const z: string | null = undefined; // error
const w: string | undefined = undefined; // ok
~~~

---

## Primitive types	

---

## Structural type system

---

## Function declarations & function expressions

### Generic arrow function

~~~ts
const chooseFromTwo = <T>(a: T, b: T) => {
    if (Math.random() < 0.5) {
        return a;
    } else {
        return b;
    }
};

const result = chooseFromTwo("alma", "körte");
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

## Interfaces

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

But you get an error if a property is less strict than the base interface's property:

~~~ts
// not allowed
interface Test3 extends Test {
    a: number | null;
}
~~~

---

## String templates & tagged templates