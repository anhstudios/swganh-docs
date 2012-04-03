========
Services
========

Services are systems that are available the entire lifetime of the application. They also generally handle interactions directly with the SWG Protocol and they expose an API for controlling or accessing data that covers a feature set that can be used by all other services.

Services are generally broken up into game system categories, but they can also be more generic than that. For example there is a Login Service that handles all of the interaction between the Client Login Messages, the database and server itself. This service handles these messages and allow you to do what is necessary with the data. In most cases, this involves interacting with the database and sending a SWG Protocol Message reply back to the Client. Most services follow this basic pattern, but they are not restricted to it.
