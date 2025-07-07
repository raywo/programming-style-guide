# Style guide for programming

This style guide aims not to replace existing style guides for programming 
languages. Instead, it tries to augment those guides with some (opinionated) 
rules to simplify coding in a multilanguage environment.

## Why this style guide?

In environments where multiple programming languages are used, it is often 
challenging to find a programming style that suits all languages. Yet it is 
very desirable to be consistent with your coding style regardless of the used 
programming language. 

This guide tries to suggest rules that can be applied to every programming 
language without conflicting with existing style guides.

## Conventions

### General

Use `UTF-8` as file encoding whenever possible.

On smaller displays or situations where multiple files are displayed side by 
side, it is desirable to not have to scroll horizontally. Therefore, aim for a 
maximum of **80** characters per line. If not possible, do a hard break at 120 
characters at the latest.

### Comments

There are two distinct types of comments. Comments for documentation (like
JavaDoc) and comments that explain what the code does. Avoid the latter!

Aim for code that explains itself. Instead of having a complex if statement
which needs explanation, extract the statement to a variable or method with a
self-explanatory name.

This is not an endorsement to completely avoid comments. Use comments whenever
they seem appropriate.

**:x: Wrong**

```typescript
// has correct length and starts with an uppercase S
if (line.length >= 10 && line.length <= 50 && line.startsWith("S")) {
  /* ... */
}
```

**:white_check_mark: Good**

```typescript
const hasValidLength = line.length >= 10 && line.length <= 50;

if (hasValidLength && line.startsWith("S")) {
  /* ... */
}
```

> [!NOTE]
> Consider a comment as an antipattern. Whenever you feel the need to write a comment, try to avoid it.

### Strings

Some languages allow for multiple different styles of quotes. Unless there is
a special meaning attached to it, choose one style and stick with it. If
possible, use linter or prettifier to enforce the choice.

**:x: Wrong**

```typescript
const hello = 'hello';
const world = "world";
```

**:white_check_mark: Good**

```typescript
const hello = "hello";
const world = "world";
```

or

```typescript
const hello = 'hello';
const world = 'world';
```

### Magic numbers / strings

Magic numbers or strings are values typically used for method calls or
comparisons. A reader can normally not know what these values mean and why they
are set the way they are. Use instead constants with a meaningful name.

**:x: Wrong**

```typescript
if (line.length >= 10 && line.length <= 50) {
  /* ... */
}
```

**:white_check_mark: Good**

```typescript
const MIN_LENGTH = 10;
const MAX_LENGTH = 50;

if (line.length >= MIN_LENGTH && line.length <= MAX_LENGTH) {
  /* ... */
}
```

### Blocks

Blocks are lines of code with a start and an end. In many programming languages
blocks are enclosed in braces (`{}`). Some other languages (Python) blocks are
given through indentation.

Methods are the prime example for blocks. `for`- and `if`-statements also
constitute blocks, as well as `try-catch`-statements. In many languages a block
can even be started inside a method.

#### Length of blocks

Typically, any block should do only one thing. This is usually referred to as
the [Single Responsibility Principle (SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle).
Adhering to this principle leads to more easily readable and maintainable code.
If a block or method is only doing one thing, it can be held very shortly. Aim
for **ten** to **fiveteen** lines at most. In more verbose languages like Java,
this can go higher.

This rule is only a guiding principle. There is no such thing as “right” or
“wrong” in this case. Shorter blocks are easier to read and therefore easier
to understand.

> [!TIP]
> Aim for 10 to 15 lines. At 25 lines really look at the code and try to simplify it.

#### Placing of braces

In languages that support defining blocks with braces, choose a style and stick
to it. If there is already a style guide for a language, stick to this style
guide. Java, for instance, defines that braces should start at the end of a line,
where as most C/C++ style guides suggest starting at a new line. Choose one and
stick to it.

#### One line blocks

Especially `if`-statements are often very short, and the body contains only one 
line of code. Most languages allow omitting the braces for the body. Do not omit 
braces! This often leads to hard to find errors when adding more lines later.

**:x: Wrong**
```typescript
if (someCondition) 
  myMethod();
```

**:white_check_mark: Good**
```typescript
if (someCondition) {
  myMethod();
}
```

> [!TIP]
> If the condition and the action are very short, it is possible to put everything 
> on one line.
> ```typescript
> if (someCondition) myMethod();
> ```

**:x: Wrong**

```typescript
function myMethod1() {
  /* ... */
}

function myMethod2() {
  /* ... */
}
```

**:white_check_mark: Good**

```Java
void myMethod1() {
  /* ... */
}


void myMethod2() {
  /* ... */
}
```

or

```CPP
void myMethod1() 
{
  /* ... */
}


void myMethod2() 
{
  /* ... */
}
```

### Naming

Naming things in programming is arguably one of the most challenging tasks. A
general rule of thumb is to use a name which clearly describes what the named
thing is supposed to do or which role it plays in the grand theme of things.
Aim for short yet meaningful names.

How to write the names is strongly dependent on the chosen programming language.
Many languages define a camel case scheme (`camelCase`), others prefer a
snake case scheme (`snake_case`). If there are any style guides in the chosen
language, stick to the rules outlined there.

#### Variable names

Use descriptive names for variables. The variable name should make it possible
for the reader to know what is stored in this variable and what role it plays
in the program. Usually very short names (one or two letters) are no good names
for variables. Yet there are exceptions. If you want to store x and y
coordinates in a grid `x` and `y` are suitable names.

Avoid abbreviations.

**:x: Wrong**

```typescript
const len = name.length;
const l = name.length;
```

**:white_check_mark: Good**

```typescript
const length = name.length;
```

#### Constant names

To distinguish constants from variables, constants in many languages usually
use all uppercase letters.

In TypeScript, Java and other languages the `const` respectively `final` is
often used to prevent references to be changed later. This is not considered
a constant in this documentation. In those cases the naming of variables applies.

“Proper” constants are values that are set once and will definitely not change
during the execution of the program.

Apart from using uppercase letters the same considerations as for variables
apply.

```typescript
// An example for a variable which uses the const keyword
const returnValue = aMethod();
// A “proper” constant
const MIN_LENGTH = 10;
```

#### Method names

Methods are blocks of code that execute a certain task. This should reflect in
their naming. Use **verbs** in method names. Some more modern approaches to
naming like to omit the verbs. Instead of `getSettings()` the method would be
named just `settings()`. As long as it is crystal clear what the method is
supposed to do, this might be okay. Since this is usually not the case, still
use verbs in method names.

> [!TIP]
> If naming a method is hard, check if the method is actually doing only one
> thing. Finding it hard to name methods is usually a good indicator for a wrong
> abstraction.

Methods that only use a verb as their name are problematic. Consider a method
named `run()`. What should be run? This is not immediately clear to the reader.
Yet if the method is part of a class named `Task`, it might be clear what `run()`
means. Always consider the context when naming.

#### Class names

Classes are “things” that fulfill a certain purpose in the program. Use nouns
to name classes. In many languages class names are capitalized and use camel
case. Stick to the style guides of the used programming language.

**:x: Wrong**

```typescript
class Run {
}
```

In the above example it would be hard to name the method that actually runs
the code since `run()` is already used as a class name. Finding it hard to name
methods in a class can also serve as an indicator for good or bad class names.

**:white_check_mark: Good**

```typescript
class Runner {
}
```

Do not name interfaces with a leading `I`. An interface is still a type and
should therefore adhere to the naming principle of classes.

### Spacing

Spacing (i.e., blank lines) conveys a meaning. If lines of codes do similar
things like declaring variables or implementing an algorithm, it can be
beneficial to separate those “blocks” with blank lines. This makes it easier
for the reader to see the structure of the code.

#### Blocks

Always enclose blocks (`for`, `if`, ...) with blank lines. If the block is the
first statement in a method, omit the leading blank line. If the block is the
last statement in a method, omit the trailing blank line.

**:x: Wrong**

```typescript
function method() {
  const aVariable = anotherMethod();
  if (aVariable === "what I wanted") {
    /* ... */
  }
  return aVariable;
}
```

**:white_check_mark: Good**

```typescript
function method() {
  const aVariable = anotherMethod();

  if (aVariable === "what I wanted") {
    /* ... */
  }

  return aVariable;
}
```

Always separate a `return`-statement from the rest of the code. If the statement
is the first statement in an enclosing block, omit the leading blank line.

**:x: Wrong**

```typescript
function method(value: SomeEnum) {
  switch (value) {
    case "value1":

      return "A";

    case "value2":

      return "B";

    default:

      return "C";
  }
}
```

**:white_check_mark: Good**

```typescript
function method(value: SomeEnum) {
  switch (value) {
    case "value1":
      return "A";
    case "value2":
      return "B";
    default:
      return "C";
  }
}
```

Since a `case` constitutes a block the following is good too. Chose whatever is 
better readable.

```typescript
function method(value: SomeEnum) {
  switch (value) {
    case "value1":
      return "A";
      
    case "value2":
      return "B";
      
    default:
      return "C";
  }
}
```

#### Methods

Always use **two** leading blank lines with methods.

#### Variables

Don’t separate variables with blank lines.

If different visibilities (`private`, `public`,…) are declared, separate the
blocks with blank lines.

**:x: Wrong**

```typescript
class AClass {
  private readonly hello = "hello"; // used as a variable, therefore no uppercase name
  private readonly world = "world"; // used as a variable, therefore no uppercase name
  public greeting = "Your name";
}
```

**:white_check_mark: Good**

```typescript
class AClass {
  private readonly hello = "hello"; // used as a variable, therefore no uppercase name
  private readonly world = "world"; // used as a variable, therefore no uppercase name

  public greeting = "Your name";
}
```

### Order of things

Always keep structurally similar things grouped together. Start with declaring
variables / constants, then proceed with the algorithmic things.

In classes group by structure and then by visibility. Start with instance /
class variables / constants, then declare constructors, then declare methods.

#### Variables / Constants

Always declare “proper” constants first, then declare the variables.

Order by visibility.

1. `private`
2. `protected`
3. `public`

**:white_check_mark: Good**

```typescript
class AClass {

  public readonly A_PROPER_CONSTANT = "Avoids a magic string";

  private readonly field = 0;
  private readonly anotherField = 0;

  protected aProtectedField = 0;


  public constructor() {
    /* ... */
  }


  public aMethod() {
    /* ... */
  }


  public anotherMethod() {
    /* ... */
  }


  private aPrivateMethod() {
    /* ... */
  }

}
```

#### Methods

Always group methods by visibility. Declare the `public` methods first. They
define the usable API of a class and should therefore be declared first. Follow
with `protected` methods and then with `private` methods.

Aim to group methods also by their purpose. This is not always possible.
Consider a `public` function that uses some `private` helper functions. If other
`public` methods are present too, it is not always possible to group by function.

## Author

Ray Wojciechowski
