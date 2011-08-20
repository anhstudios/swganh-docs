.. index::
    single: Plugins; Best Practices

Plugin Structure and Best Practices
===================================

The SWG:ANH plugin system has been designed to be easy to use and simple to 
adapt to most situations. While creating a new plugin is not difficult, 
following a few best practices will make things easier for the future 
maintainers of your plugin (including yourself!).

 
Directory Structure
-------------------

.. code-block:: text

    XXX/...
        my_plugin/
            CMakeLists.txt
            LICENSE
            main.cc
            resources/
                config/
                    my_plugin.cfg.dist
                doc/
                    index.rst
                    
The ``XXX`` directory(ies) reflects the namespace structure of the plugin.

The following files are mandatory:

* ``CMakeLists.txt``: Describes how to build the plugin;
* ``main.cc``: Entry-point for the plugin;
* ``LICENSE``: The full license for the plugin code;
* ``resources/config/my_plugin.cfg.dist``: The default configuration file for your plugin;
* ``resources/doc/index.rst``: The root file for the plugin documentation.

.. note::

    These rules are in place to ensure that the build system and other 3rd party
    tools can rely on this default structure to perform their work.

