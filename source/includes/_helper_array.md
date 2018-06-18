# Array Helper

The Array Helper file contains functions that assist in working with arrays.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('array');
```

## Random element

Takes an array as input and returns a random element

```php
<?php
/**
 * @param	array
 *
 * @return	mixed	depends on what the array contains
*/
$array = array('blue', 'red', 'green');
$this->array->random_element($array);
```

## Array to string

Convert Array to string expected output: <key1>="value1" <key2>="value2"

```php
<?php
/**
 * @param  array $array
 *
 * @return string
*/
$array = array(1 => 'blue', 2 => 'red', 3 => 'green');
$this->array->random_element($array);
```

## Chunk an array

Chunks an array into smaller arrays of a specified size.

```php
<?php
/**
* @param array $array
* @param int $size
*
* @return array
*/
$this->array->chunk([1, 2, 3, 4, 5], 2);
// output: [[1, 2], [3, 4], [5]]
```

## Deep Flatten

Deep flattens an array.

```php
<?php
$this->array->deep_flatten([1, [2], [[3], 4], 5]);
// output: [1, 2, 3, 4, 5]
```

## Drop

Returns a new array with n elements removed from the left.

```php
<?php
/**
* @param array $array
* @param int $n default: 1
*
* @return array
*/
$this->array->drop([1, 2, 3]);
// output: [2,3]

$this->array->drop([1, 2, 3], 2);
// output: [3]
```

## Flatten

Flattens an array up to the one level depth.

```php
<?php
$this->array->flatten([[1, [2], 3, 4]);
// output: [1, 2, 3, 4]
```

## Has duplicates

Checks a flat list for duplicate values. Returns *true* if duplicate values exists and *false* if values are all unique.

```php
<?php
var_dump($this->array->has_duplicates([1, 2, 3, 4, 5, 5]));
// output: true
```
