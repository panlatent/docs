Dispatching Controllers
=======================
:doc:`Phalcon\\Mvc\\Dispatcher <../api/Phalcon_Mvc_Dispatcher>` is the component responsible for instantiating controllers and executing the required actions
on them in an MVC application. Understanding its operation and capabilities helps us get more out of the services provided by the framework.

The Dispatch Loop
-----------------
This is an important process that has much to do with the MVC flow itself, especially with the controller part. The work occurs within the controller
dispatcher. The controller files are read, loaded, and instantiated. Then the required actions are executed. If an action forwards the flow to another
controller/action, the controller dispatcher starts again. To better illustrate this, the following example shows approximately the process performed
within :doc:`Phalcon\\Mvc\\Dispatcher <../api/Phalcon_Mvc_Dispatcher>`:

.. code-block:: php

    <?php

    //Dispatch loop
    while (!$finished) {

        $finished = true;

        $controllerClass = $controllerName . "Controller";

        //Instantiating the controller class via autoloaders
        $controller = new $controllerClass();

        // Execute the action
        call_user_func_array(array($controller, $actionName . "Action"), $params);

        // '$finished' should be reloaded to check if the flow
        // was forwarded to another controller
        $finished = true;
    }

The code above lacks validations, filters and additional checks, but it demonstrates the normal flow of operation in the dispatcher.

Dispatch Loop Events
^^^^^^^^^^^^^^^^^^^^
:doc:`Phalcon\\Mvc\\Dispatcher <../api/Phalcon_Mvc_Dispatcher>` is able to send events to an :doc:`EventsManager <events>` if it is present. Events are triggered using the type "dispatch". Some events when returning boolean false could stop the active operation. The following events are supported:

+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| Event Name           | Triggered                                                                                                                                                                                                      | Can stop operation? | ?????????????????????? ??????       |
+======================+================================================================================================================================================================================================================+=====================+=======================+
| beforeDispatchLoop   | Triggered before entering in the dispatch loop. At this point the dispatcher don't know if the controller or the actions to be executed exist. The Dispatcher only knows the information passed by the Router. | Yes                 | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| beforeDispatch       | Triggered after entering in the dispatch loop. At this point the dispatcher don't know if the controller or the actions to be executed exist. The Dispatcher only knows the information passed by the Router.  | Yes                 | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| beforeExecuteRoute   | Triggered before executing the controller/action method. At this point the dispatcher has been initialized the controller and know if the action exist.                                                        | Yes                 | Listeners/Controllers |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| initialize           | Allow to globally initialize the controller in the request                                                                                                                                                     | No                  | Controllers           |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| afterExecuteRoute    | Triggered after executing the controller/action method. As operation cannot be stopped, only use this event to make clean up after execute the action                                                          | No                  | Listeners/Controllers |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| beforeNotFoundAction | Triggered when the action was not found in the controller                                                                                                                                                      | Yes                 | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| beforeException      | Triggered before the dispatcher throws any exception                                                                                                                                                           | Yes                 | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| afterDispatch        | Triggered after executing the controller/action method. As operation cannot be stopped, only use this event to make clean up after execute the action                                                          | Yes                 | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+
| afterDispatchLoop    | Triggered after exiting the dispatch loop                                                                                                                                                                      | No                  | Listeners             |
+----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+-----------------------+

The :doc:`INVO <tutorial-invo>` tutorial shows how to take advantage of dispatching events implementing a security filter with :doc:`Acl <acl>`

The following example demonstrates how to attach listeners to this component:

.. code-block:: php

    <?php

    use Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function(){

        //Create an event manager
        $eventsManager = new EventsManager();

        //Attach a listener for type "dispatch"
        $eventsManager->attach("dispatch", function($event, $dispatcher) {
            //...
        });

        $dispatcher = new MvcDispatcher();

        //Bind the eventsManager to the view component
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;

    }, true);

An instantiated controller automatically acts as a listener for dispatch events, so you can implement methods as callbacks:

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public function beforeExecuteRoute($dispatcher)
        {
            // Executed before every found action
        }

        public function afterExecuteRoute($dispatcher)
        {
            // Executed after every found action
        }

    }

Forwarding to other actions
---------------------------
The dispatch loop allows us to forward the execution flow to another controller/action. This is very useful to check if the user can
access to certain options, redirect users to other screens or simply reuse code.

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function saveAction($year, $postTitle)
        {

            // .. store some product and forward the user

            // Forward flow to the index action
            $this->dispatcher->forward(array(
                "controller" => "post",
                "action" => "index"
            ));
        }

    }

Keep in mind that making a "forward" is not the same as making an HTTP redirect. Although they apparently got the same result.
The "forward" doesn't reload the current page, all the redirection occurs in a single request, while the HTTP redirect needs two requests
to complete the process.

More forwarding examples:

.. code-block:: php

    <?php

    // Forward flow to another action in the current controller
    $this->dispatcher->forward(array(
        "action" => "search"
    ));

    // Forward flow to another action in the current controller
    // passing parameters
    $this->dispatcher->forward(array(
        "action" => "search",
        "params" => array(1, 2, 3)
    ));


A forward action accepts the following parameters:

+----------------+--------------------------------------------------------+
| Parameter      | Triggered                                              |
+================+========================================================+
| controller     | A valid controller name to forward to.                 |
+----------------+--------------------------------------------------------+
| action         | A valid action name to forward to.                     |
+----------------+--------------------------------------------------------+
| params         | An array of parameters for the action                  |
+----------------+--------------------------------------------------------+
| namespace      | A valid namespace name where the controller is part of |
+----------------+--------------------------------------------------------+

Preparing Parameters
--------------------
Thanks to the hooks points provided by :doc:`Phalcon\\Mvc\\Dispatcher <../api/Phalcon_Mvc_Dispatcher>` you can easily
adapt your application to any URL schema:

For example, you want your URLs look like: http://example.com/controller/key1/value1/key2/value

Parameters by default are passed as they come in the URL to actions, you can transform them to the desired schema:

.. code-block:: php

    <?php

    use Phalcon\Dispatcher,
        Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        //Attach a listener
        $eventsManager->attach("dispatch:beforeDispatchLoop", function($event, $dispatcher) {

            $keyParams = array();
            $params = $dispatcher->getParams();

            //Use odd parameters as keys and even as values
            foreach ($params as $number => $value) {
                if ($number & 1) {
                    $keyParams[$params[$number - 1]] = $value;
                }
            }

            //Override parameters
            $dispatcher->setParams($keyParams);
        });

        $dispatcher = new MvcDispatcher();
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

If the desired schema is: http://example.com/controller/key1:value1/key2:value, the following code is required:

.. code-block:: php

    <?php

    use Phalcon\Dispatcher,
        Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        //Attach a listener
        $eventsManager->attach("dispatch:beforeDispatchLoop", function($event, $dispatcher) {

            $keyParams = array();
            $params = $dispatcher->getParams();

            //Explode each parameter as key,value pairs
            foreach ($params as $number => $value) {
                $parts = explode(':', $value);
                $keyParams[$parts[0]] = $parts[1];
            }

            //Override parameters
            $dispatcher->setParams($keyParams);
        });

        $dispatcher = new MvcDispatcher();
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

Getting Parameters
------------------
When a route provides named parameters you can receive them in a controller, a view or any other component that extends
:doc:`Phalcon\\DI\\Injectable <../api/Phalcon_DI_Injectable>`.

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function saveAction()
        {

            // Get the post's title passed in the URL as parameter
            // or prepared in an event
            $title = $this->dispatcher->getParam("title");

            // Get the post's year passed in the URL as parameter
            // or prepared in an event also filtering it
            $year = $this->dispatcher->getParam("year", "int");
        }

    }

Preparing actions
-----------------
You can also define an arbitrary schema for actions before be dispatched.

Camelize action names
^^^^^^^^^^^^^^^^^^^^^
If the original URL is: http://example.com/admin/products/show-latest-products,
and for example you want to camelize 'show-latest-products' to 'ShowLatestProducts',
the following code is required:

.. code-block:: php

    <?php

    use Phalcon\Text,
        Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        //Camelize actions
        $eventsManager->attach("dispatch:beforeDispatchLoop", function($event, $dispatcher) {
            $dispatcher->setActionName(Text::camelize($dispatcher->getActionName()));
        });

        $dispatcher = new MvcDispatcher();
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

Remove legacy extensions
^^^^^^^^^^^^^^^^^^^^^^^^
If the original URL always contains a '.php' extension:

http://example.com/admin/products/show-latest-products.php
http://example.com/admin/products/index.php

You can remove it before dispatch the controller/action combination:

.. code-block:: php

    <?php

    use Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        //Remove extension before dispatch
        $eventsManager->attach("dispatch:beforeDispatchLoop", function($event, $dispatcher) {

            //Remove extension
            $action = preg_replace('/\.php$/', '', $dispatcher->getActionName());

            //Override action
            $dispatcher->setActionName($action);
        });

        $dispatcher = new MvcDispatcher();
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

Inject model instances
^^^^^^^^^^^^^^^^^^^^^^
In this example, the developer wants to inspect the parameters that an action will receive in order to dynamically
inject model instances.

The controller looks like:

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {
        /**
         * Shows posts
         *
         * @param \Posts $post
         */
        public function showAction(Posts $post)
        {
            $this->view->post = $post;
        }
    }

Method 'showAction' receives an instance of the model \Posts, the developer could inspect this
before dispatch the action preparing the parameter accordingly:

.. code-block:: php

    <?php

    use Phalcon\Text,
        Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        $eventsManager->attach("dispatch:beforeDispatchLoop", function($event, $dispatcher) {

            //Possible controller class name
            $controllerName =   Text::camelize($dispatcher->getControllerName()) . 'Controller';

            //Possible method name
            $actionName = $dispatcher->getActionName() . 'Action';

            try {

                //Get the reflection for the method to be executed
                $reflection = new \ReflectionMethod($controllerName, $actionName);

                //Check parameters
                foreach ($reflection->getParameters() as $parameter) {

                    //Get the expected model name
                    $className = $parameter->getClass()->name;

                    //Check if the parameter expects a model instance
                    if (is_subclass_of($className, 'Phalcon\Mvc\Model')) {

                        $model = $className::findFirstById($dispatcher->getParams()[0]);

                        //Override the parameters by the model instance
                        $dispatcher->setParams(array($model));
                    }
                }

            } catch (\Exception $e) {
                //An exception has occurred, maybe the class or action does not exist?
            }

        });

        $dispatcher = new MvcDispatcher();
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

The above example has been simplified for academic purposes.
A developer can improve it to inject any kind of dependency or model in actions before be executed.

Handling Not-Found Exceptions
-----------------------------
Using the :doc:`EventsManager <events>` it's possible to insert a hook point before the dispatcher throws an exception
when the controller/action combination wasn't found:

.. code-block:: php

    <?php

    use Phalcon\Dispatcher,
        Phalcon\Mvc\Dispatcher as MvcDispatcher,
        Phalcon\Events\Manager as EventsManager,
        Phalcon\Mvc\Dispatcher\Exception as DispatchException;

    $di->set('dispatcher', function() {

        //Create an EventsManager
        $eventsManager = new EventsManager();

        //Attach a listener
        $eventsManager->attach("dispatch:beforeException", function($event, $dispatcher, $exception) {

            //Handle 404 exceptions
            if ($exception instanceof DispatchException) {
                $dispatcher->forward(array(
                    'controller' => 'index',
                    'action' => 'show404'
                ));
                return false;
            }

            //Alternative way, controller or action doesn't exist
            if ($event->getType() == 'beforeException') {
                switch ($exception->getCode()) {
                    case \Phalcon\Dispatcher::EXCEPTION_HANDLER_NOT_FOUND:
                    case \Phalcon\Dispatcher::EXCEPTION_ACTION_NOT_FOUND:
                        $dispatcher->forward(array(
                            'controller' => 'index',
                            'action' => 'show404'
                        ));
                        return false;
                }
            }
        });

        $dispatcher = new \Phalcon\Mvc\Dispatcher();

        //Bind the EventsManager to the dispatcher
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;

    }, true);

Of course, this method can be moved onto independent plugin classes, allowing more than one class
take actions when an exception is produced in the dispatch loop:


.. code-block:: php

    <?php

    use Phalcon\Mvc\Dispatcher,
        Phalcon\Events\Event,
        Phalcon\Mvc\Dispatcher\Exception as DispatchException;

    class ExceptionsPlugin
    {
        public function beforeException(Event $event, Dispatcher $dispatcher, $exception)
        {

            //Handle 404 exceptions
            if ($exception instanceof DispatchException) {
                $dispatcher->forward(array(
                    'controller' => 'index',
                    'action' => 'show404'
                ));
                return false;
            }

            //Handle other exceptions
            $dispatcher->forward(array(
                'controller' => 'index',
                'action' => 'show503'
            ));

            return false;
        }
    }

.. highlights::

    Only exceptions produced by the dispatcher and exceptions produced in the executed action
    are notified in the 'beforeException' events. Exceptions produced in listeners or
    controller events are redirected to the latest try/catch.

Implementing your own Dispatcher
--------------------------------
The :doc:`Phalcon\\Mvc\\DispatcherInterface <../api/Phalcon_Mvc_DispatcherInterface>` interface must be implemented to create your own dispatcher
replacing the one provided by Phalcon.
