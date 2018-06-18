# Cache Class

A light, simple but powerful PHP5 Cache Class which uses the filesystem for caching.

Basically the caching class stores its data in files in the JSON format. These files will be created if you store data under a Cache name.

If you set a new Cache name with set_cache(), a new cache file will be generated. The Cache will store all further data in the new file. The Setter method allows you to switch between the different Cache files.

> First of all you need to load the library file

```php
<?php
$this->load->library('cache');
```

## Set cache

*set_cache($name)*

If you want to switch to another Cache or create a new one, then use this method to set a new Cache name.

```php
<?php
$this->cache->set_cache('newcache');
```

## Store

*store($key, $data, <$expiration>)*

* The key value defines a tag with which the cached data will be associated.
* The data value can be any type of object (will be serialized).
* The expiration value allows you to define an expiration time.

To change data you can overwrite it by using the same key identifier.
Beside the data, the Cache will also store a timestamp.

> Store a string

```php
<?php
$this->cache->store('hello', 'Hello World!');
```

> Store an array

```php
<?php
$this->cache->store('movies', array(
    'description' => 'Movies on TV',
    'action' => array(
        'Tropic Thunder',
        'Bad Boys',
        'Crank'
    )
));
```

## Get cache elements

*retrieve($key, <$timestamp>)*

Get particular cached data by its key.
To retrieve the timestamp of a key, set the second parameter to true.

*retrieve_all(<$meta>)*

This allows you retrieve all the cached data at once. You get the meta data by setting the $meta argument to true.

```php
<?php
$result = $this->cache->retrieve('moviews');

print_r($result);

$description = $result['description'];
```

## Switch back

Switch back to the first cache

```php
<?php
$this->cache->set_cache('mycache');

$this->cache->store('hello', 'Hello everybody out there!');
```

## Erase entry

For erasing cached data are these three methods available:

* erase($key) Erases a single entry by its key.
* erase_all() Erases all entries from the Cache file.
* erase_expired() Erases all expired entries.

```php
<?php
$this->cache->erase('hello');

// OR
$this->cache->erase_all();

// OR
$this->cache->erase_expired();
```

## Check cached data

*is_cached($key)*

Check whether any data is associated with the given key.
Returns true or false.

## Set Cache path

*set_cache_path($path)*

The path to the Cache folder must end with a backslash: my_path_to_the_cache_folder/

## Get Cache file path

*get_cache_dir()*

The method returns the path to your current Cache file (the Cache name is always sh1 encoded):

cache/7505d64a54e061b7acd54ccd58b49dc43500b635.cache
