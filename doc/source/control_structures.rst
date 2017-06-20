Control Structures
==================

As the name implies, control structures are used to control which parts of the
program will execute. They are used to select between different branches in
code or repeat some action multiple times. Often the choice of executing a
specific branch is done based on some value or combination of several values.
First we'll have a look at some common predicates [#f1]_ and how to combine
them. After covering them, we'll dive into various control structures and their
applications.

Boolean algebra
---------------

Boolean algebra [#f2]_ is very basis of how computers operate. It consists of
two values *True* and *False* and set of basic operations *and*, *or*, *not*
and *xor*.

Hy adds a little twist in the Boolean algebra. Instead of operating strictly
only on True and False values, it can operate on almost any value. Values that
evaluate to True in Boolean algebra are considered to be *truthy* values.
Conversely, values that evaluate to False in Boolean algebra are considered to
be *falsey* values. Following list shows values considered falsey, all other
values are considered truthy:

* None
* number 0
* any empty sequence or collection

.. index:: 
   single: boolean logic; and

*And* operator takes 0 or more arguments and returns last argument if all of
them are True or no parameters were passed at all, as shown below. Notice how
in the last example we're passing in three numbers and last one of them is
returned as the result. This is because all of them are truthy, so the final
one will be returned. Sometimes this technique can be useful, for example
when you want to first check that set of variables are truthy and then perform
an operation with the last one of them.

.. code-block:: hylang

   => (and True True)
   True
   => (and True False)
   False
   => (and False False)
   False
   => (and)
   True
   => (and 1 2 3)
   3

.. index:: 
   single: boolean logic; or

*Or* operator takes 0 or more arguments and returns first truthy argument if
one or more of them are True. Some examples of usage are shown below. Notice
how *or* without any arguments doesn't seem to return anything. This is
because in that case it returns *None*, a special value denoting non-existant
value (which is also considered falsey) and REPL doesn't print it on screen.

.. code-block:: hylang

   => (or True True)
   True
   => (or True False)
   False
   => (or False False)
   False
   => (or)
   => (or 1 2 3)
   1

In order to see actual return *type*, one can use *type* as shown here:

.. code-block:: hylang

   => (type (or))
   <class 'NoneType'>

.. index:: 
   single: boolean logic; not

Sometimes there's need to reverse Boolean value, so that True turns to False
and False turns to True. That can be achieved with *not* operator. It takes
one argument and always return either True or False, as shown below. While
it's possible to call *not* with truthy values, it will not be able to deduce
corresponding opposite value, which is the reason why only True or False is
returned.

.. code-block:: hylang

   => (not True)
   False
   => (not False)
   True
   => (not 1)
   False
   => (not [])
   True

.. index:: 
   single: boolean logic; xor

The final operator we're going to learn now is *xor*, short for exclusive or.

fill in xor here

Short circuiting
----------------

fill in details here

.. _control-structures-common-predicates:

Common predicates
-----------------

<, >, <=, >=, =, !=, integer?, odd?, even?

Branching
---------

do

if, if*, if-not, when, cond, lif, lif-not, while, unless

every?

Looping
-------

for, break, continue

while

reference to recursion

.. [#f1] predicate is a test that evaluates to True or False
.. [#f2] Boolean algebra, also known as Boolean logic, is named after its
         inventor George Boole
