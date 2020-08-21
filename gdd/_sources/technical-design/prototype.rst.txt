.. index:: throwaway prototype

#########################
Prototype Lessons Learned
#########################
The development team is new to the Rust game development ecosystem; the
:ref:`ref-objective-build-risk-reduction-prototype` objective allowed the team
to explore various techniques and come up with a design that fulfills the game
requirements listed in this document.

The game's design is influenced by the knowledge learned during the prototyping
phase. This section describes the knowledge gathered during the prototyping
phase including why Amethyst was chosen as the engine to base the game's design
around.

.. index:: Piston game engine, ggez game engine, Amethyst game engine

=================
Rust Game Engines
=================
While doing research for the prototype three popular Rust game engines were
considered: `Amethyst v0.15.0 <https://github.com/amethyst/amethyst/tree/v0.15.0>`__,
`ggez v0.5.1 <https://github.com/ggez/ggez>`__, and
`Piston v0.48.0 <https://github.com/PistonDevelopers/piston>`__. An overview of
these engines with regards to how their features meet FossXO's requirements
is provided in :numref:`table-rust-engines-comparison` . [#enginedisclaimer]_

..  tabularcolumns:: |l|L|L|L|
..  _table-rust-engines-comparison:
..  list-table:: Comparison of Rust game engines with regards to meeting FossXO's requirements.
    :header-rows: 1

    *   - Feature
        - Amethyst
        - ggez
        - Piston
    *   - crates.io downloads
        - 50k
        - 45k
        - 190k
    *   - GitHub stars
        - 5.9k
        - 2.4k
        - 3.5k
    *   - Tagged releases
        - Yes
        - No
        - No
    *   - Usage Model
        - Complete engine for games large and small
        - Complete engine for basic games
        - Loose collection of libraries
    *   - Getting Started Guide
        - Excellent
        - Ok
        - Disorganized
    *   - API Docs
        - OK
        - Excellent
        - Ok
    *   - Examples
        - Excellent
        - Excellent
        - Poor
    *   - GUI Widgets
        - Basic
        - No
        - Full support via conrod
    *   - Wayland support
        - No
        - No
        - Yes
    *   - Test support
        - Yes
        - No
        - Unknown
    *   - Camera System
        - Yes
        - No
        - Unknown
    *   - Sprite Drawing
        - Yes
        - Yes
        - Unknown
    *   - Vector Drawing
        - No (Debug lines only)
        - Yes
        - Yes
    *   - Audio / Sound FX
        - Yes
        - Yes
        - Unknown
    *   - Animation System
        - Yes
        - No
        - Unknown
    *   - Special FX
        - Lighting
        - Can create simple custom shaders and compose multiple buffers
        - Unknown
    *   - Particle Systems
        - No, but very easy to add new systems
        - No
        - Unknown
    *   - Keyboard / Mouse
        - High level binding system in addison to basic access
        - Basic access
        - Unknown
    *   - Resource Management
        - Full resource system
        - Basic loading of resources
        - Unknown

Amethyst, ggez, and Piston are all built on top of a similar collection of
underlying Rust libraries that handel things such as graphics, keyboard and
mouse support, and audio. However, the engines different in the API and
abstractions created around these libraries.

------
Piston
------
Piston was the first engine considered for FossXO and is one of the most
downloaded game engines from crates.io. However, its documentation and design
as a loose collection of libraries made it hard to use as a new Rust programmer.

This was especially true when looking at Piston's examples. Different examples
would do basic tasks like create a window in vastly different ways leading to
confusion on why these tasks needed to be done differently or if one way was
preferred over another.

Because of Piston's complexity it was ruled out for use in FossXO.

----
ggez
----
ggez's goal is to allow developers to make simple games quickly. Additionally,
it provided good getting started documentation and an excellent set of examples.
Additionally, ggez's core set of features work well for FossXO's requirements.
E.g. the easy of use of rendering lines, textures, and allowing multiple texture
buffers to be composed for creating special FX.

There were a few rough spots such as the programmer needing to know about
details of the underlying libraries used. ggez is also very opinionated about
where user data is saved. Additionally, ggez does not provide any UI widgets so
those would have to be created. However, overall ggez was a strong contender for
use with FossXO.

Unfortunately, the design phase of the project exposed a major flaw with ggez:
it does not provide any functionality or guidance on how to structure your game.
FossXO contains multiple environments, game modes, and menus. Trying to manage
the data, logic, relationships, and ownership of all these items quickly became
complicated. Being new to Rust and wanting to focus on making a game and not a
game engine it was decided to look elsewhere.

--------
Amethyst
--------
Amethyst is built around a entity component system which provides structure
on where all the pieces of the game go. Amethyst has excellent getting started
documentation and examples that demonstrate the engine. Additionally, Amethyst
provides basic UI widgets which would otherwise take a fair amount of time to
implement. [#expandable]_

Compared to ggez, Amethyst works at a higher level. This has a few down sides
such as it being harder, or at least not as oblivious, on how to do some basic
tasks such as drawing lines. However, its architecture allows different
rendering systems to be used or swapped in and out.

One more unique aspect about Amethyst compared to the other engines considered
it is the only one to tag its releases per the Rust API Guidelines. [#rustapiguidelines]_

Therefore, of the engines considered Amethyst aligned best with FossXO's
requirements and made the foundation for the team to build upon.


=======================
Entity Component System
=======================
The main feature that Amethyst provides over the other engines is its
entity component system. The ECS model works well with Rust's ownership model.
This greatly simplified the game's design.

Therefore, considering using a ECS architecture for future games even if not
using the Amethyst engine.


.. index:: Wayland

=============
Wayland Issue
=============
While working with both ggez and Amethyst an issue with Wayland was discovered
due to an issue in an underlying library. To resolve the issue set the
``WINIT_UNIX_BACKEND`` environmental variable to ``x11`` before creating the
game's window. :numref:`code-set-winit-unix-backend-envar` shows how to this
in code.

..  _code-set-winit-unix-backend-envar:
..  code-block:: rust
    :caption: Example of setting the WINIT_UNIX_BACKEND environmental variable.

    // Workaround for crash on Wayland.
    env::set_var("WINIT_UNIX_BACKEND", "x11");

For details see:

* https://github.com/ggez/ggez/issues/579
* https://github.com/rust-windowing/winit/issues/793


..  rubric:: Footnotes

..  [#enginedisclaimer] The comparison of Rust engines is based on a cursory
        examination of each engine with a focus on how they meet FossXO's
        specific requirements. The engines might support more features than
        described here and the feasibility findings might be different if
        evaluating the engines for a different application or game.
..  [#expandable] Amethyst makes it easy to add new systems and features which
        helps with the :ref:`ref-objective-easily-expandable-and-modifiable`
        objective.
..  [#rustapiguidelines] `Rust API Guidelines: Documentation
        <https://rust-lang.github.io/api-guidelines/documentation.html>`_
