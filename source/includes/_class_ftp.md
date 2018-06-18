# FTP Class

CodeDmx has a FTP Class, that permits files to be transferred to a remote server. By this class you can move, rename and delete files or folders.

<aside class="warning">SFTP AND SSL FTP protocols are not supported.</aside>

If you prefer you can store your FTP preferences in a config file in /application/Config/ftp.settings.php, and this will be used automatically.

> First of all you need to load class

```php
<?php
$this->load->library('ftp');

//Then
$this->ftp->connect();
```

## Connect to host

Connects to the FTP server. Connection preferences are set by passing an array to the function, or you can store them in the config file in /application/Config/ftp.settings.php.

```php
<?php

$this->ftp->connect();
```

## Upload a file

Uploads a file to your server. You must supply the local path and the remote path, and you can optionally set the mode and permissions. Example:

```php
<?php
$this->ftp->put('/local/path/to/myfile.html', '/public_html/myfile.html');
```

### Parameters:
Variable | Definition | Default value
-------------- | -------------- | --------------
$local_path (string) | Local file path | n/a
$remote_path (string) | Remote file path | n/a
$mode (string) | FTP mode (options are: ‘auto’, ‘binary’, ‘ascii’) | 'auto'
$permissions (int) | File permissions (octal) | '0777'

## Download a file

Downloads a file from your server. You must supply the remote path and the local path, and you can optionally set the mode. Example:

```php
<?php
$this->ftp->download('/public_html/myfile.html', '/local/path/to/myfile.html');
```

### Parameters:
Variable | Definition | Default value
-------------- | -------------- | --------------
$remote_path (string) | Remote file path | n/a
$local_path (string) | Local file path | n/a
$mode (string) | FTP mode (options are: ‘auto’, ‘binary’, ‘ascii’) | 'auto'

## Delete a file

Supply the source path with the file name.

```php
<?php
$this->ftp->delete_file('/public_html/joe/blog.html');
```

## Delete a folder

Supply the source path to the directory with a trailing slash.

<aside class="warning">Be VERY careful with this method! It will recursively delete everything within the supplied path, including sub-folders and all files. Make absolutely sure your path is correct. Try using list() first to verify that your path is correct.</aside>

```php
<?php
$this->ftp->delete_dir('/public_html/path/to/folder/');
```

## List files

Permits you to retrieve a list of files on your server returned as an array. You must supply the path to the desired directory.

```php
<?php
$list = $this->ftp->list('/public_html/');
print_r($list);
```

## Create directory

Lets you create a directory on your server. Supply the path ending in the folder name you wish to create, with a trailing slash.

Permissions can be set by passing an octal value in the second parameter.

```php
<?php
// Creates a folder named "bar"
$this->ftp->mkdir('/public_html/foo/bar/', 0755);
```

## Rename a file

Permits you to rename a file. Supply the source file name/path and the new file name/path.

```php
<?php
// Renames green.html to blue.html
$this->ftp->rename('/public_html/foo/green.html', '/public_html/foo/blue.html');
```

## Move a file

Lets you move a file. Supply the source and destination paths:

```php
<?php
// Moves blog.html from "joe" to "fred"
$this->ftp->move('/public_html/joe/blog.html', '/public_html/fred/blog.html');
```
