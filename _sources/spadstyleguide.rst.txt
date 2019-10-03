SPAD Style Guide
================

Maximum line length
-------------------

No firm rule, but use about 70 characters unless that causes problems.
Try to avoid lines longer than 78 characters.

File splitting during compilation
---------------------------------

There is a script `unpack_file
<https://github.com/fricas/fricas/blob/master/src/scripts/unpack_file>`_
that splits ``.spad`` source files at ``)abbrev`` boundaries. As a
result, everything before the first ``)abbrev`` will be discarded.

In particular, it means that there cannot be a global macro definition
for the whole ``.spad`` file.

Literate documentation
----------------------

SPAD files can contain LaTeX parts. In fact there is an awk script in
the commit message of `r1773
<http://sourceforge.net/p/fricas/code/1773/>`_ that turns `xhash.spad
<https://github.com/fricas/fricas/blob/master/src/algebra/xhash.spad>`_
into a proper LaTeX file.

Such documentation must be included into blocks of the form::

  )if LiterateDoc
  ...
  )endif

These parts must form a proper LaTeX document. The code parts are
included and typeset via the `listings package
<https://www.ctan.org/pkg/listings/>`_ .

See `http://sourceforge.net/p/fricas/code/1773/
<http://sourceforge.net/p/fricas/code/1773/>`_ or git commit `5147241e
<https://github.com/fricas/fricas/commit/5147241e890e8e93c47424912342477d55253be2>`_.

To support a weak form of literate programming, the two files

* `literatedoc.sty
  <https://github.com/hemmecke/fricas/blob/master-hemmecke/src/doc/literatedoc.sty>`_


* `literatedoc.awk
  <https://github.com/hemmecke/fricas/blob/master-hemmecke/src/doc/literatedoc.awk>`_

from the `master-hemmecke
<https://github.com/hemmecke/fricas/tree/master-hemmecke>`_ branch are
provided.
::

  awk -f litdoc.awk foo.spad > foo.tex
  pdflatex foo.tex

Whitespace
----------

Do not use tabs, but rather explicit spaces.

Block indentation
`````````````````

4 spaces (no tabs)

Whitespaces around punctuation
``````````````````````````````

- ":" space before and after

- "," space after (but not before)

- ";" space after (but not before)

- '+' spaces optional for most arithmetic infix operations

- "-" no space after for unary operations


Continuation lines
------------------

explicit ``_`` (underscore) even when not required

Function signature
------------------

explicit types, that is::

  func(a : Integer) : Integer ==

rather than::

  func(a) ==

Empty list
----------

use []$T rather than empty()$T

=> (early exit)
---------------

The ternary operator ``cond ? a : b`` known from the C programming
language can be encoded in SPAD as follows.

Either::

  e := (v > 0 => 1; -1)

or::

  e := if v > 0 then 1 else -1

Dangling ``else``
-----------------

If there is space then the best option is all on one line::

  if .... then .... else ....

Otherwise it should be like this::

      if .... then
        ....
      else
        ....

Or is this a valid option?::

  if ....
    then ....
    else ....

elt vs. qelt
------------

I prefer better error messages over speed.

_+ vs. "+"
----------

Prefer the escaped version of an operator instead of letting it look
like a string, i.e. use::

  _+(a : %, b : %) : Boolean ==

instead of::

  "+"(a : %, b : %) : Boolean ==

Boolean valued functions
------------------------

Functions that return boolean values have names that end in ``?``.
Rather define::

  positive? : Integer -> Boolean

instead of::

  isPositive : Integer -> Boolean

Destructive operations
----------------------

Identifiers of functions that modify their arguments should be ended
with an exclamation mark (``!``) to remind other programmers that they
should be very careful in using such functions.

Compare ``reverse`` with ``reverse!``.

Non-public constructors
-----------------------

Constructors starting with "Inner" are meant for library developers
but not for end-users.

Calling unary functions
-----------------------

Use::

  foo arg

instead of::

  foo(arg)

for unary functions if the argument is simple.
