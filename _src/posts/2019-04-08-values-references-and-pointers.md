    Title: Values, References, and Pointers
    Date: 2019-04-08T20:04:40
    Tags: DRAFT, terminology

TODO

<!-- more -->

# What's a value?
A value is data that is created, passed around, and stored in a program.

# How about a reference?
A reference is an alias for a value stored somewhere else in the program. You can think of a reference as a short-hand way of accessing a value and passing it around or doing something with it.

## Mini Python Example: Values and References in Action
```python
s = "snek"
```
Here, `s` is a reference to the value `"snek"`. Since `s` is a reference, it can be re-assigned to another value; in that case, you would no longer have a way of referring to or even accessing `"snek"`. Different languages deal with reference-less values in different ways. Java and python will eventually "garbage collect" (overwrite) the memory storing the disused value; c and c++ on the other hand, will leave the value as-is, causing mayhem and distress for uninitiated plebes (a.k.a. us ♪~ ᕕ(ᐛ)ᕗ).

# O.K., but what about a pointer?
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

# The Dictionary: A Metaphor
Metaphorically speaking, the dictionary is a good illustration of values, references, and pointers in action. When two people are conversing, they don't have to define every word they use; it is assumed that both people know what all the words mean, and these words act as references to their own definitions. However, if person A uses a word that person B doesn't know, then person B can use the word's spelling as a pointer to find its definition (i.e. its value) in the dictionary.

> "metaphor" (reference)
>
>> ↓ spelling
>
> M-E-T-A-P-H-O-R (pointer)
>
>> ↓ definition
>
> the dictionary: "a figure of speech in which a word or phrase literally denoting one kind of object or idea is used in place of another to suggest a likeness or analogy between them"[^1] (value)

[^1]: [As defined by Merriam-Webster](https://www.merriam-webster.com/dictionary/metaphor)

Alternately, person B can skip the pointer altogether, and ask the person A directly (i.e. dereference) for a definition.

> "metaphor" (reference)
>
>> ↓ person B: "what does that mean?"
>
> person A: "a figure of speech to allude to or explain another concept" (value)

Note: this metaphor isn't perfect, as there can be multiple definitions for any given word, whereas in code there is a single value per reference.

# Case Study: Screen Updater
You are getting swindled by your internet provider, and you're sick of not being able to smoothly stream videos. You've had enough! Instead of verbally wrestling with the internet company again, you decide to take matters into your own hands and write a screen updater that will buffer the currently-streaming video and play it at a steady 24 frames per second (fps). You've already finished most of the program; all you need to do now is create the screen updater that will load and then display the next frame of the video.

```c++
```

# Wrapup: Language Breakdown
## In c++

## In c

## In Java
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

## In Python
In Python, all names are references.

```python

```
