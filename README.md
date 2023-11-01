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

+	In Java:

	+	everything belongs to a class or interface

	+	it's meaningful to think of a one-to-one correspondence between runtime types and 
		their compile-time declarations

	+	types are related to their declarations, not their structures.

	+	the type system is (reified) _nominal_.

		> A [type system](https://en.wikipedia.org/wiki/Type_system) is **nominal,** 
		> **nominative,** or **name-based** if compatibility and equivalence of 
		> [data types](https://en.wikipedia.org/wiki/Data_type) is determined by explicit 
		> declarations and/or the name of the types. Nominal systems are used to determine if types 
		> are equivalent, as well as if a type is a subtype of another. 
		> 
		> Nominal type systems contrast with 
		> [structural systems](https://en.wikipedia.org/wiki/Structural_type_system), where 
		> comparisons  are based on the structure of the types in question and do not require
		> explicit declarations.

		Source: [wikipedia.org](https://en.wikipedia.org/wiki/Nominal_type_system)

+	In TypeScript:

	+	_free functions_ (those not associated with a class) working over data without an implied 
		OOP hierarchy are the preferred model for writing programs (in JavaScript more broadly)
	+	types are _sets_ (a particular value can belong to *many* sets or types at the same 
		time)
	+	classes and many common patterns such as  interfaces, inheritance, and static methods
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