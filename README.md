Organic Data Science
====================
Organic Data Science is a framework for scientific collaboration.  This is a MediaWiki extension that uses Semantic MediaWiki for organizing information.

For more information, visit http://www.organicdatascience.org

Installation Instrutions
========================

1) Install mediawiki

2) Install extensions:
* semantic mediawiki
* maps
* semantic maps
* bagit
* easyrdf

[wikidir] = Main wiki folder
// From a command prompt, go to [wikidir] and type the following commands:
```
curl -sS https://getcomposer.org/installer | php
php composer.phar require mediawiki/semantic-media-wiki "~2.3" --update-no-dev
php composer.phar require mediawiki/maps "*"
php composer.phar require mediawiki/semantic-maps "*"
php composer.phar require scholarslab/bagit "~0.2"
php composer.phar require easyrdf/easyrdf "*"
php maintenance/update.php
```

3) Go to [wikidir]/extensions, and clone the organicdatascience repository's Linked.Earth branch. Rename the organicdatascience directory to WorkflowTasks.

4) You will also need the PageObjectModel extension, that isn't maintained any more (We need to switch to something else later).

5) Append the following to LocalSettings.php :
```
# Enable uploads:
$wgEnableUploads = true;
$wgFileExtensions = array( 'png', 'gif', 'jpg', 'jpeg', 'jp2', 'webp', 'ppt', 'pdf', 'psd', 'mp3', 'xls', 'xlsx', 'swf', 'doc','docx', 'odt', 'odc', 'odp', 'odg', 'mpp', 'txt', 'zip', 'csv');
enableSemantics('wiki.linked.earth');

# Enable ODS Extension
require_once( "$IP/extensions/PageObjectModel/PageObjectModel.php" );
require_once( "$IP/extensions/WorkflowTasks/WorkflowTasks.php" );
$wgUseSimpleTasks = true;
$wgUseLiPD = true;
$wgDisableTracking = true;
//$wgShowExceptionDetails = true;

// Sparql Triple store Repository -- uncomment to enable mirroring in triple store
/*$smwgDefaultStore = 'SMWSparqlStore';
$smwgSparqlDatabaseConnector = 'fuseki';
$smwgSparqlQueryEndpoint = 'http://localhost:3030/ds/query';
$smwgSparqlUpdateEndpoint = 'http://localhost:3030/ds/update';
$smwgSparqlDataEndpoint = '';*/

// Use responsive skin
$wgVectorResponsive = true;

// Larger depth for SMW queries (default is 4)
$smwgQMaxDepth = 6;

// Group permissions
$wgGroupPermissions['*']['edit']    = false;
$wgGroupPermissions['*']['delete']    = false;
$wgGroupPermissions['*']['createtalk']    = false;
$wgGroupPermissions['*']['createpage']    = false;
$wgGroupPermissions['*']['createaccount'] = false;
$wgGroupPermissions['user']['edit']    = true;
$wgGroupPermissions['user']['createpage']    = false;
$wgGroupPermissions['sysop']['edit']    = true;
$wgGroupPermissions['sysop']['delete']    = true;
$wgGroupPermissions['sysop']['createpage']    = true;

// Lock project (policy/disclaimer etc) pages
$wgGroupPermissions['sysop']['editpolicy'] = true;
$wgNamespaceProtection[NS_PROJECT] = array( 'editpolicy' );

// Namespaces to be searched
$wgNamespacesToBeSearchedDefault = [
   NS_MAIN => true,
   NS_PROPERTY => true,
   NS_CATEGORY => true,
   NS_PROJECT => true,
];

// Linked Earth Groups & special rights
$wgGroupPermissions['visitor'] = array('createtalk'=>1);
#$wgGroupPermissions['contributor'] = array('createtalk'=>1, 'edit-page-text'=>1);
$wgGroupPermissions['basic-editor'] = array('createpage'=>1, 'createtalk'=>1,
        'edit-page-text'=>1, 'edit-page-metadata'=>1,
        'edit-ontology-text'=>1);
$wgGroupPermissions['advanced-editor'] = array('createpage'=>1, 'createtalk'=>1,
        'edit-page-text'=>1, 'edit-page-metadata'=>1,
        'edit-ontology-text'=>1, 'edit-ontology-semantics'=>1);
$wgGroupPermissions['editorial-board'] = array('createpage'=>1, 'createtalk'=>1,
        'edit-page-text'=>1, 'edit-page-metadata'=>1,
        'edit-ontology-text'=>1, 'edit-ontology-semantics'=>1,
        'edit-core-ontology'=>1, 'editpolicy'=>1);
```

** Use PHP version 7.0.  Use different instances of MySQL if you plan to maintain different versions of MediaWiki. 

6) In step 5 of instruction, comment - require_once( "$IP/extensions/WorkflowTasks/WorkflowTasks.php" );

7) Log into the wiki as Admin and click on the "Initialise or upgrade tables" button under Special:SemanticMediaWiki -> Administrative Functions page

8) Uncomment -> require_once( "$IP/extensions/WorkflowTasks/WorkflowTasks.php" );


9) Login as Admin and Load ontologies, from Special:WTBootstrapLE page using the link http://linked.earth/ontology

** Solving timeout error while loading ontologies, Make following changes–
In php.ini -
post_max_size = 750M
upload_max_filesize = 750M
max_execution_time = 0
max_input_time = 5000
memory_limit = 1000M

Add below code to phpmyadmin/config.inc.php - 
$cfg['ExecTimeLimit'] = 6000; 

Add below code on 1st line of file .\mediawiki-1.28.2\includes\libs\rdbms\database\ DatabaseMysqli.php -
ini_set('max_execution_time', 0);
