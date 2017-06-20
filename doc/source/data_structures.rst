Data Structures
===============

Simple programs can be written that use just few variables to hold the data
that they need. But as the programs grow more complex and they handle more
data, single variables are not going to be enough. At that point you will
need to use data structures to hold your data and keep it organized. Different
data structures are suited for different kinds of tasks and there are lots
of different structures, each with pros and cons. In this chapter we are
going to have a look at some of the most common ones that you might need.

We will meet a new friend, apprentice filer Oseo, who has recently started
working at the filing department of 4th Order. He will introduce several
different ways of organizing data for us.

.. index:: 
   single: datastructures; mutability
.. _data-structures-mutability:

Mutability
----------

Before we start exploring different kinds of data structures, we have to talk
a bit about mutability. Some of the data structures covered here are mutable,
while others are immutable. The difference between the two are that once you
have created an immutable data structure, it can never be modified. Using
immutable data structures can help you to minimize errors where program
inadvertly changes some important data.

In terms of filing department, it means that after Oseo has created an filing
entry, nobody is ever allowed to change it. If such information will need
updating later, the only way to do so is to create a new entry and replace the
old one with it.

.. index:: 
   single: datastructures; tuple
.. _data-structures-tuples:

Tuples
------

Our first structure is tuple. They're *ordered* (meaning that the values you
put in have well defined and meaningful location within the data structure),
*immutable* collection of zero or more values. 

Creation
++++++++

There are two ways of creating tuples: using *,* literal or *tuple* initializer
(initializers are covered in more detail
:ref:`later <classes-and-objects-initializing>` when we talk about classes and
objects).

The *,* literal takes a list of values that are placed into the tuple:

.. code-block:: hylang

   => (, 1 2 3)
   (, 1 2 3)

   => (, "one" 2 "III")
   (, "one" 2 "III")

And nothing prevents you from creating a nested tuple, a tuple that has one
or more tuples inside of it:

.. code-block:: hylang

   => (setv address (, (, "John" "Smith") (, "Pyramid Road 1" "Cairo")))
   (, (, "John" "Smith") (, "Pyramid Road 1" "Cairo"))
   => (first address)
   (, "John" "Smith")

.. todo:: link to iterables here

*tuple* initializer take a single *iterable*, which items are used to
initialize a new tuple. For example, one can take a list and use it to create
a new tuple that has same elements in it:

.. code-block:: hylang

   => (setv sizes ["small" "medium" "large"])
   => (tuple sizes)
   (, "small" "medium" "large")

Common operations
+++++++++++++++++

While it is not possible to modify tuple once it has been created, there are
still several other operations that can be done. Next we are going to look
at some of the most common ones.

Just creating tuples is not going to be that useful, we want a way to get back
the data that was originally put in. For that we have three operations at our
disposal: *first*, *second* and *get*.

.. code-block:: hylang

   => (setv data (, "quick" "brown" "fox"))
   (, "quick" "brown" "fox")
   => (first data)
   "quick"
   => (second data)
   "brown"
   => (get data 2)
   "fox"

Notice how *get* uses zero based indexing. Number of the first element is 0,
second one is 1 and third one is 2 (and so on). Using negative indexes is
possible too. In that case counting starts from the end of the tuple:

.. code-block:: hylang

   => (get data -1)
   "fox"
   => (get data -2)
   "brown"

If you have two tuples, you can generally compare if they are equal or not
(I say generally, because if they contain things that can not be compared, you
can not compare two tuples). For the comparison, old friends of *=* and *!=*
are used (we talked about them earlier in 
:ref:`control-structures-common-predicates`).

.. code-block:: hylang

   => (= (, 1 2 3) (, 1 2 3))
   true
   => (!= (, 1 2 3) (, 1 2 3))
   false

There are few functions for examining contents of a tuple. You can check if
an element is in tuple using *in*, how many times certain element is in tuple
using *.count* (note the dot at the beginning, it is significant as it
denotes that count is a *method* of tuple. We will cover these in more detail
when we talk about :ref:`methods <classes-and-objects-methods>`) or you
can find first index the item appears in tuple using *.index*. Finally, to
count how many items are in tuple, we use *len*.

.. code-block:: hylang

  => (setv data (, 3 1 4 1 5 9 2 6))
  => (len data)
  8
  => (in 5 data)
  true
  => (.index data 5)
  4
  => (.count data 1)
  2

Finally, there are two ways of building larger tuples from smaller one: *+*
creates a new tuple that has all the elements of two other tuples combined
and *\** is used to create new tuple that has items of the original tuple
repeatedly:

.. code-block:: hylang

   => (+ (, 1 2 3) (, 4 5 6))
   (, 1 2 3 4 5 6)
   => (* 2 (, 1 2))
   (, 1 2 1 2)

Oseo's work
+++++++++++

Within filing department, Oseo usually uses tuples for storing small pieces of
data. Since they has to remember what each data in given index stands for,
having large structures can be error prone. But for things like coordinates,
measurements (value and unit together) and such tuples are good.

.. code-block:: hylang

   => (defn make-measurement [value unit]
   ...  (, value unit))

   => (set stick-length (make-measurement 1.2 "meters"))
   => (first stick-length)
   1.2
   => (second stick-length)
   "meters"

Sometimes Oseo's work was related to mailing. Not the actual carrying the
letters and parcels around though, but marking them based on which priority
they were. Parcels were then moved forward to a sorting section, where they
would be dispatched to correct direction, depending on the label Oseo had used.

.. code-block:: hylang

  (defn attach-label [label parcel]
    (, label parcel))

  (defn set-priority [parcel]
    (if (paid-extra? parcel) (attach-label 'priority parcel)
        (vip-customer? parcel) (attach-label 'priority parcel)
        (no-postage-paid? parcel) (attach-label 'snail parcel)
        (attach-label 'regular parcel)))

  (defn dispatch-by-priority [tagged-parcel]
    (setv (, speed parcel) tagged-parcel)
    (if (= speed 'priority) (send-immediately parcel)
        (= speed 'regular) (send-evening parcel)
        (send-later parcel)))

.. index:: 
   single: tuple; destructuring

Notice how *dispatch-by-priority* deftly takes apart tuple of two items and
assigns them to local variables with one *setv*. This is called destructuring
and is useful technique. It allows you to take a tuple or list (which we will
cover a bit later in :ref:`data-structures-lists`) and assign each individual
element to a variable. Amount of symbol names in first tuple have to match to
the amount of elements in the second one, otherwise an error will be reported.

In some rare cases already labeled parcel is deemed to be more important or
less important than what Oseo originally labeled it. Since labeling can not
be altered after the creation, a completely new label has to be created that
has new priority, but the old parcel. In such a case simply extracting the
tuple and creating a new one is all that needs to be done.

.. code-block:: hylang

   (defn relabel-parcel [label tagged-parcel]
     (setv (, old-label parcel) tagged-parcel)
     (, label parcel))

.. index:: 
   single: datastructures; named tuple
.. _data-structures-named-tuples:

Named tuples
------------

.. index:: 
   single: datastructures; set
.. _data-structures-sets:

.. code-block:: hylang

   => (import [collections [namedtuple]])

Oseo's work
+++++++++++

Sets
----

.. index:: 
   single: datastructures; list
.. _data-structures-lists:

Oseo's work
+++++++++++

Lists
-----

.. index:: 
   single: datastructures; dictionary
.. _data-structures-dictionaries:

Dictionaries
------------

Oseo's work
+++++++++++
