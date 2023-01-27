# Постановка задачи урока - определение текущей команды

lesson = 318781

## SKIP VIDEO Цель урока

https://youtu.be/fLK5npAnUSY

## Цель урока

Мы уже умеем загружать исполняемый файл в наш эмулятор. На этом уроке добавим трассировку, которая будет приближаться к формату файла листинга и трассировке эталонного эмулятора.

```
001000: mov     #000002,r0              [001002]=000002 
001004: mov     #000003,r1              [001006]=000003 
001010: add     r0,r1                   R0=000002 R1=000005 
001012: halt                            

---------------- halted ---------------
r0=000002 r2=000000 r4=000000 sp=000000
r1=000005 r3=000000 r5=000000 pc=001014
psw=000000: cm=k pm=k pri=0        [4]
```
На следующих шагах мы начнем печатать похожее на эту трассировку. **Последний столбец трассировки не нужно реализовывать**. В нем пишется по каким адресам и как изменяются данные текущей командой.

Сегодня будем печатать:

* адрес слова,
* само слово,
* если это слово - команда `add`, `mov` или `halt`, то название этой команды, иначе 'unknown' (откуда взялись эти unknown, расскажем в следующем модуле).

Для первого теста должно получиться так:

```
001000 012700: mov
001002 000002: unknown
001004 012701: mov
001006 000003: unknown
001010 060001: add
001002 000000: halt
```
Попробуйте сами ответить на вопросы:

* почему адреса только четные?
* почему после 6 идет 10?

## Ответы на вопросы

* почему адреса только четные?
    * потому что машинный код организован в виде машинных слов.
* почему после 6 идет 10?
    * потому что все числа восьмеричные 6 + 2 = 10
* почему 012700 это mov?
    * об этом расскажем в следующем уроке.