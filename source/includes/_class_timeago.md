# TimeAgo Class

> First of all you need to load the library file

```php
<?php
// The array must have two values
// First one is about TimeZone like 'America/Mexico'
// Second one is about language, by default is in spanish (es)
$arg = array(null, 'en');
$this->load->library('timeago', $arg);
```



> How to determine, what "ago" is

```php
<?php
  0 <-> 29 secs                                                             # => less than a minute
  30 secs <-> 1 min, 29 secs                                                # => 1 minute
  1 min, 30 secs <-> 44 mins, 29 secs                                       # => [2..44] minutes
  44 mins, 30 secs <-> 89 mins, 29 secs                                     # => about 1 hour
  89 mins, 29 secs <-> 23 hrs, 59 mins, 29 secs                             # => about [2..24] hours
  23 hrs, 59 mins, 29 secs <-> 47 hrs, 59 mins, 29 secs                     # => 1 day
  47 hrs, 59 mins, 29 secs <-> 29 days, 23 hrs, 59 mins, 29 secs            # => [2..29] days
  29 days, 23 hrs, 59 mins, 30 secs <-> 59 days, 23 hrs, 59 mins, 29 secs   # => about 1 month
  59 days, 23 hrs, 59 mins, 30 secs <-> 1 yr minus 1 sec                    # => [2..12] months
  1 yr <-> 2 yrs minus 1 secs                                               # => about 1 year
  2 yrs <-> max time or date                                                # => over [2..X] years
```

Simple module, that displays the date in a "time ago" format.


> Using this class

```php
<?php
echo $this->timeago->in_words('2016-06-05 10:10:10');
```

> Do you want the actual years, months, days, hours, minutes, seconds difference?

```php
<?php
$this->load->library('timeago');
$dateDifferenceArray =  $this->timeago->date_difference("2017-03-02 07:53:00", "2017-03-02 07:53:01");

// This will return an array with the following data:

[
  'years' => 0
  'months' => 0
  'days' => 0
  'hours' => 0
  'minutes' => 0
  'seconds' => 1
]
```
