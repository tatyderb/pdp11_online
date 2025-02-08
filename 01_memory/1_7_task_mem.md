# Проверка функций работы с памятью

lesson = 870973
lang = c

## TASKINLINE Проверка функций работы с памятью

**Эта задача работает только для языка Си. Поэтому цена этого шага 0 баллов.**

Проверим, что вы написали. Пошлите код, в котором:

* include файлов `stdio.h` и `assert.h`
* Определены типы `byte`, `word`, `address`;
* Объявлен массив для эмуляции памяти PDP-11, имя массива - какое вы хотите;
* Функции для работы с памятью (этим массивом):

```cpp
void b_write (address adr, byte val);
byte b_read (address adr);
void w_write (address adr, word val);
word w_read (address adr);
```
**Не надо** посылать функцию `main`.

CONFIG
score: 0
visible_tests: 0
FOOTER
#include <stdio.h>
#include <assert.h>

void b_write (address adr, byte val);
byte b_read (address adr);
void w_write (address adr, word val);
word w_read (address adr);

void test_mem_one(address a, word w, byte b1, byte b0)
{
    byte bres;
    word wres;
    
    // пишем байт, читаем байт
    fprintf(stderr, "Пишем и читаем байт по четному адресу\n");
    b_write(a, b0);
    bres = b_read(a);
    // тут полезно написать отладочную печать a, b0, bres
    fprintf(stderr, "a=%06o b0=%hhx bres=%hhx\n", a, b0, bres);
    assert(b0 == bres);
    
    // аналогично стоит проверить чтение и запись по нечетному адресу
    a ++;
    fprintf(stderr, "Пишем и читаем байт по НЕчетному адресу\n");
    b_write(a, b1);
    bres = b_read(a);
    // тут полезно написать отладочную печать a, b0, bres
    fprintf(stderr, "a=%06o b1=%hhx bres=%hhx\n", a, b1, bres);
    assert(b1 == bres);
       
    // пишем слово, читаем слово
    fprintf(stderr, "Пишем и читаем слово\n");
    a ++;        
    w_write(a, w);
    wres = w_read(a);
    // тут полезно написать отладочную печать a, w, wres
    fprintf(stderr, "a=%06o w=%04x wres=%04x\n", a, w, wres);
    assert(w == wres);
    
    // пишем 2 байта, читаем 1 слово
    fprintf(stderr, "Пишем 2 байта, читаем слово\n");
    a += 2;        // другой адрес
    // little-endian, младшие разряды по меньшему адресу
    b_write(a, b0);
    b_write(a+1, b1);
    wres = w_read(a);
    // тут полезно написать отладочную печать a, w, wres
    fprintf(stderr, "a=%06o b1=%02hhx b0=%02hhx wres=%04x\n", a, b1, b0, wres);
    assert(w == wres);
    
    // еще тесты
    // пишем 1 слово, читаем 2 байта
    fprintf(stderr, "Пишем 2 байта, читаем слово\n");
    a += 2;        // другой адрес
    // little-endian, младшие разряды по меньшему адресу
    w_write(a, w);
    byte bres0 = b_read(a);
    byte bres1 = b_read(a+1);
    wres = w_read(a);
    // тут полезно написать отладочную печать a, w, wres
    fprintf(stderr, "a=%06o bres1=%02hhx bres0=%02hhx wres=%04x\n", a, bres1, bres0, wres);
    assert(b0 == bres0);
    assert(b1 == bres1);
    
}
void test_mem()
{
    test_mem_one(0100, 0x0201, 0x02, 0x01);
    test_mem_one(0120, 0x02a1, 0x02, 0xa1);
    test_mem_one(0140, 0xb201, 0xb2, 0x01);
    test_mem_one(0160, 0xb2a1, 0xb2, 0xa1);
    
    test_mem_one(0200, 0x1234, 0x12, 0x34);
    test_mem_one(0220, 0x6431, 0x64, 0x31);
    test_mem_one(0240, 0xabcd, 0xab, 0xcd);

}
int main()
{
    printf("dummy\n");
    test_mem();
    return 0;
}
TEST
dummy
----
dummy
====

## Сохранить в репозиторий

Поздравляем! Вы сделали первые, но очень важные шаги.

Не забудьте!

* git add файлы
* git commit -m "функции работы с памятью"
* git push

Студенты МФТИ посылают ссылки на свой репозиторий преподавателю.

Все остальные могут просить ревью кода на [канале в дискорде](https://discord.gg/rEcqNB3) 
