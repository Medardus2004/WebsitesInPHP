# Symfony 5
 
Symfony 5.4.12 

Php 7.2.34

Apache 2.4.52

Composer 2.2.13

https://symfony.com/doc/5.4/setup.html

## Getting Started

### Setup/Installation

Create a tradional web application

$> symfony new my_project_directory --version=5.4 --webapp

Create a microservice, console app or API

$> symfony new my_project_directory --version=5.4

Display Information about the project

$> php bin/console about

If not using Apache2 or Nginx, start the local web server

$> symfony server:start

and go to localhost:8000

Symfony Flex is a Composer plugin that is installed by default when creating a new Symfony application and which automates the most common tasks of Symfony applications.

$> Install a package, for example logger

composer require logger

Lots of Symfony packages/bundles define "recipes", which are a set of automated instructions to install and enable packages into Symfony applications.

A common practice when developing Symfony applications is to install packages (Symfony calls them bundles) that provide ready-to-use features. 

Sometimes a single feature requires installing several packages and bundles. Instead of installing them individually, Symfony provides packs, which are Composer metapackages that include several dependencies.

Add debugging features in your application

$> composer require --dev debug

Check Security Vulnerabilities

$> symfony check:security

In continuous integration services you can check security vulnerabilities using a different stand-alone project called Local PHP Security Checker. https://github.com/fabpot/local-php-security-checker

### Create pages

A *route* is the URL (e.g. /about) to your page and points to a controller;

A *controller* is the PHP function you write that builds the page. You take the incoming request information and use it to create a Symfony Response object, which can hold HTML content, a JSON string or even a binary file like an image or PDF.

Create File src/Controller/LuckyController.php and add route in config/router.yaml

Using apache, it is necessary on install Apache Symfony Pack
 
 $> composer require symfony/apache-pack

 Instead of defining your route in YAML, Symfony also allows you to use annotation or attribute routes. Attributes are built-in in PHP starting from PHP 8. In earlier PHP versions you can use annotations. To do this, install the annotations package:

 $> composer require annotations

 now add your route directly above the controller:

 Your project already has a powerful debugging tool inside that lists all comands 

 $> php bin/console

Get a list of all of the routes in your system:

$> php bin/console debug:router

Use twig to render a template

$> composer require twig

extend Abstract Controller and use the render() method to render a template.

Create a new templates/lucky directory with a new number.html.twig file inside:

#### Most Important Folder

config/

Contains... configuration!. You will configure routes, services and packages.

src/

All your PHP code lives here.

templates/

All your Twig templates live here.

bin/

The famous bin/console file lives here (and other, less important executable files).

var/

This is where automatically-created files are stored, like cache files (var/cache/) and logs (var/log/).

vendor/

Third-party (i.e. "vendor") libraries live here! These are downloaded via the Composer package manager.

public/

This is the document root for your project: you put any publicly accessible files here.

### Routing / Generating URLs

When your application receives a request, it calls a controller action to generate the response.

By default, routes match any HTTP verb (GET, POST, PUT, etc.) Use the methods option to restrict the verbs each route should respond to:

@Route("/api/posts/{id}", methods={"GET","HEAD"})

Use the condition option if you need some route to match based on some arbitrary matching logic:

condition="context.getMethod() in ['GET', 'HEAD'] and request.headers.get('User-Agent') matches '/firefox/i'"

Pass the name (or part of the name) of some route to this argument to print the route details:

$> php bin/console debug:router app_lucky_number

The other command is called router:match and it shows which route will match the given URL.

$> php bin/console router:match /lucky/number/8

The previous examples defined routes where the URL never changes (e.g. /blog). However, it's common to define routes where some parts are variable. For example, the URL to display some blog post will probably include the title or slug (e.g. /blog/my-first-post or /blog/all-about-symfony).

In Symfony routes, variable parts are wrapped in { ... } and they must have a unique name. 

The requirements option defines the PHP regular expressions that route parameters must match for the entire route to match. In this example, \d+ is a regular expression that matches a digit of any length.

If you prefer, requirements can be inlined in each parameter using the syntax {parameter_name<requirements>}.

A common routing need is to convert the value stored in some parameter (e.g. an integer acting as the user ID) into another value (e.g. the object that represents the user). This feature is called a "param converter".

To add support for "param converters" we need SensioFrameworkExtraBundle:

$> composer require sensio/framework-extra-bundle

### Controller

A controller is a PHP function you create that reads information from the Request object and creates and returns a Response object. The response could be an HTML page, JSON, XML, a file download, a redirect, a 404 error or anything else. The controller runs whatever arbitrary logic your application needs to render the content of a page.

The generateUrl() method is just a helper method that generates the URL for a given route:

$url = $this->generateUrl('app_lucky_number', ['max' => 10]);

If you want to redirect the user to another page, use the redirectToRoute() and redirect() methods:

return $this->redirectToRoute('homepage');

If you're serving HTML, you'll want to render a template. The render() method renders a template and puts that content into a Response object for you:

return $this->render('lucky/number.html.twig', ['number' => $number]);

Symfony comes packed with a lot of useful classes and functionalities, called services. These are used for rendering templates, sending emails, querying the database and any other "work" you can think of.

To save time, you can install Symfony Maker and tell Symfony to generate a new controller class:

$> php bin/console make:controller BrandNewController

If you want to generate an entire CRUD from a Doctrine entity, use:

$> php bin/console make:crud Product

When things are not found, you should return a 404 response. To do this, throw a special type of exception:

throw $this->createNotFoundException('The product does not exist');

What if you need to read query parameters, grab a request header or get access to an uploaded file? That information is stored in Symfony's Request object. To access it in your controller, add it as an argument and type-hint it with the Request class:

$page = $request->query->get('page', 1);

Symfony provides a session object that you can use to store information about the user between requests. Session is enabled by default, but will only be started if you read or write from it.

    // stores an attribute for reuse during a later user request
    $session->set('foo', 'bar');

    // gets the attribute set by another controller in another request
    $foobar = $session->get('foobar');

    // uses a default value if the attribute doesn't exist
    $filters = $session->get('filters', []);
