# TypeScript (Cheat Sheet)

This is a cheat sheet on TypeScript (mostly notes-to-self). They are incomplete by default.

### Initialize a TypeScript project

```bash
npm init -y # quickly initializes nodejs project with package.json file
```

```bash
npm install typescript --save-dev # installs typescript
tsc --init # initializes TS project with tsconfig.json file
```

Source: [typescriptlang.org](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

### Microsoft Cheat Sheets

Source: [typescriptlang.org](https://www.typescriptlang.org/cheatsheets)

<img src="assets/images/TypeScript-Control-Flow-Analysis.png" width="950">

<img src="assets/images/TypeScript-Types.png" width="950">

<img src="assets/images/TypeScript-Classes.png" width="950">

<img src="assets/images/TypeScript-Interfaces.png" width="950">

### TypeScript for Java/OOP Programmers

Source: [typescriptlang.org](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-oop.html)

-   In Java:

    -   everything belongs to a class or interface

    -   it's meaningful to think of a one-to-one correspondence between runtime types and
        their compile-time declarations

    -   types are related to their declarations, not their structures.

    -   the type system is (reified) _nominal_.

        > A [type system](https://en.wikipedia.org/wiki/Type_system) is **nominal,** 
        > **nominative,** or **name-based** if compatibility and equivalence of 
        > [data types](https://en.wikipedia.org/wiki/Data_type) is determined by explicit
        > declarations and/or the name of the types. Nominal systems are used to determine if types
        > are equivalent, as well as if a type is a subtype of another.
        >
        > Nominal type systems contrast with 
        > [structural systems](https://en.wikipedia.org/wiki/Structural_type_system), where
        > comparisons are based on the structure of the types in question and do not require
        > explicit declarations.

        Source: [wikipedia.org](https://en.wikipedia.org/wiki/Nominal_type_system)

-   In TypeScript:

    -   _free functions_ (those not associated with a class) working over data without an implied
        OOP hierarchy are the preferred model for writing programs (in JavaScript more broadly)
    -   types are _sets_ (a particular value can belong to *many* sets or types at the same
        time)
    -   classes and many common patterns such as interfaces, inheritance, and static methods
        are supported

For example:

```ts
// Example
interface Pointlike {
    x: number;
    y: number;
}
interface Named {
    name: string;
}

function logPoint(point: Pointlike) {
    console.log("x = " + point.x + ", y = " + point.y);
}

function logName(x: Named) {
    console.log("Hello, " + x.name);
}

const obj = {
    x: 0,
    y: 0,
    name: "Origin",
};

logPoint(obj);
logName(obj);
```

Source: [other notes](other.md#typescript-for-javac-programmers).

## Non-null assertion operator (`!`)

Source: chatGPT

The exclamation mark (`!`) is known as the non-null assertion operator. It is a post-fix expression
that essentially removes `null` and `undefined` from the type of the operand, allowing you to
assure the TypeScript compiler that the value is non-null or non-undefined.

### Background Context

In TypeScript, strict null checks can be enabled by setting the `strictNullChecks` flag in the
`tsconfig.json` file. When this is enabled, variables that could be `null` or `undefined` must be
checked before being used. This is done to prevent runtime errors due to `null` or `undefined`
values. However, there might be cases where you, as the developer, are certain that a value would
be non-null or non-undefined, even though TypeScript's type checker cannot guarantee this.
This is where the non-null assertion operator comes into play.

### Step-by-Step Thinking

1.  **Type Inference**: TypeScript tries to infer the types of variables and object properties.
    When you destructure an object, TypeScript will infer the type of the variables being destructured.

    ```ts
    const { block } = celo.formatters; // TypeScript infers the type of block
    ```

2.  **Nullable Types**: If `celo.formatters` could potentially be `null` or `undefined`, 
    TypeScript will complain that you are trying to destructure properties from a possibly `null` 
    or `undefined` object.

    ```ts
    const { block } = celo.formatters; // Error if celo.formatters could be null or undefined
    ```

3.  **Non-null Assertion**: By appending `!` after `celo.formatters`, you are telling TypeScript 
    to treat `celo.formatters` as non-null or non-undefined, even if its type suggests otherwise.

    ```ts
    const { block } = celo.formatters!; // No error, we asserted that celo.formatters is non-null
    ```

### Example

Here's a TypeScript example to illustrate:

```ts
interface Celo {
    formatters?: {
        block: string;
    };
}

// Initialize with a null value for formatters
const celo: Celo = {
    formatters: null,
};

// This will result in a TypeScript error due to potential null or undefined
// const { block } = celo.formatters;

// Using ! to assert that formatters is non-null or non-undefined.
// Note: This is risky if you're not certain that formatters will always be non-null.
const { block } = celo.formatters!;
```

### Caveats

It's important to use the non-null assertion operator judiciously. Overusing it can lead to runtime 
errors if the value turns out to be `null` or `undefined`. Always ensure that you have sufficient 
reason to believe that the value is non-null before using the `!` operator.

I hope this provides a thorough understanding of the non-null assertion operator in TypeScript.
