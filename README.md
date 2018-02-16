# legacy router

Laravel router has deprecated some features, like `Route::controller`, 
and `Route::controllers`. If your code looks like something like the code below then chances are 
they are no longer supported by the latest versions of the Laravel frameworks. To be fore precise
this feature has been deprecated since laravel 5.2.

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
In your laravel app/Http/Kernel.php add/edit your constructor like that code given below
make sure you import the  

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

In your laravel app/Console/Kernel.php add/edit your constructor like that code given below
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