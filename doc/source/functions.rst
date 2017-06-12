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

Functions are defined using *defn*, as show in listing below. It defines a
function *sum-if-even*, which has two formal parameters *a* and *b*. Inside of
the function there's if statement that will first check if both arguments a
and b are even and then add them together and return the resulting number.
If either one of the arguments is not even, function simply returns 0. Defn
is relatively complex tool and has several options. Next we'll take closer
look on how to use them to your advantage.

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

Higher order functions
----------------------

Decorators
----------

Recursion
---------

tco and all that
----------------
