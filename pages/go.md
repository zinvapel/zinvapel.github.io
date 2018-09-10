---
layout: page
title: "[Конспект] Курс Mail.Ru по Go"
date: '2017-11-29'
category:
  - it
  - prog
  - lang
noindex: true
---

## Установка golang
1. Windows - загрузить инсталятор с оф. сайта https://golang.org/dl/ и следовать инструкциям https://golang.org/doc/install.

2. Linux - можно воспользоваться стандартным пакетным менеджером, скриптом https://gist.github.com/jniltinho/8758e15a9ef80a189fce или статьей https://www.tecmint.com/install-go-in-linux/.

3. MacOS - следуйте инструкциям с официального сайта (ссылки выше) или http://sourabhbajaj.com/mac-setup/Go/README.html.

Не забудьте перелогиниться после установки для обновления GOPATH - это бывает частой проблемой.

### Установка
{%- highlight bash -%}
$ brew install golang
{%- endhighlight -%}

### Настройка окружения
Go работает с системной переменной `$GOPATH` - адрес директории.

Добавить переменные окружения в `.bashrc` или `.bash_profile`.

{%- highlight bash -%}
export GOPATH=$HOME/go
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
{%- endhighlight -%}

### Создание директорий
Необходимо выполнить команды
{%- highlight bash -%}
$ mkdir -p $GOPATH $GOPATH/src $GOPATH/pkg $GOPATH/bin
{%- endhighlight -%}

- `bin` - содержит собранные бинарники.
- `pkg` - содержит временные файлы.
- `src` - содержит исходники программ.

### Создание программы
Все программы находятся в директории `$GOPATH/src`.

Чтобы исполнить программу выполняем команду:
{%- highlight bash -%}
$ go run hello.go
{%- endhighlight -%}

Чтобы скомпилировать программу выполняем команду:
{%- highlight bash -%}
$ go build hello.go
{%- endhighlight -%}

Если получить бинарный файл в `$GOPATH/bin`, то можно выполнять программу из любого места:
{%- highlight bash -%}
$ go install hello.go
{%- endhighlight -%}

Если программа находится в каталоге, то вызываем как:
{%- highlight bash -%}
$ go install <dirname>
{%- endhighlight -%}

Например:
> pavel@msk-wifi-6fap5-p_murtazin-p ~/go/src/mailrucourse > pwd
>
> /Users/pavel/go/src/mailrucourse
>
> pavel@msk-wifi-6fap5-p_murtazin-p ~/go/src/mailrucourse > go install mailrucourse
>
> pavel@msk-wifi-6fap5-p_murtazin-p ~/go/bin > mailrucourse
>
> hello, world

### Импорт Go-пакетов
Можно скачать пакеты с помощью команды `go get`:
{%- highlight bash -%}
$ go get -u github.com/gorilla/mux
{%- endhighlight -%}

Пакет будет размещен по пути `$GOPATH/src/github.com/gorilla/mux`.

Использовать импортированный пакет в коде можно так:
{%- highlight go -%}
package main

import (
    "net/http"
    "log"
    "github.com/gorilla/mux" // Your imported package
)

func YourHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Gorilla!\n")){%comment%}*{%endcomment%}
}

func main() {
    r := mux.NewRouter()
    // Routes consist of a path and a handler function.
    r.HandleFunc("/", YourHandler)

    // Bind to a port and pass our router in
    log.Fatal(http.ListenAndServe(":8000", r))
}
{%- endhighlight -%}

Форматирование кода осуществляется с помощью команд `go fmt path/to/your/package` для пакета, `gofmt path/to/your/file` для одиночного файла. Флаг `-w` сразу перезапишет файл.

Чтобы просмотреть документацию используется команда `godoc`.
{%- highlight bash -%}
$ godoc fmt                # документация пакета fmt
$ godoc fmt Printf         # документация функции fmt.Printf
$ godoc -src fmt           # исходный код пакета fmt
{%- endhighlight -%}

## Основы языка
### Переменные
Переменная в go объявляется при помощи ключевого слова `var`, после которой следует имя переменной и тип переменной. Тип переменной не может меняться.

При этом вы можете не указывать значения при инициализации, и тогда переменная будет инициализирована значением по умолчанию. Для некоторых типов данных вы можете пропустить указание типа, и компилятор определит этот тип автоматически. Используя короткое объявление с помощью оператора `:=`, вы можете объявить только новую переменную.

{%- highlight go -%}
// По умолчанию 0
var num0 int32

// При инициализации
var num1 int32 = 1

// Пропуск типа
var num2 = 32

// Короткое объявление
num3 := 14
{%- endhighlight -%}

Есть операторы постфиксный-`++` и `+=`.

{%- highlight go -%}
num0++
num1 += 1
{%- endhighlight -%}

Общепринятый стиль объявления переменных - `camelCase`.

Можно объявить сразу несколько переменных.
{%- highlight go -%}
// Объявление нескольких переменных с инициализацией
var weight, height int = 10, 20

// Присваивание в существующие переменные
weight, height = 11, 21

// Короткое присваивание
// Хотя-бы одна переменная должна быть новой!
weight, age := 12, 22
{%- endhighlight -%}

Неиспользуемые переменные вызовут ошибку компилятора.

### Простые типы
#### Числовые
Целые числа:
{%- highlight go -%}
// int - платформозависимый тип, 32/64
var i int = 10

// Автоматически выбранный int
var autoInt = -10

// Можно явно указать int8, int16, int32, int64
var bigInt int64 = 1<<32 - 1

// Беззнаковый платформозависимый тип, 32/64
var unsignedInt uint = 100500

// Беззнаковый явный uint8, unit16, uint32, unit64
var unsignedBigInt uint64 = 1<<64 - 1
{%- endhighlight -%}

Числа с плавающей точкой:
{%- highlight go -%}
// Явные float32, float64
var pi float32 = 3.141

// Автоматически выбранный float
var e = 2.718

// 0.0 по-умолчанию
var zero float32
{%- endhighlight -%}

Булевый тип:
{%- highlight go -%}
// false по-умолчанию
var b bool

// Явно указанный
var isOk bool = true

// Автоматически выведенный компилятором
var success = true
{%- endhighlight -%}

Комплексные числа:
{%- highlight go -%}
// complex64, complex128
var c complex128 = -1.1 + 7.12i

// Автоматически выведенный платформозависимый
c2 := -1.1 + 7.12i
{%- endhighlight -%}

#### Строки
Литералами являются двойные и обратные кавычки, причем для двойных доступны спецсимволы, а для обратных - нет. UTF-8 поддерживается по-умолчанию.
{%- highlight go -%}
// Пустая строка по-умолчанию
var str string

// Со спец символами
var hello string = "Привет\n\t"

// Без спец символов
var world string = `Мир\n\t`

// UTF-8 из коробки
var helloWorld = "Привет, Мир!"
hi := "你好，世界"
{%- endhighlight -%}

Символы в Go представлены двумя типами - `byte`, алиас от `uint8`, использует одинарные кавычки, а для UTF-8 символов используется тип `rune`, который является алиасом для `uint32`.
{%- highlight go -%}
// Одинарные кавычки для байт (uint8)
var rawBinary byte = '\x27'

// Тип rune (uint32) для UTF-8 символов
var someChinese rune = '茶'
{%- endhighlight -%}

Оператором конкатенации является `+`. Строки неизменяемы.
{%- highlight go -%}
// Конкатенация строк
andGoodMorning := helloWorld + " и доброе утро!"

// Строки неизменяемы
// cannot assign to helloWorld[0]
helloWorld[0] = 72
{%- endhighlight -%}

Узнать длину строки можно с помощью функций `len` - количество байт, `utf8.RuneCountInString` - количество рун (символов)
{%- highlight go -%}
// получение длины строки
byteLen := len(helloWorld)                    // 19 байт
symbols := utf8.RuneCountInString(helloWorld) // 10 рун
{%- endhighlight -%}

Получение подстроки осуществляется с помощью оператора среза `[from_include:to_exclude]`. Срез снимается в __байтах__. Причем срез длиной в единицу вернет тип `byte`.
{%- highlight go -%}
// Получение подстроки, в БАЙТАХ, не символах
hello := helloWorld[:12] // Привет, 0-11 байты
H := helloWorld[0]       // byte, 72, не "П"
{%- endhighlight -%}

Строку межно конвертировать в slice байт и обратно.
{%- highlight go -%}
// Конвертация в слайс байт и обратно
byteString = []byte(helloWorld)
helloWorld = string(byteString)
{%- endhighlight -%}

#### Константы
Константы определяются с помощью ключевого слова `const`. Можно объявить сразу несколько констант.
{%- highlight go -%}
const pi = 3.141

const (
    hello = "Привет"
    e     = 2.718
)
{%- endhighlight -%}

При объявлении нескольких констант можно объявить специальное значение `iota` - автоинкремент для константы. Если объявить `iota`, то в одном блоке констант она будет увеличиваться на единицу, а сама константа будет равна предыдущему выражению с `iota`. Пропуск значение декларируется через `_`.
{%- highlight go -%}
const (
    zero  = iota
    _     // Пустая переменная, пропуск iota
    three // = 3
)

const (
    _         = iota             // пропускаем первое значне
    KB uint64 = 1 << (10 * iota) // 1024
    MB                           // 1048576
)
{%- endhighlight -%}

Нетипизированные константы имеют возможность конвертироваться в необходимый тип.
{%- highlight go -%}
const (
    // нетипизированная константа
    year = 2017
    // типизированная константа
    yearTyped int = 2017
)

func main() {
    var month int32 = 13
    fmt.Println(month + year)

    var day int64 = 123
    fmt.Println(day + year)

    // month + yearTyped (mismatched types int32 and int)
    // fmt.Println( month + yearTyped )
}
{%- endhighlight -%}

#### Определение типов
Можно определить собственный тип с помощью ключевого слова `type`. П
{%- highlight go -%}
package main

type UserID int

func main() {
    idx := 1
    var uid UserID = 42

    // Даже если базовый тип одинаковый, разные типы несовместимы
    // cannot use uid (type UserID) as type int64 in assignment
    // myID := idx
}
{%- endhighlight -%}

ри этом даже тип-алиас не будет совместим с исходным. В Go нет автоматического приведения типов. Можно привести совместимые типы:
{%- highlight go -%}
myID := UserID(idx)

println(uid, myID)
{%- endhighlight -%}

### Составные типы
#### Указатели
В Go нет адресной арифметики. Нельзя прибавить какое-то значение к указателю и получить указатель на другую область памяти.

В Go указатель — это отдельный тип данных. Указатели, это переменные, которые хранят ссылки на другие переменные.

Создать ссылку можно с помощью `&`. Сам указатель имеет тип `*{type}`, где `{type}` - тип на который он ссылается. Получить значение, которое стоит за ссылкой, можно с помощью `*`.
{%- highlight go -%}
package main

import "fmt"

func main() {
    a := 2

    // Создание указателя на a, тип *int{% comment %}*{% endcomment %}
    b := &a

    // Установка значения за ссылку
    *b = 3  // a = 3{% comment %}*{% endcomment %}

    c := &a // новый указатель на переменную a
}
{%- endhighlight -%}
С функции `new` можно создать указатель на значение по-умолчанию для типа.
{%- highlight go -%}
// Получение указателя на переменнут типа int
// Инициализировано значением по-умолчанию
d := new(int)
*d = 12
*c = *d // c = 12 -> a = 12
*d = 13 // c и a не изменились

c = d   // теперь с указывает туда же, куда d
*c = 14 // с = 14 -> d = 14, a = 12{% comment %}*{% endcomment %}
{%- endhighlight -%}

#### Массив
Массив - набор элементов одного типа.

В Go длина массива является частью его типа. То есть массив размера 2 и массив размера 3 являются разными типами данных.

Массив объявляется с помощью `[len]type{elements_if_exist}`.
{%- highlight go -%}
// Инициализация значениями по-умолчанию
var a1 [3]int // [0,0,0]
{%- endhighlight -%}

Можно использовать константы для вычисления длины.
{%- highlight go -%}
const size = 2
var a2 [2 * size]bool // [false,false,false,false]
{%- endhighlight -%}

Можно вычислять длину массива при инициализации.
{%- highlight go -%}
// определение размера при объявлении
a3 := [...]int{1, 2, 3}
{%- endhighlight -%}

При обращении к несуществующему элементу массива программа завершится с `panic`.

#### Срез
Срез `slice` - тип данных, у которого есть свойства длины `len` - количество элементов в срезе, и вместимости `cap` - количество максимального количества элементов без перестроения памяти.
{%- highlight go -%}
// Создание slice
var buf0 []int             // len=0, cap=0
buf1 := []int{}            // len=0, cap=0
buf2 := []int{42}          // len=1, cap=1
buf3 := make([]int, 0)     // len=0, cap=0 []int{}
buf4 := make([]int, 5)     // len=5, cap=5 []int{0,0,0,0,0}
buf5 := make([]int, 5, 10) // len=5, cap=10 []int{0,0,0,0,0}
{%- endhighlight -%}

Обращение к элементам подобно массивам. Добавление элементов с помощью функции `append`. Если размер выходит за пределы вместимости, то вместимость увеличивается в два раза.
{%- highlight go -%}
// Ошибка при обращении к элементу за пределами len
// panic: runtime error: index out of range
// someOtherInt := buf2[1]

// Добавление элементов
var buf []int            // len=0, cap=0
buf = append(buf, 9, 10) // len=2, cap=2
buf = append(buf, 12)    // len=3, cap=4
{%- endhighlight -%}

Можно добавить другой slice с помощью оператора `...`.
{%- highlight go -%}
// добавление друго слайса
otherBuf := make([]int, 3)     // [0,0,0]
buf = append(buf, otherBuf...) // len=6, cap=8
{%- endhighlight -%}

С помощью `len` можно получить длину, а `cap` - вместимость.

Можно взять часть slice, которая будет ссылаться на часть исходного slice. При этом, при изменении `capacity` подсреза, ссылка на исходный срез будет утеряна.
{%- highlight go -%}
buf := []int{1, 2, 3, 4, 5}

// Получение среза, указывающего на ту же память
sl1 := buf[1:4] // [2, 3, 4]
sl2 := buf[:2]  // [1, 2]
sl3 := buf[2:]  // [3, 4, 5]

// Получение полного подсреза
newBuf := buf[:] // [1, 2, 3, 4, 5]

// Получаем buf = [9, 2, 3, 4, 5], т.к. та же память
newBuf[0] = 9

// Capacity увеличился, слайс перенесен в новую область памяти
// Теперь newBuf указывает на другие данные
newBuf = append(newBuf, 6)

// buf    = [9, 2, 3, 4, 5], не изменился
// newBuf = [1, 2, 3, 4, 5, 6], изменился
newBuf[0] = 1
{%- endhighlight -%}

Копирование slice осуществляется с помощью функции `copy`. При этом она копирует в существующие элементы. То есть если у slice len = 0, то ничего скопировано не будет.
{%- highlight go -%}
// Копирование одного слайса в другой
var emptyBuf []int // len=0, cap=0

// Неправильно - скопирует меньшее (по len) из 2-х слайсов
copied := copy(emptyBuf, buf) // copied = 0

// Правильно
newBuf = make([]int, len(buf), len(buf))
copy(newBuf, buf)
fmt.Println(newBuf)

// Можно копировать в часть существующего слайса
ints := []int{1, 2, 3, 4}
copy(ints[1:3], []int{5, 6}) // ints = [1, 5, 6, 4]
fmt.Println(ints)
{%- endhighlight -%}

#### Хеш-таблицы
Хеш-таблица `map` позволяет по ключу получить значение. Объявляется как `map[key_type]value_type{elems}`.
{%- highlight go -%}
// Инициализация при создании
var user map[string]string = map[string]string{
    "name":     "Some Name",
    "lastName": "Some Last Name",
}
{%- endhighlight -%}

Подобно срезам map имеет длину и вместимость.
{%- highlight go -%}
// Сразу с нужной вместимостью
profile := make(map[string]string, 10)

// Количество элементов 0
mapLength := len(profile)
{%- endhighlight -%}

Получение значения. Если значения нет, то вернет nil.
{%- highlight go -%}
// Если ключа нет - вернёт значение по умолчанию для типа
mName := user["middleName"]
fmt.Println("mName:", mName)
{%- endhighlight -%}

Проверка на существование ключа:
{%- highlight go -%}
// Проверка на существование ключа
mName, mNameExist := user["middleName"]
fmt.Println("mName:", mName, "mNameExist:", mNameExist)

// Пустая переменная - только проверяем что ключ есть
_{% comment %}_{% endcomment %}, mNameExist2 := user["middleName"]
fmt.Println("mNameExist2", mNameExist2)
{%- endhighlight -%}

Удаление ключа с помощью функции `delete`.
{%- highlight go -%}
// Удаление ключа
delete(user, "lastName")
{%- endhighlight -%}

### Управляющие конструкции
#### Условный оператор if
Условный оператор представлен конструкцией `if`. В качестве условия всегда должен выступать `bool` тип.
{%- highlight go -%}
// Простое условие
boolVal := true
if boolVal {
    fmt.Println("boolVal is true")
}

mapVal := map[string]string{"name": "zinvapel"}
// Условие с блоком инициализации
if keyValue, keyExist := mapVal["name"]; keyExist {
    fmt.Println("name =", keyValue)
}
// Получаем только признак сущестования ключа
if _,{% comment %}_{% endcomment %} keyExist := mapVal["name"]; keyExist {
    fmt.Println("key 'name' exist")
}
{%- endhighlight -%}

Go поддерживает `else if`, `else`.
{%- highlight go -%}
cond := 1
// Множественные if else
if cond == 1 {
    fmt.Println("cond is 1")
} else if cond == 2 {
    fmt.Println("cond is 2")
} else {
    fmt.Println("cond is other")
}
{%- endhighlight -%}

#### Условный оператор switch
Существует 2 типа оператора `switch`. По одной переменной и без переменных. По умолчанию каждый кейс не проваливается, чтобы принудительно прокинуть вызов вниз используется `fallthrough`.
{%- highlight go -%}
// switch по 1 переменной
strVal := "name"
switch strVal {
case "name":
    fallthrough
case "test", "lastName":
    // some work
default:
    // some work
}

// switch как замена многим ifelse
var val1, val2 = 2, 2
switch {
case val1 > 1 || val2 < 11:
    fmt.Println("first block")
case val2 > 10:
    fmt.Println("second block")
}
{%- endhighlight -%}

Чтобы выйти из кейса используется оператор `break`.
{%- highlight go -%}
    // Выход из цикла, находясь внутри switch
Loop:
    for key, val := range mapVal {
        println("switch in loop", key, val)
        switch {
        case key == "lastName":
            break
            println("dont pront this")
        case key == "firstName" && val == "Pavel":
            println("switch - break loop here")
            break Loop
        }
    }
{%- endhighlight -%}

#### Циклы
Циклы в Go представлены одним оператором `for`, но он может принимать различные виды. `break` - выходит из цикла, `continue` - переходит к следующей итерации. Операции по коллекциям осуществляются с помощью оператора `range`.
{%- highlight go -%}
// Бесконечный цикл
for {
    fmt.Println("loop iteration")
    break
}

// Цикл с условием
isRun := true
for isRun {
    fmt.Println("loop iteration with condition")
    isRun = false
}

// Цикл с условием и блоком инициализации
for i := 0; i < 2; i++ {
    fmt.Println("loop iteration", i)
    if i == 1 {
        continue
    }
}

// Операции по slice
sl := []int{1, 2, 3}
idx := 0

for idx < len(sl) {
    fmt.Println("while-stype loop, idx:", idx, "value:", sl[idx])
    idx++
}

for i := 0; i < len(sl); i++ {
    fmt.Println("c-style loop", i, sl[i])
}
for idx := range sl {
    fmt.Println("range slice by index", sl[idx])
}
for idx, val := range sl {
    fmt.Println("range slice by idx-value", idx, val)
}

// Операции по map
profile := map[int]string{1: "Mr", 2: "Smith"}

for key := range profile {
    fmt.Println("range map by key", key)
}

for key, val := range profile {
    fmt.Println("range map by key-val", key, val)
}

for _{% comment %}_{% endcomment %}, val := range profile {
    fmt.Println("range map by val", val)
}

str := "Привет, Мир!"
for pos, char := range str { // Возвращается rune
    fmt.Printf("%#U at pos %d\n", char, pos)
}
{%- endhighlight -%}

### Функции
Функции объявляются с помощью конструкции `func (arg1 type, argN typeN) returnType {}`. Значение возвращается с помощью оператора `return`.
{%- highlight go -%}
// Обычное объявление
func singleIn(in int) int {
    return in
}

// Много параметров одного типа (a, b)
func multIn(a, b int, c int) int {
    return a + b + c
}

// Именованный результат, в скобках указывается переменная, которая будет возвращена.
// При объявлении можно принудительно вернуть любое значение соответствующего типа.
func namedReturn() (out int) {
    out = 2
    return
}

// Несколько результатов
func multipleReturn(in int) (int, error) {
    if in > 2 {
        return 0, fmt.Errorf("some error happend")
    }
    return in, nil
}

// Несколько именованных результатов
func multipleNamedReturn(ok bool) (rez int, err error) {
    rez = 1
    if ok {
        err = fmt.Errorf("some error happend")
        // аналогично return rez, err
        // return 1, fmt.Errorf("some error happend")
        return
    }
    rez = 2
    return
}
{%- endhighlight -%}

Вариативные функции могут принимать переменное количество аргументов, но одного типа.
{%- highlight go -%}
// Переменное количество параметров
func sum(in ...int) (result int) {
    fmt.Printf("in := %#v \n", in)
    for _{% comment %}_{% endcomment %}, val := range in {
        result += val
    }
    return
}

nums := []int{1, 2, 3, 4}
fmt.Println(nums, sum(nums...))
{%- endhighlight -%}

Функции являются объектом первого класса. Можно присваивать функцию в переменную, передавать в качестве аргумента и возвращать. Можно вызывать функцию в месте объявления.
{%- highlight go -%}
// Анонимная функция с вызовом
func(in string) {
    fmt.Println("anon func out:", in)
}("nobody")

// Присваивание анонимной функции в переменную
printer := func(in string) {
    fmt.Println("printer outs:", in)
}
printer("as variable")

// Определяем тип функции
type strFuncType func(string, int) (string, error)

// Функция принимает другую функцию
worker := func(callback func(string)) {
    callback("as callback")
}
worker(printer)

// Функция возвращает функцию-замыкание
prefixer := func(prefix string) strFuncType {
    return func(in string) {
        fmt.Printf("[%s] %s\n", prefix, in)
    }
}
{%- endhighlight -%}

Отложить выполнение функции можно с помощью оператора `defer`.

Функции с defer выполняются после выполнения текущей функции. Причем в порядке обратном к их объявлению.

Переменные для отложенных функций вычисляются в момент вычисления.
{%- highlight go -%}
func getSomeVars() string {
    fmt.Println("getSomeVars execution")
    return "getSomeVars result"
}

func main() {
    defer fmt.Println("After work")
    defer func() {
        fmt.Println(getSomeVars())
    }()
    fmt.Println("Some userful work")
}

// Some userful work
// getSomeVars execution
// getSomeVars result
// After work
{%- endhighlight -%}

Порядок выполнения:
- Вычисление переменных пакета.
- Функция `init` пакета `main`.
- Функция `init` всех импортируемых пакетов.
- Функция `main`.

```
importPackage.variableCreation
importPackage.Init
main.variableCreation
main.Init
main.main
```

### Паника
Функция `panic(string)` останавливает выполнение программы. При этом при возникновении паники `defer` всё равно отработает.

Получить ошибку паники можно с помощью функции `recover`.
{%- highlight go -%}
func deferTest() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Println("panic happend FIRST:", err)
        }
    }()
    defer func() {
        if err := recover(); err != nil {
            fmt.Println("panic happend SECOND:", err)
             panic("second panic")
        }
    }()
    fmt.Println("Some userful work")
    panic("something bad happend")
    return
}
{%- endhighlight -%}

### Структуры данных
Структуры используются для объединения более простых типов. Объявляется с помощью конструкции `type Name struct {fields}`.
{%- highlight go -%}
type Person struct {
    Id      int
    Name    string
    Address string
}
{%- endhighlight -%}

Создать структуру можно с помощью полного объявления (необъявленые поля примут значение по-умолчанию), а также короткого - нельзя пропускать поля структуры.

Каждое поле при создании заканчивается запятой.
{%- highlight go -%}
// Полное объявление структуры
var person Person = Person{
    Id: 1,
    Address: "Moscow, Lenina, 1"
}

// Короткое объявление структуры
var person2 Person = Person{2, "Pavel", "Moscow, Lenina, 2"}
{%- endhighlight -%}

Обращение к полям структуры происходит через точку.
{%- highlight go -%}
fmt.Println("Name is", person2.Name)
{%- endhighlight -%}

В Go нет ООП в классическом понимании, но можно встроить одну структуру в другую, таким образом получив возможность использовать его поля.
{%- highlight go -%}
type Unit struct {
    Name string
}

type FlyingUnit struct {
    MaxDistance uint32
    Unit // Наследует Name
}
{%- endhighlight -%}

Объявлять поля вложенной структуры необходимо явно.
{%- highlight go -%}
u := FlyingUnit{
    Unit: Unit{
        Name: "Dragon",
    },
}
{%- endhighlight -%}

Если есть одноименные поля, то они оба сохраняются, но для получения родительского поля нужно будет явно его указать.
{%- highlight go -%}
type FlyingUnit struct {
    MaxDistance uint32
    Name string // Одноименное
    Unit
}

u := FlyingUnit{
    MaxDistance: 1400,
    Name: "Dracaris",
    Unit: Unit{
        Name: "Dragon",
    },
}

fmt.Println(u.Name, u.Unit.Name)
{%- endhighlight -%}

### Методы структур
Метод от функции отличается тем, что перед именем указывается тип для которого этот метод определяется.

Может быть как передача по значению, так и по ссылке, при вызове компилятор определит это автоматически на основании объявления.
{%- highlight go -%}
// Не изменит оригинальной структуры, для который вызван
func (p Person) UpdateName(name string) {
    p.Name = name
}

// Изменяет оригинальную структуру
func (p *{% comment %}*{% endcomment %}Person) SetName(name string) {
    p.Name = name
}
{%- endhighlight -%}

Вызов родительских методов работает по тому же принципу, что и поля структуры.

При этом операция выполняется над полем той структуры, тип которой объявлен в определении метода.
{%- highlight go -%}
func (u *{% comment %}*{% endcomment %}Unit) PrintName() {
    fmt.Println(u.Name)
}

func (fu *{% comment %}*{% endcomment %}FlyingUnit) PrintName() {
    fmt.Println(fu.Name)
}
u.PrintName() // Dracaris
u.Unit.PrintName() // Dragon
{%- endhighlight -%}

Можно также переопределить тип без создания структуры и добавить ему методов.
{%- highlight go -%}
type Slice []int

func (sl *{% comment %}*{% endcomment %}Slice) Add(val int) {
    *{% comment %}*{% endcomment %}sl = append(*{% comment %}*{% endcomment %}sl, val)
}
{%- endhighlight -%}

### Пакеты
Главный пакет имеет фиксированное имя `main`. Объявляется в начале файла с помощью ключевого слова `package`.
{%- highlight go -%}
package code
{%- endhighlight -%}

Для дополнительных пакетов именем является имя директории, в которой он лежит.
{%- highlight list -%}
~/go
├───bin
├───pkg
└───src
    ├───coursera
    │   ├───visibility
    │   │   │───person
    │   │   │   │───person.go // Пакет person
    │   │   │   └───func.go // Пакет person
    │   │   └───main.go
    └───github.com
        └───zinvapel
            └───zinvapel.github.io
{%- endhighlight -%}

Область видимости функции, констант, переменных, методов:
- Имя начинается со строчной (маленькой) буквы - приватная область, видимость только внутри пакета.
- Имя начинается со прописной (большой) буквы - публичная область, видимость везде, где импортируется пакет.
{%- highlight go -%}
package unit

type Unit struct {
    Name string // Доступен везде, где импортируется пакет unit
    id uint // Доступен во всех файлах пакета unit
}

// Доступен во всех файлах пакета unit
func (u *{% comment %}*{% endcomment %}Unit) printIdentifier() {
    fmt.Println(u.id)
}

// Доступен везде, где импортируется пакет unit
func (u *{% comment %}*{% endcomment %}Unit) PrintIdentity() {
    u.printIdentifier()
}
{%- endhighlight -%}

Чтобы импортировать пакет, необходимо использовать ключевое слово `import` и путь до пакета относительно директории `src`.

Область видимости импортируемых пакетов внутри файла, то есть для разных файлов одного пакета нужно писать отдельные импорты.

Несколько импортов указывается в круглых скобках с новой строчки. Неиспользуемые импорты запрещены. Алиасы указываются перед пакетом.
{%- highlight go -%}
import (
    "fmt"
    lpq "github.com/lib/pq"
)
{%- endhighlight -%}

Файлы, заканчивающиеся на `_darwin`, `_linux`, `_windows` будут использоваться только для систем Mac OS, Linux, Windows, соответственно.

### Интерфейсы
Через интерфейсы в Go реализован полиморфизм.

В Go используется «утиная типизация» - если что-то выглядит как утка, крякает как утка, то это и есть утка.

При объявлении интерфейса мы задаем методы, которыми он должен обладать. В дальнейшем мы можем указывать в функциях в качестве типа аргумента какой-либо интерфейс, а передавать ту структуру, которая имеет соответствующие методы.

Объявляется с помощью `type Name interface {method list}`.
{%- highlight go -%}
package marathon

type Runner interface {
    Run(uint) uint
}

type Men struct {
    Name string
    maxDistance uint
}

func (m *{% comment %}*{% endcomment %}Men) Run(distance uint) (r uint) {
    r = distance

    if distance > m.maxDistance {
        r = m.maxDistance
    }

    return
}

func Marathon(runners []Runner, distance uint) {
    for _{% comment %}_{% endcomment %}, runner := range runners {
        runner.Run(distance)
    }
}
{%- endhighlight -%}

#### Приведение типов для интерфейсов
Привести тип можно с помощью конструкции `interfaceVariable.(*typeTo)`.

Можно уточнить тип с помощью *type-switch*.
{%- highlight go -%}
func MarathonDeclaration(runner Runner) {
    unknownRunner := func () {
        fmt.Println("Бежит неизвестный бегун!")
    }

    switch runner.(type) {
    case *{% comment %}*{% endcomment %}Men:
        if menRunner, ok := runner.(*{% comment %}*{% endcomment %}Men); ok {
            fmt.Println("Бежит", menRunner.Name)
        } else {
            unknownRunner()
        }
    default:
        unknownRunner()
    }
}
{%- endhighlight -%}

Можно объявить пустой интерфейс. Они нужны для динамических функций, которые могут работать с чем угодно.
{%- highlight go -%}
func SomeFunc(in interface{}) {
    // Попытка преобразования к ConcreteInterface
    // result всегда преобразуется в тип, возможно это будет значение по-умолчанию
    // ok - bool значение указывает удалось преобразовать или нет
    if result, ok := in.(ConcreteInterface); ok {
        return
    }
}
{%- endhighlight -%}

Интерфейсы можно встраивать друг в друга.
{%- highlight go -%}
type Payer interface {
    Pay(int) error
}

type Ringer interface {
    Ring(string) error
}

type NFCPhone interface {
    Payer
    Ringer
}
{%- endhighlight -%}

## Тестирование
Тесты лежат в том же пакете, но имеют окончание файла `_test`.

Все тестовые функции начинаются с префикса `Test`, на вход принимают параметром тестирующий модуль из пакета `testing`.

Все ассерты делаются вручную. Если мы хотим сообщить об ошибке, то вызываем метод `testing.T.Errorf`.
{%- highlight go -%}
package ts

import "testing"

var menMax, much, less uint = 10, 100, 5

func TestMen_Run(t *{% comment %}*{% endcomment %}testing.T) {
    men := Men{maxDistance: menMax}

    if result := men.Run(much); result != menMax {
        t.Errorf("Expected value is", menMax)
    }

    if result := men.Run(less); result != less {
        t.Errorf("Expected value is", less)
    }
}
{%- endhighlight -%}

Запускаются тесты с помощью команды `go test filename.go`

Чтобы посчитать покрытие используется флаг `-cover`. Чтобы посторить отчет `-coverprofile=file.out`. Полученный фал можно трансформировать в `html` с помощью `go tool cover`.

## Асинхронное программирование
Чтобы выполнить функцию в другом потоке используется ключевое слово `go`. Такие функции называются `горутинами`.
{%- highlight go -%}
go func() {
    fmt.Println("Do this async!")
}()
{%- endhighlight -%}

Чтобы передать работу другой горутине используется функция `runtime.Gosched()`.

Горутины не могут возвращать данные, поэтому для передачи данных между горутинами используются каналы, имеют ти `chan <type>`. Каналы типизированы.

Для того чтобы отправить данные в канал используется правое `<-`, а для получения левое.
{%- highlight go -%}
go func(in chan int) {
    val := <-in
    fmt.Println("GO: get from chan", val)
}(ch1)

ch1 <- 42
{%- endhighlight -%}

Канал имеет ограниченный буфер. То есть если из канала никто не прочитает, то текущая горутина, которая отправила данные в канал, будет ждать когда из канала прочитают.
Функция, которая читает из канала будет ждать когда в канал положат значение.

Можно контролировать величину буфера (количество сообщений перед тем как произойдет блокировка).
{%- highlight go -%}
ch1 := make(chan int, 1)
{%- endhighlight -%}

Как объявить канал.
{%- highlight go -%}
// Канал с буфером 10
ch1 = make(chan int, 10)

// Канал из которого можно только читать
go func (ch <-chan int) {
    fmt.Println(<- ch)
}(ch1)

// Канал в который можно только писать
func (ch chan<- int) {
    ch <- 42
}(ch1)
{%- endhighlight -%}

При обходе канала с помощью `range` его необходимо закрывать с помощью функции `close` во избежание deadlock. Например, мы передали буферизированный на единицу канал в горутину, которая читает его через `for range`. Передали несколько значений, а затем не закрыли. Горутина будет ждать канал.

Для работы с несколькими каналами одновременно используется `select`.
{%- highlight go -%}
ch1 := make(chan int, 1)
ch2 := make(chan int)

select {
// Если кто-то запишет в канал
case val := <-ch1:
    fmt.Println("ch1 val", val)
// Если кто-то читает из канала
case ch2 <- 1:
    fmt.Println("put val to ch2")
// В ином случае
 default:
    fmt.Println("default case")
}
{%- endhighlight -%}

### Инструменты для многопроцессорного программирования
Таймаут можно создать с помощью `time.Timer`.
{%- highlight go -%}
package main

import (
    "fmt"
    "time"
)

func longSQLQuery() chan bool {
    ch := make(chan bool, 1)
    go func() {
        time.Sleep(5 * time.Second)
        ch <- true
    }()
    return ch
}

func main() {
    timer := time.NewTimer(3 * time.Second)
    select {
    case <-timer.C:
        // Выполнится через 3 секунды
        fmt.Println("timer.C timeout happend")
        // Выполнится через 1 секунду, но
        // Сразу создает таймер и будет занимать ресурсы
    case <-time.After(1 * time.Second):
        fmt.Println("time.After timeout happend")
    case result := <-longSQLQuery():
        // Освобождет ресурс, если longSQLQuery выполнится раньше таймеров, то закончится выполнение
        if !timer.Stop() {
            <-timer.C
        }
        fmt.Println("operation result:", result)
    }
}
{%- endhighlight -%}

Создать периодические события можно с помощью `time.Ticker`.
{%- highlight go -%}
func main() {
    ticker := time.NewTicker(time.Second)
    i := 0
    for tickTime := range ticker.C {
        i++
        fmt.Println("step", i, "time", tickTime)
        if i >= 5 {
            // надо останавливать, иначе потечет
            ticker.Stop()
            break
        }
    }
    fmt.Println("total", i)

    // не может быть остановлен и собран сборщиком мусора
    // используйте если должен работать вечено
    c := time.Tick(time.Second)
    i = 0
    for tickTime := range c {
        i++
        fmt.Println("step", i, "time", tickTime)
        if i >= 5 {
            break
        }
    }
}
{%- endhighlight -%}

Отложеные операцию создаются с помощью `time.AfterFunc`.
{%- highlight go -%}
func main() {
    timer := time.AfterFunc(1 * time.Second, sayHello)

    fmt.Scanln()
    timer.Stop()

    fmt.Scanln()
}
{%- endhighlight -%}

### Отмена асинхронных операций
В Go это реализуется с помощью пакета `context`. Для реализации данных операций используется select, функции контекста `Done`, `finish`.
{%- highlight go -%}
package main

import (
    "context"
    "fmt"
    "math/rand"
    "time"
)

func worker(ctx context.Context, workerNum int, out chan<- int) {
    waitTime := time.Duration(rand.Intn(100)+10) * time.Millisecond
    fmt.Println(workerNum, "sleep", waitTime)
    select {
    case <-ctx.Done():
        return
    case <-time.After(waitTime):
        fmt.Println("worker", workerNum, "done")
        out <- workerNum
    }
}

func main() {
    ctx, finishFunc := context.WithCancel(context.Background())
    result := make(chan int, 1)

    for i := 0; i <= 10; i++ {
        go worker(ctx, i, result)
    }

    foundBy := <-result
    fmt.Println("result found by", foundBy)
    finishFunc()

    time.Sleep(time.Second)
}
{%- endhighlight -%}

{%- highlight go -%}
package main

import (
    "context"
    "fmt"
    "math/rand"
    "time"
)

func worker(ctx context.Context, workerNum int, out chan<- int) {
    waitTime := time.Duration(rand.Intn(100)+10) * time.Millisecond
    fmt.Println(workerNum, "sleep", waitTime)
    select {
    case <-ctx.Done():
        return
    case <-time.After(waitTime):
        fmt.Println("worker", workerNum, "done")
        out <- workerNum
    }
}

func main() {
    workTime := 50 * time.Millisecond
    ctx, _ := context.WithTimeout(context.Background(), workTime)
    result := make(chan int, 1)

    for i := 0; i <= 10; i++ {
        go worker(ctx, i, result)
    }

    totalFound := 0
LOOP:
    for {
        select {
        case <-ctx.Done():
            break LOOP
        case foundBy := <-result:
            totalFound++
            fmt.Println("result found by", foundBy)
        }
    }
    fmt.Println("totalFound", totalFound)
    time.Sleep(time.Second)
}
{%- endhighlight -%}

Чтобы дождаться выполнения пачки горутин используется `sync.WaitGroup`.
{%- highlight go -%}
package main

import (
    "fmt"
    "runtime"
    "strings"
    "sync"
    "time"
)

const (
    iterationsNum = 7
    goroutinesNum = 5
)

func startWorker(in int, wg *{%comment%}*{%endcomment%}sync.WaitGroup) {
    defer wg.Done() // wait_2.go уменьшаем счетчик на 1
    for j := 0; j < iterationsNum; j++ {
        fmt.Printf(formatWork(in, j))
        runtime.Gosched()
    }
}

func main() {
    wg := &sync.WaitGroup{} // wait_2.go инициализируем группу
    for i := 0; i < goroutinesNum; i++ {
        wg.Add(1) // wait_2.go добавляем воркер
        go startWorker(i, wg)
    }
    time.Sleep(time.Millisecond)

    // fmt.Scanln()
    wg.Wait() // wait_2.go ожидаем, пока waiter.Done() не приведёт счетчик к 0
}

func formatWork(in, j int) string {
    return fmt.Sprintln(strings.Repeat("  ", in), "█",
        strings.Repeat("  ", goroutinesNum-in),
        "th", in,
        "iter", j, strings.Repeat("■", j))
}
{%- endhighlight -%}

### Состояние гонки
Для структур в Go возможны состояния **race condition**. Чтобы отследить их используется ключ `-race`.
{%- highlight bash -%}
$ go run -race main.go
{%- endhighlight -%}

Для борьбы с состоянием гонци используется `sync.Mutex`.
{%- highlight go -%}
package main

import (
    "fmt"
    "sync"
)

func main() {
    var counters = map[int]int{}
    mu := &sync.Mutex{}
    for i := 0; i < 5; i++ {
        go func(counters map[int]int, th int, mu *{% comment %}*{% endcomment %}sync.Mutex) {
            for j := 0; j < 5; j++ {
                mu.Lock()
                counters[th*10+j]++
                mu.Unlock()
            }
        }(counters, i, mu)
    }
    fmt.Scanln()
    mu.Lock()
    fmt.Println("counters result", counters)
    mu.Unlock()
}
{%- endhighlight -%}

Для синхронизации счетчиков и других простых структур используется пакет `sync/atomic`.
{%- highlight go -%}
package main

import (
    "fmt"
    "sync/atomic"
    "time"
)

var totalOperations int32

func inc() {
    atomic.AddInt32(&totalOperations, 1) // автомарно
}

func main() {
    for i := 0; i < 1000; i++ {
        go inc()
    }
    time.Sleep(2 * time.Millisecond)
    fmt.Println("total operation = ", totalOperations)
}
{%- endhighlight -%}

## Работа с JSON
Процесс запаковки/распаковки JSON в Go называется маршелингом/анмаршелингом. Распаковщик находится в пакете `encoding/json`. Распаковщик работает со слайсом байт `[]byte`.
{%- highlight go -%}
type User struct {
    ID       int
    Username string
    phone    string
}

var jsonStr = `{"id": 42, "username": "zinvapel", "phone": "123"}`

data := []byte(jsonStr)

u := &User{}
json.Unmarshal(data, u)

u.phone = "987654321"
result, err := json.Marshal(u)
if err != nil {
    panic(err)
}
fmt.Printf("json string:\n\t%s\n", string(result))
{%- endhighlight -%}

Чтобы изменить имя поля в JSON, который будет собран из объекта, используются теги.
{%- highlight go -%}
type User struct {
    // json тег, имя поля user_id в json-формате, тип поля в json - строка
    ID       int `json:"user_id,string"`
    Username string
    // json тег, имя поля не меняется в json-формате, omitempty - если значение по-умолчанию, то удалять.
    Address  string `json:",omitempty"`
    // поле не участвует в сериализации
    Comnpany string `json:"-"`
}
{%- endhighlight -%}

Если тип данных не определен, то можно сериализовать/десериализовать данные в `interface{}`.
{%- highlight go -%}
data := []byte(jsonStr)

var user1 interface{}
json.Unmarshal(data, &user1)
{%- endhighlight -%}

## Работа с XML
Для работы с `xml` используются теги и `encoding/xml`.
{%- highlight go -%}
type User struct {
	ID      int    `xml:"id,attr"`
	Login   string `xml:"login"`
	Name    string `xml:"name"`
	Browser string `xml:"browser"`
}
{%- endhighlight -%}

XML можно парсить потоком, что заметно ускоряет процесс:
{%- highlight go -%}
func CountDecoder() {
	input := bytes.NewReader(xmlData)
	decoder := xml.NewDecoder(input)
	logins := make([]string, 0)
	var login string
	for {
		tok, tokenErr := decoder.Token()
		if tokenErr != nil && tokenErr != io.EOF {
			fmt.Println("error happend", tokenErr)
			break
		} else if tokenErr == io.EOF {
			break
		}
		if tok == nil {
			fmt.Println("t is nil break")
		}
		switch tok := tok.(type) {
		case xml.StartElement:
			if tok.Name.Local == "login" {
				if err := decoder.DecodeElement(&login, &tok); err != nil {
					fmt.Println("error happend", err)
				}
				logins = append(logins, login)
			}
		}
	}
}
{%- endhighlight -%}

## Динамические данные
Для работы с динамическими типами можно использовать рефлексию.
{%- highlight go -%}
package main

import (
    "fmt"
    "reflect"
)

type User struct {
    ID       int
    RealName string `unpack:"-"`
    Login    string
    Flags    int
}

func PrintReflect(u interface{}) error {
    val := reflect.ValueOf(u).Elem()

    fmt.Printf("%T have %d fields:\n", u, val.NumField())
    for i := 0; i < val.NumField(); i++ {
        valueField := val.Field(i)
        typeField := val.Type().Field(i)

        fmt.Printf("\tname=%v, type=%v, value=%v, tag=`%v`\n", typeField.Name,
            typeField.Type.Kind(),
            valueField,
            typeField.Tag,
        )
    }
    return nil
}

func main() {
    u := &User{
        ID:       42,
        RealName: "zinvapel",
        Flags:    32,
    }
    err := PrintReflect(u)
    if err != nil {
        panic(err)
    }
}
{%- endhighlight -%}

{%- highlight go -%}
package main

import (
    "bytes"
    "encoding/binary"
    "fmt"
    "reflect"
)

type User struct {
    ID       int
    RealName string `unpack:"-"`
    Login    string
    Flags    int
}

func UnpackReflect(u interface{}, data []byte) error {
    r := bytes.NewReader(data)

    val := reflect.ValueOf(u).Elem()

    for i := 0; i < val.NumField(); i++ {
        valueField := val.Field(i)
        typeField := val.Type().Field(i)

        if typeField.Tag.Get("unpack") == "-" {
            continue
        }

        switch typeField.Type.Kind() {
        case reflect.Int:
            var value uint32
            binary.Read(r, binary.LittleEndian, &value)
            valueField.Set(reflect.ValueOf(int(value)))
        case reflect.String:
            var lenRaw uint32
            binary.Read(r, binary.LittleEndian, &lenRaw)

            dataRaw := make([]byte, lenRaw)
            binary.Read(r, binary.LittleEndian, &dataRaw)

            valueField.SetString(string(dataRaw))
        default:
            return fmt.Errorf("bad type: %v for field %v", typeField.Type.Kind(), typeField.Name)
        }
    }

    return nil
}

func main() {
    /*{% comment %}*{% endcomment %}
        perl -E '$b = pack("L L/a* L", 1_123_456, "zinvapel", 16);
            print map { ord.", "  } split("", $b); '
    *{% comment %}*{% endcomment %}/
    data := []byte{
        128, 36, 17, 0,

        9, 0, 0, 0,
        118, 46, 114, 111, 109, 97, 110, 111, 118,

        16, 0, 0, 0,
    }
    u := new(User)
    err := UnpackReflect(u, data)
    if err != nil {
        panic(err)
    }

    fmt.Printf("%#v", u)
}
{%- endhighlight -%}

Интересно знать, что компилятор Go написан на Go. Поэтому в Go есть встроенная кодогенерация в пакетах `go/ast`, `go/parser`, `go/token`.

## Бенчмарки кода
Бенчмарки используются для измерения скорости кода. Они находятся в пакете `test`. Функции-бенчмарки по аналогии с функциями-тестами начинаются со слова `Benchmark`.
{%- highlight go -%}
// go test -v -bench=. json/*{% comment %}*{% endcomment %}.go
// go test -bench <какую функцию вызвать, dot . - все бенчмарки> <path/to/file.go>
// Флаг -benchmem измеряет память

func BenchmarkDecodeStandart(b *{% comment %}*{% endcomment %}testing.B) {
    // N - некоторое количество, которое придет
	for i := 0; i < b.N; i++ {
		_ = json.Unmarshal(data, &c)
	}
}
{%- endhighlight -%}

Чтобы выяснить что именно выполнялось быстро или медленно, нужно воспользоваться профилированием через `pprof`.
{%- highlight bash -%}
$ go test -bench . -benchmem -cpuprofile=cpu.out -memprofile=mem.out -memprofilerate=1 unpack_test.go
goos: darwin
goarch: amd64
BenchmarkCodegen-8        100000             19063 ns/op             224 B/op         12 allocs/op
BenchmarkReflect-8         30000             43922 ns/op             416 B/op         20 allocs/op
PASS
ok      command-line-arguments  4.049s
{%- endhighlight -%}

- `-cpuprofile` - профилировать CPU в файл.
- `-memprofile` - профилировать память в файл.
- `-memprofilerate` - профилировать память.

В результате получаем файл `main.test`, для которого можно воспользоваться профайлером `go tool pprof`. Части функций нужен [Graphviz](http://www.graphviz.org/download/).
{%- highlight bash -%}
pavel@pmurtazin ~/go/src/coursera/json/perfomance > go tool pprof main.test cpu.out
File: main.test
Type: cpu
Time: Dec 7, 2017 at 8:27pm (MSK)
Duration: 4.63s, Total samples = 4.21s (90.98%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof) top
Showing nodes accounting for 3520ms, 83.61% of 4210ms total
Dropped 33 nodes (cum <= 21.05ms)
Showing top 10 nodes out of 70
      flat  flat%   sum%        cum   cum%
    1370ms 32.54% 32.54%     1390ms 33.02%  runtime.addspecial /usr/local/opt/go/libexec/src/runtime/mheap.go
     500ms 11.88% 44.42%      660ms 15.68%  runtime.step /usr/local/opt/go/libexec/src/runtime/symtab.go
     440ms 10.45% 54.87%     1100ms 26.13%  runtime.pcvalue /usr/local/opt/go/libexec/src/runtime/symtab.go
     380ms  9.03% 63.90%      380ms  9.03%  runtime.usleep /usr/local/opt/go/libexec/src/runtime/sys_darwin_amd64.s
     280ms  6.65% 70.55%     1610ms 38.24%  runtime.gentraceback /usr/local/opt/go/libexec/src/runtime/traceback.go
     160ms  3.80% 74.35%      160ms  3.80%  runtime.readvarint /usr/local/opt/go/libexec/src/runtime/symtab.go
     140ms  3.33% 77.67%      140ms  3.33%  runtime.findfunc /usr/local/opt/go/libexec/src/runtime/symtab.go
     120ms  2.85% 80.52%     1600ms 38.00%  runtime.setprofilebucket /usr/local/opt/go/libexec/src/runtime/mheap.go
      80ms  1.90% 82.42%       80ms  1.90%  runtime.pcvalue /usr/local/opt/go/libexec/src/runtime/stubs.go
      50ms  1.19% 83.61%       50ms  1.19%  runtime.markrootSpans /usr/local/opt/go/libexec/src/runtime/mgcmark.go
(pprof) list Unpack
Total: 4.21s
{%- endhighlight -%}

## Ускорение кода
Повторяемые данные можно создавать с помощью `sync.Pool`.
{%- highlight go -%}
var dataPool = sync.Pool{
	New: func() interface{} {
		return bytes.NewBuffer(make([]byte, 0, 64))
	},
}

func BenchmarkAllocPool(b *{% comment %}*{% endcomment %}testing.B) {
	b.RunParallel(func(pb *{% comment %}*{% endcomment %}testing.PB) {
		for pb.Next() {
			data := dataPool.Get().(*{% comment %}*{% endcomment %}bytes.Buffer)
			_ = json.NewEncoder(data).Encode(Pages)
			data.Reset()
			dataPool.Put(data)
		}
	})
}
{%- endhighlight -%}

## Пакет net
Пакет позволяет слушать tcp-сокет.
{%- highlight go -%}
package main

import (
	"bufio"
	"fmt"
	"net"
)

func handleConnection(conn net.Conn) {
	name := conn.RemoteAddr().String()

	fmt.Printf("%+v connected\n", name)
	conn.Write([]byte("Hello, " + name + "\n\r"))

	defer conn.Close()

	scanner := bufio.NewScanner(conn)
	for scanner.Scan() {
		text := scanner.Text()
		if text == "Exit" {
			conn.Write([]byte("Bye\n\r"))
			fmt.Println(name, "disconnected")
			break
		} else if text != "" {
			fmt.Println(name, "enters", text)
			conn.Write([]byte("You enter " + text + "\n\r"))
		}
	}
}

func main() {
	listner, err := net.Listen("tcp", ":8080")
	if err != nil {
		panic(err)
	}
	for {
		conn, err := listner.Accept()
		if err != nil {
			panic(err)
		}
		go handleConnection(conn)
	}
}
{%- endhighlight -%}

Использование пакета `net`:
- `net.http` - https://golang.org/pkg/net/http/ используется для обработки и отправки HTTP.
{%- highlight go -%}
package main

import (
	"fmt"
	"net/http"
	"time"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "URL:", r.URL.String())
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", handler)

	server := http.Server{
		Addr:         ":8080",
		Handler:      mux,
		ReadTimeout:  10 * time.Second,
		WriteTimeout: 10 * time.Second,
	}

	fmt.Println("starting server at :8080")
	server.ListenAndServe()
}
{%- endhighlight -%}

{%- highlight go -%}
package main

import (
	"crypto/md5"
	"encoding/json"
	"fmt"
	"io"
	"io/ioutil"
	"net/http"
)

var uploadFormTmpl = []byte(`
<html>
	<body>
	<form action="/upload" method="post" enctype="multipart/form-data">
		Image: <input type="file" name="my_file">
		<input type="submit" value="Upload">
	</form>
	</body>
</html>
`)

func mainPage(w http.ResponseWriter, r *http.Request) {
	w.Write(uploadFormTmpl)
}

func uploadPage(w http.ResponseWriter, r *http.Request) {
	r.ParseMultipartForm(5 * 1024 * 1025)
	file, handler, err := r.FormFile("my_file")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer file.Close()

	fmt.Fprintf(w, "handler.Filename %v\n", handler.Filename)
	fmt.Fprintf(w, "handler.Header %#v\n", handler.Header)

	hasher := md5.New()
	io.Copy(hasher, file)

	fmt.Fprintf(w, "md5 %x\n", hasher.Sum(nil))
}

type Params struct {
	ID   int
	User string
}

/*
curl -v -X POST -H "Content-Type: application/json" -d '{"id": 2, "user": "rvasily"}' http://localhost:8080/raw_body
*/

func uploadRawBody(w http.ResponseWriter, r *http.Request) {

	body, err := ioutil.ReadAll(r.Body)
	defer r.Body.Close()

	p := &Params{}
	err = json.Unmarshal(body, p)
	if err != nil {
		http.Error(w, err.Error(), 500)
		return
	}

	fmt.Fprintf(w, "content-type %#v\n",
		r.Header.Get("Content-Type"))
	fmt.Fprintf(w, "params %#v\n", p)
}

func main() {
	http.HandleFunc("/", mainPage)
	http.HandleFunc("/upload", uploadPage)
	http.HandleFunc("/raw_body", uploadRawBody)

	fmt.Println("starting server at :8080")
	http.ListenAndServe(":8080", nil)
}{% comment %}*{% endcomment %}
{%- endhighlight -%}

Шаблонизаторы в Go также встроены:
- `text/template` - простой шаблонизатор
- `html/template` - шаблонизатор с возможностью вызова методов и функций

Профилирование возможно через `net/http/pprof`.

Тестирование возможно с помощью пакета `httptest`.
