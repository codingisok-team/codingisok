    Title: Our Testing Philosophy
    Date: 2019-03-18T20:53:17
    Tags: general, testing

To test or not to test, that is the question (jkjk don't be a dumbus; TEST YOUR
CODE). The real question is, when and how to test?

Spoiler alert: we're not the biggest fans of test-driven development.

<!-- more -->

# A few common approaches to testing

1. No testing whatsoever, or: write the entire program and, when it fails, give
   up. We don't recommend this method.

2. Inserting "helpful" print statements, or: write some code and, when it
   fails, print out some hopefully useful values and place markers to track
   down the bugs. This method is inelegant, but can get the job done if you
   know what you're doing. We *don't not* recommend this method, but if something
   breaks, you'll have some re-testing to do. Also it makes your code messy. Also also it makes it hard for anyone else to dive in and feel confident they're not breaking something since you don't have a test suite.

3. Test-driven development (TDD), or: write an excessive number of debatably-useful tests before writing any code. Then write some code and fix
   any bugs using the testing output. Rinse and repeat. TDD is trendy approach at
   the moment, but honestly not our favorite; we think TDD is pedantic and
   cumbersome and cements your program's design before you've even decided what the best design is.

# Our approach to testing

We believe a good program begins with a program outline (just like how you're supposed to create an outline before writing that essay about [*The Tenant of Wildfell Hall* and how Anne Brontë was ahead of her time and is still completely underrated](http://www.harkavagrant.com/index.php?id=202), or at least that's what our high school English teachers told us to do). A program outline allows the coder to determine the general structure and flow of the program including core classes, methods, functions, and variables. No real code is necessary here, pseudocode and stubs are fine to get the point across. Once you have a general outline, then you can start writing tests, feeling confident that you (probably) won't have to frantically change a bunch of names and refactor like, 10 times.

## Case Study: Hot Dog Order Tracker

You are a 10-year-old, newly-minted, proud owner of a hot dog stand who needs a
way to keep track of orders. Fortunately, you're also a [*seasoned*
programmer](https://crouton.net) (geddit??). You decide to code an order
tracker that will allow you to input orders and display an itemized receipt
when an order is completed, including a unique order number. Since you're a
child, you don't need to worry about taxes. Or working, actually — why are you running a business anyway?

### Option 1: Basic Programming (not to be confused with [BASIC](https://en.wikipedia.org/wiki/BASIC) programming)

You want to feel like you know what you're doing, so you figure you'll start with something simple: have the program take in the name of each item and its cost. Done!

```python
def get_food_from_console():  # Returns a str
  """
  Returns (str, False) if the user entered a food, (None, True) otherwise.
  """
  pass

def get_cost_from_console():  # Returns a float
  pass

def make_receipt(order_number, order):  # Returns a str
  """
  Prints the receipt nicely, for example:

    Order #5

    Fries:   $1.00
    Hot Dog: $2.00
    Ketchup: $0.50

    Total:   $3.50
  """
  pass

def main():
  order_number = 1

  while True:
    done = False
    order = []
    while not done:
      food, done = get_food_from_console()
      if food:
        cost = get_cost_from_console()
        order.append(food, cost)

    if order:
      print(make_receipt(order_number, order))
      order_number += 1
```

Example usage:

```
$ python3 hot_dogge.py
Food? Fries
Cost? 1.00

Food? Hot Dog
Cost? 2.00

Food? Ketchpu
Cost? 0.50

Food? Ketchpu
Cost? -0.50

Food? Ketchup
Cost? 0.50

Food?

  Order #1

  Fries:    $1.00
  Hot Dog:  $2.00
  Ketchpu:  $0.50
  Ketchpu: -$0.50
  Ketchup:  $0.50

  Total:    $3.50

Food? <Ctrl-C>
$
```

O.K., it may look like you just wrote most of this program, but because the design is so simple, this pseudocode happens to be mostly executable. If this was your finalized design, here would be the point where we would advocate to start writing tests.

A few issues to consider before committing to this design though: to "remove" an item from the order, you need to add an item with a negative cost — kinda clunky, but functional. There also isn't any grouping in the order; that is, it's not obvious if ketchup is supposed to be a side or go on the hot dog. You could always specify in the name of the item, but maybe that's too much of a bother.


### Option 2: Fancier Programming

You realize that there are some flaws with your current approach — the menu isn't saved anywhere, there's a lot of typing involved, and it's easy to make a mistake when entering orders. You decide to go back to the drawing board.

```
class Food(object):
  """A container to make it easy to group names and costs."""

  def __init__(self, name, cost):
    self.name = name
    self.cost = cost


MENU = {
  'A': Food('Fries', 1.00),
  'B': Food('Hot Dog', 2.00),
  'C': Food('Ketchup', 0.50),
}


def get_food_from_console():
  """
  Returns (bool, str, False) if the user inputs a food
    bool for whether to add or remove the food
    str for the food's identifier
    False to indicate that the user isn't done

  Returns (None, None, True) if the user doesn't input a food.
  """
  pass


def make_receipt(order_number, order):  # Returns a str
  """
  Prints the receipt nicely, for example:

    Order #5

    Fries:   $1.00
    Hot Dog: $2.00
    'Chup:   $0.50

    Total:   $3.50

  Note: Contrasting with the make_receipt function in Option 1, this function
  deals with Food objects instead of (str, float) tuples.
  """
  pass


def main():
  order_number = 1

  # Assumes the input is either "<ID>" or "remove <ID>"
  while True:
    done = False
    order = []
    while not done:
      should_add, identifier, done = get_food_from_console()
      if identifier is not None:
        food = MENU[identifier]

        if should_add:
          order.append(food)
        else:
          order.remove(food)

    if order:
      print(make_receipt(order_number, order))
      order_number += 1
```

Example usage:

```
$ python3 hot_dogge2.py
Item? A1
Item? A2
Item? remove A2
Item? A2
Item? A3
Item?

  Order #1

  Fries:    $1.00
  Hot Dog:  $2.00
  Ketchup:  $0.50

  Total:    $3.50

Item? <Ctrl-C>
$
```

On the bright side, this approach makes it much easier to create orders and fix mistakes. It's also impossible to mis-type a food name or cost, and mistakes don't show up in the final receipt.

That said, there are still a few problems: the grouping issue that Option 1 had is still around (is the ketchup a side or on the hot dog???) and since the menu is hard-coded within the program, you can't make custom orders — that is, you can't change the without changing the program itself. This is especially bad when it's your younger sibling's shift and they in their infinite wisdom attempt to make everything cost $1000.00 but screw up the code instead.

### Option 3: Fanciest Programming

```
class Menu(object):
  """A menu object.

  The user is able to add/amend menu entries.
    Adding lets you create custom items — they are lost when the program stops.
    Amending lets you change the name and/or cost of a food on the menu.
  """


def create_menu(filename):
  """
  Creates a menu from a file of items.

  Crashes if the file repeats an ID.
  """
  pass


class Food(object):
  """A container to make it easy to group names and costs."""

  def __init__(self, name, cost):
    self.name = name
    self.cost = cost


class Item(object):
  """Groups Foods together, e.g. Fries with Ketchup and Mayonnaise."""

  def __init__(self, foods=None):
    self.foods = foods or []  # list of Food


class Order(object):
  """Groups Items together, e.g. [Fries with Mayonnaise, Hot Dog with 'Chup]."""

  def __init__(self, number):
    self.number = number
    self.items = dict()  # dict of item_id: [Item]

  def generate_item_id(self):  # Returns a unique identifier str
    pass


def make_receipt(order):  # Returns a str
  """
  Prints the receipt nicely, for example:

    Order #5

    Fries           $1.00
      w/ Ketchup    $0.50
      w/ Mayonnaise $0.50

    Hot Dog:        $2.00
      w/ Ketchup    $0.50

    Total:          $4.50

  Note: Contrasting with the make_receipt function in Option 2, this function
  deals with Order objects, which are now more complex and represent a
  hierarchy.
  """
  pass


def handle_menu_command(menu, subcommand):
  """
  Parses subcommand and modifies the menu accordingly.
  """
  pass


def handle_order_command(order, subcommand):
  """
  Parses subcommand and modifies the order accordingly.
  """
  pass


def handle_item_command(order, subcommand):
  """
  Parses subcommand and modifies the order accordingly.
  """
  pass


def handle_food_command(order, subcommand):
  """
  Parses subcommand and modifies the order accordingly.
  """
  pass


def main():
  if len(sys.argv) <= 1:
    menu = Menu()
  else:
    menu = create_menu(sys.argv[1])

  # Commands:
  #
  # menu add <food_id> "<name>" <cost>
  # menu edit <food_id> "<name>" <cost>
  #
  # <food_id>               (item with one food)
  # <food_id> <food_id> ... (item with multiple foods)
  #
  # item <item_id> add <food_id>    (retroactively add a food to an item)
  # item <item_id> remove <food_id> (retroactively remove a food from an item)
  #
  # order add "<name>" <cost> (add a special item to the order)
  #                           (can be used to add a discount)
  # order remove <item_id>
  # order end (finish the current order and print its receipt)

  order_number = 1
  while True:
    order = Order(order_number)

    subject, subcommand = parse(get_command())
    if subject == 'menu':
      handle_menu_command(menu, subcommand)
    elif subject == 'order':
      handle_order_command(order, subcommand)
    elif subject == 'item':
      handle_item_command(order, subcommand)
    elif subject == 'food':
      handle_food_command(order, subcommand)

    print(make_receipt(order))
    order_number += 1
```

Example usage:

```
$ python3 hot_dogge3.py ~/Desktop/no_younger_siblings_allowed/menu.txt
Parsed menu:
  A: Fries      $1.00
  B: Hot Dog    $2.00
  C: Ketchup    $0.50

Order #1
Command? menu add D "Mayonnaise" $0.50
Command? A C D
  item id = 1
    Fries           $1.00
      w/ Ketchup    $0.50
      w/ Mayonnaise $0.50

Command? B
  item id = 2
    Hot Dog:        $2.00

Command? item 2 add C
  modified item 2:
    Hot Dog:        $2.00
      w/ Ketchup    $0.50

Command? item 2 add C
  modified item 2:
    Hot Dog:        $2.00
      w/ Ketchup    $0.50
      w/ Ketchup    $0.50

Command? item 2 remove C
  modified item 2:
    Hot Dog:        $2.00
      w/ Ketchup    $0.50

Command? C
  item id = 3
    Ketchup:        $0.50

Command? D
  item id = 4
    Mayonnaise:     $0.50

Command? order remove 5
  removed item 4:
    Mayonnaise:     $0.50

Command? order 1 add "special friend discount" -$1.00
  item id = 5
    special friend discount: -$1.00

Command? order end

  Order #1
    Fries                     $1.00
      w/ Ketchup              $0.50
      w/ Mayonnaise           $0.50

    Hot Dog                   $2.00
      w/ Ketchup              $0.50

    Ketchup                   $0.50

    special friend discount: -$1.00

    Total:                    $4.00

Order #2
Command? <Ctrl-C>
$
```

Whoof, this one's a long one. Buuuuuuut now your sibling can't permanently mess up the menu, you can add special orders, and you can keep track of which items go together — heck yeah! Maybe all that thinking and restructuring was worth it after all! 

In fact, this program isn't even that complicated once you break it down: in order to keep track of toppings vs. sides, you had to add a few classes to create a hierarchy, so you have a handful of functions (all those `handle_<object>_command` functions) that are the same basic function modified for each new class. Note that the menu is read in from an external file when the program is launched, keeping it safe from any meddling younger siblings.

#### A New Hierarchy

Now, instead of just having orders comprised of items, you have a `Menu` object which contains `Food` objects, and `Order` objects which contain `Item` objects which are actually just one or more `Food` objects. This way, your program knows how to group foods within an order when it prints out the receipt; that is, it will print out each `Item` object as its own block of foods.

More generally, the hierarchy works like this:

	- Menu
	  - Food
	  - Food
	  - Food
	  - Food
	  - Food

	- Order
	  - Item
	    - Food
	    - Food
	    - Food
	  - Item
	    - Food

#### Decisions, decisions, decisions!

- **`menu`** → commands related to modifying the menu
	- **`add <food_id> "<name>" <cost>`** → add a new food option to the menu (note that if the program is restarted, this new item will not persist, so this option is a good way to add a temporary item like a special of the day)
	- **`edit <food_id> "<name>" <cost>`** → modify an existing food option on the menu (note that the `<food_id>` stays the same, but the `name` and `cost` of the item may be modified. this is the only way to effectively delete an item from the menu without modifying the menu file and restarting the program)

- **`<food_id> ...`** → add an item consisting of one or more food to the current order

- **`item`** → commands related to modifying an item in the current order
	- **`<item_id> add <food_id>`** → add food `<food_id>` to item `<item_id>`
	- **`<item_id> remove <food_id>`** → remove food `<food_id>` from item `<item_id>`

- **`order`** → commands related to modifying the current order
	- **`add "<name>" <cost>`** → add a special item to the current order (note that this option can be used to add either custom items with a positive cost, or discounts with a negative cost)
	- **`remove <item_id>`** → remove item `<item_id>` from the order (note that the item id will not be re-used if another item is added to the current order)
	- **`end`** → end the current order and print the receipt

## The Aftermath

Now that you've squared away your design, you can finally break out the fancy TDD toolbox and start writing incremental tests, confident that you won't end up stranding yourself with an obsolete test suite and a bunch of refactored, untested code. We're not going to talk about how to test here, since this post is just about our philosophy surrounding testing, but we will probably write a post in the future about how to test.


