.. hy-files documentation master file, created by
   sphinx-quickstart on Mon Jun 12 20:38:09 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to hy-files
===================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   intro
   getting_started
   crash_course
   variables
   control_structures
   functions
   data_structures
   working_with_sequences
   multimethods
   building_pipelines_for_data
   reading_and_writing_files
   classes_and_objects
   modules_and_packages
   being_friends_with_python
   macros

What you need for this book
===========================

Computer is a good start. Hy is built using Python, so an installed Python
environment is a very good thing to have. While Hy works with several different
kinds of Pythons, CPython 3.5 is recommended.

A special integrated development environment isn't neccessary, simple text
editor like Notepad, Notepad++, vi or such will suffice. There are ways of
setting up really nice Hy development environment using emacs or vim for
example, but that's beyond the scope of this book.

Who is this book for?
=====================

Previous experience with programming isn't neccessary, as the book tries to
build from ground up. The book is aimed at people who haven't programmed much
(if at all) in Hy. Occasionally the book might refer to other programming
languages to highlight some specific aspect of Hy, but understanding of those
languages isn't needed for reading this book.

The book is aimed for people who might have some programming experience with
other languages and who are curious about Hy or Lisps in general. It should
reasonably work for people who don't have any programming experience at all.

Conventions
===========

In this book, you'll find a number of text styles used to distinguish between
different types of information. Here are some examples of them and their
explanation.

A block of code is set as shown in listing below. This style is often used to
highlight particular concept, structure or behaviour of code.

.. code-block:: hylang

   (defseq fibonacci [n]
     "infinite sequence of fibonacci numbers"
     (if (= n 0) 0
         (= n 1) 1
         (+ (get fibonacci (- n 1))
            (get fibonacci (- n 2)))))

Different kind of notation is used for code entered in interactive mode, as
shown below. Code in such an example should work when entered in interactive
mode (or REPL for short). =>  and ... at the beginning of the lines shouldn't
be entered, they're there to denote that the example can be entered in the
interactive mode. Lines without => or ... show output of commands you just
entered.

.. code-block:: hylang

   => (defn add-up [a b]
   ...  (+ a b))
   => (add-up 2 3)
   5

New terms and important words are shown as: *Important new term*.

Resources
=========

As this book can't possibly cover everything about Hy, some more handy resources
are listed here in no particular order:

Code, open issues and other matters related to development of the language can
be found at `GitHub: <https://github.com/hylang/hy>`_.

Up to date documenation is available online at
`Read the Docs <http://docs.hylang.org/en/latest/>`_. Be mindful that there are
several versions of the documentation online, so be sure to select the most
applicable one from the menu bottom left.

There's active community on IRC [#f1]_ at #hy on freenode. This is probably the
best place to start asking questions as many core developers frequent the
channel.

Semi-actively discussion is also held in hylang-discuss at
`Google groups <https://groups.google.com/forum/#!forum/hylang-discuss>`_.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. [#f1] Albeit slowly falling out of favor, Internet Relay Chat is still
         commonly used in many open source projects
