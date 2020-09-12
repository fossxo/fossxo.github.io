########
Gameplay
########

.. index:: game rules
.. _ref-gameplay-rules-of-tic-tac-toe:

====================
Rules of Tic-tac-toe
====================
Tic-tac-toe is a game of strategy where two players, X and O, take turns placing
their mark in a 3 x 3 gird. The rules for tic-tac-toe used for each game mode
are as follows:

1.  Play occurs on a board composed of a 3 x 3 grid of squares. The board starts
    empty with no marks.
#.  The first player places their mark in one of the grid's squares. Traditionally,
    the mark is the letter *X*.
#.  The second player places their mark in one of the grid's empty squares. A square
    that already contains a mark cannot be updated or altered. Traditionally, the
    second player uses the letter *O* as their mark.
#.  Turns alternate between the players until the game is over.
#.  The first player to get three of their marks in a line wins the game.
    That is: they have three marks in a row, column, or diagonally.
    Examples of winning games are shown in :numref:`fig-example-wining-games`.

    ..  _fig-example-wining-games:
    ..  figure:: img/example-wining-games.*
        :alt: Examples of winning tic-tac-toe games.

        Examples of winning tic-tac-toe games showing player X winning by getting
        three marks a row, diagonal, and column. The red line shows the squares
        that contributed to the win. Notice that it is possible to get multiple
        sets of three marks in a row.

#.  The game ends in a draw, known as a :index:`cat's game`, if no more empty
    squares remain and a player has failed to get three marks in a line.
    Examples of cat's games are shown in :numref:`fig-example-cats-games`.

    ..  _fig-example-cats-games:
    ..  figure:: img/example-cats-games.*
        :alt: Examples of tic-tac-toe cat's games.

        Examples of tic-tac-toe games ending in a cat's game. No player managed
        to get three marks in a line.

#.  The steps above are repeated for a series of games. The starting player
    alternates between games. For example, the second game player *O* gets to
    make the first move.


.. index:: single-player; mode
.. _ref-gameplay-single-player-mode:

==================
Single-player Mode
==================
In single-player mode the player battles a computer controlled opponent. There
are three difficulty settings: **easy**, **medium**, and **hard**.

Easy difficulty is targeted at players who are new to tic-tac-toe and/or
computer games. The computer picks random squares allowing players to learn the
game's controls and rules.

Medium difficulty is for players who have some experience with tic-tac-toe. The
computer provides a challenge to the player but games are still winnable.

At hard difficulty the computer plays almost perfect games. The player must
capitalize on rare mistakes made by the computer to win. This is the recommended
difficulty for experienced tic-tac-toe players.

.. index:: two player mode, multiplayer
.. _ref-gameplay-two-player-mode:

===============
Two Player Mode
===============
In two player mode two players take turns placing their marks according to the
rules of tic-tac-toe previously described.


.. index:: speedrun; mode
.. _ref-gameplay-speed-run-mode:

=============
Speedrun Mode
=============
A speedrun mode provides an additional challenge for experienced players.
Players battle a flawless computer opponent through ten environments completing
the games as fast as possible. At the end of the run the total time is
displayed along with the previous best times. [#timecalculation]_

The player is disqualified and the run halted if the player loses a game.
Since the computer opponent never makes a mistake, each game in the speedrun
ends in a cat's game. In other words, each speedrun requires the same total
number of moves to complete.

Unnecessary animations are disabled in speedrun mode so they do not get in the
way of the speedup gameplay. Additionally, speedrun mode has its own dramatic
music that replaces the music tracks of each environment --- environments are
played so fast there is not sufficient time to appreciate their individual sound
tracks.


..  rubric:: Footnotes

..  [#timecalculation] The time it takes the computer to pick a square is not counted
      towards the player's time. This ensures times are consistent between
      slower and faster computers.
