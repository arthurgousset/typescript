# TypeScript (Cheat Sheet)

This is a cheat sheet on TypeScript (mostly notes-to-self). They are incomplete by default.

### Initialize a TypeScript project

```sh
yarn init -y # quickly initializes nodejs project with package.json file
yarn add typescript --dev # installs typescript
yarn tsc --init # initializes TS project with tsconfig.json file
```

You need to use `yarn tsc --int` instead of `tsc --init` because `tsc` is not installed globally.
When you run `yarn` (or `npm install`), the dependencies and devDependencies from your
`package.json` are installed locally within the `node_modules` folder of your project. 
However, the binaries from the local `node_modules/.bin` directory are not automatically 
available in your terminal environment. This is why when you try to run `tsc --init` directly, 
the terminal is unable to find the command, as it's not installed globally.

Source: ChatGPT

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

## TypeScript for JavaScript Programmers

Source: 
[typescriptlang.org](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

### Types by Inference

> TypeScript knows the JavaScript language and will generate types for you in many cases. 
> For example in creating a variable and assigning it to a particular value, TypeScript will use 
> the value as its type.

```ts
let helloWorld = "Hello World";
// let helloWorld: string
```

> By understanding how JavaScript works, TypeScript can build a type-system that accepts JavaScript 
> code but has types. This offers a type-system without needing to add extra characters to make 
> types explicit in your code.

> You may have written JavaScript in Visual Studio Code, and had editor auto-completion. 
> Visual Studio Code uses TypeScript under the hood to make it easier to work with JavaScript.

### Defining Types

> You can use a wide variety of design patterns in JavaScript. However, some design patterns make 
> it difficult for types to be inferred automatically (for example, patterns that use dynamic 
> programming). To cover these cases, TypeScript supports an extension of the JavaScript language, 
> which offers places for you to tell TypeScript what the types should be.

> For example, to create an object with an inferred type which includes `name: string` and 
> `id: number`, you can write:

```ts
const user = {
  name: "Hayes",
  id: 0,
};
```

> You can explicitly describe this object's shape using an `interface` declaration:

```ts
interface User {
  name: string;
  id: number;
}
```

> You can then declare that a JavaScript object conforms to the shape of your new `interface` 
> by using syntax like `: TypeName` after a variable declaration:

```ts
const user: User = {
  name: "Hayes",
  id: 0,
};
```

> If you provide an object that doesn't match the interface you have provided, TypeScript will warn 
> you:

```ts
interface User {
	name: string;
	id: number;
}

const user: User = {
	username: "Hayes",
	id: 0,
};
// Type '{ username: string; id: number; }' is not assignable to type 'User'. 
// Object literal may only specify known properties, and 'username' does not exist in type 'User'.
```

> Since JavaScript supports classes and object-oriented programming, so does TypeScript. 
> You can use an interface declaration with classes:

```ts
interface User {
  name: string;
  id: number;
}
 
class UserAccount {
  name: string;
  id: number;
 
  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
 
const user: User = new UserAccount("Murphy", 1);
```

You can use interfaces to annotate parameters and return values to functions:

```ts
function deleteUser(user: User) {
  // ...
}
 
function getAdminUser(): User {
  //...
}
```

> There is already a small set of primitive types available in JavaScript: `boolean`, `bigint`, 
> `null`, `number`, `string`, `symbol`, and `undefined`, which you can use in an interface. 
> TypeScript extends this list with a few more, such as `any` (allow anything), 
> [`unknown`](https://www.typescriptlang.org/play#example/unknown-and-never) (ensure someone using 
> this type declares what the type is), 
> [`never`](https://www.typescriptlang.org/play#example/unknown-and-never) (it's not possible that 
> this type could happen), and `void` (a function which returns `undefined` or has no return value).

> You'll see that there are two syntaxes for building types: 
> [Interfaces and Types](https://www.typescriptlang.org/play/?e=83#example/types-vs-interfaces). 
> You should prefer `interface`. Use `type` when you need specific features.

### Composing Types

> With TypeScript, you can create complex types by combining simple ones. There are two popular ways 
> to do so: with unions, and with generics.

#### Unions

> With a union, you can declare that a type could be one of many types. For example, you can describe a `boolean` type > as being either `true` or `false`:

```ts
`type MyBool = true | false;
```

> *Note:* If you hover over `MyBool` above, you'll see that it is classed as `boolean`. 
> That's a property of the Structural Type System. More on this below.

> A popular use-case for union types is to describe the set of `string` or `number` 
> [literals](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) 
> that a value is allowed to be:

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

> Unions provide a way to handle different types too. For example, you may have a function that 
> takes an `array` or a `string`:

```ts
function getLength(obj: string | string[]) {
  return obj.length;
}
```

> To learn the type of a variable, use `typeof`:

| Type | Predicate |
| --- |  --- |
| string | `typeof s === "string"` |
| number | `typeof n === "number"` |
| boolean | `typeof b === "boolean"` |
| undefined | `typeof undefined === "undefined"` |
| function | `typeof f === "function"` |
| array | `Array.isArray(a)` |

For example, you can make a function return different values depending on whether it is passed a 
string or an array:

```ts
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
	// (parameter) obj: string
  }
  return obj;
}
```

#### Generics

> Generics provide variables to types. A common example is an array. An array without generics 
> could contain anything. An array with generics can describe the values that the array contains.

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

> You can declare your own types that use generics:

```ts
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
// Argument of type 'number' is not assignable to parameter of type 'string'.
```

### Structural Type System

> One of TypeScript's core principles is that type checking focuses on the *shape* that values have. 
> This is sometimes called "duck typing" or "structural typing".

> In a structural type system, if two objects have the same shape, they are considered to be of the 
> same type.

```ts
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

> The `point` variable is never declared to be a `Point` type. However, TypeScript compares the 
> shape of `point` to the shape of `Point` in the type-check. They have the same shape, so the code passes.

> The shape-matching only requires a subset of the object's fields to match.

```ts
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"

const color = { hex: "#187ABF" };
logPoint(color);
// Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'. 
// Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

> There is no difference between how classes and objects conform to shapes:

```ts
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

> If the object or class has all the required properties, TypeScript will say they match, 
> regardless of the implementation details.

## What is a `tsconfig.json`

Source: [typescriptlang.org](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

> The presence of a `tsconfig.json` file in a directory indicates that the directory is the root of 
> a TypeScript project. The `tsconfig.json` file specifies the root files and the compiler options 
> required to compile the project.

> A project is compiled in one of the following ways using`tsconfig.json`or`jsconfig.json`:
> 
> +   By invoking tsc with no input files, in which case the compiler searches for the 
> 	`tsconfig.json` file starting in the current directory and continuing up the 
> 	parent directory chain.
> 
> +   By invoking tsc with no input files and a `--project` (or just `-p`) command line option that 
> 	specifies the path of a directory containing a `tsconfig.json` file, or a path to a valid 
> 	`.json` file containing the configurations.

Example `tsconfig.json` files:

+   Using the [`files`](https://www.typescriptlang.org/tsconfig#files) property

    ```json
	{
	"compilerOptions": {
		"module": "commonjs",
		"noImplicitAny": true,
		"removeComments": true,
		"preserveConstEnums": true,
		"sourceMap": true
	},
	"files": [
		"core.ts",
		"sys.ts",
		"types.ts",
		"scanner.ts",
		"parser.ts",
		"utilities.ts",
		"binder.ts",
		"checker.ts",
		"emitter.ts",
		"program.ts",
		"commandLineParser.ts",
		"tsc.ts",
		"diagnosticInformationMap.generated.ts"
	]
	}
    ```

+	Using the [`include`](https://www.typescriptlang.org/tsconfig#include) and 
	[`exclude`](https://www.typescriptlang.org/tsconfig#exclude) properties

	```json
	{
	"compilerOptions": {
		"module": "system",
		"noImplicitAny": true,
		"removeComments": true,
		"preserveConstEnums": true,
		"outFile": "../../built/local/tsc.js",
		"sourceMap": true
	},
	"include": ["src/**/*"],
	"exclude": ["**/*.spec.ts"]
	}
	```

### TSConfig Bases

> Depending on the JavaScript runtime environment which you intend to run your code in, there may be 
> a base configuration which you can use at 
> [github.com/tsconfig/bases](https://github.com/tsconfig/bases/). These are `tsconfig.json` files 
> which your project extends from which simplifies your `tsconfig.json` by handling the runtime 
> support.
> 
> For example, if you were writing a project which uses Node.js version 12 and above, then you 
> could use the npm module [`@tsconfig/node12`](https://www.npmjs.com/package/@tsconfig/node12):

```json
{
  "extends": "@tsconfig/node12/tsconfig.json",
  "compilerOptions": {
    "preserveConstEnums": true
  },
  "include": ["src/**/*"],
  "exclude": ["**/*.spec.ts"]
}
```

> This lets your `tsconfig.json` focus on the unique choices for your project, and not all of the 
> runtime mechanics. There are a few tsconfig bases already, and we're hoping the community can 
> add more for different environments.

## tsc CLI Options

### Using the CLI

> Running `tsc` locally will compile the closest project defined by a `tsconfig.json`, or you can 
> compile a set of TypeScript files by passing in a glob of files you want. When input files are 
> specified on the command line, `tsconfig.json` files are ignored.

```bash
# Run a compile based on a backwards look through the fs for a tsconfig.json
tsc

# Emit JS for just the index.ts with the compiler defaults
tsc index.ts

# Emit JS for any .ts files in the folder src, with the default settings
tsc src/*.ts

# Emit files referenced in with the compiler settings from tsconfig.production.json
tsc --project tsconfig.production.json

# Emit d.ts files for a js file with showing compiler options which are booleans
tsc index.js --declaration --emitDeclarationOnly

# Emit a single .js file from two files via compiler options which take string arguments
tsc app.ts util.ts --target esnext --outfile index.js
```

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
