Hiling at Tugon na mga Object
##########################

.. php:namespace:: Cake\Http

Ang hiling at tugon na mga object ay nagbibigay ng isang abstraksyon na umiikot sa HTTP na mga hiling at
mga tugon. Ang hiling na object sa CakePHP ay nagpapahintulot sa ito na mag-introspect sa isang papasok
na hiling, habang ang tugon na ay nagpapahintulot sa iyo na walang kahirap-hirap na paglikha ng HTTP
na mga tugon mula sa iyong mga controller.

.. index:: $this->request
.. _cake-request:

Hiling
=======

.. php:class:: ServerRequest

Ang ``ServerRequest`` ay isang default na hiling na object na ginamit sa CakePHP. Nagsasagitna ito ng ilang mga tampok
para magtanong at makipag-ugnayan sa hiniling na datos.
Bawat isang kahilingan sa isang Request ay nalikha at pagkatapos napasa sa reperensiya sa 
iba-ibang mga layer ng isang aplikasyon na gumagamit ng hiling na datos. Bilang default ang hiling
ay nakatalaga sa ``$this->request``, at magagamit sa mga Controller, mga Cell, mga View
at mga Helper. Maaari mo ring ma-access ito sa mga Component na gumagamit ng controller
na reperensiya. Ang ilang mga tungkulin na isinasagawa ng ``ServerRequest`` ay nagsasama ng:

* Ang pagproseso sa GET, POST, at mga FILE na mga array sa istraktura ng datos na ikaw ay
  pamilyar.
* Pagbibigay ng environment na introspeksyon na nauukol sa hiling. Ang impormasyon
  na tulad ng mga header na pinadala, ang IP address ng kliyente, at ang subdomain/domain
  na mga pangalan sa server ng iyong aplikasyon na pinatakbo.
* Pagbibigay ng access sa hiling na mga parameter sa parehong bilang ng array na mga index at object
  na mga katangian.

Tulad ng 3.4.0, ang CakePHP na hiling na object na nagpapatupad sa `PSR-7
ServerRequestInterface <http://www.php-fig.org/psr/psr-7/>`_ na ginagawang mas madali ang
paggamit ng mga library mula sa labas ng CakePHP.

Hiling na mga Parameter
-----------------------

Ang hiling ay nagbubunyag ng routing na mga parameter sa pamamagitan ng ``getParam()`` na pamamaraan::

    $controllerName = $this->request->getParam('controller');

    // Bago ang 3.4.0
    $controllerName = $this->request->param('controller');

Lahat ng :ref:`route-elements` ay ma-access sa pamamagitan ng interface na ito.

At saka sa :ref:`route-elements`, madalas din kayo na mangangailangan ng access sa
:ref:`passed-arguments`. Ang mga ito ay parehong magagamit sa hiling na object 
din::

    // Naipasa na mga argumento
    $passedArgs = $this->request->getParam('pass');

Ang lahat ay magkakaloob sa iyo ng access upang maipasa ang mga argumento. Doon
ay may ilang importante/kapaki-pakinabang na mga parameter na ang CakePHP ay gumagamit sa panloob, ang mga ito
ay natagpuan din ang lahat sa routing na mga parameter:

* ``plugin`` Ang plugin na paghahawak ng hiling. Ay magiging null kapag walang
  plugin.
* ``controller`` Ang controller na paghahawak sa kasalukuyang hiling.
* ``action`` Ang aksyon na paghahawak ay kasalukuyang hiling.
* ``prefix`` Ang prefix para sa kasalukuyang aksyon. Tingnan ang :ref:`prefix-routing` para sa
  karagdagang impormasyon.

Query String na mga Parameter
-----------------------------

.. php:method:: getQuery($name)

Ang Query string na mga parameter ay maaaring mabasa gamit ang ``getQuery()`` na pamamaraan::

    // Ang URL ay /posts/index?page=1&sort=title
    $page = $this->request->getQuery('page');

    // Bago ang 3.4.0
    $page = $this->request->query('page');

Maaari kang direktang mag-access sa query na katangian, o maaari kang makagamit sa
``getQuery()`` na pamamaraan upang basahin ang URL query na array sa isang walang pagkakamali na paraan.
Anumang mga key na hindi umiiral ay babalik sa ``null``::

    $foo = $this->request->getQuery('value_that_does_not_exist');
    // $foo === null

    // Maaari ka ring magbigay ng default na mga halaga
    $foo = $this->request->getQuery('does_not_exist', 'default val');

Kung gusto mong ma-access ang lahat ng query na mga parameter maaari kang gumamit ng
``getQueryParams()``::

    $query = $this->request->getQueryParams();

.. versionadded:: 3.4.0
    ``getQueryParams()`` and ``getQuery()`` were added in 3.4.0

Humiling sa Buong Datos
-----------------------

.. php:method:: getData($name, $default = null)

Lahat ng POST na datos ay maaaring ma-access gamit ang
:php:meth:`Cake\\Http\\ServerRequest::getData()`.  Anumang porma ng datos na
naglalaman ng ``data`` na prefix ay tatanggalan ng datos na prefix na ito. Halimbawa::

    // Ang isang input na may isang pangalan na katangian na pantay sa 'MyModel[title]' ay naa-access sa 
    $title = $this->request->getData('MyModel.title');

Anumang key na hindi umiiral ay magbabalik ng ``null``::

    $foo = $this->request->getData('Value.that.does.not.exist');
    // $foo == null

PUT, PATCH o DELETE na Datos
----------------------------

.. php:method:: input($callback, [$options])

Kapag nagbubuo ng REST na mga serbisyo, madalas mong tanggapin ang hiling na datos sa ``PUT`` at
``DELETE`` na mga hiling. Anumang ``application/x-www-form-urlencoded`` na hiling sa buong datos
ay awtomatikong ma-parse at maitakda sa ``$this->data`` para sa ``PUT`` at
``DELETE`` na mga hiling. Kung ikaw ay tumatanggap ng JSON o XML na datos, tingnan sa ibaba para sa kung paano
ikaw maka-access sa mga hiling na katawan na iyon.

Kapag nag-access sa input na datos, maaari kang maka-decode nito na may isang opsyonal na function.
Ito ay kapaki-pakinabang kapag nakipag-ugnayan sa XML o JSON na hiling sa buong nilalaman.
Ang karagdagang mga parameter para sa pag-decode ng function ay maaaring mapasa bilang mga argumento sa
``input()``::

    $jsonData = $this->request->input('json_decode');

Environment na mga Variable (from $_SERVER and $_ENV)
-----------------------------------------------------

.. php:method:: env($key, $value = null)

Ang ``ServerRequest::env()`` ay isang tagapagbalot para sa ``env()`` sa global na punsyon at gumaganap bilang
isang kumukuha/tagapagtakda para sa enviromnent na mga variables nang hindi nagbabago ng mga global
``$_SERVER`` and ``$_ENV``::

    // Kunin ang host
    $host = $this->request->env('HTTP_HOST');

    // Itakda ang halaga, sa pangkalahatan ay makakatulong sa pagsusubok.
    $this->request->env('REQUEST_METHOD', 'POST');

Para ma-access ang lahat ng environment na mga variable sa isang hiling gamitin ang ``getServerParams()``::

    $env = $this->request->getServerParams();

.. versionadded:: 3.4.0
    ``getServerParams()`` was added in 3.4.0

XML o JSON na Datos
-------------------

Ang mga aplikasyon ay gumagamit ng :doc:`/development/rest` nang madalas na pagpapalit ng datos sa
non-URL-encoded post na mga body. Maaari kang bumasa ng input na datos sa anumang format gamit ang
:php:meth:`~Cake\\Http\\ServerRequest::input()`. Sa pamamagitan ng pagbibigay ng pag-decode na function,
maaari kang makakuha sa nilalaman sa isang deserialized na format::

    // Kunin ang JSON na naka-encode na datos na sinumete sa isang PUT/POST na aksyon
    $jsonData = $this->request->input('json_decode');

Ang ilang deserializing na mga pamamaraan ay kailangan ng karagdagang mga parameter kapag natawag, tulad ng
isang 'as array' na parameter sa ``json_decode``. Kung gusto mo ang XML na naka-convert sa isang
DOMDocument na object, :php:meth:`~Cake\\Http\\ServerRequest::input()` na sumusuporta
sa pagpasa sa karagdagang na mga parameter din::

    // Kunin ang XML na naka-encode na datos na sinumete sa isang PUT/POST na aksyon
    $data = $this->request->input('Cake\Utility\Xml::build', ['return' => 'domdocument']);

Path na Impormasyon
-------------------

Ang hiling na object ay nagbibigay din ng kapaki-pakinabang na impormasyon tungkol sa iyong mga path sa iyong
aplikasyon. Ang ``base`` at ``webroot`` na mga katangian ay kapaki-pakinabang para sa
pagbubuo ng mga URL, at pagtukoy kung o hindi ang iyong aplikasyon ay nasa isang
subdirektoryo. Ang mga katangian na maaari mong gamitin ay::

    // Ipagpalagay ang kasalukuyang hiling na URL ay /subdir/articles/edit/1?page=1

    // Humahawak sa /subdir/articles/edit/1?page=1
    $here = $request->getRequestTarget();

    // Humahawak sa /subdir
    $base = $request->getAttribute('base');

    // Humahawak sa /subdir/
    $base = $request->getAttribute('webroot');

    // Bago ang 3.4.0
    $webroot = $request->webroot;
    $base = $request->base;
    $here = $request->here();

.. _check-the-request:

Pagsusuri ng Hiling na mga Kondisyon
------------------------------------

.. php:method:: is($type, $args...)

Ang hiling na object ay nagbibigay ng isang madaling paraan para tingnan ang mga kondisyon sa binigay na
hiling. Sa pamamagitan ng paggamit sa ``is()`` na pamamaraan ay maaaring makasuri ng isang numero sa karaniwan na
mga kondisyon, pati na rin siyasatin ang ibang aplikasyon na partikular na pamantayan::

    $isPost = $this->request->is('post');

Maaari mo ring palawakin ang hiling na mga detektor na magagamit, sa pamamagitan sa paggamit ng
:php:meth:`Cake\\Http\\ServerRequest::addDetector()` upang lumikha ng bagong mga uri ng
mga detektor. Mayroong apat na magkaibang mga uri ng mga detektor na maaari kang lumikha:

* Ang Environment na halaga na paghahambing - ay naghahambing sa halaga ng nakuha mula sa :php:func:`env()`
  para sa pagkakapantay-pantay na may binigay na halaga.
* Ang Pattern na halaga na paghahambing - Ang pattern na halaga ng paghahambing ay nagpapahintulot sa iyo na maghambing sa
  halaga na nakuha mula sa :php:func:`env()` sa isang regular na ekspresyon.
* Pagpipilian batay sa paghahambing -  Nakabase sa Opsyon na paghahambing sa paggamit ng isang listahan ng mga opsyon upang
  lumikha ng regular na ekspresyon. Kasunod na mga tawag upang magdagdag ng natukoy na
  mga opsyon na detektor ay pagsasama-sama ng mga opsyon.
* Callback na mga detektor - Ang mga callback detektor ay nagpapahintulot sa iyo para magbigay ng isang 'callback' na uri
  upang hawakan ang pagsuri. Ang callback ay makakatanggap ng isang hiling na object na ito lamang
  ang parameter.

.. php:method:: addDetector($name, $options)

Ang ilang mga halimbawa ay maaaring maging::

    // Magdagdag ng environment na detektor.
    $this->request->addDetector(
        'post',
        ['env' => 'REQUEST_METHOD', 'value' => 'POST']
    );

    // Magdagdag ng pattern na halaga na detektor.
    $this->request->addDetector(
        'iphone',
        ['env' => 'HTTP_USER_AGENT', 'pattern' => '/iPhone/i']
    );

    // Magdagdag ng opsyon na detektor
    $this->request->addDetector('internalIp', [
        'env' => 'CLIENT_IP',
        'options' => ['192.168.0.101', '192.168.0.100']
    ]);

    // Magdagdag ng callback na detektor. Kailangang isang balido na matatawagan.
    $this->request->addDetector(
        'awesome',
        function ($request) {
            return $request->getParam('awesome');
        }
    );

    // Magdagdag ng isang dektektor na gumagamit sa karagdagan na mga argumento. Batay sa 3.3.0
    $this->request->addDetector(
        'controller',
        function ($request, $name) {
            return $request->getParam('controller') === $name;
        }
    );

``Request`` kasama rin dito ang mga pamamaraan na tulad sa 
:php:meth:`Cake\\Http\\ServerRequest::domain()`,
:php:meth:`Cake\\Http\\ServerRequest::subdomains()` at
:php:meth:`Cake\\Http\\ServerRequest::host()` upang tumulong sa mga aplikasyon na may mga subdomain,
magkaroon ng isang bahagyang mas madaling buhay.

Mayroong ilang mga built-in na mga detektor na magagamit mo:

* ``is('get')`` Suriin upang makita kung ang kasalukuyang hiling ay isang GET.
* ``is('put')`` Suriin upang makita kung ang kasalukuyang  hiling ay isang PUT.
* ``is('patch')`` Suriin upang makita kung ang kasalukuyang hiling ay isang PATCH.
* ``is('post')`` Suriin upang makita kung ang kasalukuyang hiling ay isang POST.
* ``is('delete')`` Suriin upang makita kung ang kasalukuyang hiling ay isang DELETE.
* ``is('head')`` Suriin upang makita kung ang kasalukuyang hiling ay HEAD.
* ``is('options')`` Suriin upang makita kung ang kasalukuyang hiling ay OPTIONS.
* ``is('ajax')`` Suriin upang makita kung ang kasalukuyang hiling na darating na may
  X-Requested-With = XMLHttpRequest.
* ``is('ssl')`` Suriin upang makita kung ang hiling ay sa pamamagitan ng SSL.
* ``is('flash')`` Suriin upang makita kung ang hiling ay mayroong isang User-Agent ng Flash.
* ``is('requested')`` Suriin upang makita ang kasalukuyang hiling ay mayroong isang query param
  'requested' na may halaga na 1.
* ``is('json')`` Suriin upang makita ang hiling ay mayroong 'json' na ekstensyon at
  tumatanggap ng 'application/json' na mimetype.
* ``is('xml')`` Suriin upang makita ang hiling ay mayroong 'xml' na ekstensyon at tumatanggap ng
  'application/xml' o 'text/xml' na mimetype.

.. versionadded:: 3.3.0
    Ang mga Detektor ay maaaring tumanggap ng karagdagang mga parameter batay sa 3.3.0.

Session Data
------------

To access the session for a given request use the ``session()`` method::

    $userName = $this->request->session()->read('Auth.User.name');

For more information, see the :doc:`/development/sessions` documentation for how
to use the session object.

Host and Domain Name
--------------------

.. php:method:: domain($tldLength = 1)

Returns the domain name your application is running on::

    // Prints 'example.org'
    echo $request->domain();

.. php:method:: subdomains($tldLength = 1)

Returns the subdomains your application is running on as an array::

    // Returns ['my', 'dev'] for 'my.dev.example.org'
    $subdomains = $request->subdomains();

.. php:method:: host()

Returns the host your application is on::

    // Prints 'my.dev.example.org'
    echo $request->host();

Reading the HTTP Method
-----------------------

.. php:method:: getMethod()

Returns the HTTP method the request was made with::

    // Output POST
    echo $request->getMethod();

    // Prior to 3.4.0
    echo $request->method();

Restricting Which HTTP method an Action Accepts
-----------------------------------------------

.. php:method:: allowMethod($methods)

Set allowed HTTP methods. If not matched, will throw
``MethodNotAllowedException``. The 405 response will include the required
``Allow`` header with the passed methods::

    public function delete()
    {
        // Only accept POST and DELETE requests
        $this->request->allowMethod(['post', 'delete']);
        ...
    }

Reading HTTP Headers
--------------------

Allows you to access any of the ``HTTP_*`` headers that were used
for the request. For example::

    // Get the header as a string
    $userAgent = $this->request->getHeaderLine('User-Agent');

    // Get an array of all values.
    $acceptHeader = $this->request->getHeader('Accept');

    // Check if a header exists
    $hasAcceptHeader = $this->request->hasHeader('Accept');

    // Prior to 3.4.0
    $userAgent = $this->request->header('User-Agent');

While some apache installs don't make the ``Authorization`` header accessible,
CakePHP will make it available through apache specific methods as required.

.. php:method:: referer($local = false)

Returns the referring address for the request.

.. php:method:: clientIp()

Returns the current visitor's IP address.

Trusting Proxy Headers
----------------------

If your application is behind a load balancer or running on a cloud service, you
will often get the load balancer host, port and scheme in your requests. Often
load balancers will also send ``HTTP-X-Forwarded-*`` headers with the original
values. The forwarded headers will not be used by CakePHP out of the box. To
have the request object use these headers set the ``trustProxy`` property to
``true``::

    $this->request->trustProxy = true;

    // These methods will now use the proxied headers.
    $port = $this->request->port();
    $host = $this->request->host();
    $scheme = $this->request->scheme();
    $clientIp = $this->request->clientIp();

Checking Accept Headers
-----------------------

.. php:method:: accepts($type = null)

Find out which content types the client accepts, or check whether it accepts a
particular type of content.

Get all types::

    $accepts = $this->request->accepts();

Check for a single type::

    $acceptsJson = $this->request->accepts('application/json');

.. php:method:: acceptLanguage($language = null)

Get all the languages accepted by the client,
or check whether a specific language is accepted.

Get the list of accepted languages::

    $acceptsLanguages = $this->request->acceptLanguage();

Check whether a specific language is accepted::

    $acceptsSpanish = $this->request->acceptLanguage('es-es');

.. _request-cookies:

Cookies
-------

Request cookies can be read through a number of methods::

    // Get the cookie value, or null if the cookie is missing.
    $rememberMe = $this->request->getCookie('remember_me');

    // Read the value, or get the default of 0
    $rememberMe = $this->request->getCookie('remember_me', 0);

    // Get all cookies as an hash
    $cookies = $this->request->getCookieParams();

    // Get a CookieCollection instance (starting with 3.5.0)
    $cookies = $this->request->getCookieCollection()

See the :php:class:`Cake\\Http\\Cookie\\CookieCollection` documentation for how
to work with cookie collection.

.. versionadded:: 3.5.0
    ``ServerRequest::getCookieCollection()`` was added in 3.5.0

.. index:: $this->response

Response
========

.. php:class:: Response

:php:class:`Cake\\Http\\Response` is the default response class in CakePHP.
It encapsulates a number of features and functionality for generating HTTP
responses in your application. It also assists in testing, as it can be
mocked/stubbed allowing you to inspect headers that will be sent.
Like :php:class:`Cake\\Http\\ServerRequest`, :php:class:`Cake\\Http\\Response`
consolidates a number of methods previously found on :php:class:`Controller`,
:php:class:`RequestHandlerComponent` and :php:class:`Dispatcher`. The old
methods are deprecated in favour of using :php:class:`Cake\\Http\\Response`.

``Response`` provides an interface to wrap the common response-related
tasks such as:

* Sending headers for redirects.
* Sending content type headers.
* Sending any header.
* Sending the response body.

Dealing with Content Types
--------------------------

.. php:method:: withType($contentType = null)

You can control the Content-Type of your application's responses with
:php:meth:`Cake\\Http\\Response::withType()`. If your application needs to deal
with content types that are not built into Response, you can map them with
``type()`` as well::

    // Add a vCard type
    $this->response->type(['vcf' => 'text/v-card']);

    // Set the response Content-Type to vcard.
    $this->response = $this->response->withType('vcf');

    // Prior to 3.4.0
    $this->response->type('vcf');

Usually, you'll want to map additional content types in your controller's
:php:meth:`~Controller::beforeFilter()` callback, so you can leverage the
automatic view switching features of :php:class:`RequestHandlerComponent` if you
are using it.

.. _cake-response-file:

Sending Files
-------------

.. php:method:: withFile($path, $options = [])

There are times when you want to send files as responses for your requests.
You can accomplish that by using :php:meth:`Cake\\Http\\Response::withFile()`::

    public function sendFile($id)
    {
        $file = $this->Attachments->getFile($id);
        $response = $this->response->withFile($file['path']);
        // Return the response to prevent controller from trying to render
        // a view.
        return $response;
    }

    // Prior to 3.4.0
    $file = $this->Attachments->getFile($id);
    $this->response->file($file['path']);
    // Return the response to prevent controller from trying to render
    // a view.
    return $this->response;

As shown in the above example, you must pass the file path to the method.
CakePHP will send a proper content type header if it's a known file type listed
in `Cake\\Http\\Reponse::$_mimeTypes`. You can add new types prior to calling
:php:meth:`Cake\\Http\\Response::withFile()` by using the
:php:meth:`Cake\\Http\\Response::withType()` method.

If you want, you can also force a file to be downloaded instead of displayed in
the browser by specifying the options::

    $response = $this->response->withFile(
        $file['path'],
        ['download' => true, 'name' => 'foo']
    );

    // Prior to 3.4.0
    $this->response->file(
        $file['path'],
        ['download' => true, 'name' => 'foo']
    );

The supported options are:

name
    The name allows you to specify an alternate file name to be sent to
    the user.
download
    A boolean value indicating whether headers should be set to force
    download.

Sending a String as File
------------------------

You can respond with a file that does not exist on the disk, such as a pdf or an
ics generated on the fly from a string::

    public function sendIcs()
    {
        $icsString = $this->Calendars->generateIcs();
        $response = $this->response;
        $response->body($icsString);

        $response = $response->withType('ics');

        // Optionally force file download
        $response = $response->withDownload('filename_for_download.ics');

        // Return response object to prevent controller from trying to render
        // a view.
        return $response;
    }

Callbacks can also return the body as a string::

    $path = '/some/file.png';
    $this->response->body(function () use ($path) {
        return file_get_contents($path);
    });

Setting Headers
---------------

.. php:method:: withHeader($header, $value)

Setting headers is done with the :php:meth:`Cake\\Http\\Response::withHeader()`
method. Like all of the PSR-7 interface methods, this method returns a *new*
instance with the new header::

    // Add/replace a header
    $response = $response->withHeader('X-Extra', 'My header');

    // Set multiple headers
    $response = $response->withHeader('X-Extra', 'My header')
        ->withHeader('Location', 'http://example.com');

    // Append a value to an existing header
    $response = $response->withAddedHeader('Set-Cookie', 'remember_me=1');

    // Prior to 3.4.0 - Set a header
    $this->response->header('Location', 'http://example.com');

Headers are not sent when set. Instead, they are held until the response is
emitted by ``Cake\Http\Server``.

You can now use the convenience method
:php:meth:`Cake\\Http\\Response::withLocation()` to directly set or get the
redirect location header.

Setting the Body
----------------

.. php:method:: withStringBody($string)

To set a string as the response body, do the following::

    // Set a string into the body
    $response = $response->withStringBody('My Body');

    // If you want a json response
    $response = $response->withType('application/json')
        ->withStringBody(json_encode(['Foo' => 'bar']));

.. versionadded:: 3.4.3
    ``withStringBody()`` was added in 3.4.3

.. php:method:: withBody($body)

To set the response body, use the ``withBody()`` method, which is provided by the
:php:class:`Zend\\Diactoros\\MessageTrait`::

    $response = $response->withBody($stream);

    // Prior to 3.4.0 - Set the body
    $this->response->body('My Body');

Be sure that ``$stream`` is a :php:class:`Psr\\Http\\Message\\StreamInterface` object.
See below on how to create a new stream.

You can also stream responses from files using :php:class:`Zend\\Diactoros\\Stream` streams::

    // To stream from a file
    use Zend\Diactoros\Stream;

    $stream = new Stream('/path/to/file', 'rb');
    $response = $response->withBody($stream);

You can also stream responses from a callback using the ``CallbackStream``. This
is useful when you have resources like images, CSV files or PDFs you need to
stream to the client::

    // Streaming from a callback
    use Cake\Http\CallbackStream;

    // Create an image.
    $img = imagecreate(100, 100);
    // ...

    $stream = new CallbackStream(function () use ($img) {
        imagepng($img);
    });
    $response = $response->withBody($stream);

    // Prior to 3.4.0 you can use the following to create streaming responses.
    $file = fopen('/some/file.png', 'r');
    $this->response->body(function () use ($file) {
        rewind($file);
        fpassthru($file);
        fclose($file);
    });

Setting the Character Set
-------------------------

.. php:method:: withCharset($charset)

Sets the charset that will be used in the response::

    $this->response = $this->response->withCharset('UTF-8');

    // Prior to 3.4.0
    $this->response->charset('UTF-8');

Interacting with Browser Caching
--------------------------------

.. php:method:: withDisabledCache()

You sometimes need to force browsers not to cache the results of a controller
action. :php:meth:`Cake\\Http\\Response::withDisabledCache()` is intended for just
that::

    public function index()
    {
        // Disable caching
        $this->response = $this->response->withDisabledCache();

        // Prior to 3.4.0
        $this->response->disableCache();
    }

.. warning::

    Disabling caching from SSL domains while trying to send
    files to Internet Explorer can result in errors.

.. php:method:: withCache($since, $time = '+1 day')

You can also tell clients that you want them to cache responses. By using
:php:meth:`Cake\\Http\\Response::withCache()`::

    public function index()
    {
        // Enable caching
        $this->response = $this->response->withCache('-1 minute', '+5 days');
    }

The above would tell clients to cache the resulting response for 5 days,
hopefully speeding up your visitors' experience.
The ``withCache()`` method sets the ``Last-Modified`` value to the first
argument. ``Expires`` header and the ``max-age`` directive are set based on the
second parameter. Cache-Control's ``public`` directive is set as well.

.. _cake-response-caching:

Fine Tuning HTTP Cache
----------------------

One of the best and easiest ways of speeding up your application is to use HTTP
cache. Under this caching model, you are only required to help clients decide if
they should use a cached copy of the response by setting a few headers such as
modified time and response entity tag.

Rather than forcing you to code the logic for caching and for invalidating
(refreshing) it once the data has changed, HTTP uses two models, expiration and
validation, which usually are much simpler to use.

Apart from using :php:meth:`Cake\\Http\\Response::withCache()`, you can also use
many other methods to fine-tune HTTP cache headers to take advantage of browser
or reverse proxy caching.

The Cache Control Header
~~~~~~~~~~~~~~~~~~~~~~~~

.. php:method:: withSharable($public, $time = null)

Used under the expiration model, this header contains multiple indicators that
can change the way browsers or proxies use the cached content. A
``Cache-Control`` header can look like this::

    Cache-Control: private, max-age=3600, must-revalidate

``Response`` class helps you set this header with some utility methods that will
produce a final valid ``Cache-Control`` header. The first is the
``withSharable()`` method, which indicates whether a response is to be
considered sharable across different users or clients. This method actually
controls the ``public`` or ``private`` part of this header.  Setting a response
as private indicates that all or part of it is intended for a single user. To
take advantage of shared caches, the control directive must be set as public.

The second parameter of this method is used to specify a ``max-age`` for the
cache, which is the number of seconds after which the response is no longer
considered fresh::

    public function view()
    {
        // ...
        // Set the Cache-Control as public for 3600 seconds
        $this->response = $this->response->withSharable(true, 3600);
    }

    public function my_data()
    {
        // ...
        // Set the Cache-Control as private for 3600 seconds
        $this->response = $this->response->withSharable(false, 3600);
    }

``Response`` exposes separate methods for setting each of the directives in
the ``Cache-Control`` header.

The Expiration Header
~~~~~~~~~~~~~~~~~~~~~

.. php:method:: withExpires($time)

You can set the ``Expires`` header to a date and time after which the response
is no longer considered fresh. This header can be set using the
``withExpires()`` method::

    public function view()
    {
        $this->response = $this->response->withExpires('+5 days');
    }

This method also accepts a :php:class:`DateTime` instance or any string that can
be parsed by the :php:class:`DateTime` class.

The Etag Header
~~~~~~~~~~~~~~~

.. php:method:: withEtag($tag, $weak = false)

Cache validation in HTTP is often used when content is constantly changing, and
asks the application to only generate the response contents if the cache is no
longer fresh. Under this model, the client continues to store pages in the
cache, but it asks the application every time
whether the resource has changed, instead of using it directly.
This is commonly used with static resources such as images and other assets.

The ``withEtag()`` method (called entity tag) is a string
that uniquely identifies the requested resource, as a checksum does for a file,
in order to determine whether it matches a cached resource.

To take advantage of this header, you must either call the
``checkNotModified()`` method manually or include the
:doc:`/controllers/components/request-handling` in your controller::

    public function index()
    {
        $articles = $this->Articles->find('all');
        $response = $this->response->withEtag($this->Articles->generateHash($articles));
        if ($response->checkNotModified($this->request)) {
            return $response;
        }
        $this->response = $response;
        // ...
    }

.. note::

    Most proxy users should probably consider using the Last Modified Header
    instead of Etags for performance and compatibility reasons.

The Last Modified Header
~~~~~~~~~~~~~~~~~~~~~~~~

.. php:method:: withModified($time)

Also, under the HTTP cache validation model, you can set the ``Last-Modified``
header to indicate the date and time at which the resource was modified for the
last time. Setting this header helps CakePHP tell caching clients whether the
response was modified or not based on their cache.

To take advantage of this header, you must either call the
``checkNotModified()`` method manually or include the
:doc:`/controllers/components/request-handling` in your controller::

    public function view()
    {
        $article = $this->Articles->find()->first();
        $response = $this->response->withModified($article->modified);
        if ($response->checkNotModified($this->request)) {
            return $response;
        }
        $this->response;
        // ...
    }

The Vary Header
~~~~~~~~~~~~~~~

.. php:method:: withVary($header)

In some cases, you might want to serve different content using the same URL.
This is often the case if you have a multilingual page or respond with different
HTML depending on the browser. Under such circumstances you can use the ``Vary``
header::

    $response = $this->response->withVary('User-Agent');
    $response = $this->response->withVary('Accept-Encoding', 'User-Agent');
    $response = $this->response->withVary('Accept-Language');

Sending Not-Modified Responses
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. php:method:: checkNotModified(Request $request)

Compares the cache headers for the request object with the cache header from the
response and determines whether it can still be considered fresh. If so, deletes
the response content, and sends the `304 Not Modified` header::

    // In a controller action.
    if ($this->response->checkNotModified($this->request)) {
        return $this->response;
    }

.. _response-cookies:

Setting Cookies
===============

Cookies can be added to response using either an array or a :php:class:`Cake\\Http\\Cookie\\Cookie`
object::

    // Add a cookie as an array using the immutable API (3.4.0+)
    $this->response = $this->response->withCookie('remember_me', [
        'value' => 'yes',
        'path' => '/',
        'httpOnly' => true,
        'secure' => false,
        'expire' => strtotime('+1 year')
    ]);

    // Before 3.4.0
    $this->response->cookie('remember', [
        'value' => 'yes',
        'path' => '/',
        'httpOnly' => true,
        'secure' => false,
        'expire' => strtotime('+1 year')
    ]);

See the :ref:`creating-cookies` section for how to use the cookie object. You
can use ``withExpiredCookie()`` to send an expired cookie in the response. This
will make the browser remove its local cookie::

    // As of 3.5.0
    $this->response = $this->response->withExpiredCookie('remember_me');

.. _cors-headers:

Setting Cross Origin Request Headers (CORS)
===========================================

As of 3.2 you can use the ``cors()`` method to define `HTTP Access Control
<https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS>`__
related headers with a fluent interface::

    $this->response->cors($this->request)
        ->allowOrigin(['*.cakephp.org'])
        ->allowMethods(['GET', 'POST'])
        ->allowHeaders(['X-CSRF-Token'])
        ->allowCredentials()
        ->exposeHeaders(['Link'])
        ->maxAge(300)
        ->build();

CORS related headers will only be applied to the response if the following
criteria are met:

#. The request has an ``Origin`` header.
#. The request's ``Origin`` value matches one of the allowed Origin values.

.. versionadded:: 3.2
    The ``CorsBuilder`` was added in 3.2

Common Mistakes with Immutable Responses
========================================

As of CakePHP 3.4.0, response objects offer a number of methods that treat
responses as immutable objects. Immutable objects help prevent difficult to
track accidental side-effects, and reduce mistakes caused by method calls caused
by refactoring that change ordering. While they offer a number of benefits,
immutable objects can take some getting used to. Any method that starts with
``with`` operates on the response in an immutable fashion, and will **always**
return a **new** instance. Forgetting to retain the modified instance is the most
frequent mistake people make when working with immutable objects::

    $this->response->withHeader('X-CakePHP', 'yes!');

In the above code, the response will be lacking the ``X-CakePHP`` header, as the
return value of the ``withHeader()`` method was not retained. To correct the
above code you would write::

    $this->response = $this->response->withHeader('X-CakePHP', 'yes!');

.. php:namespace:: Cake\Http\Cookie

Cookie Collections
==================

.. php:class:: CookieCollection

``CookieCollection`` objects are accessible from the request and response objects.
They let you interact with groups of cookies using immutable patterns, which
allow the immutability of the request and response to be preserved.

.. _creating-cookies:

Creating Cookies
----------------

.. php:class:: Cookie

``Cookie`` objects can be defined through constructor objects, or by using the
fluent interface that follows immutable patterns::

    use Cake\Http\Cookie\Cookie;

    // All arguments in the constructor
    $cookie = new Cookie(
        'remember_me', // name
        1, // value
        new DateTime('+1 year'), // expiration time, if applicable
        '/', // path, if applicable
        'example.com', // domain, if applicable
        false, // secure only?
        true // http only ?
    );

    // Using the builder methods
    $cookie = (new Cookie('remember_me'))
        ->withValue('1')
        ->withExpiry(new DateTime('+1 year'))
        ->withPath('/')
        ->withDomain('example.com')
        ->withSecure(false)
        ->withHttpOnly(true);

Once you have created a cookie, you can add it to a new or existing
``CookieCollection``::

    use Cake\Http\Cookie\CookieCollection;

    // Create a new collection
    $cookies = new CookieCollection([$cookie]);

    // Add to an existing collection
    $cookies = $cookies->add($cookie);

    // Remove a cookie by name
    $cookies = $cookies->remove('remember_me');

.. note::
    Remember that collections are immutable and adding cookies into, or removing
    cookies from a collection, creates a *new* collection object.

You should use the ``withCookie()`` method to add cookies to ``Response``
objects as it is simpler to use::

    $response = $this->response->withCookie($cookie);

Cookies set to responses can be encrypted using the
:ref:`encrypted-cookie-middleware`.

Reading Cookies
---------------

Once you have a ``CookieCollection`` instance, you can access the cookies it
contains::

    // Check if a cookie exists
    $cookies->has('remember_me');

    // Get the number of cookies in the collection
    count($cookies);

    // Get a cookie instance
    $cookie = $cookies->get('remember_me');

Once you have a ``Cookie`` object you can interact with it's state and modify
it. Keep in mind that cookies are immutable, so you'll need to update the
collection if you modify a cookie::

    // Get the value
    $value = $cookie->getValue()

    // Access data inside a JSON value
    $id = $cookie->read('User.id');

    // Check state
    $cookie->isHttpOnly();
    $cookie->isSecure();

.. versionadded:: 3.5.0
    ``CookieCollection`` and ``Cookie`` were added in 3.5.0.

.. meta::
    :title lang=en: Request and Response objects
    :keywords lang=en: request controller,request parameters,array indexes,purpose index,response objects,domain information,request object,request data,interrogating,params,previous versions,introspection,dispatcher,rout,data structures,arrays,ip address,migration,indexes,cakephp,PSR-7,immutable
