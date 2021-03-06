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

Named tuples are quite similar to regular ones and offer all the same tools
(and then some more). The advantage of named tuples over regular ones is that
their fields can have descriptive names. Instead of having to remember what
data is stored in which index, the programmer only has to remember what name
fields have been given. This also makes it easier to read code written by
someone else, as you do not have to guess what is stored in index 2 for
example.

Named tuples are not available by default, so you have to import the
appropriate function as described in :ref:`modules-and-packages-importing`.
This is needed at the beginning of every file where you want to use
*namedtuple* function and in :ref:`REPL <crash-course-repl>` before you use
the function:

.. code-block:: hylang

   => (import [collections [namedtuple]])

First step is to create a :ref:`type <variables-types>` to represent the
specific named tuple and store it somewhere for future use:

.. code-block:: hylang

   (setv Measurement (namedtuple "Measurement" ["value" "unit"]))

After the type has been created, it can be used just like any other ordinary
type. Attributes that we specified earlier are available and can be accessed:

.. code-block:: hylang

  => (setv box-width (Measurement 5 "meters"))
  => (. box-width unit)
  "meters"
  => (. box-width value)
  5

.. TODO:: keyword parameters link here

One can use keyword parameters to make creation of a tuple clear:

.. code-block:: hylang

  => (setv box-height (Measurement :unit "meters" :value 2)
  => (. box-height value)
  2

Indexed access and deconstructing works just like with regular tuples:

.. code-block:: hylang

  => (first box-width)
  5
  => (setv (, measurement unit) box-width)
  (5, "meters")
  => unit
  "meters"

Oseo's work
+++++++++++

Oseo was really delighted when he learned about named tuples. No more second
guessing what was stored in the 3rd element of a tuple. Names would help
him and his colleaques at the filing department to keep things in neat order
from now on. Instead of fixing the old labeling system for parcels (after all,
it was working just fine), he was tasked with a new problem: interdepartment
mail system.

Filing department was only one of the many departments of the 4th Order and
sometimes they needed to communicate with each other. The problem was that
the 4th Order had grown very organically, sprouting new departments here and
there and was generally really convoluted mess. All communications between
the departments was rather ad hoc solutions and it took considerable amount of
time and energy to even track down how to send message to a specific
department. There were just so many different systems in use.

To alleaviate the problem, Oseo decided that there needs to be one central
location within filing department, where one can simply drop their message
in a tiny metal cylinder, that then gets delivered to destination with 
appropriate means:

.. code-block:: hylang

  (setv Message (namedtuple "Message" [recipient department message-text]))

  (create-message [recipient department text]
    (Message :recipient recipient
             :department department
             :message-text text))

  (drop-off-message [message]
    (setv wrapped (wrap-message message))
    (dispatch-message wrapped))

Because there are many different ways of delivering message, Oseo built a
system that wraps original message inside another metal tube. This metal tube
is constructed specifically to the specifications of the target department.
Some prefer plain tube, some require intricate carvings. They all have slightly
different expectations on how addresses are written even. But all these
details are taken care by ``wrap-message`` function.

``dispatch-message`` is responsible for routing wrapped message to correct
direction. It does this by examining wrapped message, which contains
information what kind of system should be used in routing:

.. code-block:: hylang

  (defn dispatch-message [message]
    (setv system (. message routing-method))
    (if (= system 'pressure-tube) (route-pressure-tube message)
        (= system 'pigeon) (route-pigeon message)
        (= system 'courier) (route-courier message))
        (manual-routing message))

Each and every routing system has unique set of data that they need for
correctly routing the wrapped message:

.. code-block:: hylang

  (defn route-pressure-tube [message]
    (setv tube (. message tube))
    (setv pressure (if (= (. message priority) 'high)
                       'extra-pressure
                       'regular-pressure))
    (insert-message tube message)
    (set-pressure tube pressure))

  (defn route-pigeon [message]
    (setv destination (. message district))
    (setv pigeon-house (select-house destination))
    (setv pigeon (take-pigeon pigeon-house))
    (attach-message pigeon)
    (release-pigeon pigeon))

Here is a little trick that Oseo came up with: Since every wrapped message has
some common information and some information specific to the routing method,
Oseo defined multiple types of named tuples for the task. This way wrapped
messages sent via pressure tube do not have to know anything about carrier
pigeons and vice versa. Oseo anticipates that new methods will be added in the
future and wanted to build a system that is easy enough to extend at that point:

.. code-block:: hylang

  (setv PigeonMessage (namedtuple "PigeonMessage"
                                  ["routing-method" "district"]))

  (setv TubeMessage (namedtuble "TubeMessage"
                                ["routing-method" "tube" "priority"]))

.. index:: 
   single: datastructures; set
.. _data-structures-sets:

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
