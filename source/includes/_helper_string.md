# String helper

The String Helper file contains functions that assist in working with strings.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('string');
```

## Random string

```php
<?php
/**
 * @param	string	type of random string.  basic, alpha, alnum, numeric, nozero, unique, md5, encrypt and sha1
 * @param	int	number of characters
 *
 * @return	string
*/
echo $this->string->random_string('alnum', 8);
// output: xg9tCHZb
```

## Reduce multiples

```php
<?php
/**
 * @param	string
 * @param	string	the character you wish to reduce
 * @param	bool	TRUE/FALSE - whether to trim the character from the beginning/end
 *
 * @return	string
*/
$string = 'Fred, Bill,, Joe, Jimmy';
echo $this->string->reduce_multiples($string, ',');
// output: Fred, Bill, Joe, Jimmy
```

## Create string

```php
<?php
echo $this->string->create_string(8);
// output: xg9tCHZb
```

## Reduce double slashes

Converts double slashes in a string to a single slash, except those found in http://

```php
<?php
$str = 'http://www.some-site.com//index.php';
echo $this->string->reduce_double_slashes(8);
// output: http://www.some-site.com/index.php
```

## Number to word

Convert number to word representation

```php
<?php
echo $this->string->number_to_word(8);
// output: eight
```

## Starts with

Check if a string is starts with a given substring.

```php
<?php
var_dump($this->string->starts_with('Hi, this is me', 'Hi'));
// output: true
```


## Ends with

Check if a string is ends with a given substring.

```php
<?php
var_dump($this->string->ends_with('Hi, this is me', 'me'));
// output: true
```

## First string between

Returns the first string there is between the strings from the parameter start and end.

```php
<?php
/**
* @param string $string
* @param string $start
* @param string $end
*
* @return string
*/
echo $this->string->first_string_between('This is a [custom] string', '[', ']');
// output: custom
```
