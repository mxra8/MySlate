# Encryptation Class

CodeDmx has an encryptation class. Some people believe MD5 would be a safe way to encode passwords. Is a lie! MD5 is a good method to obscure non-sensitive data, because it's quite fast.

<aside class="warning">The default schema is $2y$, which makes use of the new, corrected hash implementation. This can be modify in encrypt.settings.php file</aside>

> First of all you need to load the library file

```php
<?php
$this->load->library('crypt');
```

## A simple example

> To hash the password, you only need to call one method

```php
<?php
$password = 'my_password';
echo $this->crypt->hash_password($password);
// Something like: $2y$12$lyAb7hOusPTvEGJBVmia4OGV8nU0pFhBOvG4NZRT3P/1lA3SAMoFK
```

> To check if password against the $hashed, and this returns TRUE / FALSE

```php
<?php
if ($this->crypt->check_password($password, $hashed))
{
  // continue
}
```

## Work factor

To increase your hash security this method accepts the optional parameter $work_factor. This defines the number of rounds. With each round the creation time doubles, so the system is exponentially.

```php
<?php
$work_factor = 16;
$password = 'my_password';
echo $this->crypt->hash_password($password, $work_factor);
```

## Change the schema

To alter the default scheme, change the GLOBAL variable on your config file (/application/Config/encrypt.settings.php)$GLOBALS['COD']->identifier to the next strings

2a - Hash wich is potentially generated with the buggy algorithm

2x - "compatibility" option the buggy Bcrypt implementation

2y - Hash generated with the new, corrected algorithm implementation (crypt_blowfish 1.1 and newer)

## Structure

$2a$12$Some22CharacterSaltXXO6NC3ydPIrirIzk1NdnTz0L/aCaHnlBa

$2a$ tells PHP to use which Blowfish scheme (Bcrypt is based on Blowfish)

12$ is the number f iterations the hashing mechanism uses

Some22CharacterSaltXX0 is a random salt (by OpenSSL)

> Diagram:

```php
<?php
$2a$12$Some22CharacterSaltXXO6NC3ydPIrirIzk1NdnTz0L/aCaHnlBa
\___________________________/\_____________________________/
  \                            \
   \                            \ Actual Hash (31 chars)
    \
     \  $2a$   12$   Some22CharacterSaltXXO
        \__/    \    \____________________/
          \      \              \
           \      \              \ Salt (22 chars)
            \      \
             \      \ Number of Rounds (work factor)
              \
               \ Hash Header
```
