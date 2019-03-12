    Title: the static keyword
    Date: 2019-03-05T13:21:49
    Tags: terminology, keywords, java

If you program in Java, then you are most likely familiar with `public static void main(String[] args)` (and if you're not, then are you sure you're programming in Java?), but what exactly does the `static` part mean?

<!-- more -->

## In Java

In Java, the `static` keyword allows the programmer to treat a method as if it's outside of its class. In other words, it allows you to call a class method using the class name instead of through an instance.

## Case Study: programming a calculator

Let's say you want to build your own, customized calculator. Yeah you could just use the one on your phone, but that would be too easy.

### Creating a `static`-y calculator

Fun fact: for the most basic possible calculator, you won't ever need to create an instance of your class. Why? Because the calculator doesn't need to save anything, just perform operations and return results.

```
public class StaticCalculator {

	public static double add(double a, double b){
		return a+b;
	}

	public static double subtract(double a, double b){
		return a-b;
	}

	public static double multiply(double a, double b){
		return a*b;
	}

	public static double divide(double a, double b){
		return a/b;
	}
	
}

```

To use this calculator, all you would need to do is call your desired method by referencing the class name, like so:

```
public class StaticCalculatorExample {

	public static void main(String[] args) {
		// You don't need an instance.
		System.out.println(StaticCalculator.add(1.0, 2.0)); // 3.0

		System.out.println(StaticCalculator.subtract(1.0, 2.0)); // -1.0

		System.out.println(StaticCalculator.multiply(1.0, 2.0)); // 2.0

		System.out.println(StaticCalculator.divide(1.0, 2.0)); // 0.5
	}
	
}
```

Note that if you want to perform multiple operations in a row with this calculator, you would need to pass your previous result in to your next call as an argument.

### Creating a calculator with less `static`

O.K., that other calculator was fine and dandy and simple and whatever, but what if you want to be able to perform operations on your last result? Here's where instances and non-`static` methods and come in.

```
public class InstanceCalculator {

	private double res = 0.0;

	public double add(double a, double b){
		res = a+b;
		return res;
	}

	public double add(double a){
		res = res+a;
		return res;
	}

	public double subtract(double a, double b){
		res = a-b;
		return res;
	}

	public double subtract(double a){
		res = res-a;
		return res;
	}

	public double multiply(double a, double b){
		res = a*b;
		return res;
	}

	public double multiply(double a){
		res = res*a;
		return res;
	}

	public double divide(double a, double b){
		res = a/b;
		return res;
	}

	public double divide(double a){
		res = res/a;
		return res;
	}

}
```

To use this calculator, you need to create an instance of the calculator and perform your operations through that instance, like so:

```
public class InstanceCalculatorExample {
	
	public static void main(String[] args) {
		// You need an instance.
		InstanceCalculator calculator = new InstanceCalculator();

		System.out.println(calculator.add(1.0, 2.0)); // 3.0
		System.out.println(calculator.add(2.0)); // 5.0

		System.out.println(calculator.subtract(1.0, 2.0)); // -1.0
		System.out.println(calculator.subtract(2.0)); // -3.0

		System.out.println(calculator.multiply(1.0, 2.0)); // 2.0
		System.out.println(calculator.multiply(2.0)); // 4.0

		System.out.println(calculator.divide(1.0, 2.0)); // 0.5
		System.out.println(calculator.divide(2.0)); // 0.25
	}

}
```

Note that 

1. this calculator class has twice as many methods (i.e. two methods per arithmetical option) so as to allow the user to perform either operations on the last result or entirely new operations, and

2. because the previous result is saved, none of the calculator methods can be static any more. After all, it wouldn't make sense for you to have a calculator, and for a friend to use static methods from your class to add two numbers together and thereby change the value of your `res` variable.


## To sum up:

In java, `static` is useful when nothing needs to be saved, nothing related to the class changes, and you don't need an instance of your class.

