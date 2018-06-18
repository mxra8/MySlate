# Loader

CodeDmx Framwork has a master class that can help you to make easy the code that you need

## Load library

Load library it called in __construct function to know if you set some class into autoload config.

For example:

You have a database, and you want to use it only in a php file.

> You need to use this

```php
<?php
$this->load->library('db');

// Then
$this->db->get('users');
```

> If you set this library in autoload config file, you only need to use the method.

```php
<?php
$this->db->get('users');
```

## Load view

Load a file from your server it's so easy, if the file path doesn't exists, will return not found file alert:

> You need to use this

```php
<?php
$this->load->view('my_file');
```

> You can set the path of the file:

```php
<?php
//$file, $path
$this->load->view('my_file', $GLOBALS['COD']->doc . 'system' . DS);
```

> You can send arguments into an array

```php
<?php
// $file, $path, $args
$array = array(
    'test' => 1,
    'other' => 2
);
$this->load->view('my_file', null, $array);
```

> You can set what is the extension of the file, by default is php

```php
<?php
// $file, $path, $args, $extension
$this->load->view('my_file', null, array(), 'html');
```

## Load helper

Like load a library, you can autoload a helper file

> You need to use this

```php
<?php
$this->load->helper('url');

echo $this->url->get_current_URL();
```

## Load custom class

We know that you'll have some custom classes, well, you have a folder to save your classes, and you can call them like this

> You need to use this

```php
<?php
/**
 * @param string $filename // File name in custom class
 * @param string $classname // Class name
 * @param string $new_name // What will be it new name ($this->my_class->method())
*/
$this->load->custom('myclass', 'MyClass', 'myclass');

echo $this->myclass->method();
```

## Method extend

When you create a custom class you need to extends all COD_Loader class

> Like this, imagine you create user.class.php

```php
<?php
class Users extends COD_Loader {
    function __construct() {
        $this->extend();
        $this->load->library('db');
    }

    function get_users() {
        return $this->db->get('users');
    }
}
```

> And you have an index file

```php
<?php
$this->load->custom('user.class', 'Users', 'users');

print_r($this->users->get_users());
```

## Construct

The __construct method firstly know if you set autoload libraries, then check if you set autoload helpers, then Load routers
