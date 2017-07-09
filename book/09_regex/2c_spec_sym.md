## Специальные символы

* ```.``` - любой символ, кроме символа новой строки
* ```^``` - начало строки
* ```$``` - конец строки
* ```[abc]``` - любой символ в скобках
* ```[^abc]``` - любой символ, кроме тех, что в скобках
* ```a|b``` - элемент a или b
* ```(regex)``` - выражение рассматривается как один элемент. Кроме того, подстрока, которая совпала с выражением, запоминается


### ```.```

Точка обозначает любой символ.
Выражение ```.0``` означает любой символ, а затем 0.
В строке line совпадением с таким регулярным выражением будет подстрока '10':
```python
In [1]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [2]: re.search('.0', line).group()
Out[2]: '10'
```

> Функция search ищет только одно совпадение.

Выражение ```... ``` означает три любых символа и пробел:
```py
In [3]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [4]: re.search('... ', line).group()
Out[4]: '100 '

```

### ```^```

Символ ```^``` означает начало строки. Выражению ```^..``` соответствует подстрока:
```python
In [5]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [6]: re.search('^..', line).group()
Out[6]: '10'
```

Первые три символа с начала строки
```py
In [7]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [8]: re.search('^...', line).group()
Out[8]: '100'

```

### ```$```

Символ ```$``` обозначает конец строки:
```python
In [9]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"


In [10]: re.search('...$', line).group()
Out[10]: '0/1'
```

### ```[]```

Символы, которые перечислены в квадратных скобках, означают, что любой из этих символов будет совпадением.
Таким образом можно описывать разные регистры:
```python
In [11]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [12]: re.search('[Ff]ast', line).group()
Out[12]: 'Fast'

In [13]: re.search('[Ff]ast[Ee]thernet', line).group()
Out[13]: 'FastEthernet'

```

В квадратных скобках можно указывать диапазоны символов.
Например, таким образом можно указать, что нас интересует любая цифра от 0 до 9:
```py
In [14]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [15]: re.search('[0-9]', line).group()
Out[15]: '1'

```

Аналогичным образом можно указать буквы:
```py
In [16]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [17]: re.search('[a-z]', line).group()
Out[17]: 'a'

In [18]: re.search('[A-Z]', line).group()
Out[18]: 'F'

```

В квадратных скобках можно указывать несколько диапазонов:
```py
In [19]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [20]: re.search('[A-Za-z][0-9][A-Za-z]', line).group()
Out[20]: 'a5d'

```

Выражение ```[A-Za-z][0-9][A-Za-z]``` описывает три символа: букву, цифру и букву.
Соответственно и совпадение получилось 'a5d'.

Выражение ```[abc][0-9]``` означает два символа: первый - a, b или c, а второй - число от 0 до 9:
```py
In [21]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [22]: re.search('[abc][0-9]', line).group()
Out[22]: 'a1'
```

Еще одна особенность квадратных скобок - специальные символы внутри квадратных скобок теряют свое специальное значение и обозначают просто символ.
Например, точка внутри квадратных скобок будет обозначать точку, а не любой символ.

Выражение ```[a-f0-9][./][a-f0-9]``` описывает три символа:
1. буква a, b, c, d, e или f или цифра от 0 до 9
2. точка или слеш
3. буква a, b, c, d, e или f или цифра от 0 до 9

Для строки line совпадением будет такая подстрока:
```py
In [23]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [24]: re.search('[a-f0-9][./][a-f0-9]', line).group()
Out[24]: '2.3'

```


Если после открывающейся квадратной скобки, указан символ ```^```, совпадением будет любой символ, кроме указанных в скобках:
```python
In [25]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [26]: re.search('[0-9][^a-zA-Z]', line).group()
Out[26]: '10'
```

В данном случае, выражение описывает два символа:
* цифру
* все, кроме букв

### ```|```

Вертикальная черта работает как 'или':
```python
In [27]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [28]: re.search('Fast|0/1', line).group()
Out[28]: 'Fast'
```

Обратите внимание на то, как срабатывает ```|``` - Fast и 0/1 воспринимаются как целое выражение.
То есть, в итоге выражение означает, что мы ищем Fast или 0/1, а не то, что мы ищем  Fas, затем t или 0 и 0/1.


### ```()```

Скобки используются для группировки выражений.
Как и в математических выражениях, с помощью скобок можно указать к каким элементам применяется операция.

Например, выражение ```[0-9]([a-f]|[0-9])[0-9]``` описывает три символа: цифра, потом буква или цифра и цифра:
```python
In [29]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

In [30]: re.search('[0-9]([a-f]|[0-9])[0-9]', line).group()
Out[30]: '100'

```

Скобки позволяют указывать какое выражение является одним целым.
Это особенно полезно при использовании символов повторения.
Они будут рассматриваться позже, но пока что небольшой пример:
```py
In [34]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

In [35]: re.search('a1+', line).group()
Out[35]: 'a1'
```

Тут выражение описывает, что должна идти буква a, а затем 1, как минимум, один раз.

Если же взять в скобки a1, это будет означать, что подстрока 'a1' должна встретиться один или более раз:
```py
In [36]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

In [37]: re.search('(a1)+', line).group()
Out[37]: 'a1a1'
```

Тут скобки использовались, чтобы указать, что повторяться должна подстрока a1, а не только единица.


Скобки позволяют не только группировать выражения.
Кроме того, строка, которые совпала с выражением в скобках, запоминается.
Ее можно получить отдельно с помощью специальных методов groups и group(n).
Это рассматривается в отдельном разделе "Группировка выражений".

