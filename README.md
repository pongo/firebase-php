Оригинал использует опцию CURL'а `CURLOPT_FOLLOWLOCATION`. Из-за нее на некоторых хостингах возникает ошибка:

> Warning: curl_setopt() [function.curl-setopt]: CURLOPT_FOLLOWLOCATION cannot be activated when safe_mode is enabled or an open_basedir is set in

В этом форке опция `CURLOPT_FOLLOWLOCATION` отключена. А редиректы обрабатываются другим способом.

---

# Firebase PHP Client

[![Build Status](https://drone.io/github.com/pongo/firebase-php/status.png)](https://drone.io/github.com/pongo/firebase-php/latest) [![Build Status](https://semaphoreci.com/api/v1/pongo/firebase-php/branches/master/badge.svg)](https://semaphoreci.com/pongo/firebase-php) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/pongo/firebase-php/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/pongo/firebase-php/?branch=master)

Based on the [Firebase REST API](https://www.firebase.com/docs/rest-api.html).


### Example
```php
const DEFAULT_URL = 'https://kidsplace.firebaseio.com/';
const DEFAULT_TOKEN = 'MqL0c8tKCtheLSYcygYNtGhU8Z2hULOFs9OKPdEp';
const DEFAULT_PATH = '/firebase/example';

$firebase = new \Firebase\FirebaseLib(DEFAULT_URL, DEFAULT_TOKEN);

// --- storing an array ---
$test = array(
    "foo" => "bar",
    "i_love" => "lamp",
    "id" => 42
);
$dateTime = new DateTime();
$firebase->set(DEFAULT_PATH . '/' . $dateTime->format('c'), $test);

// --- storing a string ---
$firebase->set(DEFAULT_PATH . '/name/contact001', "John Doe");

// --- reading the stored string ---
$name = $firebase->get(DEFAULT_PATH . '/name/contact001');
```

### Supported Commands
```php
// -- Firebase API commands

$firebase->set($path, $value);   // stores data in Firebase
$value = $firebase->get($path);  // reads a value from Firebase
$firebase->delete($path);        // deletes value from Firebase
$firebase->update($path, $data); // updates data in Firebase
$firebase->push($path, $data);   // push data to Firebase

// -- Query Parameters can be optionally used on all operations, example:

$value = $firebase->get($path, array('shallow' => 'true'));

// -- Query Parameter values with quotes, example (https://firebase.google.com/docs/database/rest/retrieve-data#filtering-by-a-specified-child-key):

$value = $firebase->get($path, array('orderBy' => '"height"'));

// -- Firebase PHP Library commands

$firebase->setToken($token);     // set up Firebase token
$firebase->setBaseURI($uri);     // set up Firebase base URI (root node)
$firebase->setTimeOut($seconds); // set up maximum timeout / request
```

Please refer to the [Firebase REST API documentation](https://www.firebase.com/docs/rest/api/) for further details.

### Firebase PHP Stub
A Firebase PHP Stub has been created to allow for integration with phpunit without actually interacting with FirebaseIO.

To use the firebaseLib and firebaseStub in your application and testing, you must pass in a firebase object to your application.

For example, if your current code is:

```php
public function setFirebaseValue($path, $value) {
  $firebase = new Firebase('https://radiant-fire-2427.firebaseio.com', 'czvEX8vMU8FZn4wYCvf466P3J6zH5ZlKQeuwxmEZ');
  $firebase->set($path, $value);
}
```

You will change it to be:

```php
public function setFirebaseValue($path, $value, $firebase) {
  $firebase->set($path, $value);
}
```

Then your phpunit tests will look like:

```php
<?php
  require_once '<path>/lib/firebaseInterface.php';
  require_once '<path>/lib/firebaseStub.php';

  class MyClass extends PHPUnit_Framework_TestCase
  {
    public function testSetFirebaseValue() {
      $myClass = new MyClass();
      $firebaseStub = new FirebaseStub($uri, $token);
      $myClass->setFirebaseValue($path, $value, $firebaseStub);
    }
  }
?>
```

### Unit Tests
All the unit tests are found in the "/tests" directory. Due to the usage of an interface, the tests must run in isolation.

The firebaseLib tests have inherent latency due to actual cURL calls to a live firebaseIO account. The firebaseLib tests can be executed by running the following command:

```bash
$ phpunit tests/firebaseTest.php
```

The FirebaseStub tests can be executed by running the following command:

```bash
$ phpunit tests/firebaseStubTest.php
```


#### The MIT License (MIT)
```
Copyright (c) 2012-2016 Tamas Kalman

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
