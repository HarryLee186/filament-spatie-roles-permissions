# Description

[![Latest Version on Packagist](https://img.shields.io/packagist/v/althinect/filament-spatie-roles-permissions.svg?style=flat-square)](https://packagist.org/packages/althinect/filament-spatie-roles-permissions)
[![Total Downloads](https://img.shields.io/packagist/dt/althinect/filament-spatie-roles-permissions.svg?style=flat-square)](https://packagist.org/packages/althinect/filament-spatie-roles-permissions)
![GitHub Actions](https://github.com/althinect/filament-spatie-roles-permissions/actions/workflows/main.yml/badge.svg)

This plugin is built on top of [Spatie's Permission](https://spatie.be/docs/laravel-permission/v5/introduction) package. 

Provides Resources for Roles and Permissions

Permission and Policy generations
- Check the ``config/filament-spatie-roles-permissions-config.php``

Supports permissions for teams
- Make sure the ``teams`` attribute in the ``app/permission.php`` file is set to ``true``

## Updating

After performing a ```composer update```, run

```php
php artisan vendor:publish --tag="filament-spatie-roles-permissions-config"
```

## Installation

You can install the package via composer:

```bash
composer require althinect/filament-spatie-roles-permissions
```

Since the package depends on [Spatie's Permission](https://spatie.be/docs/laravel-permission/v5/introduction) package. You have to publish the migrations by running:
```bash
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

Now you should add any other configurations needed for the Spatie-Permission package.

You can publish the config file of the package with:
```bash
php artisan vendor:publish --tag="filament-spatie-roles-permissions-config"
```

## Usage

### Form

You can add the following to your *form* method in your UserResource 

```php
return $form->schema([
    Select::make('roles')->multipe()->relationship('roles', 'name')
])
```

In addition to the field added to the **UserResource**. There will be 2 Resources published under *Roles and Permissions*. You can use these resources manage roles and permissions.

### Generate Permissions

You can generate Permissions by running
```bash
php artisan permissions:sync
```

This will not delete any existing permissions. However, if you want to delete all existing permissions, run

```bash
php artisan permissions:sync -C|--clean
```

#### Example: 
If you have a **Post** model, it will generate the following permissions
```
post.view-any
post.view
post.create
post.update
post.delete
post.restore
post.force-delete
```

### Generating Policies
Policies will be generated with the respective permission

```bash
php artisan permissions:sync -P|--policies
```

### Ignoring prompts
You can ignore any prompts by add the flag ``-Y`` or ``--yes-to-all`` 

***Recommended only for new projects as it will replace Policy files***

```bash
php artisan permissions:sync -CPY
```

### Adding a Super Admin

* Create a Role with the name `Super Admin` and assign the role to a User
* Add the following trait to the User Model

```php
use Althinect\FilamentSpatieRolesPermissions\Concerns\HasSuperAdmin;

class User extends Authenticatable{

...
use HasSuperAdmin;
```

* In the `boot` method of the `AuthServiceProvider` add the following

```php
Gate::before(function (User $user, string $ability) {
    return $user->isSuperAdmin();     
});
```

### Configurations

In the **filament-spatie-roles-permissions.php** config file, you can customize the permission generation

## Security

If you discover any security related issues, please create an issue.

## Credits

-   [Tharinda Rodrigo](https://github.com/UdamLiyanage/)
-   [Udam Liyanage](https://github.com/UdamLiyanage/)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## Laravel Package Boilerplate

This package was generated using the [Laravel Package Boilerplate](https://laravelpackageboilerplate.com).
