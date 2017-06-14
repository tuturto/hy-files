Functions
=========

Functions are basic building block of any Hy program. They let you to bundle up
functionality into a reusable piece of code with a well defined interface and
then use that function in other parts of your programs. So far we have been
using functions that come with Hy and Python, but now it's time to look into
how to define them by yourselves.

Defining functions
------------------

Functions are the main mean of packaging some functionality into a reusable box
and giving it a name. Once declared, a function can be used over and over again
in different parts of the program. It both saves typing and helps you keep your
program organized. A function may accept parameters that are used to relay
information into it, these can be anything data that function is supposed to
process, parameters that control how function behaves and even other functions.
Functions often have return value, that is the result of a function. In this
sense they're akin to mathematical functions. In Hy functions can also have
side-effects. These are changes that occur somewhere in the program (like
setting a variable or printing on screen) or elsewhere in the computer system
(like writing into a file).

.. index:: function; defining

Functions are defined using *fn*, which creates a new function. In order to
use the defined function, it usually needs to be bound to a name. This pattern
is so common that a special shortcut called *defn* has been added. It takes
name and definition of a function and then creates and binds the function into
the name. Following two examples will have identical results:

.. code-block:: hylang

   (setv hello (fn [person] (print "hello " person)))

   (defn hello [person]
     (print "hello " person))

Below is another example of using *defn*. It defines a function *sum-if-even*,
which has two formal parameters *a* and *b*. Inside of the function there's if
statement that will first check if both arguments a and b are even and then
add them together and return the resulting number. If either one of the
arguments is not even, function simply returns 0. Unlike many other languages,
Hy doesn't have explicit return keyword. As you can see, the value of last
expression executed is also the return value of the function. Defn is
relatively complex tool and has several options. Next we'll take closer look
on how to use them to your advantage.

.. code-block:: hylang

   => (defn sum-if-even [a b]
   ...  (if (and (even? a)
   ...           (even? b))
   ...    (+ a b)
   ...    0))
   => (sum-if-even 1 2)
   0
   => (sum-if-even 2 4)
   6
   => (+ (sum-if-even 2 4) 
   ...   (sum-if-even 4 4)
   ...   (sum-if-even 1 2))
   14

Remember how I mentioned that functions let you to abstract away functionality
behind a nicely defined interface? This actually has two facets. On the other
hand, you're giving a specific name to a specific type of functionality. This
lets you to think in terms of that name, instead of trying to keep track of
everything that is going inside of that function. Another related matter is
called variable scope. If you're defining formal parameters for your function,
they're unique inside of it. It doesn't matter if you (or somebody else) has
already used those names somewhere in the program. Inside of your function
they're yours to do as you please, without causing mix-up somewhere else in
the program. We can demonstrate this as shown below:

.. code-block:: hylang

   => (defn scope-test []
   ...  (setv a 1
   ...        b 2)
   ...  (+ a b))
   => (setv a 10
   ...      b 5)
   => (scope-test)
   3
   => a
   10
   => b
   5

Variables *a* and *b* are declared outside of the *scope-test* function.
Variables with same names are also declared inside of the function and used to
perform a calculation. But the variables declared inside the function cease to
exist after the function completes. Hy (and Python) use something called
*lexical scoping*, originally introduced by ALGOL. The name itself isn't that
important, but the idea is. It might be worth your time to write little play
programs and try out different things with variables and functions to get a
good grip on this.

Functions are good for breaking down a complex problem into more manageable
chunks. Instead of writing down complete instructions in one huge block how to
solve the problem, you can write the basic structure or the bare essence of
the problem. A hypothetical AI routine for a wizard is shown here:

.. code-block:: hylang

   (defn wizard-ai [wizard]
     (if (and (in-combat? wizard)
              (badly-hurt? wizard)) (cast-teleport wizard)
         (in-combat? wizard) (cast-magic-missile wizard)
         (in-laboratory? wizard) (research-spells wizard)
         (wander-around wizard)))

It's very simple and hopefully easy to read too. At this level, we aren't
interested what kind of magical components teleport spell requires or what
spell research actually means. We're just interested on breaking down the
problem into more manageable pieces. In a way, we're coming up with our own
language too, a language that talks about wizards and spells. And it's
perfectly ok to write this part down (at least the first version), without
knowing all the details of the functions we're using. Those details can be
sorted out later and it might even be someone else's task to do so. Later on,
we might want to add a new creature in our game and realize that we can
actually use some of the functions we came up earlier as shown below.
In a way we're building our own mini-language that talks about wizards, combat
and spells.

.. code-block:: hylang

   (defn warrior-ai [warrior]
     (if (in-combat? warrior) (hit-enemy warrior)
         (badly-hurt? warrior) (find-wizard warrior)
         (wander-around warrior)))

Optional parameters
-------------------

Sometimes you might need to write a function or method that takes several
parameters that either aren't always needed or can be supplied with reasonable
default. One such method is *string.rjust* that pads a string to certain
length. By default a space is used, but different character will be used if
supplied as show in next. In such occasions *optional parameters* are used.

.. code-block:: hylang

   => (.ljust "hello" 10)
   "hello     "
   => (.ljust "hello" 10 ".")
   "hello....."

Optional parameters are declared using *&optional* keyword as shown in the 
example about fireballs. Parameters after optional are declared having default
values that are denoted as two item lists with the parameter name being first
and default value being the second element. If the default value isn't
supplied (as is the case with strength in the example), None is used. Be
mindful to use only immutable values as defaults. Using things like lists will
lead into very unexpected results.

.. code-block:: hylang

   => (defn cast [character &optional [name "fireball"] strength]
   ...  (if strength
   ...    (.join " " [character "casts" strength name])
   ...    (.join " " [character "casts" name])))

Our cast function has three parameters, out of which one (the caster) must
always be given. Second parameter can defaults to *"fireball"* and third one
(strenght of the spell) doesn't have default value. Inside of the function
parameters are joined together to form a string that represents spell casting.
There are several ways of calling the function, as shown here:

.. code-block:: hylang

   => (cast "wizard")
   "wizard casts fireball"

   => (cast "wizard" "lightning")
   "wizard casts lightning"

   => (cast "mage" "acid cloud" "super-strong")
   "mage casts super-strong acid cloud"

Positional parameters
---------------------

Sometimes you might want to write a function that handles varying amount of
parameters. One way to get around that is to define large number of optional
parameters, but that is both clumsy and error prone. Also, you would have to
guess maximum amount of parameters that will ever be needed and such guesses
tend to go wrong.

Luckilly, there's elegant way around the problem: *positional parameters*.
They allow you to define a special parameter, that holds 0 or more arguments
when the function is called, depending on the amount of arguments supplied.
And of course you can mix them with the regular parameters, just make sure you
don't try to declare regular or optional parameters after the positional one.

Positional arguments are defined with *\&rest* keyword as shown below, where
a thief err.. treasure hunter collects some loot, which is defined as
positional parameters.

.. code-block:: hylang

   => (defn collect [character &rest loot]
   ...  (if loot
   ...    (.join " " [character "collected:"
   ...           (.join ", " loot)])
   ...    (.join " " [character "didn't find anything"])))

In :doc:`working_with_sequences` we'll go through some useful information for
working with positional arguments. After all, they're supplied to you as a
list, so things like *map*, *filter* and *reduce* might become handy. Below is
excerpt of REPL session showing our little looting routing in action. As you
can see, we can define a variable amount of items that the characters has found
and decides to collect for the future use. In case where no positional
arguments haven't been supplied, a different message is given.

.. code-block:: hylang

   => (collect "tresure hunter" "diamond")
   "tresure hunter collected: diamond"

   => (collect "thief" "goblet" "necklace" "purse")
   "thief collected: goblet, necklace, purse"

   => (collect "burglar")
   "burglar didn't find anything"

Higher-order functions
----------------------

Higher-order functions are just ordinary functions that have functions as
their formal parameters or return value (or even both). In essence, they are
functions that deal with other functions, hence the name. They are useful in
many situations, allowing one to write generic code that can be easily adapted
to handle specific cases. For example, here is an example of making an
alchemy potion. Each potion has dry *ingredients* and one or more *liquids*.
Dry ingredients are simply mixed together, while liquids might need different
approach depending on what kind of potion is being made. The choice of how to
*prepare* liquids is left to the discretion of the alchemist and they need to
supply *mix-potion* with the function that they would like to use to prepare
liquids.

.. code-block:: hylang

   (defn mix-potion [ingredients liquids prepare]
     (setv mixture (mix ingredients))
     (setv liquid (prepare liquids))
     (combine liquid mixture))

Lets pretend that some other alchemist has defined different ways of preparing
mixtures for us as show below:

.. code-block:: hylang

   (defn stir [liquids]
      ...)

   (defn slosh [liquids]
      ...)

   (defn carefully-mix [liquids]
      ...)

On a superficial level, each function looks same. They might have different
names, but they have same amount of parameters and return similar things
(mixture of liquids). To use them in potion making, our alchemist can do
something like this:

.. code-block:: hylang

   (mix-potion ["pixie dust", "fly wings"]
               ["water", "juice"]
               slosh)

   (mix-potion ["olive"]
               ["gin", "vermouth"]
               stir)

   (mix-potion ["newt eyes", "dragon nail", "basilisk scale"]
               ["nitric acid", "hydrochloric acid"]
               carefully-mix)

Each of these would create a new potion, using the specified ingredients,
liquids and method of combining liquids. Such way of programming lets us to
write general code, which is not interested on the tiny details, but in the
overall process of how to do something. While working with such code, the
programmer can concentrate on problem at the hand and defer details to another
time or even have somebody else to help writing them.

.. index:: 
   single: function; anonymous

It is also possible to write functions that create new function when called.
While it is possible to use *defn* to do so, often it is simpler to use *fn*.
These functions are sometimes called anonymous, as they are not bound to a
name.

To illustrate this, lets look a different kind of problem. Our friends in
gnomish bank are handling deposits of customers from various different
regions. While gold is easy to handle, it is the letters that are causing
teller gnomes headache. Even simple things like greetings in the beginning
of a letter are hard to keep in order as elfs, humans and orcs all have
different customs that gnomes try to observe. In order to alleviate this
problem, one particularly crafty gnome has designed an automatic letter
writing system. Like everything that gnomes do, the system is very ornate
and flexible. It consists of very many pieces that can be combined together
in myriad ways. One such part is greeter-crafter. When given a culture, this
device will construct another device which will know how to greet a person
of that culture.

.. code-block:: hylang

   (defn greeter-crafter [culture]
     (if (= culture "elven") (fn [person]
                               (+ "The most illustrious " person.name))
         (= culture "human") (fn [person]
                               (+ "Greetings " person.name))
         (= culture "orcish") (fn [person]
                               (+ "Saluations " person.name))
         (fn [person]
           "Dear sir or madam")))

Heart of the routine is a case study. *culture* parameter is examined and
corresponding branch of if statement is executed. There are three special
cases, each corresponding to a specific culture and a generic one that is used
when unknown culture is given as an argument. Each of the branches will
create a new function and return it. Following piece of code highlights how
greeter-crafter could be used to personalize monthly report letter.

.. code-block:: hylang

   (defn handle-monthly-letter [person]
     (setv greeter (greeter-crafter (culture-of person)))
     (setv letter (+ (greeter person)
                     (write-body person)
                     (in-closing person)))
     (send letter))

First a *greeter* is constructed by using greeter-crafter. Then a letter
consisting of greeting, body of text and closing statement is crafted and
finally the letter is sent. In case gnomes would like to send yearly letters
too, they could reuse the greeter-crafter and would only need to create
new gadget that knows how to write body of the yearly letter. And if later
a new culture would start doing business with the gnomes, they would add
this culture to greeter-crafter and all different types of letters would
automatically start greeting this new culture correctly.

And if gnomes would require more intricate system, nothing would stop them
from creating *greeter-crafter-creator*, a device that can build
greeter-crafters which know how to build greeters that know how to address
members of a specific culture. Very sophisticated, intricate and maybe
even confusing system.

Decorators
----------

Recursion
---------

tco and all that
----------------
