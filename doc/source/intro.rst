Introduction
============

Welcome to hy-files. This book explores some aspect of a exciting new language
called Hy, which is a dialect of Lisp [#f1]_. What makes Hy somewhat different
is that the language is *embedded* inside other language, Python. This allows
routines written in Hy call code written in Python and vice-versa.

A challenge with programming books is that while books is usually read
sequentially, learning isn't sequential. There's all kinds of paths, loops and
detours through the subject. Some may be faster than others, while some are
more useful than others. Concepts in programming relate to each other and form
a web where almost everything is connected with everything in a manner or
another. This books tries to present a path through subject of programming in
Hy. While it covers substantial amount of matter, even more matter had to be
left out. Otherwise this book would have been an unwieldy tome that was never
finished. Sometimes it refers to concepts that will be covered later in
detail, but I have tried to keep that in minimum.

`Source code <https://github.com/tuturto/hy-files>`_ for the book is available
for anyone to view and offer their feedback or even contributions.

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

As this is a living book, it will evolve and change over time. At the beginning
it is rather empty, but it will be filled eventually. Readers are encouraged to
check :doc:`version` for information what parts have recently been added or
majorly edited.

Resources
=========

As this book can't possibly cover everything about Hy, some more handy resources
are listed here in no particular order:

Code, open issues and other matters related to development of the language can
be found at `GitHub <https://github.com/hylang/hy>`_.

Up to date documenation is available online at
`Read the Docs <http://docs.hylang.org/>`_. Be mindful that there are
several versions of the documentation online, so be sure to select the most
applicable one from the menu bottom left.

There's active community on IRC [#f2]_ at #hy on freenode. This is probably the
best place to start asking questions as many core developers frequent the
channel.

Semi-actively discussion is also held in hylang-discuss at
`Google groups <https://groups.google.com/forum/#!forum/hylang-discuss>`_.

.. [#f1] Lisp is a whole family of languages, originating from the 1950s. 
         Modern variants include such languages as Clojure, LFE and Racket.

.. [#f2] Albeit slowly falling out of favor, Internet Relay Chat is still
         commonly used in many open source projects
