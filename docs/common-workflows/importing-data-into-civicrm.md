Import de données dans CiviCRM
==============================

La plupart des organisations disposent de données à l'extérieur de CiviCRM,
telles que des bases de données utilisées précédemment, des tableaux créés à la
volée pour des événements spécifiques ou d'autres usages, et des répertoires
d'adresses e-mail. Parce qu'entrer de grandes quantités de données manuellement
peut être très fastidieux, CiviCRM propose une fonction d'import en masse si
la source peut exporter les données dans un format commun tel qu'un fichier CSV
(Comma Separated Version).

Les imports peuvent aussi être utilisés pour mettre à jour les données. Cette 
fonctionnalité sera abordée dans la section en fin de ce chapitre.

Considérations avant l'import
-----------------------------

Pour plus de détails à propos de l'organisation des données avant import dans
CiviCRM, veuillez lire la section "Organiser vos données", et spécialement
"Mapper vos données dans CiviCRM".

Préparation de l'import
-----------------------

L'import de données requiert une attention considérable et un grand soin, nous
allons donc présenter ici quelques concepts que vous devriez connaître avant de
commencer votre premier import. Vous pouvez importer à la fois des données de 
base et des données personnalisées pour les contacts, comme pour les événements,
les activités, les cotisations et les contributions. Ce chapitre se concentre
sur le processus d'import pour les contacts. Le processus pour les autres données
est similaire.

Il y a deux façons d'importer des données:

-   à partir de fichiers CSV. La plupart des applications de base de données et de
    tableur (par exemple OpenOffice.org Calc, Google Spreadsheets, Microsoft Excel)
    peuvent créer et manipuler des fichiers dans ce format. Il est souvent plus
    facile d'afficher et de nettoyer vos données quand elles sont dans un fichier
    CSV que lorsqu'elles sont dans votre ancienne base de données.

    Chaque colonne dans votre fichier CSV va correspondre à un champ dans CiviCRM, 
    il faut donc s'assurer d'utiliser une colonne différente pour chaque
    information.

    En fonction de votre pays ou région, les champs dans votre fichier CSV peuvent
    être séparés par des points-virgules (;) au lieu des virgules. Si c'est le cas,
    vous devrez modifier la valeur du champ Séparateur Import/Export dans les
    paramètres de Localisation de CiviCRM en allant dans le menu de navigation
    Administrer > Configurer > Paramètres Globaux > Localisation.

-   à partir d'une autre base de données SQL ou MySQL stockée sur le même serveur,
    en utilisant une requête SQL. (Cette option est seulement pour les utilisateurs
    avancés qui ont une bonne compréhension du serveur et de l'administration d'une
    base de données.)

Si vous n'avez pas une bonne compréhension de vos données existantes et de comment
elles seront converties en champs dans CiviCRM, vous allez ressentir une grande
frustration par rapport aux problèmes rencontrés lors de l'import des données.
Veuillez lire la description de chaque type de données dans les autres sections de
ce manuel CiviCRM et visiter la documentation en ligne CiviCRM pour plus
d'information:
[http://wiki.civicrm.org/confluence/display/CRMDOC/Importing+Data](http://wiki.civicrm.org/confluence/display/CRMDOC/Importing+Data)

Les règles et recommendations suivantes vont vous aider à importer vos données
avec un minimum de problèmes:

-   Testez toujours votre import de données avec un petit nombre d'enregistrements.
    Après l'import de ceux-ci, affichez-les dans CiviCRM afin de vous assurer que
    les données ont été importées correctement et fonctionnent comme prévu.
-   Il peut être utile de créer un contact de test qui contient tous les attributs
    que vous avez définis dans le jeu de données. Importez ce contact et vérifiez
    le résultat afin de vous assurer que toutes les données sont correctement
    représentées dans CiviCRM.
-   Lorsque vous mappez les colonnes ou les champs de vos données sources dans
    CiviCRM pendant l'import, CiviCRM peut sauvegarder ce mapping des champs dans
    une *import map* qui peut être réutilisée. Ceci est très utile pour importer
    des fichiers multiples utilisant la même structure. Pour réaliser cette
    sauvegarde, cliquez sur la case à cocher "Sauver ce mapping" en bas de
    l'écran Correspondance des Champs de l'assistant d'import et entrez un nom
    et une description appropriés. Pour réutiliser une *import map*,
    sélectionnez-la à partir du menu déroulant Charger un mapping existant sur
    l'écran Choix de la source de données (step 1) de l'assistant d'import.
-   Si vos imports prennent trop de temps, essayez de diviser les données sources
    en plusieurs lots plus petits. Si vous disposez des permissions appropriées
    sur votre serveur web server, vous pouvez aussi augmenter les paramètres
    memory_limit et max_execution_time values dans le fichier php.ini.
-   Vous pouvez ajouter tous les contacts importés à de nouveaux groupes et tags,
    ou à des existants. Tous les contacts d'un import donné recevront les mêmes
    groupes et tags. Cette limitation a certains effets sur votre import:
    -   Assurez-vous que les groupes et tags sont applicables à tous les contacts
        repris dans un imported. Si vous devez assigner des groupes et tags au
        cas par cas, alors il faut importer les contacts dans de petits lots
        reprenant les contacts qui partagent les mêmes groupes et tags. Une
        alternative est de créer des données personnalisées utilisables pour la
        recherche dans CiviCRM et qui contiennent les groupes et tags que vous 
        souhaitez assigner aux contacts importés. Après l'import vous pouvez
        lancer une recherche sur ces champs et utiliser la fonction "Ajouter les
        contact au Groupe" ou "Marquer les contacts" pour modifier en masse les
        contacts.
    -   Vous pouvez utiliser cette fonctionnalité pour gérer les imports. En effet
        vous pouvez ajouter les contacts à un nouveau groupe ou tag qui indique
        de quel lot de données les contacts faisaient partie, ce qui permet de les
        identifier facilement, et de pouvoir les traiter en masse si nécessaire.
-   CiviCRM stocke le prénom et le nom dans des champs séparés, ils doivent donc
    apparaître comme des colonnes séparées du fichier CSV. Il en va de même pour
    la ville et le code postal. La plupart des tableurs contiennent des outils qui
    automatisent le processus de séparation de texte sur plusieurs champs.
-   Assurez-vous que les noms de pays sont exprimés de la même façon que dans
    CiviCRM, par exemple 'United States' et pas 'United States of
    America', ou 'United Kingdom' et pas 'Britain'.
-   Si vous importez plusieurs emplacements, le premier sera considéré comme
    adresse principale. Vous pouvez déplacer les colonnes dans le fichier CSV afin
    de vous assurer le bon emplacement est considéré comme adresse principale.
    Vous pouvez également diviser votre import afin que certains enregistrements
    utilisent un type d'emplacement comme adresse principale, et d'autres en utilisent
    un différent.
-   Si vous importez des données dans un champ personnalisé à choix multiple (par
    exemple une case à cocher ou un radio-bouton) vos données sources peuvent être
    représentées soit par le libellé (ce qui est visible pour l'utilisateur dans
    CiviCRM) ou la valeur (ce qui est effectivement stocké dans la base de données
    pour cette sélection). CiviCRM reconnaît et importe le champ automatiquement.
    Lors de l'import dans un champ de base à choix multiple, vous ne pouvez utiliser
    que les valeurs, pas les libellés.
-   If you are updating multiple choice options, new values will replace
    the entire field. For example, if you update the value of the Colors
    field to be "orange" for a contact that currently has Colors set to
    "blue", the result will be that Colors is set to orange, not orange
    and blue.
-   Make sure your data source uses an accepted date format and that you
    select the same date format on the Choose Data Source screen of the
    import wizard.
-   Make sure any name prefixes and suffixes you use have been set up in
    the administration interface (go to: Administer > Option
    Lists****in the navigation menu).
-   If you plan to do additional imports of related data that's
    associated with your contact data, e.g. contribution data, event
    participation data, membership data, you can make things easier by
    ensuring that your contact records have unique IDs that are also
    associated with the related data. When you do the initial import of
    your contact data, import these unique IDs and map them to CiviCRM's
    External ID field, so that you can then use your original (or legacy
    data) IDs to match to the contact records records for later imports
    of the related data.
-   Master Address Belongs To is a special import field that only works
    with the CiviCRM_Address.id. The information needed to use this
    field for imports is only available directly from the MySQL database
    tables directly. They are not shown anywhere in CiviCRM including
    on data screens, link urls, profiles, or exports. [Information on
    how to use this special field is available in the
    Wiki](http://wiki.civicrm.org/confluence/display/CRMDOC/Importing+Data+-+Notes "CiviCRM Wiki - Importing Data").

Required Fields for Contact Imports
-----------------------------------

When preparing your data import it is helpful to know what fields are
required for Import. You'll want to be sure that these fields are
included in your CSV import file. Below is a list of the required
fields. They are marked red and starred in the interface. In case you
have less data, **selecting one field is enough**. The External
Identifier field is only useful if you want to update existing contacts.
Please note that the field with the identifier **(Match to Contact)**is
required for deduplication purposes.

-   **Email (Match to Contact)**
-   **External Identifier**
-   **First Name**
-   **Last Name**

Setting up a CSV file for importing
-----------------------------------

Example of spreadsheet .csv format

![student_sample](../img/CiviCRM-student_sample-en.png)

When thinking about setting up your spreadsheet, think about the data
that you are collecting and plan out your column headings. Keep in mind
that you may need to create more than one .csv file and perform multiple
imports before you are finished.

If you plan to import related data that pertains to a specific contact,
e.g. event participant information, contribution data, etc., you will
need to make sure that each contact record has a unique identifier or
the contact record should have First Name, Last Name and Email, so that
you can link their related data during later imports. If you have
unique ID, you would map the ID to CiviCRM's External Identifier on
import.

Running an import
-----------------

The import process has four steps.

### Step 1: Setup

Setup lets you specify the basic details of your import, including the
source of the data. Data can come from either a CSV file, or an SQL
query of a database on your server. A check-box lets you indicate
whether the first row of your file contains column headers.

![image](../img/Screen%20Shot%202015-04-29%20at%203.54.21%20PM.png)

Note that imports use the default **unsupervised** rule to decide
whether a contact record is a duplicate (refer to the *Deduping and
Merging* chapter in this section for information on duplicate matching
rules in CiviCRM). You can specify what action to take when an import
encounters a duplicate:

-   **Skip**: skip the duplicate contact, i.e. leave the original record
    as it is.
-   **Update**: update the original record with the database fields from
    the import data. Fields that are not included in the import data
    will be left as they are.
-   **Fill**: fill in the additional contact data, if it contains fields
    that are missing or blank in the original records, and leave fields
    which currently have values as they are.
-   **No Duplicate Checking**: this inserts all valid records without
    comparing them to existing contact records for possible duplicates.

![image](../img/Import%20Options.png)

**Import mappings** tell CiviCRM how the fields of data in your import
file correspond to the fields in CiviCRM. The first time you import from
a particular data source, it's a good idea to check the box to "Save
this field mapping" at the bottom of the page before continuing. The
saved mapping can then be easily reused the next time similar data is
imported, by requesting that it be loaded at this step.

### Step 2: Match the fields

If you had column headings in your file, these headings will appear in
the first column on the left-hand side of the Field Map, while the next
two columns show two rows of data in your file to be imported, and the
fourth column is the Matching CiviCRM Field. If you loaded an import
mapping in Step 1, your choices will be reflected here. You can change
them if they are inappropriate for this import.

![ImportMatchFields](../img/CiviCRM_update-CiviCore-ImportMatchFields-en.png "ImportMatchFields")

The matching CiviCRM fields include standard CiviCRM data such as First
Name and Last Name as well as any custom data fields that have been
configured for use with contact records on your site. Match the fields
by clicking the dropdown list and selecting the appropriate data. For
example, if the heading of the second column in your input is Surname,
you should choose Last Name as your Matching CiviCRM Field.

Select "- do not import -" for any columns in the import file that you
don't want to import into CiviCRM.

If you have a saved mapping for a specific set of spreadsheet columns,
and your spreadsheet layout has changed (for instance, you need to
import additional fields, so you add the appropriate columns of data in
the spreadsheet), you can modify and save the field mapping. One tip to
ease the mapping process when you need to import additional fields is to
place the additional columns of data in your import spreadsheet to the
right of the columns you've previously mapped in CiviCRM. This allows
you to use the existing saved field mapping to map the initial import
fields, and then continue mapping the new data fields.

![Step2d](../img/CiviCRM-AddingImporting-Step2d-en.png "Step2d")

Note that if you add new data columns in your spreadsheet and do not
position the columns AFTER the columns you previously mapped, you then
can't use the saved mapping and will have to map all your import fields
again.

Once you've mapped your fields, you can decide if you want to keep the
original saved mapping unchanged, or check the box to "Update this field
mapping" to include the new field mappings.

### Step 3: Preview

This screen previews the results of importing your data, reports the
number of rows to be imported, and allows you to double check your field
matches.

If some of the rows in your spreadsheet contain data that doesn't match
CiviCRM's requirements for one or more fields, you'll see an error
message with a count of the invalid rows (see the screenshot below).
Click the Download Errors link and review the errors reported in the
downloaded file, so you can fix them before doing the import.

![ImportPreviewErrs](../img/CiviCRM_update-CiviCore-ImportPreviewErrs-en.png "ImportPreviewErrs")

At the bottom of the form, you can choose to add the contacts to an
existing group, import to a new group, create a new tag, or tag imported
records. Adding imported records to a separate group is strongly
recommended in order to be able to quickly find the imports and, if
necessary, delete and reimport them.

![Step3b_1](../img/CiviCRM-AddingImporting-Step3b_1-en.png "Step3b_1")

### Step 4: Summary

The final screen reports the successful imports along with Duplicate
Contacts and Errors. If you have set the import to add all contacts to a
Group or Tag, you can click through to see your imported contact
records.

![Step4a_2](../img/CiviCRM-AddingImporting-Step4a_2-en.png "Step4a_2")

At this point it makes sense to check to make sure that your import has
worked as expected. Search for the contacts that you just imported and
examine their fields and custom data to make sure all is as expected.

Importing relational data
-------------------------

We have just described the process of importing one data file. But what
about if you want to import related data, like organizational addresses
with employees, parent child relationships, activities, contributions,
etc.? For each type of data you want to import, you will need to import
a separate CSV file.

CiviCRM has specific tools for importing related contact data and a set
of specific import tools for contributions, memberships, event
participation etc. (and you should see specific chapters for details of
how to use these tools). To import relationships, you should run
multiple contact imports.

For example if we want to import data for children and then for both
parents, we run three imports, one for the child, one for the father and
one for the mother.

We first import the child remembering to include an external identifier
that we can use to match the child to their parents. We then import the
father, and then the mother, as related contacts, linking them to the
child using the child's external identifier.

In the example below we have one CSV file which contains father and
mother information. We use this CSV file twice as part of the import.
Have a look at the fields below to understand what is happening.

![Parent1a](../img/CiviCRM-AddingImporting-Parent1a-en.png "Parent1a")

![Parent1b](../img/CiviCRM-AddingImporting-Parent1b-en.png "Parent1b")

We are linking the father to the original child using the external
identifier and are then importing the related father name using the
'Child of' relationship type.

When the import is done, go back and verify the data by searching for
the parent and examining the relationship tab. They should have a
relationship linking them to the child.

You can then repeat this process for the mother, and also for other
relationships as necessary.

Address standardisation
------------------------

For many organisations, an important element of cleaning your data is
standardising addresses. In the US, this means conform to conventions
defined by the United States Postal Service's Standards for Addresses.
Standardising how addresses are entered into CiviCRM will allow for more
accurate search results when searching by address, as CiviCRM can parse
addresses based on the USPS standards if you choose to do so. To find
out more about how Address Parsing is handled and used in CiviCRM, refer
to the Installation chapter of the Configuration section of this manual.
When adding or editing contacts, you will enter and edit such address
elements as street number, street name, and Apt/Unit/Suite number
according to these standards.

Import Activities
-----------------

When preparing your data import it is helpful to know what fields are
required for Import. You'll want to be sure that these fields are
included in your CSV import file. Below is a list of the required
fields. The Contact ID field is used to cross reference and attach the
activity to the contact so it must match the contact ID of the contact
in the system exactly.

-   Activity Date
-   Activity Type IDs
-   Activity Type Label
-   Contact ID (Match to Contact)
-  Subject

The import tool for Activities is similar to that of contacts, but there
are some pre-requisites which must be met before running the import.
Firstly, Activities cannot be imported unless the contacts and Activity
Types already exist in the database. If you need to import Activities
for contacts that are not yet available, run a contact import first,
preferably including a unique external identifier (most often an ID
assigned by the database or application you are importing records from).

Remember, CSV files must be less than 2MB in size. If the file size
exceeds this, create multiple CSV files and distribute the data between
them.

Import Contributions
--------------------

You can insert new contributions or update existing ones.

If you **insert new contributions,** your CSV file must include at least
the following fields:

-   Contact Id or External Identifier or all the fields used in your
    Unsupervised Duplicate Matching rule (to match to an existing
    contact)
-   Financial Type
-   Total Amount

If you want to **update existing contributions,** your CSV file must
include at least the following fields:

-   Transaction ID or Invoice ID or Payment ID (to match to an existing
    contribution)
-   Financial Type
-   Total Amount

You can use also use **update existing contributions** to import new or
change existing data in other core or custom contribution fields. When
doing this you will still need to include an ID to match to an existing
contribution and the Financial Type and Total Amount fields in you CSV
file, even if the values you import for those fields are no different
from the values already in your database.

Import Memberships
------------------

You can insert new memberships or update existing memberships.

If you **insert new memberships** your CSV file must include at
least the following fields:

-   Contact Id or External Identifier or all the fields used in your
    Unsupervised Duplicate Matching rule (to match to an existing
    contact)
-   Membership Type
-   Membership Start Date

If you want to **update existing memberships** your CSV file must
include at least the following fields:

-   Membership Id (to match to an existing membership)
-   Membership Type
-   Membership Start Date

You can use also use **update existing memberships** to import new or
change existing data in other core or custom membership fields. When
doing this you will still need to include Membership ID to match to an
existing membership, and the Membership Type and Membership Start Date
fields in you CSV file, even if the values you import for those fields
are no different from the values already in your database.

Import Participants
-------------------

In each import session you can either insert new registrations or update
existing participant records.

If you **insert new registrations**you need to decide whether to
restrict registrations for each event to just one per person (set **On
duplicate entries** to **Skip)** or to allow duplicate registrations for
the same event from a given contact (set **On duplicate entries** to
**No Duplicate Checking)**. In either case your CSV file must include
at least the following fields:

-   Contact Id or External Identifier or all the fields used in your
    Unsupervised Duplicate Matching rule (to match to an existing
    contact)
-   Event ID
-   Participant Status

If you want to **update existing registrations,** you should set **On
duplicate entries** to **Update**. Your CSV file must include at least
the following fields:

-   Participant ID (to match to an existing registration)
-   Event ID or Event Title
-   Participant Status

You can use also use **update existing registrations** to import new or
change existing data in custom participant fields. When doing this you
will still need to include Participant ID to match to an existing
registration, and the Event ID or Event Title and Participant Status
fields in you CSV file, even if the values you import for those fields
are no different from the values already in your database.

Import Tags
------------

There is currently no inbuilt way of importing tags or tag sets. You can
use [this advanced
extension](https://civicrm.org/extensions/api-csv-import-gui "API csv Import GUI")
though.

If you want to assign individual tags during your contacts import, you
will have to either:

-   split your CSV file by individual tags and import each subset
    separately as described above,
-   create temporary custom fields and import tags into them as standard
    data, then after the import use advanced searches to isolate
    contacts with particular values and mass tag them. Once you're done,
    you can remove the custom fields.
