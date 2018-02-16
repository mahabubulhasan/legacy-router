# legacy router

Laravel router has been deprecated some features, like `Route::controller`, 
and `Route::controllers`. If your code looks something like the code below then chances are 
they are no longer supported by the latest versions of the Laravel. To be more precise
these feature has been deprecated since laravel 5.2.

This library brings back those legacy route features.

```php
Route::controllers([
    'user'  => 'UserController',        
    'asset/report' => 'Asset\AssetReportController',
    'asset' => 'Asset\AssetController'        
]);
``` 
or
```php
Route::controller('/user', 'UserController');
```

### install

```
composer require uzzal/legacy-router
```

### configure
In your laravel `app/Http/Kernel.php` add/edit your constructor like that code given below

```php
<?php
namespace App\Http;
...
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Foundation\Http\Kernel as HttpKernel;
use Uzzal\LegacyRouter\LegacyRouter;
...
class Kernel extends HttpKernel
{

    ...
    
    public function __construct(Application $app)
    {
        $router = new LegacyRouter($app['events'], $app);
        $app->singleton('router', function($app) use ($router){
            return $router;
        });
        parent::__construct($app, $router);
    }

    ...
}
```

In your laravel `app/Console/Kernel.php` add/edit your constructor like the code given below
make sure you import the  

```php
<?php
namespace App\Console;
...
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;
use Uzzal\LegacyRouter\LegacyRouter;
use Illuminate\Contracts\Foundation\Application;
...
class Kernel extends ConsoleKernel
{
    ...    
    public function __construct(Application $app)
    {
        $router = new LegacyRouter($app['events'], $app);
        $app->singleton('router', function($app) use ($router){
            return $router;
        });
        parent::__construct($app, $app['events']);
    }
    ...    
}
```

That's it. You're done!

## extra
If you are one of those rare people who don't know how this router worked this part if your you.
Imagine you have a controller in your `app/Http/Controllers` directory like this
```php
class TestController extends Controller
{

    public function getIndex(){
        return 'this is a get request';
    }

    public function postStore(){
        return 'this is a post request';
    }
}
```
then you can call it using the legacy router like this
```php
Route::controller('/test', 'TestController');
```
This will automatically mapped with your controller. For more on this take a look at this link
[implicit-controllers](https://laravel.com/docs/5.0/controllers#implicit-controllers)