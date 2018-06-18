# Validator Class

CodeDmx has a Validator class. This class can help you to validate inputs, form, or data that you sent.

> First of all you need to load class

```php
<?php
$this->load->library('validate');
```

## A simple example

Before to explain this example, let's to describe the ideal scenario:

* A form is displayed
* The user fill it and submit

Runs the validation of the form submited:

```php
<?php
$is_valid = $this->validate->is_valid($_POST, array(
  'username' => 'required|alpha_numeric',
  'password' => 'required|max_len,100|min_len,6'
));

if ($is_valid === TRUE)
{
  //continue
}
else
{
  print_r($is_valid);
}
```

## Available Methods

This are all available methods of Validator Class

```php
<?php
// The short way to validate
$this->validate->is_valid(array $data, array $rules);

// Get or set the validation rules
$this->validate->validation_rules(array $rules);

// Get or set the filtering rules
$this->validate->filter_rules(array $rules);

// Run the filter and validation routines
$this->validate->run(array $data);

// Strips and encodes unwanted characters
$this->validate->xss_clean(array $data);

// Sanitizes data and converts strings to UTF-8, this isn't required, but it's safest to do so.
$this->validate->sanitize(array $input);

// Validates input data according to the provided rules
$this->validate->validate(array $input, array $rules);

// Filters input data according to the provided filter
$this->validate->filter(array $input, array $filter);

// Returns human readable error text in an array or string
$this->validate->get_readable_errors($convert_to_string = false);

// Fetch an array of validation errors indexed by the field names
$this->validate->get_errors_array();

// Override field names with readable ones for errors
$this->validate->set_field_name($field, $readable_name);
```

## Long format example

Runs the validation of the form submited:

```php
<?php
$_POST = $this->validate->sanitize($_POST); // You don't have to do this, but it's safest to do.
$this->validate->validation_rules(array(
  'username' => 'required|alpha_numeric|max_len,20|min_len,6',
  'password' => 'required|max_len,100|min_len,6',
  'email' => 'required|valid_email'
));
$this->validate->filter_rules(array(
  'username' => 'trim|sanitize_string',
  'password' => 'trim',
  'email' => 'trim|sanitize_email'
));

$validated_data = $this->validate->run($_POST);

if ($validated_data === FALSE)
{
  echo $this->validate->get_readable_errors(TRUE);
}
else
{
  print_r($validated_data); // validation successful
}
```

## Creating your own validators and filters

Adding custom validators and filters is made easy by using callback functions.

```php
<?php
/*
  Create a csutom validation rule named "is_object".
  This callback receives 3 arguments:
  The field to validate, the values being validated, and any parameters used in the validation rule.
  It sould return a boolean value indicating whether the value is valid.
*/
$this->validate->add_validator('is_object', function($field, $input, $param = NULL) {
  return is_object($input[$field]);
});

/*
  Create a custom filter named "upper".
  The callback function receives two arguments:
  The value to filter, and any parameters used in the filter rule. It should returned the filtered value.
*/
$this->validate->add_filter('upper', function($value, $param = NULL) {
  return strtoupper($value);
});
```

## Available Filters

Filters can be any PHP function that returns a string. You don't need to create your own if a PHP function exists that does what you want the filter to do.

Rule | Description
--------- | ------- | -----------
sanitize_string | Remove script tags and encode HTML entities, similar to $this->validate->xss_clean();
urlencode | Encode url entities
htmlencode |  Encode HTML entities
sanitize_email |  Remove illegal characters from email addresses
sanitize_numbers |  Remove any non-numeric characters
sanitize_floats | Remove any non-float characters
trim |  Remove spaces from the beginning and end of strings
base64_encode | Base64 encode the input
base64_decode | Base64 decode the input
sha1 |  Encrypt the input with the secure sha1 algorithm
md5 | MD5 encode the input
noise_words | Remove noise words from string
json_encode | Create a json representation of the input
json_decode | Decode a json string
rmpunctuation | Remove all known punctuation characters from a string
basic_tags | Remove all layout orientated HTML tags from text. Leaving only basic tags
whole_number | Check that the provided numeric value is represented as a whole number

## Validate file fields

When you use a form that want to upload files, you can validate the file too

```php
<?php
$is_valid = $this->validate->is_valid(array_merge($_POST, $_FILES), array(
  'title' => 'required|alpha_numeric',
  'image' => 'required_file|extension,png;jpg'
));

if ($is_valid === TRUE)
{
  //continue
}
else
{
  print_r($is_valid);
}
```

## URL Exists (Example)

```php
<?php
$_POST = array(
  'url' => 'http://asidnqowineoqiwneoinspoqwehpi1.com' // This url doesn't exist
);

$rules = array(
  'url' => 'url_exists'
);

$is_valid = $this->validate->validate($_POST, $rules);

if ($is_valid === TRUE)
{
  echo 'The URL provided is valid';
}
else
{
  print_r($this->validate->get_readable_errors());
}
```

## Validate street address (Example)

```php
<?php
$data = array(
  'street' => 'Kuwait 6958'
);

$validate = $this->validate->is_valid($data, array(
  'street' => 'required|street_address'
));

if ($validate === TRUE)
{
  echo 'Valid Street Address';
}
else
{
  print_r($validate);
}
```

## Sanitize string (Example)

```php
<?php
$_POST = array(
  'string' => '<script>alert(1); $("body").remove(); </script>'
);

$filter = array(
  'string' => 'sanitize_string'
);

print_r($this->validate->filter($_POST, $filter));
```

## Match strings (Example)

```php
<?php
$data = array(
  'username' => 'myusername',
  'password' => 'mypassword',
  'password_confirm' => 'mypa33word'
);

$is_valid = $this->validate->is_valid($data, array(
  'username' => 'required|alpha_numeric',
  'password' => 'required|max_len,100|min_len,6',
  'password_confirm' => 'equalsfield,password'
));

if ($is_valid === TRUE)
{
  // continue
}
else
{
  print_r($is_valid);
}
```

## Escaping Mysql Strings (Example)

```php
<?php
$_POST = array(
  'username' => 'my username',
  'password' => "' OR ''='"
);

$this->validate->sanitize($_POST);

$filter = array(
  'username' => 'noise_words',
  'password' => 'trim|strtolower|addslashes'
);

print_r($this->validate->filter($_POST, $filter));
```

## Custom validator (Example)

```php
<?php
// Add the custom validator
$this->validate->add_validator('is_object', function($field, $input, $param = NULL) {
  return is_object($input[$field]);
});

// Generic data
$input_data = array(
  'not_object' => 'asdqwezxc',
  'valid_object' => new stdClass()
);

$rules = array(
  'not_object' => 'is_object',
  'valid_object' => 'is_object'
);

/*
Long Method
*/

$validated = $this->validate->validate(
  $input_data, $rules
);

if ($validated === TRUE)
{
  echo 'Validation passed!';
}
else
{
  echo $this->validate->get_readable_errors(TRUE);
}

/*
Short Method
*/

$is_valid = $this->validate->is_valid($input_data, $rules);

if ($is_valid === TRUE)
{
  echo 'Validation passed!';
}
else
{
  print_r($is_valid);
}
```
