In the last [article](http://imranic.me/article/10/what-is-static-property?-when-should-we-use-static-property?) we
tried to explore what is static property. We have seen why and where we should use them. As promised in that
article, we are going to discover about static method in this article. Let's begin!

## Non-static Method:
Let's consider the following example that demonstrate a simple `Math` class that can perform addition and subtraction
only, so we have two methods `add()` and `subtract(). 
 
 ```php
class Math
{
    protected $number1;

    protected $number2;

    public function add()
    {
        return $this->number1 + $this->number1; 
    }
    
    public function subtract()
    {
        return $this->number1 - $this->number2;
    }
}
```

We have just created world's most innovative way of adding or subtracting! We don't need a calculator app or
a physical calculator anymore to add or subtract! Well, I am joking, it's not totally lie though. Now, how can we add 
or subtract using the Math class you have designed above?

```php
$math1 = new Math(10, 7);
echo $math1->add(); // 17
echo $math1->subtract(); // 3

// Two perform same operation in different set of numbers
$math2 = new Math(15, 5);
echo $math2->add(); // 20
echo $math2->subtract(); // 10
``` 
If we can look carefully, we will notice, there are several issues in this code block. 
One of them is, we are instantiating the `Math` class whenever we need to perform a new addition or subtraction
operation. This is because the only way to pass numbers in this class is through the constructor. We can solve this 
problem using setter method or making these two properties `$number2` and `$number2` public. But then, we would need
to assign the numbers using setter method or assignment operator. Thus we may not need to instantiate the `Math` class 
for each operator but still we need to use two extra lines to set two numbers which is not convenient.

**What would be more convenient?**
How about `$math->add(10, 7)`? Won't it be more natural way of performing an addition operation?
To achieve this capability, let's modify the `Math` class as below:

```php
class Math
{
    public function add($number1, $number2)
    {
        return $number1 + $number1; 
    }
    
    public function subtract($number1, $number2)
    {
        return $number1 - $number2;
    }
}
```

Now, we can use as below:

```php
<?php
$math1 = new Math();
echo $math1->add(10, 7); // 17
echo $math1->subtract(20, 15); // 5
``` 

Isn't it better now? As our newly designed `Math` class we don't need to instantiate the class whenever we need to
perform any operation. 

## Static Method
But wait, we are still creating an object to perform any operation, right? Is it necessary? 
`add()` or `subtract()` method is not using any property of the object. As we already know from the 
[previous article](http://imranic.me/article/10/what-is-static-property?-when-should-we-use-static-property?)
that each object has it's own property which is separate from other object even from the same class. These methods
have become independent to the property of any specific object. These methods are no more behaviour of an specific
object but behaviour of the class. Here comes **Static method**. Static method is not a method of an object thus they
are not dependent on any property of object but they are self dependent. We can call static method as class method. So, 
let's modify our `Math` class with static method.

```php
class Math
{
    public static function add($number1, $number2)
    {
        return $number1 + $number1; 
    }
    
    public static function subtract($number1, $number2)
    {
        return $number1 - $number2;
    }
}
```

To, use this class now, let's see the example below:

```php
echo Math::add(10, 7); // 17
echo Math::subtract(20, 15); // 5
```    

Very simple, isn't it? As the method `add()` and `subtract()` has no dependency with the object or it's any property, 
why should we bother to create an object by instantiating the class? In PHP we need to use double colon(`::`) to call
a static method where other programing languages have there own way of calling static method. However, the concept
of static method is same. 

## Why can't we use `$this` keyword inside static method?
So far, I think we are clear when and why we should use static method. Now, have you noticed while writing code
inside a static method we cannot use the keyword `$this`? I was puzzled first time when I faced this constrain. By
the time I was clear with the concept of `static`, it became obvious to me that `$this` should not be allowed
inside a static method. Because, same as static property static method is not a method of an object but method of the
class where `$this` keyword represent the object. We call a static method without even creating any object, so using
the reference of object(`$this`) inside a static method doesn't make any sense. 

## Finally
Interesting thing is, we already know that we can call a static method even without creating an object. Doesn't it mean
that static method even don't have relation with a class? As, we all know that class is the blueprint of object! 
Well, I think this is a valid question but it is not ideal in OOP to create method which doesn't belongs to any class. 
Though languages like PHP allow us to define function which is not belongs to any class where languages like Java don't
allow as far as I know. I think it's better to define a static method inside a class instead of defining
parent-less(or class less) function. Keeping aside this argument, as a general rule of thumb, if we don't use any 
non-static property inside a method we should consider to it a static method. 
