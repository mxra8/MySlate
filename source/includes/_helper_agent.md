# Agent Helper

Agent helper can help you about browser or agent user is use it.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('agent');
```

## Know browser

```php
<?php
echo $this->agent->know_browser();
// In my case, will output: Google Chrome, Version: 67.0.3396.79, Mac OS2
```

## Is mobile

```php
<?php
// This will return void
var_dump($this->agent->is_mobile());
```

## Curl

```php
<?php
// You can use this method to make curl calls
/**
 * @param  string  $url
 * @param  string  $method
 * @param  mixed $data
 * @param  mixed $headers
 * @param  boolean $return
*/
var_dump($this->agent->curl('http://yoursite.com', 'GET'));
```
