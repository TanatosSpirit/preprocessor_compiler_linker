# preprocessor_compiler_linker

Конспект к [этому](https://youtu.be/Y6U9662gaa8) видео.
## Собираем

$ gcc -o program main.c sum.c  
$ ./program; echo $?

Вариант 2

$ gcc -c main.c  
$ gcc -c sum.c 
$ ls 
main.c main.o sum.c  sum.o

$ gcc -o program main.o sum.o  
$ ./program; echo $?

## Символы и релокации

Содержимое объектных файлов:  

$ objdump -d -r sum.o
$ objdump -d -r main.o  

Список символов:  

$ readelf -s main.o

## Секции

Список секций  

$ readelf -t main.o 

Компоновщик оперирует секциями. (9.33)

## Исполняемый файл

Содержимое исполняемого файлы:  

$ objdump -d -r program

Заголовок объектных файлов:   

`$ readelf -h main.o`  
`$ readelf -h sum.o`

К примеру,   

`$ readelf -h main.o`

    ELF Header:
    Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
    Class:                             ELF64
    Data:                              2's complement, little endian
    Version:                           1 (current)
    OS/ABI:                            UNIX - System V
    ABI Version:                       0
    Type:                              REL (Relocatable file)
    Machine:                           Advanced Micro Devices X86-64
    Version:                           0x1
    Entry point address:               0x0
    Start of program headers:          0 (bytes into file)
    Start of section headers:          616 (bytes into file)
    Flags:                             0x0
    Size of this header:               64 (bytes)
    Size of program headers:           0 (bytes)
    Number of program headers:         0
    Size of section headers:           64 (bytes)
    Number of section headers:         12
    Section header string table index: 9  

,где  

    Entry point address:               0x0

точка входа. 0x0 означает что файл нельзя запустить.  
А теперь программа `$ readelf -h program` и тут уже  

    Entry point address:               0x4003e0


# second_part  

## Препроцессор

Результат обработки исходного файла препроцессором

$ gcc -E main.c

## Процесс сборки и использующиеся программы  

Чтобы увидеть препроцессор (cc1), компилятор (cc1, as), компоновщик (ld(collect2)).  

$ gcc -o programm -v main.c sum.c

## Детали работа препроцессора

Соглашение 

"sum.h" - локальный файл. Сначала ищется в текущей директории.  

<sum.h> - системный файл


# third_part

$ gcc -E -H main.c > main.i - посмотреть какие хедеры подтяннулись

$ wc main.i - посмотреть размер файла

# fourth_part

## Декорирование (mangling) имен  

$ echo _Z3sumii | c++filt

Поэтому можно сделать  

$ objdump -r -d -C sum.o - раздекорировать имя

Используется компилятор cc1plus:

$ gcc -v main.cpp sum.cp

Закончил на 22.28
