---
title: Routes
icon: fal fa-road
excerpt: >
    How you can easily affect the routes in your Laravel application.
tags:
    - affects
    - tenant
    - routes
---

# Affects-Routes

1. [Overview](#overview)
2. [Installation](#installation)
3. [Configuration](#configuration)
    1. [Example](#example)

## Overview

**Purpose**

The purpose of this package is to modify the routes that are loaded once a Tenant is identified.

**Use Cases**

- Clear system only routes
- Load tenant-specific routes

**Events & Methods**

- `Tenancy\Affects\Routes\Events\ConfigureRoutes`
  - `flush`
  - `fromFile`


## Introduction
In order to keep your application clean, you might want to separate the routes for tenants into different files.

## Installation

### Using Tenancy/Framework
Install via composer:
```bash
composer require tenancy/affects-routes 
```

### Using Tenancy/Tenancy or with provider discovery disabled
Register the following ServiceProvider: 
  - `Tenancy\Affects\Routes\Provider::class`

## Configuration
After the installation of the package, all you have to do is configure the package in the way you want. Like most affects, this package will fire an event `Tenancy\Affects\Routes\Events\ConfigureRoutes`, which will provide you with some functionality to load and configure your routes:
- `flush()`, this will remove all currently loaded routes.
- `fromFile()`, this will allow you to load routes from a specific file.

### Example
In the example we will register the routes for a Tenant if one is identified.
```php
namespace App\Listeners;

use Tenancy\Affects\Routes\Events\ConfigureRoutes;

class TenantRoutes 
{
    public function handle(ConfigureRoutes $event) 
    {
        if($event->event->tenant)
        {
            $event
                ->flush()
                ->fromFile(
                    ['middleware' => ['web']],
                    base_path('/routes/tenant.php')
                );
        }
    }
}
```
> Note: This example does not set a namespace for your routes. If you're following the default laravel convention you might want to specify a namespace. Alternative is to use tuple notation.
