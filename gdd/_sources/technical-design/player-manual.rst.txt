#############
Player Manual
#############

The player manual provides information about how to play the game, different
gameplay modes, and contains content to fulfill the
:ref:`ref-distribution-license-compliance` requirements.


..  index:: player manual

====================
Player Manual Topics
====================
The list of topics included in player manual are:

*   How to play tic-tac-toe and the :ref:`ref-gameplay-rules-of-tic-tac-toe`.
*   The different :doc:`../gameplay` modes of FossXO.
*   The :ref:`ref-ui-controls` and key bindings available.
*   How to get support. [#githubissues]_ To help with support requests the
    manual includes information such as the game's version number. This cold
    also include information on how to resolve common issues.
*   The game's credits.
*   Software bill of materials that provides a concise overview of the
    libraries used by the game.
*   Licenses information for third party software and assets.

To help users quickly find the information they are looking for each section is
kept short and pictures are used to demonstrate concepts.


===========================
HTML Generation Suggestions
===========================
As the :ref:`ref-ui-help` menu describes, local HTML files store the user
manual. There are some tools worth considering for creating the player manual.


..  index:: mdBook

------
mdBook
------
`mdBook <https://github.com/rust-lang/mdBook>`__ is a Rust based tool for
generating HTML based books from Markdown files. This tool both popular in the
Rust community and is used for other game manuals. [#openrabook]_


..  index:: cargo-about

-----------
cargo-about
-----------
`cargo-about <https://crates.io/crates/cargo-about>`__ is a Cargo plugin for
generating a listing of all creates used by the application. It uses a
templating engine to render the output listing allowing for Markdown or HTML
output which can then be included in the manual.

..  note::
    A similar technique can be used for generating the third party asset
    license information.


..  rubric:: Footnotes

..  [#githubissues] For example, provide links to the GitHub issue tracker.
..  [#openrabook] The `OpenRA book <https://github.com/OpenRA/book>`_ is mdBook
        based.
