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
# gosh darn lists
here Pythonistas
```rasfix
release mylist: Required[list] = ["poop", "buttcam"]
Console.PrintLine(mylist[1])
^^ slicing with [] also works like in python with one indexing!
^^ like this:
Console.PrintLine(mylist[2:]) ^^ everything AFTER item 2 INCLUDING item 2, and :2 for everything before two INCLUDING two ✅
```
and before you scream at me...
```rasfix
", ".join(mylist)
```
More to come in Changelog v1.2: **error handling (to all the pythonistas im sorry for adding rust style `match`), string handling, dict methods and bools (booleans)**
# Changelog v1.2 (Oct. 13 2026)
Example error:
```rasfix
rasfix error greet, line 4, column 1
   greet(name)
   ^^^^^^^^^^^
rferror, crashed at local timezone clock 5:00 PM UTC: undefined variable 'name'
@this error was caught at runtime
check for typos, function names, arguments, types (i.e. `release var: Annotation[type] = value` has wrong type as value compared to the type set in annotation)
```
# new

COLOR YOU OUTPUTS YOOOOHOOOO!!!!

color.red()
color.bold()
color.magenta()
color.blue()
color.bright_*()
color.yellow()
color.pink()
color.purple()
✅✅✅✅✅✅✅

# RUST STTLE ERROR HANDLIHG

```rasfix
fn your_unsafe_function()
    ^^ code...

match Result<Success, Fail()>
    Method[your_unsafe_function()]
    ^^ calls the func, can be multi
    case Success => Nothing() ^^ no-op
    case Fail(err) => {
        ^^ MULTILINE CODE HERE!!!
        Event.Raise(Crash, "it broke!!!")
        Console.PrintLine(err)
    }

^^ IMPORTANT NOTE: you DEFINE EVERY ACTION on EVERY CASE. the interpreter does NOTHING. not even a crash. you do it yourself.
^^ can be used in vars too in release using type `expect` (NEW)
```

to C devs, TypeScript devs, Python devs and Rust devs im sorry for making an abomination
actually. **GET F#CKED EVERYBODY THIS IS MY LANE!**

# Changelog v1.3 (Oct. 13 2026)
Yes, I said v1.2 would cover errors, strings, dicts and bools. Here is the rest, plus a few things that snuck in. 😤
# string method that i forgot in 1.2
```rasfix
release s: Required[string] = "  Hello, World  "
Console.PrintLine(s.trim().upper())
^^ methods chain, like JS
Console.PrintLine(s.contains("World"))
Console.PrintLine(s.replace("World", "rasfix"))
release parts: Required[list] = "a,b,c".split(",")
Console.PrintLine(", ".join(parts))
```
Other methods: `.lower()` `.len()` `.starts_with()` `.ends_with()` `.reverse()` `.repeat(n)` `.substr(start, len)`.
**ONE INDEXED!** index_of gives you 0 when it finds nothing, because -1 isn't a position in this house.
# dicts
release user: Required[dict] = {"name": "ras", "level": 5}
Console.PrintLine(user["name"])
user.insert("level", 6)
user.remove("name")
Console.PrintLine(user.keys())
Console.PrintLine("level" in user)
for k in user
    Console.PrintLine(k)
`.get(key)`, `.values()` and `.len()` also work. Missing keys give None, not a crash.
bools
`true`, `false`, `and`, `or`, `not`. Typing is strict now:
```rasfix
Console.PrintLine(5 == "5") ^^ false. no, python, we do not coerce.
```
try / catch / finally
```rasfix
try
    throw "it broke!!!"
catch err
    Console.PrintLine("caught: " :: err)
finally
    Console.PrintLine("always runs")
```
`throw` raises, `catch` names the error, and `finally` runs no matter what. Skip the catch and the error crashes with a full traceback, like the one in v1.2. **USE THIS ONLY IF YOU ARE A COWARD. IN MY LANG WE DONT COWARD**

# classes (finally)
```rasfix
class Animal
    fn init(self, name: Required[string])
        self.name = name
    fn speak(self)
        Console.PrintLine(self.name :: " makes a sound")

class Dog -> Animal
    fn speak(self)
        Console.PrintLine(self.name :: " says woof")

release d: Required[any] = Dog("Rex")
d.speak()
```
Classes can't be annotations, so use `any`. -> Parent is inheritance. A leading _ on a method makes it private. Define `__str__` and `Console.PrintLine(obj)` uses it.
stuff that snuck in
`unchanging x: Required[int] = 5` freezes the name, the type and the value.
yield is like return, but it keeps going and hands back a list of everything yielded.
`Accessing[T]` borrows for real: `release r: Accessing[int] = &x`. Reassign `x` and `r` is dead, and rasfix refuses to run if you use it. (Yes, the rustaceans got their wish after all 😭)
`json.parse()`, `json.stringify()`, `http.get(url)`, `import "file.rfx" as lib, sleep(n)`.
`rasfix --pack script.rfx -o app` makes a standalone binary.
More to come in Changelog v1.4: async/await, the rillow image module, --compile to ARM, and whatever else I break ✅✅✅✅✅✅✅✅✅✅✅✅✅✅✅✅✅✅✅
