# Spark — A Classy Web Framework for Rapid Development

Spark is a full stack framework built on top of [Silex][], made for
Rapid Development in the same spirit as Ruby on Rails.

Spark is a framework for people who believe:

* The structure of most applications are nearly the same.
* Convention > Configuration
* Asset management should be shipped out of the box.
* ORMs solve 80% of problems.

Building a framework is of course a _huge_ project, so there isn't
nearly all done. This is what works for now:

* Application generator.
* Asset Pipeline based on [Pipe](http://github.com/CHH/pipe), with
  deployment support.
* Action Controllers with Action and View Helpers built on Traits, and
  support for multiple templating strategies.
* Basic framework for generators.
* Extensible command line utility based on Symfony Console.

[Silex]: http://silex.sensiolabs.org

## Quick Start

### Create an Application

First get the `spark` executable:

    % wget http://chh.github.com/spark/spark.phar

Then invoke the `create` command and grab some coffee while Composer
installs all dependencies:

    % php spark.phar create MyApp

Go into the application directory and start a development server:

    % cd MyApp
    % ./vendor/bin/spark server

Then open <http://localhost:3000> in your browser, you should see "Hello
World" in big letters.

### Controllers

Controllers make your application do things when someone views it in
their browser. Controllers go into the directory `app/controllers/`, 
and are best generated by the `generate controller` command:

    % spark.phar generate controller user

If you open up `app/controllers/MyApp/UserController.php`
you should see this:

```php
<?php

namespace MyApp;

class UserController
{
    function indexAction()
    {
    }
}
```

Each controller consists of actions. Each action is a public method of
the controller class, and ends with "Action".

The HTML goes into a "View". The view's file name gets taken from the
controller and action name. For example for the "index" action in the 
"UserController" the view "user/index.phtml" gets used.

Put this into `app/views/user/index.phtml`:

```html
<h1>Hello World <?= $this->name ?>!</h1>
```

The view has access to each property of the controller class. This
allows you to pass variables from the controller to the view. Also add
the parameter `name` to the method, we will need it later.

```php
<?php

namespace MyApp;

class UserController
{
    function index($name)
    {
        $this->name = $name;
    }
}
```

The last part of getting our controller to do something (remotely)
useful, is to add a route. A Route connects a URI to a controller and
action. Routes are configured in `config/routes.php`.

This will do for now:

```php
$routes->match('/user/{name}', 'user#index');
```

That thing within the curly braces is a variable, which gets extracted
from the URI and then gets assigned the name `name`. We've previously
declared that our action needs an argument `name`, so Spark figures this
out and passes the route variable along to the action.

Last but not least, we assign the controller and action to the route.
This is done with `$controller#$action`. Controller names are converted
from `under_score` to `UnderScore` and then suffixed with `Controller`.
So `user` gets transformed to `UserController`.

If you open <http://localhost:3000/John%20Doe> in your browser you
should see "Hello World John Doe!" in big letters.

## License

Copyright (c) 2012 Christoph Hochstrasser

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
