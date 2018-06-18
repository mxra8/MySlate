# Zip Class


> First of all you need to load the library file

```php
<?php
$this->load->library('zip');
```

## Adding new files to the zip / Creating new zip file:

> Multi-line style

```php
<?php
$this->zip->zip_start("path/to/new/or/old/zip_file.zip");
$this->zip->zip_add("path/to/example.png"); // adding a file
$this->zip->zip_add(array("path/to/example1.png","path/to/example2.png")); // adding two files as an array
$this->zip->zip_add(array("path/to/example1.png","path/to/directory/")); // adding one file and one directory
$this->zip->zip_end();
```

> One-line style

```php
<?php
$this->zip->zip_files(array("path/to/new/or/old/zip_file.zip","path/to/directory/"),"path/to/zip/file.zip");
```

## Extracting (unzipping) files:

> Multi-line style

```php
<?php
$this->zip->unzip_file("path/to/new/or/old/zip_file.zip");
$this->zip->unzip_to("path/to/containing/directory/");
```

> One-line style

```php
<?php
$this->zip->unzip_file("path/to/new/or/old/zip_file.zip","path/to/containing/directory/");
```
