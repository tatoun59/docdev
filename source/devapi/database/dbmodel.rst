.. _dbmodel:

Modèle de base de données
-------------------------

La base de données GLPI actuelle contient plus de 250 tables. Le but de cette documentation est de vous aider à comprendre la logique du projet et non de détailler chaque table.

Comme dans toute base de données, il y a des tables, des relations entre elles (plus ou moins complexes). Certaines relations ont des descriptions stockées dans une autre table. Certaines tables sont liées entre elles... Tout ceci est assez habituel. :-) Commençons par un exemple simple :

.. image:: ../images/db_model_computer.png
   :alt: Computer model part
   :align: center

.. note::

   Le schéma ci-dessus est un exemple. Il est loin d'être complet !

Ce que nous pouvons voir ici :

* les ordinateurs sont directement liés aux systèmes d'exploitation, aux versions des systèmes d'exploitation, aux architectures des systèmes d'exploitation, ...,
* les ordinateurs sont liés aux mémoires, aux processeurs et aux moniteurs à l'aide d'une table de relations (qui permet dans ce cas de relier ces composants à d'autres éléments qu'un ordinateur),
* les mémoires ont un type.

Comme indiqué dans la note ci-dessus, ceci est loin d'être complet ; mais cela est assez représentatif de l'ensemble du schéma de base de données.

Ensembles de résultats
^^^^^^^^^^^^^^^^^^^^^^

Tous les ensembles de résultats renvoyés à partir de la base de données GLPI doivent toujours être des tableaux associatifs.

.. _dbnaming_conventions:

Conventions de nommage
^^^^^^^^^^^^^^^^^^^^^^

Tous les noms de tables et de champs sont en minuscules et suivent la même logique. Si vous ne respectez pas cela, GLPI ne trouvera pas les informations pertinentes.

Les tables
++++++++++

Les noms de tables sont liés aux noms de classes PHP. ils portent tous le préfixe ``glpi_`` et le nom de la classe est défini au pluriel. Les tables de plugins doivent être préfixées par ``glpi_plugin_``, suivi du nom du plugin, d'un autre tiret, puis du nom de la classe au pluriel.

Quelques exemples :

========================  ================================
Nom de la classe PHP	     Nom de la table
========================  ================================
``Computer``              ``glpi_computers``
``Ticket``                ``glpi_tickets``
``ITILCategory``          ``glpi_itilcategories``
``PluginExampleProfile``  ``glpi_plugin_example_profiles``
========================  ================================

Les champs
++++++++++

.. warning::

   Chaque table **doit** avoir une clé primaire auto-incrémentée nommée ``id``.


La dénomination des champs dépend principalement de vous excepté pour les identifiants et les clés étrangères. Restez clair et concis !

Pour ajouter une clé étrangère, utilisez simplement le nom de la table étrangère sans glpi_préfixe et ajoutez un suffixe ``_id``.

.. warning::

   Même si l'ajout d'une clé étrangère dans une table devrait être parfaitement correct, ce n'est pas la façon habituelle de faire les choses dans GLPI, voir `Créer des relations`_ pour en savoir plus.

Quelques exemples :

================================  ==============================
Nom de la table	                 Nom du champ de la clé étrangère
================================  ==============================
``glpi_computers``                ``computers_id``
``glpi_tickets``                  ``tickets_id``
``glpi_itilcategories``           ``itilcategories_id``
``glpi_plugin_example_profiles``  ``plugin_example_profiles_id``
================================  ==============================

.. _complex-relations:

Faire des relations
+++++++++++++++++++

Souvent, vous souhaiterez créer un lien entre plusieurs éléments. Supposons que vous souhaitiez lier un `ordinateur`, une `imprimante` ou un `téléphone` à un `composant de mémoire`. Vous devez ajouter des clés étrangères dans les tables d'éléments mais sur quelque chose d'aussi énorme que GLPI, ce n'est peut-être pas une bonne idée.

Au lieu de cela, créez une table de relations, qui référencera le composant de mémoire avec un identifiant d'élément et un type, comme par exemple:

.. code-block:: SQL

   CREATE TABLE `glpi_items_devicememories` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `items_id` int(11) NOT NULL DEFAULT '0', 
      `itemtype` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL,
      `devicememories_id` int(11) NOT NULL DEFAULT '0',
      PRIMARY KEY (`id`),
      KEY `items_id` (`items_id`),
      KEY `devicememories_id` (`devicememories_id`),
      KEY `itemtype` (`itemtype`,`items_id`),
   ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

Encore une fois, ceci est un exemple très simplifié de ce qui existe déjà dans la base de données, mais vous avez compris le principe ;-)

Dans cet exemple, ``itemtype`` serait ``Computer``, ``Printer`` ou ``Phone`` et ``items_idle``, ``l'id`` de l'élément lié.


Les index
^^^^^^^^^

Pour obtenir des performances correctes en interrogeant la base de données, vous devrez vous occuper de la configuration de certains index. Il est absurde d'ajouter des index sur tous les champs de la base de données mais certains d'entre eux doivent être définis :

* champs de clé étrangère,
* champs qui sont très souvent utilisés (par exemple des domaines tels que ``is_visible``, ``itemtype``, ...),
* clés primaires ;-)

Vous devriez simplement utiliser le nom du champ comme nom de clé.


