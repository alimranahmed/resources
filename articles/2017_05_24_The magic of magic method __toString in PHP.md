PHP has some of its method’s name that starts with __ (double underscore) as prefix. These methods are called magic method. It is recommended by PHP not to create any custom method as a name with __ prefix. Example of available magic methods in PHP are `__toString()`, `__sleep()`, `__isset()`, `__set()`, `__construct()` etc.

Today we are going to talk about `__toString` method of PHP. <br><br>

## __toString() Method
This method allows a class to decide how to react when an object of that class is being used as a string.<br>

For example, let’s think of a very simple class named as `TestClass`<br>

```php
class TestClass{
    public $msg = "Hello world";
}
```
<br>
Now, let’s create an object of that class as below

`$test = new TestClass();` <br><br>

What if we echo out this `$test` object of `TestClass` just like string? Let’s try it out

`echo $test;`<br><br>

The complete code is:

```php
class TestClass{
    public $msg = "Hello world";
}
$test = new TestClass();
echo $test;
```
<br>

Let’s execute the code, BOOM!! It’s an error! The error message says:

`PHP Catchable fatal error:  Object of class TestClass could not be converted to string in TestClass`
<br>

The error message has a very clear indication that we are using an instance of `TestClass`  as String. This is a SIN! I mean a **FATAL ERROR**.<br><br>

Can’t we do this in PHP? Can’t we use an object(or instance) of a Class as string? <br>
The answer is Yes, we can. But the real question is what do we expect? What do we want to see when that `$test` object is used as String?<br>
If we just want to see the content of `$msg` property of the object then we can do this using magic method `__toString` as below: <br>

```php
class TestClass{
    public $msg = "Hello world";
    public function __toString(){
        return $this->msg;
    }
}
```
<br>

What have we done here? We have added the magic method `__toString()` in our `TestClass`. That method return the content of `$msg`. We could have returned what we want as string from the `__toString()` method. I mean, it’s not mandatory to return a property of the object.

Let’s test it again our modified `TestClass` now. <br>

```php
$test = new TestClass();
echo $test;
```
<br>

The complete code is:
```php
class TestClass{
    public $msg = "Hello world";
    public function __toString(){
        return $this->msg;
    }
}
$test = new TestClass();
echo $test;
```
<br>

Let’s run it, YES!! The output is:<br><br>
```
Hello world
```
<br><br>

<strong>This is the magic of magic method `__toString()`</strong>
