# Session Class

CodeDmx has a Session class for you guessed it, managing sessions. Back in the day, when there weren't many open source frameworks on the market, developers had to code things from scratch to achieve certain functionality. This class's inception, is the from one of those instances. With this class you can easily time-out your authenticated users after a specified interval.

> First of all you need to load the library file

```php
<?php
$this->load->library('session');
```

## Set session

Set key/value in session

```php
<?php
$this->session->set('id', 12345);
// LIKE $_SESSION['id'] = 12345;

// OR

$session = array(
    'id' => 12345,
    'name' => 'John',
    'email' => 'test@mysite.com'
);
$this->session->set($session);
```

## Has seted

Verify that a session value exists

```php
<?php
var_dump($this->session->has_seted('id'));
// Output: bool
```

## Unset

Remove session data

```php
<?php
$this->session->_unset('id');
// LIKE unset($_SESSION['id']);
```

## Get

Retrieve value stored in session by key.

```php
<?php
echo $this->session->get('name');
// Output: John
// LIKE echo $_SESSION['name'];
```

## Destroy

Destroys the session.

```php
<?php
$this->session->end();
// similar like session_destroy();
```
