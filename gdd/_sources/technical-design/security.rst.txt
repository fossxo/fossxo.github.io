########################
System and Data Security
########################

Software security is an important consideration of any application, including
games. This section lists potential security risks the game poses to users'
systems and the steps taken to minimize those risks.

..  index:: supply chain attack

===================
Supply Chain Attack
===================
A supply chain attack is where cyber-attacker compromises libraries or other
dependencies of an application to attack systems that use the application.
The game makes extensive use of third party libraries, thus at an elevated risk
for this type of attack. It only takes one of the dependent libraries being
compromised for the user's system to be put at risk.

To mitigate this risk several steps are taken including generating a software
bill of materials and using tools to automate package auditing.

..  index:: software bill of materials

--------------------------
Software Bill of Materials
--------------------------
The :doc:`user-manual` contains a listing of every library used by the game.
The software BOM is specific for each release of the game. [#offlinebom]_

The software BOM ensures developers and users alike know exactly what version of
each library is being used. The list can be audited for libraries that have
known vulnerabilities or other issues.


..  index:: RustSec advisory database, cargo-deny

-------------------------
RustSec Advisory Database
-------------------------
The `RustSec Advisory Database <https://github.com/RustSec/advisory-db/>`_ is a
repository of security advisories for Rust crates. Tools such as
`cargo-deny <https://crates.io/crates/cargo-deny>`_ use this repository to
automatically warn when a Rust application is using problematic version of a
crate. Ideally, this check is part of the game's build process.

---------------
Package Sources
---------------
FossXO and its dependent libraries are open-source: anyone can send a pull
request to modify the codes functionality. Unfortunately, even with a careful
code reviews, it is possible to sneak in alternate versions of libraries. [#trustedsource]_
The cargo-deny tool can also be used to detect when packages are pulled in from
sources other than crates.io.


..  index:: asset file

-----------
Game Assets
-----------
It is possible a cyber-attacker create a special crafted game asset file that
when loaded by FossXO triggers a bug in one of the asset loading libraries used
by the game. This type of attack is less probable than attacking a package
source as an attacker would have to create an asset where the vulnerable library
successfully loads its main content yet provides the attacker some benefit such
as running arbitrary code contained in the asset. [#assetattack]_

Usage of the RustSec Advisory Database should help mitigate this attack by
identifying bugs in the asset loading libraries. Furthermore, assets provided
in an untrusted pull request are carefully scrutinized to prevent a targeted
attack against the game. [#pngcheck]_


..  index:: SQL injection attack

==============
Direct Attacks
==============
FossXO does not connect to internet servers for any reason or even have local
network multiplayer. The game does not allow users to open arbitrary files.
Finally, the game does not require any special or elevated permissions to run. [#install]_
Therefore, its direct attack surface should be minimal to would be attackers.

Regardless, the following steps are taken to help reduce the risk of compromise
to a user's system.

*   The Amethyst networking functionality is optional and not needed by this game.
    Therefore, the amethyst_network feature is disabled.
*   User input is sanitized, in particular the user's initials in the speedrun
    best times table. :numref:`fig-exploits-of-a_mom` shows a famous example
    of this type of attack.
*   Unsafe Rust is used only when needed, in the smallest scope possible, and
    each unsafe block is thoroughly reviewed.
*   Standard software best practices are followed to help minimize potential
    bugs or unexpected software behavior. Lint tools are used to help enforce
    these best practices. [#codecomplete]_

..  _fig-exploits-of-a_mom:
..  figure:: ../img/exploits_of_a_mom.png

    XKCD's `Exploits of a Mom <https://xkcd.com/327/>`_ is a famous example of
    an SQL injection attack.
    `Comic copyright xkcd.com under the CC BY-NC 2.5 license <https://xkcd.com/license.html>`_.


=============
Personal Data
=============
The game uses the user's initials when displaying the :ref:`ref-ui-speedrun`
best times. This data is stored in the :ref:`ref-user-data-file`. The game does
not use or store any other personal data.

No personal data leave's the users system. This includes ensuring personal data
is not included in the game's :ref:`log file <ref-dev-tools-logging>` as users
may attach log files to bug reports.

Examples of data that is excluded from log files includes:

* User's initials
* User's computer account name
* The computer name
* WiFi access point names
* Names of applications installed on the system


Non personal data that can be included in the logs files:

* FossXO version
* Operating system version
* Graphics driver provider and version
* Game settings


..  rubric:: Footnotes

..  [#offlinebom] Users might have old versions of the game installed on their
        system. Including an offline software bill of materials in the user
        manual ensures the BOM is accurate for the version of the game
        they are actually using.
..  [#trustedsource] For details on how malicious packages can be injected in
        pull requests see
        `Why npm lockfiles can be a security blindspot for injecting malicious modules
        <https://snyk.io/blog/why-npm-lockfiles-can-be-a-security-blindspot-for-injecting-malicious-modules/>`_
..  [#assetattack] If the asset fails to load the main content is very unlikely
        to still be included in a game. E.g. if the brick texture fails to show
        up a different brick texture will be used in place of the broken one.
..  [#pngcheck] Tools such as ``PNGcheck`` can help detect corrupted asset files.
..  [#install] Ideally, the application does not require elevated permissions to
        install.
..  [#codecomplete] McConnell (2004) *Code Complete: A Practical Handbook of Software Construction, Second Edition*
        provides a detailed guide to software best practices.
