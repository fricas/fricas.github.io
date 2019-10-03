.. _fricas-mirror:

FriCAS git repository (svn-mirror)
==================================

We describe here how one can setup a mirror of the official
sourceforge repository of FriCAS. Since this is time consuming and
only necessary to do once, the text describing the setup is here only
as a record of how to reproduce it. Don't consider doing this
yourself. It's a waste of time. Rather follow
`How to work with the FriCAS mirror`_
in order to more quickly setup a git repository
that has the same properties as the one you get here.

Since 07-May-2014, the official git repository (mirror of the official
SVN sourcforge repository) is `https://github.com/fricas/fricas
<https://github.com/fricas/fricas>`_.


Relocation of the FriCAS SVN repository
---------------------------------------

31-Mar-2013: Due to SourceForge Project Upgrade (`see mail archive
<https://groups.google.com/forum/?hl=en&fromgroups=#!topic/fricas-devel/JZMu4iO1M1w>`_),
the git repository had to be relocated to the new location. The chosen
method was to use "rewriteRoot" as described at
`https://git.wiki.kernel.org/index.php/GitSvnSwitch
<https://git.wiki.kernel.org/index.php/GitSvnSwitch>`_.


Pros
~~~~

    The method is much simpler then what is described at
    `http://stackoverflow.com/questions/268736/git-svn-whats-the-equivalent-to-svn-switch-relocate
    <http://stackoverflow.com/questions/268736/git-svn-whats-the-equivalent-to-svn-switch-relocate>`_.
    It allows for recreation of the git repo from scratch by using:
    ::

       git svn init --rewrite-root https://fricas.svn.sourceforge.net/svnroot/fricas ...

    See below.

Cons
~~~~

    The ``git-svn-id`` will no longer point to the actual SVN
    repository place, but from now on must be considered as just an
    identifier.

Recreation of the git repository
--------------------------------

30-Sep-2019: Two errors in the current git repository have been
detected.

    * The SVN commit r1922 from ``2015-07-26 16:41:45`` was
      mysteriously recorded in the git repository as
      ``2015-07-16 16:15:34``.

    * The SVN commits
      ::

         r2371 2018-03-05 11:14:28 Improve Texmacs interface

      and ::

        r2372  2018-03-05 17:20:56 Fix 'write' to Postscript file

      appeared together in the git repository in `eaeb4f93
      <https://github.com/fricas/fricas/commit/eaeb4f937325a84d5c297fe973dcc6ee8f84b591>`_.

Since we want to keep the history (at list of the mainline) intact and
since ``hemmecke`` now wants to be listed with ``ralf@hemmecke.org``
as e-mail address, it has been decided to rebuild the whole git
repository.

In fact, the new repository has identical hashes with the old
(corrupted one) up to `996bb9d0
<https://github.com/fricas/fricas/commit/996bb9d0b304aab140481298a329451ad8d7d4e0>`_
(r287) and (if only the mainline is considered) even up to `03e23911
<https://github.com/fricas/fricas/commit/03e23911cb2c79507892f15d21982311f442340f>`_
(r413).

Creating a mirror of FriCAS from scratch
----------------------------------------

*This step is only necessary, for creating a git repository that is
identical to the one currently at github if for some reason the*
`https://github.com/fricas/fricas <https://github.com/fricas/fricas>`_
*repository is corrupt.*

*However, the errors that are currently still part of the git
repository and marked with* ``wrong/`` *will be lost.*

First create a directory ``fricas``.
::

   mkdir -p $HOME/v/autosync/fricas
   cd       $HOME/v/autosync/fricas

From now on the steps are done inside a directory called "fricas".
::

   # original URL (now only used as idenifier)
   GITSVNID=https://fricas.svn.sourceforge.net/svnroot/fricas
   # new URL after relocation
   SVNREPO=svn+ssh://hemmecke@svn.code.sf.net/p/fricas/code # read+write
   # SVNREPO=svn://svn.code.sf.net/p/fricas/code # read-only

Since the directory layout of FriCAS is non-standard, we must specify
the layout for ``git-svn`` to work correctly.
::

   LAYOUT="--prefix svn/ -T trunk -t releases -b branches"
   git svn init $LAYOUT --rewrite-root $GITSVNID $SVNREPO

Configure the initial ``authors.txt`` file that contains all people
that have committed to the SVN repository.
::

   cat <<'EOF' > .git/authors.txt
   billpage = Bill Page <bill.page@newsynthesis.org>
   cerambyx = Franz Lehner <cerambyx@users.sourceforge.net>
   cahirwpz = Krystian Bacławski <krystian.baclawski@gmail.com>
   hemmecke = Ralf Hemmecke <ralf@hemmecke.org>
   mantepse = Martin Rubey <mantepse@users.sourceforge.net>
   oldk1331 = Qian Yun <oldk1331@gmail.com>
   whebisch = Waldek Hebisch <hebisch@math.uni.wroc.pl>
   EOF

   git config svn.authorsfile .git/authors.txt

First create a clone of the SVN repository on your local computer.
That will take a while until all commits are transmitted from
SourceForge.
::

   git svn fetch

Create a new project ``fricas`` at `github.org
<https://github.org>`_ and push the repo to github. (Of course you
have to replace the account name ``fricas`` by your github account.)
We just make a one-to-one copy (mirror) of the local repository from above.
::

   git remote add origin git@github.com:fricas/fricas.git
   git push --mirror

`Ralf Hemmecke <http://hemmecke.org>`_ has set up the above mirror so
that it will be automatically synchronized with the FriCAS sourceforge
SVN repository in not more than one hour after the commit mail is
received on his email account. Actually, it should be immediate.

In fact, update mechanism only keeps the ``master`` branch in sync
with the SVN ``trunk``. The tags for the releases have to be created
and pushed manually to github.


How to work with the FriCAS mirror
----------------------------------

In order to work with the fricas source code you will basically have
two repositories, a public one at github (which is a fork of the
above `http://github.com/fricas/fricas
<http://github.com/fricas/fricas>`_) and a private one on your local
computer.

So from your point of view there are 3 repositories:

    #. git@github.com:fricas/fricas.git (ORIG)
    #. git@github.com:YOURNAME/fricas.git (PUB)
    #. The repository on your local computer (LOC)

After you have created PUB from ORIG, and LOC from PUB, you basically
only work on LOC and publish your changes (i.e. ``git push``) to PUB
if you feel that others should see your code. Other people can then
get (``git fetch``) your code from PUB into their respective local
repository. Yes, it's called distributed version control. You have the
full history on your local computer.

Here is how to create PUB and LOC.

    #. Create an account at https://github.com.
    #. Fork the "official" FriCAS mirror at github, i.e. create your
       PUB.

       #. Log into github.

       #. Go to `http://github.com/fricas/fricas <http://github.com/fricas/fricas>`_
          and press the "Fork" button.
          If all goes well you will now have a clone at
          ``http://github.com/YOURNAME/fricas``.
    #. Create a clone of your github repository on your local
       computer, i.e. create your LOC.
       ::

          git clone git@github.com:YOURNAME/fricas.git

       You see a subdirectory "fricas" which contains an (offline)
       history of FriCAS sourceforge.net "trunk" in a git repository.


    #. In case you like to see all branches from the FriCAS
       sourceforge repository... (This step is optional.)
       ::

          cd fricas
          git remote add upstream https://github.com/fricas/fricas.git

       Your repository is now connected to your github clone (to which
       you can push, i.e. write to) and the "official" FriCAS mirror
       (which is read-only for you).

       Get all the other SVN branches and tags... (Note that here you
       do not connect to Sourceforge, but rather to Github.)
       ::

          git fetch upstream

    #. The (remote) branch "upstream/master" will be identical to the
       "trunk" from the FriCAS sourceforge SVN repository. Check with
       ::

          gitk --all

       to see under "upstream/master" what the current latest
       official FriCAS development line is. You simply call
       ::

          git fetch upstream

       in order to update "upstream/master".

    #. (Optional) You can create a local branch "trunk" which tracks
       the "master" branch from upstream (i.e. "fricas" on github).
       Your "trunk" branch will mirror the current "trunk" from the
       FriCAS sourceforge SVN repository.
       ::

          git branch trunk --track upstream/master

       You can bring your local "trunk" branch in sync with the
       "master" branch from the FriCAS github repository as follows.
       ::

          git checkout trunk
          git pull upstream master     # or simpy:   git pull
          git checkout YOUR-PREVIOUS-BRANCH

       If you have currently some uncommitted changes, you might
       consider wrapping the above commands with ``git stash`` and
       ``git stash pop``. Always check with
       ::

          gitk --all

       what the current situation of your repository is.

    #. Now you have the full power of git at your disposal. Just hack
       your code into your copy of the fricas sources commit as often
       as you like locally and when you think that others should see
       your code, just ``git push`` it to github. You should never
       commit to your local "trunk" branch but rather keep it in sync
       with SVN "trunk", i.e. "upstream/master". Synchronization can
       simply be done by a ``git pull``, since you have setup the
       trunk branch as a tracking branch.

    #. New work should be developed on a branch and should be made
       publicly available at github. Unless you have several features
       that you develop at the same time, you probably want to develop
       on your master branch (which should be identical to
       "origin/master", i.e. the "master" branch on your github
       repository). For maximal flexibility, below the new branch name
       "foo" is used. Since "master" is not really a particular branch
       name for git, you can also replace "foo" by "master".

       Suppose you want to add a new feature to FriCAS, then you
       should create a new branch "foo" pointing to "upstream/master".
       Don't forget to update "upstream/master" (or optionally
       "trunk") as in mentioned above, before you create the new
       branch.
       ::

          git branch foo upstream/master
          git checkout foo

       If the branch "foo", already existed before, then you can force
       to create the branch here. Be sure that you know what you are
       doing when you use the ``-f`` flag.
       ::

          git branch -f foo

       Now you start editing and committing. If you are finished you
       can push your changes back to github. Then tell people about
       your new commit.
       ::

          git push origin foo

       In case git tells you that your push did not succeed then you
       most probably already have a branch "foo" on github and the
       push would mean that your github "foo" branch is not in the
       history of your new "foo" branch, i.e. it is not a fast-forward
       commit. If you know what you are doing, you can still force the
       push with the ``-f`` flag.
       ::

          git push -f origin foo

    #. The foo branch can be removed (locally and on github) if your
       changes have made it into the official FriCAS (SVN) repository.
       ::

          git branch -D foo
          git push origin :foo

       Note the colon in front of "foo" in the last command. You should
       (of course) not delete your "master" branch.

    #. It might happen that your changes collide with changes done on
       "trunk". In that case, it is best to rebase your branch on top
       of the latest "trunk" ("upstream/master"). For that you first
       commit all changes, fetch the newest commits from
       "upstream/master". Then rebase you branch on top of
       "upstream/master".


Collaborating with other people
-------------------------------

Once you have PUB and LOC, you can easily share patches with other
people. You simply have to add another remote repository and then
fetch the data from that other repository.
::

   cd fricas
   git remote add foo git://github.com/foo/fricas.git
   git fetch foo

Now you have the branches of "foo" in you local repository and can
investigate them. Use ``gitk --all`` in order to see what "foo" has
done. Of course you could already investigate everything directly on
github, but having the published work of "foo" locally in your
repository allows you to git merge or git rebase them to your own
branches.


Special note to FriCAS developers with write-access to the FriCAS sourceforge repository
----------------------------------------------------------------------------------------

After you have executed the commads described in `How to work with the
FriCAS mirror`_, you can connect your local git repository back to
Sourceforge so that you will be able to commit directly to the SVN
repository. Just go to your cloned repository and make sure you have
executed step 4 from above, i.e. you got everything from "upstream".
Then execute the steps starting with ``git svn init ...``. This may
take some time. (About 5 min or less and definitely less than it took
me to import the FriCAS history in the first place. Actually, it
depends on the time when you contact sourceforge. So try later if it
takes too long.)

Look at the documentation of `git svn
<https://git-scm.com/docs/git-svn>`_ in order to figure out how ``git
svn dcommit`` will commit back to sourceforge. (You should use this
command with the option ``--dry-run`` first.)

Look at `Collaborating with other people`_ to get patches of others
from github. In order to commit them to "trunk", just ``git
cherrypick`` the corresponding patches (easily done with ``gitk
--all`` as a GUI, just right-click on a commit). Or ``git rebase``
them onto the "upstream/master" branch (which should be identical to SVN
"trunk").

Assume person ``foo`` has asked you to incorporate his patches that
live in his branch "new-foo" into the main trunk at sourceforge. You
would do the following steps.

#. If you have uncommitted changes, save them for later. Let's assume
   you are on the branch ``my-branch``.
   ::

      git stash

#. Make sure trunk is up-to-date.
   ::

      git checkout trunk
      git pull
      git svn rebase

   incorporate foo's repository as described in
   `Collaborating with other people`_.
   ::

      # Don't do the following command, if foo appears in the output of "git remote -v".
      git remote add foo git://github.com/foo/fricas.git
      # Now fetch from foo.
      git fetch foo

#. Then checkout the relevant branch and rebase it onto your trunk.
   ::

      git checkout new-foo
      git rebase trunk

#. If everything went fine (no conflicts), you can now commit back to
   sourceforge. But first check with the -vn flags what will actually
   be transmitted to sourceforge and check that it will be committed
   at the trunk.
   ::

      git svn dcommit -v --dry-run
      # If everything is fine then remove "--dry-run"
      git svn dcommit -v

#. Now you can go back to where you interrupted your work and continue
   on "my-branch" with the changes that you stashed away in the
   beginning.
   ::

      git checkout
      git stash pop



Relevant links
--------------

* http://git.wiki.kernel.org/index.php/GitFaq#How_do_I_mirror_a_SVN_repository_to_git.3F
* http://wiki.gnucash.org/wiki/Git

If you feel like you want to know more about the internals of git read
the article that is linked from
`http://www.newartisans.com/2008/04/git-from-the-bottom-up
<http://www.newartisans.com/2008/04/git-from-the-bottom-up>`_. Getting
more information about version control with Git

There is a lot of git documentation in the net. Usually using google
with something like "git add", "git commit", "git push", will lead you
to an appropriate help page. Otherwise there is

* `http://git-scm.com/ <http://git-scm.com/>`_
* `http://gitready.com/ <http://gitready.com/>`_
* `Use gitk to understand git <http://www.lostechies.com/blogs/joshuaflanagan/archive/2010/09/03/use-gitk-to-understand-git.aspx>`_
* `Git Cheat Sheet <http://cheat.errtheblog.com/s/git>`_
* `Git Wiki <https://git.wiki.kernel.org/index.php/Main_Page>`_
