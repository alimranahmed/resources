Have you seen a static property in a class?
<br><br>
**What is static property? When should we use static property?**<br>
If you are searching the answers of these questions than this article is for you. I have used PHP to explain this
concept. But, I believe anyone from any other programming language, will be able to understand.

I am sure that you have heard the line while learning object oriented programming(OOP) that "a class is a
blueprint or design of objects", right? So,if we have a class of a Car, we can programmatically make an instance of
Car. Same as, if we have an architectural design(blueprint) of house we can build one or multiple houses using that
blueprint. In OOP the architectural design is class and the house(s) we have build is object. Then, what is property? A
property is something that define the state of the created object.
For better understanding, let's see the code below:

```php
class Car{
  public $color;
  
  public function __construct($color) {
    $this->color = $color;
  }
}

$car1 = new Car('Blue');
$car2 = new Car('Black');

echo $car1->color; // Blue
echo $car2->color; // Black
```

This is obvious, right? But why? `color` seems a property of one single class but still `color` of `$car1` and `$car2
` is `Blue` and `Black` respectively. No! `color` is not a property of `Car` class but property of created `objects`.
It is because, this property is not state of the Class, it's state of Object. Let's verify this little bit further:

```php
$car1->color = 'Red';

echo $car1->color; // Red
echo $car2->color; // Black
```

Here, we have changed the color of `$car1` from `Black` to `Red` but this operation has not effect on the `color` of
another car object `$car2`. Now, we are quite sure that the property `color` is associated with Object not with the
Class. In other words, the `color` not the state of architectural design of Car but it is state of an specific
instance.

Now, let's say we are in a situation where we need to count the number of created car(eg. `$totalCarCreated`) using the
`Car` class. Is it a property of a specific car object? Let's think about it...

```php
class Car{
    public $color;
    public $totalCarCreated = 0;
    
    public function __construct($color) {
        $this->color = $color;
        $this->totalCarCreated++;
    }
}

$car1 = new Car('Blue');
$car2 = new Car('Black');

echo $car1->totalCarCreated; // 1
echo $car2->totalCarCreated; // 1
```

We have created 2 car using our class but `$car1` and `$car2` is returning 1!
Is it logical to have the `$totalCarCreated` property in a specific `car`. Is it logical to write `$car1->totalCarCreated`?
I mean should we ask `$car1`, how many car have been created? Should our `Black` car know how many car have been
created using this same design? NO! Here, we are trying to associate a property with an object which is not a state
of that object but an state of the Class itself. Here comes our `static` property. When a property is not associated
with an object but with the class, we define that property as static property or class property.

Let's modify our previous code as below to implement what we have just learned:
```php
class Car{
    public $color;
    public static $totalCarCreated = 0;
    
    public function __construct($color) {
        $this->color = $color;
        self::$totalCarCreated++; // static::totalCarCreated++;
    }
} 

$car1 = new Car('Blue');
$car2 = new Car('Black');

echo Car::$totalCarCreated; // 2
```

**Have you noticed?**<br>
1. The static property `$totalCarCreated` is defined using the keyword `static`.
2. In the constructor we have used `self`(or `static`) keyword to access the static property.
3. `$totalCarCreated` is not property of `$car1` or `$car2` so we accessed it as `Car::$totalCarCreated`

I hope this article explains clearly what is `static` property and when we should use `static` property. My initial
plan was to explain both static property and method in this very same article. But I think it is already a long one.
So, in my next article I will try to explain what is static method and when we should use static method. Till then,
Happy Programming!
