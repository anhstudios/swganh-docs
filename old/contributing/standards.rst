Coding Standards
================

These are a list of things to keep in mind, check for and apply to code you're writing for the SWG:ANH application. Keeping to this will help make it easier for everyone who works on the codebase to identify what is going on in the code you've written. These are also points that all reviewers should be applying to incoming code before it hits the develop line. Sharing these changes back with the developer is vital to improving everyone's common working knowledge.

.. note:: When going through code it's very, very tempting to start applying this to everything, including sections outside the area you are currently giving attention to or reviewing. Do not give into this temptation as it will lead off into unknown territory and also cause the diffs to be even more crazy than normal. All code will get it's time in the light, be patient and stay the course because it's slow and steady that wins this race.

Follow the agreed upon style guide
----------------------------------

When writing source or accepting it in ensure that it fits with the agreed upon style guide, a variation of the `Google C++ Style Guide`_. The following are where the ANH guide deviates from above:

* Exceptions are allowed
* C++0x libraries are allowed (and encouraged)
* Enums must be ALL_CAPS_WITH_UNDERSCORES
* Non accessor/mutator functions should be camel case starting with a lower case, non-numeric character
* Documenting comments in headers should follow the Doxygen format
* Indent by 4 spaces instead of 2
* No indentation before public, protected, private

Order includes appropriately
----------------------------

See the Google C++ Style Guide `section`_ on this topic for further information on the how and why behind this item.

Prefer standard algorithms to hand-written loops
------------------------------------------------

Hand written loops are often more verbose and slower than solutions that use the standard library algorithms, particularly when given the addition of lambdas in the latest standard. Always keep a copy of the `latest standard`_ handy, particularly section 25 on the Algorithms library.

When you come up against a hand crafted loop consider: 

*   What does this loop currently do?

    It is important when reviewing a piece of code to truly understand what it is currently doing. When looking at a fresh piece of code this is the most objective question to ask and should be the first one asked.

*   What is it trying to achieve?

    With an understanding of what the code is actually doing take a step back and look at what the code is doing in the context of the surrounding code and ask: What problem was the original author trying to solve? Compare the answer to this with what the code is currently doing. If they do not match roll up those sleeves and get to cleaning!

*   Can the intention and execution be improved by using a standard algorithm?

    How hard was identifying the intention and behavior of the code? If it was difficult to infer what the code was doing from a simple reading chances are it needs to be refactored. Consult the standard's algorithm section to see if there are any solutions to help. 

Simplify overly verbose logic
-----------------------------

Take the time to fully understand the purpose of any function/method you are currently looking at. Once you know why it exists take a closer look at how the problem was originally solved. Often times our legacy code was written on code binges where constant tweaks were made to get things working and then never reviewed before making it into the main source line. There are many places still where hundreds of lines of code can be shrunk to tens of lines (or less).

Add missing API documentation
-----------------------------

Before the start of this initiative there existed almost no API documentation. In order to change this and build up a useful base of documentation whenever entering into an existing function/method take the time to understand what it's purpose is and then add a Doxygen style doc above its declaration.

Remove commented sections of code
---------------------------------

If code shouldn't be there, remove it. Don't comment on how silly it is and leave it there as a testimony to the ignorance of others... nobody really cares. Just remove the code and clean up our source. And don't worry about losing any information, our projects and all their history are always available thanks to Git, just browse a files history to see its previous states.

.. _`Google C++ Style Guide`: http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml
.. _`section`: http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml?showone=Names_and_Order_of_Includes#Names_and_Order_of_Includes
.. _`latest standard`: http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3242.pdf