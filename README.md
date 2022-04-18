# "Pro Git" - Scott Chacon, Ben Straub
*Notes [book](https://github.com/progit/progit2/releases/download/2.1.338/progit.pdf) by Kate Bay*

## Первоначальная настройка Git
Файл конфигурации может быть: --system (для всех пользователей), --global (на уровне пользователя), --local (по-умолчанию на уровне репо).
`git config --list --show-origin` //просмотр настроек с указанием файлов где лежат настройки
`git config --global user.name' "John Doe"` //задать имя пользователя
`git config --global user.email johndoe@example.com` //задать эмэйл
`git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`  //выбор редактора для текста
`git config --global init.defaultBranch main`  //установить по-умолчанию названии ветки main вместо master для новых репо при инициализации
`git help <команда>  или git <команда> -h` - справка по команде
## Создание Git-репозитория
Репозиторий Git можно получить одним из двух способов:
1. Вы можете взять локальный каталог, который в настоящее время не находится под версионным контролем, и превратить его в репозиторий Git.
`git init` - эта команда создаёт в текущем каталоге новый подкаталог с именем .git, содержащий все необходимые файлы репозитория — структуру Git репозитория.
2. Вы можете клонировать существующий репозиторий Git из любого места.
`git clone https://github.com/libgit2/libgit2 mylibgit`
Эта команда создаёт каталог mylibgit, инициализирует в нём подкаталог .git, скачивает все данные для этого репозитория и извлекает рабочую копию последней версии
При выполнении git clone с сервера забирается (pulled) каждая версия каждого файла из истории проекта.

В Git реализовано несколько транспортных протоколов. В предыдущем примере использовался протокол https://, вы также можете встретить git:// или user@server:path/to/repo.git, использующий протокол передачи SSH.
## Запись изменений в репозиторий
Каждый файл в рабочем каталоге может находиться в одном из двух
состояний: под версионным контролем (отслеживаемые) и нет (неотслеживаемые).
*Отслеживаемые файлы (tracket) * —  это те файлы, которые были в последнем снимке состояния проекта (это те файлы о которых знает Git); они могут быть *неизменёнными (unmodified)*, *изменёнными (modified)* или *подготовленными к коммиту (staged)*.
 *Неотслеживаемые файлы   (untracket)* —  это всё остальное, любые файлы в вашем рабочем каталоге, которые не входили в ваш последний снимок состояния и не подготовлены к коммиту.
Когда вы впервые клонируете репозиторий, все файлы будут отслеживаемыми и
неизменёнными, потому что Git только что их извлек и вы ничего пока не редактировали.
![Рисунок 1. Жизненный цикл состояний файлов](https://git-scm.com/book/en/v2/images/lifecycle.png)
Рисунок 1. Жизненный цикл состояний файлов

`git status` //определение состояния файлов
`git add <FILE>` //сделать файл отслеживаемым (если он был новым) и добавить в индекс (подготовленным к коммиту).
Файл попадёт в коммит в том состоянии, в котором он находился, когда вы последний раз выполняли команду git add , а не в том, в котором он находится в вашем рабочем каталоге в момент выполнения git commit. Если вы изменили файл после выполнения git add, вам придётся снова выполнить git add, чтобы проиндексировать последнюю версию файла.
## Игнорирование файлов
Для того чтобы файлы не попадали в репозиторий необходим добавить эти файлы/шаблоны в файл .gitignore. Желательно сделать это перед началом работы с проектом.
Правила шаблонов в файле .gitignore:
- Пустые строки, а также строки, начинающиеся с #, игнорируются.
- Стандартные шаблоны являются глобальными и применяются рекурсивно для всего дерева каталогов.
- Чтобы избежать рекурсии используйте символ слеш (/) в начале шаблона.
- Чтобы исключить каталог добавьте слеш (/) в конец шаблона.
- Можно инвертировать шаблон, использовав восклицательный знак (!) в качестве первого символа.

Glob-шаблоны представляют собой упрощённые регулярные выражения, используемые
командными интерпретаторами. Символ (*) соответствует 0 или более символам;
последовательность [abc] — любому символу из указанных в скобках (в данном примере a, b
или c); знак вопроса (?) соответствует одному символу; и квадратные скобки, в которые
заключены символы, разделённые дефисом ([0-9]), соответствуют любому символу из
интервала (в данном случае от 0 до 9). Вы также можете использовать две звёздочки, чтобы
указать на вложенные каталоги: a/**/z соответствует a/z, a/b/z, a/b/c/z, и так далее.

Пример файла:
```
# Исключить все файлы с расширением .a
*.a
# Но отслеживать файл lib.a даже если он подпадает под исключение выше
!lib.a
# Исключить файл TODO в корневом каталоге, но не файл в subdir/TODO
/TODO
# Игнорировать все файлы в каталоге build/
build/
# Игнорировать файл doc/notes.txt, но не файл doc/server/arch.txt
doc/*.txt
# Игнорировать все .txt файлы в каталоге doc/
doc/**/*.txt
```
## Просмотр индексированных и неиндексированных изменений
`git diff` //покажет что изменили, но пока не проиндексировали. сравнивает содержимое вашего рабочего каталога с содержимым индекса. Результат показывает ещё не проиндексированные изменения.
`git diff --staged` //что проиндексировано и что войдёт в следующий коммит. Эта команда сравнивает ваши проиндексированные изменения с последним коммитом.
`git difftool` //то же самое но просмотр с помощью предустановленной утилиты
## Коммит изменений
`git commit -m "First commit"`//коммит сохраняет снимок состояния вашего индекса
Каждый раз, когда вы делаете коммит, вы сохраняете снимок состояния вашего проекта, который позже вы можете восстановить или с которым можно сравнить текущее состояние.
`git commit -a` //параметр -a заставляет Git автоматически индексировать каждый уже отслеживаемый на момент коммита файл, позволяет обойтись без git add.
## Удаление файлов
Чтобы удалить файл из Git, необходимо удалить его из отслеживаемых файлов (точнее, удалить его из индекса), а затем выполнить коммит.
`git rm <file>` //удаляет файл из вашего рабочего каталога и удаляет из отслеживаемых
`git rm --cached README` //удалить файл из индекса (перестать отслеживать изменения), оставив его при этом в рабочем каталоге.
## Перемещение файлов
Git не отслеживает перемещение файлов явно.
`git mv file_from file_to`
это аналогично:
`mv README.md README`
`git rm README.md`
`git add README`
Можно использовать любой удобный способ для переименования файла, а затем воспользоваться командами add/rm перед коммитом.
## Просмотр истории коммитов
`git log` - перечисляет в обратном к хронологическому порядке коммиты с их SHA-1 контрольными суммами, именем и электронной почтой автора, датой создания и сообщением коммита.
`git log -p -2` //вывести два послдених коммита и разницу (выводит патч), внесенную в каждый коммит
`git log --pretty=oneline` //вывод лога одной строкой
*Полезные опции для 'git log --pretty=format'*
`%H` Хеш коммита
`%h` Сокращенный хеш коммита
`%T` Хеш дерева
`%t` Сокращенный хеш дерева
`%P` Хеш родителей
`%p` Сокращенный хеш родителей
`%an` Имя автора
`%ae` Электронная почта автора
`%ad` Дата автора (формат даты можно задать опцией --date=option)
`%ar` Относительная дата автора
`%cn` Имя коммитера
`%ce` Электронная почта коммитера
`%cd` Дата коммитера
`%cr` Относительная дата коммитера
`%s` Содержание
Например, `git log --pretty=format:"%h - %an, %ar : %s"`
`git log --pretty=format:"%h %s" --graph` //выводит текущую ветку и историю слияний
*Наиболее распространенные опции для команды `git log`*
`-p` Показывает патч для каждого коммита.
`--stat` Показывает статистику измененных файлов для каждого коммита.
`--shortstat` Отображает только строку с количеством изменений/вставок/удалений для команды `--stat`.
`--name-only` Показывает список измененных файлов после информации о коммите.
`--name-status` Показывает список файлов, которые добавлены/изменены/удалены.
`--abbrev-commit` Показывает только несколько символов SHA-1 чек-суммы вместо всех 40.
`--relative-date` Отображает дату в относительном формате (например, «2 weeks ago») вместо стандартного формата даты.
`--graph` Отображает ASCII граф с ветвлениями и историей слияний.
`--pretty` Показывает коммиты в альтернативном формате. Возможные варианты опций: `oneline`, `short`, `full`, `fuller` и `format` (с помощью последней можно указать свой формат).
`--oneline` Сокращение для одновременного использования опций --pretty=oneline --abbrev-commit.
## Ограничение вывода
*Опции для ограничения вывода команды `git log`*
`-(n)` Показывает только последние n коммитов.
`--since`, --after Показывает только те коммиты, которые были сделаны после указанной даты.
`--until`, `--before` Показывает только те коммиты, которые были сделаны до указанной даты.
`--author` Показывает только те коммиты, в которых запись author совпадает с указанной строкой.
`--committer` Показывает только те коммиты, в которых запись committer совпадает с указанной строкой.
`--grep` Показывает только коммиты, сообщение которых содержит указанную строку.
`-S` Показывает только коммиты, в которых изменение в коде повлекло за собой добавление или удаление указанной строки.'
Например:
`git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \ --before="2008-11-01" --no-merges -- t/` //коммиты, в которых произошли изменения в тестовых файлах в исходном коде Git в октябре 2008 года, автором которых был Junio Hamano и которые не были коммитами слияния.
##Операции отмены
`git commit --amend` - команда использует область подготовки (индекс) для внесения правок в последний коммит. Если вы ничего не меняли с момента последнего коммита (например, команда запущена сразу после предыдущего коммита), то снимок состояния останется в точности таким же, а всё что вы сможете изменить — это ваше сообщение к коммиту.
Например, если вы сделали коммит и поняли, что забыли проиндексировать изменения в файле, который хотели добавить в коммит, то можно сделать следующее:
`git commit -m 'Initial commit'`
`git add forgotten_file`
`git commit --amend`
В итоге получится единый коммит — второй коммит заменит результаты первого.
Когда вы вносите правки в последний коммит, вы не столько исправляете его, сколько заменяете новым, который полностью его перезаписывает. В результате всё выглядит так, будто первоначальный коммит никогда не существовал, а так же он больше не появится в истории вашего репозитория.
###Отмена индексации файла
`git reset HEAD <file>` - удаление файла из индекса (измененный файл остается в папке).
###Отмена изменений в файле
`git checkout -- CONTRIBUTING.md` - вернуть файл в локальной папке к тому состоянию, которое было в последнем коммите (или к начальному после клонирования, или еще как-то полученному).
###Отмена действий с помощью `git restore`
Начиная с версии 2.23.0, Git будет использовать `git restore` вместо `git reset` для многих операций отмены.
####Отмена индексации файла с помощью `git restore`
`git restore --staged <file>` - отмена индексации файла.
####Откат измененного файла с помощью `git restore`
`git restore <file>` - изменения в локальной папке, внесенные в этот файл, исчезнут  —  Git просто заменит файл последней зафиксированной версией.
##Основы Git - Работа с удалёнными репозиториями
###Работа с удалёнными репозиториями
У вас может быть несколько удалённых репозиториев, каждый из которых может быть доступен для чтения или для чтения-записи. Удаленный репозиторий может находиться на вашем локальном компьютере.
###Просмотр удалённых репозиториев
`git remote` - просмотр списка удаленных репозиториев.
`git remote -v` - адреса для чтения и записи, привязанные к репозиторию
###Добавление удалённых репозиториев
`git remote add <shortname> <url>` - добавить удалённый репозиторий и присвоить ему имя (shortname)
###Получение изменений из удалённого репозитория — Fetch и Pull
`git fetch [remote-name]` - получение данных из удалённого репозитория.
Данная команда связывается с указанным удалённым проектом и забирает все те данные проекта, которых у вас ещё нет. После того как вы выполнили команду, у вас должны появиться ссылки на все ветки из этого удалённого проекта, которые вы можете просмотреть или слить в любой момент.
Когда вы клонируете репозиторий, команда clone автоматически добавляет этот удалённый репозиторий под именем `origin`. Таким образом, `git fetch origin` извлекает все наработки, отправленные на этот сервер после того, как вы его клонировали (или получили изменения с помощью fetch). Важно отметить, что команда `git fetch` забирает данные в ваш локальный репозиторий, но не сливает их с какими-либо вашими наработками и не модифицирует то, над чем вы работаете в данный момент. Вам необходимо вручную слить эти данные с вашими, когда вы будете готовы.
Если ветка настроена на отслеживание удалённой ветки, то вы можете использовать команду `git pull` чтобы автоматически получить изменения из удалённой ветки и слить их со своей текущей. Этот способ может для вас оказаться более простым или более удобным. К тому же, по умолчанию команда `git clone` автоматически настраивает вашу локальную ветку `master` на отслеживание удалённой ветки master на сервере, с которого вы клонировали репозиторий. Название веток может быть другим и зависит от ветки по умолчанию на сервере. Выполнение `git pull`, как правило, извлекает (`fetch`) данные с сервера, с которого вы изначально клонировали, и автоматически пытается слить (`merge`) их с кодом, над которым вы в данный момент работаете.
###Отправка изменений в удаленный репозиторий (Push)
`git push <remote-name> <branch-name>` - отправить свои изменения в удалённый репозиторий.
Чтобы отправить вашу ветку `master` на сервер `origin` (повторимся, что клонирование обычно настраивает оба этих имени автоматически), вы можете выполнить следующую команду для отправки ваших коммитов:
`git push origin master`
Эта команда срабатывает только в случае, если вы клонировали с сервера, на котором у вас есть права на запись, и если никто другой с тех пор не выполнял команду `push`. Если вы и кто-то ещё одновременно клонируете, затем он выполняет команду `push`, а после него выполнить команду `push` попытаетесь вы, то ваш push точно будет отклонён. Вам придётся сначала получить изменения и объединить их с вашими и только после этого вам будет позволено выполнить `push`.
###Просмотр удаленного репозитория
git remote show <remote> - выдаёт URL удалённого репозитория, а также информацию об отслеживаемых ветках.
Показывает какая именно локальная ветка будет отправлена на удалённый сервер по умолчанию при выполнении `git push`. Она также показывает, каких веток с удалённого сервера у вас ещё нет, какие ветки всё ещё есть у вас, но уже удалены на сервере, и какие удалённые ветки будут в них влиты при выполнении `git pull`.
###Удаление и переименование удалённых репозиториев
`git remote rename <old_name> <new_name>` - переименование удаленного репозитория.
`git remote remove <name>` - удаление удаленного репозитория.
При удалении ссылки на удалённый репозиторий все отслеживаемые ветки и настройки, связанные с этим репозиторием, так же будут удалены.