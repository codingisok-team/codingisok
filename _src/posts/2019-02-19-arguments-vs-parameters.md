    Title: Arguments vs. Parameters
    Date: 2019-02-19T13:30:24
    Tags: terminology

The words "argument" and "parameter" are thrown around a lot in similar contexts, but what's the difference? 

Spoiler alert: parameters are used in reference to arguments.

<!-- more -->

## What's an argument?
An argument is any of the information (e.g. objects, values, pointers, etc.) passed to a function or method.[^1]

[^1]: We wrote a post about the difference between functions and methods [here](https://codingisok.com/2019/02/functions-vs-methods.html)!

## So what's a parameter then?
A parameter is the reference to the arguments within a function or method. You can think of parameters as placeholder names for currently unknown values.

## Case Study: Adding integers

Let's pretend you want to add integers together. Lots of integers. For some reason, you decide you want a class to help you do that, so you write the following in java:[^2]

[^2]: If you need an explanation for why you decided to use static methods, we wrote a post all about the `static` keyword [here](https://codingisok.com/2019/03/the-static-keyword-in-java.html)!

```
import java.util.List;

public class IntAdder {

	public static int add(int a, int b){
		return a+b;
	}

	public static int add(List<Integer> nums){
		int sum = 0;

		for (int i : nums){
			sum += i;
		}

		return sum;
	}

}
```

Note that you, a clever person, have used method overloading here to allow yourself to add together either two ints or a `List` of `Integers` (a.k.a. any number of integers).

### The Parameters
Since we don't have any method calls yet, let's start with the parameters for the two methods.

The first method takes in two `int`s, adds them together, and returns the result. Here, `a` and `b` are our parameters, as they are the in-method names we have given our `int`s.

The second method takes in a `List` (e.g. an `ArrayList`, a `LinkedList`, etc.) of `int`s, adds all the values together, and returns the result. For this method, we only have one parameter, our `List` `nums,` which we reference when getting the size of the `List` and each individual `int` within the `List`.

### The Arguments

O.K., time to actually use these methods! Quick — what's 12,345 + 67,890? ¯\\_(ツ)_/¯ Good thing you just wrote a method to figure it out! You make a call to `add()`, using the arguments `12345` and `67890`:

```
add(12345, 67890);
```

Since you used two `int`s as arguments, your `IntAdder` class knows to use your first method (the one with `int` parameters `a` and `b`), which returns the answer 80,235.

More practically though, let's pretend you're a teacher who wants to find a student's grade average. You already have an `ArrayList` of the student's grades called `studentGrades` (don't ask me why). As you may recall, an average of n numbers is the sum of the numbers divided by n. Since you have your `add()` method, you can make a call to it to find the sum of the student's grades:

```
add(studentGrades);
```

Since you used one `ArrayList` as your argument, your `IntAdder` class knows to use your second method (the one with `List` parameter `nums`), which returns the sum of all the grades. Now all you need to do is divide that sum by the total number of grades that was in the `ArrayList` and you'll have your average!

---