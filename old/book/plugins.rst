=======
Plugins
=======

In order to add a large amount of customizability and flexibility within the SWGANH server. The developers have decided to utilize the idea of loading in objects dynamically. This allows custom plugins to be created that tweaks functionality, utilizes a different data source, and more just with a simple tweak in the configuration file.

The plugin system provides a way to register how to create and destroy objects and then it uses registrations to create new objects on the fly. This registration is then available to the server at runtime and allows a quick and easy method to get objects as needed.
