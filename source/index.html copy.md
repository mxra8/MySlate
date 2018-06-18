---
title: CodeDmx Documentation

toc_footers:
  - <a href='https://github.com/mxra8/slate' target="_blank">Made with Slate</a>

search: true
---

# Welcome

CodeDmx is a Our PHP framework project that want to enable you to develop projects much faster than you could if you were writing code from scratch, by providing a rich set of libraries for commonly needed task.

CodeDmx PHP Framwork can help you if:

* You want a framework with a small footprint.
* You want a framework that requires nearly zero configuration.
* You want a framework that does not require you to use the command line.
* You want a framework that does not require you to adhere to restrictive coding rules.
* You are not interested in large-scale monolithic libraries like PEAR.
* You do not want to be forced to learn a templating language (although a template parser is optionally available if you desire one).
* You need clear, thorough documentation.

## Server Requirements

1. Web server: Ngnix or Apache (with mod_rewrite)
2. PHP version 7.0 or newer is required.
It should works on 5.6, but we work with the last version of PHP. (**you need to remove in index file php version condition**)
3. MySQLi PHP Extension
4. OpenSSL PHP Extension
5. Mbstring PHP Extension

## License

**The MIT License (MIT)**

Copyright (c) 2014 - 2017, CodeDMx

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

## Installation

> To use database

```php
<?php
// Modify this global variables on the file db.settings.php located in /application/Config/
$GLOBALS['COD']->db = array(
  'default' => array(
    'host' => 'localhost', // Required
    'username' => '', // Required
    'password' => '', // Required
    'db'=> '', // Required
    'port' => 3306,
    'charset' => 'utf8'
  )
);
$GLOBALS['COD']->host = '';
$GLOBALS['COD']->db = '';
$GLOBALS['COD']->usr = '';
$GLOBALS['COD']->pwd = '';
$GLOBALS['COD']->port = 3306;
$GLOBALS['COD']->charset = 'utf8';

// Then, to use on your files
$this->load->library('db'); #Load db library

$this->db->where('username', 'test');
$this->db->where('password', md5('password'));
$user = $this->db->get_once('users');

// Or you can auto load db library in CFG.php file
$GLOBALS['COD']->autoload = array('db');
```

CodeDmx is installed really quickly:

1. Unzip the package.
2. Upload the CodeDmx folders and files to your server.
3. If you intend to use a database, open the *Model/CFG.php* file with a text editor and set your database settings.

One additional measure to take in production environments is to disable PHP error reporting and any other development-only functionality.  This can be disable in index.php

**That's it!**

## Example

When enter on a site, CodeDmx enter to Loader Class, and if exists in View folder, include the file that you called by the URL, for example:

> http://mysite.com/contact/

```php
<?php
include $GLOBALS['COD']->doc . 'View' . DS . $page . '.php';
```

# CodeDmx Loader Functions

## Detect HTTPS

This method checks for $_SERVER['HTTPS']

```php
<?php
$is_https = $this->is_https();
var_dump($is_https);
```

## Detect AJAX

This method check for $_SERVER['HTTP_X_REQUESTED_WITH']

```php
<?php
$is_ajax = $this->is_ajax();
var_dump($is_ajax);
```

## Get Client IP

```php
<?php
$ip = $this->get_ip();
echo $ip;

// Or include a specific header
$ip = $this->get_ip('HTTP_CLIENT_IP');
echo $ip;
```

## Detect mobile

```php
<?php
$is_mobile = $this->is_mobile();
var_dump($is_mobile);
```

## Get browser

```php
<?php
$browser = $this->know_browser();
echo $browser;
```

## Cut string

```php
<?php
$string = 'The Fresh Prince of Bel-Air';
$shorten = $this->cut_string($string, 10);
echo $shorten;

// $add_ellipsis '...' at the end of the string
$shorten = $this->cut_string($string, 10, $add_ellipsis = FALSE);
echo $shorten;

// $word_safe to cut or not in the middle
$shorten = $this->cut_string($string, 10, $add_ellipsis = FALSE, $word_safe = TRUE);
echo $shorten;
```

## Create id

```php
<?php
echo $this->create_id();
// Gives: 1412512123
```

## Validate email address

```php
<?php
$isValid = $this->validate_email("user@gmail.com");
var_dump($isValid);
// outputs: true (bool)
```

## Generate random string

```php
<?php
echo $this->random_string();
// outputs: ioj12038

echo $this->random_string(16);
// outputs: 12j3091j20930sd1
```

## Get current URL

```php
<?php
echo $this->get_current_URL();
// outputs: http://mydomain.com
```

## Crypt string

> You need to determinate the encryption keys and encryption method on CFG.php file

```php
<?php
$GLOBALS['COD']->emethod = ''; // AES-256-CBC | aes-128-cbc, AES-128-CFB
$GLOBALS['COD']->ekey = ''; //qw1209132j
$GLOBALS['COD']->eiv = ''; // *123+1*23*
```

> The you can crypt and decrypt strings

```php
<?php
$string = 'password';
$encrypted = $this->crypt_this('encrypt', $string);
echo $encrypted;

// Then you can decrypt
echo $this->crypt_this('decrypt', $encrypted);
```

## Create Google analytics code

> You can add your google analytics account id on CFG.php file

```php
<?php
echo $this->create_ga();
```

## Compres page

Captures output via ob_get_contents(), tries to enable gzip, removes whitespace from captured output and echos back.

```php
<?php
$this->compress_page();
?>
<html>
<body>
  <p></p>
  <div>
    <div></div>
  </div>
</body>
</html>

<?php
// outputs: <html><body><p></p><div><div></div></div></body></html>
```


# Functions class (COD_Functions)

## All this functions need to load with two options

```php
<?php
$this->load->library('func');
# Then
$this->func->your_function();

// OR
$GLOBALS['COD']->autoload = array('func');
```

## Generating QR Code

```php
<?php
$qr_code = $this->func->get_qr_code('test');
echo $qr_code;
// Gives: <img src="http://chart.apis.google.com/chart?chs=150x150&cht=qr&chl=test" />
```

## Generating QR code and adding HTML attributes:

```php
<?php
$qr_code = $this->func->get_qr_code('test',
  $width = 350,
  $height = 350,
  $attributes = array(
    'class' => 'QRCode'
  )
);
echo $qr_code;
// Gives: <img src="http://chart.apis.google.com/chart?chs=350x350&cht=qr&chl=test" class="QRCode" />
```

## Create simple link tag

```php
<?php
$link = $this->func->create_link_tag('google.com');
echo $link;
// Gives: <a href="http://google.com">google.com</a>
```

## Create simple link tag with title

```php
<?php
$link = $this->func->create_link_tag('google.com', 'Visit Google');
echo $link;
// Gives: <a href="http://google.com" title="Visit Google" >google.com</a>
```

## Create simple link tag with title and HTML attributes

```php
<?php
$link = $this->func->create_link_tag('google.com', 'Visit Google', array(
  'class' => 'my_class'
));
echo $link;
// Gives: <a href="http://google.com" title="Visit Google" class='my_class' >google.com</a>
```

## Array to String

```php
<?php
$array =array(
    'foo' => 'bar',
    'baz' => 'qux'
);
$string = $this->func->array_to_string($array);
echo $string;
// Gives: foo="bar" baz="qux"
```

## Generate random string

```php
<?php
$random = $this->func->random_string(10);
echo $random;
// Gives: 10 random character string
```

## Get Locations

```php
<?php
$location = $this->func->get_location();
echo $location;
```

## cURL

> Simple GET example

```php
<?php
$data = $this->func->curl('https://api.ipify.org');
var_dump($data);
```

> POST Example

```php
<?php
$cURL = $this->func->curl('http://jsonplaceholder.typicode.com/posts', $method = 'POST', $data = array(
    'title' => 'foo',
    'body' => 'bar',
    'userId' => 1
));
var_dump($cURL);
```

> Custom Headers

```php
<?php
$cURL = $this->func->curl('http://jsonplaceholder.typicode.com/posts', $method = 'POST', $data = FALSE, $header = array(
  'Accept' => 'application/json'
), $return_info = TRUE);
var_dump($cURL);
```

## Get Alexa Rank

```php
<?php
$rank = $this->func->get_alexa_rank('github.com');
echo $rank;
```

## Get Google Page Rank

```php
<?php
$rank = $this->func->get_google_rank('github.com');
echo $rank;
```

## Expand Short URL

```php
<?php
$short = 'https://goo.gl/8SbJTG';
$expand = $this->func->get_short_url($short);
echo $expand;
// Gives https://github.com/mxra8
```

## Short URL

```php
<?php
$url = $this->func->short_url('https://github.com/mxra8');
echo $url;
```

## Embed an url

```php
<?php
$string = 'Need more Pidgey for Eminem "Rap God" in Pokémon GO https://www.youtube.com/watch?v=c7_CcBgZ2e4';
echo $this->func->embed($string);
// outputs:
// Need more Pidgey for Eminem "Rap God" in Pokémon GO<iframe width="560" height="315" src="https://www.youtube.com/embed/c7_CcBgZ2e4?feature=oembed" frameborder="0" allowfullscreen></iframe>
// supported providers are: youtube.com, blip.tv, vimeo.com, dailymotion.com, flickr.com, smugmug.com, hulu.com, revision3.com, wordpress.tv, funnyordie.com, soundcloud.com, slideshare.net and instagram.com
```

## Number To Word conversion

```php
<?php
$number = "864210";
$number_in_words = $this->func->number_to_word($number);
echo $number_in_words;
// outputs: eight hundred and sixty-four thousand, two hundred and ten
```

# Validator Class (COD_Validator)

CodeDmx has a Validator class. This class can help you to validate inputs, form, or data that you sent.

> First of all you need to load class

```php
<?php
$this->load->library('validate');

//Then
$this->validate->sanitize($_POST);
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

# Query Builder Class (COD_QueryBuilder)

CodeDmx has a Mysqli Query Builder. This class can help you minified your code to connect with the database.

> First of all you need to load class

```php
<?php
$this->load->library('db');

// Then
$this->db->get('users');
```

## Select Query

The following functions allow you to build SQL SELECT statements:

```php
<?php
$users = $this->db->get('users');
print_r($users);
$users = $this->db->get('users', 10);
print_r($users);
```

> You can select with custom columns set.

```php
<?php
$cols = array('id', 'name', 'email');
$users = $this->db->get('users', null, $cols);
if ($this->db->count > 0)
{
  foreach ($users as $user)
  {
    print_r($user);
  }
}
```

> You can select just one row

```php
<?php
$this->db->where('id', 1);
$user = $this->db->get_one('users');
echo $user['id'];

$stats = $this->db->get_one('users', 'sum(id), count(*) as cnt');
echo "total " . $stats['cnt'] . "users found";
```

> You can select one column value of function result

```php
<?php
$count = $this->db->get_value('users', 'count(*)');
echo "{$count} users found";
```

> You can select one column value or function result from multiple rows:

```php
<?php
// Select login from users
$logins = $this->db->get_value('users', 'login', null);
// Select login from users limit 5
$logins = $this->db->get_value('users', 'login', 5);

foreach ($logins as $login)
{
  echo $login;
}
```

## Insert Query

> Just a simple example:

```php
<?php
$data = array(
  "login" => "admin",
  "firstName" => "John",
  "lastName" => "Dow"
);
$id = $this->db->insert('users', $data);
if ($id)
{
  echo "user was created. Id=" . $id;
}
```

> Insert with functions:

```php
<?php
$data = array(
  'login' => 'admin',
  'active' => true,
  'firstName' => 'John',
  'lastName' => 'Dow',
  'password' => $this->db->func('SHA1(?)', array('secretpassword+salt')),
  // password = SHA1('secretpassword+salt')
  'createdAt' => $this->db->now(),
  // createdAt = NOW()
  'expires' => $this->db->now('+1Y')
  // expires = NOW() + interval 1 year
  // Supported intervals [s]econd, [m]inute, [h]our, [d]ay, [M]onth, [Y]ear
);

$id = $this->db->insert('users', $data);
if ($id)
{
  echo "user was created. Id=" . $id;
}
else
{
  echo "insert failed " . $this->db->get_last_error();
}
```

> Insert with on duplicate key update

```php
<?php
$data = array(
  'login' => 'admin',
  'firstName' => 'John',
  'lastName' => 'Dow',
  'createdAt' => $this->db->now(),
  'updatedAt' => $this->db->now()
);

$update_columns = array('updatedAt');
$last_insert_id = 'id';
$this->db->on_duplicate($update_columns, $last_insert_id);
$id = $this->db->insert('users', $data);
```

> Insert multiple data at once

```php
<?php
$data = array(
  array('login' => 'admin',
    'firstName' => 'John',
    'lastName' => 'Doe'
  ),
  array('login' => 'other',
    'firstName' => 'Another',
    'lastName' => 'User',
    'password' => 'very_cool_hash'
  )
);

$ids = $this->db->insert_multi('users', $data);

if ( ! $ids)
{
  echo 'insert failed: ' . $this->db->get_last_error();
}
else
{
  echo 'new users inserted with following id\'s: ' . implode(', ', $ids);
}
```

> If all datasets only have the same keys, it can be simplified

```php
<?php
$data = array(
  array('admin', 'John', 'Doe'),
  array('other', 'Another', 'User'),
);
$keys = array('login', 'firstName', 'lastName');

$ids = $this->db->insert_multi('users', $data, $keys);
if ( ! $ids)
{
  echo 'insert failed: ' . $this->db->get_last_error();
}
else
{
  echo 'new users inserted with following id\'s: ' . implode(', ', $ids);
}
```

## Update Query

```php
<?php
$data = array(
  'firstName' => 'Bobby',
  'lastName' => 'Tables',
  // editCount = editCount + 2;
  'editCount' => $this->db->inc(2),
  // active = !active;
  'active' => $this->db->not()
);

$this->db->where('id', 1);
if ($this->db->update('users', $data))
{
  echo $this->db->count . ' records were updated';
}
else
{
  echo 'update failed: ' . $this->db->get_last_error();
}
```

> Update also support limit parameter

```php
<?php
$this->db->update('users', $data, 10);
// Gives: UPDATE users SET ... LIMIT 10
```

## Delete Query

```php
<?php
$this->db->where('id', 1);
if ($this->db->delete('users'))
{
  echo 'successfully deleted';
}
```

## Where / Having Methods

All conditions supported by where() are supported by having() as well.

```php
<?php
$this->db->where('id', 1);
$this->db->where('login', 'admin');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id = 1 AND login = 'admin'

$this->db->where('id', 1);
$this->db->having('login', 'admin');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id = 1 HAVING login = 'admin'
```

> Compare with colum to colum

```php
<?php
// WRONG
$this->db->where('lastLogin', 'createdAt');

// CORRECT
$this->db->where('lastLogin = createdAt');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE lastLogin = createdAt;

$this->db->where('id', 50, ">=");
// or $this->db->where('id', array('>=' => 50));
$results = $this->db->get('users');
print_r($results);
//Gives: SELECT * FROM users WHERE id >= 50
```

> LIKE

```php
<?php
$this->db->where('column_name', '%string%', 'LIKE');
$rows = $this->db->get('table_name');
// Gives: SELECT * FROM table_name WHERE column_name LIKE '%string%'

// Or you can use
$this->db->where('IP', '%' . $VAR . '%', 'LIKE');
```

> BETWEEN / NOT BETWEEN

```php
<?php
$this->db->where('id', array(4, 20), 'BETWEEN');
// or $this->db->where('id', array('BETWEEN' => array(4, 20)));

$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id BETWEEN 4 AND 20
```

> IN / NOT IN

```php
<?php
$this->db->where('id', array(1, 5, 27, -1, 'd'), 'IN');
// or $this->db->where('id', array('IN' => array(1, 5, 27, -1, 'd')));

$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id IN (1, 5, 27, -1, 'd');
```

> OR CASE

```php
<?php
$this->db->where('firstName', 'John');
$this->db->or_where('firstName', 'Peter');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE firstName='John' OR firstName='peter'
```

> NULL COMPARISON

```php
<?php
$this->db->where('lastName', NULL, 'IS NOT');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE lastName IS NOT NULL
```

> Also you can use raw where conditions:

```php
<?php
$this->db->where("id != companyId");
$this->db->where('DATE(createdAt) = DATE(lastLogin)');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id != companyId AND DATE(createdAt) = DATE(lastLogin)
```

> Or raw condition with variables:

```php
<?php
$this->db->where("(id = ? OR id = ?)", array(6, 2));
$this->db->where('login', 'mike');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE (id = 6 OR id = 2) AND login='mike'
```

> Find the total number of rows matched. Simple pagination example:

```php
<?php
$offset = 10;
$count = 15;
$users = $this->db->with_total_count()->get('users', array($offset, $count));
echo "Showing {$count} from {$this->db->total_count}";
```

## Ordering method

```php
<?php
$this->db->order_by('id', 'ASC');
$this->db->order_by('login', 'DESC');
$this->db->order_by('RAND ()');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users ORDER BY id ASC, login DESC, RAND();
```

> Order by values

```php
<?php
$this->db->order_by('userGoup', 'ASC', array('superuser', 'admin', 'users'));
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users ORDER BY FIELD (userGroup, 'superuser', 'admin', 'users') ASC;
```

> If you are using set_prefix() functionality and need to use tables names in order_by() method make sure that table names are escaped with ''.

```php
<?php
// WRONG
$this->db->set_prefix("t_");
$this->db->order_by("users.id", 'ASC');
$results = $this->db->get('users');
// WRONG: SELECT * FROM t_users ORDER BY users.id ASC;

$this->db->set_prefix("t_");
$this->db->order_by("users.id", 'ASC');
$results = $this->db->get('users');
// CORRECT: SELECT * FROM t_users ORDER BY t_users.id ASC;
```

## Groping Method

```php
<?php
$this->db->group_by('name');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users GROUP BY name;
```

## JOIN Method

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->where('u.id', 6);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
```

> JOIN with 3 tables

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join('articles a', 'a.id=u.id', 'LEFT');
$this->db->where('u.id', 6);
$results = $this->db->get('posts p', null, 'u.firstName, p.title, a.lolname');
print_r($results);
```

> JOIN with AND condition

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join_where('users u', 'u.id', 5);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
// Gives: SELECT u.firstName, p.title FROM products p LEFT JOIN users u ON (p.id=u.id AND u.id=5)
```

> JOIN with OR condition

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join_or_where('users u', 'u.id', 5);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
// Gives: SELECT u.firstName, p.title FROM products p LEFT JOIN users u ON (p.id=u.id OR u.id=5)
```

## Has method

A convenient function that returns TRUE if exists at least an element that satisfy the where condition specified calling the "where" method before this one.

```php
<?php
$this->db->where('user', $user);
$this->db->where('password', md5($password));
if ($this->db->has('users'))
{
  echo 'You are logged';
}
else
{
  echo 'Wrong user/password';
}
```

## Helper methods

> Reconnect in case mysql connection died:

```php
<?php
if ( ! $this->db->ping())
{
  $this->db->connect();
}
```

> Get last executed SQL query

```php
<?php
$this->db->get('users');
echo 'Last executed query was '. $this->db->get_last_query();
```

> Check if table exists

```php
<?php
if ($this->db->table_exists('users'))
{
  echo 'lml';
}
```

> Real escape string

```php
<?php
$escaped = $this->db->escape("' and 1=1");
```

> Error helpers

```php
<?php
$this->db->where('login', 'admin')->update(users, ['firstName' => 'Jack']);
if ($this->db->get_last_errno() === 0)
{
  echo 'Update succesfull';
}
else
{
  echo 'Update failed. Erro: '. $this->db->get_last_error();
}
```

## Properties sharing

Also is possible to copy properties

```php
<?php
$this->db->where('agentId', 10);
$this->db->where('active', TRUE);

$customers = $this->db->copy();
$res = $customers->get('customers', array(10, 10));
// Gives: SELECT * FROM customers WHERE agentId = 10 AND active = 1 LIMIT 10,10

$cnt = $this->db->get_value('customers', 'count(id)');
echo 'total records found: '. $cnt;
//Gives: SELECT count(id) FROM users WHERE agentId = 10 AND active = 1
```

## Pagination

Use paginate() instead of get() to fetch paginated result

```php
<?php
$page = 1;
$this->db->page_limit = 2;
$products = $this->db->array_builder()->paginate('products', $page);
echo 'showing $page out of '. $this->db->total_pages;
```

## Query exectution time benchmarking

To track query execution time set_trace() function should be called.

```php
<?php
$this->db->set_trace(TRUE);
$this->db->get('users');
$this->db->get('test');
print_r($this->db->trace);

/*
Array
(
    [0] => Array
        (
            [0] => SELECT  * FROM users
            [1] => 0.00048494338989258
        )

    [1] => Array
        (
            [0] => SELECT  * FROM test
            [1] => 0.0076069831848145
        )
)
*/
```

## Subqueries

> Subquery without an alias to use in insert/update/where Eg. (SELECT * FROM users)

```php
<?php
$subq = $this->db->sub_query();
$subq->get('users');
```

> Subquery in select

```php
<?php
$ids = $this->db->sub_query();
$ids->where('qty', 2, '>');
$ids->get('products', null, 'userId');

$this->db->where('id', $ids, 'in');
$res = $this->db->get('users');
// Gives SELECT * FROM users WHERE id IN (SELECT userId FROM products WHERE qty > 2)
```

> Subquery in join

```php
<?php
$users = $this->db->sub_query('u');
$users->where('active', 1);
$users->get('users');

$this->db->join($users, 'p.userId=u.id', 'LEFT');
$products = $this->db->get('products p', null, 'u.login, p.productName');
print_r($products);
```

> Subquery in insert

```php
<?php
$user = $this->db->sub_query();
$user->where('where', 6);
$user->get_one('users', 'name');

$data = array(
  'productName' => 'test product',
  'userId' => $user,
  'lastUpdated' => $this->db->now()
);

$id = $this->db->insert('products', $data);
// Gives: INSERT INTO products (productName, userId, lastUpdated) VALUES ("test product", (SELECT name FROM users WHERE id = 6), NOW());
```

## EXISTS / NOT EXISTS condition

```php
<?php
$sub = $this->db->sub_query();
$sub->where('company', 'testCompany');
$sub->get('users', null, 'userId');

$this->db->where(null, $sub, 'exists');
$products = $this->db->get('products');
print_r($products);
// Gives: SELECT * FROM products WHERE EXISTS (SELECT userId FROM users WHERE company='testCompany')
```

## Queries

This also can run your SQL queries directly

```php
<?php
$users = $this->db->raw_query('SELECT * FROM users WHERE id >= ?', array(10));
foreach ($users as $user)
{
  print_r($user);
}
```

> Get one row of results

```php
<?php
$user = $this->db->raw_query_one('SELECT * FROM users WHERE id=?', array(10));
echo $user['login'];

// Object return type
$user = $this->db->object_builder()->raw_query_one('SELECT * FROM users WHERE id=?', array(10));
echo $user->login;
```

> Get one column value as a string

```php
<?php
$password = $this->db->raw_query_value('SELECT password FROM users WHERE id=? limit 1', array(10));
// For a raw_query_value() return a string instead of an array, 'limit 1' should be added to the end of the query
echo 'Password is {$password}';
```

> Get one colum value from multiple rows

```php
<?php
$login = $this->db->raw_query_value('SELECT login FROM users LIMIT 10');
foreach ($login as $var)
{
  echo $var;
}
```

> An advanced examples

```php
<?php
$params = array(1, 'admin');
$users = $this->db->raw_query('SELECT id, firstName, lastName FROM users WHERE id = ? AND login = ?', $params);
print_r($users);

$params = array(10, 1, 10, 11, 2, 10);
$q = '(
  SELECT a FROM t1
    WHERE a = ? AND B = ?
    ORDER BY a LIMIT ?
) UNION (
  SELECT a FROM t2
    WHERE a = ? AND B = ?
    ORDER BY a LIMIT ?
);';
$result = $this->db->raw_query($q, $params);
print_r($result);
```

# Encryptation Class (COD_Bcrypt)

CodeDmx has an encryptation class. Some people believe MD5 would be a safe way to encode passwords. Is a lie! MD5 is a good method to obscure non-sensitive data, because it's quite fast.

<aside class="warning">The default schema is $2y$, which makes use of the new, corrected hash implementation. This can be modify con CFG.php file</aside>

## First of all: A simple example

> To hash the password, you only need to call one method

```php
<?php
$password = 'my_password';
$hashed = $this->crypt->hash_password($password);
echo $hashed;
// Something like: $2y$12$lyAb7hOusPTvEGJBVmia4OGV8nU0pFhBOvG4NZRT3P/1lA3SAMoFK
```

> To check if password against the $hashed, and this returns TRUE / FALSE

```php
<?php
if ($this->crypt->check_password($password, $hashed))
{
  // continue
}
```

## Work factor

To increase your hash security this method accepts the optional parameter $work_factor. This defines the number of rounds. With each round the creation time doubles, so the system is exponentially.

```php
<?php
$work_factor = 16;
$password = 'my_password';
echo $this->crypt->hash_password($password, $work_factor);
```

## Change the schema

To alter the default scheme, change the GLOBAL variable on your config file (/Model/CFG.php)$GLOBALS['COD']->identifier to the next strings

2a - Hash wich is potentially generated with the buggy algorithm

2x - "compatibility" option the buggy Bcrypt implementation

2y - Hash generated with the new, corrected algorithm implementation (crypt_blowfish 1.1 and newer)

## Structure

$2a$12$Some22CharacterSaltXXO6NC3ydPIrirIzk1NdnTz0L/aCaHnlBa

$2a$ tells PHP to use which Blowfish scheme (Bcrypt is based on Blowfish)

12$ is the number f iterations the hashing mechanism uses

Some22CharacterSaltXX0 is a random salt (by OpenSSL)

> Diagram:

```php
<?php
$2a$12$Some22CharacterSaltXXO6NC3ydPIrirIzk1NdnTz0L/aCaHnlBa
\___________________________/\_____________________________/
  \                            \
   \                            \ Actual Hash (31 chars)
    \
     \  $2a$   12$   Some22CharacterSaltXXO
        \__/    \    \____________________/
          \      \              \
           \      \              \ Salt (22 chars)
            \      \
             \      \ Number of Rounds (work factor)
              \
               \ Hash Header
```

# ImageClass Class (COD_ImageClass)

We updated this class on August 30, 2017.

CodeDmx has a class to manipulate images in PHP as simple as possible.
With this class, we can do:

1. Rezie images (free resize, resize to width, resize to height, resize to fit)
2. Crop images
3. Flip / rotate / adjust orientation
4. Adjust brightness and contrast
5. Desaturate, colorize, pixelate, blur, etc
6. Overlay one image onto another (Watermarking)
7. Add text using a font of your choice
8. Convert between GIF, JPG, and PNG formats

## First of all: A simple example

In the first line we try to load the image.
In the second line we shrink it to fit within a 300x250 box, and save it to another path.

```php
<?php
$this->load->library('image');
$this->image->from_file('image.jpg')
			->best_fit(300, 250)
			->to_file('/path/to/new/image.png', 'image/png');
```

## Saving

It's necessary to save the image after you manipulate it.

```php
<?php
// Or you can specify a new filename
$this->image->to_file('new-image.jpg', 'image/jpeg');

// Also, you can specify quality as a second parameter in percents within range 0 - 100
$this->image->to_file('new-image.jpg', 'image/jpeg', 90);
```

## Converting between Formats

When you try to save it, you can determined by the file extension.

```php
<?php
$this->image->from_file('/path/to/image.jpg')
			->to_file('/path/to/new/image.png', 'image/png');
```

## Method Chaining

This class supports method chaining, so you can make multiple changes and save the resulting image with just one line of code:

```php
<?php
$this->image->from_file('/path/to/image.jpg')
			->flip('x')
			->rotate(90)
			->best_fit(300, 250)
			->desaturate()
			->invert()
			->to_file('/path/to/new-image.jpg', 'image/jpeg');
// You can chain all of the methods below as well methods above.
```

## Loaders

### `from_data_uri($uri)`

- `$uri`* (string) - A data URI.

### `from_file($file)`

Loads an image from a file.

- `$file`* (string) - The image file to load.

### `from_new($width, $height, $color)`

Creates a new image.

- `$width`* (int) - The width of the image.
- `$height`* (int) - The height of the image.
- `$color` (string|array) - Optional fill color for the new image (default 'transparent').

### `from_string($string)`

Creates a new image from a string.

- `$string`* (string) - The raw image data as a string. Example:
  ```
  $string = file_get_contents('image.jpg');
  ```

## Savers

### `to_data_uri($mime_type, $quality)`

Generates a data URI.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

Returns a string containing a data URI.

### `to_download($filename, $mime_type, $quality)`

Forces the image to be downloaded to the clients machine. Must be called before any output is sent to the screen.

- `$filename`* (string) - The filename (without path) to send to the client (e.g. 'image.jpeg').
- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_file($file, $mime_type, $quality)`

Writes the image to a file.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_screen($mime_type, $quality)`

Outputs the image to the screen. Must be called before any output is sent to the screen.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_string($mime_type, $quality)`

Generates an image string.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

## Utilities

### `get_aspect_ratio()`

Gets the image's current aspect ratio.

Returns the aspect ratio as a float.

### `get_exif()`

Gets the image's exif data.

Returns an array of exif data or null if no data is available.

### `get_height()`

Gets the image's current height.

Returns the height as an integer.

### `get_mime_type()`

Gets the mime type of the loaded image.

Returns a mime type string.

### `get_orientation()`

Gets the image's current orientation.

Returns a string: 'landscape', 'portrait', or 'square'

### `get_width()`

Gets the image's current width.

Returns the width as an integer.

## Manipulation

### `auto_orient()`

Rotates an image so the orientation will be correct based on its exif data. It is safe to call this method on images that don't have exif data (no changes will be made).

### `best_fit($max_width, $max_height)`

Proportionally resize the image to fit inside a specific width and height.

- `$max_width`* (int) - The maximum width the image can be.
- `$max_height`* (int) - The maximum height the image can be.

### `crop($x1, $y1, $x2, $y2)`

Crop the image.

- $x1 - Top left x coordinate.
- $y1 - Top left y coordinate.
- $x2 - Bottom right x coordinate.
- $y2 - Bottom right x coordinate.

### `flip($direction)`

Flip the image horizontally or vertically.

- `$direction`* (string) - The direction to flip: x|y|both

### `max_colors($max, $dither)`

Reduces the image to a maximum number of colors.

- `$max`* (int) - The maximum number of colors to use.
- `$dither` (bool) - Whether or not to use a dithering effect (default true).

### `overlay($overlay, $anchor, $opacity, $x_offset, $y_offset)`

Place an image on top of the current image.

- `$overlay`* (string|SimpleImage) - The image to overlay. This can be a filename, a data URI, or a SimpleImage object.
- `$anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right', 'top left', 'top right', 'bottom left', 'bottom right' (default 'center')
- `$opacity` (float) - The opacity level of the overlay 0-1 (default 1).
- `$x_offset` (int) - Horizontal offset in pixels (default 0).
- `$y_offset` (int) - Vertical offset in pixels (default 0).

### `resize($width, $height)`

Resize an image to the specified dimensions. If only one dimension is specified, the image will be resized proportionally.

- `$width`* (int) - The new image width.
- `$height`* (int) - The new image height.

### `rotate($angle, $background_color)`

Rotates the image.

- `$angle`* (int) - The angle of rotation (-360 - 360).
- `$background_color` (string|array) - The background color to use for the uncovered zone area after rotation (default 'transparent').

### `text($text, $options, &$boundary)`

Adds text to the image.

- `$text*` (string) - The desired text.
- `$options` (array) - An array of options.
  - `font_file`* (string) - The TrueType (or compatible) font file to use.
  - `size` (int) - The size of the font in pixels (default 12).
  - `color` (string|array) - The text color (default black).
  - `anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right',
    'top left', 'top right', 'bottom left', 'bottom right' (default 'center').
  - `x_offset` (int) - The horizontal offset in pixels (default 0).
  - `y_offset` (int) - The vertical offset in pixels (default 0).
  - `shadow` (array) - Text shadow params.
    - `x`* (int) - Horizontal offset in pixels.
    - `y`* (int) - Vertical offset in pixels.
    - `color`* (string|array) - The text shadow color.
- `$boundary` (array) - If passed, this variable will contain an array with coordinates that
  surround the text: [x1, y1, x2, y2, width, height]. This can be used for calculating the
  text's position after it gets added to the image.

### `thumbnail($width, $height, $anchor)`

Creates a thumbnail image. This function attempts to get the image as close to the provided dimensions as possible, then crops the remaining overflow to force the desired size. Useful for generating thumbnail images.

- `$width`* (int) - The thumbnail width.
- `$height`* (int) - The thumbnail height.
- `$anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right', 'top left', 'top right', 'bottom left', 'bottom right' (default 'center').

##  Drawing

### `arc($x, $y, $width, $height, $start, $end, $color, $thickness)`

Draws an arc.

- `$x`* (int) - The x coordinate of the arc's center.
- `$y`* (int) - The y coordinate of the arc's center.
- `$width`* (int) - The width of the arc.
- `$height`* (int) - The height of the arc.
- `$start`* (int) - The start of the arc in degrees.
- `$end`* (int) - The end of the arc in degrees.
- `$color`* (string|array) - The arc color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `border($color, $thickness)`

Draws a border around the image.

- `$color`* (string|array) - The border color.
- `$thickness` (int) - The thickness of the border (default 1).

### `dot($x, $y, $color)`

Draws a single pixel dot.

- `$x`* (int) - The x coordinate of the dot.
- `$y`* (int) - The y coordinate of the dot.
- `$color`* (string|array) - The dot color.

### `ellipse($x, $y, $width, $height, $color, $thickness)`

Draws an ellipse.

- `$x`* (int) - The x coordinate of the center.
- `$y`* (int) - The y coordinate of the center.
- `$width`* (int) - The ellipse width.
- `$height`* (int) - The ellipse height.
- `$color`* (string|array) - The ellipse color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `fill($color)`

Fills the image with a solid color.

- `$color` (string|array) - The fill color.

### `line($x1, $y1, $x2, $y2, $color, $thickness)`

Draws a line.

- `$x1`* (int) - The x coordinate for the first point.
- `$y1`* (int) - The y coordinate for the first point.
- `$x2`* (int) - The x coordinate for the second point.
- `$y2`* (int) - The y coordinate for the second point.
- `$color` (string|array) - The line color.
- `$thickness` (int) - The line thickness (default 1).

### `polygon($vertices, $color, $thickness)`

Draws a polygon.

- `$vertices`* (array) - The polygon's vertices in an array of x/y arrays. Example:
  ```
  [
    ['x' => x1, 'y' => y1],
    ['x' => x2, 'y' => y2],
    ['x' => xN, 'y' => yN]
  ]
  ```
- `$color`* (string|array) - The polygon color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `rectangle($x1, $y1, $x2, $y2, $color, $thickness)`

Draws a rectangle.

- `$x1`* (int) - The upper left x coordinate.
- `$y1`* (int) - The upper left y coordinate.
- `$x2`* (int) - The bottom right x coordinate.
- `$y2`* (int) - The bottom right y coordinate.
- `$color`* (string|array) - The rectangle color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `rounded_rectangle($x1, $y1, $x2, $y2, $radius, $color, $thickness)`

Draws a rounded rectangle.

- `$x1`* (int) - The upper left x coordinate.
- `$y1`* (int) - The upper left y coordinate.
- `$x2`* (int) - The bottom right x coordinate.
- `$y2`* (int) - The bottom right y coordinate.
- `$radius`* (int) - The border radius in pixels.
- `$color`* (string|array) - The rectangle color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

## Filters

### `blur($type, $passes)`

Applies the blur filter.

- `$type` (string) - The blur algorithm to use: 'selective', 'gaussian' (default 'gaussian').
- `$passes` (int) - The number of time to apply the filter, enhancing the effect (default 1).

### `brighten($percentage)`

Applies the brightness filter to brighten the image.

- `$percentage`* (int) - Percentage to brighten the image (0 - 100).

### `colorize($color)`

Applies the colorize filter.

- `$color`* (string|array) - The filter color.

### `contrast($percentage)`

Applies the contrast filter.

- `$percentage`* (int) - Percentage to adjust (-100 - 100).

### `darken($percentage)`

Applies the brightness filter to darken the image.

- `$percentage`* (int) - Percentage to darken the image (0 - 100).

### `desaturate()`

Applies the desaturate (grayscale) filter.

### `duotone($light_color, $dark_color)`

Applies the duotone filter to the image.

- `$light_color`* (string|array) - The lightest color in the duotone.
- `$dark_color`* (string|array) - The darkest color in the duotone.

### `edge_detect()`

Applies the edge detect filter.

### `emboss()`

Applies the emboss filter.

### `invert()`

Inverts the image's colors.

### `opacity()`

Changes the image's opacity level.

- `$opacity`* (float) - The desired opacity level (0 - 1).

### `pixelate($size)`

Applies the pixelate filter.

- `$size` (int) - The size of the blocks in pixels (default 10).

### `sepia()`

Simulates a sepia effect by desaturating the image and applying a sepia tone.

### `sharpen()`

Sharpens the image.

### `sketch()`

Applies the mean remove filter to produce a sketch effect.

## Color utilities

### `(static) adjust_color($color, $red, $green, $blue, $alpha)`

Adjusts a color by increasing/decreasing red/green/blue/alpha values independently.

- `$color`* (string|array) - The color to adjust.
- `$red`* (int) - Red adjustment (-255 - 255).
- `$green`* (int) - Green adjustment (-255 - 255).
- `$blue`* (int) - Blue adjustment (-255 - 255).
- `$alpha`* (float) - Alpha adjustment (-1 - 1).

### `(static) darken_color($color, $amount)`

Darkens a color.

- `$color`* (string|array) - The color to darken.
- `$amount`* (int) - Amount to darken (0 - 255).

### `extract_colors($count = 10, $backgroundColor = null)`

Extracts colors from an image like a human would do.™ This method requires the third-party library \League\ColorExtractor. If you're using Composer, it will be installed for you automatically.

- `$count` (int) - The max number of colors to extract (default 5).
- `$backgroundColor` (string|array) - By default any pixel with alpha value greater than zero will be discarded. This is because transparent colors are not perceived as is. For example, fully transparent black would be seen white on a white background. So if you want to take transparency into account, you have to specify a default background color.

### `get_colorAt($x, $y)`

Gets the RGBA value of a single pixel.

- `$x`* (int) - The horizontal position of the pixel.
- `$y`* (int) - The vertical position of the pixel.

### `(static) lighten_color($color, $amount)`

Lightens a color.

- `$color`* (string|array) - The color to lighten.
- `$amount`* (int) - Amount to darken (0 - 255).

### `(static) normalize_color($color)`

Normalizes a hex or array color value to a well-formatted RGBA array.

- `$color`* (string|array) - A CSS color name, hex string, or an array [red, green, blue, alpha].

You can pipe alpha transparency through hex strings and color names. For example:

  #fff|0.50 <-- 50% white
  red|0.25 <-- 25% red

# Mailing Class (COD_Mailing)

CodeDmx provides a simple, chainable class for sending basic emails.

## First of all: A simple example

```php
<?php
$this->mail->set_to('youremail@gmail.com', 'Your Email')
  ->set_subject('Test Message')
  ->set_from('no-reply@domain.com', 'Domain.com')
  ->add_mail_header('Reply-To', 'no-reply@domain.com', 'Test Name')
  ->add_generic_header('X-Mailer', 'PHP/' . phpversion())
  ->add_generic_header('Content-type', 'text/html; charset="utf-8"')
  ->set_message('<strong>This is a test message.</strong>')
  ->set_wrap(100);
$send = $this->mail->send();
echo ($send) ? 'Email sent successfully' : 'Could not send email';
```

## Sending an Attachment

```php
<?php
$this->mail->set_to('mailtest@gmail.com', 'Test user')
  ->set_from('mailtest', 'Test user 2')
  ->set_subject('This is a test message')
  ->add_attachment('test.txt')
  ->add_attachment('test.txt', 'test_attachment.txt')
  ->set_message('Hi there');
$send = $this->mail->send();
echo ($send) ? 'Email sent successfully' : 'Could not send email';
```

## Available Methods

```php
<?php
// $email The email address to send to
// $name The name of the person to send to
$this->mail->set_to($email, $name);
/* $this->mail->set_to('rober@google.com', 'Robert'); */

// Set the email subject
$this->mail->set_subject($subject);
/* $this->mail->set_subject('Test my mail of CodeDmx'); */

// Set the message to send
$this->mail->set_message($message);
/* $this->mail->set_message('Here is my test of mailing with the PHP Framework CodeDmx'); */
/* $this->mail->set_message('<p>Here is my test with HTML</p>'); */

// $path The file path of the attachment
// Optional $filename The filename of the attachment when emailed
$this->mail->add_attachment($path, $filename = null);
/* $this->mail->add_attachment('/path/to/file.txt') */
/* $this->mail->add_attachment('/path/to/file.txt', 'new_name_file.txt') */

// $email The email to send as from
// $name The name to send as from
$this->mail->set_from($email, $name);
/* $this->mail->set_from('ruben@google.com', 'Ruben') */

// $header The header to add
// Optional $email The email to add
// Optional $name The name to add
$this->mail->add_mail_header($header, $email = null, $name = null);
/* $this->mail->add_mail_header('Reply-To', 'zuck@google.com', 'Zuck') */

// $header The generic header to add
// $value The value of the header
$this->mail->add_generic_header($header, $value);
/* $this->mail->add_generic_header('Content-type', 'text/html, charset="utf-8"') */

// $wrap The number of characters at which the message will wrap
$this->mail->set_wrap($wrap);
/* $this->mail->set_wrap(100); */

// Send to 'To: ' address
$this->mail->send();
/*
$send = $this->mail->send();
echo ($send) ? 'Email send' : 'Unable to send';
*/
```

# TimeAgo Class (COD_TimeAgo)

> How to determine, what "ago" is

```php
<?php
  0 <-> 29 secs                                                             # => less than a minute
  30 secs <-> 1 min, 29 secs                                                # => 1 minute
  1 min, 30 secs <-> 44 mins, 29 secs                                       # => [2..44] minutes
  44 mins, 30 secs <-> 89 mins, 29 secs                                     # => about 1 hour
  89 mins, 29 secs <-> 23 hrs, 59 mins, 29 secs                             # => about [2..24] hours
  23 hrs, 59 mins, 29 secs <-> 47 hrs, 59 mins, 29 secs                     # => 1 day
  47 hrs, 59 mins, 29 secs <-> 29 days, 23 hrs, 59 mins, 29 secs            # => [2..29] days
  29 days, 23 hrs, 59 mins, 30 secs <-> 59 days, 23 hrs, 59 mins, 29 secs   # => about 1 month
  59 days, 23 hrs, 59 mins, 30 secs <-> 1 yr minus 1 sec                    # => [2..12] months
  1 yr <-> 2 yrs minus 1 secs                                               # => about 1 year
  2 yrs <-> max time or date                                                # => over [2..X] years
```

Simple module, that displays the date in a "time ago" format.


> Using this class

```php
<?php
// The array must have two values
// First one is about TimeZone like 'America/Chihuahua'
// Second one is about language, by default is in spanish
$arg = array(null, 'en');
$this->load->library('timeago', $arg);

echo $this->timeago->in_words('2016-06-05 10:10:10');
```

> Do you want the actual years, months, days, hours, minutes, seconds difference?

```php
<?php
$this->load->library('timeago');
$dateDifferenceArray =  $this->timeago->date_difference("2017-03-02 07:53:00", "2017-03-02 07:53:01");

// This will return an array with the following data:

[
  'years' => 0
  'months' => 0
  'days' => 0
  'hours' => 0
  'minutes' => 0
  'seconds' => 1
]
```

# FTP class (COD_Ftp)

CodeDmx has a FTP Class, that permits files to be transferred to a remote server. By this class you can move, rename and delete files or folders.

<aside class="warning">SFTP AND SSL FTP protocols are not supported.</aside>

If you prefer you can store your FTP preferences in a config file in /Model/CFG.php, and this will be used automatically.

> First of all you need to load class

```php
<?php
$this->load->library('ftp');

//Then
$this->ftp->connect();
```

## Connect to host

Connects to the FTP server. Connection preferences are set by passing an array to the function, or you can store them in the config file in /Model/CFG.php.

```php
<?php
$ftp['hostname'] = 'ftp.host.com';
$ftp['username'] = 'your-username';
$ftp['password'] = 'your-password';

$this->ftp->connect($ftp);
```
### Available connection options

Option name | Default value
-------------- | --------------
hostname |	n/a
username |	n/a
password |	n/a
port |	21
passive |	TRUE	[TRUE/FALSE (boolean)]

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

# Upload class (COD_Upload)

CodeDmx has an Upload Class, that permits upload files to a remote server.

> First of all you need to load class

```php
<?php
$this->load->library('upload');

//Then
$this->upload->method();
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
