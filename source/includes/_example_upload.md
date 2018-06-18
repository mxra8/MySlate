# Example upload

This example can help you to understand Uploader Class.

```php
<?php
// Start
$this->load->library('upload');
// set input( name of input file: show html )
$this->upload->input( "my_file" );
// Directory output
$this->upload->destination( "upload" );
//Use "copy" function( Default: move_uploaded_file )
$this->upload->set_upload_function("copy");
//Set file size limit( Formats: 10M , 10MB, 10mb, 10485760, etc )
$this->upload->max_file_size("10M");
// Callback in before upload
$this->upload->callback_input(function( $data )
{
	echo "<h3>Start!</h3>";
});
// Callback in after upload
$this->upload->callback_output(function( $data )
{
	echo "<h3>End!</h3>";
	if( $data->status === true )
	{
		echo "<p>The ".$data->filename." file is upload</p>";
	}else{
		echo "<p>The ".$data->filename." file could not be uploaded to the server</p>";
	}
});
// Set destination directory( output file )
$this->upload->destination(dirname(__FILE__));
// Set multiples mime types
$this->upload->allowed_mime_types(array(
	'image/jpeg',
	'image/jpg',
	'image/pjpeg',
	'image/png',
));
// Set only one mime type
$this->upload->allow_mime_type('image/gif');
// Set filename output
$this->upload->filename("myfile.%s");
// Allow overwriting if exists file
$this->upload->allow_overwriting();
// Upload now!
$this->upload->save();
// Get all operation info
highlight_string(print_r($this->upload->get_info(),true));
?>

<html>
    <head>
        <title>Uploader Example</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    </head>
    <body>
        <form method="post" action="" enctype="multipart/form-data" accept-charset="utf-8">
        	<input type="file" name="my_file" />
        	<input type="submit" value="Upload now!" />
        </form>
    </body>
</html>
```
