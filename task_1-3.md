---
work: 1
code: "task 1-3"
title: "Переменные, объекты, ссылочная модель"
---

<section class="theory" markdown="1">

## Теоретические сведения
{: .section__title}

До сих пор программы работали с числами и текстом, записанными прямо в `print()`. Чтобы программа могла сохранить результат вычисления и использовать его дальше, значению дают <dfn>имя</dfn>. Имена, привязанные к значениям, называют <dfn>переменными</dfn>.

В школьной информатике переменную часто описывают как «коробку», в которую кладут значение. Для Python эта картинка неверна и приводит к ошибкам, которые трудно найти. В Python переменная — **ярлык**, прикреплённый к объекту: у одного объекта может быть несколько ярлыков. Эта модель называется <dfn>ссылочной</dfn>. Она объясняет, почему изменение набора данных «через одно имя» становится видно «через другое», — а с наборами последовательностей вы будете работать весь курс.

</section>

<section class="goal" markdown="1">

## 🎯 Цель работы
{: .section__title}

Научиться:

- определять тип значения функцией `type()`;
- объяснять, как выполняется присваивание, и прослеживать значения переменных по шагам;
- выбирать допустимые и понятные имена переменных;
- объяснять, что происходит, когда у одного объекта несколько имён, и отличать изменение объекта от перепривязки имени;
- отличать сравнение значений `==` от проверки тождества `is` и правильно проверять отсутствие значения `is None`.

</section>

<aside class="callout callout--hint" markdown="1">

⏱ Время и место выполнения
{: .callout__title}

Задания этой страницы рассчитаны примерно на 15 минут и выполняются в аудитории. Теоретическую часть используйте как справочник: к нужному разделу ведут ссылки в условиях заданий.

</aside>

<!-- ===================== 1 ===================== -->

<section class="subsection" id="s1" markdown="1">

## <span class="subsection__num">1.</span> Значение всегда имеет тип
{: .subsection__title}

Каждое значение в Python — <dfn>объект</dfn>, и у каждого объекта есть <dfn>тип</dfn>. Тип определяет, что со значением можно делать: числа можно складывать, тексты — соединять, а делить текст на число нельзя.

| Значение | Тип | Что это |
|---|---|---|
| `17` | `int` | целое число |
| `4.2` | `float` | вещественное число |
| `'Hello, World!'` | `str` | строка символов (текст) |
| `True` | `bool` | логическое значение: истина или ложь |

<div class="syntax">type(значение)</div>

Встроенная функция `type()` сообщает тип значения. Подробно каждый тип разбирается в [task 1-4](task_1-4.html); здесь достаточно уметь спросить тип у интерпретатора.

<article class="example" markdown="1">

### Пример 1. Спросить тип у интерпретатора
{: .example__title}

Файл `show_types.py`:

```python
print(type(17))
print(type(4.2))
print(type('Hello, World!'))
print(type(True))
print(type('17'))
```

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
<class 'str'>
```
{: .output}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- В каждой строке сначала вычисляется `type(...)`, затем `print()` выводит результат. Запись `<class 'int'>` читается как «класс (тип) int».
- `True` записано без кавычек: это не текст, а логическое значение типа `bool`.
- Последние две строки различаются главным: `17` — число, `'17'` — строка из двух символов. Кавычки — признак строки.

</div>

</article>

</section>

<!-- ===================== 2 ===================== -->

<section class="subsection" id="s2" markdown="1">

## <span class="subsection__num">2.</span> Присваивание: знак `=` — это не равенство
{: .subsection__title}

<div class="syntax">имя = выражение</div>

<div class="definition" markdown="1">

<span class="definition__term">Присваивание</span> — команда, которая **сначала** вычисляет выражение справа от знака `=` и получает объект, **затем** привязывает к этому объекту имя, записанное слева.

</div>

<div class="definition" markdown="1">

<span class="definition__term">Переменная</span> — имя, привязанное к объекту. Само имя не является объектом и не содержит значения внутри себя: оно лишь указывает на объект.

</div>

- Порядок выполнения обратен порядку чтения: читаем слева направо, а выполняется справа налево. Объект появляется раньше, чем имя.
- Если имя уже было привязано к другому объекту, оно <dfn>перепривязывается</dfn> к новому. Старое значение не «перезаписывается внутри имени» — имя просто начинает указывать на другой объект.
- Имя можно использовать только после того, как оно привязано. Обращение к ещё не привязанному имени вызывает `NameError`.
- В математике *x* = *x* − 1 не имеет решения. В Python `x = x - 1` — обычная команда: «вычислить x − 1 и привязать к результату имя x».

<article class="example" markdown="1">

### Пример 2. Имя с обеих сторон знака `=`
{: .example__title}

Файл `rebind.py`:

```python
x = 43
x = x - 1
print(x)
```

```text
42
```
{: .output}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

| Шаг | Строка | Что происходит | x указывает на | Вывод |
|---|---|---|---|---|
| 1 | x = 43 | создаётся объект 43; имя x привязывается к нему | 43 | — |
| 2 | x = x - 1 | правая часть: берётся объект, на который указывает x, из него вычитается 1 → новый объект 42 | 43 | — |
| 3 | x = x - 1 | левая часть: имя x перепривязывается к объекту 42 | 42 | — |
| 4 | print(x) | выводится объект, на который указывает x | 42 | 42 |
{: .data-table--trace}

- На шаге 2 имя x ещё указывает на 43: без этого вычислить правую часть было бы невозможно.
- После шага 3 на объект 43 не указывает ни одно имя, и программе он больше недоступен. Память, занятую такими объектами, Python освобождает автоматически. Небольшие целые числа — исключение: интерпретатор CPython хранит их постоянно, в единственном экземпляре (это пригодится в разделе 6).

</div>

</article>

<article class="example" markdown="1">

### Пример 3. Имя до присваивания
{: .example__title}

Файл `not_yet.py`:

```python
print(gc_percent)
gc_percent = 60
```
{: .code-block--bad}

```text
Traceback (most recent call last):
  File "C:\Users\student\Desktop\python_work\not_yet.py", line 1, in <module>
    print(gc_percent)
          ^^^^^^^^^^
NameError: name 'gc_percent' is not defined
```
{: .output .output--error}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Программа выполняется сверху вниз. В строке 1 имя `gc_percent` ещё ни к чему не привязано, поэтому возникает ошибка времени выполнения `NameError: name 'gc_percent' is not defined` — «имя не определено».
- То, что ниже есть присваивание, не помогает: до строки 2 выполнение не дошло. Исправление — поменять строки местами.

</div>

</article>

<article class="task" id="task-1" markdown="1">

### Задание 1. Счётчик прочтений <span class="level level--1">Уровень 1 · воспроизведение</span>
{: .task__title}

Файл: <span class="task__file">task\_1-3\_1.py</span>
{: .task__meta}

Секвенатор выдал 1200 прочтений, из них 200 отбраковано по качеству. Напишите программу, которая:

1. привязывает имя `reads` к числу 1200 и выводит его;
1. перепривязывает имя `reads` к результату выражения, в котором из `reads` вычитается 200, и снова выводит его;
1. выводит тип значения, на которое указывает `reads`.

```text
1200
1000
<class 'int'>
```
{: .output data-label="Требуемый вывод"}

Число 1000 в программе не записывается — оно получается при вычислении.

</article>

</section>

<!-- ===================== 3 ===================== -->

<section class="subsection" id="s3" markdown="1">

## <span class="subsection__num">3.</span> Забудьте про коробки: переменная — это ярлык
{: .subsection__title}

<figure class="diagram">
<svg role="img" viewbox="0 0 640 200">
<title id="d1-title">Слева неверная модель: две коробки a и b, в каждой свой список [1, 2, 3]. Справа верная модель: один список [1, 2, 3] и два ярлыка a и b, указывающие на него.</title>
<defs>
<marker id="arr1" markerheight="7" markerwidth="7" orient="auto-start-reverse" refx="9" refy="5" viewbox="0 0 10 10">
<path class="d-arrowhead" d="M0,0 L10,5 L0,10 z"></path>
</marker>
</defs>
<text class="d-text" text-anchor="middle" x="150" y="24">Коробка — так НЕ работает</text>
<rect class="d-box" height="85" rx="4" width="105" x="35" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="87" y="68">a</text>
<text class="d-text d-text--mono" text-anchor="middle" x="87" y="105">[1, 2, 3]</text>
<rect class="d-box" height="85" rx="4" width="105" x="160" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="212" y="68">b</text>
<text class="d-text d-text--mono" text-anchor="middle" x="212" y="105">[1, 2, 3]</text>
<text class="d-text d-text--small" text-anchor="middle" x="150" y="165">два имени = две ёмкости = два значения</text>
<path class="d-arrow" d="M320,15 L320,185"></path>
<text class="d-text" text-anchor="middle" x="480" y="24">Ярлык — так работает</text>
<rect class="d-box d-box--name" height="30" rx="4" width="50" x="375" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="400" y="65">a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="50" x="535" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="560" y="65">b</text>
<rect class="d-box d-box--object" height="44" rx="4" width="150" x="405" y="105"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="480" y="132">[1, 2, 3]</text>
<path class="d-arrow" d="M405,75 L445,103" marker-end="url(#arr1)"></path>
<path class="d-arrow" d="M555,75 L515,103" marker-end="url(#arr1)"></path>
<text class="d-text d-text--small" text-anchor="middle" x="480" y="175">один объект, на нём две наклейки</text>
</svg>
<figcaption class="diagram__caption">Имя не объект. Имя привязывается к объекту, а не наоборот. Присваивание — это прикрепить ярлык, а не положить копию внутрь.</figcaption>
</figure>

В модели «коробки» команда `b = a` означала бы: «взять значение из коробки `a` и положить копию в коробку `b`». В Python она означает другое: «привязать имя `b` к тому же объекту, к которому привязано имя `a`». Копии не создаётся.

Посмотреть на имена и объекты можно в сервисе пошагового выполнения [pythontutor.com](https://pythontutor.com/), упомянутом на странице навигации. Он необязателен: всё нужное показано на схемах этой страницы.
{: .muted}

<article class="example" markdown="1">

### Пример 4. Два имени у числа и перепривязка
{: .example__title}

Файл `reads_before.py`:

```python
reads_total = 1500
reads_before = reads_total
reads_total = reads_total - 300
print(reads_total)
print(reads_before)
```

```text
1200
1500
```
{: .output}

<figure class="diagram">
<svg role="img" viewbox="0 0 640 200">
<title id="d2-title">После строки 2 оба имени reads_total и reads_before указывают на объект 1500. После строки 3 имя reads_total указывает на новый объект 1200, а reads_before по-прежнему на 1500.</title>
<defs>
<marker id="arr2" markerheight="7" markerwidth="7" orient="auto-start-reverse" refx="9" refy="5" viewbox="0 0 10 10">
<path class="d-arrowhead" d="M0,0 L10,5 L0,10 z"></path>
</marker>
</defs>
<text class="d-text d-text--small" text-anchor="middle" x="155" y="22">после строки 2</text>
<rect class="d-box d-box--name" height="30" rx="4" width="135" x="10" y="40"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="77" y="60">reads_total</text>
<rect class="d-box d-box--name" height="30" rx="4" width="135" x="165" y="40"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="232" y="60">reads_before</text>
<rect class="d-box d-box--object" height="40" rx="4" width="110" x="100" y="120"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="155" y="145">1500</text>
<path class="d-arrow" d="M80,70 L130,118" marker-end="url(#arr2)"></path>
<path class="d-arrow" d="M230,70 L180,118" marker-end="url(#arr2)"></path>
<path class="d-arrow" d="M320,15 L320,185"></path>
<text class="d-text d-text--small" text-anchor="middle" x="485" y="22">после строки 3</text>
<rect class="d-box d-box--name" height="30" rx="4" width="135" x="340" y="40"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="407" y="60">reads_total</text>
<rect class="d-box d-box--name" height="30" rx="4" width="135" x="495" y="40"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="562" y="60">reads_before</text>
<rect class="d-box d-box--object" height="40" rx="4" width="110" x="352" y="120"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="407" y="145">1200</text>
<rect class="d-box d-box--object" height="40" rx="4" width="110" x="507" y="120"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="562" y="145">1500</text>
<path class="d-arrow" d="M407,70 L407,117" marker-end="url(#arr2)"></path>
<path class="d-arrow" d="M562,70 L562,117" marker-end="url(#arr2)"></path>
</svg>
<figcaption class="diagram__caption">Строка 3 не меняет объект 1500: она создаёт новый объект 1200 и перепривязывает к нему только имя <code>reads_total</code>.</figcaption>
</figure>

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строка 2 не копирует число: после неё оба имени указывают на один объект 1500.
- Строка 3 вычисляет правую часть (1500 − 300 = 1200) и получает **новый** объект. Имя `reads_total` перепривязывается к нему. Имя `reads_before` никто не трогал — оно по-прежнему указывает на 1500.
- Числа (как и строки) — <dfn>неизменяемые</dfn> объекты: изменить объект 1500 невозможно, можно только получить новый объект. Поэтому, сколько бы имён ни было у числа, «испортить» его через одно из них нельзя.

</div>

</article>

</section>

<!-- ===================== 4 ===================== -->

<section class="subsection" id="s4" markdown="1">

## <span class="subsection__num">4.</span> Имена переменных
{: .subsection__title}

### Правила языка
{: .block-title}

- Имя состоит из букв, цифр и знака подчёркивания `_`.
- Имя не может начинаться с цифры.
- В имени нельзя использовать пробелы и другие знаки: `@`, `-`, `.`, `$` и т. п.
- Имя не может совпадать с <dfn>ключевым словом</dfn> — словом, зарезервированным для конструкций языка.
- **Регистр важен:** `LaTeX` и `latex` — разные имена.

Ключевых слов в Python 3.11–3.14 — 35:

<table>
<tbody>
<tr><td><code>False</code></td><td><code>None</code></td><td><code>True</code></td><td><code>and</code></td><td><code>as</code></td><td><code>assert</code></td><td><code>async</code></td></tr>
<tr><td><code>await</code></td><td><code>break</code></td><td><code>class</code></td><td><code>continue</code></td><td><code>def</code></td><td><code>del</code></td><td><code>elif</code></td></tr>
<tr><td><code>else</code></td><td><code>except</code></td><td><code>finally</code></td><td><code>for</code></td><td><code>from</code></td><td><code>global</code></td><td><code>if</code></td></tr>
<tr><td><code>import</code></td><td><code>in</code></td><td><code>is</code></td><td><code>lambda</code></td><td><code>nonlocal</code></td><td><code>not</code></td><td><code>or</code></td></tr>
<tr><td><code>pass</code></td><td><code>raise</code></td><td><code>return</code></td><td><code>try</code></td><td><code>while</code></td><td><code>with</code></td><td><code>yield</code></td></tr>
</tbody>
</table>

Три из них — `False`, `None`, `True` — пишутся с заглавной буквы. Запоминать список не нужно: редактор выделяет ключевые слова цветом.
{: .muted}

<article class="example" markdown="1">

### Пример 5. Недопустимые имена
{: .example__title}

Файл `bad_names.py` с одной строкой `76trombones = 'big parade'`:

```text
  File "C:\Users\student\Desktop\python_work\bad_names.py", line 1
    76trombones = 'big parade'
     ^
SyntaxError: invalid decimal literal
```
{: .output .output--error}

Другие недопустимые имена, каждое в отдельном файле из одной строки:

| Строка программы | Что не так | Последняя строка сообщения |
|---|---|---|
| `76trombones = 'big parade'` | имя начинается с цифры | `SyntaxError: invalid decimal literal` |
| `more@ = 1000000` | недопустимый символ `@` | `SyntaxError: invalid syntax` |
| `class = 'Advanced Theoretical Zymurgy'` | `class` — ключевое слово | `SyntaxError: invalid syntax` |
| `bad name = 5` | пробел: Python видит два имени подряд без знака между ними | `SyntaxError: invalid syntax` |

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Все четыре ошибки синтаксические: программа не выполнится ни в одной строке (task 1-0).
- В первом случае Python пытается прочитать `76trombones` как число, встречает буквы и сообщает о неправильной записи числа: `invalid decimal literal` — «неверная запись десятичного числа».
- В остальных случаях сообщение общее — `invalid syntax`, и причину приходится искать самому по правилам выше.

</div>

</article>

### Понятные имена
{: .block-title}

Интерпретатору безразлично, как названы переменные. Человеку — нет.

<article class="example" markdown="1">

### Пример 6. Три одинаковые программы
{: .example__title}

```python
a = 35.0
b = 12.50
c = a * b
print(c)
```
{: data-label="Абстрактно"}

```python
hours = 35.0
rate = 12.50
pay = hours * rate
print(pay)
```
{: data-label="Понятно"}

```python
x1q3z9ahd = 35.0
x1q3z9afd = 12.50
x1q3p9afd = x1q3z9ahd * x1q3z9afd
print(x1q3p9afd)
```
{: data-label="Непонятно"}

```text
437.5
```
{: .output data-label="Вывод каждой из трёх программ"}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Для интерпретатора это одна и та же программа: умножить 35.0 на 12.50 и вывести результат.
- Во второй программе имена сами объясняют смысл: часы, ставка, оплата. Такие имена называют <dfn>мнемоничными</dfn> — они помогают запомнить, зачем переменная заведена.
- В третьей имена отличаются одной-двумя буквами, и перепутать их очень легко. Python не проверяет смысл имён — ошибку никто не заметит.

</div>

</article>

По PEP 8 (task 1-1) имена переменных пишут строчными буквами, слова разделяют подчёркиванием: `gc_content`, `sample_count`, `read_length`. Имя должно говорить, *что* в нём хранится; однобуквенные имена уместны только в коротких абстрактных примерах.

<div class="mistake" markdown="1">

Типичная ошибка: опечатка в имени — причина выше места обнаружения
{: .mistake__title}

Файл `typo_name.py`:

```python
sampel_count = 12
print('Число образцов:')
print(sample_count)
```
{: .code-block--bad}

```text
Число образцов:
Traceback (most recent call last):
  File "C:\Users\student\Desktop\python_work\typo_name.py", line 3, in <module>
    print(sample_count)
          ^^^^^^^^^^^^
NameError: name 'sample_count' is not defined. Did you mean: 'sampel_count'?
```
{: .output .output--error}

Ошибка обнаружена в строке 3, а допущена в строке 1: имя при присваивании написано с опечаткой. Python подсказывает похожее имя. Это ровно тот случай из task 1-0, когда место обнаружения не совпадает с местом ошибки.

</div>

<div class="mistake" markdown="1">

Типичная ошибка: имя встроенной функции в роли переменной
{: .mistake__title}

Файл `shadow.py`:

```python
print = 'готово'
print('Анализ завершён')
```
{: .code-block--bad}

```text
Traceback (most recent call last):
  File "C:\Users\student\Desktop\python_work\shadow.py", line 2, in <module>
    print('Анализ завершён')
    ~~~~~^^^^^^^^^^^^^^^^^^^
TypeError: 'str' object is not callable
```
{: .output .output--error data-label="Вывод терминала (Python 3.13 и новее)"}

`print` — не ключевое слово, а обычное имя, привязанное к встроенной функции. Строка 1 перепривязала его к строке `'готово'`. В строке 2 Python пытается «вызвать» строку и сообщает: `'str' object is not callable` — «объект типа str нельзя вызвать». В Python 3.11–3.12 сообщение то же, но без строки со значками `~` и `^`. Не называйте переменные `print`, `type`, `len`, `id` и другими именами встроенных функций.

</div>

</section>

<!-- ===================== 5 ===================== -->

<section class="subsection" id="s5" markdown="1">

## <span class="subsection__num">5.</span> Изменяемый объект и два имени
{: .subsection__title}

С числами ссылочная модель безопасна: изменить число нельзя. Всё меняется, когда объект <dfn>изменяемый</dfn> — его содержимое можно поменять, не создавая нового объекта. Самый частый изменяемый объект — **список**.

<div class="overview" markdown="1">

Обзорно
{: .overview__label}

Здесь показан только минимум о списках, нужный для понимания ссылочной модели. Подробно списки изучаются в теме 5 «Списки и кортежи».
{: .overview__ref}

| Запись | Что делает |
|---|---|
| `[1, 2, 3]` | создаёт новый список из трёх элементов; элементы перечисляются через запятую в квадратных скобках |
| `print(a)` | выводит список целиком, в квадратных скобках: `[1, 2, 3]` |
| `a.append(4)` | **изменяет** список: добавляет элемент 4 в конец. Новый список не создаётся |
| `a[0]` | элемент списка с номером 0; нумерация элементов начинается с нуля: `a[0]` — первый, `a[1]` — второй |
| `a[0] = 17` | **изменяет** список: заменяет первый элемент на 17 |

</div>

<article class="example" markdown="1">

### Пример 7. Копии не было
{: .example__title}

Файл `no_copy.py`:

```python
a = [1, 2, 3]
b = a
a.append(4)
print(b)
```

```text
[1, 2, 3, 4]
```
{: .output}

<figure class="diagram">
<svg role="img" viewbox="0 0 660 190">
<title id="d3-title">Три шага: после a = [1, 2, 3] имя a указывает на список; после b = a оба имени указывают на тот же список; после a.append(4) оба имени указывают на тот же список, который теперь содержит [1, 2, 3, 4].</title>
<defs>
<marker id="arr3" markerheight="7" markerwidth="7" orient="auto-start-reverse" refx="9" refy="5" viewbox="0 0 10 10">
<path class="d-arrowhead" d="M0,0 L10,5 L0,10 z"></path>
</marker>
</defs>
<text class="d-text d-text--mono" text-anchor="middle" x="100" y="24">a = [1, 2, 3]</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="20" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="42" y="65">a</text>
<rect class="d-box d-box--object" height="44" rx="4" width="150" x="25" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="100" y="137">[1, 2, 3]</text>
<path class="d-arrow" d="M48,75 L78,108" marker-end="url(#arr3)"></path>
<text class="d-text d-text--small" text-anchor="middle" x="100" y="180">1 объект, 1 имя</text>
<path class="d-arrow" d="M210,15 L210,180"></path>
<text class="d-text d-text--mono" text-anchor="middle" x="320" y="24">b = a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="240" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="262" y="65">a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="356" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="378" y="65">b</text>
<rect class="d-box d-box--object" height="44" rx="4" width="150" x="245" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="320" y="137">[1, 2, 3]</text>
<path class="d-arrow" d="M268,75 L298,108" marker-end="url(#arr3)"></path>
<path class="d-arrow" d="M372,75 L342,108" marker-end="url(#arr3)"></path>
<text class="d-text d-text--small" text-anchor="middle" x="320" y="180">1 объект, 2 имени</text>
<path class="d-arrow" d="M430,15 L430,180"></path>
<text class="d-text d-text--mono" text-anchor="middle" x="540" y="24">a.append(4)</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="460" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="482" y="65">a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="576" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="598" y="65">b</text>
<rect class="d-box d-box--object" height="44" rx="4" width="170" x="455" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="540" y="137">[1, 2, 3, 4]</text>
<path class="d-arrow" d="M488,75 L518,108" marker-end="url(#arr3)"></path>
<path class="d-arrow" d="M592,75 L562,108" marker-end="url(#arr3)"></path>
<text class="d-text d-text--small" text-anchor="middle" x="540" y="180">1 объект, 2 имени</text>
</svg>
<figcaption class="diagram__caption">Число объектов не менялось ни разу — менялось только число ярлыков и содержимое объекта.</figcaption>
</figure>

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строка 1 создаёт список и привязывает к нему имя `a`.
- Строка 2 не копирует список: имя `b` привязывается к тому же объекту.
- Строка 3 изменяет сам объект — добавляет в него 4. Изменили «через `a`» — это видно «через `b`», потому что объект один.
- Строка 4 выводит объект, на который указывает `b`, — тот самый, уже изменённый список.

</div>

</article>

<article class="example" markdown="1">

### Пример 8. Изменение через второе имя
{: .example__title}

Файл `change_via_b.py`:

```python
a = [1, 2, 3]
b = a
b[0] = 17
print(a)
print(a[1])
```

```text
[17, 2, 3]
2
```
{: .output}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строка 3 заменяет первый элемент (номер 0) объекта, на который указывает `b`. Это тот же объект, что и у `a`, поэтому строка 4 выводит `[17, 2, 3]`.
- Строка 5 выводит элемент с номером 1 — второй по счёту: `2`.
- Ярлыки равноправны: неважно, через какое имя изменили объект, — изменился сам объект.

</div>

</article>

<article class="example" markdown="1">

### Пример 9. Изменение объекта или перепривязка имени
{: .example__title}

Файл `rebind_list.py`:

```python
a = [1, 2, 3]
b = a
a = [7, 8]
print(a)
print(b)
```

```text
[7, 8]
[1, 2, 3]
```
{: .output}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строка 3 — присваивание: справа создаётся **новый** список `[7, 8]`, и к нему перепривязывается имя `a`. Старый список не изменился.
- Имя `b` по-прежнему указывает на старый список `[1, 2, 3]`.
- Сравните с примером 7: `a.append(4)` **меняет объект** — видно через все имена; `a = ...` **перепривязывает одно имя** — остальные имена это не затрагивает.

</div>

</article>

<aside class="callout callout--important" markdown="1">

Опасность — не в двух именах, а в сочетании «два имени + изменяемый объект»
{: .callout__title}

Для чисел и строк несколько имён безопасны: изменить такой объект нельзя. Для списков изменение через одно имя видно через все остальные, и если об этом забыть, данные «портятся» незаметно. Продолжение этой темы — в теме 5 «Списки и кортежи» (как сделать копию списка) и в теме 7 «Функции» (что получает функция, которой передали список).

</aside>

<article class="task" id="task-2" markdown="1">

### Задание 2. Длины праймеров <span class="level level--3">Уровень 3 · комбинирование</span>
{: .task__title}

Файл: <span class="task__file">task\_1-3\_2.py</span>
{: .task__meta}

```python
primers = [18, 20, 22]
backup = primers
same_lengths = [18, 20, 22]
backup.append(25)
primers[0] = 19
print(primers)
print(backup)
print(same_lengths)
```
{: data-label="Программа"}

1. **Не запуская программу**, запишите комментариями, что выведет каждая из трёх последних строк. Для каждого прогноза объясните, какие имена указывают на один объект.
1. Нарисуйте словами схему объектов после строки 5: сколько списков существует, какое содержимое у каждого, какие имена к какому списку привязаны.
1. Скопируйте программу в файл, запустите и сравните результат с прогнозом.
{: .steps}

</article>

</section>

<!-- ===================== 6 ===================== -->

<section class="subsection" id="s6" markdown="1">

## <span class="subsection__num">6.</span> Равны — не значит те же самые: `==` и `is`
{: .subsection__title}

| Оператор | Что сравнивает | Вопрос |
|---|---|---|
| `a == b` | значения (содержимое) | «Одинаковое ли содержимое у объектов?» |
| `a is b` | тождество | «Это один и тот же объект?» |
| `a is not b` | тождество, с отрицанием | «Это разные объекты?» |

Результат сравнения — логическое значение: `True` (истина) или `False` (ложь). Подробнее о нём — в [task 1-4](task_1-4.html) и в теме 3 «Логика и ветвления».

<div class="definition" markdown="1">

<span class="definition__term">Тождество объекта</span> — его «личность». Оно не меняется за всю жизнь объекта, даже если меняется содержимое. Встроенная функция `id(объект)` возвращает тождество в виде целого числа; оператор `is` сравнивает тождества.

</div>

<article class="example" markdown="1">

### Пример 10. Один объект и два одинаковых
{: .example__title}

Файл `eq_is.py`:

```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]
print(a == b)
print(a is b)
print(a == c)
print(a is c)
```

```text
True
True
True
False
```
{: .output}

<figure class="diagram">
<svg role="img" viewbox="0 0 640 200">
<title id="d4-title">Слева имена a и b указывают на один список [1, 2, 3]. Справа имя a указывает на один список [1, 2, 3], а имя c — на другой список с тем же содержимым.</title>
<defs>
<marker id="arr4" markerheight="7" markerwidth="7" orient="auto-start-reverse" refx="9" refy="5" viewbox="0 0 10 10">
<path class="d-arrowhead" d="M0,0 L10,5 L0,10 z"></path>
</marker>
</defs>
<text class="d-text d-text--small" text-anchor="middle" x="160" y="22">a и b — один объект</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="70" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="92" y="65">a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="206" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="228" y="65">b</text>
<rect class="d-box d-box--object" height="44" rx="4" width="150" x="85" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="160" y="137">[1, 2, 3]</text>
<path class="d-arrow" d="M98,75 L128,108" marker-end="url(#arr4)"></path>
<path class="d-arrow" d="M222,75 L192,108" marker-end="url(#arr4)"></path>
<text class="d-text d-text--mono d-text--small" text-anchor="middle" x="160" y="182">a == b → True,  a is b → True</text>
<path class="d-arrow" d="M320,15 L320,190"></path>
<text class="d-text d-text--small" text-anchor="middle" x="485" y="22">a и c — два объекта</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="388" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="410" y="65">a</text>
<rect class="d-box d-box--name" height="30" rx="4" width="44" x="538" y="45"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="560" y="65">c</text>
<rect class="d-box d-box--object" height="44" rx="4" width="130" x="345" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="410" y="137">[1, 2, 3]</text>
<rect class="d-box d-box--object" height="44" rx="4" width="130" x="495" y="110"></rect>
<text class="d-text d-text--mono" text-anchor="middle" x="560" y="137">[1, 2, 3]</text>
<path class="d-arrow" d="M410,75 L410,107" marker-end="url(#arr4)"></path>
<path class="d-arrow" d="M560,75 L560,107" marker-end="url(#arr4)"></path>
<text class="d-text d-text--mono d-text--small" text-anchor="middle" x="485" y="182">a == c → True,  a is c → False</text>
</svg>
<figcaption class="diagram__caption">Два разных объекта могут быть равны по содержимому.</figcaption>
</figure>

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строки 1 и 3 создают **два разных** списка с одинаковым содержимым. Строка 2 создаёт не список, а второе имя для первого списка.
- `a == b` и `a is b` — оба `True`: это один объект, а объект равен сам себе.
- `a == c` — `True`: содержимое одинаковое. `a is c` — `False`: это разные объекты. Если теперь выполнить `a.append(4)`, список `c` не изменится.

</div>

</article>

<article class="example" markdown="1">

### Пример 11. Функция `id()`
{: .example__title}

Числа, которые возвращает `id()`, меняются от запуска к запуску, поэтому в программах сравнивают сами тождества. Файл `ids.py`:

```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]
print(id(a) == id(b))
print(id(a) == id(c))
```

```text
True
False
```
{: .output}

В интерактивном режиме можно посмотреть и само число:

```text
>>> @@x = [1, 2, 3]@@
>>> @@id(x)@@
140128661719872
```
{: .output data-label="Интерактивный режим (пример; число у вас будет другим)"}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- `id(a) == id(b)` — то же самое, что `a is b`: тождества совпадают, объект один.
- Само число смысла не несёт: оно лишь различает объекты, пока они существуют. В обычном коде `id()` почти не пишут; функция нужна при отладке, когда два объекта выглядят одинаково, а понять надо, один это объект или два.

</div>

</article>

<div class="overview" markdown="1">

Обзорно
{: .overview__label}

Значение `None` и проверки с ним подробно изучаются в теме 3 «Логика и ветвления».
{: .overview__ref}

`None` — особое значение «ничего нет». Его используют, когда значение отсутствует: например, у записи последовательности нет описания. Это не пустая строка и не ноль, а отдельный объект.

Объект `None` в программе существует ровно один. Поэтому проверять «значение отсутствует?» правильно через тождество: `x is None`; «значение есть?» — `x is not None`. Это единственный частый случай, когда нужен `is`.

<article class="example" markdown="1">

### Пример 12. Описание есть или нет
{: .example__title}

Файл `description.py`:

```python
description = None
print(description is None)
description = 'плазмида pUC19'
print(description is None)
print(description is not None)
```

```text
True
False
True
```
{: .output}

<div class="analysis" markdown="1">

Разбор
{: .analysis__title}

- Строка 1: описания пока нет, имя `description` привязано к `None`. Строка 2 проверяет это и выводит `True`.
- Строка 3 перепривязывает имя к строке. Теперь `description is None` — `False`, а `description is not None` — `True`.

</div>

</article>

</div>

<div class="mistake" markdown="1">

Типичная ошибка: `is` для сравнения чисел и строк
{: .mistake__title}

Файл `is_number.py`:

```python
samples = 5
print(samples is 5)
```
{: .code-block--bad}

```text
C:\Users\student\Desktop\python_work\is_number.py:2: SyntaxWarning: "is" with 'int' literal. Did you mean "=="?
  print(samples is 5)
True
```
{: .output .output--error data-label="Вывод терминала (Python 3.12 и новее)"}

```python
samples = 5
print(samples == 5)
```
{: .code-block--good}

Python выполнил программу, но выдал <dfn>предупреждение</dfn> `SyntaxWarning`: «is с числом. Вы имели в виду ==?». В Python 3.11 текст чуть другой: `"is" with a literal`. Результат `True` здесь — случайность реализации: небольшие целые числа CPython хранит в единственном экземпляре (пример 2). Для других чисел результат иной — вот что показывает интерактивный режим:

```text
>>> @@a = 1000@@
>>> @@b = 1000@@
>>> @@a == b@@
True
>>> @@a is b@@
False
>>> @@c = 5@@
>>> @@d = 5@@
>>> @@c is d@@
True
```
{: .output data-label="Интерактивный режим, Python 3.11–3.14"}

Одинаковые по записи сравнения дают разный результат. Правило: значения сравнивают через `==`; `is` — только для `None` и для вопроса «один ли это объект».

</div>

</section>

<!-- ===================== ИТОГОВОЕ ===================== -->

<section class="final-task" id="final" markdown="1">

## Итоговое задание: общий штатив <span class="level level--5">Уровень 5 · итоговое</span>

Файл: <span class="task__file">task\_1-3\_rack.py</span>
{: .task__meta}

**Ситуация.** В холодильнике стоит штатив с образцами 101, 102, 103. Лаборант Анна работает с этим штативом, а лаборант Борис получает *тот же самый* штатив, а не копию. Борис ставит в штатив образец 104. В соседнем холодильнике стоит другой штатив, в котором лежат такие же образцы 101, 102, 103, 104. Затем Анна заменяет первый образец в своём штативе на 201. Описание общего штатива пока не задано.

Напишите программу, которая моделирует эту ситуацию и выводит ровно следующее:

```text
Штатив Бориса совпадает со штативом Анны по содержимому:
True
Это один и тот же штатив:
True
Соседний штатив — тот же объект:
False
После замены первого образца штатив Бориса:
[201, 102, 103, 104]
Соседний штатив:
[101, 102, 103, 104]
Описание отсутствует:
True
```
{: .output data-label="Требуемый вывод"}

**Требования:**

1. Штатив Анны и Бориса — один список, созданный одной записью в квадратных скобках; у него два имени. Соседний штатив — отдельный список.
1. Образец 104 добавляется в общий штатив через имя Бориса методом `append()`; замена первого образца выполняется через имя Анны.
1. Списки выводятся только через имена переменных; значения `True` и `False` получаются только как результаты сравнений `==`, `is`, `is None`. Отсутствие описания моделируется значением `None`.
1. Имена понятные и оформлены по PEP 8.
1. В конце файла ответьте комментарием: сколько объектов-списков создала программа и сколько имён указывает на каждый из них.

</section>

<!-- ===================== ЗАТЕМ ===================== -->

<section class="then" markdown="1">

## Затем
{: .then__title}

1. Внутри папки `projects_1` создайте папку `task_1_3`.
1. Переместите в папку `task_1_3` все файлы этого занятия: <span class="task__file">task\_1-3\_1.py</span>, <span class="task__file">task\_1-3\_2.py</span> и <span class="task__file">task\_1-3\_rack.py</span>.
{: .steps}

Итоговая структура папок должна выглядеть так:

```text
ivanov_ii/
└── projects_1/
    ├── task_1_2/
    │   ├── task_1-2_1.py
    │   ├── task_1-2_2.py
    │   ├── task_1-2_launch.py
    │   └── task_1-2_launch_log.txt
    └── @@task_1_3/@@
        ├── task_1-3_1.py
        ├── task_1-3_2.py
        └── task_1-3_rack.py
```
{: .folder-tree}

</section>
