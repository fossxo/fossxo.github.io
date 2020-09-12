########
Glossary
########

..  Please keep the glossary alphabetically sorted.

..  glossary::
    :sorted:

    asset
        A game asset is a sound, texture, font, model, or other data used by
        the game.

    background music
        Background music is a type of music that is not intended to be the
        primary focus of listeners. When described as the type of music for an
        :term:`environment`, it indicates the environment's music is not a main
        focus of the environment.

    BOM
        Acronym for bill of materials. See :term:`software bill of materials`
        for how BOMs are used in software.

    cat's game
        Term used when a game of tic-tac-toe ends in a draw where there is no winner.

    component
        In an :term:`entity component system` architecture, a component contains
        the data for one aspect of a game object. Component examples include
        color, position, or mark type.

    coordinate system
        A numerical system that describes the positions of points in space.

    ECS
        Acronym for :term:`entity component system`.

    entity
        In an :term:`entity component system` architecture, an entity represents
        a single object in the game. Entities by themselves do not store data,
        instead they identify the collection of :term:`components <component>`
        belonging to the object. Entity examples include the marks on the board
        or an AI player.

    entity component system
        An architecture pattern often used in game engines where
        :term:`entities <entity>` define the game's objects.
        :term:`Components <component>` store the actual data.
        :term:`Systems <system>` contain the logic that operates on the data.

    environment
        An environment is a unique setting or location where tic-tac-toe is
        played.

    FOSS
        Acronym for Free and :term:`open-source software`.

    hand drawn
        Hand drawn is a style of line that is wavy and imprecise.

    microtransaction
        A monetization scheme where players purchase in game currency, such as
        "gems", which can later be redeemed for other game items. These items
        often give the player an edge therefore requiring players to purchase
        said items if they wish to be competitive at the game.

    open-source software
        Open-source software is software whose source code is available for others
        to study, modify, and redistribute.

    projection matrix
        A 4x4 mathematical matrix that transforms a given point from 3D point in
        the game's world to the 2D point on the user's monitor. The game's
        camera contains this matrix.

    Rust programming language
        Rust is a systems programming language with a focus on safety and speed.
        Website: `<https://www.rust-lang.org/>`__

    RustConf
         RustConf is the annual Rust developers conference.

    software bill of materials
        A list of all libraries and components used in a piece of software.
        This is similar to how food packages list their ingredients.

    speedrun
        A speed run is a play-though of a video game where the go is to complete
        the game as fast as possible.

    supply chain attack
        An attack where a cyber-attacker compromises libraries or other
        dependencies of an application to attack systems that use the
        application.

    system
        In an :term:`entity component system` architecture, systems contain the
        game's logic. Each system is executed every game loop over a collection
        of :term:`components <component>`.

    throwaway prototype
        A prototype is a version of software that is created to demonstrate core
        features or techniques of a software application. A throwaway prototype
        is discarded once the features or techniques have been demonstrated.
        Throwaway prototypes allow for rapid experimentation with low risk.

    UI widget
        A graphical user interface control, such as a button or textbox, that
        facilitates a particular type of interaction. Typically widgets have
        consistent look and feel.
