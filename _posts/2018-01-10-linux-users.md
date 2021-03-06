---
layout: post
title: "[Пост] Управление доступом в Linux"
date: "2018-01-10"
category:
  - it
  - tools
---

{% include image.html src="/assets/it/tools/2018-linux-users.png" %}

При развертывании нового приложения в продуктовой среде возникает множество нежданных ошибок, большая часть из них - ошибки прав доступа. Примерно полгода назад собрал информацию в данном по посте. Наконец-то дошли руки довести до ума, так что представляю на суд.

<!--more-->
## Основные правила управления доступом
Объекты (например, файлы и процессы) имеют владельцев. Владельцы обладают обширным (но необязательно неограниченным) контролем над своими объектами.
- Вы являетесь владельцами новых объектов, создаваемых вами.
- Пользователь root с особыми правами, известный как суперпользователь, может действовать как владелец любого объекта в системе.
- Только суперпользователь может выполнять административные операции особого значения.

Владельцем файла всегда является один человек, тогда как в группу владельцев могут входить несколько пользователей. По традиции информация о группах хранилась в фай­ле `/etc/group`.

## Основное
**Пользователь** - это любой кто пользуется компьютером.

Под каждого пользователя, создается свой каталог, пользователю назначается командная оболочка (командный интерпретатор, используемый в операционных системах семейства UNIX). Например: _/bin/bash_, _/bin/zsh_, _/bin/sh_ и другие.

Каждому пользователю назначается идентификационный номер (User ID). Сокращенно номер обозначается как **UID**, является уникальным идентификатором пользователя. Операционная система отслеживает пользователя именно по UID, а не по их имени.

Также, каждому пользователю назначается пароль для входа в систему.

Каждый пользователь принадлежит минимум к одной или нескольким группам.

Помимо пользователей, существуют **группы**. Так же как и пользователь, группа обладает правам доступа к тем или иным каталогам, файлам, периферии. Для каждого файла определён не только пользователь, но и группа. Группы группируют пользователей для предоставления одинаковых полномочий на какие-либо действия.

Каждой группе назначается идентификационный номер (group ID). Сокращённо **GID**, является уникальный идентификатором группы. Принадлежность пользователя к группе устанавливается администратором.

## Управление пользователями
### Просмотр
Вся информация о пользователях хранится в файле `/etc/passwd`.

Каждый аккаунт занимает одну строку, в формате `account:password:UID:GID:GECOS:directory:shell`
- `account` — имя пользователя.
- `password` — зашифрованный пароль пользователя.
- `UID` — идентификационный номер пользователя.
- `GID` — идентификационный номер основной группы пользователя.
- `GECOS` — необязательное поле, используемое для указания дополнительной информации о пользователе (например, полное имя пользователя).
- `directory` — домашний каталог ($HOME) пользователя.
- `shell` — командный интерпретатор пользователя (обычно /bin/sh).

### Получение информации о пользователях
- `w` – вывод информации (имя пользователя, рабочий терминал, время входа в систему, информацию о потребленных ресурсах CPU и имя запущенной программы) о всех вошедших в систему пользователях.
- `who` – вывод информации (имя пользователя, рабочий терминал, время входа в систему) о всех вошедших в систему пользователях.
- `who am i` или `whoami` или `id` – вывод вашего имени пользователя.
- `users` – вывод имен пользователей, работающих в системе.
- `id <имя_пользователя>` – вывод о идентификаторах пользователя: его uid, имя_пользователя, gid и имя первичной группы и список групп в которых состоит пользователь
- `groups <имя_пользователя>` – вывод списка групп в которых состоит пользователь.

### Добавление пользователя
Добавление пользователя осуществляется при помощи команды **useradd**.

`sudo useradd vasyapupkin`

Ключи:
- `-b` *Базовый каталог*. Это каталог, в котором будет создана домашняя папка пользователя. По умолчанию _/home_.
- `-с` Комментарий. В нем вы можете напечатать любой текст.
- `-d` Название домашнего каталога. По умолчанию название совпадает с именем создаваемого пользователя.
- `-e` *Дата, после которой пользователь будет отключен*. Задается в формате ГГГГ-ММ-ДД. По умолчанию отключено.
- `-f` *Количество дней, которые должны пройти после устаревания пароля до блокировки пользователя*, если пароль не будет изменен (период неактивности). Если значение равно 0, то запись блокируется сразу после устаревания пароля, при -1 - не блокируется. По умолчанию -1.
- `-g` *Первичная группа пользователя*. Можно указывать как GID, так и имя группы. Если параметр не задан будет создана новая группа название которой совпадает с именем пользователя.
- `-G` Список вторичных групп в которых будет находится создаваемый пользователь
- `-k` *Каталог шаблонов. Файлы и папки из этого каталога будут помещены в домашнюю папку пользователя. По умолчанию /etc/skel*.
- `-m` Ключ, указывающий, что необходимо создать домашнюю папку. По умолчанию домашняя папка не создается.
- `-p` *Зашифрованный пароль пользователя*. По умолчанию пароль не задается, но учетная пользователь будет заблокирован до установки пароля.
- `-s` Оболочка, используемая пользователем. По умолчанию _/bin/sh_.
- `-u` Вручную задать UID пользователю.

Если при создании пользователя не указываются дополнительные ключи, то берутся настройки по умолчанию. Посмотерть настройки по-умолчанию можно с помощью команды `useradd -D`.

Если вас не устраивают такие настройки, вы можете поменять их выполнив `sudo useradd -D -s /bin/bash`, где `-s` это ключ из таблицы выше.

### Изменение пользователя
Изменение параметров пользователя происходит с помощью утилиты **usermod**. Пример использования:

`sudo usermod -c "Эта команда поменяет комментарий пользователю" vasyapupkin`

Изменить пароль пользователю можно при помощи утилиты **passwd**.

`sudo passwd vasyapupkin`

Утилита passwd может использоваться и обычным пользователем для смены пароля.

Основные ключи passwd:
- `-d` Удалить пароль пользователю. После этого пароль будет пустым, и пользователь сможет входить в систему без предъявления пароля.
- `-e` Сделать пароль устаревшим. Это заставит пользователя изменить пароль при следующем входе в систему.
- `-i` Заблокировать учетную запись пользователя по прошествии указанного количества дней после устаревания пароля.
- `-n` Минимальное количество дней между сменами пароля.
- `-x` Максимальное количество дней, после которого необходимо обязательно сменить пароль.
- `-l` Заблокировать учетную запись пользователя.
- `-u` Разблокировать учетную запись пользователя.

Установка пустого пароля пользователя

Супер пользователь с помощью утилит командной строки passwd и usermod или путем редактирования файла _/etc/shadow_ может удалить пароль пользователь, дав возможность входить в систему без указания пароля.

`sudo passwd -d vasyapupkin` или `sudo usermod -p "" vasyapupkin`

После этого имеет смысл принудить пользователя установить себе новый пароль при следующем входе в систему.

`sudo passwd -e vasyapupkin`

### Удаление пользователя
Для того, чтобы удалить пользователя воспользуйтесь утилитой **userdel**.

`sudo userdel vasyapupkin`

Пример использования:
- `-f` Принудительно удалить пользователя, даже если он сейчас работает в системе.
- `-r` Удалить домашний каталог пользователя.

## Управление группами
### Создание группы
Программа **groupadd** создаёт новую группу согласно указанным значениям командной строки и системным значениям по умолчанию.

`sudo groupadd testgroup`

Основные ключи:
- `-g` Установить собственный GID.
- `-p` Пароль группы.
- `-r` Создать системную группу.

### Изменение группы
Сменить название группы, ее GID или пароль можно при помощи **groupmod**.

`sudo groupmod -n newtestgroup testgroup # Имя группы изменено с testgroup на newtestgroup`

Опции groupmod:
- `-g` Установить другой GID.
- `-n` Новое имя группы.
- `-p` Изменить пароль группы.

### Удаление группы
Утилита **groupdel** не имеет никаких дополнительных параметров.

`sudo groupdel testgroup`

### Управление пользователями группы
Для управления пользователями группы используется утилита **gpasswd**.
Чтобы занести пользователя в группу:

`gpasswd -a [user] [group]`

Вывод пользователя из группы:

`gpasswd -d [user] [group]`

## Файлы конфигурации
### `/etc/passwd`

В файле **/etc/passwd**, который упоминался ранее, хранится вся информация о пользователях кроме пароля. Одна строка из этого файла соответствует описанию одного пользователя. Примерное содержание строки таково:

`vasyapupkin:x:1000:1000:Vasya Pupkin:/home/vpupkin:/bin/bash`

Строка состоит из нескольких полей, каждое из которых отделено от другого двоеточием. Значение каждого поля:
1. `vasyapupkin` Имя пользователя для входа в систему.
2. `x` Необязательный зашифрованный пароль.
3. `1000` Числовой идентификатор пользователя (UID).
4. `1000` Числовой идентификатор группы (GID).
5. `Vasya` Pupkin Поле комментария
6. `/home/vpupkin` Домашний каталог пользователя.
7. `/bin/bash` Оболочка пользователя.

Второе и последнее поля необязательные и могут не иметь значения.

### `/etc/group`

В **/etc/group**, как очевидно из названия хранится информация о группах. Она записана в аналогичном /etc/passwd виде:

`vasyapupkin:x:1000:vasyapupkin,petya`

Строка состоит из нескольких полей, каждое из которых отделено от другого двоеточием. Значение каждого поля:
1. `vasyapupkin` Название группы
2. `x` Необязательный зашифрованный пароль.
3. `1000` Числовой идентификатор группы (GID).
4. `vasyapupkin,petya` Список пользователей, находящихся в группе.

В этом файле второе и четвертое поля могут быть пустыми.

### `/etc/shadow`
Файл /etc/shadow хранит в себе пароли, по этому права, установленные на этот файл, не дают считать его простому пользователю. Пример одной из записей из этого файла:

```
vasyapupkin:xxx:15803:0:99999:7:::
```

Здесь:
1. `vasyapupkin` Имя пользователя для входа в систему.
2. `xxx` Необязательный зашифрованный пароль.
3. `15803` Дата последней смены пароля.
4. `0` Минимальный срок действия пароля.
5. `99999` Максимальный срок действия пароля.
6. `7` Период предупреждения о пароле.
7. `[пусто]` Период неактивности пароля.
9. `[пусто]` Дата истечения срока действия учётной записи.

## Sudo и su
Зная чей-либо пароль, можно непосредственно зарегистрироваться в системе под его именем, введя команду `su имя_пользователя`.

Программа **su** служит для выполнения от имени указанного пользователя (по умолчанию — root) указанной команды/программы (по умолчанию — той программы, что определена в качестве оболочки (shell) для указанного пользователя) и запрашивает она пароль указанного пользователя.

О программе **sudo** можно сказать почти то же самое, за двумя исключениями:
- Нет «программы по умолчанию». для запуска оболочки, определённой для указанного пользователя, надо передать программе опцию -i.
- По умолчанию запрашивается не пароль указанного пользователя, а пароль пользователя, выполняющего программу sudo. какому пользователю, какие программы и от чьего имени можно запускать, определяется содержимым конфигурационного файла `/etc/sudoers` (редактируется с помощью программы visudo).

## Управление доступом
У каждого объекта в Linux есть свой идентификатор, а так же права доступа, применяемые к данному идентификатору. Идентификатор есть у пользователя - _UID_, у группы - _GID_, у файла - _inode_.

Собственно **inode** является, как идентификатором файла/каталога, так и сущностью, которая содержит в себе информацию о файле/каталоге. Например такую, как: принадлежность к владельцу/группе, тип файла и права доступа к файлу.

Для каждого объекта файловой системы в модели полномочий Linux есть три типа полномочий:
- Полномочия чтения (r от read).
- Записи (w от write).
- Выполнения (x от execution).

В полномочия записи входят также возможности удаления и изменения объекта. Право выполнения можно установить для любого файла. Потенциально, любой файл в системе можно запустить на выполнение, как программу в Windows. В Linux является ли файл исполняемым или нет, определяется не по его расширению, а по правам доступа. Кроме того, эти полномочия указываются отдельно для владельца файла, членов группы файла и для всех остальных.

Собрав вышесказанное в кучу, то есть представив 3 правила (rwx) для трех групп (владелец, группа, остальные) запись прав доступа будет выглядеть вот так: `rwx rwx rwx`. Пример прав директории:

```
drwxr-xr-x user group
||||||||||
|||||||||+-исполнение для всех остальных - разрешено
||||||||+--запись для всех остальных - НЕ разрешено
|||||||+---чтение для всех остальных - разрешено
||||||+----исполнение для группы владельца - разрешено
|||||+-----запись для группы владельца - НЕ разрешено
||||+------чтение для группы владельца - разрешено
|||+-------исполнение для владельца - разрешено
||+--------запись для владельца - разрешено
|+---------чтение для владельца - разрешено
+----------тип файла
```

Кроме указанного представления полномочий доступа (символьного), существует так же и числовое представление. Для общего понимания, приведу таблицу соответствия числового (двоичного и десятичного) значения прав доступа и буквенного:

|   | владелец | группа | остальные |
| - | -------- | ------ | --------- |
| буквенное | rwx | r-x | r-- |
| двоичное | 111 | 101 | 100 |
| двоичное в десятичных | 421 | 401 | 400 |
| десятичное | 7 | 5 | 4 |

### Управление правами доступа
Управление правами доступа происходит с помощью команды **chmod**, управление владельцем файла происходит с помощью команды **chown**. Синтаксис команд следующий:

`chmod [к_какой_группе_прав][что_сделать_с_правами][какие_права] <над_каким_объектом>`

`chmod [права] <над_чем>`

- `[к_какой_группе_прав]` может быть:
	- `u` (от user) - владелец-пользователь.
	- `g` (от group) - владелец-группа.
	- `o` (от other) - остальные пользователи.
	- `a` (от all) - все вышеперечисленные группы вместе.
- `[что_сделать_с_правами]` может быть:
	- `+` - добавить.
	- `-` - убрать.
	- `=` - присвоить указанное.
- `[какие_права]` может быть:
	- `r` - чтение.
	- `w` - запись.
	- `x` - выполнение.
- `[над_каким_объектом]` соответственно - имя или путь к файлу
- `[права]` числовое обозначение прав доступа (755, 644 и т.п.)

Использование команды chown выглядит следующим образом:
`chown user:group file` (-R рекурсивно)

### Права доступа к символьным ссылкам
Если посмотреть на права символьных ссылок, то они всегда выглядят так: rwxrwxrwx. Дело в том, что права на символьную ссылку не имеют особого значения. При использования ссылки драйвер файловой системы пересчитывает реальный путь к файлу и применяет права доступа, определенные для реального пути уже без учета символьной ссылки.

### Специальные атрибуты
* **Sticky bit** - бит закрепления в памяти.

Сегодня _sticky bit_ используется в основном для каталогов, чтобы защитить в них файлы. В такой каталог может писать __ЛЮБОЙ__ пользователь. Из такой директории пользователь может удалить только те файлы, владельцем которых он является. Примером может служить директория `/tmp`, в которой запись открыта для всех пользователей, но нежелательно удаление чужих файлов.

* **SUID** - он же Set User ID.

Атрибут исполняемого файла, позволяющий запустить его с правами владельца. В Unix-подобных системах приложение запускается с правами пользователя, запустившего указанное приложение. Это обеспечивает дополнительную безопасность так как процесс с правами пользователя не сможет получить доступ на запись к важным системным файлам, например _/etc/passwd_, который принадлежит суперпользователю _root_. Если на исполняемый файл установлен бит **suid**, то при выполнении эта программа автоматически меняет "эффективный userID" на идентификатор того юзера, который является владельцем этого файла. То есть, не зависимо от того - кто запускает эту программу, она при выполнении имеет права хозяина этого файла.

* **SGID** - он же Set Group ID.
Аналогичен SUID, но относится к группе. При этом, если для каталога установлен бит SGID, то создаваемые в нем объекты будут получать группу владельца каталога, а не пользователя.

Хотелось бы так же провести аналогию с ОС Windows. В указанной операционной системе права регулируются на основе списков ACL. В Linux тоже такое возможно, это реализуется с помощью пакета acl, но данный вопрос в текущей теме  я рассматривать не буду. Еще одно важное замечание! В Windows можно определить права доступа на каталог, и они автоматически распространяются на все файлы и поддиректории (если вы явно не указали иного). В Linux права доступа сохраняются в _inode_ файла, и поскольку _inode_ у каждого файла свой собственный, права доступа у каждого файла свои. Так же, права доступа пользователя и группы не суммируются, как в Windows. Если программа выполняется с правами пользователя и группы, которым принадлежит файл — работают только права хозяина файла.

Исполняемый файл с установленным атрибутом suid является "потенциально опасным". Без установленного атрибута, файл не позволит обычному пользователю сделать то, что выходит за пределы прав пользователя (пример, программа passwd позволяет пользователю изменить только собственный пароль). Но, даже незначительная ошибка в такой программе может привести к тому, что злоумышленник сможет заставить её выполнить ещё какие-нибудь действия, не предусмотренные автором программы. Стоит очень осторожно относиться к данным атрибутам! Как найти в системе файлы с атрибутом SIUD и др.

При создании новой директории в директории с уже установленным SGID-битом, у созданной директории SGID-бит устанавливается автоматически!

### Обозначение атрибутов Sticky, SUID, SGID
Специальные права используются довольно редко, поэтому при выводе программы `ls -l` символ, обозначающий указанные атрибуты, закрывает символ стандартных прав доступа. Пример: rwsrwsrwt, где **s** - SUID, **s** - SGID, **t** - Sticky. В приведенном примере не понятно, rwt — это rw- или rwx? Определить, стоит ли символ стандартных прав доступа под символами s и t - просто. Если t маленькое, значит x установлен. Если T большое, значит x не установлен. То же самое правило распространяется и на s.

В числовом эквиваленте данные атрибуты определяются первым символом при четырехзначном обозначении (который часто опускается при назначении прав), например в правах 1777 - символ 1 обозначает sticky bit. Остальные атрибуты имеют следующие числовое соответствие:
- `1` - sticky bit
- `2` - SGID
- `4` - SUID

## Права доступа по-умолчанию для вновь создаваемых объектов файловой системе.
В Linux, при создании какого-либо файла или каталога предоставляемые права определяются по определенному алгоритму (формуле). Не вдаваясь в подробности и для большего понимания сути скажу, что есть исходные права доступа:
- `0666` - для файлов.
- `0777` - для каталогов.

Есть такая штука как **umask**, которая задана для каждого пользователя и хранится в виде строчки `umask <значение_umask>` в файле _.bash_profile_. Итого, у вновь создаваемого каталога будут права равные исходным правам доступа - umask.

Узнать текущий umask можно, введя команду umask без параметров. Пример:

```
[root@proxy test]# umask
0022
[root@proxy test]# touch file
[root@proxy test]# mkdir test
[root@proxy test]# ls -l
total 4
-rw-r--r-- 1 root root    0 Nov 23 14:51 file
drwxr-xr-x 2 root root 4096 Nov 23 14:51 test
```

Как видно из примера, umask установлен 0022, исходные права доступа равны 0666 - для файлов и 0777 - для каталогов. В результате получаем:

```
0666 - 0022 = 0644 (что соответствует правам -rw-r--r-- для file)
0777 - 0022 = 0755 (что соответствует правам -rwxr-xr-x для каталога test)
```
