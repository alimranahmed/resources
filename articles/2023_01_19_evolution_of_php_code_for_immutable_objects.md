You might already know about the new changes of PHP 8.1 and 8.2. In these versions PHP introduced `readonly` property and `readonly` class.
Now the question is why do we need readonly? What was the problem without readonly? Let's see the evolution of code to write a class for immutable
object.
<br>
<br>
### Before PHP 8.1 
Let's write a class for data transfer object(DTO) in a PHP version before 8.1, I don't have readonly. 
<br>

```php
class UserData {

    public function __construct(
    
        private string $name,
        
        private string $email,
        
        private DateTime $dob
    ) {}
        
    public function getName(): string {
        return $this->name;
    }
    
    public function getEmail(): string {
        return $this->email;
    }
    
    public function getDob(): DateTime {
        return $this->$dob;
    }
    
}
```
<br>

In the above code, `UserData` is a Data Transfer Object(DTO) class which means we only need this class to transfer data from one layer of the application
to another layer. This is why, once we create an object of `UserData` class we don't need to change any property of that object. In other words, the 
object of this class is immutable. To achieve this immutability what have we done? We make the properties of the class `private` and then we wrote getter
method for every property. Now, if someone creates an instance of `UserData` class, they can get the `name` property using the `public` method `getName()`.
But, as the property is `private` and there is no setter method in the class, it's not possible to change the value of any property of that object.
For example: 
<br>

```php
$userData = new UserData("Al Imran Ahmed", "imran@example.com", "2000-01-01");

print($userData->getName()); // POSSIBLE as getName() is a public method 

$userData->name = "Nure Alam"; // NOT POSSIBLE as $name is a private property
```
<br>
<br>

### After PHP 8.1
In PHP 8.1 we got a new feature! We can define a property as `readonly`. By using this `readonly` keyword we can achieve the same immutability as we have
seen above with much less code. Here is how:
<br>

```php
class UserData {

    public function __construct(
    
        public readonly string $name,
        
        public readonly string $email,
        
        public readonly DateTime $dob
        
    ) {}
}
```
<br>
Our new `UserData` class is lot smaller but behaves similar way. As our properties are now public, so we can get them directly without using any method.
On the other hand, we can modify the value of the property because of this newly introduced `readonly` keyword. Isn't it awesome? 
<br>

```php
$userData = new UserData("Al Imran Ahmed", "imran@example.com", "2000-01-01");

print($userData->name); // POSSIBLE as $name is a public property 

$userData->name = "Nure Alam"; // NOT POSSIBLE as $name is a readonly property
```
<br>

There is one difference though, in our previous `UserData` class, as the property was private and only accessible via `public` getter methods. We had
some extra control while returning the data. For instance, we could have format the `dob` before returning is not the case in our 2nd `UserData` class.
<br>
<br>

### After PHP 8.2
Now, there is more improvement in this same feature in 8.2! As you can see, in our case all the properties of `UserData` are readonly. So, we need to write
the keyword `readonly` as may time as our total property. What if we declare the whole class as `readonly`? Yes! this is what introduced in 8.2. You can
get exact same behaviour as the last class using the following code:

```php
class readonly UserData {

    public function __construct(
    
        public string $name,
        
        public string $email,
        
        public DateTime $dob
        
    ) {}
}
```

No more, unnecessary verbose code, no more repeated `readonly` and your object is still immutable? Isn't PHP getting Awesome?

