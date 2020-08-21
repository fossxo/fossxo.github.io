##########################
Licensing and Distribution
##########################

This section describes how the game is licensed, distributed, and other related
details of getting the game into player's hands.


.. index:: MIT license, Apache license

==============
Game's License
==============
The game is dual licensed under the MIT [#mitlicense]_ and
Apache-2.0 [#apachelicense]_ licenses. This is the same licensing scheme used
for the Rust language. [#rustlanglicense]_ Under these licenses users are free
to obtain, modify, and redistribute the source code for both private and
commercial use.

Any original game assets are licensed under the CC-BY-4.0 license. [#ccbylicense]_


.. _ref-distribution:

============
Distribution
============
The game is directly distributed to players via the internet. There are two main
ways to acquire a copy of the game: downloading a platform specific package, or
building the game from source.

.. index:: installer, package

-----------------
Platform Packages
-----------------
Platform specific packages can be downloaded from the game's website.
In particular, Windows users expect an installer to install the game on their
system. They do not have ready access to compilers or other tools to build
software from source. Finally, Windows is the most widely used operating system
so a Windows installer is a requirement for this project to be successful.

Linux and Mac users have easier access to compilers but they still appreciate
packages for their convenience. Packages for these platforms are created on a
best effort basis.

-----------------
Build from Source
-----------------
Because the game is open-source anyone can build the game from source. This
involves cloning the game's source code repository and following the build
instructions documented in the README file. [#cratessizelimit]_


============
Monetization
============
There is no intention to generate revenue from FossXO. The is released
free of charge. There are no advertisements, microtransactions, or data
stealing spyware that is found in other *free* games.


..  _ref-distribution-license-compliance:

==============================
Third Party License Compliance
==============================
FossXO makes extensive use of third party software libraries and game
assets to help reduce the development effort. To use these resources, the game
must comply with their various licensing terms. Additionally, the game can only
use any third party resources that have licenses that are compatible with the
game's license.

------------------
Software Libraries
------------------
Rust libraries that have the following licenses can be used by the game:

* MIT
* BSD-2-Clause, BSD-3-Clause, and similar (FTL, ISC, Zlib)
* Apache-2.0
* MPL-2.0 [#mpl2license]_
* CC0 and The Unlicense

Additionally, the game can dynamically link to libraries with the above licenses
plus libraries that are licensed under the LGPL.

A software bill of materials is included in the game's user documentation. This
ensures the game complies with the required copyright notices. Also, users can
see exactly what is included in the software.

The software bill of materials includes the following information about each
library:

* Name
* Version number
* License
* Link to website or source code repository


-----------
Game Assets
-----------
The game can freely use or remix third party music, sound FX, and textures that
have the following licenses: [#ccbysa]_

* CC0
* CC-BY
* CC-BY-NC [#ccbync]_

A list of third party assets used is accessible from the game's credits or user
documentation to fulfill the attribution requirements. The following information
is included for each asset:

*   Title - title of the work.
*   Author - name or handle of the author.
*   License - the specific CC license the work is published under.
*   Copyright notice - some works include a copyright notice supplied by the
    author that must be included in the attribution.
*   Source - link to website the asset was obtained from.
*   Is derivative - Mention if the work was modified from the original and
    provide a short summary of changes.


=====================================
Internationalization and Localization
=====================================
The game is initially released in American English. However, localizations are
accepted from contributors to include in future releases. This helps the game be
more accessible to a wider audience. To make it easy to add various
localizations, the game is designed with internationalization support.


..  rubric:: Footnotes

..  [#mitlicense] `Choose an open source license - MIT License <https://choosealicense.com/licenses/mit/>`_
..  [#apachelicense] `Choose an open source license - Apache License 2.0 <https://choosealicense.com/licenses/apache-2.0/>`_
..  [#rustlanglicense] See Rust's `README <https://github.com/rust-lang/rust#license>`_
        for licensing details.
..  [#ccbylicense] `Choose an open source license - Creative Commons Attribution 4.0 International <https://choosealicense.com/licenses/cc-by-4.0/>`_
..  [#cratessizelimit] Cargo, the Rust package manager, can download and build
        applications hosted on crates.io. However, crates.io has a 10MB size
        limit that is too small to host this game.
..  [#mpl2license] `Choose an open source license - Mozilla Public License 2.0 <https://choosealicense.com/licenses/mpl-2.0/>`_
        summary indicates it is OK to distribute programs that use unmodified
        MPL-2.0 libraries under different terms.
..  [#ccbysa] The CC-BY-SA is incompatible with the the game's licensing terms.
        It is this author's understanding that a game is considered a derivative
        work and thus must also be licensed under CC-BY-SA if it were to use
        CC-BY-SA assets.
..  [#ccbync] Per the monetization section, the game is being released for free
        and is not intended to produce a revenue stream.
