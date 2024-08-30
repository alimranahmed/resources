# How to install PCOV in Laravel Herd to see code coverage of Laravel application

![](https://cdn-images-1.medium.com/max/1760/1*0Dwypw4zVY5UsY2qfZo4HQ.png)

If you use [Laravel Herd](https://herd.laravel.com/) in MacOS and have been trying to see the test coverage using the command `php artisan test --coverage` and getting the follwoing error:

![](https://cdn-images-1.medium.com/max/1760/1*tQQTrbmYHbWidC9MtSNkIw.png)
Error while running artisan test with coverage flag

This is because Herd doesn’t come with [PCOV](https://github.com/krakjoe/pcov) installed as a [pre-installed extension](https://herd.laravel.com/docs/1/getting-started/included-extensions). You can also see if pcov is in the enabled module list using the command:
```
php -m | grep pcov
```

As you have seen, [PCOV](https://github.com/krakjoe/pcov) is not installed in your local machine. Now, following [this article in laravel-news.com](https://laravel-news.com/generate-code-coverage-in-laravel-with-pcov), you can install [PCOV](https://github.com/krakjoe/pcov) using homebrew command as below:
```
brew install shivammathur/extensions/pcov@8.3
```

Don’t forget to replace `8.3` in the with the php version you are using.

Once the [PCOV](https://github.com/krakjoe/pcov) in your local machine, ideally, if it was not Herd, you could have run that php artisan command using the coverage flag. This brew command installed [PCOV](https://github.com/krakjoe/pcov) but not for your Herd-PHP.

So, two things you need to know. One is to find out where the pcov is installed. We need pcov.so file path based on the [Herd documentation for manual extensions](https://herd.laravel.com/docs/1/advanced-usage/additional-extensions). Secondly, we need to put that path as an extension in our php.ini file. Thus we also need to know about location of php.ini file.

#### **1. Where is PCOV installed and where is pcov.so file?**

As we have installed [PCOV](https://github.com/krakjoe/pcov) using the brew, it should be installed somewhere like: `/opt/homebrew/Cellar/pcov@8.3/1.0.11`. Which means there should be file called pcov.so.

Thus, the full path of pcov.so is: `/opt/homebrew/Cellar/pcov@8.3/1.0.11/pcov.so`

#### 2. Where is our active php.ini file?

Finding php.ini file is straight forward in Laravel Herd. Click on the Herd icon from the menu bar and click on the Open Configuration files menu.

![](https://cdn-images-1.medium.com/max/1760/1*WDmQ0btThELI1fEN1V6Whg.png)

Click on Open configurations files to open php.ini file

You can also find the path of the `php.ini` using the command:

```
php --ini
```

The path should be something like:
`/Users/<username>/Library/Application Support/Herd/config/php/83/php.ini`

Open that file in your faviorite editor and add the following line at the end of that file then save and close the file:

```
extension=/opt/homebrew/Cellar/pcov@8.3/1.0.11/pcov.so
```

Then restart Herd using command:
```
herd restart
```

If Herd restarts without any error you should be able to see [PCOV](https://github.com/krakjoe/pcov) enabled using our old friend:

```
php -m | grep pcov
```

Which means Herd in our Local Machine is equiped with [PCOV](https://github.com/krakjoe/pcov)! Now it’s time to see our test coverage!
```
php artisan test --coverage
```

This should run all our test cases and then show the test coverage. Congratulations!

You can also find few other tips regarding generating code coverage from [this article from laravel-news.com](https://laravel-news.com/generate-code-coverage-in-laravel-with-pcov) I mentioned before.

Happy Coding!