Development
===========

Development is driven by the needs and wishes of the various
contributors.

Bugreports and patches should be sent to the FriCAS
`mailing list <https://groups.google.com/forum/#!forum/fricas-devel>`_.

In fact, all public discussion currently happens on that mailing list.

You can get the sources from the official SVN repository::

    svn co svn://svn.code.sf.net/p/fricas/code/trunk fricas

Alternatively, press the fork button on
`https://github.com/fricas/fricas <https://github.com/fricas/fricas>`_
and send patches in the form of Github patch links, for example, as in
`https://groups.google.com/forum/#!topic/fricas-devel/tU9DLT1lLes
<https://groups.google.com/forum/#!topic/fricas-devel/tU9DLT1lLes>`_

Code review policy
------------------

Normally patches should go to the mailing list
`fricas-devel@googlegroups.com
<https://groups.google.com/forum/#!forum/fricas-devel>`_ for review.

A patch should go in as reviewed---if there is need for change new
patch should go to the list.

Obvious things can go in without review. Really minor changes to
patches also can go without extra round of review.

Of course patches should satisfy technical requirements (pass test,
update documentation when apropriate, contain tests for changes).

Patches should be logical unit of change. Do not join unrelated things
unless it is a cleanup type patch that functionally should be a no-op.

OTOH do not split patches that implement some functionalty into small
steps---if several patches have a common purpose and there are
dependencies between them, then they probably should go in as one
patch. This is not a hard rule, it make sense to split really large
patches (say more than thousend lines) and sometimes part of the
functionality is ready and may be commited before the feature is
complete..

Contributors
------------

The following people have contributed to FriCAS since its fork from
Axiom in 2007.

* Krystian Baclawski

* Abhinav Baid
* Martin Baker
* Peter Broadbery
* Raoul Bourquin
* Frédéric Chapoton
* Tim Daly
* Gabriel Dos Reis
* David Einstein
* Bertfried Fauser
* Johannes Grabmeier
* Riccardo Guida
* `Waldek Hebisch <http://www.math.uni.wroc.pl/~hebisch/>`_ (lead developer)
* `Ralf Hemmecke <http://www.hemmecke.org>`_
* Rainer Joswig
* Franz Lehner
* Francois Maltey
* Piet van Oostrum
* Humberto Ortiz-Zuazaga
* Kurt Pagani
* Bill Page
* Alfredo Portes
* Arthur C. Ralfs
* Anatoly Raportirenko
* Renaud Rioboo
* Martin Rubey
* Grigory Sarnitskiy
* Aleksej Saushev
* Konrad Schrempf
* Alexander Solovets
* Gregory Vanuxem
* Stephen Wilson
* Qian Yun
