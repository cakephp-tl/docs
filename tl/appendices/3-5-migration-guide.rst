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
  Halimbawa: ``cake tool initMyDb`` maaari na ngayong tawagan ``cake tool
  init_my_db``. If your shells previously bound two subcommands with different
  inflections, only the last bound command will function.
* ``SecurityComponent`` will blackhole post requests that have no request data
  now. This change helps protect actions that create records using database
  defaults alone.
* ``Cake\ORM\Table::addBehavior()`` and ``removeBehavior()`` now return
  ``$this`` to assist in defining table objects in a fluent fashion.
* Cache engines no longer throw an exception when they fail or are misconfigured,
  but instead fall back to the noop ``NullEngine``. Fallbacks can also be
  :ref:`configured <cache-configuration-fallback>` on a per-engine basis.
* ``Cake\Database\Type\DateTimeType`` will now marshal ISO-8859-1 formatted
  datetime strings (e.g. 2017-07-09T12:33:00+00:02) in addition to the
  previously accepted format. If you have a subclass of DateTimeType you may
  need to update your code.

New Features
============

Scoped Middleware
-----------------

Middleware can now be conditionally applied to routes in specific URL
scopes. This allows you to build specific stacks of middleware for different
parts of your application without having to write URL checking code in your
middleware. See the :ref:`connecting-scoped-middleware` section for more
information.

New Console Runner
------------------

3.5.0 adds ``Cake\Console\CommandRunner``. This class alongside
``Cake\Console\CommandCollection`` integrate the CLI environment with the new
``Application`` class. Application classes can now implement a ``console()``
hook that allows them to have full control over which CLI commands are exposed,
how they are named and how the shells get their dependencies. Adopting this new
class requires replacing the contents of your ``bin/cake.php`` file with the
`following file <https://github.com/cakephp/app/tree/3.next/bin/cake.php>`_.

Cache Engine Fallbacks
----------------------

Cache engines can now be configured with a ``fallback`` key that defines a
cache configuration to fall back to if the engine is misconfigured (or
unavailable). See :ref:`cache-configuration-fallback` for more information on
configuring fallbacks.

dotenv Support added to Application Skeleton
--------------------------------------------

The application skeleton now features a 'dotenv' integration making it easier to
use environment variables to configure your application. See the
:ref:`environment-variables` section for more information.

Console Integration Testing
---------------------------

The ``Cake\TestSuite\ConsoleIntegrationTestCase`` class was added to make
integration testing console applications easier. For more information, visit
the :ref:`console-integration-testing` section. This test class is fully
compatible with the current ``Cake\Console\ShellDispatcher`` as well as the new
``Cake\Console\CommandRunner``.

Collection
----------

* ``Cake\Collection\Collection::avg()`` was added.
* ``Cake\Collection\Collection::median()`` was added.

Core
----

* ``Cake\Core\Configure::read()`` now supports default values if the desired key
  does not exist.
* ``Cake\Core\ObjectRegistry`` now implements the ``Countable`` and
  ``IteratorAggregate`` interfaces.

Console
-------

* ``Cake\Console\ConsoleOptionParser::setHelpAlias()`` was added. This method
  allows you to set the command name used when generating help output. Defaults
  to ``cake``.
* ``Cake\Console\CommandRunnner`` was added replacing
  ``Cake\Console\ShellDispatcher``.
* ``Cake\Console\CommandCollection`` was added to provide an interface for
  applications to define the command line tools they offer.

Database
--------

* SQLite driver had the ``mask`` option added. This option lets you set the
  file permissions on the SQLite database file when it is created.

Datasource
----------

* ``Cake\Datasource\SchemaInterface`` was added.
* New abstract types were added for ``smallinteger`` and ``tinyinteger``.
  Existing ``SMALLINT`` and ``TINYINT`` columns will now be reflected as these
  new abstract types. ``TINYINT(1)`` columns will continue to be treated as
  boolean columns in MySQL.
* ``Cake\Datasource\PaginatorInterface`` was added. The ``PaginatorComponent``
  now uses this interface to interact with paginators. This allows other
  ORM-like implementations to be paginated by the component.
* ``Cake\Datasource\Paginator`` was added to paginate ORM/Database Query
  instances.

Event
-----

* ``Cake\Event\EventManager::on()`` and ``off()`` methods are now chainable
  making it simpler to set multiple events at once.

Http
----

* New ``Cookie`` & ``CookieCollection`` classes have been added. These classes allow you
  to work with cookies in an object-orientated way, and are available on
  ``Cake\Http\ServerRequest``, ``Cake\Http\Response``, and
  ``Cake\Http\Client\Response``. See the :ref:`request-cookies` and
  :ref:`response-cookies` for more information.
* New middleware has been added to make applying security headers easier. See
  :ref:`security-header-middleware` for more information.
* New middleware has been added to transparently encrypt cookie data. See
  :ref:`encrypted-cookie-middleware` for more information.
* New middleware has been added to make protecting against CSRF easier. See
  :ref:`csrf-middleware` for more information.
* ``Cake\Http\Client::addCookie()`` was added to make it easy to add cookies to
  a client instance.
