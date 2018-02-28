3.5 Gabay sa Paglipat
###################

Ang CakePHP 3.5 ay isang API na tugma na pag-upgrade mula 3.4. Binabalangkas ng pahinang ito ang
mga pagbabago at pagpapahusay na ginawa sa 3.5.

Upang mag-upgrade sa 3.5.x patakbuhin ang sumusunod na composer na utos:

.. code-block:: bash

    php composer.phar require --update-with-dependencies "cakephp/cakephp:3.5.*"

Mga Deprecation
============

Ang sumusunod ay isang listahan ng mga hindi na ginagamit na pamamaraan, katangian at paggawa. Ang mga tampok nito ay patuloy na gagana hanggang 4.0.0 at pagkatapos ay aalisin.

* ``Cake\Http\Client\CookieCollection`` ay hindi na ginagamit. Sa halip gamitin ang
  ``Cake\Http\Cookie\CookieCollection``.
* ``Cake\View\Helper\RssHelper`` ay hindi na ginagamit. Dahil sa bihira na paggamit ng
  Hindi na ginagamit ang RssHelper.
* ``Cake\Controller\Component\CsrfComponent`` ay hindi na ginagamit. Sa halip gamitin ang
  :ref:`csrf-middleware`.
* ``Cake\Datasource\TableSchemaInterface`` ay hindi na ginagamit. Sa halip gamitin ang
  ``Cake\Database\TableSchemaAwareInterface``.
* ``Cake\Console\ShellDispatcher`` ay hindi na ginagamit. Sa halip dapat na i-update ang mga aplikasyon gamitin ang ``Cake\Console\CommandRunner``.
* ``Cake\Database\Schema\TableSchema::column()`` ay hindi na ginagamit. Sa halip gamitin ang
  ``Cake\Database\Schema\TableSchema::getColumn()``.
* ``Cake\Database\Schema\TableSchema::constraint()``ay hindi na ginagamit. Sa halip gamitin ang
  ``Cake\Database\Schema\TableSchema::getConstraint()``.
* ``Cake\Database\Schema\TableSchema::index()`` ay hindi na ginagamit. Sa halip gamitin ang
  ``Cake\Database\Schema\TableSchema::getIndex()``.

Hindi na ginagamit na mga Pinagsamang Get/Set na pamaraan
-----------------------------------

Sa nakalipas na CakePHP ay gumagamit ng 'modal' na mga pamamaraan na nagbibigay ng pareho
isang mode ng get at set. Ang mga pamamaraan na ito ay kumplikado ng IDE autocompletion at ang aming kakayahan upang magdagdag ng mga mahigpit na return type sa hinaharap. Para sa mga kadahilanang ito, pinagsama ang mga pamamaraan ng get/set ay nahahati sa mga hiwalay na paraan ng get at set.

Ang sumusunod ay isang listahan ng mga pamamaraan na hindi na ginagamit at pinalitan ng
``getX()`` at ``setX()`` na pamaraan:

``Cake\Cache\Cache``
    * ``config()``
    * ``registry()``
``Cake\Console\Shell``
    * ``io()``
``Cake\Console\ConsoleIo``
    * ``outputAs()``
``Cake\Console\ConsoleOutput``
    * ``outputAs()``
``Cake\Database\Connection``
    * ``logger()``
``Cake\Database\TypedResultInterface``
    * ``returnType()``
``Cake\Database\TypedResultTrait``
    * ``returnType()``
``Cake\Database\Log\LoggingStatement``
    * ``logger()``
``Cake\Datasource\ModelAwareTrait``
    * ``modelType()``
``Cake\Database\Query``
    * getter bahagi ng ``valueBinder()`` (ngayon ay ``getValueBinder()``)
``Cake\Database\Schema\TableSchema``
    * ``columnType()``
``Cake\Datasource\QueryTrait``
    * getter bahagi ng ``eagerLoaded()`` (ngayon ay ``isEagerLoaded()``)
``Cake\Event\EventDispatcherInterface``
    * ``eventManager()``
``Cake\Event\EventDispatcherTrait``
    * ``eventManager()``
``Cake\Error\Debugger``
    * ``outputAs()`` (ngayon ay ``getOutputFormat()`` / ``setOutputFormat()``)
``Cake\Http\ServerRequest``
    * ``env()`` (ngayon ay ``getEnv()`` / ``withEnv()``)
    * ``charset()`` (ngayon ay ``getCharset()`` / ``withCharset()``)
``Cake\I18n\I18n``
    * ``locale()``
    * ``translator()``
    * ``defaultLocale()``
    * ``defaultFormatter()``
``Cake\ORM\Association\BelongsToMany``
    * ``sort()``
``Cake\ORM\LocatorAwareTrait``
    * ``tableLocator()``
``Cake\ORM\EntityTrait``
    * ``invalid()`` (ngayon ay ``getInvalid()``, ``setInvalid()``,
      ``setInvalidField()``, at ``getInvalidField()``)
``Cake\ORM\Table``
    * ``validator()``
``Cake\Routing\RouteBuilder``
    * ``extensions()``
    * ``routeClass()``
``Cake\Routing\RouteCollection``
    * ``extensions()``
``Cake\TestSuite\TestFixture``
    * ``schema()``
``Cake\Utility\Security``
    * ``salt()``
``Cake\View\View``
    * ``template()``
    * ``layout()``
    * ``theme()``
    * ``templatePath()``
    * ``layoutPath()``
    * ``autoLayout()`` (ngayon ay ``isAutoLayoutEnabled()`` / ``enableAutoLayout()``)

Pagbabago ng Pag-uugali
================

Habang ang mga pagbabagong ito ay tugma sa API, kinakatawan nila ang mga maliit na pagbabago ng pag-uugali na maaaring makaapekto sa iyong aplikasyon:

* ``BehaviorRegistry``, ``HelperRegistry`` at ``ComponentRegistry`` ay magtataas ngayon ng mga eksepsiyon kung kailan ``unload()`` ay tinatawag na isang hindi kilalang pangalan ng bagay. Ang pagbabagong ito ay makatulong na makahanap ang mga mali na mas madali sa pamamagitan ng paggawa ng posibleng mga typo na mas nakikita
* ``HasMany`` na asosasyon ngayon ay maganda na pinangangasiwaan ang mga walang laman na halaga na itinakda para sa katangian ng asosasyon, katulad ng ``BelongsToMany`` na mga asosasyon - na tinatrato nila ang ``false``, ``null``, at walang laman na mga string sa parehong paraan tulad ng mga walang laman na mga array. Para sa
  ``HasMany`` na mga asosasyon na ito ngayon ay nagreresulta sa lahat ng nauugnay na mga rekord upang tinanggal/mai-unlink kapag ang ``replace`` na diskarte sa pag-save ang ginagamit.
  Ang resulta nito ay nagbibigay-daan sa iyo upang gumamit ng mga form upang tanggalin/i-unlink ang lahat ng nauugnay na mga rekord sa pamamagitan ng pagpasa ng isang walang laman na string. Noon ito ay nangangailangan ng pasadya na marshalling logic.
* ``ORM\Table::newEntity()`` ngayon ay nagpapahiwatig lamang ng mga kaugnayan ng mga katangiang marumi kung ang rekord ng marshalled na kaugnayan ay marumi. Sa mga sitwasyon kung saan nilikha ang isang kaugnayan ng entity na naglalaman ng walang katangian ang walang laman na rekord ay hindi mai-flag para sa pagtitiyaga.
* ``Http\Client`` hindi na gumagamit ng ``cookie()`` mga resulta ng pamamaraan kapag gumagawa ng mga kahilingan. Sa halip ang paggamit ng ``Cookie`` na header at internal na CookieCollection. Ito ay dapat lamang iepekto ng mga aplikasyon na may pasadya na HTTP adapter sa kanilang mga kliyente.
* Ang Multi-word na mga pangalan ng subcommand ay dati kinakailangan ang camelBacked na pangalan na gagamitin kapag nag-invoke ng mga shell. 
Ngayon ang subcommands maaaring mahihingi sa underscored_names.
  Halimbawa: ``cake tool initMyDb`` maaari na ngayong tawagan ``cake tool init_my_db``. Kung ang iyong mga shell dati ay nakatali dalawang subcommands na may iba't ibang mga pagbabago, tanging ang huling bound command ay gagana.
* ``SecurityComponent`` ay mag-blackhole ng mga post request ng na walang datos ng kahilingan ngayon. Ang pagbabagong ito ay tumutulong na protektahan ang mga aksyon na lumikha ng mga talaan gamit ang mga database default na nag-iisa.
* ``Cake\ORM\Table::addBehavior()`` at ``removeBehavior()`` ngayon ay magbabalik ng
  ``$this`` upang makatulong sa pagtukoy ng mga bagay sa talahanayan sa isang matatas na paraan..
* Ang Cache Engine ay hindi na magbibigay ng isang eksepsyon kapag nabigo sila o mali ang pagkompigura, ngunit sa halip ay bumabalik sa noop ``NullEngine``. Ang mga pagbagsak ay maaari ring :ref:`configured <cache-configuration-fallback>` sa isang per-engine na batayan.
* ``Cake\Database\Type\DateTimeType`` ay magsaayos ngayon ng mga string ng datetime na naka-format ng ISO-8859-1 (e.g. 2017-07-09T12:33:00+00:02) bilang karagdagan sa naunang tinanggap na format. Kung mayroon kang isang subclass ng DateTimeType maaaring kailangan mong i-update ang iyong code.

Mga Bagong Tamppok
============

Pakay ng Middleware
-----------------

Ang Middleware ay maaari na ngayong maipahintulot sa mga ruta sa mga tiyak na pakay ng URL. Ito ay nagpapahintulot sa iyo na bumuo ng mga tukoy na stack ng middleware para sa iba't ibang bahagi ng iyong aplikasyon nang hindi kinakailangang sumulat ng URL checking code sa iyong middleware. Tingnan ang :ref:`connecting-scoped-middleware` na seksyon para sa karagdagang impormasyon.

Bagong Console Runner
------------------

3.5.0 adds ``Cake\Console\CommandRunner``. Ang class na ito kasama ang
``Cake\Console\CommandCollection`` pagsamahin ang CLI na environment gamit ang bagong class ng ``Application``. Application na mga class maaari na ngayong magpatupad ng ``console ()`` hook na nagpapahintulot sa kanila na magkaroon ng ganap na kontrol sa kung aling mga CLI na utos ang nailantad, kung paano sila pinangalanan at kung paano makuha ng mga shell ang kanilang mga dependency. Ang pagsang-ayon sa bagong class na ito ay nangangailangan ng pagpapalit ng mga nilalaman ng iyong ``bin/cake.php`` file gamit ang `sumusunod na file <https://github.com/cakephp/app/tree/3.next/bin/cake.php>`_.

Kahinaan ng Cache Engine 
----------------------

Cache engines maaari na ngayong ikompigura gamit ang isang ``fallback` key na tumutukoy sa isang kompigurasyon ng cache upang bumalik sa kung ang engine ay maling nakompigura (o hindi magagamit). Tingnan ang :ref:`cache-configuration-fallback` para sa karagdagang impormasyon sa pagkompigura ng mga kahinaan.

dotenv Support idinagdag sa Application Skeleton
--------------------------------------------

Ang application skeleton ngayon ay nagtatampok na pagsasama ng 'dotenv' na ginagawang mas madali gamitin ang mga environment na variable upang ikompigura ang iyong aplikasyon. Tingnana ang :ref:`environment-variables` seksyon para sa karagdagang impormasyon.

Pagsubok ng Pagsasama ng Console
---------------------------

Ang ``Cake\TestSuite\ConsoleIntegrationTestCase`` na class ay idinagdag upang gawing mas madali ang integration testing console. Para sa karagdagang impormasyon, bisitahin ang :ref:`console-integration-testing` na seksyon. Ang test class na ito ay ganap na katugma sa kasalukuyang ``Cake\Console\ShellDispatcher`` pati na rin ang bagong ``Cake\Console\CommandRunner``.

Koleksyon
----------

* ``Cake\Collection\Collection::avg()`` ay idinagdag.
* ``Cake\Collection\Collection::median()`` ay idinagdag.

Core
----

* ``Cake\Core\Configure::read()`` ay sinusuportahan na ngayon ng mga default na halaga kung wala ang ninanais na key.
* ``Cake\Core\ObjectRegistry`` ngayon ay nagpapatupad ng ``Countable`` at
  ``IteratorAggregate`` mga interface.

Console
-------

* ``Cake\Console\ConsoleOptionParser::setHelpAlias()`` ay idinagdag. Ang pamamaraang ito ay nagpapahintulot sa iyo na itakda ang pangalan ng utos na ginagamit kapag bumubuo ng resulta ng tulong. Defaults sa ``cake``.
* ``Cake\Console\CommandRunnner`` ay idinagdag pinalitan ng
  ``Cake\Console\ShellDispatcher``.
* ``Cake\Console\CommandCollection`` ay idinagdag upang magbigay ng isang interface para sa mga aplikasyon upang tukuyin ang mga tool sa command line na kanilang inaalok.

Database
--------

* Ang SQLite na driver ay may idinagdag na ``mask`` na opsyon. Hinahayaan ka ng pagpipiliang ito na itakda mo ang mga pahintulot ng file sa SQLite database file kapag nilikha ito.

Datasource
----------

* ``Cake\Datasource\SchemaInterface`` ay idinagdag.
* Bagong mga uri ng abstract ay idinagdag sa ``smallinteger`` at ``tinyinteger``.
  Ang umiiral ``SMALLINT`` at ``TINYINT`` na mga kolum ay makikita ngayon bilang mga bagong abstract na uri. ``TINYINT(1)`` mga kolum ay patuloy na itinuturing bilang boolean na kolum sa MySQL.
* ``Cake\Datasource\PaginatorInterface`` ay idinagdag. Ang ``PaginatorComponent`` ay gumagamit na ngayon ng interface na ito upang makipag-ugnay sa mga paginator. Nagbibigay-daan ito sa iba pang mga pagpapatupad na tulad ng ORM na paginated ng bahagi.
* ``Cake\Datasource\Paginator`` ay idinagdag upang i-paginate ang ORM/Database Query na mga instance.

Event
-----

* ``Cake\Event\EventManager::on()`` at ``off()`` ang mga pamamaraan ay chainable ginagawa itong mas simple upang magtakda ng maraming mga kaganapan nang sabay-sabay.

Http
----

* Bagong ``Cookie`` & ``CookieCollection`` na class ay idinagdag. Ang mga class ay nagbibigay-daan sa iyo upang gumana sa cookies sa isang object-orientated na paraan, at magagamit sa ``Cake\Http\ServerRequest``, ``Cake\Http\Response``, at
  ``Cake\Http\Client\Response``. Tingnan ang :ref:`request-cookies` at
  :ref:`response-cookies` para sa karagdagang impormasyon.
* Bagong middleware ay idinagdag upang gawing mas madali ang pag-apply ng mga header ng seguridad. Tingnan ang :ref:`security-header-middleware` para sa karagdagang impormasyon.
* New middleware ay idinagdag sa pagpapakita ng pag-encrypt ng datos ng cookie. Tingnan ang :Bagong:`encrypted-cookie-middleware` para sa karagdagang impormasyon.
* Bagong middleware  ay idinagdag upang gawing mas madali ang pagprotekta laban sa CSRF. Tingnan ang :ref:`csrf-middleware` para sa karagdagang impormasyon.
* ``Cake\Http\Client::addCookie()`` ay idinagdag upang gawing madali upang magdagdag ng cookies sa isang kliyente na instance.

InstanceConfigTrait
-------------------

* ``InstanceConfigTrait::getConfig()`` now takes a 2nd parameter ``$default``.
  If no value is available for the specified ``$key``, the ``$default`` value
  will be returned.

ORM
---

* ``Cake\ORM\Query::contain()`` now allows you to call it without the wrapping
  array when containing a single association. ``contain('Comments', function ()
  { ... });`` will now work. This makes ``contain()`` consistent with other
  eagerloading related methods like ``leftJoinWith()`` and ``matching()``.

Routing
-------

* ``Cake\Routing\Router::reverseToArray()`` was added. This method allow you to
  convert a request object into an array that can be used to generate URL
  strings.
* ``Cake\Routing\RouteBuilder::resources()`` had the ``path`` option
  added. This option lets you make the resource path and controller name not
  match.
* ``Cake\Routing\RouteBuilder`` now has methods to create routes for
  specific HTTP methods. e.g ``get()`` and ``post()``.
* ``Cake\Routing\RouteBuilder::loadPlugin()`` was added.
* ``Cake\Routing\Route`` now has fluent methods for defining options.

TestSuite
---------

* ``TestCase::loadFixtures()`` will now load all fixtures when no arguments are
  provided.
* ``IntegrationTestCase::head()`` was added.
* ``IntegrationTestCase::options()`` was added.
* ``IntegrationTestCase::disableErrorHandlerMiddleware()`` was added to make
  debugging errors easier in integration tests.

Validation
----------

* ``Cake\Validation\Validator::scalar()`` was added to ensure that fields do not
  get non-scalar data.
* ``Cake\Validation\Validator::regex()`` was added for a more convenient way
  to validate data against a regex pattern.
* ``Cake\Validation\Validator::addDefaultProvider()`` was added. This method
  lets you inject validation providers into all the validators created in your
  application.
* ``Cake\Validation\ValidatorAwareInterface`` was added to define the methods
  implemented by ``Cake\Validation\ValidatorAwareTrait``.

View
----

* ``Cake\View\Helper\PaginatorHelper::limitControl()`` was added. This method
  lets you create a form with a select box for updating the limit value on
  a paginated result set.
