Hiling at Tugon na mga Object
#############################

.. php:namespace:: Cake\Http

Ang hiling at tugon na mga object ay nagbibigay ng isang abstraksyon na umiikot sa HTTP na mga hiling at
mga tugon. Ang hiling na object sa CakePHP ay nagpapahintulot sa ito na mag-introspect sa isang papasok
na hiling, habang ang tugon na ay nagpapahintulot sa iyo na walang kahirap-hirap na paglikha ng HTTP
na mga tugon mula sa iyong mga controller.

.. index:: $this->request
.. _cake-request:

Hiling
======

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

Sesyon na Datos
---------------

Para ma-access ang sesyon para sa ibinigay na hiling na ginamit sa ``session()`` na pamamaraan::

    $userName = $this->request->session()->read('Auth.User.name');

Para sa karagdagangn impormasyon, tingnan ang :doc:`/development/sessions` na dokumentasyon para sa kung papaano
gamitin ang sesyon na object.

Host at Domain na Pangalan
--------------------------

.. php:method:: domain($tldLength = 1)

Binabalik ang domain na pangalan sa iyong aplikasyon na pinatakbo sa::

    // Nagpapakita ng 'example.org'
    echo $request->domain();

.. php:method:: subdomains($tldLength = 1)

Binabalik ang mga subdomain sa iyong aplikasyon na iyong pinatakbo bilang isang array::

    // Binabalik ng ['my', 'dev'] para sa 'my.dev.example.org'
    $subdomains = $request->subdomains();

.. php:method:: host()

Binabalik sa host ang iyong aplikasyon sa::

    // Nagpapakita ng 'my.dev.example.org'
    echo $request->host();

Pagbabasa ng HTTP na Pamaraan
-----------------------------

.. php:method:: getMethod()

Binabalik ang HTTP na pamamaraan ang hiling na ginagawa sa::

    // Output ng POST
    echo $request->getMethod();

    // Bago ang 3.4.0
    echo $request->method();

Paghihigpit na kung saan ang HTTP na pamaraan ay isang Aksyon na Tinatanggap
----------------------------------------------------------------------------

.. php:method:: allowMethod($methods)

Itakda ang pinapayagan na HTTP na mga pamaraan. Kung hindi tumugma, ito ay nagtatapon ng
``MethodNotAllowedException``. Ang 405 na sagot ay magdaragdag ng kinakailangan na
``Allow`` na header na may naipasa na mga pamamaraan::

    public function delete()
    {
        // Tinatanggap lamang ang POST at DELETE na mga hiling
        $this->request->allowMethod(['post', 'delete']);
        ...
    }

Pagbabasa ng HTTP na mga Header
-------------------------------

Nagpapahintulot sa iyo na i-access ang anuman sa ``HTTP_*`` na mga header na iyong ginamit
para sa hiling. Halimbawa::

    // Kunin ang header bilang isang string
    $userAgent = $this->request->getHeaderLine('User-Agent');

    // Kunin ang isang array sa lahat ng mga halaga.
    $acceptHeader = $this->request->getHeader('Accept');

    // Suriin kung ang isang header ay umiiral
    $hasAcceptHeader = $this->request->hasHeader('Accept');

    // Bago ang 3.4.0
    $userAgent = $this->request->header('User-Agent');

Habang ang ilang apache na naka-install ay hindi makagawa ng ``Authorization`` na header na mapupuntahan,
Ang CakePHP ay gagawin itong magagamit sa pamamagitan ng apache na tiyak na mga pamamaraan bilang kinakailangan.

.. php:method:: referer($local = false)

Binabalik ang nagre-refer sa address para sa hiling.

.. php:method:: clientIp()

Binabalik ang kasalukuyang IP address ng bumisita.

Pagtitiwala sa Proxy na mga Header
----------------------------------

Kung ang iyong aplikasyon ay sa likod ng isang load balancer o tumatakbo sa isang cloud na serbisyo, ikaw
ay madalas na makakuha ng load balancer na host, port at scheme sa iyong mga hiling. Madalas
ng load ng mga balander ay nagpapadala din ng ``HTTP-X-Forwarded-*`` na mga header na may orihinal
na mga halaga. Ang naipasa na mga header ay hindi magagamit sa CakePHP sa labas ng kahon. Upang
makuha ang hiling na object gumagamit nitong mga header set na ``trustProxy`` na katangian sa
``true``::

    $this->request->trustProxy = true;

    // Ito mga pamamaraan ay nagpapahintulot sa iyo na gumamit ng naka-proxy na mga header.
    $port = $this->request->port();
    $host = $this->request->host();
    $scheme = $this->request->scheme();
    $clientIp = $this->request->clientIp();

Pagsusuri sa Tinanggap na mga Header
------------------------------------

.. php:method:: accepts($type = null)

Malaman kung ano ang nilalaman ng mga uri sa kliyenteng tinatanggap, o suriin kung ito ay tumatanggap ng
isang partikular na uri ng nilalaman.

Kunin ang lahat ng mga uri::

    $accepts = $this->request->accepts();

Suriin para sa isang solong uri::

    $acceptsJson = $this->request->accepts('application/json');

.. php:method:: acceptLanguage($language = null)

Kunin ang lahat ng mga language na tinatanggap sa kliyente,
o suriin kung ang tiyak na language ay tinatanggap.

Kunin ang listahan sa tinatanggap na mga language::

    $acceptsLanguages = $this->request->acceptLanguage();

Suriin kung ang isang tiyak na language ay tinatanggap::

    $acceptsSpanish = $this->request->acceptLanguage('es-es');

.. _request-cookies:

Mga Cookie
----------

Hiling na mga cookie ay maaaring mabasa gamit ang isang bilang ng mga pamamaraan::

    // Kunin ang cookie na halaga, o null kung ang cookie ay nawawala.
    $rememberMe = $this->request->getCookie('remember_me');

    // Basahin ang halaga, o kunin ang default sa 0
    $rememberMe = $this->request->getCookie('remember_me', 0);

    // Kunin ang lahat ng mga cookie bilang isang hash
    $cookies = $this->request->getCookieParams();

    // Kunin ang CookieCollection na instance (na nagsisimula sa 3.5.0)
    $cookies = $this->request->getCookieCollection()

Tingnan ang :php:class:`Cake\\Http\\Cookie\\CookieCollection` na dokumentasyon para sa kung paano
ipagana gamit ang koleksyon ng cookie.

.. versionadded:: 3.5.0
    ``ServerRequest::getCookieCollection()`` was added in 3.5.0

.. index:: $this->response

Tugon
=====

.. php:class:: Response

Ang :php:class:`Cake\\Http\\Response` ay isang default na tugon na class sa CakePHP.
Ito ay nagpapaikut-ikot sa bilang ng mga tampok atfunctionality para bumuo ng HTTP
na mga tugon sa iyong aplikasyon. Ito rin ay tumutulong sa pagsusubok, dahil maaari itong
naka-mock/naka-stubb na nagpapahintulot sa iyo na siyasatin ang mga header na maipapadala.
Tulad ng :php:class:`Cake\\Http\\ServerRequest`, :php:class:`Cake\\Http\\Response`
nagsasama ng bilang ng mga pamamaraan na dati ay nakita sa :php:class:`Controller`,
:php:class:`RequestHandlerComponent` at :php:class:`Dispatcher`. Ang lumang
mga pamamaraan ay hindi na magagamit sa pabot sa pagagamit sa :php:class:`Cake\\Http\\Response`.

``Response`` ay nagbibigay ng isang interface para balutin ang karaniwang tugon na may kaugnayad
na mga gawain tulad sa:

* Pagpapadala ng mga header para sa mga nagre-redirect.
* Pagpapadala ng nilalaman na uri ng mga header.
* Pagpapadala sa anumang header.
* Pagpapadala ng tugon sa katawan.

Pakikitungo na may nilalaman na mga uri
---------------------------------------

.. php:method:: withType($contentType = null)

Maaari kang makakontrol ng Content-Type sa iyong mga tugon sa aplikasyon na may
:php:meth:`Cake\\Http\\Response::withType()`. Kung ang iyong aplikasyon ay nangangailangan na magkasundo
na may nilalaman na mga uri na hindi itinayo sa Tugon, maaari kang makamapa sa kanila na may
``type()`` din::

    // Magdagdag ng isang vCard na uri
    $this->response->type(['vcf' => 'text/v-card']);

    // Itakda ang tugon na Content-Type sa vcard.
    $this->response = $this->response->withType('vcf');

    // Bago ang 3.4.0
    $this->response->type('vcf');

Karaniwan, gusto mo magmapa sa karagdagang nilalaman na mga uri sa iyong controller sa
:php:meth:`~Controller::beforeFilter()` na callback, maaari kang gumamit ng
awtomatiko na view na lumilipat na mga tampok sa :php:class:`RequestHandlerComponent` kung ikaw
ay gumgamit nito.

.. _cake-response-file:

Pagpapadala ng mga File
-----------------------

.. php:method:: withFile($path, $options = [])

May mga oras na gusto mong mapadala ng mga file bilang mga tugon para sa iyong mga hiling.
Maaari mong maisagawa ito sa pamamagitan ng paggamit sa :php:meth:`Cake\\Http\\Response::withFile()`::

    public function sendFile($id)
    {
        $file = $this->Attachments->getFile($id);
        $response = $this->response->withFile($file['path']);
        // Binalik ang tugon upang maiwasan ang controller mula sa sinusubukang i-render
        // ang isang view.
        return $response;
    }

    // Bago ang 3.4.0
    $file = $this->Attachments->getFile($id);
    $this->response->file($file['path']);
    // Binalik ang tugon upang pigilan ang kontroller mula sa sinusubukang i-render
    // ang isang view.
    return $this->response;

Tulad ng ipinakita sa itaas na halimbawa, kailangan mong magpasa sa file na path sa pamamaraan.
Ang CakePHP ay nagpapadala ng isang tamang nilalaman na uri ng header kung ito ay kilala na file na uri na naka-lista
sa `Cake\\Http\\Reponse::$_mimeTypes`. Maaari kang magdagdag ng bagong mga uri bago sa pagtawag sa
:php:meth:`Cake\\Http\\Response::withFile()` sa pamamagitan sa paggamit ng
:php:meth:`Cake\\Http\\Response::withType()` na pamamaraan.

Kung gusto mo, maaari ka ding pilitin ang isang file na ma-download sa halip na ipinapakita sa
browser sa pamamagitan ng pagtutukoy sa mga opsyon::

    $response = $this->response->withFile(
        $file['path'],
        ['download' => true, 'name' => 'foo']
    );

    // Bago ang 3.4.0
    $this->response->file(
        $file['path'],
        ['download' => true, 'name' => 'foo']
    );

Ang suportado na mga opsyon ay:

pangalan
    Ang pangalan ay nagpapahintulot sa iyo ng alternatibong file na pangalan na ipapadala sa
    gumagamit.
download
    Isang boolean na halaga na nagpapahiwatig kung ang mga header ay dapat itakda upang pilitin
    ang download.

Pagpapadala ng isang String bilang File
---------------------------------------

Maaari kang tumugon na may isang file na hindi umiiral sa disk, tulad ng isang pdf o isang
ics na binuo sa nauna mula sa isang string::

    public function sendIcs()
    {
        $icsString = $this->Calendars->generateIcs();
        $response = $this->response;
        $response->body($icsString);

        $response = $response->withType('ics');

        // Opsyonal na pinilit ang pag-download ng file
        $response = $response->withDownload('filename_for_download.ics');

        // Bumalik na tugon ng object upang pigilan ang controller mula sa sinusubukan pag-render
        // sa isang .
        return $response;
    }

Mga callback ay maaari ring bumalik ang katawan bilang isang string::

    $path = '/some/file.png';
    $this->response->body(function () use ($path) {
        return file_get_contents($path);
    });

Pagtatakda ng mga Header
------------------------

.. php:method:: withHeader($header, $value)

Pagtatakda ng mga header ay nagawa na may :php:meth:`Cake\\Http\\Response::withHeader()`
na pamamaraan. Tulad ng lahat sa PSR-7 na interface na mga pamamaraan, itong pamaraan ay bumabalik ng *new*
instance na may bagong header::

    // Pagdagdag/pagpalit ng isang header
    $response = $response->withHeader('X-Extra', 'My header');

    // Magtakda ng maraming mga header
    $response = $response->withHeader('X-Extra', 'My header')
        ->withHeader('Location', 'http://example.com');

    // Ilagay ang isang halaga para sa isang umiiral na header
    $response = $response->withAddedHeader('Set-Cookie', 'remember_me=1');

    // Bago ang 3.4.0 - Itakda ang isang header
    $this->response->header('Location', 'http://example.com');

Mga headers ay hindi pinadala kung naitakda. Sa halip, sila ay gaganapin hanggang ang tugon ay
napalabas sa pamamagitan ng ``Cake\Http\Server``.

Maaari mo na ngayong gumamit ng kaginhawaan na pamamaraan sa
:php:meth:`Cake\\Http\\Response::withLocation()` upang direkta na itakda o kunin ang
pag-redirect na lokasyon na header.

Pagtatakda sa Katawan
---------------------

.. php:method:: withStringBody($string)

Ihanda ang isang string bilang tugon na katawan, gawin ang mga sumusunod::

    // Itakda ang string sa katawan
    $response = $response->withStringBody('My Body');

    // Kung gusto mo ng isang json na tugon
    $response = $response->withType('application/json')
        ->withStringBody(json_encode(['Foo' => 'bar']));

.. versionadded:: 3.4.3
    ``withStringBody()`` ay idinagdag sa 3.4.3

.. php:method:: withBody($body)

Upang itakda ang tugon na katawan, gamitin ang ``withBody()`` na pamamaraan, na ibinigay sa pamamagitan sa
:php:class:`Zend\\Diactoros\\MessageTrait`::

    $response = $response->withBody($stream);

    // Bago ang 3.4.0 - Itakda ang Katawan
    $this->response->body('My Body');

Siguraduhin na ang ``$stream`` ay isang :php:class:`Psr\\Http\\Message\\StreamInterface` na object.
Tingmam sa ibaba kung papaano lumikha ng isang bagong stream.

Maaari mo ring mag-stream ng mga tugon mula sa mga file na gamit ang :php:class:`Zend\\Diactoros\\Stream` na mga stream::

    // Upang i-stream mula sa file
    use Zend\Diactoros\Stream;

    $stream = new Stream('/path/to/file', 'rb');
    $response = $response->withBody($stream);

Maaaring mo ring i-strean ang mga tugon mula sa isang callback gamit ang ``CallbackStream``. Ito
ay magagamit kapag ikaw ay mayroong mga mapagkukunan tulad ng mga imahe, mga CSV file o mga PDF na kailangan mo
i-stream sa kliyente::

    // Pag-stream mula sa isang callback
    use Cake\Http\CallbackStream;

    // Lumikha ng imahe.
    $img = imagecreate(100, 100);
    // ...

    $stream = new CallbackStream(function () use ($img) {
        imagepng($img);
    });
    $response = $response->withBody($stream);

    // Bago ang 3.4.0 maaari kang gumgamit sa sumusunod upang lumikha ng pag-stream na mga tugon.
    $file = fopen('/some/file.png', 'r');
    $this->response->body(function () use ($file) {
        rewind($file);
        fpassthru($file);
        fclose($file);
    });

Pagtatakda ng Character Set
---------------------------

.. php:method:: withCharset($charset)

Sets the charset that will be used in the response::

    $this->response = $this->response->withCharset('UTF-8');

    // Bago ang 3.4.0
    $this->response->charset('UTF-8');

Interacting with Browser Caching
--------------------------------

.. php:method:: withDisabledCache()

Kung minsan kailangan mong pilitin ang mga browser na hindi mag-cache ng mga resulta sa isang controller
na aksyon. :php:meth:`Cake\\Http\\Response::withDisabledCache()` ay nilayon para lamang
sa ganun::

    public function index()
    {
        // Hindi pinagana ang pag-cache
        $this->response = $this->response->withDisabledCache();

        // Bago ang 3.4.0
        $this->response->disableCache();
    }

.. warning::

    Ang hindi pagpagana sa pag-cache mula sa SSL na mga domain habang sinusubukang ipadala
    ang mga file sa Internet Explorer ay maaaring magresulta ng mga pagkakamali.

.. php:method:: withCache($since, $time = '+1 day')

Maaari mo ring sabihin ang mga kliyente na gusto mo sa kanila upang i-cache ang mga tugon. Sa pamamagitan sa paggamit
ng :php:meth:`Cake\\Http\\Response::withCache()`::

    public function index()
    {
        // Pagpagana sa pag-cache
        $this->response = $this->response->withCache('-1 minute', '+5 days');
    }

Ang sabi sa itaas sa mga kliyente upang i-cache ang resultang tugon hanggang sa limang araw,
sana ay mapadali ang karanasan ng iyong mga binisita.
Ang ``withCache()`` na pamamaraan ay nagtatakda ng ``Last-Modified`` na halaga para sa unang
argumento. ``Expires`` na header at ang ``max-age`` na direktiba ay nakabatay batay sa
pangalawang parameter. Ang pag-cache na pagkokontrol ng ``public`` direktiba ay itinakda rin.

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
