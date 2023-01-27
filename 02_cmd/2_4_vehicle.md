# Кодирование системы команд

lesson = 318782

## SKIP VIDEO Инсталляция эталонного эмулятора

https://youtu.be/R2HIHQUfXZ4

## Система команд машинного кода

Подумаем о кодировании системы команд в машинные коды. 

* Однозначность кодирования и декодирования.
* Мнемоничность набора команд (первые программисты писали прямо в машинных кодах и еще долго для быстрой отладки стоило понимать фрагмент в машинном коде, без мнемоник).
* Размер получающейся программы (памяти мало, программа сама занимает место в памяти).

Если бы мы разрабатывали процессор, то еще обратили бы внимание на:

* Быстроту выполнения команд,
* Насколько можно оптимизировать выполнение и тп.

*Хорошо, что мы не разрабатываем процессор.*



## Система команд игрушечного автомобиля

Попробуем создать простейшую систему команд и закодировать ее на примере кодируемого игрушечного автомобиля.

### 1-битная система команд

Система команд сильно зависит от длины машинного слова. Давайте будем делать одну команду длинной в одно слово.

Начнем с систем команд для игрушечного автомобиля, где машинное слово длиной 1 бит. То есть у нас две команды: 0 и 1. Пусть 0 будет `mov` (автомобиль 1 секунду едет вперед), а 1 будет `stop` (автомобиль стоит).

Программа `1 1 1 0` - это "ехать вперед 3 секунды, потом остановиться".

| Код команды | Команда |
|----|----|
| 1 | `mov` |
| 0 | `stop` |

Можно кодировать наоборот, 0 - ехать, 1 - стоять? Можно, какую систему придумаем, такую и реализуем в автомобиле.

### 2-битная система команд

После успешного испытания образца, решили оставить игрушечный автомобиль, но память в него и процессор установить 2-битный. То есть машинные слова будут 00, 01, 10, 11.

Добавим еще команды поворота налево и направо. Можно устраивать прохождение извилистой трассы заранее запрограммированной программой.

| Код команды | Команда |
|----|----|
| 00 | `stop` |
| 11 | `mov` |
| 01 | `turn right` |
| 10 | `turn left` |

Можно сделать другой набор команд? Можно, например так:

| Код команды | Команда |
|----|----|
| 00 | `stop` |
| 01 | `mov` |
| 10 | `turn right` |
| 11 | `turn left` |

Все зависит от нас. На последнюю систему команд можно смотреть не как на 4 команды, а как на команды 0 (движение) и 1 (поворот) с аргументами

| Код команды | Команда |
|----|----|
| **0**0 | `mov stop` |
| **0**1 | `mov forward` |
| **1**0 | `turn right` |
| **1**1 | `turn left` |


### 3-битная система команд

Если нам дадут трехбитный процессор и память, и добавят фары (сирену), то можно сделать более плавный поворот.

| Код команды | Команда |
|----|----|
| **000** | `stop` |
| **001** | `forward` |
| **01**`0` | **фары** `выключить` |
| **01**`1` | **фары** `включить` |
| **1**`00` | **turn** `right, 15 градусов` |
| **1**`01` | **turn** `right, 30 градусов` |
| **1**`10` | **turn** `left, 15 градусов`  |
| **1**`11` | **turn** `left, 30 градусов`  |

Команд 4: 000 (стоять), 001 (ехать вперед), 01 (фары) и 1 (поворот). Эти числа называют **код операции** или opcode.

У команд стоят и ехать вперед длина кода операции 3 бита, у команд фары 2 бита, а у поворота код операции размером 1 бит.

Оставшаяся часть закодированной команды - это **аргументы**. У стоять и ехать вперед аргументов нет, у *фары* один аргумент размером в 1 бит, а у поворота два аргумента - куда и на сколько поворачивать, каждый длиной 1 бит.

Если в автомобиле нет фар, то у нас машинные слова 010 и 011 останутся неиспользованные. Мы их зарезервируем для будущих команд.

Будем обозначать в описании команды D - аргумент длиной 1 бит, ХХ - один аргумент длиной 2 бита.

Тогда наша система команд:

* 000 - стоп
* 001 - вперед
* 01D - фары
* 1XX - поворот

Иногда кодом операции называют расшифровку вместе с аргументом, например, 01D.

## Исполнение программы на игрушечном автомобиле

Пусть в память автомобиля по адресам с 4 по 7 загружена программа 

```cpp
4: 001
5: 101
6: 001
7: 000
```
Как понять, какая команда выполняется сейчас и как перейти к выполнению следующей команды?

![pc](https://stepik.org/media/attachments/lesson/318782/pc.png)

Команды лежат одна за другой.

Пусть одна дополнительная ячейка памяти содержит **адрес команды**, которую мы прочтем, декодируем (поймем, что это за команда и с какими аргументами) и выполним. Эту ячейку памяти будем называть **program counter** (сокращенно **pc**).

**Как только читается слово по адресу из pc, его значение изменяется так, чтобы указывать на следующее слово (содержать адрес следующего слова)**.

pc содержит адрес 6, то есть сейчас прочитаем, разберем и выполним команду по адресу 6.

В ассемблере х86 такая дополнительная ячейка называется IP (instruction pointer).

Чтобы команды выполнялись одна за другой, нужно *в цикле*:

* прочитать команду по указанному адресу,
* изменить pc так, чтобы указывал на следующее слово,
* разобрать прочитанное слово на opcode и аргументы,
* выполнить команду.
    * цикл останавливается, если мы выполнили команду stop.

Напишем псевдокод для одного шага этого алгоритма:
```cpp
word w = w_read(pc);
pc += 1;      // чем отличается игрушечный автомобиль от PDP-11?
разбираем и выполняем команду
```

**В PDP-11 `pc` - это `R7` (последний регистр)**. 

## Декодирование команды и аргументов

Как понять, что это за команда. Возьмем таблицу кодировки команд и поймем, как мы декодируем команды.

**На этом шаге в псевдокоде все числа бинарные**.

Таблица кодировки команд:

* 000 - стоп
* 001 - вперед
* 01D - фары
* 1XX - поворот

Как определим, что слово `w = 011` это именно команда *фары*? Попробуйте придумать код, прежде чем читать дальше.

Со *стоп* и *вперед* очевидно:
```cpp
if (w == 000)
    значит команда стоп
else if (w == 001)
    значит команда вперед
```
* В команде с аргументом сделаем 0 все биты в части аргументов ("занулим аргументы"). 
* Если получим код операции команды, то мы нашли эту команду!

Какая маска зануляет последний бит в w чтобы мы потом сравнили с кодом операции 010? В этой маске должны быть 0 в тех битах, что нужно занулить и 1 в тех битах, что оставляем без изменений. Значит, маска 110.

```cpp
else if ( (w & 110) == 010)
    значит команда фары
else if ( (w & 100) == 100)
    значит команда поворот
```

## Итого

* Команда состоит из кода операции и быть может аргументов.
* Адрес команды содержится в pc (program counter), он же последний регистр R7.
* Команды считываются, разбираются и выполняются одна за другой.