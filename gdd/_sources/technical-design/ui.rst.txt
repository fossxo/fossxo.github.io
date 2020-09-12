.. index:: widget, ui module

##############################
UI Widgets, Layout, and Styles
##############################

In addition to showing the game board, FossXO's :doc:`../gui` allows players to
select different game modes, configure options, and view speed run best times.
Amethyst provides a UI module, but it is intended to be used as building blocks
for a game UI instead of being used directly.

Therefore, FossXO provides a ``ui`` module that provides a high level interface
around the low level Amethyst functionality. This section describes how high
level UI widgets are constructed from the Amethyst building blocks, including
how the UI is styled, laid out, and incorporated into the rest of the game.

.. index:: UI components

====================
Amethyst UI Overview
====================
The `amethyst_ui <https://docs.amethyst.rs/master/amethyst_ui/index.html>`_
module provides the building blocks for creating game UIs. It actually contains
three separate ways to create a UI.

1.  The UI can be constructed directly using ``UiTransform``, ``UiText``,
    ``UiImage``, and ``Interactable`` components. [#additionalcomponents]_
    All fields of each component structure must be provided during creation.
    Also, this is the method described in the Amethyst book.
#.  The UI can be loaded from a ``.ron`` file using the ``UiCreator``. The entity
    that defines the root of the UI is provided. Additionally, the ``UiFinder`` is
    used to locate child elements. The down side of this approach is creating
    the ``.ron`` files gets tedious and there is no style sheet support making
    changes difficult. The Amethyst repository contains several examples of this
    method. [#weirdbug]_
#.  The ``UiButtonBuilder`` and ``UiLabelBuilder`` builders can be used to help
    build buttons and labels. The builders use default values for any missing
    fields.

Based on initial experimentation and the Amethyst documentation, building
widgets directly from Amethyst components seems  like the best method to base
the higher level functionality around. This ensures we have visibility into the
exact set of entities and components being created. Additionally, our high level
API hides the verbosity of creating widgets using this method.


.. index:: Menu struct, GameControls struct

==============
High Level API
==============
FossXO's ``ui`` module provides a high level API to construct game menus and the
in game controls. [#apiusecase]_ The ``Menu`` structure allows creating
:ref:`ref_ui-menus` related widgets, provides UI event handling logic, and holds
the underlying entities. The ``Menu`` structure and supporting types are
shown in :numref:`uml-ui-menu-types`.

..  _uml-ui-menu-types:
..  uml::
    :caption: Menu and and supporting types.

    !include rust_types.uml
    hide empty members

    struct(Menu) {
        - ui_entities
        - observers
        + new()
        + layout(world)
        + delete(world)
        + handle_ui_event(world, ui_event)  -> Option<fn>
        + set_title(world, text)
        + set_close_button(world, text, on_close)
        + add_button(world, text, on_press)
        + add_link_button(world, text, on_press)
        + add_separator(world)
        + add_paragraph(world, text, num_lines)
        + add_player_selector(world, initial_player) -> PlayerSelector
        + add_text_box(world, label, initial_text) -> TextBox
        + add_check_box(world, label, is_checked) -> CheckBox
    }

    struct(PlayerSelector) {
        + selected_player(world) -> Player
    }

    struct(TextBox) {
        + text(world) -> String
    }

    struct(CheckBox) {
        + is_checked(world) -> bool
    }

The ``GameControls`` structure holds the controls shown during an in progress
game. This includes the hamburger menu button, status text, and next game button.
:numref:`uml-ui-game-controls-types` shows the ``GameControls`` structure and
related types.

..  _uml-ui-game-controls-types:
..  uml::
    :caption: Game controls and related types.

    !include rust_types.uml
    hide empty members

    struct(GameControls) {
        - ui_entities
        - observers
        + new()
        + handle_ui_event(ui_event) -> Option<fn>
        + layout(world)
        + delete(world)
        + set_menu_button(world, on_press)
        + set_game_over_button(world, on_press) -> GameOverButton
        + set_extra_information(world, text)
        + enable_speed_run_display(world)
    }

    struct(GameOverButton) {
        + set_text(world, text)
        + show(world)
        + hide(world)
    }

The types provided by the high level API are specific for FossXO. For example
instead of providing a generic toggle switch, the menu builder provides the
``PlayerSelector`` widget to hold the :ref:`ref-ui-single-player` menu
:guilabel:`Play as` selector. This allows us to focus on creating the controls
needed for the game without having to handle potentially many different use
cases of generic controls.


--------------------------
Building Menus and Widgets
--------------------------
The ``Menu`` and ``GameControls`` structures hide the details of constructing
widgets with Amethyst from the rest of the game. They also ensure the widgets
are constructed with a consistent style and layout as discussed in the
:ref:`ref-ui-styling` and :ref:`ref-ui-layout` sections.

The structures ensure the necessary components are created so the game's
:doc:`systems` can take care of automatically updating the UI. For example,
the game's status text is automatically updated by the game state display system,
thus there is no need to have a way to explicitly update the status text in
the ``GameControls``. Therefore, the ``Menu`` and ``GameControls`` structures
hide most of the underlying widgets.

Some additional items of interest are:

*   The ``add_*`` methods add new elements to UI. The order in which the
    ``add_*`` methods are called determines the order the elements appear in the UI.
*   The ``set_*`` methods add or overwrite a specific UI element. For example,
    menus have at most one title element that is provided via the
    ``set_title()`` method.
*   The ``layout()`` method should be called once all items have been added
    to the UI to perform the automatic :ref:`ref-ui-layout` process.
*   Use ``delete()`` to delete all widgets and their entities. Once this is
    called the menu or any widgets created from it should not be used.


-----------------
Accessing UI Data
-----------------
When a state might need access data contained in a particular widget, the `
`Menu`` and ``GameControls`` structures return an instance of the widget when
constructed. For example, ``add_player_selector()`` returns a ``PlayerSelector``
that can later be used by the state to see which selection has been made.

However, in most cases, the game's systems take care of managing and updating
UI related data so the state's don't have to micro-manage the UI.


.. index:: EntityObservers struct

-----------------------------------------
Handling UI Events with Slots and Signals
-----------------------------------------
The UI buttons use a basic slots and signals concept to handle button presses.
When a button is created, the a ``on_press`` callback is provided.
The ``handle_ui_event()`` method returns the previously registered callback
that is associated with given UI event. [#callbackborrowchecker]_
The state can then use this to invoke the logic for the specific button preventing
states from having to implement long and bug prone ``if`` / ``else if`` chains.

:numref:`code-ui-on-press-handler` shows the typical signature of the callback.

..  _code-ui-on-press-handler:
..  code-block:: rust
    :caption: Typical signature of button ``on_press`` callback.

    fn on_my_button_press(&mut self, world: &mut World) {
        // Handle the press event here
    }

The ``ui`` module uses the private ``EntityObservers`` structure, shown in
:numref:`uml-ui-event-observers`, to help with mapping UI events to callbacks
registered during the building process.

..  _uml-ui-event-observers:
..  uml::
    :caption: Event observers struct maps entities to observer callbacks.

    !include rust_types.uml
    hide empty members

    struct(EntityObservers) {
        - observers: Map<entity, fn>
        + add_observer(entity, observer: fn)
        + observer(entity) -> Option<fn>
    }


.. index:: UiStyle struct
.. _ref-ui-styling:

=======
Styling
=======
An important part of any user interface is to have a consistent style throughout.
FossXO achieves this by specifying common UI widget properties in a style
resource as shown in :numref:`uml-ui-style-resource`.

..  _uml-ui-style-resource:
..  uml::
    :caption: UI style resources. Additional style structures are added as needed.

    !include rust_types.uml
    hide empty members

    struct(UiStyle) {
        + menu
        + title
        + button
        + hamburger_button
        + game_status
    }

    struct(MenuStyle) {
        background_image
    }

    struct(ButtonStyle) {
        + text_style
        + width
        + height
        + margin
        + border_thickness
        + border_color
        + color
        + hover_color
        + press_color
    }

    struct(TextStyle) {
        + font
        + font_size
        + color
    }

    struct(HamburgerButtonStyle) {
        + icon
        + size
    }

The style resource holds the common properties to all the UI widgets. When the
widgets are being constructed, the UI style is fetched from the world and its
properties are used instead of hard coding values for each individual UI widget
as done in ``.ron`` files. For example, if the UI designer wishes to use a
different font, every widget gets updated.

The ``ui`` module provides a ``load_style()`` function that loads assets
required by the style such as fonts, icons, and background images. This ensures
these resources are available when the game board or menus are displayed.


.. index:: UI layout, coordinates; world, projection matrix
.. _ref-ui-layout:

======
Layout
======
In addition to having consistent style, it is important to have consistent and
predictable locations of UI widgets. Requiring the UI designer to manually
specify coordinates to place widgets is both tedious and error prone. FossXO's
``ui`` module automatically determines where UI widgets should be placed. This
feature is known as automatic layout.

The Amethyst ``UiTransform`` component controls where the UI widget is drawn.
The position is specified using :ref:`ref-world-coordinates` with x-right and y-up.
The Z value controls the draw order with widgets with a higher Z order drawn
over those with a lower Z order. Also, the UI uses its own projection matrix,
thus its scale is different than used for the environments.

The origin of each component is selectable via the ``Anchor`` enum, shown in
:numref:`fig-ui-anchor-points`.

..  _fig-ui-anchor-points:
..  figure:: ../img/ui-anchor-points.*

    The anchor point sets the origin of the widget.

The ``ui`` module takes a few different approaches to layout depending on the
type of widget.


-------------
Fixed Content
-------------
Many widgets have fixed locations. For example a menu's title, close button, and
the hamburger button are all placed in fixed specific locations. The layout
logic for these items is fairly straight forward, but it must still account for
any style properties that affect the positioning.


----------------
Content Stacking
----------------
The main content of menus is stacked and centered on the screen. Using the
``Anchor::Middle`` variant allows Amethyst to take care of the majority of the
work. This will both horizontally and vertically center each component.
However, to prevent all the widgets from overlapping, the layout system must
take into account the total height of the content, the height of each
widget and margin between widgets then adjust their Y offset accordingly.

The pseudocode in :numref:`code-ui-layout-stacked-content` shows one way this
can be done by first calculating retaliative Y location of each widget then
shifting the entire group to be centered.

..  _code-ui-layout-stacked-content:
..  code-block:: text
    :caption: Pseudocode for centering stacked content.

    Set widget offsets:
        calculate the widget's center point Y using its height,
            margin, and center point of the previous widget:
        center point Y = previous center point Y - (widget.height / 2 + margin)

    Vertically center widgets:
        Calculate the total height of the widgets.
        Calculate current center point Y value from the total height.
        Calculate the offset from the expected Y value. Since
            widgets are anchored at the middle, the expected
            Y value is 0.0.
        Add the offset to each widget Y value thus vertically
            centering the group of widgets.


..  rubric:: Footnotes

..  [#additionalcomponents] There are additional components that are useful such
        as ``Selectable``. See the ``amethyst_ui`` documentation for additional
        components.
..  [#weirdbug] A weird bug was encountered where creating buttons
        using the ``UiLabelBuilder`` would cause buttons loaded from via the
        ``UiCreator`` to render in weird places and not be deleted when the root
        element was deleted.
..  [#apiusecase] FossXO's UI APIs are designed specifically around FossXO's
        interface requirements. They are no intended to construct general
        purpose user interfaces.
..  [#callbackborrowchecker] A reference to the observer callback to invoke is
        returned from ``handle_ui_event()`` instead of being directly invoked
        due to Rust's reference borrowing rules. The borrow checker prevents
        using closures to capture a reference to the struct when the callback is
        created like one might do in JavaScript. Additionally, while prototyping
        it was found the Rust compiler was unhappy with passing ``&mut self``
        to ``handle_ui_event()`` as ``self`` was also being used to access the
        ``Menu`` struct.
