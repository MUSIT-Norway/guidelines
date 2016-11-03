# MUSIT-Norway Scala Guide

Code is written once by its author, but read and modified multiple times by lots of other engineers. As most bugs actually come from future modification of the code, we need to optimize our codebase for long-term, global readability and maintainability. The best way to achieve this is to write simple code.

## Guidelines

### Indentation and alignment

* Use 2 space indentation in general.
* For declarations (methods and classes), use 4 space indentation of parameters when they don't fit on a single line. Return types should then be on the same line as the closing `)`.
  
  ```scala
  def fooBar(  				// new line after `(`
      firstName: String,  // 4 space indentation
      middleName: String,
      lastName: String		// new line before closing `)`
  ): User = { 				// return type after closing `)`
    // method body   		// 2 space indentation
  }
  
  class Address(				// new line after `(`
      val street: String,  // 4 space indentation
      val number: Int,
      val zip: ZipCode,
      val area: String
  ) {  						// new line before closing `)`
    // class body			// 2 space indentation
  }
  ```
  
* Classes whose header doesn't fit in a single line, put the extend on the same line as the closing `)`. If the class has a long list of mixed in traits, add each one to a new line. And add a blank line between the class header and the first line of the body.

  ```scala
  
  class Foo(val p1: String, val p2: String) extends FooTrait
    with BarOps
    with Logging { // 2 space indentation
    
    def firstMethod(): Unit = { ... } // blank line above
  }
  
  ```

* Do **NOT** use vertical alignment. They draw attention to the wrong parts of the code and make the aligned code harder to change in the future.

  ```scala
  // Don't align vertically
  val plus     = "+"
  val minus    = "-"
  val multiply = "*"

  // Do the following
  val plus = "+"
  val minus = "-"
  val multiply = "*"
  ```
  
### Blank lines (Vertival whitespace)

* A single blank line appears:
	* Between consecutive members (or initializers) of a class: fields, constructors, methods, nested classes, static initializers, instance initializers.
		* Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
	* Within method bodies, as needed to create logical groupings of statements.
	* Optionally before the first member or after the last member of the class (neither encouraged nor discouraged).
* Use one or two blank line(s) to separate class definitions.
* Excessive number of blank lines is discouraged.

### Parantheses

* Methods should be declared with parentheses, unless they are accessors that have no side-effect (state mutation, I/O operations are considered side-effects).

  ```scala
  class Job {
    // Wrong: killJob changes state. Should have ().
    def killJob: Unit

    // Correct:
    def killJob(): Unit
  }
  ```
  
* Callsite should follow method declaration, i.e. if a method is declared with parentheses, call with parentheses. Note that this is not just syntactic. It can affect correctness when apply is defined in the return object.

  ```scala
  class Foo {
    def apply(args: String*): Int
  }

  class Bar {
    def foo: Foo
  }

  new Bar().foo  // This returns a Foo
  new Bar().foo()  // This returns an Int!
  ```  

### Curly brackets

Put curly braces even around one-line conditional or loop statements. The only exception is if you are using if/else as an one-line ternary operator that is also side-effect free. If either of if/else require curly brackets, then both should have them.

```scala
// Correct:
if (true) {
  println("Wow!")
}

// Correct:
if (true) statement1 else statement2

// Correct:
if (true) statement1
else statement2

// Correct:
try {
  foo()
} catch {
  ...
}

// Wrong:
if (true)
  println("Wow!")

// Wrong:
if (true) statement1
else {
  statement2
}

// Wrong:
try foo() catch {
  ...
}
```

### Long literals
Suffix long literal values with uppercase L. It is often hard to differentiate lowercase l from 1.

```scala
val longValue = 5432L  // Do this

val longValue = 5432l  // Do NOT do this
```

### Documentation Style
Use Java docs style instead of Scala docs style.

```scala
// Correct:
/**
 * This is correct JavaDoc comment. And
 * this is my second line, and if I keep typing, this would be
 * my third line.
 */
 
 Wrong:
 /** We don't use one liner comments like this one. */
 
 Wrong:
 /** In MUSIT, we don't use the ScalaDoc style so this
  * is not correct.
  */
```

### Imports

* Avoid using wildcard imports, unless you are importing more than 4 entities, or implicit methods. Wildcard imports make the code less robust to external changes.
* Always import packages using absolute paths (e.g. scala.util.Random) instead of relative ones (e.g. util.Random).

### Pattern matching

* For method whose entire body is a pattern match expression, put the match on the same line as the method declaration if possible to reduce one level of indentation.

  ```scala
  def test(msg: Message): Unit = msg match {
    case ...
  }
  ```
  
* When calling a function with a closure (or partial function), if there are multiple cases, indent and wrap them.

  ```scala
  list.map {
    case a: Foo =>  ...
    case b: Bar =>  ...
  }
  ```
  
* Unless _all_ cases fit within the max line length, each case body should be on a new line. Each case should then be separated with a blank line.

  ```scala
  foobar match {
    case a: Foo =>					// new line here
      // The first case body that is
      // either very long or has
      // several statements
    									// blank line here
    case b: Bar =>
      // The second case body 
  }
  ```

### Naming

We try to follow the standard Scala/Java naming conventions.

* Classes, traits, objects and enums should follow the Java class naming convention.
  
  ```scala
  class FooBar
  
  trait Money
  
  object MyObject
  ```

* Packages should follow the standard Java naming conventions. All lowercase ASCII letters.

  ```scala
  package no.uio.musit.service
  ```

* Methods and functions should be named using **camelCase**.

* Annotations should follow the Scala convention. _NOT_ the Java convention.

* Variables should be defined in **camelCase** style, and should have meaningful names.

  ```scala
  val timeout = 10 seconds
  val currUsr = "Darth Vader"
  ```
  
* In local scopes it is OK to use short (and 1 character) long names.

  ```scala
  maybeString.foreach(s => println(s))
  
  maybeInt.map(n => n * 2)
  ```
  
* It is OK to use unamed arguments in local scopes.

  ```scala
  maybeInt.map(_ * 2)
  ```
  
### Line length

* Try to keep lines below 80 characters in length.
* Max limit of lines is set to 90 characters. And anything longer than that will only be acceptable on a case by case basis.

### Methods

* Methods should not be longer than 50 lines of code.
* A class should contain no more than 30 methods/functions.


    