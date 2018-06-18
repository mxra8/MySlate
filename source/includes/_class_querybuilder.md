# Query Builder Class

CodeDmx has a Mysqli Query Builder. This class can help you minified your code to connect with the database.

<aside class="warning">You need to edit your db.settings.php file to connect with the database</aside>

> First of all you need to load the library file

```php
<?php
$this->load->library('db');

// Then
$this->db->get('users');
```

## Add multiple connection

```php
<?php
$this->db->add_connection('slave', $GLOBALS['COD']->db['other']);

// Then
$this->db->connection('slave')->get('clients');
```

## Select Query

The following functions allow you to build SQL SELECT statements:

```php
<?php
$users = $this->db->get('users');
print_r($users);
$users = $this->db->get('users', 10);
print_r($users);
```

> You can select with custom columns set.

```php
<?php
$cols = array('id', 'name', 'email');
$users = $this->db->get('users', null, $cols);
if ($this->db->count > 0)
{
    foreach ($users as $user)
    {
        print_r($user);
    }
}
```

> You can select just one row

```php
<?php
$this->db->where('id', 1);
$user = $this->db->get_one('users');
echo $user['id'];

$stats = $this->db->get_one('users', 'sum(id), count(*) as cnt');
echo "total " . $stats['cnt'] . "users found";
```

> You can select one column value of function result

```php
<?php
$count = $this->db->get_value('users', 'count(*)');
echo "{$count} users found";
```

> You can select one column value or function result from multiple rows:

```php
<?php
// Select login from users
$logins = $this->db->get_value('users', 'login', null);
// Select login from users limit 5
$logins = $this->db->get_value('users', 'login', 5);

foreach ($logins as $login)
{
    echo $login;
}
```

## Insert Query

> Just a simple example:

```php
<?php
$data = array(
    "login" => "admin",
    "firstName" => "John",
    "lastName" => "Dow"
);

$id = $this->db->insert('users', $data);

if ($id)
{
    echo "user was created. Id=" . $id;
}
```

> Insert with functions:

```php
<?php
$data = array(
    'login' => 'admin',
    'active' => true,
    'firstName' => 'John',
    'lastName' => 'Dow',
    'password' => $this->db->func('SHA1(?)', array('secretpassword+salt')),
    // password = SHA1('secretpassword+salt')
    'createdAt' => $this->db->now(),
    // createdAt = NOW()
    'expires' => $this->db->now('+1Y')
    // expires = NOW() + interval 1 year
    // Supported intervals [s]econd, [m]inute, [h]our, [d]ay, [M]onth, [Y]ear
);

$id = $this->db->insert('users', $data);
if ($id)
{
    echo "user was created. Id=" . $id;
}
else
{
    echo "insert failed " . $this->db->get_last_error();
}
```

> Insert with on duplicate key update

```php
<?php
$data = array(
    'login' => 'admin',
    'firstName' => 'John',
    'lastName' => 'Dow',
    'createdAt' => $this->db->now(),
    'updatedAt' => $this->db->now()
);

$update_columns = array('updatedAt');
$last_insert_id = 'id';
$this->db->on_duplicate($update_columns, $last_insert_id);
$id = $this->db->insert('users', $data);
```

> Insert multiple data at once

```php
<?php
$data = array(
    array('login' => 'admin',
        'firstName' => 'John',
        'lastName' => 'Doe'
    ),
    array('login' => 'other',
        'firstName' => 'Another',
        'lastName' => 'User',
        'password' => 'very_cool_hash'
    )
);

$ids = $this->db->insert_multi('users', $data);

if ( ! $ids)
{
    echo 'insert failed: ' . $this->db->get_last_error();
}
else
{
    echo 'new users inserted with following id\'s: ' . implode(', ', $ids);
}
```

> If all datasets only have the same keys, it can be simplified

```php
<?php
$data = array(
    array('admin', 'John', 'Doe'),
    array('other', 'Another', 'User'),
);
$keys = array('login', 'firstName', 'lastName');

$ids = $this->db->insert_multi('users', $data, $keys);
if ( ! $ids)
{
    echo 'insert failed: ' . $this->db->get_last_error();
}
else
{
    echo 'new users inserted with following id\'s: ' . implode(', ', $ids);
}
```

## Update Query

```php
<?php
$data = array(
    'firstName' => 'Bobby',
    'lastName' => 'Tables',
    // editCount = editCount + 2;
    'editCount' => $this->db->inc(2),
    // active = !active;
    'active' => $this->db->not()
);

$this->db->where('id', 1);
if ($this->db->update('users', $data))
{
    echo $this->db->count . ' records were updated';
}
else
{
    echo 'update failed: ' . $this->db->get_last_error();
}
```

> Update also support limit parameter

```php
<?php
$this->db->update('users', $data, 10);
// Gives: UPDATE users SET ... LIMIT 10
```

## Delete Query

```php
<?php
$this->db->where('id', 1);
if ($this->db->delete('users'))
{
    echo 'successfully deleted';
}
```

## Where / Having Methods

All conditions supported by where() are supported by having() as well.

```php
<?php
$this->db->where('id', 1);
$this->db->where('login', 'admin');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id = 1 AND login = 'admin'

$this->db->where('id', 1);
$this->db->having('login', 'admin');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id = 1 HAVING login = 'admin'
```

> Compare with column to column

```php
<?php
// WRONG
$this->db->where('lastLogin', 'createdAt');

// CORRECT
$this->db->where('lastLogin = createdAt');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE lastLogin = createdAt;

$this->db->where('id', 50, ">=");
// or $this->db->where('id', array('>=' => 50));
$results = $this->db->get('users');
print_r($results);
//Gives: SELECT * FROM users WHERE id >= 50
```

> LIKE

```php
<?php
$this->db->where('column_name', '%string%', 'LIKE');
$rows = $this->db->get('table_name');
// Gives: SELECT * FROM table_name WHERE column_name LIKE '%string%'

// Or you can use
$this->db->where('IP', '%' . $VAR . '%', 'LIKE');
```

> BETWEEN / NOT BETWEEN

```php
<?php
$this->db->where('id', array(4, 20), 'BETWEEN');
// or $this->db->where('id', array('BETWEEN' => array(4, 20)));

$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id BETWEEN 4 AND 20
```

> IN / NOT IN

```php
<?php
$this->db->where('id', array(1, 5, 27, -1, 'd'), 'IN');
// or $this->db->where('id', array('IN' => array(1, 5, 27, -1, 'd')));

$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id IN (1, 5, 27, -1, 'd');
```

> OR CASE

```php
<?php
$this->db->where('firstName', 'John');
$this->db->or_where('firstName', 'Peter');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE firstName='John' OR firstName='peter'
```

> NULL COMPARISON

```php
<?php
$this->db->where('lastName', NULL, 'IS NOT');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE lastName IS NOT NULL
```

> Also you can use raw where conditions:

```php
<?php
$this->db->where("id != companyId");
$this->db->where('DATE(createdAt) = DATE(lastLogin)');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE id != companyId AND DATE(createdAt) = DATE(lastLogin)
```

> Or raw condition with variables:

```php
<?php
$this->db->where("(id = ? OR id = ?)", array(6, 2));
$this->db->where('login', 'mike');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users WHERE (id = 6 OR id = 2) AND login='mike'
```

> Find the total number of rows matched. Simple pagination example:

```php
<?php
$offset = 10;
$count = 15;
$users = $this->db->with_total_count()->get('users', array($offset, $count));
echo "Showing {$count} from {$this->db->total_count}";
```

## Ordering method

```php
<?php
$this->db->order_by('id', 'ASC');
$this->db->order_by('login', 'DESC');
$this->db->order_by('RAND ()');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users ORDER BY id ASC, login DESC, RAND();
```

> Order by values

```php
<?php
$this->db->order_by('userGoup', 'ASC', array('superuser', 'admin', 'users'));
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users ORDER BY FIELD (userGroup, 'superuser', 'admin', 'users') ASC;
```

> If you are using set_prefix() functionality and need to use tables names in order_by() method make sure that table names are escaped with ''.

```php
<?php
// WRONG
$this->db->set_prefix("t_");
$this->db->order_by("users.id", 'ASC');
$results = $this->db->get('users');
// WRONG: SELECT * FROM t_users ORDER BY users.id ASC;

$this->db->set_prefix("t_");
$this->db->order_by("users.id", 'ASC');
$results = $this->db->get('users');
// CORRECT: SELECT * FROM t_users ORDER BY t_users.id ASC;
```

## Groping Method

```php
<?php
$this->db->group_by('name');
$results = $this->db->get('users');
print_r($results);
// Gives: SELECT * FROM users GROUP BY name;
```

## JOIN Method

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->where('u.id', 6);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
```

> JOIN with 3 tables

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join('articles a', 'a.id=u.id', 'LEFT');
$this->db->where('u.id', 6);
$results = $this->db->get('posts p', null, 'u.firstName, p.title, a.lolname');
print_r($results);
```

> JOIN with AND condition

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join_where('users u', 'u.id', 5);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
// Gives: SELECT u.firstName, p.title FROM products p LEFT JOIN users u ON (p.id=u.id AND u.id=5)
```

> JOIN with OR condition

```php
<?php
$this->db->join('users u', 'p.id=u.id', 'LEFT');
$this->db->join_or_where('users u', 'u.id', 5);
$results = $this->db->get('posts p', null, 'u.firstName, p.title');
print_r($results);
// Gives: SELECT u.firstName, p.title FROM products p LEFT JOIN users u ON (p.id=u.id OR u.id=5)
```

## Has method

A convenient function that returns TRUE if exists at least an element that satisfy the where condition specified calling the "where" method before this one.

```php
<?php
$this->db->where('user', $user);
$this->db->where('password', md5($password));
if ($this->db->has('users'))
{
    echo 'You are logged';
}
else
{
    echo 'Wrong user/password';
}
```

## Helper methods

> Reconnect in case mysql connection died:

```php
<?php
if ( ! $this->db->ping())
{
    $this->db->connect();
}
```

> Get last executed SQL query

```php
<?php
$this->db->get('users');
echo 'Last executed query was '. $this->db->get_last_query();
```

> Check if table exists

```php
<?php
if ($this->db->table_exists('users'))
{
    echo 'lml';
}
```

> Real escape string

```php
<?php
$escaped = $this->db->escape("' and 1=1");
```

> Error helpers

```php
<?php
$this->db->where('login', 'admin')->update('users', ['firstName' => 'Jack']);
if ($this->db->get_last_errno() === 0)
{
    echo 'Update succesfull';
}
else
{
    echo 'Update failed. Erro: '. $this->db->get_last_error();
}
```

## Properties sharing

Also is possible to copy properties

```php
<?php
$this->db->where('agentId', 10);
$this->db->where('active', TRUE);

$customers = $this->db->copy();
$res = $customers->get('customers', array(10, 10));
// Gives: SELECT * FROM customers WHERE agentId = 10 AND active = 1 LIMIT 10,10

$cnt = $this->db->get_value('customers', 'count(id)');
echo 'total records found: '. $cnt;
//Gives: SELECT count(id) FROM users WHERE agentId = 10 AND active = 1
```

## Pagination

Use paginate() instead of get() to fetch paginated result

```php
<?php
$page = 1;
$this->db->page_limit = 2;
$products = $this->db->array_builder()->paginate('products', $page);
echo 'showing $page out of '. $this->db->total_pages;
```

## Query exectution time benchmarking

To track query execution time set_trace() function should be called.

```php
<?php
$this->db->set_trace(TRUE);
$this->db->get('users');
$this->db->get('test');
print_r($this->db->trace);

/*
Array
(
    [0] => Array
        (
            [0] => SELECT  * FROM users
            [1] => 0.00048494338989258
        )

    [1] => Array
        (
            [0] => SELECT  * FROM test
            [1] => 0.0076069831848145
        )
)
*/
```

## Subqueries

> Subquery without an alias to use in insert/update/where Eg. (SELECT * FROM users)

```php
<?php
$subq = $this->db->sub_query();
$subq->get('users');
```

> Subquery in select

```php
<?php
$ids = $this->db->sub_query();
$ids->where('qty', 2, '>');
$ids->get('products', null, 'userId');

$this->db->where('id', $ids, 'in');
$res = $this->db->get('users');
// Gives SELECT * FROM users WHERE id IN (SELECT userId FROM products WHERE qty > 2)
```

> Subquery in join

```php
<?php
$users = $this->db->sub_query('u');
$users->where('active', 1);
$users->get('users');

$this->db->join($users, 'p.userId=u.id', 'LEFT');
$products = $this->db->get('products p', null, 'u.login, p.productName');
print_r($products);
```

> Subquery in insert

```php
<?php
$user = $this->db->sub_query();
$user->where('where', 6);
$user->get_one('users', 'name');

$data = array(
    'productName' => 'test product',
    'userId' => $user,
    'lastUpdated' => $this->db->now()
);

$id = $this->db->insert('products', $data);
// Gives: INSERT INTO products (productName, userId, lastUpdated) VALUES ("test product", (SELECT name FROM users WHERE id = 6), NOW());
```

## EXISTS / NOT EXISTS condition

```php
<?php
$sub = $this->db->sub_query();
$sub->where('company', 'testCompany');
$sub->get('users', null, 'userId');

$this->db->where(null, $sub, 'exists');
$products = $this->db->get('products');
print_r($products);
// Gives: SELECT * FROM products WHERE EXISTS (SELECT userId FROM users WHERE company='testCompany')
```

## Queries

This also can run your SQL queries directly

```php
<?php
$users = $this->db->raw_query('SELECT * FROM users WHERE id >= ?', array(10));
foreach ($users as $user)
{
    print_r($user);
}
```

> Get one row of results

```php
<?php
$user = $this->db->raw_query_one('SELECT * FROM users WHERE id=?', array(10));
echo $user['login'];

// Object return type
$user = $this->db->object_builder()->raw_query_one('SELECT * FROM users WHERE id=?', array(10));
echo $user->login;
```

> Get one column value as a string

```php
<?php
$password = $this->db->raw_query_value('SELECT password FROM users WHERE id=? limit 1', array(10));
// For a raw_query_value() return a string instead of an array, 'limit 1' should be added to the end of the query
echo 'Password is {$password}';
```

> Get one colum value from multiple rows

```php
<?php
$login = $this->db->raw_query_value('SELECT login FROM users LIMIT 10');
foreach ($login as $var)
{
    echo $var;
}
```

> An advanced examples

```php
<?php
$params = array(1, 'admin');
$users = $this->db->raw_query('SELECT id, firstName, lastName FROM users WHERE id = ? AND login = ?', $params);
print_r($users);

$params = array(10, 1, 10, 11, 2, 10);
$q = '(
    SELECT a FROM t1
        WHERE a = ? AND B = ?
        ORDER BY a LIMIT ?
) UNION (
    SELECT a FROM t2
        WHERE a = ? AND B = ?
        ORDER BY a LIMIT ?
);';
$result = $this->db->raw_query($q, $params);
print_r($result);
```
