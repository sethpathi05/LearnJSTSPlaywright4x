# JavaScript Keywords and Identifiers

## 1. What Is an Identifier?

An **identifier** is a name used in source code to refer to a variable, function, class, parameter, or other binding.

```js
const userName = "Ava"; // userName is an identifier
function greet(person) { // greet and person are identifiers
  return `Hello, ${person}`;
}
```

Identifiers are case-sensitive: `userName`, `username`, and `UserName` are three different names.

## 2. Identifier Naming Rules

An identifier follows these rules:

1. Its first character must be `$`, `_`, or a Unicode character in the `ID_Start` category. In ordinary code this means letters are valid, but a digit is not.
2. Later characters may also include Unicode characters in `ID_Continue`, which includes digits, combining marks, and the zero-width joiner and non-joiner.
3. Spaces, hyphens, and most punctuation are not allowed in an identifier.
4. Identifiers cannot be reserved words when used as declarations or bindings.

| Valid identifiers | Invalid identifiers | Reason |
| --- | --- | --- |
| `name` | `2name` | Cannot begin with a digit |
| `_count` | `first-name` | Hyphen is not allowed |
| `$value` | `first name` | Spaces are not allowed |
| `item2` | `class` | Reserved word in a declaration |
| `café` | `user.name` | `.` is syntax, not part of an identifier |

The familiar ASCII pattern is `[A-Za-z_$][A-Za-z0-9_$]*`, but this is only a simplified approximation. JavaScript also permits many Unicode identifier characters. For example:

```js
const café = "coffee";
const 你好 = "hello";
console.log(café, 你好);
```

Unicode escapes can represent identifier characters. An escaped spelling and its literal character spelling identify the same name:

```js
const \u006Eame = "Ada";
console.log(name); // Ada
```

Escapes do not bypass keyword restrictions. For example, spelling `else` with a Unicode escape is still the reserved word `else`.

## 3. Keywords and Reserved Words

A **keyword** is a word with a special role in JavaScript syntax. A **reserved word** cannot be used as a binding name in the relevant language context.

Common reserved words include:

```text
break  case  catch  class  const  continue  debugger  default
 delete  do  else  export  extends  false  finally  for
 function  if  import  in  instanceof  new  null  return
 super  switch  this  throw  true  try  typeof  var  void
 while  with
```

For example, these are syntax errors because the words are being declared as identifiers:

```js
// SyntaxError: reserved words cannot be declared as names
let class = "Web development";
function return() {}
```

Some restrictions depend on context:

- `await` is reserved in modules and in async function bodies.
- `yield` is reserved in strict mode and in generator function bodies.
- `let` has special declaration rules and cannot be used as a binding in strict mode.
- `enum` is always reserved. `implements`, `interface`, `package`, `private`, `protected`, and `public` are reserved in strict mode.
- `eval` and `arguments` are not keywords, but cannot be declared or assigned as binding names in strict mode.

Modules and class bodies are strict mode code. When unsure, avoid using language words or syntax-related names as bindings.

## 4. Contextual Keywords

Some words act like keywords only in particular grammar positions. They are often called **contextual keywords** or **context-sensitive keywords**. Examples include `async`, `of`, `as`, and `from`.

```js
const async = "this is an ordinary binding";

async function loadData() {
  // Here async marks the function as asynchronous.
}

for (const value of values) {
  // Here of is part of the for...of syntax.
}
```

Whether a word can be used as a name depends on its exact syntactic context and strict-mode rules. Prefer clear, ordinary names over names that depend on subtle context.

## 5. Identifiers Versus Property Names

An object property name is not always an identifier. Property names can be strings, numbers, computed expressions, or identifier-like names. Reserved words are allowed as property names in object literals and member access:

```js
const settings = {
  class: "beginner",
  default: true,
};

console.log(settings.class);
console.log(settings["default"]);
```

But the same words cannot be used as variable declarations:

```js
// SyntaxError
const class = "beginner";
```

Use bracket notation when a property name contains characters that cannot appear after a dot, such as spaces or hyphens:

```js
const profile = { "display-name": "Ava" };
console.log(profile["display-name"]);
```

## 6. Naming Conventions

Naming conventions are style choices, not grammar rules. Common JavaScript conventions are:

| Kind | Common style | Example |
| --- | --- | --- |
| Variables and functions | `camelCase` | `accountBalance`, `calculateTotal()` |
| Classes and constructors | `PascalCase` | `ShoppingCart` |
| Constants intended as fixed configuration values | `UPPER_SNAKE_CASE` | `MAX_RETRIES` |
| Private class elements | `#` followed by a name | `#cache` |

Choose descriptive names, use consistent casing, and avoid confusing names that differ only by capitalization. A leading underscore or dollar sign is legal, but its meaning (if any) is a project convention.

## 7. Quick Check

Classify each item as a valid identifier declaration or an invalid one:

```js
let score2 = 10;       // Valid
let _result = "ok";   // Valid
let $total = 25;       // Valid
let 2ndPlace = "A";   // Invalid: starts with a digit
let first-name = "A"; // Invalid: hyphen is an operator
let while = true;      // Invalid: reserved word
```

Remember: **identifier rules determine which names can be written; scope and declaration rules determine where and how those names can be used.**

## References

- [MDN: Lexical grammar - Identifiers and keywords](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#identifiers)
- [MDN: Grammar and types - Basics and declarations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#basics)
- [ECMAScript Language Specification: Names and Keywords](https://tc39.es/ecma262/#sec-names-and-keywords)
