# Upload class 

CodeDmx has an Upload Class, that permits upload files to a remote server.

> First of all you need to load class

```php
<?php
$this->load->library('upload');
```

## A simple example

```php
<?php
if ( ! empty($_FILES))
{
	$this->load->library('upload');

	$this->upload->input('file');
	$this->upload->destination($GLOBALS['COD']->doc . 'uploads' . DS);
	$this->upload->save();

	if ($this->upload->status())
	{
		echo 'is upload!';
	}
}
?>
<form method="post" action="" enctype="multipart/form-data">
	<input type="file" name="file" />
	<input type="submit" value="Upload now!" />
</form>
```

## allow_mime_type

This method will allow you to establish a new mime type

> Example:

```php
<?php
$this->upload->allow_mime_type("text/html");

# But you can also use the mime_helping(Show more in $mime_helping)
$this->upload->allow_mime_type("image"); // Set: image/jpeg, image/jpg, image/pjpeg, image/png and image/gif.
```

## allowed_mime_types

This method will allow you to set multiple mime types.

> Example:

```php
<?php
$this->upload->allowed_mime_types(array(
	"text/plain",
	"text/html"
));
```

## auto_filename

This method will allow you to generate a unique name for the file you are uploading.

## callback_input

This method will allow you to set a function to be executed to start the process. The method used must have a single parameter, which will be equivalent $this->upload->get_info()

> Example:

```php
<?php
$this->upload->callback_input(function($file){
	echo "start!";
});
```

## callback_output

This method will allow you to set a function to be executed at the end of the process. The method used must have a single parameter, which will be equivalent $this->upload->get_info()

> Example:

```php
<?php
$this->upload->callback_output(function($file){
	rename($file->destination, time());
});
```

## destination

This method allows you to set where the file will be saved trying to upload.

If the file path does not exist, you can set the parameter to true $create_if_not_exist when trying to create a new path.

> Examples:

```php
<?php
$this->upload->destination("./uploads");
$this->upload->destination("../uploads");
$this->upload->destination("/var/www/html/uploads");
$this->upload->destination("/var/www/html/uploads/tmp", true); // If path not exists, force create
```

## filename

This method will allow you to set the name of the file you are uploading. For the extension of the file, use the wildcard %s.

> Example:

```php
<?php
$this->upload->filename("my_new_file.%s");
```

## max_file_size

This method allows you to limit the size of file you are uploading.

> Examples:

```php
<?php
$this->upload->max_file_size("1m"); // Limit is 1MB(1048576 Bytes)
$this->upload->max_file_size("1.5Megabytes"); // Limit is 1.5MB(1572864 Bytes)
$this->upload->max_file_size("1.5MB"); // Limit is 1.5MB(1572864 Bytes)
$this->upload->max_file_size("12Gbytes"); // Limit is 12 GB(12884901888 Bytes)
$this->upload->max_file_size("1048576"); // Limit is 1MB(1048576 Bytes)
$this->upload->max_file_size("1331.2"); // Limit is 1.3KB(1331.2 Bytes)
```

## upload_function

This method allows you to use the function that you need to upload files

> Example:

```php
<?php
$this->upload->upload_function("copy"); // Default is move_uploaded_file
```

## size_format

Converts bytes to units of measurement.

> Example:

```php
<?php
$this->upload->size_format("1"); // return 1B
$this->upload->size_format("1024"); // return 1K
$this->upload->size_format("1048576"); // return 1M
$this->upload->size_format("1073741824"); // return 1G
$this->upload->size_format("1099511627776"); // return 1T
$this->upload->size_format("1331.2"); // return 1.3K
```

## size_in_bytes

Converts measurement units to bytes

> Example:

```php
<?php
$this->upload->size_in_bytes("1"); // return 1
$this->upload->size_in_bytes("1B"); // return 1
$this->upload->size_in_bytes("1K"); // return 1024
$this->upload->size_in_bytes("1M"); // return 1048576
$this->upload->size_in_bytes("1G"); // return 1073741824
$this->upload->size_in_bytes("1.56M"); // return 1635778.56
```

## get_info()

Returns all information about uploading the file.

> Example

```php
<?php
stdClass Object
(
    [status]            => false       // true if successful upload
    [mime]              => ""// File mime type
    [filename]          => "" // The new filename
    [original]          => "" // Filename before to save in destination directory
    [size]              => 0 // In bytes
    [size_formated]     => 0B // In B, K, M and G
    [destination]       => "" // Default is current dir ( ./ )
    [allowed_mime_types]=> Array () // All allowed mime types
    [log]               => Array () // All logs
    [error]             => 0 // File error
)
```

## status()

Returns the status of the upload. If the condition is false, then the file has not yet risen, if the state is true, the file upload was performed successfully.

## is_dirpath

Validates the directory path "is_dirpath($directory)"

## is_filename

Validates the filename "is_filename($filename)"

## allow_overwriting()

If the file you try to upload already exists, it can not be overwritten unless you enable overwriting using this method.

## check_mime_type

Validates the mime type of the file. If you have not enabled any mime type, the validation will return true. "check_mime_type($mime_type)"

## clear_allowed_mime_types()

Removes all previously enabled mime types.

## dir_exists

Checks if the directory exists. "dir_exists($directory)"

## file_exists

Checks if the file exists. "file_exists($file)"

## save()

This method loads the file, applies filters and save the file to the set destination.
