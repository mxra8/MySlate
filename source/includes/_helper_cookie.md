# Cookie Helper

The Cookie Helper file contains functions that assist in working with cookies.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('cookie');
```

## Set cookie

Accepts seven parameters, or you can submit an associative array in the first parameter containing all the values.

```php
<?php
/**
 * @param	mixed
 * @param	string	the value of the cookie
 * @param	int	the number of seconds until expiration
 * @param	string	the cookie domain.  Usually:  .yourdomain.com
 * @param	string	the cookie path
 * @param	string	the cookie prefix
 * @param	bool	true makes the cookie secure
 * @param	bool	true makes the cookie accessible via http(s) only (no javascript)
 *
 * @return	void
*/
$this->cookie->set('user', 2);
```

## Get cookie

Fetch an item from the COOKIE array

```php
<?php
/**
 * @param	string
 * @param	bool
 *
 * @return	mixed
*/
$this->cookie->get('user');
// If not exists will return false
// Else will return '2'
```

## Delete cookie

```php
<?php
/**
 * @param	mixed
 * @param	string	the cookie domain. Usually: .yourdomain.com
 * @param	string	the cookie path
 * @param	string	the cookie prefix
 *
 * @return	void
*/
$this->cookie->delete('user');
// Will return void
```
