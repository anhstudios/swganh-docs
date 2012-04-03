===========================
Documentation Install Guide
===========================

This guide is a walkthrough on setting up the SWGANH documentation to build as part of an existing SWGANH environment. For more information on how to set up SWGANH please see the relevant documentation.

Sphinx
~~~~~~

Sphinx is used to generate the SWGANH developer documentation. 


Sphinx on Windows
-----------------

To install it on windows you need to first download and install the distribute package. Git Bash is recommend over the windows command prompt to run the following as it already comes with the **curl** utility.

::

    curl -O http://python-distribute.org/distribute_setup.py
    python distribute_setup.py
    easy_install Sphinx
    
.. NOTE::
    
    It is important that Sphinx is installed with support for Python3. The key to this is ensuring you install Sphinx using an easy_install (via the distribute package) that was built against Python 3.
    
.. WARNING::

    If you get an error stating "Bad file number" it may indicate a confict with UAC. To get around this open a normal windows command prompt and enter the command.
    
    ::
    
        easy_install Sphinx
    
You will also need to add the path to Sphinx to the system PATH. In a default Python 3.2 install this path is `C:/Python32/Scripts`.

Sphinx on Linux
----------------

To install it on Ubuntu use the following command:

::

    sudo easy_install3 Sphinx
    
.. NOTE::
    
    It is important that Sphinx is installed with support for Python3. The key to this is ensuring you install Sphinx using an easy_install (via the distribute package) that was built against Python 3.


Checkout and Build the Documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Right click on the swganh project directory (e.g. `C:/workspace/swganh`) and choose "to open "Git Bash here". Next we'll use git to checkout the latest version of the documentation source.

::

    git clone https://github.com/anhstudios/swganh-docs.git docs

Run the following commands to generate the project and build the source with the documentation.

::

    cd build
    cmake ..
    cmake --build .

This will kick off a full build of the project. The final documentation output can be found at `C:/workspace/swganh/build/docs/html/Debug`.

