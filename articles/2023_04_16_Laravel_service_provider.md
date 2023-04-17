Laravel service provider is one of the most important parts of laravel but at the same time it's 
misunderstood by many and most of the beginners afraid of it. Naturally, we are afraid of things we don't understand. 
So, I believe one reason of being afraid of Service Provider is due to not having 
a clear understanding of this. In this article I will try to explain what, why and how of 
[Laravel Service Provider](https://laravel.com/docs/10.x/providers) in 
simple terms so that even a beginner can understand. Let's dive in...

### What is service provider?
Let's forget about service provider for now. Imagine in `web.php` route file of our Laravel application, we have a route called
`/test` which will be handled `TestController::__invoker` as below

```php
//in web.php

Route::get('/test', TestController::class);
```

When we make a get request `/test` of our Laravel application, what should happen? What first thing will it do? 
Will it directly execute the `__invoke()` method of the `TestController`? No. Because before even executing this 
`TestController`, Laravel actually does a lot of things for us. Part of which you can say Bootstrapping. As a web developer
and a non-native English speaker, the word *Boostrap* might seem a bit confusing. Like many of you I also came across
the word *Bootstrap* for the first time from the [Bootstrap](https://getbootstrap.com/) frontend framework. But after spending more time
with web development and English language I came to know that, Bootstrap basically means the loading process of any 
system. In Laravel, when we make a request, first thing it does is loading all the necessary classes that are required
for further processing. For instance, we are saying that our `/test` request will be handled by
`TestController::__invoker` of our application. How does our application even know that? Somewhere in the application 
it has to be defined how route should be resolved, right? That somewhere is actually the bootstrapping or loading
process. A part of this bootstrapping process is loading the Service Provider classes as these are also the
necessary classes to proceed next. When we call our Laravel application from Http or Console, it always loads 
the service providers class first. We can see the list of service providers that are being loaded in `providers` key
of the array in `config/app.php` file. When we install fresh a Laravel application, we can only see the providers we will
mostly interact with. Here is the last part of those list of providers in `config/app.php` file:

```php 
'providers' => [
    //...
    
    /*
     * Application Service Providers...
     */
    App\Providers\AppServiceProvider::class,
    App\Providers\AuthServiceProvider::class,
    // App\Providers\BroadcastServiceProvider::class,
    App\Providers\EventServiceProvider::class,
    App\Providers\RouteServiceProvider::class,
]
```
All those service providers are just simple PHP classes, nothing complicated. One special thing about them is, their 
`register()` and `boot()` methods is called when our application is loaded. You will see that many of these classes
don't even have the `register()` method shown, but they have it in their parent class. Now, before going to what
these two methods do, we should see in what sequence those methods are called. Here is the sequence:

```php
App\Providers\AppServiceProvider::register()
App\Providers\AuthServiceProvider::register()
App\Providers\EventServiceProvider::register()
App\Providers\RouteServiceProvider::register()

App\Providers\AppServiceProvider::boot()
App\Providers\AuthServiceProvider::boot()
App\Providers\EventServiceProvider::boot()
App\Providers\RouteServiceProvider::boot()
```
Notice, what is happening, it's called every `register()` methods of service providers in the sequence we have them
in our `config/app.php` file. After finish calling all `register()` methods, it calls every `boot()` method 
of every service provider, in the same sequence! That it, we have some simple PHP classes that have these two methods,
they are being called by Laravel in this sequence. Now, it's up to us what we can and want to do in those methods. 
As simple as that!


### Why service provider?
Now we know we have two methods in service provider's `register()` and `boot()`. All `register()` methods are called
first then the `boot()` methods. Why do we even need them? What are we supposed to do there? Well, technically we
can do anything we want during bootstrapping our application, which means before our middleware, request, controller etc.
are being called. What are the ideal things we do here then? We can register [Model observer](https://laravel.com/docs/10.x/eloquent#observers),
[define global configuration for pagination](https://laravel.com/docs/10.x/pagination#customizing-the-pagination-view),
[register service container of a package](https://laravel.com/docs/10.x/telescope#local-only-installation) etc. We 
can think the `register()` method as the place to register any dependency injection mechanism or service container and
the `boot()` method as object-oriented way of specifying any configuration of our application. Yes, we already 
have configuration PHP files insides `configs/` directory that contains only array of configurations. 
But this `boot()` method gives us the controller of calling a method, instantiating a class etc. This is the 
convention, but as I said technically we can do anything we want here before we want to execute our middleware, 
validation, controller. Let me give you an example. 
Imagine we have a service called **Sms** service. That has two class `RealSms` and `FakeSms` both of them implements an interface
called `SmsInterface`. The `SmsInterface` looks as below:

```php 
<?php

namespace App\Services\Sms;

interface SmsInterface
{
    public function send(string $phone, string $text): string;
}
```

Now, we want to register which Sms service will be used when we resolve the `SmsInterface`. Where do we register it?
Yes, you guessed it right we can register it in the `register()` method of our Service Provider like below in 
`AppServiceProvider`:

```php
//AppServiceProvider.php

public function register(): void
{
    $this->app->bind(SmsInterface::class, RealSms::class);
}
```
Why we did this here? Because we will need this to be resolved in one of our Controllers:

```php
class TestController extends Controller
{
    public function __invoke(Request $request, SmsInterface $sms)
    {
        $sms->send("+xxxxxxxxxxxx", "Test message");
    }
}
```
You see, in the `__invoke()` method we are injecting `SmsInterface` but we need to tell Laravel how this will be 
resolved. We have to do it somewhere before the `TestController` is called. That somewhere doesn't always have
to be a service provider but can be a service provider and conventionally this is the place where register
our service container in Laravel.

### When to make your own service provider?
In the previous section we have seen that we register our `SmsInterface` in the `register()` method of 
`AppServiceProvider` class. What if our Sms service provider need to register many more service containers? What
if our application has many more service like Sms service? You can imagine this `AppServiceProvider` will get messy
pretty quickly. For our Sms service we only have one class to register. At this point, I think it's fine to register in 
`AppServiceProvider`. So, yeah when you see your service demands a new place to register it's all service
container and configuration in one Service Provider, that's the time you should create a new Service Provider for 
that service.

### How to define your own service provider?
Creating or defining a new service provider in Laravel application is pretty simple. Laravel even comes with 
a command to create a Service Provider. We can create a Service Provider called `SmsServiceProvider` using 
the command below:

```shell
php artisan make:provider SmsServiceProvider
```

After executing this command, we will se that we have a new class file in `app/Providers` directory called `SmsServiceProvider.php` and that class
contains two friendly methods `register()` and `boot()`. We can move our `$this->app->bind(SmsInterface::class, RealSms::class);`
and any other service container registration related to Sms service to the `register()` method of `SmsServiceProvider` as below:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class SmsServiceProvider extends ServiceProvider
{
    /**
     * Register services.
     */
    public function register(): void
    {
        $this->app->bind(SmsInterface::class, RealSms::class);
        // Or any other class binding related to Sms service!
    }

    /**
     * Bootstrap services.
     */
    public function boot(): void
    {
        //
    }
}
```

That's it about Laravel's service provider. I hope now you understand clearly what, why and how to use Laravel's service provider. Yeah, it's just 
a class that load while bootstrapping our Laravel application.

