# Терминологиня

lesson = 312212

## SKIP VIDEO 

Терминология 9:23.
https://www.youtube.com/watch?v=eBXfExL_D70

## Биты и байты

* **Бит** (bit) - наименьшая единица информации. 0 или 1.

* **Байт** (byte) - наименьшая *адресуемая* единица информации в конкретной архитектуре. То есть байт - это ячейка памяти, у которой есть адрес.

* **Октет** - 8 бит.

Не путайте *байт* и *октет*. В истории ЭВМ были модели, где байт был 4, 6, 7 бит, а в моделях цифровой телефонии он был больше 40 бит. Очень часто сейчас используют байт в смысле "8 бит", потому что именно такие архитектурные решения сейчас наиболее распространены.

* **Машинное слово** - это размер памяти, которое обрабатывается как единое целое в данной архитектуре, то есть которым удобнее всего оперировать в данной архитектуре. Оно связано с количеством бит в процессоре (арифметико-логическом устройстве, в нем выполняются машинные команды).

Машинное слово может быть равно 1 байту, а может быть больше.

Однобайтное машинное слово, размер слова совпадает с байтом, адрес слова и адрес байта так же одинаковые:

![1 байтное машинное слово](https://stepik.org/media/attachments/lesson/312212/1byte.png)

Двухбайтное машинное слово, 1 слово = 2 байтам, какие адреса у слов и как они связаны с адресами байт?

![2 байтное машинное слово](https://stepik.org/media/attachments/lesson/312212/little_end.png)

**В PDP-11 байт состоит из 8 бит, а машинное слово из 16 бит, то есть 2 байт.** В современных компьютерах машинное слово обычно 4 или 8 байт.

## Адресация

В числе 231675 говорят, что цифра 2 лежит в **старших разрядах** (битах), а цифра 5 в **младших разрядах** (битах).

Пусть int занимает 4 байта (32 бита). У каждого байта есть адрес, пусть это адреса a, a+1, a+2, a+3. Где лежат младшие и старшие разряды числа 0x0A0B0C0D?

Можно положить старшие разряды, байт с 0A, по меньшему адресу `a`, а младший байт c 0D по адресу `a+3`. 

Можно наоборот, старший байт 0A положить в больший адрес `a+3`, а младший 0D в меньший адрес `a`.

![4byte.png](https://stepik.org/media/attachments/lesson/312212/4byte.png)

Как лучше? Жаркие споры об этом в свое время напоминали споры лилипутов из книги Свифта "Гулливер в стране лилипутов". Они начали войну из-за того, с какой стороны правильно разбивать яйцо - с острой (little end, поэтому они назывались little-endian) или с тупой (big end, поэтому они назывались big-endian). 

Архитектура, в которой **старший разряд имеет наименьший адрес** назвали **big-endian** или **от старшего к младшему**. 

![bigendian](https://stepik.org/media/attachments/lesson/312212/big_end.png)

Пример big-endian: сетевые протоколы передачи данных TPC/IP (поэтому его иногда называют *сетевой порядок данных), компьютеры SPARC, форматы файлов PNG, JPEG.

Архитектура, в которой **младший разряд имеет наименьший адрес** назвали **little-endian** или **от младшего к старшему**.

![littleendian](https://stepik.org/media/attachments/lesson/312212/little_end.png)

Примеры little-endian: PDP-11, процессоры семейства х86.

Кроме того, существуют архитектуры с встроенным или программным переключением little-endian/big-endian. Такие архитектуры называют bi-endian.

Если в слове 4 байта или больше, последовательность байт можно расположить разным образом, не обязательно по порядку. В **middle-endian** байты числа 0x0A0B0C0D расположены в памяти как 0B, 0A, 0D, 0C.

![4byte.png](https://stepik.org/media/attachments/lesson/312212/4byte.png)

## Адресация слова

Размер памяти PDP-11 64 килобайта. То есть байты имеют адреса от 0 до $ 64 *2^10 - 1 $ включительно. 
*Не обязательно адрес - это натуральное число. В некоторых архитектурах использовалась нумерация отрицательными и положительными числами.*

Какие адреса у слов? Слово в PDP-11 имеет тот же адрес, что его младшее полуслово (байт). То есть 0, 2, 4, 6 и так далее. В этом случае говорят, что слова и байты имеют **единое адресное пространство** (сквозной порядок нумерации).

Альтернатива (бывает в других архитектурах), когда у слов свои адреса, у байт свои. Байты имеют адреса 0, 1, 2, 3 и так далее. Слова тоже имеют адреса 0, 1, 2, 3 и так далее. Но слово с адресом 2 состоит из байт 4 и 5. 

![littleendian](https://stepik.org/media/attachments/lesson/312212/little_end.png)

В языке Си ближайшие аналогии адреса байта и адреса слова - это указатель на `char` и указатель на `int`.

Пусть слово со значением 7 лежит по адресу 100. Если адресное пространство единое, то:
```cpp
int x = 7;          // число 7 лежит по адресу 100
int * pi = &x;      // pi = 100 - адрес int 100
void * p = pi;      // p = 100  
char * pc = p;      // pc = 100 - адрес байта 100
char y = *pc;       // 7 
```
`y` равно 7, так как адрес слова и байта совпадают и число 7 укладывается в 1 байт.

Если бы `int` занимал 1 машинное слово, равное 2 байтам и нумерация слов и байт была у каждого своя, то машинному слову по адресу 100 соответствовали бы байты с адресами 200 и 201. Но так как мы сначала присваиваем в переменную типа `void *`, то эта информация теряется и в `p` лежит число 100. То есть в `pc` тоже число 100 и мы читаем число из *байта* с адресом 100, которое лежит совсем в другом месте в памяти. Скорее всего оно не равно 7.

Современные архитектуры обычно имеют общее адресное пространство для байт и слов.

## SKIP Match термины

Поставьте в соответствие:

* бит (bit) - 0 или 1
* байт (byte) - наименьшая адресуемая ячейка памяти, обычно 8 бит.
* слово (word) - основная компонента работы с памятью, современные архитектуры имеют размер 32, 64 или 128 бит.

## NUMBER 1 byte

Какой размер 1 байта в битах в PDP-11?

ANSWER: 8

## NUMBER 1 word

Какой размер 1 слова в битах в PDP-11?

ANSWER: 16

## QUIZ Порядок байт

Какой порядок байт в PDP-11?

A. little-endian
B. big-endian
C. другой

SHUFFLE: False
ANSWER: A

## QUIZ Адресация

Отметьте верные утверждения.

A. Байты и слова в PDP-11 имеют единое адресное пространство.
B. Адрес слова всегда четное число.
C. Адрес слова всегда нечетное число.
D. Адрес слова может быть как четным, так и нечетным.
E. Адрес байта всегда четное число.
F. Адрес байта всегда нечетное число.
G. Адрес байта может быть как четным, так и нечетным.

SHUFFLE: False
ANSWER: A, B, G

## Итого

* 1 бит (bit) - это 0 или 1.
* 1 байт (byte) - 8 бит в PDP-11.
* 1 машинное слово (word) - 16 бит в PDP-11.
* В PDP-11 порядок байт little-endian.
* Слова и байты имеют общее адресное пространство,
    * адрес байта может быть любым, 0, 1, 2, 3, 4 и так далее;
    * адрес слова только четный, 0, 2, 4, 6 и так далее.