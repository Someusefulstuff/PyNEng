## Функция all

Функция all() возвращает True, если все элементы истина (или объект пустой).
```python
In [1]: all([False, True, True])
Out[1]: False

In [2]: all([True, True, True])
Out[2]: True

In [3]: all([])
Out[3]: True
```

Например, с помощью all можно проверить, все ли октеты в IP-адресе являются числами:
```python
In [4]: IP = '10.0.1.1'

In [5]: all( i.isdigit() for i in IP.split('.'))
Out[5]: True

In [6]: all( i.isdigit() for i in '10.1.1.a'.split('.'))
Out[6]: False
```


## Функция any

Функция any() возвращает True, если хотя бы один элемент истина (или объект пустой).
```python
In [7]: any([False, True, True])
Out[7]: True

In [8]: any([False, False, False])
Out[8]: False

In [9]: any([])
Out[9]: False

In [10]: any( i.isdigit() for i in '10.1.1.a'.split('.'))
Out[10]: True
```


Например, с помощью any, можно заменить функцию ignore_command:
```python
def ignore_command(command, ignore):
    '''
    Функция проверяет содержится ли в команде слово из списка ignore.
    * command - строка. Команда, которую надо проверить
    * ignore - список. Список слов
    * Возвращает True, если в команде содержится слово из списка ignore, False - если нет
    '''
    ignore = ['duplex', 'alias', 'Current configuration']

    ignore_command = False

    for word in ignore:
        if word in command:
            return True
    return ignore_command
```

На такой вариант:
```python
def ignore_command(command, ignore):
    '''
    Функция проверяет содержится ли в команде слово из списка ignore.
    command - строка. Команда, которую надо проверить
    ignore - список. Список слов
    Возвращает True, если в команде содержится слово из списка ignore, False - если нет
    '''
    ignore = ['duplex', 'alias', 'Current configuration']

    return any(word in command for word in ignore)
```

