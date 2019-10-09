
### Integration steps
1. Install Socialite <br>
`composer require laravel/socialite`
2. Add providers and aliases in config file, where located in **config/app.php**
```
'providers' => [

    ....

    Laravel\Socialite\SocialiteServiceProvider::class,

],

'aliases' => [

    ....

    'Socialite' => Laravel\Socialite\Facades\Socialite::class,

],
```
3. Create google app 
[Follow this link](https://console.developers.google.com)
