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
    counter = counter + 1
```

here is a simple guessing game:
```rasfix
release n: Required[int] = random.randint(1, 100)
release input: Required[string] = ""
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
    
