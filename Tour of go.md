#  <span style="color:#00FFFF">Пакеты, переменные и функции </span>


## Пакеты

Каждая программа на Go состоит из пакетов.

Программы начинают выполняться в пакете main.

Эта программа использует пакеты с путями импорта "fmt" и "math/rand".

По соглашению, имя пакета такое же, как и последний элемент пути импорта. Например, пакет "math/rand" включает файлы, которые начинаются с оператора package rand.

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```

## Импорт

Этот код группирует импортируемые пакеты в заключённое в скобки "факторизованное" выражение импорта.

Также можно писать несколько операторов импорта, например:

```go
import "fmt"
import "math"
```

Но хорошим стилем считается использование факторизованного выражения импорта.

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Printf("Now you have %g problems.\n", math.Sqrt(7))
}
```

## Экспортируемые имена

В Go имя считается экспортируемым, если оно начинается с заглавной буквы. Например, Pizza — это экспортируемое имя, как и Pi, которое экспортируется из пакета math.

Имена pizza и pi не начинаются с заглавной буквы, поэтому они не экспортируются.

При импорте пакета можно ссылаться только на его экспортируемые имена. Любые "неэкспортируемые" имена недоступны извне пакета.

Запустите код. Обратите внимание на сообщение об ошибке.

Чтобы исправить ошибку, переименуйте math.pi в math.Pi и попробуйте снова.


```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.Pi)
}
```

## Функция может принимать ноль или более аргументов

В этом примере функция add принимает два параметра типа int.

Обратите внимание, что тип указывается после имени переменной.

(Для получения дополнительной информации о том, почему типы выглядят именно так, см. статью о синтаксисе объявлений в Go.)

```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

## Продолжение о функциях

Когда два или более последовательных именованных параметра функции имеют один и тот же тип, вы можете опустить тип для всех, кроме последнего.

В этом примере мы сократили

```go
x int, y int
```

до

```go
x, y int
```


```go
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}

```

## Несколько результатов

Функция может возвращать любое количество результатов.

Функция swap возвращает две строки.

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

## Именованные возвращаемые значения

В Go возвращаемые значения могут иметь имена. Если они заданы, они рассматриваются как переменные, определённые в начале функции.

Эти имена следует использовать для документирования значения возвращаемых результатов.

Оператор return без аргументов возвращает именованные возвращаемые значения. Это известно как "голый" return.

Голые return-операторы следует использовать только в коротких функциях, как в приведённом здесь примере. В более длинных функциях они могут ухудшить читаемость кода.

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

## Переменные

Оператор var объявляет список переменных; как и в списке аргументов функции, тип указывается в конце.

Оператор var может находиться на уровне пакета или функции. В этом примере мы видим оба случая.

```go
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

## Переменные с инициализаторами

Оператор var может включать инициализаторы, по одному на каждую переменную.

Если инициализатор присутствует, тип переменной можно опустить; переменная будет иметь тип инициализатора.

```go
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

## Краткое объявление переменных

Внутри функции можно использовать оператор := для краткого присваивания вместо оператора var с неявным типом.

Вне функции каждое выражение начинается с ключевого слова (var, func и т. д.), поэтому конструкция := недоступна.

```go
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Базовые типы

Базовые типы в Go:

- bool
- string
- int, int8, int16, int32, int64
- uint, uint8, uint16, uint32, uint64, uintptr
- byte (псевдоним для uint8)
- rune (псевдоним для int32, представляет кодовую точку Unicode)
- float32, float64
- complex64, complex128

В примере показаны переменные нескольких типов, а также то, что объявления переменных могут быть "факторизированы" в блоки, как и операторы импорта.

Типы int, uint и uintptr обычно имеют ширину 32 бита на 32-битных системах и 64 бита на 64-битных системах. Если вам нужно целочисленное значение, вы должны использовать тип int, если у вас нет конкретной причины использовать тип целого числа определённого размера или беззнаковый тип.

```go
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
	fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
	fmt.Printf("Type: %T Value: %v\n", z, z)
}
```


## Нулевые значения

Переменные, объявленные без явного начального значения, получают своё нулевое значение.

Нулевые значения следующие:

- 0 для числовых типов,
- false для типа boolean,
- "" (пустая строка) для строк.

```go
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s)
}
```

## Преобразование типов

Выражение `T(v)` преобразует значение `v` к типу `T`.

Некоторые числовые преобразования:

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

Или более просто:

```go
i := 42
f := float64(i)
u := uint(f)
```

В отличие от C, в Go присваивание между элементами разных типов требует явного преобразования. Попробуйте удалить преобразования `float64` или `uint` в приведённом примере и посмотрите, что произойдёт.

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	fmt.Println(f)
	var z uint = uint(f)
	fmt.Println(x, y, z)
}
```

## Вывод типа

При объявлении переменной без указания явного типа (с помощью синтаксиса `:=` или оператора `var =`), тип переменной выводится из значения справа.

Когда справа от объявления указан типизированный источник данных, новая переменная будет того же типа:

```go
var i int
j := i // j является int
```

Но если справа от объявления находится не типизированная числовая константа, новая переменная может быть типа `int`, `float64` или `complex128`, в зависимости от точности константы:

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

Попробуйте изменить начальное значение `v` в примере кода и наблюдайте, как это влияет на его тип.

```go
package main

import "fmt"

func main() {
	v := 42.2 // change me!
	fmt.Printf("v is of type %T\n", v)
}
```

## Константы

Константы объявляются так же, как переменные, но с использованием ключевого слова `const`.

Константы могут быть символьными, строковыми, логическими или числовыми значениями.

Константы нельзя объявлять с помощью синтаксиса `:=`.

```go 
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

## Числовые константы

Числовые константы представляют собой значения высокой точности.

Не типизированная константа принимает тип, необходимый для своего контекста.

Попробуйте также вывести needInt(Big).

(Тип int может хранить максимум 64-битное целое число, иногда меньше.)

```go
package main

import "fmt"

const (
	// Create a huge number by shifting a 1 bit left 100 places.
	// In other words, the binary number that is 1 followed by 100 zeroes.
	Big = 1 << 100
	// Shift it right again 99 places, so we end up with 1<<1, or 2.
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}
```


# <span style="color:#00FFFF">Управляющие конструкции: циклы (for), условия (if, else), оператор выбора (switch) и отложенные вызовы (defer).</span>

## Цикл for

В Go существует только одна конструкция цикла — цикл for.

Базовый цикл for состоит из трёх компонентов, разделённых точкой с запятой:

- оператор инициализации (init statement): выполняется перед первой итерацией,
- выражение условия (condition expression): вычисляется перед каждой итерацией,
- оператор постобработки (post statement): выполняется в конце каждой итерации.

Оператор инициализации часто является кратким объявлением переменных, и переменные, объявленные здесь, видимы только в пределах цикла for.

Цикл прекратит выполнение, как только булево условие станет ложным.

Примечание: В отличие от других языков, таких как C, Java или JavaScript, вокруг трёх компонентов оператора for нет скобок, и фигурные скобки { } всегда обязательны.

```go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

## Продолжение о цикле for

Операторы init и post являются необязательными в цикле for.

```go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}

```

## Цикл for как "while" в Go

На этом этапе вы можете опустить точки с запятой: в Go цикл "while" из C записывается как цикл for.

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

## Бесконечный цикл

Если вы опустите условие цикла, он будет выполняться бесконечно, так что бесконечный цикл компактно выражается следующим образом:

```go
package main

func main() {
	for {
	}
}
```

## Условный оператор if

Условные операторы if в Go, подобно его циклам for, не требуют обрамляющих скобок ( ), однако фигурные скобки { } обязательны.

```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}

```

## Условный оператор if с кратким оператором

Как и цикл for, оператор if может начинаться с краткого оператора, который выполняется перед проверкой условия.

Переменные, объявленные этим оператором, видны только до конца блока if.

(Попробуйте использовать переменную v в последнем операторе return.)

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## Условный оператор if и else

Переменные, объявленные внутри краткого оператора if, также доступны в любом из блоков else.

(Оба вызова функции pow возвращают свои результаты до того, как вызов fmt.Println начнётся в функции main.)

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## Упражнение: Циклы и функции

Давайте реализуем функцию вычисления квадратного корня как способ поиграть с функциями и циклами: для заданного числа x мы хотим найти число z, для которого z² наиболее близко к x.

Обычно компьютеры вычисляют квадратный корень из x с использованием цикла. Начиная с какого-то предположения z, мы можем корректировать z в зависимости от того, насколько близко z² к x, получая лучшее предположение:

z -= (z*z - x) / (2*z)

Повторяя эту корректировку, предположение становится всё лучше и лучше, пока мы не достигнем ответа, который наиболее близок к фактическому квадратному корню.

Реализуйте это в предоставленной функции Sqrt. Хорошее начальное предположение для z - это 1, не важно какое входное значение. Сначала повторите вычисление 10 раз и распечатайте каждое значение z по пути. Посмотрите, насколько близко вы приблизились к ответу для различных значений x (1, 2, 3, ...) и насколько быстро улучшается предположение.

Подсказка: Чтобы объявить и инициализировать значение с плавающей точкой, используйте синтаксис с плавающей точкой или конверсию:

z := 1.0
z := float64(1)

Затем измените условие цикла, чтобы оно останавливалось, когда значение перестанет изменяться (или изменяется только на очень малую величину). Посмотрите, будет ли их больше или меньше 10 итераций. Попробуйте другие начальные предположения для z, например, x или x/2. Насколько близки результаты вашей функции к math.Sqrt из стандартной библиотеки?

(Примечание: Если вас интересуют детали алгоритма, то z² - x выше показывает, насколько далеко z² находится от нужного значения (x), и деление на 2z - это производная от z², чтобы масштабировать, насколько мы корректируем z в зависимости от того, как быстро меняется z². Этот общий подход называется методом Ньютона. Он хорошо работает для многих функций, особенно для квадратного корня.)

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
}

func main() {
	fmt.Println(Sqrt(2))
}
```

## Оператор switch

Оператор switch в Go представляет собой более короткий способ написания последовательности операторов if-else. Он выполняет первый case, значение которого равно выражению условия.

Switch в Go подобен тому, который используется в C, C++, Java, JavaScript и PHP, за исключением того, что в Go выполняется только выбранный case, а не все последующие case. Фактически, оператор break, который необходим в конце каждого case в этих языках, в Go предоставляется автоматически. Еще одно важное отличие заключается в том, что case в switch в Go не обязательно должны быть константами, и значения могут быть не только целыми числами.

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

## Порядок вычисления в switch

В операторе switch case в Go выражения case вычисляются сверху вниз, останавливаясь, когда один из case выполняется.

(Например,

```go
switch i {
case 0:
case f():
}
```

функция f() не вызывается, если i==0.)

Примечание: Время в Go playground всегда начинается с 2009-11-10 23:00:00 UTC, значение которого оставлено читателю в качестве упражнения.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}
}
```

## Switch без условия

Switch без условия эквивалентен `switch true`.

Этот конструкт может быть чистым способом написания длинных цепочек if-then-else.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}

```

## Отложенные вызовы (defer)

Отложенный вызов (defer) откладывает выполнение функции до тех пор, пока окружающая функция не завершится.

Аргументы отложенного вызова вычисляются немедленно, но сам вызов функции выполняется только после возврата из окружающей функции.

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

## Стекирование отложенных вызовов

Отложенные вызовы функций помещаются в стек. Когда функция завершает свое выполнение и возвращает управление, ее отложенные вызовы выполняются в порядке последнего вошедшего - первым вышедшим (LIFO).

Чтобы узнать больше о операторах defer, прочитайте этот блог-пост.

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

# <span style="color:#00FFFF">Дополнительные типы: структуры, срезы и карты</span>

## Указатели

В Go есть указатели. Указатель хранит адрес памяти значения.

Тип `*T` является указателем на значение типа `T`. Его нулевое значение — `nil`.

```go
var p *int
```

Оператор `&` генерирует указатель на свой операнд.

```go
i := 42  
p = &i
```

Оператор `*` обозначает значение, на которое указывает указатель.

```go
fmt.Println(*p) // читаем значение i через указатель p  
*p = 21         // устанавливаем значение i через указатель p
```

Это известно как "разыменование" или "непрямое обращение".

В отличие от C, в Go нет арифметики указателей.

```go
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}
```

## Структуры

Структура (struct) — это коллекция полей.

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})
}

```

## Поля структур

К полям структур обращаются с использованием точки.

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## Указатели на структуры

К полям структур можно обращаться через указатель на структуру.

Чтобы получить доступ к полю `X` структуры, имея указатель на структуру `p`, мы могли бы написать `(*p).X`. Однако эта запись громоздка, поэтому язык позволяет нам вместо этого просто писать `p.X`, без явного разыменования.

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

## Литералы структур

Литерал структуры обозначает вновь выделенное значение структуры, перечисляя значения её полей.

Вы можете перечислить только подмножество полей, используя синтаксис `Name:`. (И порядок именованных полей не имеет значения.)

Специальный префикс `&` возвращает указатель на значение структуры.

```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

## Массивы

Тип `[n]T` является массивом из `n` значений типа `T`.

Выражение

```go
var a [10]int
```

объявляет переменную `a` как массив из десяти целых чисел.

Длина массива является частью его типа, поэтому массивы не могут изменять свой размер. Это может показаться ограничением, но не волнуйтесь; Go предоставляет удобный способ работы с массивами.


```go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```

## Срезы

Массив имеет фиксированный размер. Срез, с другой стороны, представляет собой динамически изменяемый, гибкий вид элементов массива. На практике срезы гораздо более распространены, чем массивы.

Тип `[]T` является срезом с элементами типа `T`.

Срез формируется путем указания двух индексов, нижней и верхней границ, разделенных двоеточием:

```go
a[low : high]
```

Это выбирает полуоткрытый диапазон, который включает первый элемент, но исключает последний.

Следующее выражение создает срез, который включает элементы с 1 по 3 массива `a`:

```go
a[1:4]
```

```go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}

```

## Срезы как ссылки на массивы

Срез не хранит сами данные, он просто описывает секцию базового массива.

Изменение элементов среза изменяет соответствующие элементы его базового массива.

Другие срезы, которые совместно используют тот же базовый массив, также увидят эти изменения.

```go
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```

## Литералы срезов

Литерал среза похож на литерал массива, но без указания длины.

Вот литерал массива:

```go
[3]bool{true, true, false}
```

И этот код создает тот же самый массив, а затем формирует срез, который на него ссылается:

```go
[]bool{true, true, false}
```

```go
package main

import "fmt"

func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
```

## По умолчанию при работе со срезами

При выполнении срезов вы можете опустить нижнюю или верхнюю границу, чтобы вместо них использовались их значения по умолчанию.

По умолчанию нижняя граница равна нулю, а верхняя граница равна длине среза.

Для массива

```go
var a [10]int
```

следующие выражения срезов эквивалентны:

```go
a[0:10]
a[:10]
a[0:]
a[:]
```

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}
```

## Длина и вместимость среза

У среза есть как длина, так и вместимость.

Длина среза — это количество элементов, которые он содержит.

Вместимость среза — это количество элементов в базовом массиве, начиная с первого элемента среза.

Длину и вместимость среза `s` можно получить с помощью выражений `len(s)` и `cap(s)` соответственно.

Вы можете увеличить длину среза путем повторного срезания, предоставив ему достаточную вместимость. Попробуйте изменить одну из операций среза в примере программы, чтобы увеличить его длину за пределы его вместимости, и посмотрите, что произойдет.

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## Нулевые срезы

Нулевым значением для среза является `nil`.

У нулевого среза длина и вместимость равны 0, и у него нет базового массива.

```go
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}
```

## Создание среза с помощью функции make

Срезы можно создавать с помощью встроенной функции `make`; это используется для создания массивов переменного размера.

Функция make выделяет массив, заполненный нулями, и возвращает срез, который ссылается на этот массив:

```go
a := make([]int, 5)  // len(a)=5
```

Чтобы указать вместимость, передайте третий аргумент функции make:

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

```go
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

```go
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

## Срезы срезов

Срезы могут содержать любой тип данных, включая другие срезы.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```

## Добавление элементов в срез

Часто бывает необходимо добавлять новые элементы в срез, поэтому в Go предоставляется встроенная функция `append`. Документация встроенного пакета описывает append.

```go
func append(s []T, vs ...T) []T
```

Первый параметр `s` функции `append` — это срез типа `T`, а остальные параметры (`vs`) — значения типа `T`, которые нужно добавить в срез.

Результатом вызова `append` будет срез, содержащий все элементы из исходного среза плюс предоставленные значения.

Если массив, лежащий в основе среза `s`, слишком мал для вмещения всех предоставленных значений, будет выделен больший массив. Возвращаемый срез будет указывать на новый выделенный массив.

(Чтобы узнать больше о срезах, можно прочитать статью "Slices: usage and internals".)

```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s)

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## Цикл с использованием `range`

Форма `range` цикла `for` итерирует по срезу или отображению (map).

При итерации по срезу с помощью `range` возвращаются два значения на каждой итерации. Первое значение — это индекс элемента, а второе — копия самого элемента по этому индексу.

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

## Продолжение по `range`

Вы можете пропустить индекс или значение, присвоив `_`.

```go
for i, _ := range pow
```

или

```go
for _, value := range pow
```

Если вам нужен только индекс, вы можете опустить вторую переменную.

```go
for i := range pow
```

```go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```

## Упражнение: Срезы

Реализуйте функцию Pic. Она должна возвращать срез длины `dy`, каждый элемент которого является срезом из `dx` восьмибитных беззнаковых целых чисел. При запуске программы она будет отображать ваше изображение, интерпретируя целые числа как значения оттенков серого (голубоватого).

Выбор изображения остается за вами. Интересные функции включают `(x+y)/2`, `x*y` и `x^y`.

(Для выделения каждого `[]uint8` внутри `[][]uint8` вам потребуется использовать цикл.)

(Используйте `uint8(intValue)` для преобразования типов.)

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
}

func main() {
	pic.Show(Pic)
}
```

## Maps

Хэш-таблица (map) отображает ключи на значения.

Нулевым значением для map является `nil`. `nil` map не содержит ключей, и к нему нельзя добавить ключи.

Функция `make` возвращает map заданного типа, инициализированный и готовый к использованию.

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```

## Литералы карт (Map literals)

Литералы карт (Map literals) похожи на литералы структур, но требуют наличия ключей.

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

func main() {
	fmt.Println(m)
}
```

## Продолжение по литералам карт (Map literals)

Если верхний уровень типа представляет собой только имя типа, вы можете опустить его из элементов литерала.

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}
```

## Изменение карт (Mutating Maps)

Вставка или обновление элемента в карту `m`:

```go
m[key] = elem
```

Получение элемента:

```go
elem = m[key]
```

Удаление элемента:

```go
delete(m, key)
```

Проверка наличия ключа с двухзначным присваиванием:

```go
elem, ok = m[key]
```

Если ключ присутствует в `m`, `ok` равно `true`. В противном случае `ok` равно `false`.

Если ключ отсутствует в карте, то `elem` будет нулевым значением для типа элемента карты.

Примечание: Если `elem` или `ok` еще не были объявлены, можно использовать краткую форму объявления:

```go
elem, ok := m[key]
```

```go
package main

import "fmt"

func main() {
	m := make(map[string]int)

	m["Answer"] = 42
	fmt.Println("The value:", m["Answer"])

	m["Answer"] = 48
	fmt.Println("The value:", m["Answer"])

	delete(m, "Answer")
	fmt.Println("The value:", m["Answer"])

	v, ok := m["Answer"]
	fmt.Println("The value:", v, "Present?", ok)
}
```

## Упражнение: Карты (Maps)

Реализуйте функцию WordCount. Она должна возвращать карту с количеством каждого "слова" в строке `s`. Функция wc.Test запускает тестовый набор для предоставленной функции и выводит успех или неудачу.

Вам может помочь функция strings.Fields.

```go
package main

import (
	"golang.org/x/tour/wc"
)

func WordCount(s string) map[string]int {
	return map[string]int{"x": 1}
}

func main() {
	wc.Test(WordCount)
}

```

## Значения функций

Функции также являются значениями. Их можно передавать как и другие значения.

Значения функций могут использоваться в качестве аргументов и возвращаемых значений функций.

```go
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```

## Замыкания функций

Функции в Go могут быть замыканиями (closures). Замыкание - это функция, значение которой ссылается на переменные из внешней области видимости. Функция может получать доступ к этим переменным и изменять их; в этом смысле функция "связана" с этими переменными.

Например, функция `adder` возвращает замыкание. Каждое замыкание привязано к своей собственной переменной `sum`.

```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

## Упражнение: Замыкание Фибоначчи

Давайте повеселимся с функциями.

Реализуйте функцию fibonacci, которая возвращает функцию (замыкание), возвращающую последовательные числа Фибоначчи (0, 1, 1, 2, 3, 5, ...).

```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

# <span style="color:#00FFFF">Методы и интерфейсы</span>

## Методы

В Go нет классов. Однако вы можете определять методы для типов.

Метод — это функция с особым аргументом получателя.

Получатель появляется в собственном списке аргументов между ключевым словом func и именем метода.

В этом примере метод Abs имеет получателя типа Vertex с именем v.

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

## Методы — это функции

Помните: метод — это просто функция с аргументом получателя.

Вот Abs, написанный как обычная функция без изменения функциональности.

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}
```

## Методы, продолжение

Вы также можете объявлять методы для типов, отличных от структур.

В этом примере мы видим числовой тип MyFloat с методом Abs.

Вы можете объявить метод только с получателем, тип которого определен в том же пакете, что и метод. Вы не можете объявить метод с получателем, тип которого определен в другом пакете (что включает встроенные типы, такие как int).

```go
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```

## Получатели-указатели

Вы можете объявлять методы с получателями-указателями.

Это означает, что тип получателя имеет литеральный синтаксис *T для некоторого типа T. (Также T не может сам быть указателем, таким как *int.)

Например, метод Scale здесь определен для *Vertex.

Методы с получателями-указателями могут изменять значение, на которое указывает получатель (как это делает Scale здесь). Поскольку методы часто должны изменять свои получатели, получатели-указатели встречаются чаще, чем получатели-значения.

Попробуйте убрать * из объявления функции Scale на строке 16 и посмотрите, как изменится поведение программы.

С получателем-значением метод Scale работает с копией исходного значения Vertex. (Это то же самое поведение, как и для любого другого аргумента функции.) Метод Scale должен иметь получателя-указателя, чтобы изменить значение Vertex, объявленное в функции main.

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs())
}
```

## Указатели и функции

Здесь мы видим методы Abs и Scale, переписанные как функции.

Снова попробуйте убрать * из строки 16. Можете ли вы понять, почему изменяется поведение? Что еще нужно было изменить, чтобы пример скомпилировался?

(Если вы не уверены, продолжайте на следующую страницу.)

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	Scale(&v, 10)
	fmt.Println(Abs(v))
}
```

## Методы и косвенная адресация указателей

Сравнивая предыдущие две программы, вы можете заметить, что функции с аргументом-указателем должны принимать указатель:

```go
var v Vertex
ScaleFunc(v, 5)  // Ошибка компиляции!
ScaleFunc(&v, 5) // OK
```

в то время как методы с получателями-указателями могут принимать либо значение, либо указатель в качестве получателя при вызове:

```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

Для выражения `v.Scale(5)`, даже если `v` является значением, а не указателем, метод с получателем-указателем вызывается автоматически. То есть, для удобства, Go интерпретирует выражение `v.Scale(5)` как `(&v).Scale(5)`, поскольку метод Scale имеет получателя-указателя.

```go
package main

import "fmt"

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(2)
	ScaleFunc(&v, 10)

	p := &Vertex{4, 3}
	p.Scale(3)
	ScaleFunc(p, 8)

	fmt.Println(v, p)
}
```

## Методы и косвенная адресация указателей (2)

Аналогичное происходит и в обратном направлении.

Функции, которые принимают аргумент-значение, должны принимать значение этого конкретного типа:

```go
var v Vertex
fmt.Println(AbsFunc(v))  // OK
fmt.Println(AbsFunc(&v)) // Ошибка компиляции!
```

в то время как методы с получателями-значениями могут принимать либо значение, либо указатель в качестве получателя при вызове:

```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```

В этом случае вызов метода `p.Abs()` интерпретируется как `(*p).Abs()`.

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func AbsFunc(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
	fmt.Println(AbsFunc(v))

	p := &Vertex{4, 3}
	fmt.Println(p.Abs())
	fmt.Println(AbsFunc(*p))
}

```

## Выбор между получателем-значением и получателем-указателем

Есть две причины использовать получатель-указатель.

Первая причина - чтобы метод мог изменять значение, на которое указывает его получатель.

Вторая причина - избежать копирования значения при каждом вызове метода. Это может быть более эффективно, если получатель является большой структурой, например.

В этом примере методы Scale и Abs имеют оба получателя типа *Vertex, даже если метод Abs не изменяет свой получатель.

В общем случае все методы для данного типа должны иметь либо получатели-значения, либо получатели-указатели, но не смешивать их. (Мы увидим причины этого на следующих страницах.)

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	v.Scale(5)
	fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}

```

## Интерфейсы

Тип интерфейса определяется как набор сигнатур методов.

Значение типа интерфейса может содержать любое значение, которое реализует эти методы.

Примечание: В примере кода на строке 22 ошибка. Тип Vertex (значение) не реализует интерфейс Abser, потому что метод Abs определен только для типа *Vertex (указатель).

```go
package main

import (
	"fmt"
	"math"
)

type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex)
	// and does NOT implement Abser.
	a = v

	fmt.Println(a.Abs())
}
```

## Интерфейсы реализуются неявно

Тип реализует интерфейс путем реализации его методов. Нет явного объявления намерений, нет ключевого слова "implements".

Неявные интерфейсы отделяют определение интерфейса от его реализации, что позволяет реализации интерфейса появляться в любом пакете без предварительных договоренностей.

```go
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}

```

## Значения интерфейсов

Под капотом значения интерфейса можно представить как кортеж из значения и конкретного типа:

`(значение, тип)`

Значение интерфейса содержит значение определенного подходящего конкретного типа.

Вызов метода на значении интерфейса выполняет метод с тем же именем на его подходящем типе.

```go
package main

import (
	"fmt"
	"math"
)

type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

type F float64

func (f F) M() {
	fmt.Println(f)
}

func main() {
	var i I

	i = &T{"Hello"}
	describe(i)
	i.M()

	i = F(math.Pi)
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}

```

## Значения интерфейсов с нулевыми внутренними значениями

Если конкретное значение внутри самого интерфейса равно nil, метод будет вызван с nil-получателем.

В некоторых языках это могло бы вызвать исключение нулевого указателя, но в Go обычно пишут методы, которые грациозно обрабатывают вызовы с nil-получателями (как метод M в этом примере).

Обратите внимание, что интерфейсное значение, которое содержит nil внутреннего значения, само по себе не является nil.

```go
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i I

	var t *T
	i = t
	describe(i)
	i.M()

	i = &T{"hello"}
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}

```

## Nil интерфейсные значения

Nil интерфейсное значение не содержит ни значения, ни конкретного типа.

Вызов метода на nil интерфейсе является ошибкой времени выполнения, потому что внутри кортежа интерфейса нет типа, который указывал бы, какой конкретный метод вызвать.

```go
package main

import "fmt"

type I interface {
	M()
}

func main() {
	var i I
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## Пустой интерфейс

Тип интерфейса, который не определяет ни одного метода, известен как пустой интерфейс:

```go
interface{}
```

Пустой интерфейс может содержать значения любого типа. (Каждый тип реализует как минимум нулевое количество методов.)

Пустые интерфейсы используются в коде, который обрабатывает значения неизвестного типа. Например, функция fmt.Print принимает любое количество аргументов типа interface{}.

```go
package main

import "fmt"

func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## Утверждения типа

Утверждение типа предоставляет доступ к конкретному значению, хранящемуся внутри интерфейсного значения.

```go
t := i.(T)
```

Этот оператор утверждает, что интерфейсное значение i содержит конкретный тип T, и присваивает значение T, хранящееся внутри, переменной t.

Если i не содержит тип T, оператор вызовет ошибку выполнения (panic).

Чтобы проверить, содержит ли интерфейсное значение конкретный тип, утверждение типа может возвращать два значения: внутреннее значение и булево значение, указывающее, успешно ли произошло утверждение.

```go
t, ok := i.(T)
```

Если i содержит тип T, то t будет внутренним значением, а ok будет true.

Если нет, ok будет false, t будет нулевым значением типа T, и ошибки выполнения не произойдет.

Обратите внимание на сходство этого синтаксиса с синтаксисом доступа к значениям в картам.

```go
package main

import "fmt"

func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}
```

## Переключение типов (Type switches)

Переключение типов (type switch) — это конструкция, которая позволяет выполнять несколько утверждений типа последовательно.

Переключение типов похоже на обычное выражение switch, но в случае переключения типов кейсы определяют типы (а не значения), и эти значения сравниваются с типом значения, хранящегося в интерфейсном значении.

```go
switch v := i.(type) {
case T:
    // здесь v имеет тип T
case S:
    // здесь v имеет тип S
default:
    // нет совпадений; здесь v имеет тот же тип, что и i
}
```

Объявление в переключении типов имеет ту же синтаксическую конструкцию, что и утверждение типа `i.(T)`, но конкретный тип T заменяется ключевым словом `type`.

Это выражение switch проверяет, содержит ли интерфейсное значение i значение типа T или S. В каждом из случаев T и S переменная v будет иметь тип T или S соответственно и содержать значение, хранящееся в i. В случае default (когда нет совпадений), переменная v имеет тот же интерфейсный тип и значение, что и i.

```go
package main

import "fmt"

func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}
```

## Stringers

Одним из наиболее распространенных интерфейсов является Stringer, определенный в пакете fmt.

```go
type Stringer interface {
    String() string
}
```

Stringer представляет собой тип, который может описать себя в виде строки. Пакет fmt (и многие другие) используют этот интерфейс для печати значений.

```go
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}

```

## Ошибки

Программы на Go выражают состояние ошибки с помощью значений ошибок.

Тип error является встроенным интерфейсом, аналогичным fmt.Stringer:

```go
type error interface {
    Error() string
}
```

(Как и с fmt.Stringer, пакет fmt ищет интерфейс error при печати значений.)

Функции часто возвращают значение ошибки, и вызывающий код должен обрабатывать ошибки, проверяя, равна ли ошибка nil.

```go
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("не удалось преобразовать число: %v\n", err)
    return
}
fmt.Println("Преобразованное целое число:", i)
```

Nil ошибка обозначает успех; ненулевая ошибка обозначает неудачу.

```go
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}

```

## Упражнение: Ошибки

Скопируйте вашу функцию Sqrt из предыдущего упражнения и измените её так, чтобы она возвращала значение ошибки.

Функция Sqrt должна возвращать ненулевое значение ошибки, когда ей передают отрицательное число, так как она не поддерживает комплексные числа.

Создайте новый тип

```go
type ErrNegativeSqrt float64
```

и сделайте его ошибкой, добавив метод

```go
func (e ErrNegativeSqrt) Error() string
```

таким образом, что вызов ErrNegativeSqrt(-2).Error() вернет "cannot Sqrt negative number: -2".

Примечание: Вызов fmt.Sprint(e) внутри метода Error приведет к бесконечному циклу. Вы можете избежать этого, предварительно преобразовав e: fmt.Sprint(float64(e)). Почему?

Измените вашу функцию Sqrt так, чтобы она возвращала значение ErrNegativeSqrt при передаче отрицательного числа.


```go
package main

import (
	"fmt"
)

func Sqrt(x float64) (float64, error) {
	return 0, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}

```

## Читатели (Readers)

Пакет io определяет интерфейс io.Reader, который представляет собой читающий конец потока данных.

Стандартная библиотека Go содержит множество реализаций этого интерфейса, включая файлы, сетевые соединения, компрессоры, шифры и другие.

Интерфейс io.Reader содержит метод Read:

```go
func (T) Read(b []byte) (n int, err error)
```

Метод Read заполняет переданный срез байтов данными и возвращает количество заполненных байтов и значение ошибки. Он возвращает ошибку io.EOF, когда поток заканчивается.

В следующем примере создается strings.Reader и его вывод потребляется по 8 байт за раз.

```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}
```

## Упражнение: Читатели

Реализуйте тип Reader, который генерирует бесконечный поток символов ASCII 'A'.

```go
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

// TODO: Add a Read([]byte) (int, error) method to MyReader.

func main() {
	reader.Validate(MyReader{})
}

```

## Упражнение: rot13Reader

Часто используемым шаблоном является io.Reader, который оборачивает другой io.Reader, модифицируя поток данных в каком-то виде.

Например, функция gzip.NewReader принимает io.Reader (поток сжатых данных) и возвращает *gzip.Reader, который также реализует io.Reader (поток распакованных данных).

Реализуйте rot13Reader, который реализует io.Reader и читает из io.Reader, модифицируя поток путем применения шифра замены ROT13 ко всем алфавитным символам.

Тип rot13Reader предоставлен для вас. Сделайте его io.Reader, реализовав его метод Read.

```go
package main

import (
	"io"
	"os"
	"strings"
)

type rot13Reader struct {
	r io.Reader
}

func main() {
	s := strings.NewReader("Lbh penpxrq gur pbqr!")
	r := rot13Reader{s}
	io.Copy(os.Stdout, &r)
}
```

## Изображения

Пакет image определяет интерфейс Image:

```go
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```

Примечание: возвращаемое значение Rectangle метода Bounds на самом деле является image.Rectangle, так как объявление находится внутри пакета image.

(Для получения дополнительной информации смотрите документацию.)

Типы color.Color и color.Model также являются интерфейсами, но мы будем игнорировать это, используя предопределенные реализации color.RGBA и color.RGBAModel. Эти интерфейсы и типы определены в пакете image/color.

```go
package main

import (
	"fmt"
	"image"
)

func main() {
	m := image.NewRGBA(image.Rect(0, 0, 100, 100))
	fmt.Println(m.Bounds())
	fmt.Println(m.At(0, 0).RGBA())
}

```

## Упражнение: Изображения

Помните генератор изображений, который вы написали ранее? Давайте напишем еще один, но на этот раз он будет возвращать реализацию image.Image вместо среза данных.

Определите свой собственный тип Image, реализуйте необходимые методы и вызовите pic.ShowImage.

Метод Bounds должен возвращать image.Rectangle, например, image.Rect(0, 0, w, h).

Метод ColorModel должен возвращать color.RGBAModel.

Метод At должен возвращать цвет; значение v в предыдущем генераторе изображений соответствует color.RGBA{v, v, 255, 255} в этом примере.

```go
package main

import "golang.org/x/tour/pic"

type Image struct{}

func main() {
	m := Image{}
	pic.ShowImage(m)
}
```

# <span style="color:#00FFFF">Дженерики</span>

## Параметры типа

Функции в Go могут быть написаны так, чтобы работать с несколькими типами с использованием параметров типа. Параметры типа функции указываются в квадратных скобках перед аргументами функции.

```go
func Index[T comparable](s []T, x T) int
```

Это объявление означает, что s является срезом любого типа T, который удовлетворяет встроенному ограничению comparable. x также является значением того же типа T.

comparable - это полезное ограничение, которое позволяет использовать операторы == и != для значений данного типа. В этом примере мы используем его для сравнения значения со всеми элементами среза до нахождения соответствия. Функция Index работает для любого типа, который поддерживает сравнение.

```go
package main

import "fmt"

// Index returns the index of x in s, or -1 if not found.
func Index[T comparable](s []T, x T) int {
	for i, v := range s {
		// v and x are type T, which has the comparable
		// constraint, so we can use == here.
		if v == x {
			return i
		}
	}
	return -1
}

func main() {
	// Index works on a slice of ints
	si := []int{10, 20, 15, -10}
	fmt.Println(Index(si, 15))

	// Index also works on a slice of strings
	ss := []string{"foo", "bar", "baz"}
	fmt.Println(Index(ss, "hello"))
}

```

## Обобщённые типы

В дополнение к обобщённым функциям, Go также поддерживает обобщённые типы. Тип может быть параметризован с помощью типового параметра, что может быть полезно для реализации обобщённых структур данных.

Этот пример демонстрирует простое объявление типа для односвязного списка, способного хранить значения любого типа.

```go
package main

import "fmt"

// Node представляет узел в односвязном списке.
type Node[T any] struct {
    Value T
    Next  *Node[T]
}

// LinkedList представляет односвязный список.
type LinkedList[T any] struct {
    Head *Node[T]
    Size int
}

// Add добавляет новое значение в список.
func (list *LinkedList[T]) Add(value T) {
    newNode := &Node[T]{Value: value, Next: nil}
    if list.Head == nil {
        list.Head = newNode
    } else {
        current := list.Head
        for current.Next != nil {
            current = current.Next
        }
        current.Next = newNode
    }
    list.Size++
}

// Print печатает все элементы в списке.
func (list *LinkedList[T]) Print() {
    current := list.Head
    for current != nil {
        fmt.Printf("%v -> ", current.Value)
        current = current.Next
    }
    fmt.Println("nil")
}

func main() {
    // Пример использования LinkedList с целыми числами
    intList := LinkedList[int]{}
    intList.Add(1)
    intList.Add(2)
    intList.Add(3)
    intList.Print()

    // Пример использования LinkedList со строками
    stringList := LinkedList[string]{}
    stringList.Add("apple")
    stringList.Add("banana")
    stringList.Add("cherry")
    stringList.Print()
}
```

## Обобщённые типы

Помимо обобщённых функций, в Go также поддерживаются обобщённые типы. Тип может быть параметризован типовым параметром, что может быть полезно для реализации обобщённых структур данных.

Этот пример демонстрирует простое объявление типа для односвязного списка, который может содержать значения любого типа.

В качестве упражнения добавьте некоторую функциональность к этой реализации списка.

```go
package main

// List represents a singly-linked list that holds
// values of any type.
type List[T any] struct {
	next *List[T]
	val  T
}

func main() {
}

```

# <span style="color:#00FFFF">Конкурентность</span>

## Горутины

Горутина — это легковесный поток, управляемый Go runtime.

`go f(x, y, z)`

запускает новую горутину, в которой выполняется

`f(x, y, z)`

Вычисление `f`, `x`, `y` и `z` происходит в текущей горутине, а выполнение `f` происходит в новой горутине.

Горутины работают в одном адресном пространстве, поэтому доступ к общей памяти должен быть синхронизирован. Пакет `sync` предоставляет полезные примитивы, хотя в Go вы их не так часто используете, так как есть другие примитивы (см. следующий слайд).

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	go say("world")
	time.Sleep(10 * time.Second)
	//say("hello")
}
```

## Каналы

Каналы представляют собой типизированный канал, через который можно отправлять и получать значения с помощью оператора канала, <-.

```
ch <- v    // Отправить значение v в канал ch.
v := <-ch  // Получить значение из канала ch и присвоить его переменной v.
```

(Данные движутся в направлении стрелки.)

Как и карты и срезы, каналы должны быть созданы перед использованием:

```
ch := make(chan int)
```

По умолчанию отправка и получение значений блокируются, пока другая сторона не будет готова. Это позволяет горутинам синхронизироваться без явных блокировок или условных переменных.

Приведённый ниже пример суммирует числа в срезе, распределяя работу между двумя горутинами. Как только обе горутины завершат свои вычисления, вычисляется окончательный результат.

```go
package main 

import (
	"fmt"
	"math"
)

func main(){
	
}
```

## Буферизованные каналы

Каналы могут быть буферизованными. Укажите длину буфера вторым аргументом при создании канала:

```go
ch := make(chan int, 100)
```

Отправки в буферизованный канал блокируются только тогда, когда буфер заполнен. Приём блокируется, когда буфер пуст.

Измените пример так, чтобы переполнить буфер и посмотреть, что произойдёт.

```go
package main

import "fmt"

func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}

```

## Range и Close

Отправитель может закрыть канал, чтобы указать, что больше значений отправляться не будет. Получатели могут проверить, закрыт ли канал, присвоив второй параметр выражению приёма:

```go
v, ok := <-ch
```

Если больше нет значений для получения и канал закрыт, `ok` будет равно false.

Цикл `for i := range c` будет повторяться, получая значения из канала `c`, пока он не будет закрыт.

Замечание: Только отправитель должен закрывать канал, никогда получатель. Отправка в закрытый канал вызовет панику.

Ещё одно замечание: Каналы не похожи на файлы; обычно их не нужно закрывать. Закрытие необходимо только тогда, когда получатель должен быть уведомлен о том, что больше значений не будет, например, для завершения цикла `range`.

```go
package main

import (
	"fmt"
)

func fibonacci(c int, ch chan int) {
	x, y := 0, 1
	for range c {
		ch <- x
		x, y = y, x+y
	}
	close(ch)
}

func main() {
	c := make(chan int, 100)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```

## Select

Оператор `select` позволяет горутине ожидать выполнения нескольких операций взаимодействия.

`select` блокируется, пока не выполнится один из его случаев, затем он выполняет этот случай. Если несколько случаев готовы к выполнению, `select` выбирает один случайным образом.

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}

```

## Выбор по умолчанию

В операторе `select` случай по умолчанию выполняется, если ни один из других случаев не готов.

Используйте случай по умолчанию для попытки отправки или получения без блокировки:

```go
select {
case i := <-c:
    // использовать i
default:
    // прием из c заблокировался бы
}
```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(100 * time.Millisecond)
	boom := time.After(500 * time.Millisecond)
	for {
		select {
		case <-tick:
			fmt.Println("tick.")
		case <-boom:
			fmt.Println("BOOM!")
			return
		default:
			fmt.Println("    .")
			time.Sleep(50 * time.Millisecond)
		}
	}
}
```

## Упражнение: Эквивалентные бинарные деревья

Может существовать много различных бинарных деревьев с одной и той же последовательностью значений. Например, вот два бинарных дерева, хранящих последовательность 1, 1, 2, 3, 5, 8, 13.

![[Pasted image 20240717205525.png]]

Функция для проверки, хранят ли два бинарных дерева одну и ту же последовательность, довольно сложна на большинстве языков программирования. Мы будем использовать конкурентность и каналы в Go для написания простого решения.

Этот пример использует пакет tree, который определяет тип:

```go
type Tree struct {
    Left  *Tree
    Value int
    Right *Tree
}
```


## Упражнение: Эквивалентные бинарные деревья

1. Реализуйте функцию Walk.
2. Протестируйте

Функция tree.New(k) создает случайно структурированное (но всегда отсортированное) бинарное дерево, содержащее значения k, 2k, 3k, ..., 10k.

Создайте новый канал ch и запустите walker:

```go
go Walk(tree.New(1), ch)
```

Затем считайте и выведите 10 значений из канала. Они должны быть числами 1, 2, 3, ..., 10.

3. Реализуйте функцию Same, используя Walk, чтобы определить, хранят ли t1 и t2 одни и те же значения.

4. Протестируйте функцию Same.

Функция Same(tree.New(1), tree.New(1)) должна возвращать true, а Same(tree.New(1), tree.New(2)) должна возвращать false.

Документация по Tree доступна [здесь](ссылка на документацию).

```go
package main

import "golang.org/x/tour/tree"

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int)

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool

func main() {
}
```

## sync.Mutex

Мы уже видели, как каналы отлично подходят для общения между горутинами.

Но что, если нам не нужно общение? Что, если мы просто хотим убедиться, что только одна горутина может обращаться к переменной в данный момент, чтобы избежать конфликтов?

Эта концепция называется взаимным исключением, и для этого используется стандартная библиотека Go с помощью sync.Mutex и его двух методов:

- Lock
- Unlock

Мы можем определить блок кода для выполнения взаимного исключения, окружив его вызовами Lock и Unlock, как показано в методе Inc.

Также мы можем использовать defer, чтобы гарантировать, что мьютекс будет разблокирован, как в методе Value.

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// SafeCounter is safe to use concurrently.
type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

// Inc increments the counter for the given key.
func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	c.v[key]++
	c.mu.Unlock()
}

// Value returns the current value of the counter for the given key.
func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	defer c.mu.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}

```

## Упражнение: Веб-скрейпер

В этом упражнении вы будете использовать функции параллелизма Go для параллелизации веб-скрейпера.

Измените функцию Crawl так, чтобы она извлекала URL-адреса параллельно, избегая извлечения одного и того же URL-адреса дважды.

Подсказка: вы можете использовать кэш URL-адресов, которые уже были извлечены, с помощью map, но сам map не является безопасным для конкурентного использования!

```go
package main

import (
	"fmt"
)

type Fetcher interface {
	// Fetch returns the body of URL and
	// a slice of URLs found on that page.
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func Crawl(url string, depth int, fetcher Fetcher) {
	// TODO: Fetch URLs in parallel.
	// TODO: Don't fetch the same URL twice.
	// This implementation doesn't do either:
	if depth <= 0 {
		return
	}
	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Printf("found: %s %q\n", url, body)
	for _, u := range urls {
		Crawl(u, depth-1, fetcher)
	}
	return
}

func main() {
	Crawl("https://golang.org/", 4, fetcher)
}

// fakeFetcher is Fetcher that returns canned results.
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```

