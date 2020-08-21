############
Introduction
############

========================
Purpose of this Document
========================
This is the FossXO game design document. This document describes in
detail the objectives, requirements, and design considerations of the game
providing a central location for this information. This is invaluable for
understanding the game's scope, planning the project milestones, and creating
the games assets.

Anyone who is involved with the game's development is encouraged to read this
document and keep a copy handy.

=================
Scope of the Game
=================
Tic-tac-toe is a game of strategy where two players, X and O, take turns placing
their mark in a 3 x 3 gird. The first player to get three marks in a row,
column, or diagonal wins the game.

FossXO is a unique take on the classic game of tic-tac-toe. Players of
all ages battle the computer or each other in a variety of stunning environments.
Each environment tells part of the story of tic-tac-toe from the past, present,
and future. Each environment has a strong visual theme and complementary
soundtrack.

FossXO is free, open-source, and no contains no annoying advertisements. It
runs on Windows, Mac, and Linux. The game launches Summer 2020.

=========================
Overview of this Document
=========================
This game design document contains chapters covering various aspects of the
game. [#rogers]_ This includes chapters for the :doc:`gameplay`,
:doc:`gui`, and various :doc:`environments`.

The :doc:`technical-design/index` chapter describe how the game is created.
This includes details of the game's software architecture and design.

Additionally, the :doc:`glossary` defines terms that are used throughout the
game design document.


==================
Project Objectives
==================
This section describes the project's primary objectives.


..  _ref-objective-create-ttt-game-with-rust:

---------------------------------
Create Tic-tac-toe Game with Rust
---------------------------------
The main deliverable of this project is a tic-tac-toe game for Windows, Mac,
and Linux. This project is the follow-up to the Ounce of Rust project [#ounceOfRust]_
that resulted in the creation of a Rust based tic-tac-toe logic library,
``open_ttt_lib``. [#openTTTlib]_

In addition to creating a fun game, the project provides more hands on experience
using Rust and is a showcase for ``open_ttt_lib``.


..  _ref-objective-free-of-charge:

-------------------------------------------------------
Provide Free of Charge and Under an Open-Source License
-------------------------------------------------------
FossXO is and will always be free, open-source, and contain no advertisements or
trackers. The game is released under a permissive open-source license and its
code is available from a public repository such as `<https://github.com/>`__.

Many of today's games casual games are released for free, but include
questionable monetization models such as microtransactions, pay-to-win schemes,
advertisements, and personal data harvesters. FossXO stands apart from
these games by respecting players who choose to spend their valuable time
playing the game.


..  index:: RustConf

------------------------
Release by RustConf 2020
------------------------
FossXO's initial release is scheduled to coincide with RustConf 2020
on August 21, 2020. [#rustconf]_ RustConf is the annual Rust developers conference; since
FossXO is developed in Rust this makes an excellent time to launch a Rust
based game. [#limitedscope]_


..  _ref-objective-easily-expandable-and-modifiable:

--------------------------------
Easily Expandable and Modifiable
--------------------------------
Playing tic-tac-toe in a variety of environments is a large part of what sets
FossXO apart from other tic-tac-toe games. The game is designed such that
developers can easily add new environments. This allows developers to focus
their time and effort creating stunning environments. Additionally, this
lowers the barrier of entry for users who are interested in modifying the game.
Finally, an easily modifiable code base allows quick turnaround of future
releases of the game.


.. index:: throwaway prototype
.. _ref-objective-build-risk-reduction-prototype:

------------------------------
Build Risk Reduction Prototype
------------------------------
The development team creating FossXO is new to the Rust programming language
and the available Rust libraries for game development. To help mitigate this
risk, a throwaway prototype game is created early in the project that explores
various technical aspects.

Using the lessons learned from the prototype also helps the development team
design a code base that is easily expandable and modifiable per the above
objective.


..  rubric:: Footnotes

..  [#rogers] The structure of this document is based on [Rogers-2014]_.
..  [#ounceOfRust] For details on the Ounce of Rust project see the
        `Ounce of Rust Project Manual <https://j-richey.github.io/project-documentation/ounce-of-rust/>`__
..  [#openTTTlib] ``open_ttt_lib`` is available at https://crates.io/crates/open_ttt_lib
        and source code is hosted at https://github.com/j-richey/open_ttt_lib
..  [#rustconf] For details on RustConf see their website: https://rustconf.com/
..  [#limitedscope] To help meet the target release date, the initial release of
        the game might contain a subset of the environments described in this
        document.
