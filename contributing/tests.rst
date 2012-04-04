===================
SWGANH Code Testing
===================

Before submitting a :doc:`pull request<pull_requests>`, you need to run the SWGANH test suite to check that your changes have not broken any existing code.

Running the Test Suite with Visual Studio
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To run the test suite in Visual Studio first open the project and then right click on the "RUN TESTS".

Running the Test Suite from the Command Line
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To run the test suite from the command line first build the project.

.. code-block:: bash

    $ cmake --build .
    
Next run the test suite:

    $ ctest -C Debug

Creating New Tests
~~~~~~~~~~~~~~~~~~

To create a new test first create a file to store the test in. All test files must be suffixed with ``_unittest.cc`` or ``_unittest.h``. Next create your test according to the `googletest`_ documentation. 

Finally, re-run the cmake command to generate the test project and then re-build and test the project:

.. code-block:: bash

    $ cmake ..
    $ cmake --build .
    $ ctest -C Debug
    
.. _googletest: http://code.google.com/p/googletest/wiki/Primer
