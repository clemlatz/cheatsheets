Symfony2
========

## Console

#### `cache:clear`
Clears the cache of dev environnement

    cache:clear --env=prod

#### `generate:bundle`
Generates a bundle

#### `doctrine:database:create`
Creates the database

#### `doctrine:generate:entity`
Generates the php class for a new entity

    doctrine:generate:entity --entity="{Vendor}{Bundle}:{Entity}" --fields="field1:int field2:string(255)"

#### `doctrine:generate:entities {Vendor}{Bundle}:{Entity}`
Generates getters and setters

#### `doctrine:schema:update`
Update databases structure according to entities

    doctrine:schema:update --dump-sql
    doctrine:schema:update --force
    
#### `security:check`
Performs a security check

#### `server:run`
Runs symfony app in the built-in php web-server

## Controller 

### Define

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;

    class ExampleController extends Controller
    {
        public function indexAction($name)
        {
          // ...
        }
    }
    
### Get Request

    public function indexAction(Request $request)
    {
        // ...
    }

### Methods

#### `$this->generateUrl('route', array(params))`
Generates a path from `route` with `params`

#### `$this->redirect('url')`
Returns a Response that redirects to `url`

#### `$this->forward('controller', array(params))`
Includes Response from another `controller` with `params`

#### `$this->renderView('template', array(params))`
Returns a `template` rendered with params

    $content = $this->renderView('AcmeDemoBundle:Demo:index.html.twig');
    return new Response($content);

#### `$this->render('template', array(params))`
Returns a response containing the rendered template

#### `this->get('service')`
Returns a service

#### `this->createNotFoundException()`
Returns a NotFoundHttpException() that triggers a HTTP 404 response

## Request

### Methods

#### `$request->query->get('page')`
Returns a `$_GET` parameter

#### `$request->request->get('email')`
Returns a `$_POST` parameter

#### `$request->isXmlHttpRequest()`
Returns `true` if AJAX request

#### `$request->getPreferredLanguage()`
Returns user preferred language

#### `$session = $request->getSession()`
Returns a session object that can be passed to other controllers between requests

    $session->set('foo', 'bar');
    $session->get('foo', 'default'); // returns bar

#### `$flashBag = $session->getFlashBag()`
Flash messages that last only until the next request

    $flashBag->add(array('foo', 'bar'));

## Response

#### `new Response('Hello world', 200)`
Simple response with content and HTTP 200 (default) code

#### `new Response(json_encode(array()))`
Returns a JSON response

### Methods

#### `$response->headers->set('Content-Type', 'application/json');`
Set a custom HTTP header


## Router

### Route format

    route_name:
        path: /_locale/path/to/{page}.{_format}
        host: domain.com
        defaults: { _controller: Bundle:Controller:method, _format: html }
        requirements:
            page: \d
            _locale: en|fr
            _method: GET
            
### Special parameters

    _controller: set the controller to use with this route
    _locale: set the session locale
    _format: set the response format (json => application/json, etc.)

### Import routes from bundle

    imported_routes:
        resource: "@Bundle/Resources/config/routing.yml"
        prefix: /prefix
        host: "domain.com"
