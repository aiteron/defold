---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Переменные

**Lua -** это язык программирования со свободной типизацией. Это означает, что переменные не имеют тип данных. В Lua переменная - это просто контейнер, который может содержать любой тип значения языка и не привязан к каким-либо предыдущим значениям.

```lua
a = 5
b = "Hello"
a = b
print(a)    --> Hello
```

Все что нужно для создания переменной в Lua - это взять идентификатор (имя) который до этого не использовался и присвоить ему значение.

```lua
bird = "DUCK"
```

## Идентификаторы переменных

Идентификаторы в Lua могут быть любой последовательностью из букв (A-Z и a-z), цифр и символов подчеркивания, не начинающейся с цифры. Например:

```
i    j    i10    _ij    aSomewhatLongName    _INPUT
```

{% hint style="warning" %}
Лучше избегать идентификаторов, начинающихся с символа подчеркивания, за которым следует одна или несколько заглавных букв (например \_VERSION). Они зарезервированы в Lua для особых целей.
{% endhint %}

Зарезервированные слова (<mark style="color:red;">их нельзя использовать в качестве идентификаторов</mark>):

```
and      break    do      else   elseif
end      false    goto    for    function
if       in       local   nil    not
or       repeat   return  then   true
until    while
```

Lua учитывает регистр букв: **and** - зарезервированное слово, но And и AND - это два отличных от него и друг от друга идентификатора.

***

{% hint style="info" %}
В Lua не нужен разделитель между идущими подряд операторами. Но иногда удобно использовать разделитель ";" для записи двух и более операторов в одной строке:

```lua
a = 1
b = "boba"
c = 2 + 2; d = a + 3 
```
{% endhint %}

## Какие переменные в Lua бывают?

Переменные в Lua могут быть глобальными и локальными. Если переменная не объявлена явно как локальная, она считается глобальной.

### Глобальные переменные&#x20;

Глобальным переменным не нужны объявления. Их можно сразу использовать. Обращение к неинициализированной переменной не является ошибкой - вы всего лишь получите значение nil в качестве результата:

```lua
print(bird)    --> nil
bird = "DUCK"
print(bird)    --> DUCK
```

Глобальная переменная существует до тех пор, пока существует среда исполнения скрипта (в нашем случае - пока запущена игра) и доступна любому Lua-коду, выполняемому в этой среде.

При необходимости удалить глобальную переменную можно явным образом, просто присвоив ей значение <mark style="color:red;">nil</mark>. В таком случае Lua поведет себя так, как если бы эта переменная никогда не использовалась.

```lua
bird = "DUCK"
--<...>--
bird = nil
print(bird)    --> nil
```

### Локальные переменные

Любые локальные переменные должны быть объявлены явно с использованием ключевого слова <mark style="color:red;">local</mark>. Объявить локальную переменную можно в любом месте скрипта. Объявление может включать в себя присваивание переменной начального значения. Если значение не присвоено, переменная содержит <mark style="color:red;">nil</mark>.

```lua
local a = 1
local b
print(a)     --> 1
print(b)     --> nil
```

#### Область видимости локальной переменной

Область видимости локальной переменной начинается после объявления и продолжается до конца блока.

{% hint style="info" %}
Под блоком понимается:

* тело управляющей конструкции (if-then, else, for, while, repeat)
* тело функции
* фрагмент кода, заключенный в ключевые слова do <...> end
{% endhint %}

Если локальная переменная определена вне какого-либо блока, ее область видимости распространяется до конца скрипта.

```lua
a = 5    -- глобальная переменная a

local i = 1    -- переменная i локальная в пределах скрипта
while i <= a do    -- цикл от 1 до 5
    local a = i*i    -- локальная переменная a внутри цикла while
    print(a)    --> 1, 4, 9, 16, 25 
    i = i + 1
end

print(a)    --> 5 (здесь обращение к глобальной a)

if i > 5 then
    local a    -- локальная переменная a внутри then
    a = 10
    print(a)    --> 10
else
    print(a)    --> 5 (здесь обращение к глобальной a)
end

do
    local a = 20    -- локальная  переменная a внутри do-end
    print(a)    --> 20
end

print(a)    --> 5 (здесь обращение к глобальной a)
```

\*Подробнее об управляющих конструкциях и функциях вы узнаете в следующих статьях.

## Полезное

{% hint style="warning" %}
Когда возможно, рекомендуется использовать локальные переменные вместо глобальных. Это позволит избежать «засорения» глобального пространства имён и обеспечит лучшую производительность (поскольку доступ к локальным переменным в Lua выполняется несколько быстрее, чем к глобальным).
{% endhint %}

{% hint style="info" %}
Для переменных, значения которых не особо важно, удобно использовать индинтефикатор "\_" (одиночный символ подчеркивания).

```lua
data = { "A", "B", "C" }
for _, value in ipairs(data) do
    print(value)
end
```

Чаще всего используется в общем цикле for и при получении нескольких значений при вызове функции.
{% endhint %}

{% hint style="info" %}
Все глобальные переменные являются полями обычной таблицы, называемой **глобальным окружением**. Эта таблица доступна через глобальную переменную <mark style="color:red;">\_G</mark>

```lua
bird = "DUCK"

print(_G.bird)        --> DUCK
print(_G._G == _G)    --> true
```
{% endhint %}
