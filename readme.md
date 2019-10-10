
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
5. Update user table by **adding google id** by issuing the command<br>
`
php artisan make:migration add_google_id_column
`
<br>then add lines of code below
```
Schema::table('users', function ($table) {
   $table->string('google_id')->nullable();
});
```
<br>

6. Update user model
```
protected $fillable = [
   'name', 'email', 'password', 'google_id'
];
```
7. Create routes in the routes/web.php
```
Route::get('redirect/{driver}', 'Auth\LoginController@redirectToProvider')->name('login.provider');
Route::get('{driver}/callback', 'Auth\LoginController@handleProviderCallback')->name('login.callback');
```
8. On LoginController add
```
public function redirectToProvider()
{
   return Socialite::driver('google')->redirect();
}
```
and
```
public function handleProviderCallback()
{
   try{
      $user = Socialite::driver('google')->user();
      $finduser = User::where('google_id', $user->id)->first();
      if($finduser){
         Auth::login($finduser);
         return return redirect('/home');
      }else{
         $newUser = User::create([
         'name' => $user->name,
         'email' => $user->email,
         'google_id'=> $user->id
      ]);
      Auth::login($newUser);
         return redirect()->back();
      }
  }catch (Exception $e) {
     return redirect('auth/google');
  }
}
```
