# Работа в консоли

## 1. Перемещение между директориями

Для просмотра текущего каталога использовалась команда `pwd`, для перехода между каталогами - команда `cd`.

```bash
pwd
cd /tmp
pwd
cd ~
pwd
cd ..
pwd
cd ~
```

Вывод:

```text
[student@student-04 ~]$ pwd
/home/student
[student@student-04 ~]$ cd /tmp
[student@student-04 tmp]$ pwd
/tmp
[student@student-04 tmp]$ cd ~
[student@student-04 ~]$ cd ..
[student@student-04 home]$ cd ~
[student@student-04 ~]$ cd ..
[student@student-04 home]$ ls
student
[student@student-04 home]$ cd ~
[student@student-04 ~]$
```

В результате были выполнены переходы между домашним каталогом `/home/student`, каталогом `/tmp` и родительским каталогом `/home`.



## 2. Вывод списка файлов в директории

Для просмотра содержимого текущего каталога использовалась команда `ls`.

Вывод:

```text
[student@student-04 ~]$ ls

```
Ничего не вывелось, в этой директории нет нескрытых файлов.




## 3. Вывод всех файлов, включая скрытые

Для отображения обычных и скрытых файлов использовалась команда:

```bash
ls -la
```

Вывод:

```text
[student@student-04 ~]$ ls -la
total 64
drwx------ 8 student student 4096 Sep 27 23:29 .
drwxr-xr-x 3 root    root    4096 Sep 14 22:34 ..
-rw------- 1 student student 1525 Sep 27 23:29 .bash_history
-rw------- 1 student student  217 Jul 26  2021 .bash_logout
-rw------- 1 student student  259 Jul 26  2021 .bash_profile
-rw------- 1 student student  188 Jul 26  2021 .bashrc
drwx------ 2 student student 4096 Jul 26  2021 .cache
drwx------ 2 student student 4096 Jul 26  2021 .config
-rw------- 1 student student   20 Sep 27 21:03 .lesshst
drwx------ 3 student student 4096 Oct  7  2025 .local
-rw------- 1 student student   17 Jul 26  2021 .lpoptions
drwx------ 3 student student 4096 Oct  7  2025 .mutt
-rw------- 1 student student  121 Jul 26  2021 .rpmmacros
drwx------ 2 student student 4096 Jul 26  2021 .ssh
-rwx------ 1 student student  319 Jul 26  2021 .xprofile
drwx------ 1 student student 4096 Jul 26  2021 .xsession.d
[student@student-04 ~]$
```


## 4. Создание папки с несколькими уровнями подпапок

Была создана следующая структура каталогов:

```bash
mkdir -p console_work/documents/texts
mkdir -p console_work/backup
```

Для проверки структуры использовались команды:

```bash
find console_work -type d
ls -R console_work
```

Вывод:

```text
[student@student-04 ~]$ mkdir -p console_work/documents/texts
[student@student-04 ~]$ mkdir -p console_work/backup
[student@student-04 ~]$ find console_work -type d
console_work
console_work/documents
console_work/documents/texts
console_work/backup
[student@student-04 ~]$ ls -R console_work
console_work:
backup  documents

console_work/backup:

console_work/documents:
texts

console_work/documents/texts:
```

Итоговая структура:

```text
console_work/
|- backup/
|- documents/
    |- texts/
```



## 5. Создание файла и запись нескольких строк текста

Файл `data.txt` был создан внутри каталога `documents/texts` с помощью команды `cat`:

```bash
cat > console_work/documents/texts/data.txt << 'EQF'
Лиукс крутая ОС
Прямо очень крутая
Самая лучшая в мире
EQF
```

Для проверки содержимого использовалась команда:

```bash
cat console_work/documents/texts/data.txt
```

Вывод:

```text
[student@student-04 ~]$ cat > console_work/documents/texts/data.txt << 'EQF'
> Лиукс крутая ОС
> Прямо очень крутая
> Самая лучшая в мире
> EQF
[student@student-04 ~]$ cat console_work/documents/texts/data.txt
Лиукс крутая ОС
Прямо очень крутая
Самая лучшая в мире
```


## 6. Перемещение файла

Файл был перемещён из каталога `documents/texts` в каталог `documents`:

```bash
mv console_work/documents/texts/data.txt console_work/documents/data.txt
```

Проверка расположения файла:

```bash
find console_work -type f
```

Вывод:

```text
[student@student-04 ~]$ mv console_work/documents/texts/data.txt console_work/documents/data.txt
[student@student-04 ~]$ find console_work -type f
console_work/documents/data.txt
```

Файл `data.txt` был успешно перемещён.



## 7. Копирование файла

Файл был скопирован из каталога `documents` в каталог `backup`:

```bash
cp console_work/documents/data.txt console_work/backup/data_copy.txt
```

Проверка:

```bash
find console_work -type f
```

Вывод:

```text
[student@student-04 ~]$ cp console_work/documents/data.txt console_work/backup/data_copy.txt
[student@student-04 ~]$ find console_work -type f
console_work/documents/data.txt
console_work/backup/data_copy.txt
```

В результате были получены два файла: `data.txt` и его копия `data_copy.txt`.



## 8. Переименование файла

Скопированный файл был переименован из `data_copy.txt` в `backup_data.txt`:

```bash
mv console_work/backup/data_copy.txt console_work/backup/backup_data.txt
```

Проверка:

```bash
find console_work -type f
```

Вывод:

```text
[student@student-04 ~]$ mv console_work/backup/data_copy.txt console_work/backup/backup_data.txt
[student@student-04 ~]$ find console_work -type f
console_work/documents/data.txt
console_work/backup/backup_data.txt
```

Переименование выполнено успешно.



## 9. Сравнение содержимого двух файлов

Для сравнения содержимого исходного файла и его копии использовалась команда:

```bash
diff -s console_work/documents/data.txt console_work/backup/backup_data.txt
```

Вывод:

```text
[student@student-04 ~]$ diff -s console_work/documents/data.txt console_work/backup/backup_data.txt
Files console_work/documents/data.txt and console_work/backup/backup_data.txt are identical
```

Полученный результат показывает, что содержимое двух файлов полностью совпадает.



## 10. Сортировка содержимого файла

Для сортировки строк по возрастанию использовалась команда:

```bash
sort console_work/documents/data.txt
```

Вывод:

```text
[student@student-04 ~]$ sort console_work/documents/data.txt
Лиукс крутая ОС
Прямо очень крутая
Самая лучшая в мире
```

Для сортировки строк в обратном порядке использовалась команда:

```bash
sort -r console_work/documents/data.txt
```

Вывод:

```text
[student@student-04 ~]$ sort -r console_work/documents/data.txt
Самая лучшая в мире
Прямо очень крутая
Лиукс крутая ОС
```


## 11. Поиск файла по имени

Для поиска файла `data.txt` без необходимости вручную переходить по каталогам использовалась команда:

```bash
find ~/console_work -type f -name "data.txt"
```

Вывод:

```text
[student@student-04 ~]$ find ~/console_work -type f -name "data.txt"
/home/student/console_work/documents/data.txt
```

В результате файл был найден по полному пути:

```text
/home/student/console_work/documents/data.txt
```



## 12. Удаление созданных файлов и каталогов

Для удаления всей созданной в ходе работы структуры использовалась команда:

```bash
rm -rf ~/console_work
```

После удаления была выполнена проверка существования каталога:

```bash
if [ ! -e ~/console_work ]; then echo "всё удалено"; fi
```

Вывод:

```text
[student@student-04 ~]$ rm -rf ~/console_work
[student@student-04 ~]$ if [ ! -e ~/console_work ]; then echo "всё удалено"; fi
всё удалено
```

Полученный результат подтверждает, что каталог `console_work` и всё его содержимое были удалены.



## Абсолютный и относительный путь

Абсолютный путь — это полный путь к файлу или каталогу, начинающийся от корневого каталога `/`. Например:

```text
/home/student/console_work/documents/data.txt
```

Такой путь однозначно определяет расположение файла независимо от текущего каталога пользователя.

Относительный путь задаётся относительно текущего каталога. Например, если текущий каталог - `/home/student`, то тот же файл можно указать следующим образом:

```text
console_work/documents/data.txt
```

Таким образом, основное различие состоит в том, что абсолютный путь начинается от корневого каталога `/` и не зависит от текущего положения пользователя, а относительный путь зависит от текущего каталога.
