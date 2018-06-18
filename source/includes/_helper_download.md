# Download helper

The Download Helper lets you download data to your desktop.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('download');
```

## Download file

Generates headers that force a download to happen

```php
<?php
/**
 * @param	mixed	filename (or an array of local file path => destination filename)
 * @param	mixed	the data to be downloaded
 * @param	bool	whether to try and send the actual file MIME type
 *
 * @return	void
*/
$this->download->file($GLOBALS['COD']->doc . 'json' . DS . 'test.json');
```
