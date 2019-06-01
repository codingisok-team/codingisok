    Title: Values, Pointers, and References
    Date: 2019-04-08T20:04:40
    Tags: terminology

value != pointer != reference. But also sometimes value != pointer but pointer == reference. But HOW???

Programming makes my head hurt...

<!-- more -->

# What's a value?
A value is data that is created, passed around, and stored in a program.

# What about a pointer?
Pointers represent addresses to where values are stored in memory. Under the hood, they are simply a number. When a program starts, the operating system assigns it a block of unique and sequential memory addresses. A value is stored at one or more memory addresses, while a pointer represents a particular memory address.

## Mini c Example: Values and Pointers in Action

```c
int value = 5;
int* pointer = &value;
```

When started, the above code produces something like this in memory:

	MEMORY  →  STORED
	ADDRESS    VALUE
        	  ————————
	0x1000   |   5    | ← value
         	  ————————
	0x4000   | 0x1000 | ← pointer
          	  ————————

In this case, `5` is stored at memory address `0x1000` and can be accessed using the variable name `value`. `pointer` is equal to `0x1000` (a.k.a. `value`'s memory address). Though irrelevant to this example, it is worth noting that `pointer` is stored at memory address `0x4000`.

	EXPRESSION  →   VALUE
	 value      →   5
	&value      →   0x1000
	 pointer    →   0x1000
	*pointer    →   5
	&pointer    →   0x4000

# O.K., how about a reference?
References are a purely language-level concept – behind the scenes they are transformed into a combination of values and pointers. In other words, they're an abstraction which are _supposed_ to make programming easier, but often end up making it more confusing if you think too much about them (see: us while writing this post).

Concretely, a reference is an alias for a value stored somewhere else in the program. You can think of a reference as a short-hand way of accessing a value, to pass it around or do something with it.

## Mini Python Example: Values and References in Action
```python
class Snake(object):
  def __init__(self, name):
    self._name = name

s = Snake("albert")
```
Here, `s` is a reference to a _specific_ instance of the `Snake` class (his name is albert). Note that in python, references can be modified to refer to other values (not true for all languages, like racket or Rust). However, if you did something like `s = Snake("NOT albert")`, you would no longer have a way of referring to or even accessing the original albert. You could try making him again, but the new `Snake` wouldn't be him. It would be a doppelgänger. A false twin. Our dear albert would be lost to the aether.

Different languages deal with reference-less values in different ways. Java and python will eventually "garbage collect" (overwrite) the memory storing the disused value; c and c++ on the other hand, will leave the value as-is, causing mayhem and distress for uninitiated plebes (a.k.a. us ♪~ ᕕ(ᐛ)ᕗ).

# Language Breakdown

## In `c++`
`c++` provides explicit syntax for each of these three concepts, but does so in a nuanced and subtle way. References and pointers both refer to values which live _somewhere_ in memory. Essentially, references are pointers that can't be `nullptr`. They were added to `c++` after it was initially created, and are usually preferred over pointers (pointers still exist in `c++` because they were inherited from `c`. Remember: `c++` is "`c` with classes").

```c++
struct IntContainer {
  int data;
};

void example() {
  IntContainer value;
  value.data = 10; // Use '.' to access things in values

  IntContainer* pointer = &value;
  pointer->data = 15; // Use '->' to dereference pointers
  assert(*pointer == value); // Dereference a pointer itself with '*'

  IntContainer& reference = value; // 'reference' is an alias of 'value'
  reference.data = 30; // Use '.' to access things in references
  assert(reference == value);
}
```

## In `c`
`c` supports values and pointers and that's it. It's old and clunky but it lets you do whatever you want. Even if that means dereferencing null pointers... or overwriting your entire operating system. Fun fact: when `c` tries to dereference a null pointer, the _operating system_ stops it with a "SEGFAULT". Because `c` knows no limits. Live your best life, `c`.

```c
// c isn't advanced enough to do `struct <name> { <stuff> };
// instead we have to do `typedef <entire struct definition> <name>`;
typedef struct {
  int data;
} IntContainer;


void example() {
  IntContainer value;
  value.data = 10; // Use '.' to access things in values

  IntContainer* pointer = &value;
  pointer->data = 15; // Use '->' to dereference pointers
  assert(*pointer == value); // Dereference a pointer itself with '*'
}
```

## In `java`
`Java` supports values and references. Here's the catch: the values are stored on the heap, so you only interact with them via `new` (to create them) and references (to use them). There are pointers too, but they're buried deep under the surface and essentially let the programmer ignore the fact that they exist. The practical upshot is that interacting with values in `java` mostly boils down to using references. There is one catch: primitive values, like `int` and `float`, aren't stored on the heap. So when we name them (`int i = 0;`), `i` is a value, not a reference.

```java
// In "IntContainer.java"
public class IntContainer {
  public int data;
}

...

// In "Example.java"
public class Example{
  void example() {
    // Primitives are the only values we interact with directly 
    int value = 10;

    // Refer to an IntContainer value
    IntContainer reference = new IntContainer(); // `new` tells Java to create a value on the heap
    reference.data = 10; // Use '.' to access things in values

    IntContainer otherReference = reference;
    otherReference.data = 15; 

    // Since reference and otherReference refer to the same IntContainer value,
    // changing one is the same as changing the other.
    assert(reference.data == otherReference.data);
  }
}
```

## In `python`
Python, like java, supports values and references. However, memory in Python is pretty confusing. That's because python is an interpreted language, so the `python` program is reading through your code and doing stuff with its own memory accordingly, including creating objects in its own memory space. So let's forget about that for now and focus on the language itself. When writing code in python, everything is an object (AKA a value), and the names of objects are references to the value. Python doesn't even have primitive values like java – e.g. `int`, `float`, etc. like `1` and `3.141` are objects too!

```python
class IntContainer(object):
  def __init__(self, data):
    self._data = data

def example():
  # No primitives! Everything is a reference to an object!
  not_a_value_actually_a_reference = 10

  # Refer to an IntContainer value
  reference = IntContainer(10)  # python doesn't need `new`

  other_reference = reference
  other_reference.data = 15

  # As with java, reference and other_reference refer to the same object,
  # changing one is the same as changing the other.
  assert(reference.data == other_reference.data)
```