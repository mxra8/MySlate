# Security helper

The Security Helper file contains security related functions.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('security');
```

## Create an id

Permits you to create one way hashes suitable for encrypting passwords.
You can set md5 TRUE.

```php
<?php
echo $this->security->create_id();

// OR
// To set MD5 string
echo $this->security->create_id(TRUE);
```

## Crypt and decrypt string

To use this method, you need to set in *encrypt.settings.php* about encrypt method, secret key and secret iv.
Crypt and decrypt something with sha256 and secret strings.

> You can encrypt by default like this:

```php
<?php
echo $this->security->cryptit('123456');
// output: TDRncE5lN3I2UE1TZjhzcThuR0RUZz09
```

> And decrypt that string:

```php
<?php
echo $this->security->decryptit('TDRncE5lN3I2UE1TZjhzcThuR0RUZz09', FALSE);
// output: 123456
```

## Get ip

Returns the IP address of the client.

```php
<?php
echo $this->security->get_ip();
// output: 158.65.359.86
```

## Get Location

Get client location

```php
<?php
echo $this->security->get_location();
// output: Monterrey, MX
```
