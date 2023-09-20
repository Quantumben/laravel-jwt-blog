How To Setup JWT (JSON Web Token) in Laravel?

You need to follow few steps to do a successful installation of JWT package in laravel

Step #1
Run composer command,

# composer require tymon/jwt-auth

Step #2

Open app.php file from /config folder.

Search for “providers“, add this line of code into it.

# 'providers' => ServiceProvider::defaultProviders()->merge([
#   //...
#    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
# ])->toArray(),

Search for “aliases“, add these lines of code into it.

# 'aliases' => Facade::defaultAliases()->merge([
#  //...
#   'Jwt' => Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
#  'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
#  'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
# ])->toArray(),

Step #3
Publish jwt.php (jwt settings) file. Run this command to terminal,

# php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"

It will copy a file jwt.php inside /config folder.


Step #4
Run migration
# php artisan migrate

It will migrate all pending migrations of application.

Step #5
Generate jwt secret token value,

# php artisan jwt:secret

It updates .env file with jwt secret key.

Step #6
Open auth.php file from /config folder.

Search for “guards“. Add these lines of code into it,

# 'guards' => [
#   //...
#    'api' => [
#       'driver' => 'jwt',
#       'provider' => 'users',
#   ],
# ],

Step #7
Update User.php (User model class file).


Successfully, you have setup JWT auth package into application.

Now, you have a middleware which you can use to protect api routes i.e “auth:api”

API Controller Settings
Run this command to create API controller class,

# php artisan make:controller Api/ApiController

It will create a file named ApiController.php inside /app/Http/Controllers folder.

ApiController class contains the api methods for,

Register
Login
Profile
Refresh Token
Logout
Setup API Routes

Open api.php file from /routes folder. Add these routes into it,


use App\Http\Controllers\Api\ApiController;

Route::post("register", [ApiController::class, "register"]);
Route::post("login", [ApiController::class, "login"]);

Route::group([
    "middleware" => ["auth:api"]
], function(){

    Route::get("profile", [ApiController::class, "profile"]);
    Route::get("refresh", [ApiController::class, "refreshToken"]);
    Route::get("logout", [ApiController::class, "logout"]);
});

Application Testing
Open project to terminal and type the command to start development server

# php artisan serve

Register API
URL – http://127.0.0.1:8000/api/register

Method – POST

Header –

Accept:application/json
Form data –

{
   "name": "Sanjay Kumar",
   "email": "sanjay.example@gmail.com",
   "password": 123456
   "password_confirmation": 123456
}
Screenshot –


Read More: How To Handle Exception in Laravel 10 Example Tutorial

Login API
URL – http://127.0.0.1:8000/api/login

Method – POST

Header –

Accept:application/json
Form data –

{
   "email": "sanjay.example@gmail.com",
   "password": 123456
}
Screenshot –


Profile API
URL – http://127.0.0.1:8000/api/profile

Method – GET

Header –

Accept:application/json
Authorization:Bearer <token>
Screenshot –


Refresh Token API
URL – http://127.0.0.1:8000/api/refresh

Method – GET

Header –

Accept:application/json
Authorization:Bearer <token>
Screenshot –


Logout API
URL – http://127.0.0.1:8000/api/logout

Method – GET

Header –

Accept:application/json
Authorization:Bearer <token>
That’s it.

