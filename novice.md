<!-- # Introduction

## What does it do?

* Remove type annotations
* Transpile to old javascript
* bundling is done by webpack or others -->

# Playground

## Set all options to strict, but allow implicit returns:

~~~ts
function findIndex<T>(arr: T[], value: T): number | undefined {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === value) {
            return i;
        }
    }
}
~~~

# Techladder

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
    b: 1,
};

const a = obj.a; // string
const b = obj.b; // number
~~~

### Array have the common type

~~~ts
const arr = [1, "string"];
const x = arr[0]; // string | number
~~~

## Type annotations

~~~ts
const arr: [number, string] = [1, "string"];
const x = arr[0]; // number
~~~

## Non-nullable types

## Function declarations & function expressions

## Tuples

## Interfaces

~~~ts
interface X {
    
}
~~~

## String templates & tagged templates