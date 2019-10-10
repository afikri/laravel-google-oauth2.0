
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

4. Setup client id, client secret and callback url in the cofig/services.php
```
'google' => [
        'client_id'     => env('GOOGLE_CLIENT_ID'),
        'client_secret' => env('GOOGLE_CLIENT_SECRET'),
        'redirect'      => env('GOOGLE_REDIRECT'),
    ],
 ```
where the values are placed in the .env file <br>
5. Update user table by **adding google id** by issuing the command
```
php artisan make:migration add_google_id_column
```
