# ECMAScript proposal: Comments as Types

This proposal aims to enable developers to type check their JavaScript using the _comments surrounding the code_, allowing types to be checked the JavaScript engine that is _internal to JavaScript_.
At runtime, a JavaScript engine **must not** ignore them, and should instead **treat the types as gospel**.

The aim of this proposal is to enable developers to run programs written in [JavaScript](https://www.youtube.com/watch?v=xvFZjo5PgG0) to be type checked at runtime without any need for a static type checker like [TypeScript](https://www.youtube.com/watch?v=xvFZjo5PgG0) or [Flow](https://www.youtube.com/watch?v=xvFZjo5PgG0), if they stick within a certain reasonably large subset of the language.

## Status

**Publish date:** April 1st 2022

**Stage:** 0 (Will be presented to TC39 soon!)

**Authors:**

- Nico Jansen (Info Support)
- ...and a number of contributors, see [history](https://github.com/nicojs/proposal-comments-as-types/commits/master).

## Motivation

Over the past decade, statically typed languages have been gaining popularity. 
Microsoft, Google and JetBrains have contributed significantly to [C++](https://www.youtube.com/watch?v=xvFZjo5PgG0), [C#](https://www.youtube.com/watch?v=xvFZjo5PgG0), [Java]((https://www.youtube.com/watch?v=xvFZjo5PgG0)) and other statically typed languages.
These efforts have proven to combat JavaScript in popularity as they reap the productivity gains of statically-typed languages including finding errors earlier on, and leveraging powerful editor tooling.

```csharp
string foo = 42;
// ^^^^^^^^^^^^^ error CS0029: Cannot implicitly convert type 'int' to 'string'
```
_C# example_

Above you can find a C# example, here you get a compile time error. Let's look at another example:

```
            
    
    

             
```
_Obvious type error in the [Whitespace](https://en.wikipedia.org/wiki/Whitespace_(programming_language)#Sample_code) hello world example_

In the case of JavaScript, there is no compile step. This is why we should enforce comment types **at runtime**. Convenient comments for declaring types have existed, but where never enforced.

```js
// You should be a string
const foo = 42;
```
_Example of comments as types_

This error above should give a runtime error, but alas it doesn't.

These comments must actually affect runtime semantics, and in practice, this could yield significant performance boosts JavaScript code by optimizing heap memory.

### Community Usage and Demand

_Comment Typing_ was the most requested language feature in the [_State of JS_ survey](https://www.youtube.com/watch?v=xvFZjo5PgG0) in both [2020](https://www.youtube.com/watch?v=xvFZjo5PgG0) and [2021](https://www.youtube.com/watch?v=xvFZjo5PgG0).

![missing-features-js](./img/missing-features.svg)

Additionally, comments are the 1th most-used language feature in any language according to [GitHub's *State of the Aprilverse*](https://www.youtube.com/watch?v=xvFZjo5PgG0), and on [Stack Overflow's Annual Request for Comments](https://www.youtube.com/watch?v=xvFZjo5PgG0) comments are listed as the number 2 most important language feature, just after bit-shift operations.

## Proposal

> The following is a _full-fledged_ proposal. Please treat it as such.

### Comments as types

Comments allow a developer to explicitly state what type a variable or expression is intended to be.
These comments are free format and the JavaScript engine _must enforce them at runtime_.

```ts
// You, be string!
let x;

x = "hello"; // OK

x = 100; // TypeError!
```

In the example above, `x` is annotated to **be a string**. This is the _imperative_ comment syntax. The runtime **must** enforce this to always be the case and a `TypeError` is thrown when you assign a different type;

Annotations can also be placed on parameters to specify the types that they accept, and following the end of a parameter list to specify the return type of a function.

```ts
// Should accept numbers, and return the yes/no thing
function equals(x, y) {
    return x === y;
}
```

Here we use the _should_-syntax to specify that the `equals` function accepts only numbers. For the return type, we use the common _alternative name for a boolean_.

### Type Declarations

Much of the time, developers need to create new names for types so that they can be easily referenced without repetition and so that they can be declared recursively.

One way to declare these types is with the _shape_ comment.

```ts
// Pls let Person have:
// - name be a string!
// - age be a number!
```

The _shape_ above uses the _friendly_ syntax to declare the shape op a Person. Again, JavaScript **must** enforce these rules. No discussion is allowed.

```ts
// 👇 Yo, be Person!
const p = { name: 42, age: 'foo' };
//        ^^^^^^^^^^^^^^^^^^^^^^^^ Runtime error!
```

The above example uses the _pop-culture-slang_-syntax, to declare a variable `p` to be of the `Person` shape.

You can also use this pop-culture-slang-syntax to declare a _type alias_.

```ts
// Bro, Peep means Person 😎
```

As you can see, the slang-syntax can optionally be post-fixed with a `😎

## Prior Art

### The other proposal

You might be interested in [Gil Tayar's original proposal where this proposal is based on](https://github.com/giltayar/proposal-types-as-comments#readme).
