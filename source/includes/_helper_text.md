# Text helper

The Text Helper file contains functions that assist in working with text.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('text');
```

## Cut string

Truncate String with or without ellipsis

```php
<?php
/**
 * @param  string  $string
 * @param  int  $max_length
 * @param  boolean $add_ellipsis
 * @param  boolean $word_safe
 *
 * @return string
*/

$string = "Here is a nice text string consisting of eleven words.";
echo $this->text->cut_string($string, 20);
//output: Here is a nice text string
```

## Word censor

Supply a string and an array of disallowed words and any matched words will be converted to #### or to the replacement word you've submitted.

```php
<?php
/**
 * @param	string	the text string
 * @param	string	the array of censored words
 * @param	string	the optional replacement value
 *
 * @return	string
*/
$string = 'Hi Sophie, how do shucks are you?';
$disallowed = array('darn', 'shucks', 'golly', 'phooey');
echo $this->text->word_censor($string, $disallowed, 'Beep!');
// Output: Hi Sophie, how do Beep! are you?
```
