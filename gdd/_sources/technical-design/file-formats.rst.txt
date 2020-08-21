############
File Formats
############

This section describes the various types of files used by FossXO including
details of the data format and considerations such as what happens if the file
cannot be loaded.


.. index:: asset file

================
Game Asset Files
================
The game uses many different assets such as textures, fonts, and audio.
The supported types of asset files are listed in
:numref:`table-supported-asset-file-types`.

..  tabularcolumns:: |l|l|L|
..  _table-supported-asset-file-types:
..  table:: Supported asset file types.

    =============  =======================  ====================================
    Asset Type     Extension                Notes
    =============  =======================  ====================================
    Texture        ``.png``                 24 bit RGB or 32 bit RGBA PNGs.
    Sound FX       ``.flac`` / ``.ogg``     FLAC is used for short sounds,
                                            Ogg Vorbis for longer sounds. [#shortsounds]_
                                            Mono or stereo. FLAC is 16 bit.
                                            44.1 kHz or 48 kHz sample rate.
    Music          ``.ogg``                 Mono or stereo Ogg Vorbis at
                                            44.1 kHz or 48 kHz.
    Font           ``.ttf``                 TrueType font.
    Prefab         ``.ron``                 Entity definitions in Rusty Object
                                            Notation.
    =============  =======================  ====================================

The asset files are located in the ``assets/`` directory that is next to the
game's executable. Each type of asset is organized in subdirectories as shown in
:numref:`code-asset-dir-organization`. [#environmentsubdirs]_

..  _code-asset-dir-organization:
..  code-block:: text
    :caption: Organization of the assets directory.

    assets/
    ├── fonts
    ├── music
    ├── prefab
    ├── shaders
    ├── sound_fx
    ├── textures
    └── ui

If needed, further subdirectories can be created, e.g. the ``textures``
directory could contain ``backgrounds`` and ``sprites`` subdirectories.

The game requires read access to its asset files. If an asset file used by an
environment cannot be accessed, the environment will not be used and an error
is emitted to the game's log. If a asset required by the game's menu or other
core systems cannot be loaded the game exits with a user facing error message.
Additionally, if all environments fail to find their required resources, the
debug environment is used as it does not require any special resources but still
provides access to the core game mechanics.

To avoid unnecessary files from accidentally being included in the game, the
game emits a warning if unused files are found in the assets directory.


.. index:: game configuration file
.. _ref-game-configuration-file:

=======================
Game Configuration File
=======================
The game's settings and options are stored in the game configuration file. This
is a TOML file named ``config.toml`` and is stored in the user's local data
directory. [#userdata]_

The file contains the following keys in the global section:

music_volume
    The volume to play the environments, speedrun, and menu music. This is a
    float value from 0.0 to 1.0 where 1.0 is full volume. The default value is 1.0.
sound_fx_volume
    The volume to play sound FX. This is a float value from 0.0 to 1.0 where 1.0
    is full volume. The default value is 1.0.

The ``single-player`` section provides the single-player settings to use when
the game is first loaded or default to when starting a new single-single player
game.

difficulty
    The difficulty to use. Default is medium.

mark
    The player's mark, X or O. Default is X.

If the file does not exist, the game creates a new file using the default values.
If there is a problem reading or parsing this file, default values are used.


.. index:: user data file
..  _ref-user-data-file:

==============
User Data File
==============
The user data file stores various pieces of user data including best speedrun
times or unlocked achievements. This file is a SQLite database stored in the
user's data directory. [#userdata]_ The file is created if it does not exist.

The ``speedruns`` table whose schema is shown in
:numref:`code-user-data-speedruns-table`, stores the best speedrun times.

..  _code-user-data-speedruns-table:
..  code-block:: sql
    :caption: The user data file speedruns table schema.

    -- Stores the results of speedruns
    CREATE TABLE speedruns (
        -- The initials of the user who completed the run.
        initials TEXT,

        -- The total time of the run.
        total_time duration,

        -- The time of the fastest game during the run.
        fastest_game duration,

        -- The date and time of the run.
        date datetime
    );

The ``duration`` and ``datetime`` custom SQLite types are mapped to Rust types
such as ``chrono::Duration`` and ``chrono::DateTime`` from the
`chrono crate <https://crates.io/crates/chrono>`__.

The SQLite ``user_version`` value is set to 1 allowing future versions of the
game to load and migrate existing user data.

If the file cannot be initially created, an existing file fails to load, or if
the ``user_version`` value is greater than 1, the functionality provided is
disabled. E.g. the best speedrun times are not displayed.

.. index:: asset license info file
..  _ref-asset-license-info-files:

========================
Asset License Info Files
========================
To ensure :ref:`ref-distribution-license-compliance` the license information of
every asset file is tracked using license info files. Every ``assets``
subdirectory contains a ``license-info.yaml`` file that contains the required
licensing information.

The ``license-info.yaml`` is a YAML format text file that contains a list of
license info objects where each license info object contains the following
keys.

files
    List of files relative to the ``license-info.yaml`` for which the
    licencing information applies. Glob patterns are accepted.
title
    Title of the work.
author
    Name or handle of the author who created the files.
license
    The specific license the work is published under. Examples include CC0-1.0
    or CC-BY-4.0. See https://spdx.org/licenses/ for a complete listing of
    allowed license identifiers.
source
    Link to website the resource was obtained from.
copyright
    Optional. Some works include a copyright notice supplied by the author that
    must be included in the attribution.
modifications
    Optional. If the work was modified from the original provide a short summary
    of changes.

When a new asset is added to the game, it is the responsibility of the
developer or artist adding the resource to update the license info files.

..  rubric:: Footnotes

..  [#shortsounds] Short sounds are around 5 seconds or less.
..  [#environmentsubdirs] An alternate way to organize the ``assets``
        directory is create subdirectories for each environment. However,
        environments may share various resources such as sound FX or brush
        textures. In fact, programmers are encouraged to extract common
        components from environments to promote their reuse. Thus, to prevent
        developers and artists of thinking assets belong to specific
        environments, an alternate approach is taken of organizing resources by
        asset type.
..  [#userdata] The user data directory is ``~/.local/share/fossxo/`` on Linux
        and ``Documents/My Games/FossXO`` on Windows. The game needs read and
        write access to this directory.
