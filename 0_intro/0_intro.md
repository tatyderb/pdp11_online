# Презентации, видео, книги

lesson = 347539

## Список команд PDP-11

Под таблицей - описание сокращений и условных обозначений.

**Все числа восьмеричные**.

| Мнемоника    | Opcode   | NZVC   | Описание                  | Комментарии        | 
|----|----|----|----|----|
| ADCb d      | B055DD   | `****`   | Add Carry                    | d=d+C        | 
| ADD  s,d    | 06SSDD   | `****`| Add                          | d=s+d         | 
| ASH  s,r    | 072RSS   | `****`| Arithmetic Shift             | r=r*2^s(EIS)# | 
| ASHC s,r    | 073RSS   | `****`| Arithmetic Shift Combined    |        (EIS)# | 
| ASLb d      | B063DD   | `****`| Arithmetic Shift Left        | d=d*2         | 
| ASRb d      | B062DD   | `****`| Arithmetic Shift Right       | d=d/2        | 
| BCC  a      | 1030XX   | `----`   | Branch if Carry Clear        | If C=0       | 
| BCS  a      | 1034XX   | `----`   | Branch if Carry Set          | If C=1       | 
| BEQ  a      | 0014XX   | `----`   | Branch if Equal              | If Z=1       | 
| BGE  a      | 0020XX   | `----`   | Branch if Greater or Equal   | If NxV=0     | 
| BGT  a      | 0030XX   | `----`   | Branch if Greater Than       | If Zv{NxV}=0 | 
| BICb s,d    | B4SSDD   | `**0-`   | Bit Clear                    | d=d&{~s}     | 
| BISb s,d    | B5SSDD   | `**0-`   | Bit Set (OR)                 | d=dvs        | 
| BITb s,d    | B3SSDD   | `**0-`   | Bit Test (AND)               | d&s          | 
| BHI  a      | 1010XX   | `----`   | Branch if Higher             | If CvZ=0     | 
| BHIS a      | 1030XX   | `----`   | Branch if Higher or Same     | If C=0       | 
| BLE  a      | 0034XX   | `----`   | Branch if Less or Equal      | If Zv{NxV}=1 | 
| BLT  a      | 0024XX   | `----`   | Branch if Less Than          | If NxV=1     | 
| BLO  a      | 1034XX   | `----`   | Branch if Lower              | If C=1       | 
| BLOS a      | 1014XX   | `----`   | Branch if Lower or Same      | If CvZ=1     | 
| BMI  a      | 1004XX   | `----`   | Branch if Minus              | If N=1       | 
| BNE  a      | 0010XX   | `----`   | Branch if Not Equal          | If Z=0       | 
| BPL  a      | 1000XX   | `----`   | Branch if Plus               | If N=0       | 
| BR   a      | 0004XX   | `----`   | Branch                       | PC=PC+2*XX   | 
| BVC  a      | 1020XX   | `----`   | Branch if Overflow Clear     | If V=0       | 
| BVS  a      | 1024XX   | `----`   | Branch if Overflow Set       | If V=1       | 
| CALL d      | 0047DD   | `----`   | Call subroutine              |  (= JSR PC,d)| 
| CCC         | 000257   | `0000`   | Clear all Condition Codes    | {C,N,V,Z}=0  | 
| CLC         | 000241   | `---0`   | Clear Carry                  | C=0          | 
| CLN         | 000250   | `0---`   | Clear Negative               | N=0          | 
| CLRb d      | B050DD   | `0100`   | Clear                        | d=0          | 
| CLV         | 000242   | `--0-`   | Clear Overflow               | V=0          | 
| CLZ         | 000244   | `-0--`   | Clear Zero                   | Z=0          | 
| CMPb s,d    | B2SSDD   | `****`| Compare                      | s-d          | 
| COMb d      | B051DD   | `**01`   | Complement                   | d=~d         | 
| DECb d      | B053DD   | `***-`   | Decrement                    | d=d-1        | 
| HALT        | 000000   | `----`   | Halt                         |              | 
| INCb d      | B052DD   | `***-`   | Increment                    | d=d+1        | 
| JMP  d      | 0001DD   | `----`   | Jump                         | PC=d         | 
| JSR  r,d    | 004RDD   | `----`   | Jump to Subroutine           | r=PC,PC=d    | 
| MOVb s,d    | B1SSDD   | `**0-`   | Move                         | d=s          | 
| NEGb d      | B054DD   | `****`| Negate                       | d=-d         | 
| NOP         | 000240   | `----`   | No Operation                 |              | 
| RESET       | 000005   | `----`   | Reset external bus           |              | 
| RETURN      | 000207   | `----`   | Return from subroutine       |    (= RTS PC)| 
| ROLb d      | B061DD   | `****`| Rotate Left                  | d={C,d}<-    | 
| RORb d      | B060DD   | `****`| Rotate Right                 | d=->{C,d}    | 
| RTS  r      | 00020R   | `----`   | Return from Subroutine       | PC=r,r=(SP)+ | 
| SBCb d      | B056DD   | `****`| Subtract Carry               | d=d-C        | 
| SCC         | 000277   | `1111`   | Set all Condition Codes      | {C,N,V,Z}=0  | 
| SEC         | 000261   | `---1`   | Set Carry                    | C=1          | 
| SEN         | 000270   | `1---`   | Set Negative                 | N=1          | 
| SEV         | 000262   | `--1-`   | Set Overflow                 | V=1          | 
| SEZ         | 000264   | `-1--`   | Set Zero                     | Z=1          | 
| SOB  r,a    | 077RNN   | `----`   | Subtract One and Branch      | PC=PC-2*NN  #| 
| SUB  s,d    | 16SSDD   | `****`| Subtract                     | d=d-s        | 
| SWAB d      | 0003DD   | `**00`   | Swap Bytes                   |              | 
| SXT  d      | 0067DD   | `-*0-`   | Sign Extend                  | d=0 or -1   #| 
| TSTb d      | B057DD   | `**00`   | Test                         | d            | 
| XOR  r,d    | 074RDD   | `**0-`   | Exclusive OR                 | d=dxr       #| 

### Условные обозначения колонки opcode

| Обозначение | Что значит |
|----|----|
| B	| 0 for word, 1 for byte (1 bit) |
| DD |	Destination field (6 bits) |
| N		| Number (3 bits)	|
| NN	|	Number (6 bits)	|
| R	|	Register (3 bits, R0-5/SP/PC)	|
| SS	|	Source field (6 bits)	|
| TT	|	Number (8 bits)	|
| XX	|	Offset (8 bits, -128 to +127)	|

### Обозначения в мнемониках и комментариях

| Обозначение | Что значит |
|----|--------|
| Rn	| General purpose Register (16-bit, n=0-5) | 
| SP	| Stack Pointer (16-bit, R6)               | 
| PC	| Program Counter (16-bit, R7)             | 
| PS	| Processor Status (16-bit)                | 

| Описания | Что значит |
|----|--------|
| a	| Relative address | 
| b	| Blank or B for word or byte operand(s) |
| d s	| Destination/source |
| n	| Register number (0 to 5) |
| nn	| 16-bit expression (0 to 65535) |
| r	| Register (Rn,SP,PC) |
| t	| Trap number (0 to 255) |
| `+` `-` `*` `/` `^`	| Add/subtract/multiply/divide/power |
| `&` `~` `v` `x`	| Logical AND/NOT, inclusive/exclusive OR |
| `<-` `->` | 	Rotate left/right |
| `{ }` `< : >`	| Combination of operands/bit range |
| `#` |	Not applicable to all PDP-11s |


## Моды адресации в аргументах SS и DD

| Мода | Обозначение | Пример | Описание |
|----|----|-----|----------|
| 0 | `R3` | `inc R3` | Значение в регистре `R3` |
| 1 | `(R3)` | `inc (R3)` | Адрес в регистре `R3` |
| 2 | `(R3)+` | `inc (R3)+` | Адрес в регистре `R3`, `R3+=2` |
| 3 | `@(R3)+` | `inc @(R3)+` | Адрес в регистре `R3` содержит адрес, `R3+=2` |
| 4 | `-(R3)` | `inc -(R3)` | `R3-=2`, Адрес в регистре `R3`  |
| 5 | `@-(R3)` | `inc -(R3)` | `R3-=2`, Адрес в регистре `R3` содержит адрес |
| 6 | `2(R3)` | `inc 2(R3)` | Сложить 2 и `R3`, это адрес |
| 7 | `@2(R3)` | `inc @2(R3)` | Сложить 2 и `R3`, по этому адресу лежит адрес |

При работе по 7 регистру, обозначения в коде ассемблера другие:

| Мода | Обозначение | Пример | Описание |
|----|----|-----|----------|
| 2 | `#3` | `mov #3, R0` | Константа 3 |
| 3 | `@#100` | `mov @#100, R0` | Значение по адресу 100 |
| 6 | `100` | `mov 100, R0` | Значение по адресу 100 |
| 7 | `@100` | `mov @100, R0` | По адресу 100 лежит адрес, по этому адресу лежит значение |


## Видео всего курса

Плейлист со всеми видео на youtube.com: [Язык С для начинающих. Курсовая работа](https://www.youtube.com/watch?v=bf9TX2DdUoM&list=PL55ElYIEKI0ApTzK4GdmVIw5uJEYTxJx5)

Каждая тема представлена одним общим видео и тем же материалом, разбитым на маленькие фрагменты.

## Презентации

* [PDP-11: память и загрузка программы](https://stepik.org/media/attachments/lesson/347539/pdp_1_bread_169.pdf), [зеркало]http://judge.mipt.ru/conline/pdf/pdp_1_bread_169.pdf)
* [Набор команд ассемблера](https://stepik.org/media/attachments/lesson/347539/pdp_2_cmd_169.pdf), [зеркало](http://judge.mipt.ru/conline/pdf/pdp_2_cmd_169.pdf)
* [Аргументы команды](https://stepik.org/media/attachments/lesson/347539/pdp_3_mode_169.pdf), [зеркало](http://judge.mipt.ru/conline/pdf/pdp_3_mode_169.pdf)
* [Условные команды](https://stepik.org/media/attachments/lesson/347539/pdp_4_psw_169.pdf), [зеркало](http://judge.mipt.ru/conline/pdf/pdp_3_mode_169.pdf)
* [Hello, world](https://stepik.org/media/attachments/lesson/347539/pdp_5_putchar_169.pdf), [зеркало](http://judge.mipt.ru/conline/pdf/pdp_5_putchar_169.pdf)
* [Подпрограммы](https://stepik.org/media/attachments/lesson/347539/pdp_6_func_169.pdf), [зеркало](http://judge.mipt.ru/conline/pdf/pdp_6_func_169.pdf)

## SKIP Тесты и книги

### Тесты

* Интеграционные тесты: [https://github.com/tatyderb/pdp11_tests](https://github.com/tatyderb/pdp11_tests)

### Книги

* [Френк Т.С. PDP-11 архитектура и программирование](https://stepik.org/media/attachments/lesson/347539/T.S._Frenk._PDP-11_arkhitektura_i_programmirovanie.djvu) - полезно приложение в конце книги, где кратко описаны все команды ассемблера
    * [Френк Т.С. PDP-11 архитектура и программирование](http://acm.mipt.ru/twiki/pub/Cintro/PDPIntro/T.S._Frenk._PDP-11_arkhitektura_i_programmirovanie.djvu) - зеркало на acm.mipt.ru
* [Сингер М. МИНИ-ЭВМ PDP-11: ПРОГРАММИРОВАНИЕ НА ЯЗЫКЕ АССЕМБЛЕРА И ОРГАНИЗАЦИЯ МАШИНЫ](https://stepik.org/media/attachments/lesson/347539/PDP-11.pdf)
    * [Сингер М. МИНИ-ЭВМ PDP-11: ПРОГРАММИРОВАНИЕ НА ЯЗЫКЕ АССЕМБЛЕРА И ОРГАНИЗАЦИЯ МАШИНЫ](http://acm.mipt.ru/twiki/pub/Cintro/PDPIntro/PDP-11.pdf) - зеркало на acm.mipt.ru
    
### Статьи и материалы

* [Описание полной системы команд](http://pdp-11.ru/mybk/doc/PDP11.TXT) на русском языке
* Статья [Чему нас научил PDP-11](https://habr.com/ru/post/435292/)

## Эталонный эмулятор

* [Linux](https://stepik.org/media/attachments/lesson/347539/pdp11em-linux.zip)
* [Windows](https://stepik.org/media/attachments/lesson/347539/pdp11em-win32.rar) - 32 битный
* [Mac](https://stepik.org/media/attachments/lesson/347539/pdp11em-win32.rar)

## Старые материалы

Этот шаг будет убран после полного написания курса с текстом и вопросами.

* [PDPIntro](http://acm.mipt.ru/twiki/bin/view/Cintro/PDPIntro) - введение в архитектуру
* [PDPrun](http://acm.mipt.ru/twiki/bin/view/Cintro/PDPrun) - как установить и запустить эталонный эмулятор
* [PDPCommandList](http://acm.mipt.ru/twiki/bin/view/Cintro/PDPCommandList) - список команд (надо сделать в виде pdf файла и оставить в курсе)
* [PDPmode](http://acm.mipt.ru/twiki/bin/view/Cintro/PDPmode) - режимы адресации
* [PDPtest](http://acm.mipt.ru/twiki/bin/view/Cintro/PDPtest) - примеры тестов для пишущих эмулятор

## Понедельный план работы, рекомендуемый студентам

1. Модуль 2: создание репозитория, функции работы с памятью w_read, w_write, b_read, b_write.
1. Модуль 2: загрузка данных, указать имя файла в аргументах командной строки. Опционально - функция логирования trace.
1. Модуль 3. Проход по программе, печать адреса, слова и имени команды (add, mov, halt и unknown, если неизвестная команда).
1. Модуль 4. Аргументы команды SS и DD. (Возможно, 2 недели)
    * Работа первого интеграционного теста.
1. Модуль 5. Команда sob для организации цикла со счетчиком. Байтовые команды.
1. Модуль 6. Флаги N, Z, V, C. Условные команды. Реализовывать только те, что встречаются в интеграционных тестах!
1. Модуль 7. Вывод на экран. Печать Hello, world!
1. Модуль 8. Реализация команд jsr и rts.