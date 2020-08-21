##########################
Packages and Source Builds
##########################
The game is :ref:`distributed <ref-distribution>` as either a precompiled
package for their platform or users can build the game from source.


..  index:: Inno Setup, installer

============================
Inno Setup Windows Installer
============================
`Inno Setup <https://jrsoftware.org/isinfo.php>`_ is used to create Windows
packages. Inno Setup has been around for many years, is straight forward to
create setup script files, and provides excellent documentation on how to use.

Installer has typical behavior for Windows setup programs:

*   The executable and game assets are stored in the user's program files
    directory. [#autoconstants]_
*   The game's configuration and user data is stored in the user's games directory.
*   Shortcuts are placed in the start menu and optionally on the desktop.
*   The game is listed in the Programs & Features control panel so it can easily
    be uninstalled.
*   The version number listed in the installer and control panel match that of
    the game. [#getenv]_

The game might require some additional Windows specific resources to run, such
as the Microsoft Visual C++ redistributable. These items are included in the
installer or listed as prerequisites on a case by case basis.


..  index:: AppImage

======================
AppImage Linux Package
======================
`AppImage <https://appimage.org/>`_ is a way to package applications into a
standalone file that can run on many different Linux distributions. [#openra]_
Therefore, this makes its use ideal for this project.

..  note::
    AppImage package include libraries traditionally installed system wide.
    These included libraries and their versions are included as part of the
    software bill of materials.


=================
Build from Source
=================
The game is open-source thus users can build the game themselves. Several steps
are taken to help users build the game with minimal friction.

*   The repository's default branch is the ``release`` branch. Code on this
    branch has been tested, tagged, and is ready for release. Development
    occurs on a different branch. This allows users to clone and build the
    repository without having to remember to check out a specific tag or branch.
*   The README file contains clear instructions on how to build the game
    including any dependencies that need to be installed. These steps are
    targeted specifically at end users. For example, build steps that typically
    occur during development, such as running unit tests or lint tools are
    omitted.
*   The build steps are automated as much as possible to reduce the possibility
    of mistakes being made.


..  rubric:: Footnotes

..  [#autoconstants] See Inno Setup's *Auto Constants* feature for automatically
        selecting the correct directories to place files.
..  [#getenv] Inno Setup's ``GetEnv`` preprocessor can get environmental
        variables --- this is likely a useful way of passing data to Inno
        including the applications version number.
..  [#openra] OpenRA is an example of a game that uses AppImage.
