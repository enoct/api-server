## onsigbaar

OAuth2 API Server authentication using [Resouce Owner Password Credential Grant](https://tools.ietf.org/html/rfc6749#section-4.3).

## Install

```bash
composer create-project --prefer-dist onsigbaar/onsigbaar projectname
```

## Create the database

Adjust .env with your database configuration/ credential

```bash
# .env

DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=
```

## Migration and seeders

```bash
php artisan migrate

php artisan db:seed
```

## Passport install

```bash
php artisan passport:install
```

Copy personal access and password grant client value into .env

```bash
# .env
PERSONAL_CLIENT_ID=
PERSONAL_CLIENT_SECRET=
PASSWORD_CLIENT_ID=
PASSWORD_CLIENT_SECRET=
```

## Run the dev server

```bash
php artisan serve
```

## Login user

Send post request into endpoint `http://localhost:8000/api/login/` with user credential :

```bash
email: user@api.com
password: user
```

_Example using CURL_

```bash
curl -X POST http://localhost:8000/api/login/ -b cookies.txt -c cookies.txt -D headers.txt -H 'Content-Type: application/json' -d '
    {
        "email": "user@api.com",
        "password": "user"
    }
'
```

## Refresh token

Send post request into endpoint `http://localhost:8000/api/login/refresh`

_Example using CURL_

```bash
curl -X POST http://localhost:8000/api/login/refresh -b cookies.txt -c cookies.txt
```

## Logout user

Send post request into endpoint `http://localhost:8000/api/logout`

_Example using CURL_

```bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>" -X POST http://localhost:8000/api/logout -b cookies.txt -c cookies.txt
```

* _Change <ACCESS_TOKEN> above with access token generated after successful login/ request access token._

## Protected resources endpoint

Implement `auth:api` middleware in any route to make the resources oauth2 protected.

```php
# Example in api/User/Routes/api.php

Route::middleware('auth:api')->get('/user', function (Request $request) {
    return $request->user();
});
```

After successful login send get request to `http://localhost:8000/api/user/` to get authenticated user data.

_Example using CURL_

```bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>" -X GET http://localhost:8000/api/user/
```

## Send all error/ exception to user email

Make sure the application can send email by providing the correct data in `.env`

```
MAIL_DRIVER=
MAIL_HOST=
MAIL_PORT=
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=
```

Set the `LOG_ACTIVITY` and `SIGNAL_EMAIL_SENT` value to `true` in `.env`.
Provide user email data where it will be sent etc.

```
LOG_ACTIVITY=true
SIGNAL_EMAIL_SENT=true
SIGNAL_EMAIL_SENT_TO=
SIGNAL_USE_TABLE=signal_log

MAIL_FROM_ADDRESS=
MAIL_FROM_NAME=
```

In `app/Exceptions/Handler.php` uncomment the line code bellow, from previously :

```php
# app/Exceptions/Handler.php
...
public function report(Exception $exception)
{
    parent::report($exception);

    # Log all error exception into database.
    # $this->fireLog('error', $exception->getMessage(), ['error' => $exception]);
}
...
```

Changed into :

```php
# app/Exceptions/Handler.php
...
public function report(Exception $exception)
{
    parent::report($exception);

    # Log all error exception into database.
    $this->fireLog('error', $exception->getMessage(), ['error' => $exception]);
}
...
```

Global application error exception will be saved into database and sent to user email.
The data saved and emailed will include the user ID, request url, request method, client ip, browser, browser version, user OS etc.

### Related resources

- Oauth2 password grants implementation for this repo located in [here](https://github.com/consigliere/Passerby)
- Log message into database through event subscribe using [signal](https://github.com/consigliere/Signal)
- [Modules](https://github.com/onsigbaar/components)
- [Framework](https://github.com/onsigbaar/framework)
- [Resouce Owner Password Credential Grant](https://tools.ietf.org/html/rfc6749#section-4.3)
- [Which grants](https://rn.netlify.com/blog/oauth2-grants.html)

---

<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel attempts to take the pain out of development by easing common tasks used in the majority of web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, yet powerful, providing tools needed for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of any modern web application framework, making it a breeze to get started learning the framework.

If you're not in the mood to read, [Laracasts](https://laracasts.com) contains over 1100 video tutorials on a range of topics including Laravel, modern PHP, unit testing, JavaScript, and more. Boost the skill level of yourself and your entire team by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for helping fund on-going Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell):

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[British Software Development](https://www.britishsoftware.co)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- [UserInsights](https://userinsights.com)
- [Fragrantica](https://www.fragrantica.com)
- [SOFTonSOFA](https://softonsofa.com/)
- [User10](https://user10.com)
- [Soumettre.fr](https://soumettre.fr/)
- [CodeBrisk](https://codebrisk.com)
- [1Forge](https://1forge.com)
- [TECPRESSO](https://tecpresso.co.jp/)
- [Runtime Converter](http://runtimeconverter.com/)
- [WebL'Agence](https://weblagence.com/)
- [Invoice Ninja](https://www.invoiceninja.com)
- [iMi digital](https://www.imi-digital.de/)
- [Earthlink](https://www.earthlink.ro/)
- [Steadfast Collective](https://steadfastcollective.com/)

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
