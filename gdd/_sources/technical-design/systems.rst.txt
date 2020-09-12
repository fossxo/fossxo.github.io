#######
Systems
#######

Systems contain small pieces of the game's logic that is applied to collections
of components or resources. The systems can be grouped into bundles that provide
a specific game feature.

This section describes how systems are constructed and provides details about
the core systems used by the game.


.. index:: System trait

============
System Trait
============
In Amethyst systems implement the `System trait <https://docs.amethyst.rs/stable/specs/trait.System.html>`__.
A summary of the System trait is shown in :numref:`uml-system-trait`.

..  _uml-system-trait:
..  uml::
    :caption: Notable methods provided by the System trait.

    !include rust_types.uml
    hide empty members

    trait(System) {
        +SystemData: type
        +run(SystemData)
    }

The ``SystemData`` type specifies what types of components and resources the
system operates on. The ``run()`` method is provided the specified system data
every time it is called. This is typically once per game loop.


============================
System Design Considerations
============================
The game contains additional systems that are not described in this document.
However, these systems are designed around the following considerations.

*   Systems contain small pieces of functionality. For example in a Pong type
    game, one system would be responsible for moving the ball and a second
    system for moving the paddles.
*   The ``run()`` method often uses database like queries when accessing
    components using the ``join()`` method. For example, a Pong move ball system
    would join the ``Ball`` and ``Transform`` components.
*   Systems should not assume the number of components they are operating on.
    For example, the Pong move ball system should not assume there is exactly
    one ball. This ensures new gameplay modes can be easily added in the future.
*   Systems should be designed with re-usability in mind. For example, if making
    a system to move spark particles, consider making a generic particle system
    that can be used for other particle types.
*   Systems should not store any data in as a local field.

..  note::
    Even though systems are implemented as structures, they should not store any
    data in local fields. If a system needs private data, store the data
    in a resource object that has private fields. In fact, the next major
    version of Amethyst uses loose functions for systems and the System trait is
    removed.


============
Core Systems
============
There are several notable systems the game uses to provide the game's core
functionality. This includes getting player input and managing AI opponents.
Additional systems for environment specific features are added as needed.


.. index:: player system, keyboard, mouse

-------------------
Local Player System
-------------------
The local player system is responsible for translating mouse clicks and keyboard
button presses into player events that indicate where the player would like to
move.

The pseudocode in :numref:`code-local-player-system-pseudocode` describes the
local player system logic.

..  _code-local-player-system-pseudocode:
..  code-block:: text
    :caption: Local player system pseudocode.

    For each local player:
        Get the any active button presses from the input bindings.
        For keyboard buttons, extract the direct mappings to the board position.
        For mouse buttons, translate the coordinates to the board position.
        Send the request move player event with the indicated position.

The local player system allows players to request moves even when it is not
their turn --- it delegates filtering invalid moves to other systems. Checking
the keyboard bindings takes prescience over the mouse.


.. index:: ai system

----------------
AI Player System
----------------
The AI player system generates player events for the AI opponents.

:numref:`code-ai-player-system-pseudocode` shows the pseudocode that describes
the AI player system logic.

..  _code-ai-player-system-pseudocode:
..  code-block:: text
    :caption: AI player system pseudocode.

    For each AI player:
        Check to see if is the player's turn, if not skip the player.
        Check the AI's move delay to see if sufficient time has elapsed since
            the last game move.
        Get the position the AI wishes to mark using it's opponent field.
        Send the request move player event with the indicated position.

To prevent burning CPU cycles evaluating positions that will not be used the
AI system skips players if it is not the player's turn.


.. index:: mouse raycast system

--------------------
Mouse Raycast System
--------------------
The mouse raycast system is responsible for converting the mouse position from
screen to world coordinates and grid position.

:numref:`code-mouse-raycast-system` shows the pseudocode that describes the
mouse raycast system logic for converting the mouse position to the different
:doc:`coordinate-systems`.

..  _code-mouse-raycast-system:
..  code-block:: text
    :caption: Mouse raycast system pseudocode.

    Get the mouse position as screen coordinates.
    Use the game's camera and screen size to convert the
        screen coordinates to world coordinates.
    Use the grid resource to convert the world coordinates
        to tic-tac-toe grid position.
    Store all results in the mouse position resource.


.. index:: Amethyst system bundle

========================
Builtin Amethyst Systems
========================
Amethyst provides several system bundles that are used by the game:

TransformBundle
    Handles updating transform component's position matrix.
InputBundle
    Provides access to OS input events and is required for the UI systems.
AudioBundle
    Provides basic audio playing support.
UiBundle
    Provides support for rendering user interfaces and processing UI input events.
RenderingBundle
    Provides the game's rendering support.
CameraOrthoSystem
    Automatically adjusts the camera matrix when the window is resized.

See the Amethyst documentation for details on each of these bundles and the
systems they provide.
