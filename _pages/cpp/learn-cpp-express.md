---
title: "Learn C++ express"
layout: page
date: 2021/01/15
---

How I would have liked to learn C++ (as a confirmed programmer)
===============================================================

Aka. "Ranting tutorial". But concise, don't worry.

Last version: 2021/01/15 12:12.


I tried to provide below a very efficient summary of the basics needed to write
C++ code and make it run as quickly as possible, assuming you know another
language (Python for myself). Curious and judgemental people can go to the
["Why and Whataboutit" part](#why-and-whataboutit) at the end to get some extra
ranting and explanation.

**Disclaimer**: I am **NOT** an experienced C++ programmer. I am just someone
with some programming/scripting who wanted learn it quickly, so I started to
write this "manual" *while* learning, that way not forgetting the important
hurdles faced as a beginner.

# Comment syntax

    // This line is commented up to the end of the line.
    /* This comment can span multiple lines and
       will be ended by the following symbol: */

Note that the choice of one-line/multi-line syntax is purely conventional.

# Basic program structure, aka Hello World

Open a new **text** file using a **text editor** such as notepad, gedit, vim,
emacs, sublime, atom...

```cpp
// program "helloworld.cpp"

#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl ;
    return(0);
    }
```

**NOTE**: the `main` function behaves differently to other functions:
1. it cannot be called from within the program;
2. if the return statement is omitted, it assumes `return(0);`.

## Statement syntax

In C++, a statement can span multiple lines, and ends with a *semicolon* `;`.
Indentation is not mandatory.
*Scopes* (group of statements sharing a same namespace) are enclosed in curly
braces `{}`. Variables defined inside curly braces are not available outside
this scope (e.g. variables from an `if`/`for`/`while` condition and block).

TODO: explanations:
- include,
- main function with return statement
- std::cout and the `<<` operator.
- intermediate values get destroyed if not stored (beware when using objects
    that reference to those, they become meaningless at the next statement)

Current questions:
- Why `<iostream>` and not `<iostream.h>`?

## Shortening some syntax:

```cpp
using namespace std;

cout << "Hello, World" << endl
```

`using` can be used to "alias" any name that we find too long to write:

e.g. if we have to repeatedly use the type `std::vector<int>::iterator`:

```cpp
using iterator = std::vector<int>::iterator;
```

# Compiling

C++ source files consist of `.cpp`/`.cc` files (same meaning), and sometimes
so-called "header" files (`.h`), which get included in the `.cpp` source code.

I don't know what object files `.o` really are, but there are obtained by
compilation. The final executable is obtained by *linking* libraries.

In short, this will make a functional program:

    g++ helloworld.cpp -o helloworld

Current questions:
- does the order of arguments matter?
- what are some useful options? `-Wall` for warnings, `-g` unknown.


# Literals

```cpp
255; 0377; 0xff;      // integers (decimal, octal, hexadecimal notations)
'a'; '\141'; '\x61';  // single characters (literal, octal, hex)
"string";              // array of characters
true; false;           // boolean values
```

# Defining variables

C++ is a *typed* language. The variables must be declared with a type (int,
float, string, etc) which cannot be changed.

```cpp
int a; // the integer variable named 'a' is defined, but not assigned any value.

int b = 42; // integer variable named 'b' is declared and assigned value 42.
```

## Valid variable names

ascii letters, numbers, underscore. Must not start with a number. No reserved
words (e.g. `while`, `if`, `for`, `break`, `continue`, etc). Conventionally,
constant variables are written in capitals.

## Other types of variables

```cpp
bool b = true;
bool b2 = false;
short i = 16;  // 16 bytes (up to 65535). usually.
long k = 123456789123L; // 32 bytes.
float x = 0.5;
float y = 1e-6;
double z = 123456789.123456789;
char c = 'a'; // single-character: must be single-quoted.
```

Variables with a specific memory size must be told so:

```cpp
char c[3] = "abc"; // chain of characters of length 3. must be double-quoted.
```

Note that you can use mutable strings from the library:

```cpp
#include <string>

string s = "the length I want, and it can be changed later";
```

### More on the types

```cpp
signed int a;  // 0
unsigned int b;
unsigned char c;
```

```cpp
static int a;   // global lifetime even if local scope
const int a=42; // must be assigned, because can't be changed.
//external int a;
```

# Operators

## Integer operations

`x/y` will be a float division if any of `x` or `y` is a float. You can also
assign integers to a double typed variable.

# Reading from a file

The `iostream` library contains `cout`, `cerr`, `cin`, `clog` for the
standard input/output streams.

## We usually want to read a file line by line:

```cpp
// readlines.cpp
// read lines into a string variable
#include <iostream>
#include <string>

using namespace std;

int main() {
    string line;
    while (getline(cin, line)) {
        cout << "10 first characters of the line: " << line[0:10] << endl;
    }
    return (0);
}
```

A single character can be retrieved with `c = cin.get();`.

## Word by word ("default" method)

_Added 2021/02/02 22:05 based on jesyspa/linear-cpp_

Note that you can use `cin >> x` to read *words* into the (predefined) variable
`x`. This stops reading input when encountering space characters, so *do not*
mix this method of input with the `getline` method if you don't know what you
are doing.

Checking valid input, for example if `x` is defined as `int` but the user did
not enter a valid integer: `cin` evaluates has a bool depending on the success,
so can be checked using e.g. `if(!cin)`. Even better, `if(!(cin >> x))` will
simultaneously assign and check.

If `cin` is in fail state (for instance, it could not convert input to the
desired type), reset it with `cin.clear()`.

`cin.eof()` returns true if we reached end of stream.

# Arrays, associative arrays, complex structured objects

Array indexing starts at zero.

```cpp
int myarray_of_int[3] = {5, 6, 7};

cout << "first element of my array: " << myarray_of_int[0] << endl;
```

Arrays of chains of characters are trickier, because in reality they are arrays
of arrays:

```cpp
char myarray_of_strings[3][10] = {"orange", "apple", "banana"};
```

However, this will usually be simpler to use the string class to hold the chain
of characters, and the vector class to hold the list of strings (both have
mutable lengths and content):

```cpp
include <string>
include <vector>

vector<string> myvector_of_strings[3] = {"orange", "apple", "banana"};
```

## Structured objects

See it as defining a new *type*.

```cpp
struct miscellaneous {
    int a;
    char thing[50];
}; // this defined a new object type "miscellaneous", holding attributes "a" and "thing"

miscellaneous my_bag_of_stuff; // this creates an instance of type miscellaneous

my_bag_of_stuff.a = 25;
my_bag_of_stuff.thing = "dead frog";
```

NOTE: I don't know if the semi-colon after the struct definition is mandatory.

## Map (associative array)

```cpp
#include <map>

map<string, int> a; // map from string to int.
```

## Vector, deque and more.


```cpp
vector<int> myvec(10);  // myvec[0] to myvec[9] are int (default size is zero).
myvec.size();
myvec.empty();
myvec.push_back(3);
myvec.pop_back();
```

Indexing:

```cpp
myvec[0]; // first element.
myvec[i]; // where `i` variable must be of type std::size_t (as returned by myvec.size())
```

Example iteration:

```cpp
std::site_t i = 0;
while (i < myvec.size()) {
    cout << myvec[i];
    ++i;
}
```


# Reading command-line arguments

This is done by providing 2 special arguments to main:

- `argc`, the expected number of arguments;
- `argv`, the array of arguments, where `argv[0]` contains the name of the
    program.

```cpp
// readcommandline.cpp

#include <iostream>

int main(int argc, char **argv) {
    for (int i=1; i<argc; i++) {
        cout << "Argument #" << i << ": " << argv[i] << endl;
    }
    return(0);
}
```

# Opening files for reading/writing

```cpp
#include <fstream>

ifstream mystream;
mystream.open("somefilename.txt", ios::in);
// read lines, do stuff
mystream.close();
```

If just for input, use `#include <ifstream>`, and for output, `#include <ofstream>`.

# Memory pointers, aka references

"lvalue reference"

```cpp
// notation
&x  // address of x
*p  // content of address p (*&x is x)

p->m // member m of struct/class pointed by p (equals x.m if p is a pointer to x)

delete p ;  // destroy and free object at address p
delete p[]; // destroy and free array of objects at p

// define reference objects:
int& y = x; // Bind y to the same content than integer x

// when using auto on references, it must be explicit:
auto& y = x;
```

# Defining functions

`return_type function_name(param1_type param1, param2_type param2) {return ...}`

Defining with *referenced* arguments (notice the `&`):

`return_type function_name(param1_type& param1, param2_type& param2) {return ...}`

## Prototyping

*Declaration* ≠ *Definition*:

you can declare a function without defining the body immediately, e.g.:

```cpp
int square(int x);
```

Once declared, a function can be called in subsequently defined functions, 
and also you can assign their return value using the `auto` type:
`auto sx = square(x);`

## Inline functions

Can be defined in multiple places (as opposed to regular ones)

```cpp
inline int square(int x) {
    return x*x;
}
```

## Declaration inside another function

Declaring inside another function is possible, but not defining.


# Loops and control structures (while, for, if, etc)

```cpp
if (condition) {
    // code if condition is true;
} else if (condition2) {
    // code if condition2 is true;
} else {
    // the other cases;
}
```

If a single statement follows the `if` or `else` checks, the curly braces can
be omitted!

```cpp
while (condition) { ... }

for (int i=0; i<10; ++i) { ... }

// range-based for loop (for each) in C++11:
for (int elem : myvec) { ... }
```

- `for (;;)` is equivalent to `while (true)`
- you cannot modify vector elements while looping over it.
- it is a good habit to increment using `++i` rather than `i++` that evaluates
    to the old value (as per jesyspa/linear-cpp).

- `break` and `continue`

# Header files

Files in `.hpp` or `.h`. They aren't given to the compiler, but *included* in
source files (`.cpp`). They are like libraries to store common functions. They
should contain function *declarations*, which can be *defined* in an eponymous
`.cpp` file.

This requires a *preprocessor* statement:

```cpp
#include "mydeclarations.hpp"
```

It is equivalent to pasting the entire file content here, so there is no
"module scope" as in python. This means that included files within
`mydefinitions.hpp` don't need to be included again (but don't rely on it).

# Macros

```cpp
#define MACRO_NAME arbitrary text
```

This preprocessor statement will replace all `MACRO_NAME` (usually uppercase)
by the arbitrary text. Do not name macros with double-underscores (special).

```cpp
#ifndef MACRO_NAME
code...
#endif
```

Insert the code if the macro has not been defined.

For header files, this allow to check that they are included only once:

```cpp
// Entire content of "mydefinitions.hpp" file.

// Check that it has not been already included
#ifndef MYDEFINITIONS_HPP
// Then mark it as included
#define MYDEFINITIONS_HPP

/* function declarations, inline functions ... */

#endif
```

Modern compilers support `#pragma once` to do this.


# Iterators

(Python users: it's not what you think).

An iterator is a *reference* to a *scalar* element inside a vector.

With a vector "myvec":

- `myvec.end()` returns an iterator that is after the last element (undefined).
- `myvec.begin()` returns an iterator with the first vector element, or the
    same value as `myvec.end()` if the vector is empty.
- `myvec.begin() + 2` refers to the third position.

To obtain the element referenced by the iterator, use *dereferencing*:

```cpp
auto it = myvec.begin();
it += 2;
int elem = *it; // same as: int elem = myvec[2]; (don't do this with myvec.end())
```


# For more, see this excellent cheat sheet

<https://web.pa.msu.edu/people/duxbury/courses/phy480/Cpp_refcard.pdf>

# Why and Whataboutit? {#why-and-whataboutit}

After years of Python or R, some gluing with Bash, some fiddling with
javascript, an abandoned experiment with Perl, I thought I could embrace
quickly the basics of C++, provided some good document targeted to people like
me, that is not completely newbie to programming (Okay, some people will laugh
about me calling Python a programming language, but I will ignore them for now[^1]).

However, such *good* document doesn't exist. Or at least, it takes so much time
to find on the internet that it is faster to read a verbose 300-pages crappy
book about C++ that tells you the story of Assembler and the different kinds of
comment boxes before actually showing you how to define a variable. Even Reddit
has been falling short on the matter.

For the record, here are the books I tried.

- Practical C++ Programming, Steve Oualline (O'reilly)
- Programming Principles and Practice Using C++, Bjarne Stroustrup (all due
    respect to the inventor of C++, but I didn't even start his book, because
    it's just too long)

Then I tried to restrict the "Google" (I mean Duckduckgo) search with keywords
like "for programmers", "quick", and so on, and what I found was deeply
deceiving:

- Accelerated C++, Andrew Koenig and Barbara E Moo
- Sams Teach yourself C++ in 21 days, J. Liberty and B. Jones
- [You can program in C++ -- A programmer introduction, Francis Glassborow](http://www.library.uc.edu.kh/userfiles/pdf/1.You%20Can%20Program%20in%20C++%20%20A%20Programmer's%20Introduction.pdf)

One of the problems is, many C++ books aimed at programmers are aimed at C
programmers. Yeah, thanks for the others.

But how is it possible that 1) the books are still so long? 2) I can't even
find the way to compile my program in those books? COME ON.
Because yes, some of these book don't contain a clue about how to compile, or
alternatively they show you 10 screenshots of a Windows IDE. What about a
simple command line tool called g++ that can actually be run everywhere??? I'm
even quoting some intersideral sentence from _Sams Teach Yourself C++ in 21
days_ stating "the vast majority of programmers are working in the Windows environment,
and the vast majority of professional programmers use the Microsoft compilers".
Wat? Is this the world we live in? I dare to refuse and live in denial.
However, linux tools are not indemn of critics either. I happen to know (from
reading Makefiles here and there), that `g++` is used to compile a program.
However `man g++` is the most atrocious and utterly useless piece of
documentation ever. There are thousand options with cryptic names,
unexplained, not sorted by usefulness, and no _standard example_ for a basic
use. Like, I saw the use of `-Wall` option, and grasped that this shows
warnings, lucky me. However I also saw the `-g` option many times, but could
not find what it does. And the most insane part is finding out that you must
give the `-o` option to control the name of the output file. Let me just tell
you that I wasn't able to find this information in the manual.


So I have an opinion on the matter. This is probably naive, inexperimented, and
plainly over-confident. In short, there is a need for an efficient resource.
This resource should provide a quick overview of the syntax, commands to
compile code, and efficiently target important topics that get you up and
running, such as importing libraries, creating arrays or opening files.

I acknowledge that C++ is an extremely rich language that takes years to learn.
However I still believe it is possible to give the essentials in a matter of
minutes/hours, so that a programmer can produce something beyond just Hello Word.
Programming comes with practice. If there is no synthetic way to get started
writing rudimental programs and get your hands dirty, how are you going to
learn anyway? 300 pages won't be of any use. Lengthy introductions about
code-style and code commenting won't be of any use if there are ZERO line to
style or comment. The people who wrote such books are very clever and talented,
no doubt. I am not, however I just started C++, so I still remember what it
feels, and what are the first hurdles. (Note to self: I have yet to try this
promising ["linear Cpp" on github](https://github.com/jesyspa/linear-cpp)).

The fact is, it's not my first attempt at C++. I tried to convert another
simple python program in C++ 1-2 years ago, but things didn't work properly and
information was hard to find, so I didn't spend more than a few days on it.

Now this time, here is how I did it in practice. I took a simple python program, one that reads
a text file, check if some string is present, replace it. Then I skimmed
through "Accelerated C++", up to the Hello World program to get the structure.
I also found some C++ cheat sheets on internet, for a visual and compact
summary, which worked as an index for me.
Then I opened a text file to write my program. And I ~~googled~~
duckduckgo-ed every single thing I needed to know to make it work:

- How to compile a C++ program in Linux?
- Can't I just write `cout` instead of `std::cout`?
- How to define a variable?
- How to define an array of strings?
- How to read a file line by line?
- How to pass arguments to a script? (This one is buried deep in some books).
- How to define a function?
- ...

In the end, thanks to the power of internet (stackoverflow), it is quite an
efficient process. But it will never be as efficient as having a concise
resource somewhere, and I think it is detrimental to the community to lack it.
But again, I barely started, was quite frustrated, and only supercially
searched (but that was too time consuming already). Also, my problems and
questions are obviously biased, as coming from a Python writer working in
bioinformatics (where parsing files is 70% of the job -- cynical take).

[^1]: Anyway, `sed` is the best programming language ever, end of the debate.

