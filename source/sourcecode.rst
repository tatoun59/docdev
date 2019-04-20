.. include:: /globals.rst

Source Code management
======================

Le code source de GLPI est gérée par `GIT <https://en.wikipedia.org/wiki/Git>`_ et hébergée sur `GitHub <https://github.com/glpi-project/glpi>`_.

Pour contribuer au code source, vous devez connaître un certain nombre de choses sur Git et le modèle de développement que nous suivons.

Branches
--------

Sur le dépôt Git, vous trouverez plusieurs branches :

* `master` contient le code source de la prochaine version majeure,
* `xx/bugfixes` contient le code source de la prochaine version mineure,
* vous ne devriez pas vous soucier de toutes les autres branches qui peuvent exister, elles devraient avoir été supprimées maintenant.

La branche `master' est l'endroit où de nouvelles fonctionnalités sont ajoutées. Ce code est réputé comme **non stable**.

Les branches `xx/bugfixes` sont celles où les bogues sont corrigés. Ce code est réputé *stable*.

Ces branches sont créées lorsque une version majeure est publiée. Au moment où j'ai écrit ces lignes, la dernière version stable est `9.1`, donc la branche de correction des bogues actuelle est `9.1/bugfixes'. Comme les anciennes versions ne sont plus supportées, les anciennes branches de corrections de bogues ne sont plus modifiées.

Tests
-----

Il y a de plus en plus de tests unitaires dans GLPI ; nous utilisons le `framework de tests unitaires atoum <http://atoum.org>`_.

Chaque proposition **doit** contenir des tests unitaires, les nouvelles fonctionnalités comme les corrections de bogues. Pour les corrections de bogues, ce n'est pas une exigence stricte si cela fait partie du code qui n'a pas encore été testé. Voir la section :ref:`unit testing section <unittests>` en bas de page.

Quoi qu'il en soit, les tests unitaires existants ne doivent jamais être cassés, si vous avez fait un changement qui casse quelque chose, vérifiez votre code, ou modifiez les tests unitaires, mais corrigez ça ! ;-)

.. _fhs:

Structuration hiérarchique des fichiers
---------------------------------------

.. note::

   Cela liste les répertoires et fichiers courants du code source de GLPI. Certains fichiers ne font pas partie des archives distribuées.

This is a brieve description of GLPI main folders and files:
Voici une brève description des dossiers et fichiers principaux de GLPI :


* |folder| `.tx`: Configuration Transifex
* |folder| `ajax`

  * |phpfile| `*.php`: Composants Ajax

* |folder| `files` Fichiers écrits par GLPI ou ses plugins (documents, fichiers de session, fichiers journaux, ...)
* |folder| `front`

  * |phpfile| `*.php`: Composants Front (toutes les pages affichées)

* |folder| `config` (uniquement rempli une fois installé)

  * |phpfile| `config_db.php`: Fichier de configuration de la base de données
  * |phpfile| `local_define.php`: Fichier optionnel pour remplacer certaines définitions de constantes (voir ``inc/define.php``)

* |folder| `css`

  * |folder| `...`: CSS stylesheets
  * |file| `*.css`: CSS stylesheets

* |folder| `inc`

  * |phpfile| `*.php`: Classes, fonctions et définitions

* |folder| `install`

  * |folder| `mysql`: MariaDB/MySQL schemas
  * |phpfile| `*.php`: scripts de mise à jour et programme d'installation

* |folder| `js`

  * |file| `*.js`: fichiers Javascript

* |folder| `lib`

  * |folder| `...`: librairies externes Javascript

* |folder| `locales`

  * |file| `glpi.pot`: fichier POT de Gettext
  * |file| `*.po`: traductions de Gettext
  * |file| `*.mo`: traductions compilées de Gettext

* |folder| `pics`

  * |file| `*.*`: images et icones

* |folder| `plugins`:

  * |folder| `...`: where all plugins lends

* |folder| `scripts`: divers scripts qui peuvent être utilisés dans les crontabs par exemple
* |folder| `tests`: tests unitaires et d'intégration
* |folder| `tools`: divers outils
* |folder| `vendor`: librairies tierces installées à partir de composer (voir composer.json ci-dessous)
* |file| `.gitignore`: Git ignore list
* |file| `.htaccess`: Some convenient apache rules (all are commented)
* |file| `.travis.yml`: fichier de configuration de Travis
* |phpfile| `apirest.php`: Point d'entrée principal de l'API REST
* |file| `apirest.md`: documentation de REST API
* |phpfile| `apixmlrpc.php`: Point d'entrée principal de l'API XMLRPC
* |file| `AUTHORS.txt`: liste des auteurs GLPI
* |file| `CHANGELOG.md`: Changements
* |file| `composer.json`: Définition des bibliothèques tierces (`voir le site de composer <https://getcomposer.org>`_)
* |file| `COPYING.txt`: Licence
* |phpfile| `index.php`: Point d'entrée principal de l'application
* |file| `phpunit.xml.dist`: fichier de configuration des tests unitaires
* |file| `README.md`: well... a README ;)
* |file| `status.php`: obtenir le statut GLPI à des fins de suivi



Workflow
--------

En résumé...
^^^^^^^^^^^

En bref, voici le workflow que nous allons suivre :

* `create a ticket <https://github.com/glpi-project/glpi/issues/new>`_
* fork, create a specific branch, and hack
* open a :abbr:`PR (Pull Request)`

Each bug will be fixed in a branch that came from the correct `bugfixes` branch. Once merged into the requested branch, developper must report the fixes in the `master`; with a simple cherry-pick for simple cases, or opening another pull request if changes are huge.

Each feature will be hacked in a branch that came from `master`, and will be merged back to `master`.

General
^^^^^^^

Most of the times, when you'll want to contribute to the project, you'll have to retrieve the code and change it before you can report upstream. Note that I will detail here the basic command line instructions to get things working; but of course, you'll find equivalents in your favorite Git GUI/tool/whatever ;)

Just work with a:

.. code-block:: bash

   $ git clone https://github.com/glpi-project/glpi.git

A directory named ``glpi`` will bre created where you've issued the clone.

Then - if you did not already - you will have to create a fork of the repository on your github account; using the `Fork` button from the `GLPI's Github page <https://github.com/glpi-project/glpi/>`_. This will take a few moments, and you will have a repository created, `{you user name}/glpi - forked from glpi-project/glpi`.

Add your fork as a remote from the cloned directory:

.. code-block:: bash

   $ git remote add my_fork https://github.com/{your user name}/glpi.git

You can replace `my_fork` with what you want but `origin` (just remember it); and you will find your fork URL from the Github UI.

A basic good practice using Git is to create a branch for everything you want to do; we'll talk about that in the sections below. Just keep in mind that you will publish your branches on you fork, so you can propose your changes.

When you open a new pull request, it will be reviewed by one or more member of the community. If you're asked to make some changes, just commit again on your local branch, push it, and you're done; the pull request will be automatically updated.

.. note::

   It's up to you to manage your fork; and keep it up to date. I'll advice you to keep original branches (such as ``master`` or ``x.y/bugfixes``) pointing on the upstream repository.

   Tha way, you'll just have to update the branch from the main repository before doing anything.

Bugs
^^^^

If you find a bug in the current stable release, you'll have to work on the `bugfixes` branch; and, as we've said already, create a specific branch to work on. You may name your branch explicitely like `9.1/fix-sthing` or to reference an existing issue `9.1/fix-1234`; just prefix it with `{version}/fix-`.

Generally, the very first step for a bug is to be `filled in a ticket <https://github.com/glpi-project/glpi/issues>`_.

From the clone directory:

.. code-block:: bash

   $ git checkout -b 9.1/bugfixes origin/9.1/bugfixes
   $ git branch 9.1/fix-bad-api-callback
   $ git co 9.1/fix-bad-api-callback

At this point, you're working on an only local branch named `9.1/fix-api-callback`. You can now work to solve the issue, and commit (as frequently as you want).

At the end, you will want to get your changes back to the project. So, just push the branch to your fork remote:

.. code-block:: bash

   $ git push -u my_fork 9.1/fix-api-callback

Last step is to create a PR to get your changes back to the project. You'll find the button to do this visiting your fork or even main project github page.

Just remember here we're working on some bugfix, that should reach the `bugfixes` branch; the PR creation will probably propose you to merge against the `master` branch; and maybe will tell you they are conflicts, or many commits you do not know about... Just set the base branch to the correct bugfixes and that should be good.

Features
^^^^^^^^

Before doing any work on any feature, mays sure it has been discussed by the community. Open - if it does not exists yet - a ticket with your detailled proposition. Fo technical features, you can work directly on github; but for work proposals, you should take a look at our `feature proposal platform <http://glpi.userecho.com/>`_.

If you want to add a new feature, you will have to work on the `master` branch, and create a local branch with the name you want, prefixed with `feature/`.

From the clone directory:

.. code-block:: bash

   $ git branch feautre/my-killer-feature
   $ git co feature/my-killler feature


You'll notice we do no change branch on the first step; that is just because `master` is the default branch, and therefore the one you'll be set on just fafter cloning. At this point, you're working on an only local branch named `feature/my-killer-feature`. You can now work and commit (as frequently as you want).

At the end, you will want to get your changes back to the project. So, just push the branch on your fork remote:

.. code-block:: bash

   $ git push -u my_fork feature/my-killer-feature

Commit messages
^^^^^^^^^^^^^^^

There are several good practices regarding commit messages, but this is quite simple:

* the commit message may refer an existing ticket if any,

  * just make a simple reference to a ticket with keywords like ``refs #1234`` or ``see #1234"``,
  * automatically close a ticket when commit will be merged back with keywords like ``closes #1234`` or ``fixes #1234``,

* the first line of the commit should be as short and as concise as possible
* if you want or have to provide details, let a blank line after the first commit line, and go on. Please avoid very long lines (some conventions talks about 80 characters maximum per line, to keep it lisible).

.. _3rd_party_libs:

Third party libraries
^^^^^^^^^^^^^^^^^^^^^

Third party libraries are handled using the `composer tool <http://getcomposer.org>`_.

To install existing dependencies, just install composer from their website or from your distribution repositories and then run:

.. code-block:: bash

   $ composer install

To add a new library, you will probably found the command line on the library documentation, something like:

.. code-block:: bash

   $ composer require libauthor/libname

.. _unittests:

Unit testing (and functionnal testing)
--------------------------------------

.. note::

   A note for the purists... In GLPI, there are both unit and functional tests; without real distinction ;-)

We use the `atoum unit tests framework <http://atoum.org>`_; see `GLPI website if you wonder why <http://glpi-project.org/spip.php?breve375>`_.

`atoum`'s documentation in available at: http://docs.atoum.org

.. warning::

   With `atoum`, test class **must** refer to an existing class of the project!

Tests isolation
^^^^^^^^^^^^^^^

Tests must be run in an isolated environment. By default, `atoum` use a concurrent mode; that launches tests in a multi-threaded environment. While it is possible to bypass this; this should not be done See http://docs.atoum.org/en/latest/engine.html.

For technical reasons (mainly because of the huge session usage), GLPI unit tests are actually limited to one only thread while running the whole suite; but while developing, the behavior should only be changed if this is really needed.

Type hitting
^^^^^^^^^^^^

Unlike PHPUnit, `atoum` is very strict on type hitting. This really makes sense; but often in GLPI types are not what we should expect (for example, we often get a string and not an integer from counting methods).

Database
^^^^^^^^

Each class that tests something in database **must** inherit from ``\DbTestCase``. This class provides some helpers (like ``login()`` or ``setEntity()`` method); and it also does some preparation and cleanup.

Each ``CommonDBTM`` object added in the database with its ``add()`` method will be automatically deleted after the test method. If you always want to get a new object type created, you can use ``beforeTestMethod()`` or ``setUp()`` methods.

.. warning::

   If you use ``setUp()`` method, do not forget to call ``parent::setUp()``!

Some bootstrapped data are provided (will be inserted on the first test run); they can be used to check defaults behaviors or make queries, but you should **never change those data!** This lend to unpredictable further tests results.

Variables declaration
^^^^^^^^^^^^^^^^^^^^^

When you use a property that has not been declared, you will have errors that may be quite difficult to understand. Just remember to always declare property you use!

.. code-block:: php

   <?php

   class MyClass extends atoum {
      private $myprop;

      public function testMethod() {
         $this->myprop = 'foo'; //<-- error here if missing "private $myprop"
      }
   }

Launch tests
^^^^^^^^^^^^

You can install `atoum` from composer (just run ``composer install`` from GLPI directory) or even system wide.

There are two directories for tests:

* ``tests/units`` for main core tests;
* ``tests/api`` for API tests.

You can choose to run tests on a whole directory, or on any file. You have to specify a bootstrap file each time:

.. code-block:: bash

   $ atoum -bf tests/bootstrap.php -mcn 1 -d tests/units/
   [...]
   $ atoum -bf tests/bootstrap.php -f tests/units/Html.php


If you want to run the API tests suite, you need to run a development server:

.. code-block:: bash

   php -S localhost:8088 tests/router.php &>/dev/null &


Running `atoum` without any arguments will show you the possible options. Most important are:

* ``-bf`` to set bootstrap file,
* ``-d`` to run tests located in a whole directory,
* ``-f`` to run tests on a standalone file,
* ``--debug`` to get extra informations when something goes wrong,
* ``-mcn`` limit number of concurrent runs. This is unfortunately mandatory running the whole test suite right now :/,
* ``-ncc`` do not generate code coverage,
* ``--php`` to change PHP executable to use,
* ``-l`` loop mode.

Note that if you do not use the `-ncc` switch; coverage will be generated in the `tests/code-coverage/` directory.
