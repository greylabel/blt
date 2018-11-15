Questions:

How to handle BLT config?
How to handle PHPUnit config?

TODO:

Write docs on testing with BLT
Write docs on contrib workflow
Finish run-tests tasks
CI workflows


# PHPUnit


`sudo -u` 

### Resources
[Automated Testing in Drupal 8](https://drupalize.me/series/automated-testing-drupal-8)


### Config

`SIMPLETEST_DB`
`SIMPLETEST_BASE_URL`
`BROWSERTEST_OUTPUT_DIRECTORY`
`MINK_DRIVER_ARGS_WEBDRIVER`


```php
if ($_SERVER['HTTP_USER_AGENT'] == 'Drupal command line') {
    $databases['default']['default'] = array(
      'driver' => 'sqlite',
      'database' => '/tmp/test.sqlite',
      'namespace' => 'Drupal\\Core\\Database\\Driver\\sqlite',
    );
}
```


Two primary ways to run test `run-tests.sh` and the `phpunit` runner.

Running tests with `run-tests.sh`

`--class`
`--url`
`@group`
`--concurrency`
`--browser`
`--xml`
`--module`
`--directory`
`--all`
`--types`
`--sqlite`

Running tests with `phpunit`

`-config`
`--verbose`
`--filter`
`--testsuite`
`--group`
`--stop-on-error`
`--stop-on-fail`
`--testdox`
`--log-junit`
`--printer` `'Drupal\Tests\Listeners\HtmlOutputPrinter'`

