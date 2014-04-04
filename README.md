Soflomo\Prototype
===
`Soflomo\Prototype` is a small module that helps developers to start creating prototypes of their application. The module aims to create various pages fast and only worry about urls and view templates. Because of its simple goals, frontend developers not experienced with php can create mockups of their designs as well.

Simply said, it allows you to create a mapping between urls and view templates, so websites with a couple of pages are extremely easy to create. Every page has a name, so it is possible to link between pages with the Zend Framework 2 `url()` view helper.

Installation
---
`Soflomo\Prototype` is available through composer. Add "soflomo/prototype" to your composer.json list. During development of `Soflomo\Prototype`, you can specify the latest available version:

```
"soflomo/prototype": "dev-master"
```

Enable the module in your `config/application.config.php` file. Add an entry `Soflomo\Prototype` to the list of enabled modules.

Usage
---
The recommended method is to create a module to hold all the mockup view scripts and the configuration of pages. For example, you call this module `Mockups`.

### Create mockup module
Create a directory `Mockup` inside the `module` directory. Create a file `Module.php` inside this `module/Mockup` directory with the following contents:

```
<?php

namespace Mockup;

class Module
{
    public function getConfig()
    {
        return include __DIR__ . '/config/module.config.php';
    }
}
```

Then create a folder `module/Mockup/config`. Create a file `module.config.php` inside this `module/Mockup/config` directory with the following contents:

```
<?php
return array(
    'soflomo_prototype' => array(
    ),

    'view_manager' => array(
        'template_path_stack' => array(
            __DIR__ . '/../view',
        ),
    ),
);
```

As a last step, create the directory `view` inside `module/Mockup`. This should be enough to get you started for this module.

### Enable Mockup module
Locate the file `config/application.config.php` and put the `Mockup` module at the end of the list of modules to be enabled. NB. Make sure you also enabled the `Soflomo\Prototype` module, as described above under "Installation".

### Create the pages
The last step is your mockup page configuration. Get back to the `module.config.php` file in the `Mockup` module. Inside the `soflomo_prototype` array, you create (as an example) two pages:

```
'homepage' => array(
    'route'    => '/',
    'template' => 'mockup/homepage'
),
'hello' => array(
    'route'    => '/hello-world',
    'template' => 'mockup/hello-world'
),
```

Now you have two pages, one called `homepage` which is loaded when you load the application `/` (e.g. `http://mysite.localhost/`) and the other is called `hello` and is loaded when you load `/hello-world` (e.g. `http://mysite.localhost/hello-world`).

For both pages you need to create a template to be loaded. Inside the `module/Mockup/view` directory you create a new one `mockup` and inside it, you create a file `homepage.phtml` and another `hello-world.phtml`. Those will be rendered for respectively the `homepage` and the `hello` page.

If you need to create a link from one page to another, just use the `url()` view helper. An example to link to the `hello` page on the `homepage`, put this in your `homepage.phtml`:

```
<a href="<?php echo $this->url('hello'); ?>">Click me</a>
```

This will render a link like this:

```
<a href="/hello-world">Click me</a>
```

### Parametrized routes

`Soflomo\Prototype` supports route parameters with the segment type route in Zend Framework 2. This means you could have a route like `/foo/bar/:id`. By default, the module only uses literal routes to match the exact url. This is faster, but less flexible. You can specify to have the segment type by an additional `type` key:

```
'homepage' => array(
    'type'     => 'segment',
    'route'    => '/foo/bar/:id',
    'template' => 'mockup/foo-bar'
),
```

These routes types are extremely useful when dealing with existing applications. If you have an application where you deal with parametrized routes, you want to have a parametrized route for your mockup page as well. This way, you could always navigate back to your real application when a user vists the mocked page.

Why?
---
You might ask why we built this module and did not use a micro framework like Silex. Or, why we do not start in Zend Framework 2 directly.

To answer the first, micro frameworks are hard to combine with existing Zend Framework 2 applications. We (at Soflomo) need a way to create prototypes also for existing applications. If we prototype a new design for a new module, the existing site with existing designs and existing modules should be kept intact.

However, "normal" Zend Framework 2 configuration is quite some work if you only want to connect routes with view scripts. Frontend developers can easily copy/paste above code for a new module and they are up and running. It simply costs too much time and effort to use the existing nested routes, create controllers, actions, make them loadable via configuration, fetch the view template from the route match options and return a correctly configured view model. For fast protyping, you now just add four lines for a page name, route and view script and it Just Works (tm).

Development
---
`Soflomo\Prototype` is intended to create prototypes really fast, but this module is by no means intended for production usage. It is (at this moment) not tested, not stable and there are no guarantees it is bug-free. Use at your own risk!

If you want to join development or found any bug, feel free to fork this project and make your adjustments. Create a pull request to ask for the changes to be merged back in `Soflomo\Prototype`. If you have any questions about this module, feel free to contact us at jurian@juriansluiman.nl.
