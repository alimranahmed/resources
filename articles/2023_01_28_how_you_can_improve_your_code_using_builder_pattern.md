Design Patterns can be divided into 3 categories. Those categories are: Creational, Structural and 
Behavioral design patterns. In this article, I am going to talk about a creational design pattern called **Builder**
pattern. This is categorised as creational design pattern because it is associated with the mechanism of creating 
objects. At first let's see a class without using builder pattern, then we will se how we can improve that 
class using builder pattern. 

```php
class User {
    public function __construct(
        protected string $name, 
        protected string $email, 
        protected DateTime $dob, 
        protected ?Gender $gender = null, 
        protected ?string $password
    ) {
    
    }
    
    public function register(): bool
    {
        // Save all user data
    }
    
    public function login(): bool
    {
        // Login using $this->email and $this->password
    }
}
```

In this `User` class, if we want to call the `register()` method we have to create an object of `User` class using 
the constructor. In the constructor we have five parameters among three of them are required and two of them are 
optional. 

```php
$user = new User(
    name: "Al Imran Ahmed",
    email: "imran@example.com",
    dob: new DateTime('1992-01-01'),
    gender: Gender::MALE,
    password: secret
); 

$user->register();
```

## What ar the problems with above code?
1. The constructor of `User` class is already taking too many arguments. What if we need more(10 or 20) properties?
2. `login()` and `register()` method might need different set's but while creating the `User` class using its
constructor, we need to pass all those arguments which can be irrelevant for different methods.

Let's say we want to call the `login()` method, but for login we only need `$email` and `$password`
of the user. To call the `login()` method we still create the `User` object as we did for `register()` method. But 
for login functionality we need different set of data than we needed for register functionality. 

```php
$user = new User(
    name: "Al Imran Ahmed",
    email: "imran@example.com",
    dob: new DateTime('1992-01-01'),
    gender: null,
    password: secret
); 

$user->login();
```

## How can we solve those problems?
I know we can solve this problem using different approaches like separating these method in different classes. 
But, to understand Builder pattern let's solve this issue in a different way. Let's rewrite the `User` class as 
below: 

```php
class User {
    protected string $name;
    protected string $email;
    protected DateTime $dob;
    protected Gender $gender;
    protected string $password;
    
    public function setName(string $name): self
    {
        $this->name = $name;
        return $this;
    }
    
    public function setEmail(string $email): self
    {
        $this->email = $this->email;
        return $this;
    }
    
    public function setDob(DateTime $dob): self
    {
        $this->dob = $dob;
        return $this;
    }
    
    public function setGender(Gender $gender): self
    {
        $this->gender = $gender;
        return $this;
    }
    
    public function setPassword(string $password): self
    {
        $this->password = $password;
        return $this;
    }
    
    public function register(): bool
    {
        // Save all user data
    }
    
    public function login(): bool
    {
        // Login using $this->email and $this->password
    }
}
```

Using this new `User` clas we can create `register()` method as below: 

```php
$user = new User();
$user->setName("Al Imran Ahmed")
    ->setEmail("imran@example.com")
    ->setDob(new DateTime("1992-01-01"))
    ->setGender(Gender::MALE)
    ->register();
```

In this above code, you can see we can now build the `$user` object using all those `setter` methods as we returned
`$this` from those methods. As we are returning `$this` object from those methods, we can chain other methods too. 
After building the object finally, we can simply call the `register()` method. Now, we call the `login()` method
as below:

```php
$user = new User();
$user->setEmail("imran@example.com")
    ->setPassword('secret')
    ->login();
```

Have you seen? Now we don't have to pass all other parameters that we don't need for login functionalities.

Does this way of building object look familiar to you?  You might have see this pattern already in Frameworks like
[Laravel](https://laravel.com/), [Symfony](https://symfony.com/) etc. Here is an example code of Laravel Query Builder.

```php
$users = User::query()
            ->where('name', $request->name)
            ->where('email', $request->email)
            ->where('dob', $request->dob)
            ->first();
```
Here, we are actually building Laravel `QueryBuilder` object using the `where()` method, and finally we are calling `first()` method to execute the query 
and return the result. 

### Disadvantages
No design pattern is solution to all problems. It's a double edge sword. It can sometimes create problems too. 
For example in our case, after implementing the `User` class using builder pattern we can build the object what 
if user called the`register()` method without setting the required data like `name` or `email`? Have me made 
this class too flexible? As we can pass any number of properties, are we encouraging to mindlessly add many other methods to 
this `User`class? Won't it be a violation of [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle).
Does it even make sense to keep this two methods `register()` and `login` in same `User` class?

### Final Thoughts
Before using Builder pattern or any other pattern you should know clearly what problems you are solving and 
what problems you are creating. As you already know, PHP 8.0.0 already introduced [named arguments](https://www.php.net/manual/en/functions.arguments.php#functions.named-arguments)
liked many other languages. You have the option to pass a particular param to constructor or a method instead
of passing them all. As a result, now in many cases you might use named arguments features instead of using Builder pattern. But, Builder pattern
still have a lot of use cases. As you  have seen the example Laravel Query Builder.
