# Configuration

Inside of the folder /application/Config/ you can find multiple files to use better this PHP Framework.

## URL Settings

*url.settings.php*

This file is so important to be the first one to load, because will have all document root files that the Framework needs.

This variables can be edit.

> Base site url

```php
<?php
// Can set manually like the URL on is host your project, but if you want you can change with a real URL
// $GLOBALS['COD']->dir = 'http://codedmx.com/';
$GLOBALS['COD']->dir = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS']=='on' ? 'https' : 'http' ).'://'.$_SERVER['HTTP_HOST'].DS;
```

> Base site document root

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project, but if you want you can change it.
// $GLOBALS['COD']->doc = '/var/www/vhosts/codedmx/';
$GLOBALS['COD']->doc = str_replace("application/Config", "", realpath(dirname(__FILE__)));
```

> Base site document root for config folder

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project by Config folder.
// $GLOBALS['COD']->doc_config = '/var/www/vhosts/codedmx/application/Config/';
$GLOBALS['COD']->doc_config = $GLOBALS['COD']->doc . 'application' . DS . 'Config' . DS;
```

> Base site document root for view folder

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project by View folder.
// $GLOBALS['COD']->doc_view = '/var/www/vhosts/codedmx/application/View/';
$GLOBALS['COD']->doc_view = $GLOBALS['COD']->doc . 'application' . DS . 'View' . DS;
```

> Base site document root for Class folder

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project by Class folder.
// $GLOBALS['COD']->doc_class = '/var/www/vhosts/codedmx/system/Classes/';
$GLOBALS['COD']->doc_class = $GLOBALS['COD']->doc . 'system' . DS . 'Classes' . DS;
```

> Base site document root for Custom Class folder

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project by Custom_class folder.
// $GLOBALS['COD']->doc_custom = '/var/www/vhosts/codedmx/system/Custom_class/';
$GLOBALS['COD']->doc_custom = $GLOBALS['COD']->doc . 'system' . DS . 'Custom_class' . DS;
```

> Base site document root for Helper folder

```php
<?php
// Is about Document Root $_SERVER['DOCUMENT_ROOT'] of your project by Helper folder.
// $GLOBALS['COD']->doc_helper = '/var/www/vhosts/codedmx/system/Helper/';
$GLOBALS['COD']->doc_helper = $GLOBALS['COD']->doc . 'system' . DS . 'Helper' . DS;
```

> Default php file when you type your url 'https://codedmx.com'

```php
<?php
$GLOBALS['COD']->default = 'home';
```

> Default php file when you type your url 'https://codedmx.com' and the file can't be found

```php
<?php
// This file will find in /application/View/static/{$GLOBALS['COD']->not_found}.php
$GLOBALS['COD']->not_found = 'File';
```

## Analytics account

You have a variable to set your analytics id account to use it in the future.

```php
<?php
$GLOBALS['COD']->analytics = 'UA-XXXXXXX-X';
```

## Database settings

*db.settings.php*

This variables are to access your database. You can add more arrays with multiple databases configs.

Key | Description
-------------- | --------------
host | The hostname of your database server.
username | The username used to connect to the database.
password | The password used to connect to the database.
db | The name of the database you want to connect.
port | The port used to connect to the database.
charset | The character set used in communicating with the database.

```php
<?php
$GLOBALS['COD']->db = array(
	'default' => array(
	    'host' => 'localhost', // Required
	    'username' => '', // Required
	    'password' => '', // Required
	    'db'=> '', // Required
	    'port' => 3306,
	    'charset' => 'utf8'
	),
	'myotherdb' => array(
		'host' => '', // Required
	    'username' => '', // Required
	    'password' => '', // Required
	    'db'=> '', // Required
	    'port' => 3306,
	    'charset' => 'utf8'
	)
);
```

## Encrypt settings

*encrypt.settings.php*

Some people believe MD5 would be a safe way to encode passwords. Is a lie! MD5 is a good method to obscure non-sensitive data, because it's quite fast.

<aside class="warning">The default schema is $2y$, which makes use of the new, corrected hash implementation. This can be modify con CFG.php file</aside>

> Hash the passwords

```php
<?php
// Can be: 2a, 2x or 2y
$GLOBALS['COD']->identifier = '2y';
```

> Encrypt settings

```php
<?php
$GLOBALS['COD']->emethod = ''; // AES-256-CBC | aes-128-cbc, AES-128-CFB
$GLOBALS['COD']->ekey = ''; //qw1209132j
$GLOBALS['COD']->eiv = ''; // *123+1*23*
```

## Autoload Settings

*autoload.settings.php*

You can set autoload classes and helpers that you need in all the project, and you don't need to load it again in the future.

Key | Description
-------------- | --------------
db | Data base class
validate | Validator class
crypt | Encryptation class
mail | Mailing class
timeago | Timeago class
image | Image class
ftp | FTP class
upload | Uploader class
cache | Cache class
paginator | Paginator class
router | Router class
session | Session class
zip | Zip class

> You need to send array

```php
<?php
$GLOBALS['COD']->autoload = array();
// Can set db and session like autoload:
$GLOBALS['COD']->autoload = array('db', 'session');

// And you don't need to use this function to load the library
// $this->load->library('db');
// $this->load->library('session');
```

Key | Description
-------------- | --------------
array | Array helper
cookie | Cookie helper
directory | Directory helper
download | Download helper
html | HTML helper
security | Security helper
string | String helper
text | Text helper
url | URL helper
agent | User helper

> You need to send array

```php
<?php
$GLOBALS['COD']->helpers = array();
// Can set db and session like autoload:
$GLOBALS['COD']->helpers = array('array', 'url');

// And you don't need to use this function to load the helper
// $this->load->helper('array');
// $this->load->helper('url');
```

## FTP Settings

*ftp.settings.php*

This variables are to access your ftp connection.

Key | Description
-------------- | --------------
hostname | FTP Server hostname (http://codedmx.com OR 172.52.33.125)
username | FTP Username
password | FTP Password
port | FTP Server port
passive | Passive mode flag

```php
<?php
$GLOBALS['COD']->ftp = array(
	'hostname' => '',
	'username' => '',
	'password' => '',
	'port' => 21,
	'passive' => TRUE
);
```
