Règles de codage
================

Indentation
-----------

- 3 espaces
- Longueur maximale de la ligne : 100

.. code-block:: php

   <?php
   // base level
       // level 1
           // level 2
       // level 1
   // base level

Espaceement
-----------

Nous avons adopté les règles «d'espacement français» dans le code. La règle est la suivante :

* pour une ponctuation *simple* (``,``, ``.``) : utilisez un espace après le signe de ponctuation
* pour une ponctuation *double* (``!``, ``?``, ``:``) : utiliser un espace après et un espace avant le signe de ponctuation
* pour une ponctuation *d'ouverture* (``(``, ``{``, ``[``): utiliser *un espace avant* le signe de ponctuation
* pour une ponctuation *de fermeture* ( ``)``, ``}``, ``]``) : utiliser *un espace après* le signe de ponctuation, à l'exception d'une fin de ligne, lorsqu'elle est suivie par un point-virgule ( ;)

Bien sûr, cela ne concerne que le code source, pas les chaînes (chaînes traduisibles, commentaires,...) !

Structures de contrôle
----------------------

Plusieurs conditions dans plusieurs lignes identifiées


.. code-block:: php

   <?php
   if ($test1) {
      for ($i=0 ; $i<$end ; $i++) {
         echo "test ".( $i<10 ? "0$i" : $i )."<br>";
      }
   }
   
   if ($a==$b
      || ($c==$d && $e==$f)) {
     ...
   } else if {
     ...
   }
   
   switch ($test2) {
      case 1 :
         echo "Case 1";
         break;
   
      case 2 :
         echo "Case 2";
         // No break here : because...
   
      default :
         echo "Default Case";
   }


Tableaux
--------

Arrays must be declared using the short notation syntax (``[]``), long notation (``array()``) is forbidden.
Les tableaux doivent être déclarés en utilisant la syntaxe de notation courte (``[]``), la notation longue (``array()``) est interdite.

vrai, faux et nul
-----------------

Les constantes ``true``, ``false`` et ``null`` doivent être en minuscules.


Fichiers inclus
---------------

Utilisez ``include_once`` pour inclure le fichier une fois et émettre un avertissement si le fichier n'existe pas :

.. code-block:: php

   include_once GLPI_ROOT."/inc/includes.php";


Balises PHP
-----------

La balise courte (``<?``) n'est pas autorisée. Utilisez des balises complètes (``<?php``).


.. code-block:: php

   <?php
   // code

La balise PHP de fermeture ``?>`` doit être évitée sur les fichiers PHP complets (donc dans la plupart des fichiers de GLPI !).


Fonctions
---------

Les noms de fonction doivent être écrits en *camelCaps*:

.. code-block:: php

   <?php
   function userName($a, $b = 'foo') {
      //do something here!
   }

Les espaces après une parenthèse ouvrante ou avant une parenthèse fermante sont interdits. Pour les paramètres qui ont une valeur par défaut, ajoutez un espace avant et après le signe égal.

Si des paramètres sont ajoutés à un bloc de commentaires, veuillez vous reporter à la section `Comments`_ pour un exemple.

S'il s'agit de l'ajout d'une fonction parent

.. code-block:: php

   <?php
   function getMenuContent()

S'il s'agit d'une nouvelle fonction, ajoutez dans le bloc commentaires (voir la section `Commentaires`_ ):

.. code-block:: php

   @since version 9.1

Appeler des méthodes statiques
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=========================== ================
Localisation de la fonction Comment appeler
=========================== ================
class elle-même             ``self::theMethod()``
parent class                ``parent::theMethod()``
une autre classs            ``ClassName::theMethod()``
=========================== ================

Statique ou non statique ?
^^^^^^^^^^^^^^^^^^^^^^^^^^

Certaines méthodes du code source sont `déclarées comme statiques <http://php.net/manual/fr/language.oop5.static.php>`_ et d'autres pas.


Bien sûr, vous ne pouvez pas effectuer d'appels statiques avec une méthode non statique. Pour appeler une telle méthode, vous devez obtenir une instance d'objet et appeler la méthode dessus :

.. code-block:: php

   <?php

   $object = new MyObject();
   $object->nonStaticMethod();

Il peut y avoir différentes façons d'appeler des classes statiques. Dans ce cas. Vous pouvez :


* appeler statiquement la méthode depuis l'objet comme ``MyObject::staticMethod()``,
* appeler statiquement la méthode à partir d'une instance d'objet comme ``$object::staticMethod()``,
* appeler de manière non statique la méthode à partir d'une instance d'objet comme ``$object->staticMethod()``.
* utiliser un `late static building <http://php.net/manual/en/language.oop5.late-static-bindings.php>`_ comme ``static::staticMethod()``.

Quand vous n'avez pas encore d'instance d'objet, la première solution est probablement la meilleure. Pas besoin d'instancier un objet pour simplement appeler une méthode statique.

Toutefois, si vous avez déjà une instance d'objet, il est préférable d'utiliser n'importe quelle solution plutôt de la derniere. Enfin, vous économiserez des performances car cette façon de faire a un coût.

Classes
-------

Les noms de classe doivent être écrits dans CamelCase :


GLPI n'utilise pas les `espaces de noms PHP <http://php.net/manual/en/language.namespaces.php>`_ pour le moment. soyez donc prudent lorsque vous créez de nouvelles classes pour prendre un nom qui n'existe pas encore.

.. code-block:: php

   <?php
   class MyExampleClass estends AnotherClass {
      // do something
   }


Remarque : même si GLPI n’utilise pas d’espaces de noms, certaines bibliothèques les utilisent. Vous devrez vous en occuper. Vous pouvez également, si vous le souhaitez, utiliser des espaces de noms pour appeler des objets PHP.

Par exemple, le code suivant:

.. code-block:: php

   <?php
   try {
      ...
      $something = new stdClass();
      ...
   } catch (Exception $e{
      ...
   }


Pourrait aussi être écrit comme ceci (voir le ``\``):

.. code-block:: php

   <?php
   try {
      ...
      $something = new \stdClass();
      ...
   } catch (\Exception $e{
      ...
   }

Variables et Constantes
-----------------------

* Les noms de variables doivent être aussi descriptifs que possible, et rester clairs et concis.
* En cas de mots multiples, utilisez le séparateur ``_``,
* Les variables doivent être en **minuscules**,
* Les variables globales et les constantes doivent être des **majuscules**.


.. code-block:: php

   <?php
   $user         = 'glpi';
   // put elements in alphabetic order
   $users        = array('glpi', 'glpi2', 'glpi3');
   $users        = array('glpi1'   => 'valeur1',
                         'nexglpi' => array('down' => '1',
                                            'up'   => array('firstfield' => 'newvalue')),
                         'glpi2'   => 'valeur2');
   $users_groups = array('glpi', 'glpi2', 'glpi3');
   
   $CFG_GLPI = array();

commentaires
------------

Pour être plus visible, ne mettez pas de commentaires de bloc ``/* */`` mais commentez chaque ligne avec ``//``. Mettez les commentaires docblocks dans  ``/** */``.

Chaque fonction ou méthode doit être documentée, ainsi que tous ses paramètres (voir `Types de variables`_ ci-dessous) et son retour.

Pour chaque documentation de méthode ou de fonction, vous devez au moins avoir une description, la version qui a été introduite, la liste des paramètres, le type de retour. Chaque bloc est séparé par une ligne vide. Par exemple, pour une fonction void:

.. code-block:: php

   <?php
   /**
    * Describe what the method does. Be concise :)
    *
    * You may want to add some more words about what the function
    * does, if needed. This is optionnal, but you can be more
    * descriptive here:
    * - it does something
    * - and also something else
    * - but it doesn't make coffee, unfortunately.
    *
    * @since 9.2
    *
    * @param string  $param       A parameter, for something
    * @param boolean $other_param Another parameter
    *
    * @return void
    */
   function myMethod($param, $other_param) {
      //[...]
   }

Une autre manière d’être ajoutée si la fonction l'exige.


Reportez-vous au `site Web PHPDocumentor <https://phpdoc.org/docs/latest>`_ pour obtenir plus d'informations sur la documentation. La `dernière documentation de l'API GLPI <https://forge.glpi-project.org/projects/glpi/embedded/index.html>`_ est également disponible en ligne.

Veuillez suivre l'ordre défini ci-dessous:


 #. Description,
 #. Description longue, le cas échéant
 #. `@deprecated`.
 #. `@since`,
 #. `@var`,
 #. `@param`,
 #. `@return`,
 #. `@see`,
 #. `@throw`,
 #. `@todo`,

Documentation des paramètres
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Chaque paramètre doit être documenté dans sa propre ligne, en commençant par la balise @param, suivie des `types de Variables`_ , suivie du nom param (``$param``) et enfin de la description elle-même. 
Si votre paramètre peut être de différents types, vous pouvez les lister séparés d'un  ``|`` ou vous pouvez utiliser le type ``mixed``.

Tous les noms de paramètres et la description doivent être alignés verticalement sur le plus long (plus d'un caractère); voir l'exemple ci-dessus.

Méthode de substitution: @inheritDoc? @voir? docblock? pas de docblock?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Il y a beaucoup de questions sur la manière de documenter une méthode enfant dans une classe enfant.


Beaucoup d'éditeurs utilisent le tag ``{@inheritDoc}`` sans rien d'autre. **Ce n'est pas bon**. Ce tag *inline* est source de confusion pour de nombreux utilisateurs. Pour plus de détails, voir `PHPDocumentor documentation about it <https://www.phpdoc.org/docs/latest/guides/inheritance.html#the-inheritdoc-tag>`_.
L'utilisation de cette balise n'est pas interdite, mais assurez-vous de l'utiliser correctement, ou tout simplement de l'éviter. Un exemple d'utilisation:

.. code-block:: php

   <?php

   abstract class MyClass {
      /**
       * This is the documentation block for the curent method.
       * It does something.
       *
       * @param string $sthing Something to send to the method
       *
       * @return string
       */
      abstract public function myMethod($sthing);
   }

   class MyChildClass extends MyClass {
      /**
       * {@inheritDoc} Something is done differently for a reason.
       *
       * @param string $sthing Something to send to the method
       *
       * @return string
       */
      public function myMethod($sthing) {
         [...]
      }


Nous constatons souvent que l’utilisation de la balise ``@see`` fait référence à la méthode parente. **C'est faux**. La balise ``@see`` est conçue pour faire référence à une autre méthode qui aiderait à comprendre celle-ci, pas pour faire référence à son parent (vous pouvez également consulter la `documentation de PHPDocumentor <https://www.phpdoc.org/docs/latest/references/phpdoc/tags/see.html>`_ à ce sujet. Lors de la génération, la classe et les méthodes parentes sont automatiquement découvertes. Un lien vers le parent sera automatiquement ajouté.
Un exemple d'utilisation:

.. code-block:: php

   <?php
   /**
    * Adds something
    *
    * @param string $type  Type of thing
    * @param string $value The value
    *
    * @return boolean
    */
   public function add($type, $value) {
      // [...]
   }

   /**
    * Adds myType entry
    *
    * @param string $value The value
    *
    * @return boolean
    * @see add()
    */
   public function addMyType($value) {
      return $this->addType('myType', $value);
   }

Enfin, dois-je ajouter un docblock ou rien?


PHPDocumentor et divers outils n'utiliseront que docblock parent si rien n'est spécifié sur les méthodes enfant. Donc, si la méthode enfant agit comme son parent (étendre une classe abstraite, ou une super classe telle que ``CommonGLPI`` ou ``CommonDBTM``); vous pouvez simplement omettre complètement le docblock. L'alternative consiste à copier-coller docblock parent entièrement, mais de cette façon, il serait nécessaire de changer tous les docblocks enfants lorsque le parent sera changé.


Types de variables
------------------

Types de variables à utiliser dans DocBlocks pour Doxygen :



========= ===========
 Type     Description
========= ===========
mixed     Une variable de type indéfini (ou multiple)
integer   Variable de type entier (nombre entier)
float     Type de flotteur (numéro de point)
boolean   Type logique (vrai ou faux)
string    Type de chaîne (n'importe quelle valeur dans ``""`` ou ``' '``)
array     Type tableau
object    Type objet
ressource Type de ressource (tel que renvoyé par la fonction ``mysql_connect``)
========= ===========

Insérer un commentaire dans le code source de doxygen. 
Résultat : doc complet pour les variables, fonctions, classes...


Guillemets simples et doubles
-----------------------------

* Vous devez utiliser des guillemets simples pour les index, la déclaration des constantes, les traductions, ...
* Utilisez les guillemets doubles dans les chaînes traduites
* Lorsque vous devez utiliser le caractère de tabulation (``\t``), le retour chariot (``\n``), etc., vous devez utiliser des guillemets doubles.
* Pour des raisons de performances depuis PHP7, vous pouvez éviter la concaténation de chaînes.

Exemples :

.. code-block:: php

   <?php
   //for that one, you should use double, but this is at your option...
   $a = "foo";
   
   //use double quotes here, for $foo to be interpreted
   //   => with double quotes, $a will be "Hello bar" if $foo = 'bar'
   //   => with single quotes, $a will be "Hello $foo"
   $a = "Hello $foo";
   
   //use single quotes for array keys
   $tab = [
      'lastname'  => 'john',
      'firstname' => 'doe'
   ];
   
   //Do not use concatenation to optimize PHP7
   //note that you cannot use functions call in {}
   $a = "Hello {$tab['firstname']}";
   
   //single quote translations
   $str = __('My string to translate');
   
   //Double quote for special characters
   $html = "<p>One paragraph</p>\n<p>Another one</p>";
   
   //single quote cases
   switch ($a) {
      case 'foo' : //use single quote here
         ...
      case 'bar' :
         ...
   }


Fichiers
--------

* Nom en minuscule.
* Longueur de ligne maximale: 100 caractères
* Indentation: 3 espaces


Requêtes en base de données
---------------------------

* Les requêtes doivent être écrites sur plusieurs lignes, une instruction par ligne.
* All SQL words must be **UPPER case**.
* Tous les mots SQL doivent être en **majuscules**.
* Pour MySQL, tous les éléments doivent être protégés par  l'apostrophe inversée (nom de la table, nom du champ, condition),
* Toutes les valeurs de variables, même les nombres entiers doivent être entre guillemets simples

.. code-block:: php

   <?php
   $query = "SELECT *
             FROM `glpi_computers`
             LEFT JOIN `xyzt` ON (`glpi_computers`.`fk_xyzt` = `xyzt`.`id`
                                  AND `xyzt`.`toto` = 'jk')
             WHERE @id@ = '32'
                   AND ( `glpi_computers`.`name` LIKE '%toto%'
                         OR `glpi_computers`.`name` LIKE '%tata%' )
             ORDER BY `glpi_computers`.`date_mod` ASC
             LIMIT 1";
   
   $query = "INSERT INTO `glpi_alerts`
                   (`itemtype`, `items_id`, `type`, `date`) // put field's names to avoid mistakes when names of fields change
             VALUE ('contract', '5', '2', NOW())";

Vérification des normes
-----------------------

Afin de vérifier que certaines règles soient respectées, nous fournissons des règles `PHP CodeSniffer <http://pear.php.net/package/PHP_CodeSniffer>`_ personnalisées . Depuis le répertoire GLPI, lancez simplement:

.. code-block:: bash

   phpcs --standard=vendor/glpi-project/coding-standard/GlpiStandard/ inc/ front/ ajax/ tests/

Si la commande ci-dessus ne fournit aucune sortie, tout va bien :)


Un exemple d'erreur en sortie ressemblerait à ceci:


.. code-block:: bash

   phpcs --standard=vendor/glpi-project/coding-standard/GlpiStandard/ inc/ front/ ajax/ tests/
   
   FILE: /var/www/webapps/glpi/tests/HtmlTest.php
   ----------------------------------------------------------------------
   FOUND 3 ERRORS AFFECTING 3 LINES
   ----------------------------------------------------------------------
    40 | ERROR | [x] Line indented incorrectly; expected 3 spaces, found
       |       |     4
    59 | ERROR | [x] Line indented incorrectly; expected 3 spaces, found
       |       |     4
    64 | ERROR | [x] Line indented incorrectly; expected 3 spaces, found
       |       |     4
