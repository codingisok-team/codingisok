    Title: Values, References, and Pointers
    Date: 2019-04-08T20:04:40
    Tags: DRAFT, terminology

_Replace this with your post text. Add one or more comma-separated
Tags above. The special tag `DRAFT` will prevent the post from being
published._

<!-- more -->

# What's a value?
A value is data that is created, passed around, and stored in a program.

# How about a reference?
A reference is an alias for a value stored somewhere else in the program. You can think of a reference as a short-hand way of accessing a value and passing it around or doing something with it.

## Mini Python Example
```python
s = "snek"
```
Here, `s` is a reference to the value `"snek"`. Since `s` is a reference, it can be re-assigned to another value; in that case, you would no longer have a way of referring to or even accessing `"snek"`. Different languages deal with reference-less values in different ways. Java and python will eventually "garbage collect" (overwrite) the memory storing the disused value; c and c++ on the other hand, will leave the value as-is, causing mayhem and distress for uninitiated plebes (a.k.a. us ♪~ ᕕ(ᐛ)ᕗ). 

# O.K., but what about a pointer?
Pointers represent addresses to where values are stored in memory. Under the hood, they are simply a number. When a program starts, the operating system assigns it a block of unique and sequential memory addresses. A value is stored at one or more memory addresses, while a pointer represents a particular memory address.

## Mini c Example

```c
int value = 5;
int* pointer = &value;
```

	ADDRESS →  VALUE
        	  ————————
	0x1230   |   5    | ← value
         	  ————————
	0x1234   | 0x1230 | ← pointer
          	  ————————

In this case, `5` is stored at memory address `0x1230` and can be accessed using the variable name `value`. `pointer` is equal to `0x1230` (a.k.a. `value`'s memory address). Though irrelevant to this example, it is worth noting that `pointer` is stored at memory address `0x1234`.

	EXPRESSION  →   VALUE
	 value      →   5
	&value      →   0x1230
	 pointer    →   0x1230
	*pointer    →   5
	&pointer    →   0x1234

# Case Study: Screen Updater


# In Java
Java lets the programmer ignore the fact that pointers exist; therefore, the programmer interacts with values and references in the same way.

```java
void something() {
  A value = new A();
  value.b = "butt";
}

void something(A reference) {
  reference.b = "butt;
}
```

# In Python
In Python, all names are references.

```python

```