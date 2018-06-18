# Directory Helper

The Directory Helper file contains functions that assist in working with directories.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('directory');
```

## Map directory

Reads the specified directory and builds an array representation of it. Sub-folders contained with the directory will be mapped as well.

```php
<?php
/**
 * @param	string	$source_dir		Path to source
 * @param	int	$directory_depth	Depth of directories to traverse
 *						(0 = fully recursive, 1 = current dir, etc)
 * @param	bool	$hidden			Whether to show hidden files
 * @return	mixed array|false
*/
var_dump($this->directory->map($GLOBALS['COD']->doc. 'uploads/'));
```

## Create directory

```php
<?php
/**
 * @param	string	$direction		Path to source
 * @param   int     $mode           Mode of the folder
 * @param   void    $recursive      Recursive mode
 *
 * @return	void
*/
var_dump($this->directory->create($GLOBALS['COD']->doc. 'uploads/', 0777, false));
```

## Delete directoru

```php
<?php
/**
 * @param	string	$direction		Path to source
 *
 * @return	void
*/
var_dump($this->directory->delete($GLOBALS['COD']->doc. 'uploads/');
```
