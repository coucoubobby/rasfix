# What is rasfix

rasfix is a one indexed programming language (like Lua) that's a bit Python, Rust, TypeScript and C mashed together.
its indentation based and tree walks with caching.
its a single file portable, with working stuff. (SOME FEATURES EXPERIMENTAL).

# Language example

it's kind of reminiscent of JavaScript when you think about it, though its only some parts.
`Console.PrintLine("Hello, World!")`
`Console.Print("This doesn't have a newline natively.\n")`
`Console.ReadLine("Prompt here: ")`
those are I/O. Variables:
`var = "value"`
OR, more "rasfically":
`release var: Required[string] = "value"`
this is much more preferred than a pythonic `var = "value"` assignment.
How annotations work: Required[T] means that the variable can be T but **NEVER** None. Optional[T] means variable can be T **OR** None. Accessing[T] is a borrow
(I'm sorry, rustaceans... But it's gotta happen.)
`NOTE: rasfix ignores redeclares using 'release'—in this case (loops, iterations), it is required to use the pythonic assignment as reassignment.`

```rasfix
release counter: Required[int] = 5
while counter > 0
    Console.PrintLine(counter)
    ^^ release counter: Required[int] = counter - 1
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    ^^ thats bad! also this is comment
    ^^ better:
    counter = counter - 1
```

here is a simple guessing game:
```rasfix
release n: Required[int] = random.randint(1, 100)
release input: Required[int] = 0
release tries: Required[int] = 5

while true and tries > 0
    input = Console.ReadLine("Guess the number (" :: tries :: " tries left): ") ^^ the double colon (::) is concatenation.
    so input > n
        Console.PrintLine("too high!")
        tries = tries - 1
    so input < n
        Console.PrintLine("too low!")
        tries = tries - 1
    so input == n
        Console.PrintLine("You found the number in " :: tries :: " tries.")
        break
```

this is a minimal example, showing that rasfix has basic capabilities.
types: list, dict, int, bool, string, any
(CLASSES NOT FOR ANNOTATIONS, USE `any` INSTEAD)

# Changelog v1.1 (Oct 13. 2026)

More examples, yuppie!

```rasfix
^^ conditionals
release MyValue: Required[int] = 10

so MyValue >= 10
    Console.PrintLine("big")
so MyValue <= 5
    Console.PrintLine("smol")
else
    Console.PrintLine("bro idfk")
```

# for loop
Structure:
```rasfix
for i = start, iterations[, per int]
    ^^ code here!
```
example:
```rasfix
for i = 1, 100, 5
    Console.PrintLine(i)
```
this prints 1, 5, 10, 15, etc until 100
OR:
```rasfix
for i = 1, 100
    Console.PrintLine(i)
```
this prints 1, 2, 3, 4, 5, 6, 7 etc until 100
memory efficiency: **4KB per function!**
before, when this README was first committed: ~20MB for 300 functions
after: **1.2MB for the exact same amount of functions**
speaking of functions...
# functions

structure:
```rasfix
fn name(param: Annotation[type])
    ^^ code
name(param)
```
simple greeter:
``` rasfix
fn greet(Name: Required[string])
    Console.PrintLine("Hello, " :: Name)

release name: Required[string] = Console.ReadLine("What's your name: ")
greet(name)
```
# dicts
here Pythonistas
```rasfix
release mydict: Required[dict] = ["poop", "buttcam"]
Console.PrintLine(mydict[1])
^^ slicing with [] also works like in python with one indexing!
^^ like this:
Console.PrintLine(mydict[:1]) ^^ everything AFTER item one, and 1: for everything before one ✅
```
More to come in Changelog v1.2: **error handling (to all the pythonistas im sorry for adding rust style `match`), string handling, dict methods and bools (booleans)**
