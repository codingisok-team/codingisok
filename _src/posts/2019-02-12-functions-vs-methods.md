    Title: Functions vs. Methods
    Date: 2019-02-12T18:02:04
    Tags: terminology, functions, methods

The words "function" and "method" are thrown around a lot and may seem interchangeable, but are they really?

Spoiler alert: Nope! There is actually a difference between the two.

<!-- more -->

## What even is a function?
A function in programming is a named collection of statements [^1] that, when performed in sequence, complete a defined task. Because functions are named, they can be called to perform the same task multiple times and so are good tools to use for making code shorter and easier to read.

[^1]: Statement: a line that carries out an action, like assigning a variable or returning a value

## O.K., but what about a method?
Easy: methods are simply functions that happen to belong to a class (i.e. methods are a subset of functions).

## Case Study: Creating a grade book
Using an example is probably the easiest way to demonstrate the difference between functions and methods, so let's pretend you're a teacher. As all teachers do, you need a way to keep track of your students' grades... A digital grade book sounds like it could be handy! Nothing too fancy, just something to keep track of individual students and their grade averages. Obviously we need a Student class, but where should the grade average be calculated?

### The "Functional" Way

If we want to use a function, then the grade average must be calculated outside of the Student class. That means our `calculate_average` function is a simple average calculator (i.e. sum of student's grades divided by the total number of grades) and needs outsider access to the student's information.

```
class FunctionalStudent(object):
  
  def __init__(self, name):
    self._name = name
    self._grades = []

  def get_grades(self):
    return self._grades
  
  def add_grade(self, new_grade):
    self._grades.append(new_grade)

def calculate_average(student): 
  grades = student.get_grades()
  return sum(grades) / len(grades)

```

Note that the student instance itself doesn't know its own average, and that the average is calculated fresh with each call to the `calculate_average` function.

### The "Methodic" Way

If, on the other hand, we want to use a method, then the grade average must by definition be calculated inside of the Student class. In this case, the student instance itself will keep track of its own average, which is both more efficient and more secure. 

```
class MethodicStudent(object):
  
  def __init__(self, name):
    self._name = name
    self._grades = []
    self._average = 0.0

  def get_average(self):
    return self._average

  def get_grades(self):
    return self._grades

  def add_grade(self, new_grade):
    recalculated_average = recalculate_average(new_grade)
    self._grades.append(new_grade)
    self._average = recalculated_average

  def recalculate_average(self, new_grade):
    return (self._average + new_grade/len(self._grades)) * len(self._grades)/(len(self._grades)+1)
```

Because no other function or object needs outside access to any of the student's information, this information is ultimately more secure. Additionally, since the student object saves its own grade average, a simple optimization makes recalculating a new average upon the addition of a new grade faster, as no work needs to be re-done.

## Conclusion

As exemplified, functions and methods can be used to complete the same task, but in rather different ways. Though using a method was more optimal in our grade book example, methods are not always preferable to functions for every problem.

### Re-interpreting function and method

Looking back at our example, we can now extend our previous definition to say that a function takes in *zero or more* inputs and produces exactly one output. A method, on the other hand, is a function that takes in *one or more* inputs where the first input is the instance of the class it belongs to. That's why methods in python always have the `self` argument first. In java, since only methods are used, the first argument is implied, but can be referenced with the `this` argument.

---