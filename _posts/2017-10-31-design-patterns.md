---
layout: post
title: "[Заметка] Шаблоны проектирования"
date: '2017-10-31'
category:
  - it
  - prog
  - oop
---
<!--more-->
## Фундаментальные шаблоны проектирования

### Шаблон делегирования. Delegation pattern
Делегирование (англ. Delegation) — шаблон, в котором объект внешне выражает некоторое поведение, но в реальности передаёт ответственность за выполнение этого поведения связанному объекту. Для реализации делегирования необходимо, чтобы делегирующий класс содержал ссылку (список ссылок) на класс, которому делегируется выполнение метода. Примуняется в ситуации, когда в разные моменты времени объект должен быть представлен разными подклассами одного и того же класса, то данный объект нельзя представить подклассом этого общего класса.

{% highlight php %}
{% raw %}
class Hand {
	public function shake()
	{
		echo "Shake!";
	}
}

class Person {
	/**
	 * @var Hand
	 */
	private $hand;

	public function __construct(Hand $hand)
	{
		$this->hand = $hand;
	}

	public function shakeHand()
	{
		$this->hand->shake();
	}
}
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
class Hand {
	def shake = println("Shake!")
}

case class Person(hand: Hand) {
	def shakeHand = hand.shake
}
{% endraw %}
{% endhighlight %}

Плюс заключается в возможности перестроения чужих алгоритмов под свои собственные. Минус же в том, что шаблон затрудняет оптимизацию по скорости написания кода в пользу улучшенной чистоты абстракции.

### Шаблон функционального дизайна. Functional design
Функциональный дизайн гарантирует, что каждый модуль программы имеет только одну обязанность и исполняет её с минимумом побочных эффектов на другие части программы. Используется для упрощения проектирования программного обеспечения, функционально разработанные модули имеют предельно низкую связанность. Как правило, это означает, что каждый метод должен выполнять только одно действие, каждый класс проектируется так, чтобы выполнять связанные задачи.

### Неизменяемый интерфейс. Immutable interface
Шаблон гарантирует неизменяемый объект, причем он может быть неизменяемым как полностью, так и частично. В некоторых случаях объект считается неизменяемым с точки зрения пользователя класса, даже если изменяются его внутренние поля. Все внутренние значения должны быть установлены до того, как объект будет использован. Как правило методы установки значений отсутствуют, все свойства инициализируются на этапе конструирования.

{% highlight php %}
{% raw %}
class Foundation {/*_*/}

class House {
	/**
	 * @var Foundation
	 */
	private $foundation;

	public function getFoundation()
	{
		return $this->foundation;
	}
}
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
class Foundation

case class Person(foundation: Foundation)
{% endraw %}
{% endhighlight %}

К плюсам следует отнести то, что упрощается исходный код программы, и ускоряется ее работа, неизменяемый интерфейс позволяет избежать некоторых дорогостоящих операций копирования и сравнения.

### Интерфейс. Interface
Основной шаблон проектирования, являющийся общим способом структурирования программ для того, чтобы их было проще понять. Определяет спецификацию методов, аргументы и тип возвращаемого значения.

{% highlight php %}
{% raw %}
interface Person
{
	/**
	 * @var string $that
	 * @return void
	 */
	public function say($that);
}

class Human implements Person
{
	public function say($that)
	{
		echo($that);
	}
}

class Robot implements Person
{
	public function say($that)
	{
		printr($that);
	}
}
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
 trait Person {
 	def say(that: String): Unit
 }

 class Human extends Person {
 	def say(that: String): Unit = println(that)
 }

 class Robot extends Person {
 	def say(that: String): Unit = println(that.map {_ toByte } mkString)
 }
{% endraw %}
{% endhighlight %}

### Интерфейс-маркер. Marker interface
Шаблон предоставляет возможность связать метаданные (интерфейс) с классом даже при отсутствии в языке явной поддержки для метаданных. В отличие от обычного интерфейса, который определяет функциональность (в виде объявлений методов и свойств), которой должен обладать реализуемый класс объектов, важен сам факт обладания класса маркером. Маркер лишь является признаком наличия определённого поведения у объектов класса, помеченного маркером.

{% highlight php %}
{% raw %}
 interface IReadable {/*_*/}

 function foo(IReadable $readable) {
 	return read($readable);
 }
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
 trait Readable

 def foo(readable: Readable) = read(readable)
{% endraw %}
{% endhighlight %}

Преимуществом использования маркера является возможность внести в языки, не поддерживающие метаданные, дополнительную информацию об особенностях поведения класса. К недостаткам следует отнести то, что интерфейс определяет контракт на реализацию классов, и что контракт наследуется всеми подклассами. Это означает, что вы не можете «отменить лишние реализации» маркером.

### Контейнер свойств. Property container
Позволяет добавлять дополнительные свойства для класса в контейнер (внутри класса), вместо расширения класса новыми свойствами. Предоставляет вместо создания множества свойств, одно, хранящее в себе все остальные. Главный минус - отсутствует строгая типизация.

{% highlight php %}
{% raw %}
 class Hstore {
 	/**
 	 * @var array
 	 */
 	private $data = [];

 	/**
 	 * @param string $key
 	 * @param mixed $value
 	 */
 	public function set($key, $value)
 	{
 		$this->data[$key] = $value;

 		return $this;
 	}

 	/**
 	 * @param string $key
 	 * @return mixed
 	 */
 	public function get($key)
 	{
 		return
 			isset($this->data[$key])
 				? $this->data[$key]
 				: null;
 	}
 }
{% endraw %}
{% endhighlight %}

## Порождающие шаблоны
### Абстрактная фабрика. Abstract factory
Предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не специфицируя их конкретных классов. Шаблон реализуется созданием абстрактного класса Factory, который представляет собой интерфейс для создания компонентов системы. Затем пишутся классы, реализующие этот интерфейс.

{% highlight php %}
{% raw %}
 interface IRim {/*_*/}

 interface ILens {/*_*/}

 interface IGlassesFactory {
 	/**
 	 * @return IRim
 	 */
 	public function createRim();

 	/**
 	 * @return ILens
 	 */
 	public function createLens();
 }

 class PlacticRim implements IRim {/*_*/}

 class PlasticBlackLens implements ILens {/*_*/}

 class SunProtectiveGlassFactory implements IGlassesFactory {
 	public function createRim()
 	{
 		return new PlacticRim;
 	}

 	public function createLens()
 	{
 		return new PlasticBlackLens;
 	}
 }

 class SteelRim implements IRim {/*_*/}

 class GlassLens implements ILens {/*_*/}

 class VisionCorrectionGlassFactory implements IGlassesFactory {
 	public function createRim()
 	{
 		return new SteelRim;
 	}

 	public function createLens()
 	{
 		return new GlassLens;
 	}
 }
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
 trait Rim

 trait Lens

 trait GlassesFactory {
 	def createRim: Rim

 	def createLens: Lens
 }

 class PlacticRim extends Rim

 class PlasticBlackLens extends Lens

 class SunProtectiveGlassFactory extends GlassesFactory {
 	def createRim: Rim = new PlacticRim

 	def createLens: Lens = new PlasticBlackLens
 }

 class SteelRim extends Rim

 class GlassLens extends Lens

 class VisionCorrectionGlassFactory extends GlassesFactory {
 	def createRim: Rim = new SteelRim

 	def createLens: Lens = new GlassLens
 }
{% endraw %}
{% endhighlight %}

Плюсы:
 - Изолирует конкретные классы.
 - Упрощает замену семейств продуктов.
 - Гарантирует сочетаемость продуктов.

К минусам стоит отнести сложность добавления нового типа продуктов.

### Фабричный метод. Factory method
Предоставляет подклассам интерфейс для создания экземпляров некоторого класса. В момент создания наследники могут определить, какой класс создавать. Иными словами, фабрика делегирует создание объектов наследникам родительского класса. Это позволяет использовать в коде программы не специфические классы, а манипулировать абстрактными объектами на более высоком уровне.

{% highlight php %}
{% raw %}
 interface IUrl {
 	/**
 	 * @return string
 	 */
 	public function toString();
 }

 abstract class BaseUrl implements IUrl {
 	/**
 	 * @var string
 	 */
 	private $host;

 	public function __construct($host)
 	{
 		$this->host = $host;
 	}

 	/**
 	 * @return string
 	 */
 	abstract protected function getProtocol();

 	public function toString()
 	{
 		return $this->getProtocol() . '://' . $this->host;
 	}
 }

 class HttpUrl extends BaseUrl {
 	protected function getProtocol()
 	{
 		return 'http';
 	}
 }

 class HttpsUrl extends BaseUrl {
 	protected function getProtocol()
 	{
 		return 'https';
 	}
 }

 interface IUrlFactory {
 	/**
 	 * @param string $action
 	 * @return HttpUrl
 	 */
 	public function createUrl($action);
 }

 class YandexHttpUrlFactory implements IUrlFactory {
 	const HOST = 'yandex.ru';

 	public function createUrl($action)
 	{
 		switch ($action) {
 			case 'payment':
 				return new HttpUrl(self::HOST . '/pay');
 			case 'payout':
 				return new HttpUrl(self::HOST . '/payout');
 			case 'status':
 				return new HttpUrl(self::HOST . '/st');
 			default:
 				return new HttpUrl(self::HOST . '/info');
 		}
 	}
 }

 class YandexHttpsUrlFactory implements IUrlFactory {
 	const HOST = 'yandex.ru';

 	public function createUrl($action)
 	{
 		switch ($action) {
 			case 'payment':
 				return new HttpsUrl(self::HOST . '/pay');
 			case 'payout':
 				return new HttpsUrl(self::HOST . '/payout');
 			case 'status':
 				return new HttpsUrl(self::HOST . '/st');
 			default:
 				return new HttpsUrl(self::HOST . '/info');
 		}
 	}
 }
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
 trait Url

 abstract class BaseUrl(val host: String) extends Url {
 	def getProtocol: String

 	override def toString() = getProtocol + "://" + host
 }

 case class HttpUrl(override val host: String) extends BaseUrl(host) {
 	def getProtocol = "http"
 }

 case class HttpsUrl(override val host: String) extends BaseUrl(host) {
 	def getProtocol = "https"
 }

 trait UrlFactory {
 	def createUrl(action: String)
 }

 class YandexHttpUrlFactory extends UrlFactory {
 	val host = "yandex+ru"

  	def createUrl(action: String) {
 		action match {
 			case "payment" => HttpUrl(host + "/pay")
 			case "payout" => HttpUrl(host + "/payout")
 			case "status" => HttpUrl(host + "/st")
 			case _ => HttpUrl(host + "/info")
 		}
 	}
 }
{% endraw %}
{% endhighlight %}

Плюсы:

 - Позволяет сделать код создания объектов более универсальным, не привязываясь к конкретным классам, а оперируя лишь общим интерфейсом.
 - Позволяет установить связь между параллельными иерархиями классов.

### Одиночка. Singleton
Гарантирует, что в однопоточном приложении будет единственный экземпляр класса с глобальной точкой доступа. Существенно то, что можно пользоваться именно экземпляром класса, так как при этом во многих случаях становится доступной более широкая функциональность. Например, к описанным компонентам класса можно обращаться через интерфейс, если такая возможность поддерживается языком. Применение:

 - Должен быть ровно один экземпляр некоторого класса, легко доступный всем клиентам.
 - Единственный экземпляр должен расширяться путем порождения подклассов, и клиентам нужно иметь возможность работать с расширенным экземпляром без модификации своего кода.

К плюсам следует отнести то, что доступ к единственному экземпляру абсолютно контролируется. К минусам то, что в некоторых случаях приводят к созданию немасштабируемого проекта и усложняет написание модульных тестов.

{% highlight php %}
{% raw %}
 class Singleton {
 	/**
 	 * @var static
 	 **/
 	private static $instance = null;

 	private function __construct() {/*_*/}

 	public static function getInstance()
 	{
 		if (is_null(static::$instance)) {
 			static::$instance = new static();
 		}

 		return static::$instance;
 	}
 }
{% endraw %}
{% endhighlight %}

{% highlight scala %}
{% raw %}
 object Singleton
{% endraw %}
{% endhighlight %}

### Объектный пул. Object pool
Набор инициализированных и готовых к использованию объектов. Когда системе требуется объект, он не создаётся, а берётся из пула (то есть удаляется и возвращается методом пула). Когда объект больше не нужен, он не уничтожается, а возвращается в пул.

{% highlight php %}
{% raw %}
 class Person {/*_*/}

 class PersonPool {
 	const POOL_SIZE = 10;

 	/**
 	 * @var Person[]
 	 **/
 	private $persons = [];

 	public function __construct()
 	{
 		for ($i = 0; $i < self::POOL_SIZE; $i++) {
 			$this->persons[] = new Person;
 		}
 	}

 	public function getPerson()
 	{
 		return array_shift($this->persons);
 	}

 	public function releasePerson(Person $person)
 	{
 		$this->persons[] = $person;

 		return $this;
 	}
 }
{% endraw %}
{% endhighlight %}

В случае нехватки объектов возможны различные стратегии: расширение пула, аварийная остановка или ожидание, когда появится свободный объект. К плюсам стоит отнести повышение производительности, когда объекты часто создаются и уничтожаются. К минусам большие затраты в начале и при завершении программы (для PHP такой шаблон врядли применим), а также ожидающие ловушки:

 - После того, как объект возвращён, он должен вернуться в состояние, пригодное для дальнейшего использования. Если объекты после возвращения в пул оказываются в неправильном или неопределённом состоянии, такая конструкция называется объектной клоакой (англ. object cesspool).
 - Повторное использование объектов также может привести к утечке информации. Если в объекте есть секретные данные, после освобождения объекта эту информацию надо затереть.

### Пул одиночек. Multiton
Создает и содержит заданное число экземпляров своего класса. Обеспечивает их идентификацию и точку доступа к ним. Имеет все недостатки singleton и объектного пула, применяется, как правило, в случае, когда необходимо предоставить доступ к определенным данным из различных блоков приложения. Не рекомендуется к применению.

{% highlight php %}
{% raw %}
 class Multiton {
 	const
 		KEY_1 = 'one',
 		KEY_2 = 'two'
 	;

 	/**
 	 * @var static[]
 	 **/
 	private static $instances = [self::KEY_1 => null, self::KEY_2 => null];

 	private function __construct() {/*_*/}

 	public static function getInstance($key)
 	{
 		if (!array_key_exist($key, static::$instances)) {
 			throw new Exception;
 		}

 		return
 			is_null(static::$instances[$key])
 				? static::$instances[$key] = new static
 				: static::$instances[$key];
 	}
 }
{% endraw %}
{% endhighlight %}

### Строитель. Builder
Отделяет алгоритм поэтапного конструирования сложного объекта (продукта) от его внешнего представления так, что с помощью одного и того же алгоритма можно получать разные представления этого продукта. Поэтапное создает объект по частям. После того как построена последняя часть, объект можно использовать.

Алгоритм поэтапного создания продукта в специальном классе Director (распорядитель), а ответственность за координацию процесса сборки отдельных частей продукта возлагает на иерархию классов Builder. В этой иерархии базовый класс Builder объявляет интерфейсы для построения отдельных частей продукта, а соответствующие подклассы ConcreteBuilder их реализуют подходящим образом, например, создают или получают нужные ресурсы, сохраняют промежуточные результаты, контролируют результаты выполнения операций. Класс Director содержит указатель или ссылку на Builder, который перед началом работы должен быть сконфигурирован экземпляром ConcreteBuilder, определяющим соответствующе представление. После этого Director может обрабатывать клиентские запросы на создание объекта. Получив такой запрос, с помощью имеющегося экземпляра строителя Director строит продукт по частям, а затем возвращает его пользователю.

{% highlight php %}
{% raw %}
 class Auto {
     /**
      * @var integer
      */
     public $wheel;

     /**
      * @var float
      */
     public $engine;
 }

 interface IAssemblyLine {
     /**
      * @return static
      */
     public function createAuto();

     /**
      * @return static
      */
     public function buildWheel();

     /**
      * @return static
      */
     public function buildEngine();

     /**
      * @return Auto
      */
     public function getAuto();
 }

 class AutoPlant {
     /**
      * @var IAssemblyLine
      */
     private $assemblyLine;

     public function __construct(IAssemblyLine $assemblyLine)
     {
         $this->assemblyLine = $assemblyLine;
     }

     /**
      * @return Auto
      */
     public function createAuto()
     {
         return
             $this->assemblyLine
                 ->createAuto()
                 ->buildWheel()
                 ->buildEngine()
                 ->getAuto();
     }
 }

 class VolvoAssemblyLine implements IAssemblyLine {
     const
         WHEEL = 4,
         ENGINE = 1.5
     ;

     /**
      * @var Auto
      */
     private $auto;

     public function createAuto()
     {
         $this->auto = new Auto();

         return $this;
     }

     public function buildWheel()
     {
         $this->auto->wheel = self::WHEEL;

         return $this;
     }

     public function buildEngine()
     {
         $this->auto->engine = self::ENGINE;

         return $this;
     }

     public function getAuto()
     {
         return $this->auto;
     }
 }

 class VazAssemblyLine extends VolvoAssemblyLine {
     const
         WHEEL = 1,
         ENGINE = 0.01
     ;
 }

 (new AutoPlant(new VazAssemblyLine))->createAuto();
{% endraw %}
{% endhighlight %}

Плюсы:

 - Возможность контролировать процесс создания сложного продукта
 - Возможность получения разных представлений некоторых данных.

Минусы:

 - Слишком жесткая связь между конкретным строителем и создаваемым объектом, при изменении объекта скорее всего предется менять и строителя.

### Прототип. Prototype
Cоздания объекта через клонирование другого объекта (экземпляра-прототипа) вместо создания через конструктор. Использовать следует, если система должна оставаться независимой как от процесса создания новых объектов, так и от типов порождаемых объектов, или же необходимо создавать объекты, точные классы которых становятся известными уже на стадии выполнения программы.

Возможны различные реализации паттерна Prototype, например:

 - В виде обобщенного конструктора на основе прототипов, когда в полиморфном базовом классе Prototype определяется статический метод, предназначенный для создания объектов. При этом в качестве параметра в этот метод должен передаваться идентификатор типа создаваемого объекта.
 - На базе специально выделенного класса-фабрики.
 - В виде обобщенного конструктора на основе прототипов, когда в полиморфном базовом классе Prototype определяется абстрактный метод клонирования.

{% highlight php %}
{% raw %}
 interface Auto {/*_*/}

 abstract class Prototyped {
     /**
      * @var array
      **/
     private $prototype = null;

     /**
      * @return static
      */
     public function create()
     {
     	if (is_null($this->prototype)) {
     		$this->prototype = new static;
     	}

         return clone $this->prototype;
     }
 }

 class Volvo extends Prototyped implements Auto {/*_*/}

 class Vaz extends Prototyped  implements Auto {/*_*/}
{% endraw %}
{% endhighlight %}

{% highlight php %}
{% raw %}
 interface Auto {/*_*/}

 class Volvo implements Auto {/*_*/}

 class Vaz implements Auto {/*_*/}

 class AutoPrototypeFactory {
     /**
      * @var Auto
      */
     private $auto;

     public function __construct(Auto $auto)
     {
         $this->auto = $auto;
     }

     public function getAuto()
     {
         return clone $this->auto;
     }
 }
{% endraw %}
{% endhighlight %}

К плюсам следует отнести возможность гибкого управления процессом создания новых объектов, а то, что для создания новых объектов клиенту необязательно знать их конкретные классы. Главным минусом является то, что каждый тип создаваемого продукта должен реализовывать операцию клонирования clone(). В случае, если требуется глубокое копирование объекта (объект содержит ссылки или указатели на другие объекты), это может быть непростой задачей.

### Ленивая инициализация. Lazy initialization
Некоторая ресурсоёмкая операция (создание объекта, вычисление значения) выполняется непосредственно перед тем, как будет использован её результат. Плюсы:
 - Инициализация выполняется только в тех случаях, когда она действительно необходима.
 - Ускоряется начальная инициализация.

Минусы:
 - Невозможно явным образом задать порядок инициализации объектов.
 - Возникает задержка при первом обращении к объекту.

{% highlight php %}
{% raw %}
 class FirstAidKit {/*_*/}

 class Auto {
     /**
      * @var FirstAidKit
      */
     private $firstAidKit = null;

     public function getFirstAidKit()
     {
         if (is_null($this->firstAidKit)) {
             $this->firstAidKit = new FirstAidKit();
         }

         return $this->firstAidKit;
     }
 }
{% endraw %}
{% endhighlight %}

### Получение ресурса есть инициализация. Resource Acquisition Is Initialization
Получение некоторого ресурса совмещается с инициализацией, а освобождение — с уничтожением объекта. Получение доступа к ресурсу происходит в конструкторе, а освобождение в деструкторе. Поскольку деструктор переменной автоматически вызывается при выходе её из области видимости, то ресурс гарантированно освобождается при уничтожении переменной.

{% highlight php %}
{% raw %}
 class CallbackPromise {
     /**
      * @var Closure
      */
     private $function;

     /**
      * @var boolean
      */
     private $isCalled = false;

     public function __construct(Closure $function)
     {
         $this->function = $function;
     }

     public function __destruct()
     {
         if (!$this->isCalled) call_user_func($this->function);
     }

     /**
      * @return void
      */
     public function call()
     {
         call_user_func($this->function);

         $this->isCalled = true;
     }
 }
{% endraw %}
{% endhighlight %}

## Структурные шаблоны
### Адаптер. Adapter
Предназначен для организации использования функций объекта, недоступного для модификации, через специально созданный интерфейс. Используется, если система поддерживает требуемые данные и поведение, но имеет неподходящий интерфейс. Шаблон состоит из интерфейса адаптера и конкретных реализаций, каждая из которых агрегирует по значению адаптируемый класс. Участники:
 - Адаптируемый объект (Adaptee).
 - Цель, определяющая требуемый интерфейс, который реализует адаптер.
 - Адаптер (Adapter), агрегирующий или наследующий адаптируемый объект.

{% highlight php %}
{% raw %}
 class Wrapper {
     public function wrap(stdClass $object)
     {/*_*/}
 }

 class Decorator {
     public function decorate(stdClass $object)
     {/*_*/}
 }

 interface IWrapperAdapter {
     public function wrap(stdClass $object);
 }

 class WrapperAdapter implements IWrapperAdapter {
     /**
      * @var Wrapper
      */
     private $wrapper;

     public function __construct()
     {
         $this->wrapper = new Wrapper();
     }

     public function wrap(stdClass $object)
     {
         return $this->wrapper->wrap($object);
     }
 }

 class DecoratorAdapter implements IWrapperAdapter {
     /**
      * @var Decorator
      */
     private $decorator;

     public function __construct()
     {
         $this->wrapper = new Wrapper();
     }

     public function wrap(stdClass $object)
     {
         return $this->decorator->decorate($object);
     }
 }
{% endraw %}
{% endhighlight %}

 Плюсом паттерна Adapter является возможность повторно использовать уже имеющийся код, адаптируя его несовместимый интерфейс к виду, пригодному для использования. Главным минусом - проблема совместимости параметров, они могут отличаться в адаптируемых классах.

### Мост
Позволяет разделять абстракцию и реализацию так, чтобы они могли изменяться независимо. Шаблон Мост предполагает, что основной код, необходимый для функционирования объекта, переносится в реализацию. Всё остальное, включая взаимодействие с клиентом, содержится в абстракции. Её методы, при необходимости, могут быть изменены или дополнены. Кроме того, она содержит экземпляр реализации и использует его для обработки поступающих от клиентов запросов. Под обработкой подразумевается как прямая переадресация запроса, так и вызов группы методов реализации для получения результата. Участники:
 - Abstraction - определяет базовый интерфейс и хранит ссылку на объект Implementor. Выполнение операций в Abstraction делегируется методам объекта Implementor.
 - RefinedAbstraction - уточненная абстракция, наследуется от Abstraction и расширяет унаследованный интерфейс.
 - Implementor - определяет базовый интерфейс для конкретных реализаций. Как правило, Implementor определяет только примитивные операции. Более сложные операции, которые базируются на примитивных, определяются в Abstraction.
 - ConcreteImplementor - конкретные реализации, которые унаследованы от Implementor
{% highlight php %}
{% raw %}
 interface IStringPositionFinder {
     /**
      * @param string $string
      * @param string $text
      * @return integer[]
      */
     public function findLineNumbers($string, $text);
 }

 class StringFirstLinePositionFinder implements IStringPositionFinder {
     public function findLineNumbers($string, $text)
     {
         $lineNumber = 0;

         foreach (explode("\n", $text) as $line) {
             $lineNumber++;
             if (strpos($string, $line)) {
                 return [$lineNumber];
             }
         }

         return [];
     }
 }

 class StringBitSetLinesPositionFinder implements IStringPositionFinder {
     public function findLineNumbers($string, $text)
     {
         $lineNumbers = 0b1;

         foreach (explode("\n", $text) as $line) {
             $lineNumbers =
                 strpos($string, $line) !== false
                     ? $lineNumbers << 1 | 1
                     : $lineNumbers << 1 | 0;
         }

         return
             array_map(
                 function ($value) {
                     return $value + 1;
                 },
                 array_keys(
                     array_filter(
                         str_split(
                             substr(
                                 decbin($lineNumbers),
                                 1
                             )
                         )
                     )
                 )
             );
     }
 }

 interface IReplacer {
     /**
      * @param string $that
      * @param string $on
      * @param string $text
      * @return boolean
      */
     public function replace($that, $on, $text);
 }

 class Replacer implements IReplacer {
     /**
      * @var IStringPositionFinder
      */
     private $stringPositionFinder;

     public function __construct(IStringPositionFinder $stringPositionFinder)
     {
         $this->stringPositionFinder = $stringPositionFinder;
     }

     public function replace($that, $on, $text)
     {
         $lineNumbers = $this->stringPositionFinder->findLineNumbers($that, $text);

         $exit = explode("\n", $text);

         foreach ($lineNumbers as $lineNumber) {
             $exit[$lineNumber] = $on;
         }

         return join('\n', $exit);
     }
 }
{% endraw %}
{% endhighlight %}

Плюсы шаблона:
 - Проще расширять систему новыми типами за счет сокращения общего числа родственных подклассов.
 - Возможность динамического изменения реализации в процессе выполнения программы.
 - Паттерн Bridge полностью скрывает реализацию от клиента. В случае модификации реализации пользовательский код не требует перекомпиляции.

### Компоновщик. Composite
Позволяет упростить и стандартизировать взаимодействие между клиентом и группой объектов, представляющих древовидную структуру "составной объект – его части". Применяется, когда необходимо, чтобы клиент одинаково обращался как к составным объектам, так и к отдельным частям, то есть необходимо использовать группу объектов как один составной. Участники:
 - Component - определяет базовый интерфейс объекта, описывает методы и конкретного объекта и композиции.
 - Leaf - объект, в котором имеют смысл только конкретные методы, а методы композиции являются заглушками.
 - Composite - объект-контейнер лепестков, в котором методы композиции (добавление, удаление лепестков), а конкретные методы вызывают методы всех компонентов.

{% highlight php %}
{% raw %}
 class Ticket {
     /**
      * @var integer
      */
     public $number;

     /**
      * @var float
      */
     public $price;

     /**
      * @return float
      */
     public function getPrice()
     {
         return $this->price;
     }

     /**
      * @return integer
      */
     public function getNumber()
     {
         return $this->number;
     }

     public function __construct($number, $price)
     {
         $this->number = $number;
         $this->price = $price;
     }
 }

 interface IPassenger {
     /**
      * @return integer
      */
     public function getId();

     /**
      * @param IPassenger $passenger
      * @return static
      */
     public function add(IPassenger $passenger);

     /**
      * @param IPassenger $passenger
      * @return static
      */
     public function remove(IPassenger $passenger);

     /**
      * @return Ticket
      */
     public function getTicketPrice();
 }

 class Passenger implements IPassenger {
     /**
      * @var Ticket
      */
     public $ticket;

     public function __construct(Ticket $ticket)
     {
         $this->ticket = $ticket;
     }

     public function getId()
     {
         return $this->getTicketPrice()->getNumber();
     }

     public function add(IPassenger $passenger)
     {
         throw new Exception('Not in leaf');
     }

     public function remove(IPassenger $passenger)
     {
         /*_*/
     }

     /**
      * @return Ticket
      */
     public function getTicketPrice()
     {
         return $this->ticket;
     }
 }

 class Passengers implements IPassenger {
     /**
      * @var IPassenger[]
      */
     public $passengers = [];

     public function getId()
     {
         return max(array_keys($this->passengers));
     }

     public function add(IPassenger $passenger)
     {
         $this->passengers[$passenger->getId()] = $passenger;

         return $this;
     }

     public function remove(IPassenger $passenger)
     {
         unset($this->passengers[$passenger->getId()]);

         return $this;
     }

     /**
      * @return Ticket
      */
     public function getTicketPrice()
     {
         $sum = 0;

         foreach ($this->passengers as $passenger) {
             /* @var IPassenger $passenger */
             $sum += $passenger->getTicketPrice();
         }

         return $sum;
     }
 }
{% endraw %}
{% endhighlight %}

Плюсы шаблона:
 - В систему легко добавлять новые примитивные или составные объекты, так как паттерн Composite использует общий базовый класс Component.
 - Код клиента имеет простую структуру – примитивные и составные объекты обрабатываются одинаковым образом.
 - Паттерн Composite позволяет легко обойти все узлы древовидной структуры.

Минусом является сложность запрета на добавление в составной объект Composite объектов определенных типов.

### Декоратор. Decorator
Предназначен для динамического подключения дополнительного поведения к объекту, не прибегая к наследованию. Участники:
 - Общий интерфейс (IComponent) – определяет интерфейс объектов и декоратора.
 - Конкретный компонент (Concrete component) – компонент, реализующие общий интерфейс, функциональность которых необходимо модифицировать.
 - Декоратор (Decorator) – реализация декоратора, добавляющая определенные функции компоненту.

{% highlight php %}
{% raw %}
 interface IIntegerCollectionGenerator {
     /**
      * @param integer $count
      * @return array
      */
     public function generate($count);
 }

 class IntegerCollectionGenerator implements IIntegerCollectionGenerator {
     public function generate($count)
     {
         $result = [];

         for ($i = 0; $i < $count; $i++) {
             $result[] = rand(0, 30);
         }

         return $result;
     }
 }

 class IntegerCollectionGeneratorUniqueValuesDecorator implements IIntegerCollectionGenerator {
     /**
      * @var IIntegerCollectionGenerator
      */
     private $generator;

     public function __construct(IIntegerCollectionGenerator $generator)
     {
         $this->generator = $generator;
     }

     public function generate($count)
     {
         return array_unique($this->generator->generate($count));
     }
 }
{% endraw %}
{% endhighlight %}

Плюсы:
 - Добавляемая функциональность реализуется в небольших объектах. Преимущество состоит в возможности динамически добавлять эту функциональность до или после основной функциональности объекта.
 - Позволяет избегать перегрузки функциональными классами на верхних уровнях иерархии.

### Фасад. Facade
Позволяет скрыть сложность системы путем сведения всех возможных внешних вызовов к одному объекту, делегирующему их соответствующим объектам системы. Создает более простой интерфейс. Участники:
 - Компоненты (Сomponents) - множество компонентов системы, функции которых необходимо сокрыть.
 - Фасад (Facade) - объект, в котором сокрыто выполнение методов компонентов.

{% highlight php %}
{% raw %}
 class StatusEnum {
     const
         SUCCESS = 'YES',
         DECLINE = 'NO'
     ;

     /**
      * @var string
      */
     private $id;

     /**
      * StatusEnum constructor.

      * @param string $id
      */
     public function __construct($id)
     {
         $this->id = $id;
     }
 }

 class RequestBuilder {
     /**
      * @param array $data
      * @return HttpRequest
      */
     public function build(array $data)
     {
         /*_*/
     }
 }

 class RequestSender {
     /**
      * @param HttpRequest $request
      * @return HttpResponse
      */
     public function send(HttpRequest $request)
     {
         /*_*/
     }
 }

 class ResponseParser {
     /**
      * @param HttpResponse $response
      * @return StatusEnum
      */
     public function parse(HttpResponse $response)
     {
         /*_*/
     }
 }

 class SenderFacade {
     /**
      * @var RequestBuilder
      */
     private $builder;

     /**
      * @var RequestSender
      */
     private $sender;

     /**
      * @var ResponseParser
      */
     private $parser;

     public function __construct(RequestBuilder $builder, RequestSender $sender, ResponseParser $parser)
     {
         $this->builder = $builder;
         $this->sender = $sender;
         $this->parser = $parser;
     }

     /**
      * @param array $data
      * @return string
      */
     public function send(array $data)
     {
         $httpRequest = $this->builder->build($data);

         $httpResponse = $this->sender->send($httpRequest);

         return $this->parser->parse($httpResponse);
     }
 }
{% endraw %}
{% endhighlight %}

### Приспособленец. Flyweight
Объект, представляющий себя как уникальный экземпляр в разных местах программы, по факту не является таковым. Используется при наличии в системе большого количества однообразных объектов. Данный паттерн используется преимущественно для оптимизации работы с памятью. Участники:
 - Приспособленец (Flyweight) - определяет интерфейс, через который приспособленцы-разделяемые объекты могут получать внешнее состояние или воздействовать на него.
 - Конкретный приспособленец (Concrete flyweight) - конкретный класс разделяемого приспособленца. Реализует интерфейс, объявленный в типе Flyweight, и при необходимости добавляет внутреннее состояние. Причем любое сохраняемое им состояние должно быть внутренним, не зависящим от контекста. Внешнее состояние передается приспособленцу при вызове его методов.
 - Фабрика приспособленцев (Flyweight factory) - создает объекты разделяемых приспособленцев. Так как приспособленцы разделяются, то клиент не должен создавать их напрямую. Все созданные объекты хранятся в пуле. В примере выше для определения пула используется объект Hashtable, но это не обязательно. Можно применять и другие классы коллекций. Однако в зависимости от сложности структуры, хранящей разделяемые объекты, особенно если у нас большое количество приспособленцев, то может увеличиваться время на поиск нужного приспособленца - наверное это один из немногих недостатков данного паттерна..

{% highlight php %}
{% raw %}
 interface MathematicsOperation {
     /**
      * @param integer $left
      * @param integer $right
      * @return integer
      */
     public function make($left, $right);
 }

 class PlusOperation implements MathematicsOperation {
     public function make($left, $right)
     {
         return $left + $right;
     }
 }

 class MinusOperation implements MathematicsOperation {
     public function make($left, $right)
     {
         return $left - $right;
     }
 }

 class OperationsFactory {
     const
         MINUS = '-',
         PLUS = '+'
     ;

     /**
      * @var array
      */
     private $operations = [];

     public function getOperation($operation)
     {
         if (!array_key_exists($operation, $this->operations)) {
             switch ($operation) {
                 case '+':
                     $this->operations[$operation] = new PlusOperation();
                     break;
                 case '-':
                     $this->operations[$operation] = new MinusOperation();
                     break;
                 default:
                     return null;
             }
         }

         return $this->operations[$operation];
     }
 }
{% endraw %}
{% endhighlight %}

Главная особенность - присобленец подразумевает вынесение части состояния (данных) объекта во внешнюю среду.

### Заместитель. Proxy
Предоставляет объект, который контролирует доступ к другому объекту, перехватывая все вызовы (выполняет функцию контейнера). Данный шаблон используется если работа с объектом не должна зависеть от того, где он реально расположен (от адресного пространства приложения до удаленного сервера), или нужно выполнять определенные действия при доступе к объекту, или необходимо оптимизировать взаимодействие объекта с клиентом. Виды:
 - Протоколирующий прокси: сохраняет в лог все вызовы «Субъекта» с их параметрами.
 - Удалённый заместитель (англ. remote proxies): обеспечивает связь с «Субъектом», который находится в другом адресном пространстве или на удалённой машине. Также может отвечать за кодирование запроса и его аргументов и отправку закодированного запроса реальному «Субъекту»,
 - Виртуальный заместитель (англ. virtual proxies): обеспечивает создание реального «Субъекта» только тогда, когда он действительно понадобится. Также может кэшировать часть информации о реальном «Субъекте», чтобы отложить его создание,
 - Копировать-при-записи: обеспечивает копирование «субъекта» при выполнении клиентом определённых действий (частный случай «виртуального прокси»).
 - Защищающий заместитель (англ. protection proxies): может проверять, имеет ли вызывающий объект необходимые для выполнения запроса права.
 - Кэширующий прокси: обеспечивает временное хранение результатов расчёта до отдачи их множественным клиентам, которые могут разделить эти результаты.
 - Экранирующий прокси: защищает «Субъект» от опасных клиентов (или наоборот).
 - Синхронизирующий прокси: производит синхронизированный контроль доступа к «Субъекту» в асинхронной многопоточной среде.
 - «Умная» ссылка (англ. smart reference proxy): производит дополнительные действия, когда на «Субъект» создается ссылка, например, рассчитывает количество активных ссылок на «Субъект».

{% highlight php %}
{% raw %}
 interface IRequestSender {
     /**
      * @param HttpRequest $request
      * @return HttpResponse
      */
     public function send(HttpRequest $request);
 }

 class RequestSender implements IRequestSender {
     public function send(HttpRequest $request)
     {
         new HttpResponse();
     }
 }

 class RequestSenderLoggerProxy implements IRequestSender {
     /**
      * @var IRequestSender
      */
     private $requestSender;

     public function __construct()
     {
         $this->requestSender = new RequestSender();
     }

     public function send(HttpRequest $request)
     {
         $this->logRequest($request);

         $response = $this->requestSender->send($request);

         $this->logResponse($response);

         return $response;
     }

     /**
      * @param HttpRequest $request
      * @return void
      */
     private function logRequest(HttpRequest $request)
     {
         /*_*/
     }

     /**
      * @param HttpResponse $request
      * @return void
      */
     private function logResponse(HttpResponse $request)
     {
         /*_*/
     }
 }
{% endraw %}
{% endhighlight %}

## Поведенческие шаблоны
### Цепочка обязанностей. Chain of responsibility
Предназначен для предотвращения связывания инициатора сообщения с конкретным экземпляром его обработчика. При этом само сообщение передается по цепочке, в которой каждое звено выбирает между обработкой и пересылкой данных дальше. Подразумевается вызов следующего обработчика в **определенном порядке**. Участники:
 - Интерфейс обработчика (IHandler) – определяет общий интерфейс для взаимодействия с клиентами.
 - Обработчик (Handler) – непосредственная реализация, которая обрабатывает запрос.
 - Управляющий объект (Manager) – не обязательный участник, который используется для построения структуры обработчиков и управления ей.

Цепочка ответственностей применяется в случаях, если:
 - Существует более одного обработчика исходного запроса.
 - Обработчики сообщения определяются во время выполнения приложения.
 - Необходимо отправить сообщение без явного указания получателя.

Классическая цепочка представляет конструкцию, в которой обрабочик знает о следующем.

{% highlight php %}
{% raw %}
 interface IMessageGenerator {
     /**
      * @param string $to
      * @param string $content
      * @return string
      */
     public function generate($to, $content);

     /**
      * @param string $to
      * @return boolean
      */
     public function shouldGenerate($to);
 }

 class MessageGeneratorChain implements IMessageGenerator {
     /**
      * @var IMessageGenerator[]
      */
     private $messageGenerators;

     public function __construct(array $messageGenerators)
     {
         $this->messageGenerators = $messageGenerators;
     }

     public function generate($to, $content)
     {
         foreach ($this->messageGenerators as $generator) {
             if ($generator->shouldGenerate($to)) {
                 return $generator->generate($to, $content);
             }
         }

         throw new Exception;
     }

     public function shouldGenerate($to)
     {
         return true;
     }
 }
{% endraw %}
{% endhighlight %}

В результате применения шаблона:
 - Уменьшается связанность, так как отправителю и получателю нет необходимости знать обо всех обработчиках в цепочке, а так же какие именно их них отреагируют на запрос. Более того, сами обработчики могут не знать друг о друге и даже о следующем звене. Наглядным примером может служить обработка исключений: метод, выбросивший его, ничего не знает о том где и как оно будет обработано.
 - Увеличивается гибкость приложения, т.к. сама цепочка обработчиков может меняться независимо от клиента. Последнему необходимо знать только точку для отправки сообщения.

### Команда. Command
Паттерн Command преобразовывает запрос на выполнение действия в отдельный объект-команду. Такая инкапсуляция позволяет передавать эти действия другим функциям и объектам в качестве параметра, приказывая им выполнить запрошенную операцию. Команда – это объект, поэтому над ней допустимы любые операции, что и над объектом. Участники:
 - Интерфейс представляющий команду (Command) - обычно определяет метод execute() для выполнения действия, а также нередко включает метод undo(), реализация которого должна заключаться в отмене действия команды.
 - Конкретная реализация команды (ConcreteCommand) - реализует метод execute(), в котором вызывается определенный метод, определенный в классе Receiver.
 - Получатель команды (Receiver) - определяет действия, которые должны выполняться в результате запроса.
 - Инициатор команды (Invoker) - вызывает команду для выполнения определенного запроса
{% highlight php %}
{% raw %}
 interface Command {
     /**
      * @param array $args
      * @return int
      */
     public function execute(array $args);
 }

 class ListCommand implements Command {
     public function execute(array $args)
     {
         exec("ls -la", $o, $code);

         return $code;
     }
 }

 class CdCommand implements Command {
     public function execute(array $args)
     {
         exec("cd" . (isset($args[0]) ? $args[0] : ""), $o, $code);

         return $code;
     }
 }
{% endraw %}
{% endhighlight %}

Паттерн Команда придает системе гибкость, отделяя инициатора запроса от его получателя.

### Интерпретатор. Interpreter
Паттерн Interpreter определяет грамматику простого языка для проблемной области, представляет грамматические правила в виде языковых предложений и интерпретирует их для решения задачи. Для представления каждого грамматического правила паттерн Interpreter использует отдельный класс. А так как грамматика, как правило, имеет иерархическую структуру, то иерархия наследования классов хорошо подходит для ее описания. Является самым сложным из шаблонов проектирования. Участники:
 - AbstractExpression (RegularExpression) - объявляет абстрактную операцию interpret, которая является общей для всех узлов в абстрактном дереве синтаксиса.
 - TerminalExpression (LiteralExpression) - осуществляет операцию interpret, связанную с **символами** терминала в грамматике. Экземпляр требуется для каждого символа терминала в предложении.
 - NonterminalExpression (AlternationExpression, RepetitionExpression, SequenceExpressions) - один такой класс требуется для каждого **правила** в грамматике. Поддерживает переменные типа AbstractExpression. Осуществляет операцию interpret для нетерминальных символов в грамматике.
 - Context - содержит глобальную для интерпретатора информацию.

{% highlight php %}
{% raw %}
 class Answer {
     /**
      * @var boolean
      */
     public $right;

     /**
      * @return boolean
      */
     public function isRight()
     {
         return $this->right;
     }

     /**
      * @param boolean $isRight
      * @return Answer
      */
     public function setRight($isRight)
     {
         $this->right = $isRight;

         return $this;
     }

     /**
      * @var string
      */
     public $answer;

     public function __construct($answer)
     {
         $this->answer = $answer;
     }
 }

 abstract class AnswerCheck {
     /**
      * @param Answer $answer
      * @return Answer
      */
     public function process(Answer $answer)
     {
         if ($this->check($answer)) {
             return $answer->setRight(true);
         }

         return $answer->setRight(false);
     }

     /**
      * @param Answer $answer
      * @return boolean
      */
     abstract protected function check(Answer $answer);
 }

 class AnswerCheckUnit extends AnswerCheck {
     /**
      * @var string
      */
     private $right;

     public function __construct($right)
     {
         $this->right = $right;
     }

     protected function check(Answer $answer)
     {
         return $this->right == $answer->answer;
     }
 }

 class AnswerCheckOr extends AnswerCheck {
     /**
      * @var AnswerCheck
      */
     private $first;

     /**
      * @var AnswerCheck
      */
     private $second;

     /**
      * @param AnswerCheck $first
      * @param AnswerCheck $second
      * @return AnswerCheckOr
      */
     public static function create(AnswerCheck $first, AnswerCheck $second)
     {
         return new self($first, $second);
     }

     public function __construct(AnswerCheck $first, AnswerCheck $second)
     {
         $this->first = $first;
         $this->second = $second;
     }

     protected function check(Answer $answer)
     {
         return
             $this->checkPart('first', $answer)
                 ||
             $this->checkPart('second', $answer)
         ;
     }

     /**
      * @param string $part
      * @param Answer $answer
      * @return bool
      */
     private function checkPart($part, Answer $answer)
     {
         $this->{$part}->process($answer);

         return $answer->isRight();
     }
 }

 var_dump(
     AnswerCheckOr::create(
         new AnswerCheckUnit('4'),
         new AnswerCheckUnit('Четыре')
     )
         ->process(new Answer('Четыре'))
             ->isRight()
 );

 var_dump(
     AnswerCheckOr::create(
         new AnswerCheckUnit('4'),
         new AnswerCheckUnit('Четыре')
     )
         ->process(new Answer('Пять'))
             ->isRight()
 );
{% endraw %}
{% endhighlight %}

{% highlight console %}
{% raw %}
 MacBook-Pro-Pavel:project zinvapel$ php ololo.php
 bool(true)
 bool(false)
{% endraw %}
{% endhighlight %}

### Итератор. Iterator
Представляет собой объект, позволяющий получить последовательный доступ к элементам объекта-агрегата без использования описаний каждого из объектов, входящих в состав агрегации. Участники:
 - Интерфейс итератора (IIterator) – определяет методы для доступа к элементам коллекций.
 - Итератор (Iterator) – реализация итератора для конкретного типа коллекции.
 - Интерфейс составного объекта, коллекции (IContainer) – задает способ получения итератора клиентом.
 - Составной объект, коллекция (Container) – реализация коллекции или фабрики, предоставляющие итератор.

Контейнер предоставляет метод, возвращающий итератор с содержимым. Как правило итератор имееет следующие методы:
 - reset – устанавливает первый элемент списка в качестве текущего.
 - next – переход на следующий элемент.
 - hasNext – проверяет есть ли еще элементы в коллекции.
 - current – возвращает текущий элемент.

{% highlight php %}
{% raw %}
 class Unit {/*_*/}

 class UnitIterator {
     /**
      * @var Unit[]
      */
     private $units;

     private $current = 0;

     public function __construct(array $units)
     {
         $this->units = $units;
     }

     public function next()
     {
         $this->current++;
     }

     /**
      * @return boolean
      */
     public function hasNext()
     {
         return $this->current < count($this->units);
     }

     /**
      * @return null|Unit
      */
     public function current()
     {
         return
             isset($this->units[$this->current])
                 ? $this->units[$this->current]
                 : null
             ;
     }

     public function rewind()
     {
         $this->current--;
     }
 }

 class UnitsContainer {
     /**
      * @var array
      */
     private $units;
     /**
      * @param Unit $unit
      * @return UnitsContainer
      */
     public function addUnit(Unit $unit)
     {
         $this->units[] = $unit;

         return $this;
     }

     public function __construct(array $units)
     {
         $this->units = $units;
     }

     public function iterator()
     {
         return new UnitIterator($this->units);
     }
 }
{% endraw %}
{% endhighlight %}

### Посредник. Mediator
Инкапсулирует взаимодействие множества объектов. Mediator делает систему слабо связанной, избавляя объекты от необходимости ссылаться друг на друга, что позволяет изменять взаимодействие между ними независимо. Паттерн Mediator вводит посредника для развязывания множества взаимодействующих объектов. Заменяет взаимодействие "все со всеми" взаимодействием "один со всеми". Участники:

 - Посредник (Mediator) - представляет интерфейс для взаимодействия с объектами Colleague
 - Коллега (Colleague) - представляет интерфейс для взаимодействия с объектом Mediator
 - Конкретные коллеги (ConcreteColleague) - обмениваются друг с другом через объект Mediator
 - Конкретный посредник (ConcreteMediator) - реализующий интерфейс типа Mediator
Коллеги (Colleague) посылают запросы посреднику (ConcreteMediator) и получают запросы от него. Посредник реализует заданную логику взаимодействия (кооперативное поведение) путем переадресации каждого запроса подходящему коллеге (или сразу нескольким).

{% highlight php %}
{% raw %}
 interface IChatClient {
     /**
      * @param string $message
      * @return void
      */
     public function send($message);

     /**
      * @param string $message
      * @return void
      */
     public function receive($message);
 }

 class ChatClient implements IChatClient {
     /**
      * @var
      */
     private $server;

     public function __construct(IServer $server)
     {
         $this->server = $server;
     }

     public function send($message)
     {
         $this->server->notify($message, $this);
     }

     public function receive($message)
     {
         /*_*/
     }
 }

 interface IChatServer {
     /**
      * @param string $message
      * @param IChatClient $from
      * @return void
      */
     public function notify($message, IChatClient $from);
 }

 class ChatServer implements IChatServer {
     /**
      * @var IChatClient[]
      */
     private $clients = [];

     /**
      * @param IChatClient $client
      * @return static
      */
     public function addClient(IChatClient $client)
     {
         if (!in_array($client, $this->clients)) {
             $this->clients[] = $client;
         }

         return $this;
     }

     public function notify($message, IChatClient $from)
     {
         foreach ($this->clients as $client) {
             if ($from == $client) {
                 continue;
             }

             $client->receive($message);
         }
     }
 }
{% endraw %}
{% endhighlight %}

В результате применения шаблона:

 - Устраняется сильная связанность между объектами Colleague
 - Упрощается взаимодействие между объектами: вместо связей по типу "все-ко-всем" применяется связь "один-ко-всем"
 - Взаимодействие между объектами абстрагируется и выносится в отдельный интерфейс
 - Централизуется управления отношениями между объектами

### Хранитель. Memento
Не нарушая инкапсуляции, паттерн Memento получает и сохраняет за пределами объекта его внутреннее состояние так, чтобы позже можно было восстановить объект в таком же состоянии. Является средством для инкапсуляции "контрольных точек" программы. Паттерн Memento придает операциям "Отмена" (undo) или "Откат" (rollback) статус "полноценного объекта". Участники:
 - Хранитель (Memento) - сохраняет состояние объекта Originator и предоставляет полный доступ только этому объекту Originator.
 - Хозяин (Originator) - создает объект хранителя для сохранения своего состояния.
 - Опекун (Caretaker) - выполняет только функцию хранения объекта Memento, в то же время у него нет полного доступа к хранителю и никаких других операций над хранителем, кроме собственно сохранения, он не производит.

{% highlight php %}
{% raw %}
 class StatusSnapshop {
     /**
      * @var integer
      */
     private $status;

     /**
      * @return integer
      */
     public function getStatus()
     {
         return $this->status;
     }

     /**
      * @param integer $status
      * @return $this
      */
     public function setStatus($status)
     {
         $this->status = $status;

         return $this;
     }
 }

 class Transaction {
     /**
      * @var integer
      */
     private $status;

     public function getStatusSnapshot()
     {
         return new StatusSnapshop($this->status);
     }

     /**
      * @param StatusSnapshop $snapshop
      * @return $this
      */
     public function restoreFrom(StatusSnapshop $snapshop)
     {
         $this->status = $snapshop->getStatus();

         return $this;
     }
 }

 class SnapshotCaretaker {
     /**
      * @var StatusSnapshop
      */
     private $snapshot;

     /**
      * @return StatusSnapshop
      */
     public function getSnapshot()
     {
         return $this->snapshot;
     }

     /**
      * @param StatusSnapshop $snapshot
      * @return SnapshotCaretaker
      */
     public function setSnapshot($snapshot)
     {
         $this->snapshot = $snapshot;

         return $this;
     }
 }
{% endraw %}
{% endhighlight %}

### Null Object
Объект с нейтральным поведением. Целью Null object является инкапсулирование отсутствия объекта путем замещения его другим объектом, который ничего не делает.

{% highlight php %}
{% raw %}
 interface ISender {
     /**
      * @param string $message
      * @return integer
      */
     public function send($message);
 }

 class SMSSender implements ISender {
     public function send($message)
     {
         echo 'SMS: ' + $message;
     }
 }

 class NullSender implements ISender {
     public function send($message)
     {
         /*_*/
     }
 }
{% endraw %}
{% endhighlight %}

### Наблюдатель. Observer
Создает механизм у класса, который позволяет получать экземпляру объекта этого класса оповещения от других объектов об изменении их состояния, тем самым наблюдая за ними. При этом наблюдаемому объекту не надо ничего знать о наблюдателе кроме того, что тот реализует метод Update(). С помощью отношения агрегации реализуется слабосвязанность обоих компонентов. Изменения в наблюдаемом объекте не виляют на наблюдателя и наоборот. В определенный момент наблюдатель может прекратить наблюдение. И после этого оба объекта - наблюдатель и наблюдаемый могут продолжать существовать в системе независимо друг от друга. Участники:
 - Интерфейс наблюдаемого объекта (IObservable) - определяет три метода: addObserver() (для добавления наблюдателя), removeObserver() (удаление набюдателя) и notifyObservers() (уведомление наблюдателей).
 - Реализация IObservable (ConcreteObservable) - определяет коллекцию объектов наблюдателей.
 - Интерфейс наблюдателя (IObserver) - подписывается на все уведомления наблюдаемого объекта. Определяет метод update(), который вызывается наблюдаемым объектом для уведомления наблюдателя.
 - Реализация IObservable (ConcreteObserver).

{% highlight php %}
{% raw %}
 interface Observable {
     /**
      * @param IObserver $observer
      * @return static
      */
     public function addObserver(IObserver $observer);

     /**
      * @return static
      */
     public function notify();
 }

 interface IObserver {
     /**
      * @return static
      */
     public function update(Observable $observable);
 }

 class StatusModel implements Observable {
     /**
      * @var IObserver[]
      */
     private $observers = [];

     /**
      * @var integer
      */
     private $status;

     /**
      * @return integer
      */
     public function getStatus()
     {
         return $this->status;
     }

     /**
      * @param integer $status
      * @return StatusModel
      */
     public function setStatus($status)
     {
         $this->status = $status;

         return $this;
     }

     public function addObserver(IObserver $observer)
     {
         if (!in_array($observer, $this->observers)) {
             $this->observers[] = $observer;
         }

         return $this;
     }

     public function notify()
     {
         foreach ($this->observers as $observer) {
             $observer->update($this);
         }
     }
 }

 class StatusObserver implements IObserver {
     public function update(Observable $observable)
     {
         if ($observable instanceof StatusModel) {
             echo "New status: " . $observable->getStatus();
         }

         return $this;
     }
 }
{% endraw %}
{% endhighlight %}

### Слуга. Servant
Предназначен для обеспечения общей функциональности группы классов вместо её определения в каждом из классов. Шаблон похож на паттерн Команда. Разница в том, что для Команды объект-команды передается в метод класса, функциональность которого мы используем. В случае с Servant это происходит в обратном порядке. Клиент знает о слуге, но классы, функциональность которых обеспечивает слуга, не знают о слуге.

{% highlight php %}
{% raw %}
 interface IPosition {
     /**
      * @return integer
      */
     public function getX();

     /**
      * @param float $x
      * @return static
      */
     public function setX($x);
 }

 class PositionMultiplier {
     /**
      * @var integer
      */
     private $factor;

     public function __construct($factor)
     {
         $this->factor = $factor;
     }

     public function multiply(IPosition $position)
     {
         $position->setX($position->getX() * $this->factor);

         return $this;
     }
 }

 class Position implements IPosition {
     /**
      * @var integer
      */
     private $x;

     public function __construct($x)
     {
         $this->x = $x;
     }

     /**
      * @return integer
      */
     public function getX()
     {
         return $this->x;
     }

     /**
      * @param integer $x
      * @return static
      */
     public function setX($x)
     {
         $this->x = $x;

         return $this;
     }
 }
{% endraw %}
{% endhighlight %}

### Спецификация. Specification
Шаблон, посредством которого представление правил бизнес логики может быть преобразовано в виде цепочки объектов, связанных операциями булевой логики. То есть для каждой следущей проверки на соответствие условиям объекты на проверку будут отсеиваться или будут вызываться исключения. Существует несколько реализаций спецификации.
 - Жесткая - конкретные проверки зашиты в классе спецификации, фильтрует или вызывает исключения.
 - Параметризованная - дополнительно принимает условия спецификации.
 - Композитная - проверяет на соответствие спецификации, возвращает булевое значение.

{% highlight php %}
{% raw %}
 class Item {
     public $amount;
 }

 class MinAmountSpecification_HARD {
     const MIN = 0;

     public function check(Item $item)
     {
         if ($item->amount < self::MIN) throw new Exception;

         return $this;
     }
 }

 class MinAmountSpecification_PARAMETRIC {
     public function check(Item $item, $min)
     {
         if ($item->amount < $min) throw new Exception;

         return $this;
     }
 }

 class MinAmountSpecification_COMPOSITE {
     const MIN = 0;

     public function isSatisfied(Item $item)
     {
         if ($item->amount < self::MIN) return false;

         return true;
     }
 }
{% endraw %}
{% endhighlight %}

### Состояние. State
Используется в тех случаях, когда во время выполнения программы объект должен менять своё поведение в зависимости от своего состояния. Создается впечатление, что объект изменил свой класс. Участники:
 - Интерфейс состояния (IState).
 - Состояние (StateN).
 - Контекст (Context) - представляет объект, поведение которого должно динамически изменяться в соответствии с состоянием. Выполнение же конкретных действий делегируется объекту состояния.

Класс Context определяет внешний интерфейс для клиентов и хранит внутри себя ссылку на текущее состояние объекта State. Интерфейс абстрактного базового класса State повторяет интерфейс Context за исключением одного дополнительного параметра - указателя на экземпляр Context. Производные от State классы определяют поведение, специфичное для конкретного состояния. Класс "обертка" Context делегирует все полученные запросы объекту "текущее состояние", который может использовать полученный дополнительный параметр для доступа к экземпляру Context.

{% highlight php %}
{% raw %}
 class Woman {
     const
         FLOOR = "FLOOR",
         DISHES = "DISHES"
     ;

     /**
      * @var WomanState
      */
     private $state;

     public function __construct(WomanState $initialState)
     {
         $this->state = $initialState;
     }

     /**
      * @param string $state
      * @return static
      * @throws Exception
      */
     public function setState($state)
     {
         $newState = null;

         switch ($state) {
             case self::FLOOR:
                 $newState = new WomanFloorState($this);
                 break;
             case self::DISHES:
                 $newState = new WomanDishesState($this);
                 break;
             default:
                 throw new Exception;

         }

         $this->state = $newState;

         return $this;
     }

     public function wash()
     {
         $this->state->wash();
     }
 }

 interface WomanState {
     /**
      * @return void
      */
     public function wash();
 }

 class WomanFloorState implements WomanState {
     /**
      * @var Woman
      */
     private $woman;

     public function __construct(Woman $woman)
     {
         $this->woman = $woman;
     }

     public function wash()
     {
         echo "...";

         $this->woman->setState($this->getNextState());
     }

     protected function getNextState()
     {
         return Woman::DISHES;
     }
 }

 class WomanDishesState implements WomanState {
     /**
      * @var Woman
      */
     private $woman;

     public function __construct(Woman $woman)
     {
         $this->woman = $woman;
     }

     public function wash()
     {
         echo "...";

         $this->woman->setState($this->getNextState());
     }

     protected function getNextState()
     {
         return Woman::FLOOR;
     }
 }
{% endraw %}
{% endhighlight %}

### Стратегия. Strategy
Предназначен для определения семейства алгоритмов, инкапсуляции каждого из них и обеспечения их взаимозаменяемости. Существуют системы, поведение которых может определяться согласно одному алгоритму из некоторого семейства. Все алгоритмы этого семейства являются родственными: предназначены для решения общих задач, имеют одинаковый интерфейс для использования и отличаются только реализацией (поведением). Пользователь, предварительно настроив программу на нужный алгоритм (выбрав стратегию), получает ожидаемый результат. Участники:
 - Интерфейс стратегии (IStrategy) - определяет метод algorithm(). Это общий интерфейс для всех реализующих его алгоритмов. Вместо интерфейса здесь также можно было бы использовать абстрактный класс.
 - Конкретные стратегии (ConcreteStrategyN) - реализуют интерфейс IStrategy, предоставляя свою версию метода algorithm(). Подобных классов-реализаций может быть множество.
 - Контекст (Context) - хранит ссылку на объект IStrategy и связан с интерфейсом IStrategy отношением агрегации.

Контекст может передать стратегии все необходимые алгоритму данные в момент его вызова, место этого контекст может позволить обращаться к своим операциям в нужные моменты, передав ссылку на самого себя операциям класса Strategy. Контекст переадресует запросы своих клиентов объекту-стратегии. Обычно клиент создает объект ConcreteStrategy и передает его контексту, после чего клиент «общается» исключительно с контекстом. Часто в распоряжении клиента находится несколько классов ConcreteStrategy, которые он может выбирать.

{% highlight php %}
{% raw %}
 interface Replacer {
     /**
      * @return string
      */
     public function replace($line);
 }

 class A2BReplacer implements Replacer {
     const
         FROM = 'A',
         TO = 'B'
     ;

     public function replace($line)
     {
         return str_replace(self::FROM, self::TO, $line);
     }
 }

 class R2UppercaseReplacer implements Replacer {
     const
         TARGET = 'r',
         R = 'R'
     ;

     public function replace($line)
     {
         return str_replace(self::TARGET, self::R, $line);
     }
 }

 class Replace {
     /**
      * @var string
      */
     private $line;

     /**
      * @var Replacer
      */
     private $replacer;

     public function __construct($line, Replacer $replacer)
     {
         $this->line = $line;
         $this->replacer = $replacer;
     }

     /**
      * @return string
      */
     public function getLine()
     {
         return $this->line;
     }

     public function replace()
     {
         $this->line = $this->replacer->replace($this->getLine());
     }
 }
{% endraw %}
{% endhighlight %}

Возможны различные реализации, когда конкретная стратегия знает о контексте, а также когда лишь передает необходимые параметры.

### Шаблонный метод. Template method
Определяет основу алгоритма и позволяющий наследникам переопределять некоторые шаги алгоритма, не изменяя его структуру в целом. Участники:
 - Абстрактный класс(Abstract class) - определяет абстрактные операции, замещаемые в наследниках для реализации шагов алгоритма; реализует шаблонный метод, определяющий скелет алгоритма. Шаблонный метод вызывает замещаемые и другие, определенные в Abstract class, операции.
 - Конкретный класс (Concrete class) - реализует замещаемые операции необходимым для данной реализации способом.

{% highlight php %}
{% raw %}
 abstract class BaseUrlGenerator {
     /**
      * @return string
      */
     abstract public function getProtocol();

     /**
      * @return string
      */
     public function generate()
     {
         return $this->getProtocol() . '://ya.ru';
     }
 }

 class HttpUrlGenerator extends BaseUrlGenerator {
     public function getProtocol()
     {
         return 'https';
     }
 }

 class FtpUrlGenerator extends BaseUrlGenerator {
     public function getProtocol()
     {
         return 'ftp';
     }
 }
{% endraw %}
{% endhighlight %}

### Посетитель. Visitor
Определяет операцию, выполняемую на каждом элементе из некоторой структуры. Позволяет, не изменяя классы этих объектов, добавлять в них новые операции. Участники:
 - Посетитель (Visitor) - объявляет операцию visit для каждого класса ConcreteElement в структуре объектов, который принимает один аргумент - указатель или ссылку на подкласс Element.
 - Конкретный посетитель (Concrete Visitor) - реализует все операции, объявленные в классе visitor.
 - Элемент (Element) - определяет операцию accept, которая принимает посетителя в качестве аргумента, а затем просто вызывает его метод visit(), передавая в качестве единственного параметра указатель this.
 - Конкретный элемент (ConcreteElement) - конкретный элемент.

{% highlight php %}
{% raw %}
 interface RichStringVisitor {
     public function visit(CanBeRichString $node);
 }

 class SizeRichStringVisitor implements RichStringVisitor {
     public function visit(CanBeRichString $node)
     {
         return mb_strlen($node->getString());
     }
 }

 class ToUpperRichStringVisitor implements RichStringVisitor {
     public function visit(CanBeRichString $node)
     {
         return mb_strtoupper($node->getString());
     }
 }

 class CanBeRichString {
     /**
      * @var string
      */
     private $string;

     public function __construct($string)
     {
         $this->string = $string;
     }

     /**
      * @return string
      */
     public function getString()
     {
         return $this->string;
     }

     public function accept(RichStringVisitor $visitor)
     {
         return $visitor->visit($this);
     }
 }
{% endraw %}
{% endhighlight %}
