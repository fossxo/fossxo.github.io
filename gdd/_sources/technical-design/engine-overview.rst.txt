###############
Engine Overview
###############

FossXO is developed using the Rust programming language per the
:ref:`ref-objective-create-ttt-game-with-rust` objective. To help reduce the
development effort, and avoid creating a game engine from the ground up, FossXO
built on top of the Amethyst game engine and makes extensive use of other
existing Rust libraries.

.. index:: Amethyst game engine, ECS, entity component system

========
Amethyst
========
FossXO uses `Amethyst v0.15.0 <https://github.com/amethyst/amethyst/tree/v0.15.0>`_
as the game's engine. [#otherengines]_  Amethyst is a data-driven game engine
written in Rust. Amethyst uses a entity component system
(ECS) architecture where entity represents a single object in the game.
Components store data about one aspect of the object. Systems contain logic that
is executed on collections of components every loop. Amethyst also
contains support for game states, resources, and events.

In addition to the ECS architecture, Amethyst provides a rendering engine that
supports various backends including Vulkan, a basic UI framework, audio support,
and IO utilities.

..  note::
    FossXO makes extensive use of the features provided by Amethyst. It is
    highly recommended to read the
    `Concepts behind Amethyst <https://book.amethyst.rs/stable/concepts/intro.html>`_
    chapter of The Amethyst Engine book ([Amethyst-book]_) that
    introduces the fundamental concepts in Amethyst that this game is built
    around. [#amethystguide]_

The :doc:`states`, :doc:`systems`, and :doc:`components-resources-entities`
sections describe the details of how the game is built on top of Amethyst.


.. index:: open_ttt_lib

============
open_ttt_lib
============
FossXO uses the ``open_ttt_lib`` library to manage the game rules and AI
algorithms per the :ref:`ref-objective-create-ttt-game-with-rust` objective.


..  index::
    fossxo crate, states module, systems module, environments module
    resources module, components module, events module, file_io module, ui module

=======
Modules
=======
Rust applications and libraries use modules to organize their functionality.
An overview of the game's modules is shown in :numref:`uml-modules-overview`.

..  _uml-modules-overview:
..  uml::
    :caption: FossXO's major modules and their relationships.

    !include rust_types.uml
    hide empty members

    package "<fossxo crate>" { }
    package states { }
    package systems { }
    package file_io { }
    package events { }
    package environments { }
    package components { }
    package resources { }
    package ui { }

    "<fossxo crate>" <-- states
    "<fossxo crate>" <-- systems
    states <-- file_io
    states <-- environments
    states <-- events
    states <-- resources
    states <-- ui

    systems <-- components
    systems <-- resources
    systems <-- events
    environments <-- components

A brief description of each module follows:

<fossxo crate>
    Contains the entry point of the application. This initializes the engine,
    parses command line arguments, loads any required data to start the game,
    starts the game's systems, and finally sets the first game state to run.
states
    Contains the game's states that are described in the :doc:`states` section.
systems
    Contains the game's systems that are described in the :doc:`systems`
    section. This also provides system bundles for convenient access to groups
    of systems.
environments
    Contains the game's environments including the :ref:`ref-environments-resource`.
resources
    Contains the games public resources, with the exception of the
    :ref:`ref-environments-resource`.
components
    Contains the game's components.
events
    All events sent by the game are contained in the ``events`` module. This
    includes the ``EventData`` enum in :numref:`uml-event-data-enum`.
file_io
    Holds functionality related to loading and saving the games custom files.
ui
    Provides support for creating and managing UI widgets.

Developers are encouraged to add additional modules if needed to help with the
maintainability of the project. [#utilsmodule]_


..  rubric:: Footnotes

..  [#otherengines] See the :doc:`prototype` section for why Amethyst was chosen
        over other popular Rust game engines.
..  [#amethystguide] Readers might also find this unofficial
        `Amethyst Architectural Guidelines <https://github.com/bonsairobo/amethyst-architecture-guidelines>`_
        `(archive) <https://web.archive.org/web/20200807215439/https://github.com/bonsairobo/amethyst-architecture-guidelines>`_
        useful, especially when designing game states, systems, components, and resources.
..  [#utilsmodule] One such additional module could be the ``utils`` module for
        holding functionality that is required by a many of the other modules.
