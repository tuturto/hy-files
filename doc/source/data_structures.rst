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

Our first structure is tuple. They're *ordered* (meaning that the values you put
in have well defined and meaningful location within the data structure),
*immutable* collection of zero or more values. 

...

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


.. index:: 
   single: datastructures; named tuple
.. _data-structures-named-tuples:

Named tuples
------------

.. index:: 
   single: datastructures; set
.. _data-structures-sets:

Sets
----

.. index:: 
   single: datastructures; list
.. _data-structures-lists:

Lists
-----

.. index:: 
   single: datastructures; dictionary
.. _data-structures-dictionaries:

Dictionaries
------------
