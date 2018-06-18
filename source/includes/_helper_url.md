# URL Helper

The URL Helper file contains functions that assist in working with URLs.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('url');
```

## Current URL

Returns the full URL (including segments) of the page being currently viewed.

```php
<?php
echo $this->url->get_current_URL();
// Output: https://yoursite.com/contact/
```

## Is https

Check if your site run on https

```php
<?php
var_dump($this->url->is_https());
```

## Is AJAX

Determine if current page request type is ajax

```php
<?php
var_dump($this->url->is_ajax());
```
