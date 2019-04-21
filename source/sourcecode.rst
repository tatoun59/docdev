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

* `Créez un ticket <https://github.com/glpi-project/glpi/issues/new>`_
* forkez, créer une branche spécifique, et modifier le code
* Ouvrez une :abbr:`PR (Pull Request)`

Chaque bogue sera corrigé dans une branche provenant de la branche `bugfixes' correcte. Une fois fusionné dans la branche demandée, le développeur doit rapporter les correctifs dans la branche `master' ; avec un simple cherry-pick pour les cas simples, ou en ouvrant une autre pull request si les changements sont importants.

Chaque nouvelle fonctionnalité sera developpée dans une branche provenant de `master', et sera fusionnée en `master'.

Général
^^^^^^^

La plupart du temps, lorsque vous voudrez contribuer au projet, vous devrez récupérer le code et le modifier avant de pouvoir faire un rapport en amont. Notez que je vais détailler ici les instructions de base en ligne de commande pour faire fonctionner les choses ; mais bien sûr, vous trouverez des équivalents dans votre GUI/outil/peu importe Git préféré ;-)

Just work with a:
Il suffit de faire un :

.. code-block:: bash

   $ git clone https://github.com/glpi-project/glpi.git

Un répertoire ``glpi`` sera créé dans lequel se trouveront les fichiers.

Ensuite, si vous ne l’avez pas déjà fait, vous devrez créer un fork du repository sur votre compte github; en utilisant le bouton Fork de la page Github de GLPI. Cela prendra quelques instants et vous aurez un repository créé, {votre nom d'utilisateur}/glpi-créé à partir de glpi-project/glpi.

Ajouter votre fork (sur Github) comme référence distante de votre répertoire cloné (localement) :

.. code-block:: bash

   $ git remote add my_fork https://github.com/{your user name}/glpi.git

You can replace `my_fork` with what you want but `origin` (just remember it); and you will find your fork URL from the Github UI.
Vous pouvez remplacer `my_fork` par ce que vous voulez, mais par l’ origine (rappelez-vous-en); et vous trouverez l'URL de votre fork dans l'interface utilisateur de Github.

Une bonne pratique de base en utilisant Git est de créer une branche pour tout ce que vous voulez faire. nous en parlerons dans les sections ci-dessous. N'oubliez pas que vous allez publier vos branches sur votre fork pour pouvoir proposer vos modifications.

Lorsque vous ouvrez une nouvelle pull request, celle-ci sera examinée par un ou plusieurs membres de la communauté. Si vous êtes invité à apporter des modifications, il vous suffit de valider à nouveau dans votre branche locale, de commiter à nouveau et c'est tout. La demande de pull request sera automatiquement mise à jour.

.. note::

   C'est à vous à gérer votre fork et le garder à jour. Je vous conseillerai de garder les branches d'origine (telles que ``master`` ou ``x.y/bugfixes``) pointant sur le référentiel en amont.

   De cette façon, il vous suffira de mettre à jour la branche à partir du référentiel principal avant de faire quoi que ce soit.

Bogues
^^^^^^

Si vous trouvez un bogue dans la version stable actuelle, vous devrez travailler sur la branche `bugfixes` et, comme nous l’avons déjà dit, créez une branche spécifique sur laquelle travailler. Vous pouvez nommer explicitement votre branche comme `9.1/fix-sthing` ou faire référence à un problème existant `9.1/fix-1234` ; préfixez-le simplement avec `{version}/fix-`.

Généralement, la toute première étape d'un bogue consiste à `renseigner un ticket <https://github.com/glpi-project/glpi/issues>`_.

À partir du répertoire de clonage:

.. code-block:: bash

   $ git checkout -b 9.1/bugfixes origin/9.1/bugfixes
   $ git branch 9.1/fix-bad-api-callback
   $ git co 9.1/fix-bad-api-callback

À ce stade, vous travaillez sur une seule branche locale nommée `9.1/fix-api-callback`. Vous pouvez maintenant travailler pour résoudre le problème et commiter (aussi souvent que vous le souhaitez).

À la fin, vous voudrez que vos modifications soient apportées au projet. Donc, il suffit de pousser la branche sur votre fork distant:

.. code-block:: bash

   $ git push -u my_fork 9.1/fix-api-callback

La dernière étape consiste à créer un PR pour que vos modifications soient apportées au projet. Vous trouverez le bouton permettant de le faire en visitant votre page fork ou même la page principale de github du projet.

Rappelez-vous simplement que nous travaillons sur un correctif, qui devrait atteindre la branche `bugfixes` ; la création d'un PR va probablement vous proposer de fusionner avec la branche `master`  et peut-être vous dira-t-il qu'il y a des conflits, ou de nombreux commits dont vous ignorez l'existence... Réglez simplement la branche de base sur les bons correctifs et cela devrait être bon.

fonctionnalités
^^^^^^^^^^^^^^^

Avant de commencer tout travail sur une fonctionnalité, il est certain que la communauté en a discuté. Ouvrez - s'il n'existe pas encore - un ticket avec votre proposition détaillée. Pour les fonctionnalités techniques, vous pouvez travailler directement sur github; mais pour les propositions de travail, vous devriez jeter un coup d'œil à notre `plateforme de propositions de fonctionnalités <http://glpi.userecho.com/>`_.

Si vous souhaitez ajouter une nouvelle fonctionnalité, vous devrez travailler sur la branche principale et créer une branche locale portant le nom de votre choix, préfixée de `feature/`.

À partir du répertoire de clonage:

.. code-block:: bash

   $ git branch feautre/my-killer-feature
   $ git co feature/my-killler feature


Vous remarquerez que nous ne changeons pas de branche à la première étape? C'est simplement parce que `master` est la branche par défaut et donc celle que vous allez définir sur le clonage juste après. À ce stade, vous travaillez sur une branche locale uniquement appelée `feature/my-killer-feature`. Vous pouvez maintenant travailler et commiter (aussi souvent que vous le souhaitez).

À la fin, vous voudrez que vos modifications soient apportées au projet. Donc, il suffit de pousser la branche sur votre fork distant :


.. code-block:: bash

   $ git push -u my_fork feature/my-killer-feature

Commit des messages
^^^^^^^^^^^^^^^^^^^

Il existe plusieurs bonnes pratiques concernant les messages de validation, mais ceci est assez simple:

* le message de validation peut faire référence à un ticket existant, le cas échéant,

  * il suffit de faire référence à un ticket avec des mots clés tels que ``refs #1234`` or ``see #1234"``,
  * fermer automatiquement un ticket quand le commit sera fusionné avec des mots clés comme ``closes #1234`` or ``fixes #1234``,

* la première ligne du commit doit être aussi courte et concise que possible
* si vous voulez ou devez fournir des détails, laissez une ligne vierge après la première ligne de commit. Évitez les longues lignes (certaines conventions parlent de 80 caractères maximum par ligne, afin de garder de la lisibilité).

.. _3rd_party_libs:

Third party libraries
^^^^^^^^^^^^^^^^^^^^^

Les bibliothèques tierces sont gérées à l'aide de `l'outil composer <http://getcomposer.org>`_.

Pour installer des dépendances existantes, installez simplement composer à partir de son site Web ou des dépots de votre distribution, puis exécutez:

.. code-block:: bash

   $ composer install

Pour ajouter une nouvelle bibliothèque, vous aurez probablement trouvé la ligne de commande dans la documentation de la bibliothèque qui doit ressembler à quelque chose comme:


.. code-block:: bash

   $ composer require libauthor/libname

.. _unittests:

Tests unitaires (et tests fonctionnels)
---------------------------------------

.. note::

   Un mot pour les puristes… Dans GLPI, il existe des tests unitaires et fonctionnels; sans réelle distinction ;-)

We use the `atoum unit tests framework <http://atoum.org>`_; see `GLPI website if you wonder why <http://glpi-project.org/spip.php?breve375>`_.
Nous utilisons `le framework de tests unitaires atoum <http://atoum.org>`_. Consultez le `site Web de GLPI si vous vous demandez pourquoi <http://glpi-project.org/spip.php?breve375>`_.

La documentation d’`atoum` est disponible à l’ adresse suivante : http://docs.atoum.org

.. warning::

   Avec `atoum`, la classe de test doit faire référence à une classe existante du projet !

Isolation des tests
^^^^^^^^^^^^^^^^^^^

Les tests doivent être exécutés dans un environnement isolé. Par défaut, atoum utilise un mode simultané qui lance des tests dans un environnement multi-thread. Bien qu'il soit possible de contourner cela, cela ne devrait pas être fait. Voir http://docs.atoum.org/en/latest/engine.html.

For technical reasons (mainly because of the huge session usage), GLPI unit tests are actually limited to one only thread while running the whole suite; but while developing, the behavior should only be changed if this is really needed.
Pour des raisons techniques (principalement en raison de l'utilisation important de la session), les tests unitaires GLPI sont en réalité limités à un seul thread lors de l'exécution de la suite complète mais en développant, le comportement ne devrait être changé que si cela est vraiment nécessaire.

Les types
^^^^^^^^^

Contrairement à PHPUnit, atoum est très strict sur les types. Cela a vraiment du sens mais souvent dans GLPI, les types ne sont pas ce à quoi nous devrions nous attendre (par exemple, nous obtenons souvent une chaîne et non un entier à partir de méthodes de comptage).

Base de données
^^^^^^^^^^^^^^^

Chaque classe qui teste quelque chose dans la base de données doit en hériter ``\DbTestCase``. Cette classe fournit des helpers (comme les methodes ``login()`` ou ``setEntity()``); et il fait aussi de la préparation et du nettoyage.

Chaque objet ``CommonDBTM`` ajouté à la base de données avec la méthode ``add()`` sera automatiquement supprimé après la méthode de test. Si vous souhaitez toujours créer un nouveau type d'objet, vous pouvez utiliser les méthodes ``beforeTestMethod()`` ou ``setUp()``.

.. warning::

   Si vous utilisez la méthode ``setUp()``, n'oubliez pas d'appeler ``parent::setUp()``!

Certaines données amorcées sont fournies (seront insérées lors du premier test); elles peuvent être utilisées pour vérifier les comportements par défaut ou faire des requêtes, **mais vous ne devriez jamais changer ces données !** Cela conduit à des résultats de tests imprévisibles.

Déclaration de variables
^^^^^^^^^^^^^^^^^^^^^^^^

Lorsque vous utilisez une propriété qui n'a pas été déclarée, vous aurez des erreurs qui peuvent être assez difficiles à comprendre. N'oubliez pas de toujours déclarer la propriété que vous utilisez!

.. code-block:: php

   <?php

   class MyClass extends atoum {
      private $myprop;

      public function testMethod() {
         $this->myprop = 'foo'; //<-- error here if missing "private $myprop"
      }
   }

Lancer des tests
^^^^^^^^^^^^^^^^

Vous pouvez installer atoum à partir de composer (il suffit de lancer ``composer install`` dans le répertoire GLPI).

Il existe deux répertoires pour les tests:


* ``tests/units`` pour les principaux tests de base;
* ``tests/api`` pour les tests API.

Vous pouvez choisir d'exécuter des tests sur un répertoire entier ou sur n'importe quel fichier. Vous devez spécifier un fichier d'amorçage à chaque fois :

.. code-block:: bash

   $ atoum -bf tests/bootstrap.php -mcn 1 -d tests/units/
   [...]
   $ atoum -bf tests/bootstrap.php -f tests/units/Html.php


Si vous souhaitez exécuter la suite de tests d'API, vous devez exécuter un serveur de développement:

.. code-block:: bash

   php -S localhost:8088 tests/router.php &>/dev/null &


Exécuter `atoum` sans argument vous montrera les options possibles. Les plus importantes sont :

* ``-bf`` pour définir le fichier de démarrage,
* ``-d`` exécuter des tests situés dans un répertoire entier,
* ``-f`` exécuter des tests sur un fichier autonome,
* ``--debug`` pour obtenir des informations supplémentaires en cas de problème,
* ``-mcn`` limite le nombre d'exécutions simultanées. Ceci est malheureusement obligatoire pour exécuter toute la suite de tests maintenant :/,
* ``-ncc`` dne pas générer de couverture de code,
* ``--php`` changer l'exécutable PHP à utiliser,
* ``-l`` mode "en boucle.

Note that if you do not use the `-ncc` switch; coverage will be generated in the `tests/code-coverage/` directory.
Notez que si vous n'utilisez pas l'option `-ncc` ; la couverture sera générée dans le répertoire `tests/code-coverage/`.
