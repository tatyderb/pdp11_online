# JSR и RTS

lesson = 320752

## SKIP VIDEO Видео

https://youtu.be/yVaDx1Taxks

## Подпрограммы

Давайте подумаем, как организовать переход к подпрограмме (функция без аргументов и возвращаемого значения) и возврат из него.

Пусть у нас есть код, который умеет печатать строку. Он начинается с адреса, на котором установлена метка `PRN`. Я хочу напечатать две строки, "Hello!" и "Good bye!" То есть этот код нужно два раза вызвать и два раза вернуться в основную программу, откуда был вызов.

Пусть наша подпрограмма (печати) начинается с метки `PRN`. Тогда нужно перейти по этому адресу командой `jmp PRN` (или командой `br`). Вопрос - как вернуться (тоже `jmp` по какому-то адресу) на то место, откуда вызывали подпрограмму? 

Одну и ту же подпрограмму `PRN` можно вызывать из разных мест. Тогда адреса возврата будут тоже разные. На рисунке в одном случае это адрес команды *next cmd1*, а в другом вызове нужно вернуться по адресу, где лежит команда *next cmd2*. В месте, где мы должны вернуться, мы не знаем куда именно переходить.

![вызов подпрограммы из двух разных мест](https://stepik.org/media/attachments/lesson/320750/return.png)

Можно хранить адрес возврата в каком-то месте, например, положить в регистр `R5`. Тогда `jmp (R5)` вернет нас в точку вызова.

Но есть проблемы.

1. Хранить нужно не адрес команды `jmp PRN`, а адрес следующей команды. Иначе мы будем в бесконечном цикле переходить на метку `PRN` (и что-то печатать).

2. Хотим передать аргументы при вызове.

3. Хотим получить возвращаемое значение. 

4. В языке Си у меня есть функция печати строки `puts` и функция печати символа `putchar`. В главной программе вызывается `puts`, которая вызывает `putchar`. После печати последнего символа нужно не только из `putchar` вернуться в `puts`, но и потом вернуться из `puts` в главную программу. А в регистре может быть записан только один адрес возврата.

Давайте все эти проблемы решать по одной. 

## Коррекция адреса возврата

Функция без аргументов. Переходим по адресу подпрограммы известными нам командами. Сохраняем адрес возврата в регистре `R5`. Посмотрим на листинг кода записи адреса возврата и перехода по адресу подпрограммы по метке `PRINT`:

```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: jmp PRINT    ; переход по метке PRINT (2 слова!)   
024620 021622:
024622 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 010507: mov R5, PC   ; вернулись по сохраненному адресу
```
Посмотрите, следующая команда, которую нужно выполнить после выполнения подпрограммы, начинается с адреса 0246**22**, а в момент, когда мы выполняем `mov PC, R5`, у нас `PC` указывает на следующее слово, 0246**16**. Именно по этому адресу мы вернемся (и опять пойдем выполнять `jmp PRINT`, получится бесконечный цикл).

Просто "запомнить PC в R5" не помогло. Программа закцилилась. Попробуем откорректировать записанное в `R5` значение так, чтобы при возврате перейти по нужному адресу.

## Вариант 1. Правка адреса возврата перед возвратом

Если надо вернуться в 0246**22**, а в `R5` записано 0246**16**, давайте прибавим 4 к `R5` (мы же помним, числа восьмеричные). И вернемся по верному адресу.

```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: jmp PRINT    ; переход по метке PRINT (2 слова!)   
024620 021622:
024622 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 062707: add #4, R5   ; <---- добавили коррекцию к адресу возврата
046554 000004: 
046556 010507: mov R5, PC   ; вернулись по сохраненному адресу
```
Работает? Иногда. При возврате из `PRINT` мы не знаем, как нас вызывали.

Могли вызвать через **jmp** и тогда поправка должна быть 024622 - 024616 = 4:
```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: jmp PRINT    ; переход по метке PRINT (2 слова!)   
024620 021622:
024622 ...     следующая команда, сюда надо вернуться
```
Могли вызвать через **br** и тогда поправка должна быть 024620 - 024616 = 2:
```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: br PRINT     ; переход по метке PRINT (1 слово!)   
024620 ...     следующая команда, сюда надо вернуться
```
В месте возврата из подпрограммы мы ничего не знаем о том, как нас вызывали. То есть такой подход не работает.

Попробуем вносить поправку перед вызовом `jmp` или `br`.

## Вариант 2. Правка адреса возврата перед вызовом

Давайте добавим `add` перед `jmp` или `br`.

Но сама команда `add #число, R5` занимает 2 слова и адрес возврата теперь должен быть не 024622, а на 2 слова дальше, 024626.
```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: add #???, R5 ; коррекция адреса возврата, 2 слова   
024620 ??????:
024622 000167: jmp PRINT    ; переход по метке PRINT (2 слова!)   
024624 021622:
024626 ...     следующая команда, сюда надо вернуться
```
То есть поправка должна быть 024626 - 024616 = 10. Запишем код полностью:
```
024614 010705: mov PC, R5   ; записали адрес возврата в R5
024616 000167: add #10, R5  ; <---- коррекция адреса возврата, 2 слова   
024620 000010:
024622 000167: jmp PRINT    ; переход по метке PRINT (2 слова!)   
024624 021622:
024626 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 010507: mov R5, PC   ; вернулись по сохраненному адресу
```
Возврат простой, но при вызове функции устанешь считать поправки, одни для `jmp`, другие для семейства `br`. Хочется, чтобы посчитали за нас. Давайте переход по метке с сохранением правильного адреса возврата будет делать **одна** команда ассемблера, а не мы (с кучей ошибок) будем высчитывать поправки на пальцах.

Потому что вызов подпрограмм - это основа программирования и его нужно уметь делать легко и просто. Или я пишу и отлаживаю простые функции, или большая программа в виде монолитного кода на ассемблере у меня никогда не заработает.

## Минусы хранения адреса возврата в регистре

1. Регистров мало. Они нужны для быстрых вычислений, а не для хранения адресов возврата.

2. Очень сложно из главной программы вызвать подпрограмму `puts`, а из нее `putchar` и при этом не потерять адрес возврата в главную подпрограмму. Пока мы переходим из `puts` в `putchar`, адрес возврата в `main` надо сохранить где-то в сторонке, а то мы его затрем, если будем хранить в том же регистре `R5`.

![вызов подпрограммы из двух разных мест](https://stepik.org/media/attachments/lesson/320750/return.png)

3. Рекурсивный вызов функций невозможен. Нам негде хранить старые адреса возврата.

4. Значение `R5`, которое мы затерли, могло содержать нужные данные. Мы их потеряли.

Что делать? У нас же есть аппаратный стек по `SP`! Давайте сохранять "затираемые" значения в стек, и после возврата восстанавливать их из стека.

## Вариант 3. Храним адрес возврата в стеке

Добавились команды сохранения старого значения `R5` в стек и восстановления его из стека после возврата из подпрограммы.

```
024612 010546: mov R5, -(SP) ; <--- старое значение регистра сохранили в стек
024614 010746: mov PC, R5    ; адрес возврата сохранили в регистре
024616 000167: add #10, R5   ; коррекция адреса возврата, 2 слова   
024620 000010:
024622 000167: jmp PRINT     ; переход по метке PRINT (2 слова!)   
024624 021622:
024626 012605: mov (SP)+, R5 ; <--- восстановили старое значение R5 из стека
024630 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 010507: mov R5, PC   ; вернулись по сохраненному адресу
```
То есть переход по адресу:
```
024612 010546: mov R5, -(SP) ; <--- старое значение регистра сохранили в стек
024614 010746: mov PC, R5    ; адрес возврата сохранили в регистре
024616 000167: add #10, R5   ; коррекция адреса возврата, 2 слова   
024620 000010:
024622 000167: jmp PRINT     ; переход по метке PRINT (2 слова!)   
024624 021622:
```
а эта часть относится к возврату из подпрограммы:
```
024626 012605: mov (SP)+, R5 ; <--- восстановили старое значение R5 из стека
024630 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 010507: mov R5, PC   ; вернулись по сохраненному адресу
```
Все это заменено на две команды.

* **JSR** (**J**ump to **S**ub**R**outine - переход к подпрограмме)
* **RTS** (**R**e**T**urn from **S**ubroutine - возврат из подпрограммы).

| Мнемоника    | Opcode   | NZVC   | Описание                  | Комментарии        | 
|----|----|----|----|----|
| JSR  r,d    | 004RDD   | `----`   | Jump to Subroutine           | r=PC,PC=d    | 
| RTS  r      | 00020R   | `----`   | Return from Subroutine       | PC=r,r=(SP)+ | 

То есть весь этот код записывается как:

```
024614 004567: jsr R5, PRINT ; команда занимает 2 слова, ибо адрес перехода DD
024616 021624: 
024620 ...     следующая команда, сюда надо вернуться
...
046444        PRINT:        ; начало подпрограммы
                ...
046552 000205: rts R5       ; вернулись по сохраненному адресу
```
### Что делает команда JSR Rn, adr

Неявно использует регистр `SP`.

* Сохраняет старое значение регистра **Rn** в стек.
* Адрес возврата записывает в регистр **Rn**.
* Переходит по указанному в **adr** адресу.

### Что делает команда RTS Rn

Неявно использует регистр `SP`. Действия **JSR** разворачиваются в обратном порядке.

* Переходит по указанному в **Rn** адресу (возврата).
* Восстанавливает старое значение регистра **Rn** из стека.

**Номера регистров в JSR и RTS для пары вызов-возврат должны совпадать.**

## Подпрограмма SUBA вызывает подпрограмму SUBB

Пусть в коде есть подпрограммы `SUBA` и `SUBB`. Из главной программы вызываем `SUBA`, из `SUBA` вызываем `SUBB`. В обратном порядке возвращаемся из подпрограмм.

Напишем в комментариях что лежит в регистре R2 и что находится в стеке по регистру `R6` в формате `[дно_стека, еще_значение, ..., верхушка_стека]`. Изначально считаем, что стек пуст `[]`.

```cpp
024612 012705: mov #12, R2      ; R2=12,     stack:[]
024614 004267: jsr R2, SUBA     ; R2=024620, stack:[12]
024616 ??????: 
024620 ...     следующая команда, сюда надо вернуться
...
              ;;;;;;;;;;;;  начало подпрограммы SUBA
046444        SUBA:        
                ...            ; R2=024620, stack:[12]
046450 004267: jsr R2, SUBB    ; R2=046454, stack:[12, 024620]
046452 ??????: 
046454 ...     следующая команда, сюда надо вернуться из SUBB
                ...
                ...            ; R2=024620, stack:[12]
046464 000202: rts R2          ; R2=12,     stack:[], возврат по адресу 024620
              ;;;;;;;;;;;;  конец подпрограммы SUBA
              
              ;;;;;;;;;;;;  начало подпрограммы SUBB
046466        SUBB:        
                ...            ; R2=046454, stack:[12, 024620]
046572 000202 rts R2           ; R2=024620, stack:[12], возврат по адресу 046454
              ;;;;;;;;;;;;  конец подпрограммы SUBB
```
Рекурсивный вызов подпрограмм так же работает на едином регистре. Адреса возврата закладываются в стек по мере вызова рекурсивных функций и достаются из стека для рекурсивного возврата.

## JSR и RTS по R7

Иногда в качестве регистра для команд `jsr` и `rts` используют `PC`. В этом случае.

* **JSR PC, adr**:
    * Сохраняет старое значение регистра **PC** в стек. Это адрес возврата. Он хранится теперь в стеке.
    * Адрес возврата записывает в регистр **PC**. То есть `PC=PC`, холостой ход, значение `PC` не изменяется.
    * Переходит по указанному в **adr** адресу. `PC=adr`.
* **RTS PC**
    * Переходит по указанному в **PC** адресу (возврата). То есть `PC=PC`.
    * Восстанавливает старое значение регистра **PC** из стека. Следующая команда разбирается по "восстановленному" адресу. Так происходит возврат по адресу, который хранился в стеке.

