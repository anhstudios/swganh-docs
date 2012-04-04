=========================
Submitting a Pull Request
=========================

The SWGANH team accepts code contributions via the GitHub Pull Request system. This allows us a tight integration with our issue tracking functionality as the two are often heavily entwined. 

To submit a patch first you will need to get and build the SWGANH source code:

* Create and sign into a `GitHub`_ account.
* Fork the `SWGANH repository`_ by clicking the "**Fork**" button.
* After the forking process has completed, clone your fork:

.. code-block:: bash

    $ git clone git@github.com:USERNAME/swganh.git

* Add the official SWGANH repository as a remote named upstream:

.. code-block:: bash

    $ cd swganh
    $ git remote add upstream https://github.com/anhstudios/swganh.git

* Build the code using the :doc:`official install guide </book/installation>`.

Working on a Pull Requests
~~~~~~~~~~~~~~~~~~~~~~~~~~

Every pull request you create for a bug fix or new feature needs its own topic branch.

.. note::

    Until we reach a 1.0 release, all pull requests should be directed to the **develop** branch.
    
Create a topic branch with the following command:

    git checkout -b BRANCH_NAME develop

.. tip::

    Use descriptive names for your topic branches. Pull requests that fix a particular issue should be named issue/XXX where XXX is the issue number. Branches for new features should be named with a feature/ prefix (e.g., feature/guild_system).
    
The above will switch your working directory to the newly created branch. You are now ready to begin working on your issue or feature. Keep the following points in mind while working on code.

* Follow the :doc:`coding standards <standards>`.
* If possible, add unit tests to prove that the bug is fixed or that the feature works as specified.
* Commit often and use good commit messages.

Submitting a Pull Request
~~~~~~~~~~~~~~~~~~~~~~~~~

Before you submit a pull request make sure to pull in the latest changes from the main develop line. This is especially important for long running feature additions.

.. code-block:: bash

    $ git checkout develop
    $ git fetch upstream
    $ git merge upstream/develop
    $ git checkout BRANCH_NAME
    $ git rebase develop

While running a ``rebase`` you may encounter merge conflicts. Use the ``git status`` command to see the unmerged files. Resolve all of the conflicts and then continue on with the rebase:

.. code-block:: bash

    $ git add ... # add resolved files
    $ git rebase --continue
    
Check that all :doc:`tests <tests>` pass and then push your branch to your GitHub repository:

.. code-block:: bash

    $ git push origin BRANCH_NAME
    
You can now discuss your branch on the `SWGANH IRC`_ or the `forums`_. When you are ready you can make a pull request from your GitHub page using the **Pull Request** button.

Your pull request may generate feedback that requires additional work to your topic branch before it can be merged into the main development line. When pushing the requested changes back up to your GitHub repository be sure to rebase and not merge; then force the push back to origin:

.. code-block:: bash

    $ git rebase -f upstream/master
    $ git push -f origin BRANCH_NAME
    
.. warning::

    Always specify the branch name explicitly when doing a push -f (or --force) to avoid causing issues with other branches.
    
Sometimes you may be asked to "squash" your commits. This is common when there are a number of similar changes to many files. Squashing commits will turn many commits into a single commit. Squashing is accomplished via ``rebase``:

.. code-block:: bash

    $ git rebase -i head~3
    
The number 3 here must equal the amount of commits in your branch. After running this command a text editor will pop up and display a list of commits:

::

    pick 1a31be6 first commit
    pick 7fc64b4 second commit
    pick 7d33018 third commit

To squash all commits into the first one, remove the word "pick" before the second and last commits and replace it with the word "squash" or just "s". When you save git will start rebasing and once complete will ask you to enter a new commit message (by default this is set to a listing of messages from all the commits). When finished execute the push command.

.. code-block:: bash

    $ git push -f origin BRANCH_NAME

.. _GitHub: http://github.com
.. _SWGANH repository: http://github.com/anhstudios/swganh
.. _SWGANH IRC: irc://irc.swganh.org
.. _forums: http://swganh.com/forums

