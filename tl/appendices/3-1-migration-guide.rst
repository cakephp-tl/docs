3.1 Gabay sa Paglipat
#####################

Ang CakePHP 3.1 ay isang buong API na tumutugmang pag-upgrade mula sa 3.0. Ang 
pahinang ito ay binabalangkas ang mga pagbabago at mga pagpapabuting
ginawa sa 3.1.

Routing
=======

- Ang default na route class ay binago sa ``DashedRoute`` sa ``cakephp/app``
  na repo. Ang iyong kasalukuyang code base ay hindi naapektuhan nito, ngunit
  ito ay nagrerekomenda na gumamit nitong route class mula ngayon.
- Ang name prefix na mga opsyon ay dinagdag sa iba't ibang route builder na mga
  paraan. Tingnan ang :ref:`named-routes` na seksyon para sa karagdagang 
  impormasyon.

Console
=======

- Ang ``Shell::dispatchShell()`` ay hindi na nag-aawtput ng pambating mensahe
  mula sa na ipinadalang shell.
- Ang ``breakpoint()`` na helper function ay dinagdag. Ang function ay nagbibigay
  ng isang snippet ng code na maaaring ilagay sa ``eval()`` upang mag-trigger
  ng isang interaktibong console. Ito ay sobrang nakakatulong kapag ikaw ay
  nagde-debug sa pagsubok na mga kaso, o iba pang CLI na mga iskrip.
- Ang ``--verbose`` at ``--quiet`` console na mga opsyon ay kumukontrol na ngayon
  sa stdout/stderr logging output na mga antas.

Nagdagdag ng Shell na mga Helper
--------------------------------

- Ang console na mga aplikasyon ay maaari na ngayong lumikha ng helper na mga class
  na nag-e-encapsulate ng magagamit muling mga bloke ng awtput na lohika. Tingnan
  ang :doc:`/console-and-shells/helpers` na seksyon para sa karagdagang impormasyon.

RoutesShell
-----------

- Ang RoutesShell ay dinagdag at nagbibigay na ngayon sa iyo ng isang simpleng
  magagamit na CLI interface para sa pagsubok at pag-debug na mga ruta. Tingnan
  ang :doc:`/console-and-shells/routes-shell` na seksyon para sa karagdagang
  impormasyon.

Controller
==========

- Ang sumusunod na Controller na mga katangian ay hindi na magagamit ngayon:

  * layout
  * view - pinalitan ng ``template``
  * theme
  * autoLayout
  * viewPath - pinalitan ng ``templatePath``
  * viewClass - pinalitan ng ``className``
  * layoutPath

  Sa halip ng pagtatakda ng mga katangiang ito sa iyong mga controller, dapat 
  mo silang itakda sa view gamit ang mga paraan na may tumutugmang mga pangalan::
 
    // Sa isang controller, sa halip ng
    $this->layout = 'advanced';

    // Dapat mong gamitin ang
    $this->viewBuilder()->layout('advanced');

Ang mga paraang ito ay dapat matawag pagkatapos mong matukoy kung anong view na class
ang gagamitin ng isang controller/aksyon.

AuthComponent
-------------

- Ang bagong config na opsyon na ``storage`` ay dinagdag. Ito ay naglalaman 
  ng storage class na pangalan na ginagamit ng ``AuthComponent`` upang 
  mag-imbak ng rekord ng user. Bilang default ang ``SessionStorage`` ang ginamit.
  Kung gumagamit ng isang walang estado na authenticator sa halip ay dapat mong 
  i-configure ang ``AuthComponent`` upang gamitin ang ``MemoryStorage``.
- Ang bagong config na opsyon na ``checkAuthIn`` ay dinagdag. Ito ay naglalaman
  ng pangalan ng kaganapan kung saan nagaganap ang pagsusuri ng auth. Bilang 
  default ang ``Controller.startup`` ang ginamit, ngunit maaari mo itong itakda
  sa ``Controller.initialize`` kung gusto mo na ang pagpapatunay ay susuriin muna
  bago mapatakbo ang ``beforeFilter()`` na paraan ng controller.
- Ang mga opsyon na ``scope`` at ``contain`` para sa mga authenticator class
  ay hindi na nagagamit. Sa halip, gamitin ang bagong ``finder`` na opsyon upang
  mag-configure ng isang pasadyang finder na paraan at baguhin ang query na ginamit
  upang maghanap ng isang user doon.
- Ang lohika para sa pagtatakda ng ``Auth.redirect`` na sesyon na variable, na
  ginagamit upang kumuha ng URL na pupuntahan pagkatapos ng login, ay binago. 
  Ito ngayon ay tinatakda lamang kapag sinusubukang mag-access ng isang protektadong
  URL pagkatapos ng login. Sa ilalim ng normal na mga pangyayari, kapag ang isang
  user ay direktang nag-aaccess ng login na pahina, ang ``Auth::redirectUrl()`` ay
  nagsasauli ng halagang itinakda para sa ``loginRedirect`` na config.

FlashComponent
--------------

- Ang ``FlashComponent`` ngayon ay nagsasalansan ng Flash na mga mensahe kapag
  tinakda gamit ang ``set()`` o ``__call()`` na paraan. Ito ay nangangahulugan
  na ang istraktura sa Session para sa naimbak na Flash na mga mensahe ay nabago.

CsrfComponent
-------------

- Ang CSRF cookie expiry time ay maaari na ngayong itakda bilang isang 
  ``strtotime()`` compatible na halaga.
- Ang imbalidong CSRF na mga token ay nagtatapon na ngayon ng isang 
  ``Cake\Network\Exception\InvalidCsrfTokenException`` sa halip ng 
  ``Cake\Network\Exception\ForbiddenException``.

RequestHandlerComponent
-----------------------

- Ang ``RequestHandlerComponent`` ay pinapalitan na ngayon ang layout at template
  base sa na-parse na ekstensyon o ``Accept`` header sa ``beforeRender()`` na 
  callback sa halip ng ``startup()``.
- Ang ``addInputType()`` at ``viewClassMap()`` ay hindi na nagagamit. Dapat mong
  gamitin ang ``config()`` upang baguhin ang kumpigurasyon na datos na ito sa 
  runtime.
- Kapag ang ``inputTypeMap`` o ``viewClassMap`` ay tinukoy sa komponent na 
  mga setting, *io-overwrite* nila ang mga default na mga halaga. Ang
  pagbabagong ito ay ginagawang posible ang pagtanggal ng default na kumpigurasyon.

Network
=======

Http\Kliyente
-------------

- Ang default mime type na ginamit kapag nagpapadala ng mga kahilingan ay nabago.
  Dati ang ``multipart/form-data`` ay palaging ginagamit. Sa 3.1, ang
  ``multipart/form-data`` ay ginagamit lamang kapag ang mga file upload ay nandoon.
  Kapag walang mga file upload, sa halip ang ``application/x-www-form-urlencoded`` ang 
  gagamitin.

ORM
===

Maaari ka na ngayong mag-:ref:`Lazily Eager Load ng mga Asosasyon
<loading-additional-associations>`. Ang tampok na ito ay nagpapahintulot sa
iyo na kondisyonal na mag-load ng karagdagang mga asosasyon sa isang 
hanay ng resulta, entity o koleksyon ng mga entity.

Ang ``patchEntity()`` at ``newEntity()`` na paraan ay sumusuporta na ngayon
ng ``onlyIds`` na opsyon. Ang opsyon na ito ay pinapayagan kang paghigpitan
ang hasMany/belongsToMany na asosasyon na pag-marshall upang gumamit lamang
ng ``_ids`` na listahan. Ang opsyon na ito ay nagde-default ng ``false``.

Query
-----

- Ang ``Query::notMatching()`` ay nadagdag.
- Ang ``Query::leftJoinWith()`` ay nadagdag.
- Ang ``Query::innerJoinWith()`` ay nadagdag.
- Ang ``Query::select()`` ay sumusuporta na ngayon ng ``Table`` at ``Association``
  na mga object bilang mga parameter. Ang mga parameter type na ito ay pipiliin
  ang lahat ng mga column sa ibinigay na table o target na table ng asosasyon na 
  instansya.
- Ang ``Query::distinct()`` ay tumatanggap na ngayon ng isang string upang 
  mag-distinct sa isang solong column.
- Ang ``Table::loadInto()`` ay nadagdag.
- Ang ``EXTRACT``, ``DATE_ADD`` at ``DAYOFWEEK`` na hilaw na mga SQL function
  ay na-abstract sa ``extract()``, ``dateAdd()`` at ``dayOfWeek()``.

View
====

- Maaari mo na ngayong itakda ang ``_serialized`` sa ``true`` para sa
  ``JsonView`` at ``XmlView`` upang i-serialize lahat ang mga view variable
  sa halip ng tahasang pagtukoy sa mga ito.
- Ang ``View::$viewPath`` ay hindi na nagagamit. Sa halip ay dapat mong gamitin
  ang ``View::templatePath()``.
- Ang ``View::$view`` ay hindi na nagagamit. Sa halip ay dapat mong gamitin
  ang ``View::template()``.
- Ang ``View::TYPE_VIEW`` ay hindi na nagagamit. Sa halip ay dapat mong gamitin
  ang ``View::TYPE_TEMPLATE``.

Helper
======

SessionHelper
-------------

- Ang ``SessionHelper`` ay hindi na nagagamit. Maaari mong direktang gamitin
  ang ``$this->request->session()``.

FlashHelper
-----------

- Ang ``FlashHelper`` ay maaaring mag-render ng maramihang mga mensahe kung
  ang maramihang mga mensahe ay tinakda gamit ang ``FlashComponent``.
  Ang bawat mensahe ay mare-render sa kanyang sariling elemento. Ang mga 
  mensahe ay mare-render sa pagkakaayos ng pagkakatakda nila.

FormHelper
----------

- Ang bagong opsyon na ``templateVars`` ay nadagdag. Ang ``templateVars`` ay
  nagpapahintulot sa iyo na magpasa ng karagdagang mga variable sa iyong
  pasadyang form control na mga template.

Email
=====

- Ang ``Email`` at ``Transport`` na mga class ay inilipat sa ilalim ng 
  ``Cake\Mailer`` na namespace. Ang nakaraan nilang mga namespace ay
  magagamit pa rin dahil ang class na mga alias ay itinakda para sa kanila.
- Ang ``default`` email na profile ay awtomatiko na ngayong itatakda kapag
  ang isang ``Email`` na instansya ang nabuo. Ang pagkilos na ito ay
  katulad sa kung ano ang nagawa sa 2.x.

Mailer
------

- Ang ``Mailer`` na class ay nadagdag. Ang class na ito ay tumutulong sa paglikha
  ng nagagamit muling mga email sa isang aplikasyon.

I18n
====

Time
----

- Ang ``Time::fromNow()`` ay nadagdag. Ang paraang ito ay ginagawang mas madali
  ang pagkalkula ng pagkakaiba mula sa 'now'.
- Ang ``Time::i18nFormat()`` ay sumusuporta na ngayon ng hindi gregorian na mga
  kalendaryo kapag nagpo-format ng mga petsa.

Balidasyon
==========

- Ang ``Validation::geoCoordinate()`` ay nadagdag.
- Ang ``Validation::latitude()`` ay nadagdag.
- Ang ``Validation::longitude()`` ay nadagdag.
- Ang ``Validation::isInteger()`` ay nadagdag.
- Ang ``Validation::ascii()`` ay nadagdag.
- Ang ``Validation::utf8()`` ay nadagdag.

Pagsusubok
==========

TestFixture
-----------

Ang ``model`` na key ay suportado na ngayon upang makakuha ng pangalan ng table
para sa pag-import.
