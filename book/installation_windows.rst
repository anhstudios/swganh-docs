=====================
Windows Install Guide
=====================

The most common platform for building and developing SWGANH has been Windows and this chapter covers the easiest way to get started. The SWGANH project is developed on modern technologies and as such has a few minimum version requirements to be mindful about. Most notable is the minimum supported windows version, Vista.

Installing Build Dependencies
-----------------------------

The following is a complete list of the 3rd party dependencies that will need to be installed before building the SWGANH source.

- Microsoft Visual Studio 11.0 Beta+
- CMake 2.8.7+
- Git 1.7+
- Python 3.2
- Mysql 5.1+
- SWGANH Dependencies Package
- SWGANH Game Client

Visual Studio 11.0 Beta
~~~~~~~~~~~~~~~~~~~~~~~

When installing Visual Studio, be sure the version you are installing is at least the Beta version and **NOT** the Developer Preview. The Beta provides an opportunity to use the best tools Microsoft has to offer, so download the Ultimate edition.

    http://www.microsoft.com/visualstudio/11/en-us/downloads#vs

CMake 2.8.7+
~~~~~~~~~~~~


CMake is used to generate the project files for SWGANH source builds. When installing be sure to select the option to add CMake to the PATH.

    http://www.cmake.org/cmake/resources/software.html

Git 1.7+
~~~~~~~~

Git is used to check out and keep your local source in sync with the latest SWGANH chanages. We recommend installing the GitExtensions package which, in addition to the normal git utilities, provides nice GUI.

While the Git Extensions package be sure to select the "msysgit" option to install the base Git utilitites. It is also advised that you select the option to add Git GUI and Git Bash to the context menu (right click). This makes it easy to right click on a directory and open the git gui or git bash prompts.

    http://code.google.com/p/gitextensions/downloads/list

Python 3.2+
~~~~~~~~~~~

Python is used for scripting as well as for generating the SWGANH documentation. After downloading and installing the latest Python 3.2 x86 binaries you will need to add the path to python to the system **PATH**. In a default Python 3.2 install this path is `C:/Python32`.

    http://python.org/download/

.. note::

    Be sure to install the x86 (32bit) binaries and *NOT* the x86-64 (64bit) binaries, it would be bad.
    
Mysql 5.1+
~~~~~~~~~~
    
Mysql is used for database storage in the ANH project. This guide only covers the simplest of installation instructions, other more in-depth topics on database management may be provided in another document in the future. 

To start, download and install the latest Mysql Installer. Select the default developer installation which will provide you with all the tools needed to run and manage the SWGANH database. When the installation is complete you will need to add the path to mysql to the system PATH. In a default mysql installation this is `C:/Program Files/MySQL/MySQL Server 5.5/bin`.

    http://www.mysql.com/downloads/installer/

Setting Up the SWGANH Environment
---------------------------------

Now it's time to get the SWGANH environment set up and ready to begin building the source. First create a directory to hold all of your SWGANH related files. In this example we will use `C:/workspace` as our base directory.

SWGANH Dependencies Package
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Windows environment gives us the opportunity to prebuild the libraries that you will need to build the SWGANH source. Download the latest Visual Studio 11 dependencies from the official SWGANH downloads page and unpack the contents into the workspace directory you created in the previous step. Afterwards, the directory structure should be as follows: `C:/workspace/vendor`

    https://github.com/anhstudios/swganh/downloads

SWGANH Game Client
~~~~~~~~~~~~~~~~~~

The SWGANH Game Client can be installed from the unofficial SWGANH client installer.

    https://github.com/downloads/anhstudios/swganh/anhclient_setup.exe
    
.. note::

    The installer at this time requires a valid Star Wars Galaxies game client installation. In the future there will be an official installer that forgoes this requirement.

Checkout and Build the Source
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Finally we get to the good stuff! Right click on the swganh workspace directory (e.g. `C:/workspace`) and choose "to open "Git Bash here". Next we'll use git to checkout the latest version of the source.

::

    git clone https://github.com/anhstudios/swganh.git

Run the following commands to generate the project and build the source.

::

    cd swganh
    git submodule update --init
    mkdir build
    cd build
    cmake -G "Visual Studio 11" ..
    cmake --build .

.. WARNING::
    If you get an error about cmake not being able to find your PYTHON_LIBRARY. re-run the cmake -G command above adding in the following:
    cmake -G "Visual Studio 11" -DPYTHON_LIBRARY="LOCATION_TO_PYTHON_DIR/libs" ..
    where "LOCATION_TO_PYTHON_DIR" is where your Python32 folder resides. This seems to occur if Python is installed in Program Files x86..

This will kick off a full build of the project. The final output can be found at `C:/workspace/swganh/build/bin/Debug`.

.. note::

    The Visual Studio solution can be found at `C:/workspace/swganh/build/swganh.sln`. Use this to modify and build changes to existing source files.

.. note::
    
    Since the project files are located outside the source directory adding new files from within visual studio requires changing the default save location.
    
    To add a new file, manually create it in the src directory and then run the following from within the build directory.

    ::

        cmake ..
        
.. note::

    Documentation can be found in the `C:/workspace/swganh/build/docs/html/Debug` directory. Just open the **index.html** file in your favorite browser.
        
Setting up the Database
~~~~~~~~~~~~~~~~~~~~~~~

A new database installation is needed before the server can be started for the first time. To install the server navigate to the `C:/workspace/swganh/data/sql` folder and copy the **setup.cfg-example** file to **setup.cfg**. Edit this file with the appropriate login information for the Mysql server you intend to use.

.. NOTE::

    Be sure to copy and **NOT** rename the setup.cfg-example file, lest you accidently try to remove it from the source on your next commit.
    
.. NOTE::

    You can use the root user for simple local installations, however, it is advised that you create a dedicated mysql user for your SWGANH installation in production environments.

Next double click the setup.bat script. This will open up the database installer. Choose option #1 for a complete installation by typing 1 and hitting enter. Once this process completes you can quit the installer.

Configuring and Running the Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You are now entering the home stretch, all that's left is to update the SWGANH configuration and kick off the server.

Open the `C:/workspace/swganh/build/bin/Debug/config/swganh.cfg` file and edit the following items. First you will need to update the **tre_config** setting with the path to the **live.cfg** file in your SWGANH Game Client directory.

.. note::

    Some older SWGANH clients have this file named as **swg2uu_live.cfg**.
    
.. warning::

    Be sure to specify the live.cfg file that is **inside** the SWGANH Game Client directory and **NOT** the one inside the official Star Wars Galaxies directory.

Second, update the mysql database connection information with the address and user you used to setup the database in the previous section.

Finally, set the address in the **service.connection** section to your public facing IP and then save and close the file.

You can now kick off the server by running the **swganh.exe** at `C:/workspace/swganh/build/bin/Debug/swganh.exe`.
