# Example form validation

This example can help you to understand Validation Class.

## Basic tags

```php
<?php
$this->load->library('validate');

$this->validate->validation_rules(array(
	'comment' => 'required|max_len,500',
));
$this->validate->filter_rules(array(
	'comment' => 'basic_tags',
));
// Valid Data
$_POST = array(
    'comment' => '<strong>this is freaking awesome</strong><script>alert(1);</script>'
);
$_POST = $this->validate->run($_POST);
print_r($_POST);
/*
Output:
Array
(
    [comment] => this is freaking awesomealert(1);
)
*/
```

## Contains

```php
<?php
$this->load->library('validate');

$rules = array(
    'account_type' => "required|contains,pro free basic premium",
    'priority'     => "required|contains,'low' 'medium' 'very high'",
);

echo "\nVALID DATA TEST:\n\n";

// Valid Data
$_POST_VALID = array(
    'account_type' => 'pro',
    'priority'     => 'very high'
);
$valid = $this->validate->validate($_POST_VALID, $rules);

if ($valid !== true) {
  echo $this->validate->get_readable_errors(true);
} else {
  echo "Validation passed! \n";
}

echo "\nINVALID DATA TEST:\n\n";
// Invalid
$_POST_INVALID = array(
    'account_type' => 'bad',
    'priority'     => 'unknown'
);
$invalid = $this->validate->validate(
    $_POST_INVALID, $rules
);

if($invalid !== true) {
    echo $this->validate->get_readable_errors(true);
    echo "\n\n";
    // Output: The Account Type field needs to contain one of these values: pro, free, basic, premiumThe Priority field needs to contain one of these values: low, medium, very high

} else {
    echo "Validation passed!\n\n";
}
```

## Credit card

```php
<?php
$this->load->library('validate');

$_POST = array(
	'cc' => '987230234-2498234-24234-23' // This is not a valid credit card number
);
$rules = array(
	'cc' => 'valid_cc'
);
print_r($this->validate->is_valid($_POST, $rules));
/*
Output:
Array
(
    [0] => The Cc field needs to contain a valid credit card number
)
*/
```


## Custom validator

```php
<?php
$this->load->library('validate');
// Add the custom validator
$this->validate->add_validator("is_object", function($field, $input, $param = NULL) {
    return is_object($input[$field]);
});
// Generic test data
$input_data = array(
    'not_object'   => "asdasd",
    'valid_object' => new stdClass()
);
$rules = array(
    'not_object'   => "is_object",
    'valid_object' => "is_object"
);
// METHOD 1 (Long):
$validated = $this->validate->validate(
	$input_data, $rules
);
if($validated === true) {
	echo "Validation passed!";
} else {
	echo $this->validate->get_readable_errors(true);
}
// METHOD 2 (Short):
$is_valid = $this->validate->is_valid($input_data, $rules);
if($is_valid === true) {
	echo "Validation passed!";
} else {
    print_r($is_valid);
}
```

## Escaping mysql strings

```php
<?php
$this->load->library('validate');

$_POST = array(
	'username' => "my username",
	'password' => "' OR ''='"
);
$this->validate->sanitize($_POST);

$filters = array(
	'username' => 'noise_words',
	'password' => 'trim|strtolower|addslashes'
);
print_r($this->validate->filter($_POST, $filters));

/*
Output:
Array
(
    [username] => my username
    [password] => &#39; OR &#39;&#39;=&#39;
)
*/

$filters = array(
	'username' => 'noise_words',
	'password' => 'trim|strtolower|addslashes'
);
print_r($this->validate->filter($_POST, $filters));
/*
Output:
Array
(
    [username] => username
    [password] => &#39; or &#39;&#39;=&#39;
)
*/
```

## Explicit fields

```php
<?php
$this->load->library('validate');

$data = array(
	'str' => null
);
$rules = array(
	'str' => 'required'
);

$this->validate->set_field_name("str", "Street");
$validated = $this->validate->is_valid($data, $rules);
if($validated === true) {
	echo "Valid Street Address\n";
} else {
	print_r($validated);
    /*
    Output:
    Array
    (
        [0] => The Street field is required
    )
    */
}
```

## Files

```php
<?php
$this->load->library('validate');

$_FILES = array(
	'attachments' => array(
		'name'     => array("test1.png"),
		'type'     => array("image/png"),
		'tmp_name' => array("/tmp/phpmFkEUe"),
		'error'    => array(0),
		'size'     => array(9855)
	)
);
$errors = array();
$length = count($_FILES['attachments']['name']);
for ($i = 0; $i < $length; $i++)
{
	$struct = array(
		'type'     => $_FILES['attachments']['type'][$i],
		'error'    => $_FILES['attachments']['error'][$i],
		'size'     => $_FILES['attachments']['size'][$i],
	);

	$validated = $this->validate->is_valid($struct, array(
	    'name'     => 'required',
	    'type'     => 'required',
	    'tmp_name' => 'required',
	    'size'     => 'required|numeric',
	));

	if ($validated !== true) {
		$errors[] = $validated;
	}
}
print_r($errors);
/*
Output:
Array
(
    [0] => Array
        (
            [0] => The Name field is required
            [1] => The Tmp Name field is required
        )

)
*/
```

## Full example

```php
<?php
$this->load->library('validate');
// Set the data
$_POST = array(
	'username' 	  => 'SeanNieuwoudt',
	'password' 	  => 'mypassword',
	'email'	      => 'sean@wixel.net',
	'gender'   	  => 'm',
	'credit_card' => '9872389-2424-234224-234', // Obviously an invalid credit card number,
	'bio'		  => 'This is good! I think I will switch to another language'
);
$_POST = $this->validate->sanitize($_POST); // You don't have to sanitize, but it's safest to do so.

// Let's define the rules and filters
$rules = array(
	'username'    => 'required|alpha_numeric|max_len,100|min_len,6',
	'password'    => 'required|max_len,100|min_len,6',
	'email'       => 'required|valid_email',
	'gender'      => 'required|exact_len,1',
	'credit_card' => 'required|valid_cc',
	'bio'		  => 'required'
);
$filters = array(
	'username' 	  => 'trim|sanitize_string',
	'password'	  => 'trim|base64_encode',
	'email'    	  => 'trim|sanitize_email',
	'gender'   	  => 'trim'
);
$_POST = $this->validate->filter($_POST, $filters);
// You can run filter() or validate() first
$validated = $this->validate->validate(
	$_POST, $rules
);
// Check if validation was successful
if($validated === TRUE)
{
	echo "Successful Validation\n\n";
	print_r($_POST); // You can now use POST data safely
	exit;
}
else
{
	print_r($_POST);
	print_r($validated); // Shows all the rules that failed along with the data
}
```

## GUID

```php
<?php
$this->load->library('validate');

$data = array(
  'guid' => "A98C5A1E-A742-4808-96FA-6F409E799937"
);
$is_valid = $this->validate->is_valid($data, array(
	'guid' => 'required|guidv4',
));
if($is_valid === true) {
	// continue
} else {
	print_r($is_valid);
    /*
    Output:
    Array
    (
        [0] => The Guid field is invalid
    )
    */
}
```

## Handling Errors

```php
<?php
$this->load->library('validate');

// Set the data
$_POST = array(
	'username' 	  => 'SeanNieuwoudt',
	'password' 	  => 'mypassword',
	'email'	      => 'sean@wixel.net',
	'gender'   	  => 'm',
	'credit_card' => '9872389-2424-234224-234', // Obviously an invalid credit card number,
	'bio'		  => 'This is good! I think I will switch to another language'
);
$_POST = $this->validate->sanitize($_POST); // You don't have to sanitize, but it's safest to do so.
// Let's define the rules and filters
$rules = array(
	'username'    => 'required|alpha_numeric|max_len,100|min_len,40',
	'password'    => 'required|max_len,100|min_len,6',
	'email'       => 'required|valid_email',
	'gender'      => 'required|exact_len,1',
	'credit_card' => 'required|valid_cc',
	'bio'		  => 'required'
);
$filters = array(
	'username' 	  => 'trim|sanitize_string',
	'password'	  => 'trim|base64_encode',
	'email'    	  => 'trim|sanitize_email',
	'gender'   	  => 'trim'
);
$_POST = $this->validate->filter($_POST, $filters);
// You can run filter() or validate() first
$validated = $this->validate->validate(
	$_POST, $rules
);
if($validated === TRUE)
{
	echo "Successful Validation\n\n";
	print_r($_POST); // You can now use POST data safely
	exit;
}
else
{
	// You should know what form fields to expect, so you can reference them here for custom messages
	echo "There were errors with the data you provided:\n";

	foreach($validated as $v) {
		switch($v['field']) {
			case 'credit_card':
				echo "- The credit card provided is not valid.\n";
				break;
			case 'username':
				echo "- The username provided is not valid.\n";
				break;				
		}
	}

	// Or you can simply use the built in helper to generate the error messages for you
	// Passing a boolean true to is returns the errors as html, otherwise it returns an array
	echo $this->validate->get_readable_errors(true);
}
```

## Match

```php
<?php
$this->load->library('validate');

$data = array(
    'username'         => "myusername",
    'password'         => "mypassword",
    'password_confirm' => "mypa33word",
);
$is_valid = $this->validate->is_valid($data, array(
    'username'         => 'required|alpha_numeric',
    'password'         => 'required|max_len,100|min_len,6',
    'password_confirm' => 'equalsfield,password',
));
if($is_valid === true) {
	// continue
} else {
	print_r($is_valid);
    /*
    Output:
    Array
    (
        [0] => The Password Confirm field does not equal password field
    )
    */
}
```

## Noise words

```php
<?php
$this->load->library('validate');
// What are noise words? http://support.dtsearch.com/webhelp/dtsearch/noise_words.htm
$_POST = array(
	'words' => "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English"
);
$filters = array(
	'words' => 'noise_words'
);
print_r($this->validate->filter($_POST, $filters));
/*
Output:
Array
(
    [words] => long established fact reader will distracted readable content page when looking layout. point using Lorem Ipsum more-or-less normal distribution letters, opposed using 'Content here, content here', making look readable English
)
*/
```

## Sanitize string

```php
<?php
$this->load->library('validate');

$_POST = array(
	'string' => '<script>alert(1); $("body").remove(); </script>'
);
$filters = array(
	'string' => 'sanitize_string'
);
print_r($this->validate->filter($_POST, $filters));
/*
Output:
Array
(
    [string] => alert(1); $(&#34;body&#34;).remove();
)
*/
```

## Sanitize whitelist

```php
<?php
$this->load->library('validate');

$_POST = array(
	'first_name' 	=> 'Joe',
	'last_name'	=> 'Black',
	'nickname'	=> 'blackjoe', // unexpected field
);
$rules = array(
	'first_name' 	=> 'required|valid_name',
	'last_name' 	=> 'required|valid_name'
);
/**
 * You can "whitelist" the submitted fileds: other fields will be ignored.
 * Pass an array of fields as 2nd argument in 'sanitize' method, e.g.:
 * $whitelist = array( 'first_name', 'last_name' );
 *
 * Tip: you can use the keys of rule/filter array as a whitelist
 */
$whitelist = array_keys($rules);
$_POST = $this->validate->sanitize( $_POST, $whitelist );
$validated = $this->validate->validate($_POST, $rules);
if ( $validated === TRUE )
{
	/**
	 * Now you are sure that the $_POST array contains only the fields
	 * included in whitelist.
	 *
	 * It's a good practice anyway, but it's very useful if you are
	 * using an ORM/active-records library to store data into database
	 * and you have to be sure that the fields match the table columns.
	 *
	 * E.g.: ... $db->table('products')->insert($_POST) ...
	 */
	print_r($_POST);
}
```

## Street address

```php
<?php
$this->load->library('validate');

$data = array(
	'street' => '6 Avondans Road'
);
$validated = $this->validate->is_valid($data, array(
	'street' => 'required|street_address'
));
if($validated === true) {
	echo "Valid Street Address\n";
} else {
	print_r($validated);
}
```

## URL exists

```php
<?php
$this->load->library('validate');

$_POST = array(
	'url' => 'http://sudygausdjhasgdjasjhdasd987lkasjhdkasdkjs.com/' // This url obviously does not exist
);
$rules = array(
	'url' => 'url_exists'
);

$is_valid = $tihs->validate->validate($_POST, $rules);
if($is_valid === true) {
	echo "The URL provided is valid";
} else {
	print_r($tihs->validate->get_readable_errors());
}
```

## UTF-8

```php
<?php
$this->load->library('validate');

$data = array(
	'one' => 'Freiheit, Mobilität und Unabhängigkeit lebt. ö, Ä, é, or ß',
	'two' => 'ß'
);
$validated = $this->validate->is_valid($data, array(
	'one' => 'required|min_len,10',
	'two' => 'required|min_len,1',
));
if($validated === true) {
	echo "Valid Text\n";
} else {
	print_r($validated);
}
```
