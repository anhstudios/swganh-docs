===================
Linux Install Guide
===================

This chapter cover's the setup of SWGANH from source in a linux environment. This particular guide cover's installation using Ubuntu and should apply directly to most debian based flavors. Other flavors of linux should at minimum support GCC 4.6 and Python3.

.. note::

    This guide is based on Ubuntu 11.10. If you are building on another distro you may be required to install some of the 3rd party libraries manually.

Installing Build Dependencies
-----------------------------

The first step in a linux build is to ensure the major dependencies for building the project are installed and available.

- Git 1.7+
- GCC 4.6+
- CMake 2.8.5+
- Python 3.2+
- GLM Math Library 0.9.2+
- Boost 1.49+
- Boost-Log 1.1+
- TBB (Thread Building Blocks) 3.0+
- Turtle 1.1.1
- Mysql Server 5.1+
- Mysql Connector C++ (Custom ANH Fork)

In most cases modern linux distros will provide easily installable packages for most of the dependencies. First lets install the all the dependencies except for Boost, Mysql Connector C++ and Sphinx:

::

    sudo apt-get install g++ python3-dev libmysqlclient-dev mysql-server libtbb-dev libglm-dev git git-gui gitk subversion cmake make curl unzip libbz2-dev
    
.. NOTE::
    
    You may have noticed a few extra dependencies at the end, these are tools that'll make it easier to download and unpack files later on.

During the installation of the above you will be asked to enter a password for the root mysql user. Be sure to remember this password for later in the installation process.
    
The remaining dependencies require a little extra work to install properly on any linux distro. 

First up is Boost which needs to be linked specifically against Python3 so requires manual compilation and installation. In addition we make use of Boost-Log, a logging library that has been officially accepted into Boost but is currently in an incubation period. The interface is stable however and meets our requirments for logging.

Download the latest source packages for Boost and Boost-log (at the time of this writing that is 1.49 and 1.1, respectively).

::

    curl -L -O http://downloads.sourceforge.net/project/boost/boost/1.49.0/boost_1_49_0.tar.bz2
    
    svn co https://boost-log.svn.sourceforge.net/svnroot/boost-log/trunk/boost-log/
    
Next unpack both source packages and copy the Boost-log files into the main Boost package.

::

    tar -xvjf boost_1_49_0.tar.bz2
    
    cp -R boost-log/boost/log boost_1_49_0/boost/log
    cp -R boost-log/libs/log boost_1_49_0/libs/log
    
When configuring its important to specify the version of Python that Boost should compile against. To do that navigate to the boost_1_49_0 directory and run the following command.

::

    cd boost_1_49_0
    ./bootstrap.sh --with-python=python3.2

Now that the environment is ready you can build boost by simply running the b2 command.

::

    ./b2
    sudo ./b2 install

.. note::

    Due to a bug in the bootstrap.sh file the Python root is not detected. For builds using the Ubuntu package version of Python this is not a problem, however, if you have installed Python to a non-standard location then you may get compile errors. In that case open up the **project-config.jam** file and add the path to the Python root as in the example below.

    ::

        python : 3.2 : /usr ;
        
Turtle is a mocking library that is used for testing interactions between objects. This is a header only library and easy to install. Just download and unpack the library headers to the appropriate system path.

::

    cd ~
    curl -L -O http://downloads.sourceforge.net/project/turtle/turtle/1.1.1/turtle-1.1.1.tar.bz2
    sudo tar -xvjf turtle-1.1.1.tar.bz2 -C /usr/local
    
This leaves only Mysql Connector C++ left on our list. This project is the official C++ connector from Mysql, however, it doesn't appear to be in active development at this time. For this reason we have had to create our own fork to support a few features that are missing from the official source. First make sure you're not in the Boost directory from the previous step and run the following:

::

    git clone https://github.com/anhstudios/mysql-connector-cpp.git
    cd mysql-connector-cpp
    cmake .
    make
    sudo make install

Building the SWGANH Source
--------------------------

With the dependencies out of the way the hard part is over. It's now time to checkout the SWGANH source and kick off the first build. Make sure you're not still inside the Mysql Connector C++ directory from the previous section before running these commands.

::

    git clone https://github.com/anhstudios/swganh.git
    mkdir swganh/build
    cd swganh/build
    cmake ..
    make

Setting up the Database
-----------------------

A new database installation is needed before the server can be started for the first time. To install the server navigate to the `swganh/data/sql` folder and execute the following command:

::

    ./setup.sh -u MYSQL_USERNAME -p MYSQL_PASSWORD
    
Replace MYSQL\_USERNAME and MYSQL\_PASSWORD with the authentication data of a mysql user with appropriate privileges.

.. NOTE::

    You can use the root user for simple local installations, however, it is advised that you create a dedicated mysql user for your SWGANH installation in production environments.

.. NOTE::

    You can set a custom host using the -h flag (e.g. -h 192.168.0.100).

    The mysql executable can also be customized via the -m flag (e.g. -m mysql5).

Configuring and Running the Server
----------------------------------

You are now entering the home stretch, all that's left is to update the SWGANH configuration and kick off the server.

Open the `swganh/build/bin/config/swganh.cfg` file and edit the following items. First you will need to update the **tre_config** setting with the path to the **live.cfg** file in your SWGANH Game Client directory.

.. note::

    Some older SWGANH clients have this file named as **swg2uu_live.cfg**.
    
.. note::

    The game client does not run on unix environments, in this case the easiest solution is to upload a client directory from an already existing windows installation.
    
.. warning::

    Be sure to specify the live.cfg file that is **inside** the SWGANH Game Client directory and **NOT** the one inside the official Star Wars Galaxies directory.

Second, update the mysql database connection information with the address and user you used to setup the database in the previous section.

Finally, set the address in the **service.connection** section to your public facing IP and then save and close the file.

You can now kick off the server by running this command in the **swganh/build/bin** directory:

::

    ./swganh
    
To start the server in a background process you can use a tool like screen.

::

    screen ./swganh
    
Hitting **ctrl+a** then **ctrl+d** will disconnect from the screen session but will leave the server running in the background. You can rejoin the server to shut it down with the following command:

::

    screen -r
    
No output is sent to the console, you can view output from the server by viewing the log at `build/swganh.log`.
