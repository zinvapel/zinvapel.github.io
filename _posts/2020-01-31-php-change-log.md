---
layout: post
title: "[Шпаргалка] История версий PHP"
date: '2020-01-31'
category:
  - it
  - prog
---

{% include image.html src="/assets/it/prog/php-change-log.jpg" %}
Периодически приходится работать с различными версиями PHP. И я постоянно забываю, что и когда можно использовать. 
Конечно, современные IDE подсказывают что нельзя использовать, но они не подсказывают что можно. 
Поэтому приходится постоянно лезть сначала в один changelog, потом в другой. 
В этой шпаргалке собраны воедино все фичи актуальных версий.
Когда я говорю актуальных, то ссылаюсь на статистику [Packagist](https://blog.packagist.com/php-versions-stats-2019-2-edition/).  

<!--more-->
## 5.3
-  Closures
{% highlight php %}
$lambda = function () {
    echo 'Hello World!';
};
$lambda();

class myLambda
{
    public function __invoke() {echo 'Hello World!';}
}
$lambda = new myLambda;
$lambda();
{% endhighlight %}
-  Namespaces
-  Short ternary
{% highlight php %}
$first ?: $second; 
{% endhighlight %}
-  Labels:
{% highlight php %}
start:
// do_something();
goto start;
{% endhighlight %}
-  [Garbage Collector](https://www.php.net/manual/ru/features.gc.php)
-  `__callStatic`
-  Public Magic functions __get(), __set(), __isset(), __unset(), __call()

## 5.4
-  Traits
-  Short array syntax
{% highlight php %}
$arr = [1, 2, 3];
{% endhighlight %}
-  Array returned by function dereferencing
{% highlight php %}
foo()[0];

switch ($arr) {
    case [1, 2]:
}
{% endhighlight %}
-  Closures $this
{% highlight php %}
function () {
    $this->doSomething();
}
{% endhighlight %}
-  Class access on creation
{% highlight php %}
(new Bar())->foo();
{% endhighlight %}
-  Static call syntax with expressions
{% highlight php %}
Class::{$foo . '_' . $bar}()
{% endhighlight %}
-  Binary integers `0b0001`
-  Build-in WEB server

## 5.5
-  Generators
-  `finally`
-  list in foreach
{% highlight php %}
$arr = [[1, 2]];

foreach ($arr as list($one, $two)) {

}
{% endhighlight %}
-  Array and string dereferencing
{% highlight php %}
[1, 2, 3][2];
"string"[1];
{% endhighlight %}
-  Class fullname with `::class`
{% highlight php %}
namespace Foo\Bar;
class Baz {}
echo Baz::class; // Foo\Bar\Baz
{% endhighlight %}
-  [OPcache](https://www.php.net/manual/ru/book.opcache.php)

## 5.6
-  Constant expressions
{% highlight php %}
class Some {
    const A = 1;
    const B = self::A + 1; 
}
{% endhighlight %}
-  Constant array
{% highlight php %}
class A {
    const AR = [1, 2, 3];
}
{% endhighlight %}
-  Variadic function with argument unpacking
{% highlight php %}
function name(...$args) {}
$args = [5, 'Yellow', true];
name(...$args);
{% endhighlight %}
-  Exponential `**`
-  `use function` and `use const`
-  `__debugInfo` magic function

## 7.0
-  Scalar types argument
-  Return types `:type`
-  Null coalescing
{% highlight php %}
isset($foo) ? $foo : $bar;
$foo ?? $bar;
{% endhighlight %}
-  Spaceship operator
{% highlight php %}
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1
{% endhighlight %}
-  Constant array using define
-  Anonymous classes
-  Unicode escape symbols
{% highlight php %}
echo "\u{9999}";
{% endhighlight %}
-  `Closure::call($newthis)`
-  Grouped `use`
{% highlight php %}
use Foo\{Bar, Baz as Bar2};
{% endhighlight %}
-  Generator return
-  `yield from`
-  `intdiv` function
-  Throwable interface
-  Variables of variables
{% highlight php %}
                        PHP 5                   PHP 7
$$foo['bar']['baz']	    ${$foo['bar']['baz']}	($$foo)['bar']['baz']
$foo->$bar['baz']	    $foo->{$bar['baz']}	    ($foo->$bar)['baz']
$foo->$bar['baz']()	    $foo->{$bar['baz']}()	($foo->$bar)['baz']()
Foo::$bar['baz']()	    Foo::{$bar['baz']}()	(Foo::$bar)['baz']()
{% endhighlight %}
-  foreach does not change internal array point
{% highlight php %}
$array = [0, 1, 2];

foreach ($array as &$val) {
    var_dump(current($array));
}
// 5: int(1) int(2) bool(false)
// 7: int(0) int(0) int(0)
{% endhighlight %}

## 7.1
-  Nullable types `?int`
-  `void` return type
-  `iterable` pseudo-type
-  Constant visibility modifiers
-  List short syntax
-  List key bindings
-  Catching multiple exceptions types
-  `Closure::fromCallable`

## 7.2
-  Abstract functions overloading
{% highlight php %}
abstract class A           { abstract function bar(stdClass $x);  }
abstract class B extends A { abstract function bar($x): stdClass; }
{% endhighlight %}
-  Argument type extending
-  Trailing commas in namespaces
{% highlight php %}
use A\B\{C,D,E,};
{% endhighlight %}
-  `object` type hint

## 7.3
-  Flexible Heredoc
{% highlight php %}
foo(<<<HEREDOC

HEREDOC, true); 
{% endhighlight %}
-  Trailing commas in function call
-  `JSON_THROW_ON_ERROR`
-  `is_countable`
-  Multibyte string improvements

## 7.4 
-  Typed properties
{% highlight php %}
public int $count;
{% endhighlight %}
-  Arrow functions
{% highlight php %}
fn() => $this->x * 2
{% endhighlight %}
-  Null coalescing assignment
{% highlight php %}
// Before $data['date'] = $data['date'] ?? new DateTime();
$data['date'] ??= new DateTime(); 
{% endhighlight %}
-  Scodead operator for array
{% highlight php %}
$arr = [1, 2];
[0, ...$arr, 3, 4];
{% endhighlight %}
-  [FFI](https://www.php.net/manual/ru/class.ffi.php)
-  Preloading
-  Covariant returns and contravariant parameters
{% highlight php %}
interface A {
  function m(B $z);
}
interface B extends A {
  function m($z): A;
}
{% endhighlight %}
-  `__serialize`, `__unserialize`
-  `mb_str_split`
- WeakRef
- Numeric literal separator
{% highlight php %}
1_000_000_000;
{% endhighlight %}
- Deprecate left-associative ternary operator
