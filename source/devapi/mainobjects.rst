Framework principal des objects
-------------------------------

GLPI contient de nombreuses classes mais il y a quelques objets communs que vous devez connaître. Toutes les classes GLPI sont dans le répertoire ``inc``.

CommonGLPI
^^^^^^^^^^

C'est **le principal** objet GLPI. La plupart des classes de GLPI ou des Plugins héritent de celui-ci, directement ou non. La classe est dans le fichier ``inc/commonglpi.class.php``.

Cet objet vous aidera à :

* gérer le nom du type d'élément,
* gérer les éléments des onglets,
* gérer les éléments des menus,
* faire un peu d'affichage,
* obtenir des URL (formulaire, recherche, ...),
* ...

Consultez la `documentation complète de l'API pour l'objet CommonGLPI  <https://forge.glpi-project.org/apidoc/class-CommonGLPI.html>`_ pour obtenir une liste complète des méthodes fournies.

CommonDBTM
^^^^^^^^^^

Ceci est un objet permettant de gérer tout les éléments de base de données. Il hérite bien sûr de `CommonGLPI`_. La classe est dans le fichier ``inc/commondbtm.class.php``.

Il vise à gérer la persistance de la base de données et des tables pour tous les objets et vous aidera à :

* ajouter, mettre à jour ou supprimer des lignes de base de données,
* charger une ligne de la base de données,
* récupèrer les informations des tables (nom, index, relations, ...)
* ...

L'objet CommonDBTM fournit plusieurs :doc:`hooks <../plugins/hooks>`.

Consultez la `documentation complète de l'API pour l'objet CommonDBTM <https://forge.glpi-project.org/apidoc/class-CommonDBTM.html>`_ pour une liste complète des méthodes fournies.


CommonDropdown
^^^^^^^^^^^^^^

Cette classe a pour objectif de gérer les listes déroulantes d'éléments de la base de données. Elle hérite de `CommonDBTM`_. Cette classe est dans le fichier ``inc/commondropdown.class.php``.

Cela vous aidera à :

* gérer les listes,
* importer les données,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonDropdown <https://forge.glpi-project.org/apidoc/class-CommonDropdown.html>`_pour obtenir une liste complète des méthodes fournies.


CommonTreeDropdown
^^^^^^^^^^^^^^^^^^

Cette classe a pour objectif de gérer les listes arborescentes d'éléments de la bases de données. Elle hérite de `CommonDropdown`_. Cette classe est dans le fichier ``inc/commontreedropdown.class.php``.

Cela vous aidera principalement à gérer l’aspect arbre d’un menu déroulant (parents, enfants, etc.).

Consultez la `documentation complète de l'API pour l'objet CommonTreeDropdown <https://forge.glpi-project.org/apidoc/class-CommonTreeDropdown.html>`_ pour une liste complète des méthodes fournies.


CommonImplicitTreeDropdown
^^^^^^^^^^^^^^^^^^^^^^^^^^

Cette classe gère des listes d'arborescence qui ne peuvent pas être gérées par l'utilisateur. Elle hérite de `CommonTreeDropdown`_. Cette classe est dans le fichier ``inc/commonimplicittreedropdown.class.php``.

Consultez la `documentation complète de l'API pour l'objet CommonTreeDropdown <https://forge.glpi-project.org/apidoc/class-CommonTreeDropdown.html>`_ pour une liste complète des méthodes fournies.


CommonDBVisible
^^^^^^^^^^^^^^^

Cette classe aide à la gestion de la visibilité. Il hérite de `CommonDBTM`_. La classe est dans le fichier ``inc/commondbvisible.class.php``.

Il fournit des méthodes pour:

* savoir si l'utilisateur peut voir l'élément,
* obtenir les paramètres des listes déroulants,
* ...


Consultez `la documentation complète de l'API pour l'objet CommonDBVisible <https://forge.glpi-project.org/apidoc/class-CommonDBVisible.html>`_ pour une liste complète des méthodes fournies.


CommonDBConnexity
^^^^^^^^^^^^^^^^^

Cette classe factorise les relations et les héritages. Elle hérite de `CommonDBTM`_. Elle est dans le fichier ``inc/commondbconnexity.class.php``.

Elle n'est pas conçu pour être utilisée directement, voir `CommonDBChild`_ et `CommonDBRelation`_.

Consultez la `documentation complète de l'API pour l'objet CommonDBConnexity <https://forge.glpi-project.org/apidoc/class-CommonDBConnexity.html>`_ pour obtenir une liste complète des méthodes fournies.

CommonDBChild
^^^^^^^^^^^^^

Cette classe gère les relations simples. Elle hérite de `CommonDBConnexity`_ . Cette classe est dans le fichier ``inc/commondbchild.class.php``.

Cet objet vous aidera à définir et à gérer les relations parent/enfant.

Consultez la `documentation complète de l'API pour l'objet CommonDBChild <https://forge.glpi-project.org/apidoc/class-CommonDBChild.html>`_ pour obtenir une liste complète des méthodes fournies.


CommonDBRelation
^^^^^^^^^^^^^^^^

Cette classe gère les relations. Elle hérite de `CommonDBConnexity`_. Cette classe est dans le fichier ``inc/commondbrelation.class.php``.

Contrairement à `CommonDBChild`_, il est conçu pour déclarer des :ref:`relations plus complexes telles que définies dans le modèle de base de données <complex-relations>`. C’est donc plus complexe que de simplement utiliser une relation simple mais cela offre aussi beaucoup plus de possibilités.

Pour configurer une relation complexe, vous devez définir plusieurs propriétés, telles que:

* ``$itemtype_1`` and ``$itemtype_2``; pour définir les deux types d'items utilisés;
* ``$items_id_1`` and ``$items_id_2``; pour définir le nom d'identifiant de champ.

D'autres propriétés vous permettent de configurer la gestion des héritages d'entités, ACL, de quoi se connecter sur chaque partie de différentes plusieurs actions, et ainsi de suite.


L'objet vous aidera également à:

* obtenir des options de recherche et de requête,
* trouver des droits dans la liste des ACL,
* gérer des actions massives,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonDBRelation <https://forge.glpi-project.org/apidoc/class-CommonDBRelation.html>`_ pour obtenir une liste complète des méthodes fournies.


CommonDevice
^^^^^^^^^^^^

Cette classe prend en compte les exigences communes relatives aux périphériques. Elle hérite de `CommonDropdown`_. Cette classe est dans le fichier ``inc/commondevice.class.php``.

Cela vous aidera à :


* l'importation, d'appareils,
* gérer les menus,
* faire un peu d'affichage,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonDevice <https://forge.glpi-project.org/apidoc/class-CommonDevice.html>`_ pour obtenir la liste complète des méthodes fournies.


Objets ITIL Communs
^^^^^^^^^^^^^^^^^^^

Tous les objets ITIL Communs vous aideront à gérer les objets ITIL <https://en.wikipedia.org/wiki/ITIL>`_ (tickets, modifications, problèmes).


CommonITILObject
++++++++++++++++

Gére les objets ITIL. Hérite de `CommonDBTM`_. Cette classe est dans le fichier ``inc/commonitilobject.class.php``.

Cela vous aidera à :

* obtenir des utilisateurs, des fournisseurs, des groupes, ...
* les compter,
* obtenir des objets pour les utilisateurs, techniciens, fournisseurs, ...
* obtenir le statut,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonITILObject  <https://forge.glpi-project.org/apidoc/class-CommonITILObject.html>`_pour une liste complète des méthodes fournies.


CommonITILActor
+++++++++++++++

Gére les acteurs ITIL. Hérite de `CommonDBRelation`_. Cette classe est dans le fichier ``inc/commonitilactor.class.php``.

Cela vous aidera à :

* obtenir des acteurs,
* afficher les notifications,
* obtenir des ACL,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonITILActor <https://forge.glpi-project.org/apidoc/class-CommonITILActor.html>`_ pour obtenir une liste complète des méthodes fournies.


CommonITILCost
++++++++++++++

Traiter les coûts ITIL. Hérite de `CommonDBChild`_. Cette classe est dans le fichier ``inc/commonitilcost.class.php``.

Cela vous aidera à :

* obtenir le coût de l'article,
* faire un peu d'affichage,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonITILCost <https://forge.glpi-project.org/apidoc/class-CommonITILCost.html>`_ pour une liste complète des méthodes fournies.


CommonITILTask
++++++++++++++

Gére les tâches ITIL. Hérite de `CommonDBTM`_. Cette classe est dans le fichier ``inc/commonitiltask.class.php``.

Cela vous aidera à :

* gérer les tâches ACL,
* faire un peu d'affichage,
* obtenir des options de recherche,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonITILTask  <https://forge.glpi-project.org/apidoc/class-CommonITILTask.html>`_ pour une liste complète des méthodes fournies.


CommonITILValidation
++++++++++++++++++++

Gére le processus de validation ITIL. Hérite de `CommonDBChild`_. Cette classe est dans le fichier ``inc/commonitilvalidation.class.php``.

Cela vous aidera à :

* Gèrer les ACL,
* obtenir et définir le statut,
* obtenir des comptes,
* faire un peu d'affichage,
* ...

Consultez la `documentation complète de l'API pour l'objet CommonITILValidation <https://forge.glpi-project.org/apidoc/class-CommonITILValidation.html>`_ pour une liste complète des méthodes fournies.
