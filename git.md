# GIT: Система контроля версий

- [GIT: Система контроля версий](#git-система-контроля-версий)
  - [Локальные изменения](#локальные-изменения)
  - [Тайник/Копилка](#тайниккопилка)
  - [Работа с удалённым репозиторием](#работа-с-удалённым-репозиторием)
  - [Ветки](#ветки)
  - [Теги/метки](#тегиметки)
  - [Настройка репозитория](#настройка-репозитория)
    - [Создание чистого репозитория](#создание-чистого-репозитория)
  - [Игнорирование файлов](#игнорирование-файлов)
  - [История коммитов](#история-коммитов)
  - [Алиасы](#алиасы)
  - [Cherry-pick](#cherry-pick)
  - [Трюки в GIT](#трюки-в-git)
  - [Описание коммита](#описание-коммита)
  - [Настройка GIT](#настройка-git)
  - [NAS](#nas)
  - [Полезные ссылки](#полезные-ссылки)

## Локальные изменения

```bash
# Статус/отслеживание изменённых файлов
git status

# Добавить все файлы в папке для отслеживания
git add .

# Добавить файл для отслеживания
git add [ФАЙЛ]

# Добавить папку для отслеживания
git add [ПАПКИ]

# Переименовать/переместить файл/папку
git mv <ФАЙЛ ОТКУДА> <ФАЙЛ КУДА>

# Удалить файл
git rm <ФАЙЛ>

# Удалить файл который уже проиндексирован
git rm -f <ФАЙЛ>

# Удалить папку которая уже проиндексирована
git rm -fd <ПАПКУ>

# Удалить файл из репозитория, но оставить в рабочей папке
git rm --cached <ФАЙЛ>

# Очистить все не зафиксированные файлы и директории
git clean -fd

# Сделать коммит отслеживаемых фалов
git commit

# Сделать коммит отслеживаемых фалов и добавить комментарий
git commit -m "Комментарий"

# Сделать коммит повторно, при этом можно обновить добавить файлы или переписать комментарий
git commit --amend
git commit -am "Новый комментарий"

# Покажет информацию по коммиту
git show <ХЭШ>

# Вытащить файл из ветки/коммита
git show <ВЕТКА>:<ФАЙЛ>

# Экспортировать файл из ветки/коммита
git show <ВЕТКА>:<ФАЙЛ> > <НОВОЕ ИМЯ ФАЙЛА>

# Убрать из отслеживания файл
git reset [ФАЙЛ]

# Для отмены изменений
git checkout

# Для отмены изменений
git checkout

# Для отмены изменений файла
git checkout -- ./folder/file

# Убрать из буфера (если файлы были добавлены в stage git add file)
git reset HEAD <ФАЙЛ>

# Восстановить файл из определённого коммита
git checkout <ХЭШ> -- <ФАЙЛ>

# Откатить последний коммит
git reset --soft HEAD^
git reset --soft HEAD~

# Удалить полностью все изменения, т.е. вернуть состояние до коммита
git reset --hard

# Откатиться к коммиту (по умолчанию) откатывается к комиту не добавляя файлы в индекс
git reset [--mixed] <commit_hash>
# Откатывается к комиту добавляя файлы к индексу (git add .)
git reset --soft <commit_hash>

# Откатывается к комиту и изменяет рабочую директорию!!! (удалит все не закоммиченые файлы)
git reset --hard <commit_hash>

# Сравнить какие есть изменения в не отслеживаемых файлах
git diff

# Сравнить какие есть изменения в не отслеживаемых файлах и в последнем коммите
git diff --staged
```

## Тайник/Копилка

Тайник позволяет сохранять изменения временно и после этого можно переходить в различные ветки.
После перехода в другую ветку можно вытащить изменения из копилки и сохранить уже в другой ветке.

```bash

# Сохранить в копилку
git stash save "Заголовок тайника"

# Показать список копилок
git stash list

# Показать изменения как git diff --shortstat
git stash show stash@{0}

# Показать изменения как git diff
git stash show -p stash@{0}

# Достать изменения из тайника
git stash apply stash@{0}

# Достать изменения из тайника и удалить копилку
git stash pop stash@{0}

# Удалить изменения из тайника
git stash drop stash@{0}

# Очистить тайник
git stash clear
```

## Работа с удалённым репозиторием

```bash
# Получить и объединить все коммиты из отслеживаемой удаленной ветки
git pull

# Затянуть изменения мастера и соединить со своими, при этом все соединения будут в своём верхнем коммите
git pull --rebase origin master

# Отправить локальную ветку на сервер
git push origin <ВЕТКА>

# Отправить локальную ветку на сервер и переименовав её
git push origin <ВЕТКА>:<НОВОЕ ИМЯ ВЕТКИ>

# Создать удалённую ветку на основе локальной (имя можно указать другое от локальной)
git push [УДАЛ. СЕРВЕР] [ЛОК. ВЕТКА]:[УДАЛ. ВЕТКА]

# Удалить ветку на удалённом сервере вариант #1
git push origin --delete <ВЕТКА>

# Удалить ветку на удалённом сервере вариант #2
git push [УДАЛ. СЕРВЕР] :[УДАЛ. ВЕТКА]
git push origin :<ВЕТКА>

# Создаёт локальную ветку на основе удалённой
git checkout -b <ВЕТКА> origin/<ВЕТКА>

# Показать имя удалённого репозитория
git remote

# Показать удалённый репозитории и его URL
git remote -v

# Добавление удалённый репозиторий
git remote add [ИМЯ] [URL]

# Пример
git remote add origin git@github.com:YourUserName/project.git
git branch -M master
git push -u origin master

# Данная команда связывается с указанным удалённым проектом и забирает все те данные проекта, которых у вас ещё нет. fetch забирает данные в ваш локальный репозиторий, но не сливает их с какими-либо вашими наработками и не модифицирует то, над чем вы работаете в данный момент
git fetch [ИМЯ УДАЛ. СЕРВЕРА]

# Синхронизировать локальные ветки с удалёнными
git fetch origin --prune

# Добавить удалённый репозиторий
git remote add origin https://github.com/user/project.git
git push -u origin master
```

## Ветки

```bash

# Создать ветку и перейти в неё
git checkout -b <ВЕТКА>

# Перейти в ветку
git checkout <ВЕТКА>

# Удалить ветку
git branch -d <ВЕТКА>

# Удалить не слитую ветку
git branch -D <ВЕТКА>

# Переименовать ветку
git branch -m <НОВОЕ ИМЯ ВЕТКИ>

# Слить другую ветку в которую находишься
git merge <ВЕТКА>

# Показать список веток
git branch

# Показать список веток и не только локальных
git branch -a

# Покажет слитые ветки
git branch --merged

# Покажет не слитые ветки
git branch --no-merged

# Покажет последний коммит для каждой ветке
git branch -v

# Смержит одним коммитом
git merge --no-ff <ВЕТКА>

# Показать количество веток
git branch | wc -l
git branch -a | wc -l
```

## Теги/метки

```bash
# Показать список тэгов
git tag

# Создать аннотиррованный тег
git tag -a v1.4 -m 'my version 1.4'

# Покажет информацию о теге/метке
git show v1.4


# (без -s, -m -a) создать легковесный тег
git tag -a v1.5

# Задать тег для прошлого коммита (если забыл)
git tag -a v1.3 -m 'version 1.2' <ХЕШ>

# Просмотр журнала ссылок
git log -g master

# По умолчанию, команда git push не отправляет метки на удалённые серверы
# You just need to push an 'empty' reference to the remote tag name:
git push origin :tagname

# Or, more expressively, use the --delete option (or -d if your git version is older than 1.8.0):

git push --delete origin tagname

# If you also need to delete the local tag, use
git tag --delete tagname
git push origin [имя метки]

# Или отправит скопом:
git push --tags : отправить на сервер метки
```

## Настройка репозитория

```bash
# Создать пустой репозиторий в папке которой находишься
git init

# Создать пустой репозиторий в указанной папке
git init [ПАПКА]

# Клонировать репозиторий в папку в которой находишься
git clone [URL РЕПОЗИТОРИЯ]

# Клонировать репозиторий в указанную папку
git clone [URL РЕПОЗИТОРИЯ] [ПАПКА]
```

### Создание чистого репозитория

```bash
git init
git add .
git commit -m "First commit"
git clone --bare project project.git
scp -r project.git user@address:/home/git
```

## Игнорирование файлов

```bash
touch .gitignore
```

```.gitignore
# комментарий — эта строка игнорируется
# не обрабатывать файлы, имя которых заканчивается на .a
*.a
# НО отслеживать файл lib.a, несмотря на то, что мы игнорируем все .a файлы с помощью предыдущего правила
!lib.a
# игнорировать только файл TODO находящийся в корневом каталоге, не относится к файлам вида subdir/TODO
/TODO
# игнорировать все файлы в каталоге build/
build/
# игнорировать doc/notes.txt, но не doc/server/arch.txt
doc/*.txt
# игнорировать все .txt файлы в каталоге doc/
doc/**/*.txt
```

## История коммитов

```bash
# Показать историю всех коммитов в конкретной ветке
git log

# Показать историю всех коммитов в конкретной ветке на N последних записи изменений
git log -N

# Показать историю и дельта изменений всех коммитов в конкретной ветке
git log -p

# Показать историю и дельта изменений 2х последних коммитов в конкретной ветке
git log -p -2

# Показать историю всех коммитов в конкретной ветке и суммирование изменений
git log --stat

# Показать историю всех коммитов в конкретной ветке и суммирование изменений по пользователю
git log --stat --author <АВТОР>
git log --stat --author Konstantin
git log --author="Konstantin" --oneline --shortstat
git log --author="Konstantin" --shortstat

# Однострочный вывод истории
git log --pretty=oneline

# Количество коммитов
git shortlog -nse
git shortlog -sn --no-merges

# Посмотреть количество добавленных и убранных строк
git diff --stat


Этот параметр добавляет миленький ASCII-граф, показывающий историю ветвлений и слияний
git log --graph

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --

git log --pretty=format:"%h - %an, %ar : %s"

# --since, --after Ограничить коммиты теми, которые сделаны после указанной даты.
# --until, --before   Ограничить коммиты теми, которые сделаны до указанной даты.
# После 2х недель
git log --since=2.weeks
# До 2х недель
git log --until=2.weeks
git log --until="2 years 1 day 3 minutes ago"

```

## Алиасы

В файл `~/.gitconfig` добавить в раздел `[alias]`

```conf
# Кто создал ветки на удалённом сервере
who-creator-branches = "for-each-ref --format='%(color:cyan)%(authordate:format:%m/%d/%Y %I:%M %p)    %(align:25,left)%(color:yellow)%(authorname)%(end) %(color:reset)%(refname:strip=2)' --sort=author refs/remotes"

# Сколько строк добавленно/удалено в незакомиченом статусе
lines = diff --stat

# Показать дерево
graph = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative

co = checkout
ci = commit
```

```bash

# Показать список всех алиасов
git config -l | grep alias | sed 's/^alias\.//g'

git config --global --unset alias.br

# alias для git checkout
git config --global alias.co checkout

# alias для git branch
git config --global alias.br branch

# alias для git commit
git config --global alias.ci commit

# alias для
git config --global alias.reci 'commit --amend' git commit --amend

# alias для git status
git config --global alias.st status

# Посмотреть когда последний раз модифицировался файл
git config --global alias.last_modified 'log -1 --pretty="format:%ci %aN"'

git config --global alias.unstage 'reset HEAD --'

git config --global alias.shortstat 'log --shortstat'

# Количество добавленных/убранных строк
git config --global alias.lines 'diff --stat'

# Где удалена строка
git config --global alias.where-deleted "log --all --sourc -S"
```

## Cherry-pick

Как и при операции git rebase в процессе переноса коммита могут возникнуть конфликты. Как и при обычном git merge их следует разрешить, добавить изменения в индекс с помощью git add, а затем продолжить запустив git cherry-pick --continue.

```bash
# Из ветки в которую надо затянуть коммит другой ветки
git cherry-pick <hash-commit>

# В случае, когда необходимо перенести последний коммит ветки master, можно вместо хэша коммита просто написать master, поскольку в данный момент указатель master ссылается на последний коммит.
git cherry-pick master

#Если же нужно перенести не один, а несколько подряд идущих коммитов от <hash-commit-begin> до <hash-commit-end>, то это делается похожим образом.
git cherry-pick <hash-commit-begin>..<hash-commit-end>

#Если же вы хотите, чтобы при переносе изменений коммит не создавался, то используйте параметр -n (—no-commit)
git cherry-pick -n d112ecf96
```

## Трюки в GIT

```bash
git log --all --full-history -- \*_/thefile._: Найти комит с удалённым файлом

git log -S 'искомая строка' --source --all : найти комиты где была удалена "искомая строка"

git log -1 -- [file path]

# Если требуется объединить последние N коммитов, есть другой, изящный способ. Например для последних 3-х коммитов:
git reset --soft HEAD~3
git commit

git reset --soft HEAD~3 && git commit --edit -m"$(git log --format=%B --reverse HEAD..HEAD@{1})"

# Авторы
git shortlog -s | cut -c8-

# Найти когда был создан файл
git log --oneline <ФАЙЛ> | tail -1

# Покажет кто удалил строку в файле
git log -S <СТРОКА> <ФАЙЛ>
git log -S "unique=True" ./authentication/models.py

#Коммиты в Git можно объединять в один несколькими способами (git rebase -i , git merge --squash ) и они неплохо работают, если вам нужно объединить ревизии где-то из середины ветки.


# Посмотреть когда последний раз модифицировался файл
git log -1 --pretty="format:%ci %aN" <ФАЙЛ>
```

## Описание коммита

Краткое **(до 50 символов)** описание изменений

Более детальное объяснение, если необходимо. Перенос на 72 символе или около того. В некоторых контекстах первая строка рассматривается как тема письма, а остальное как тело. Пустая строка, отделяющая сводку от тела, важна (если вы не опустили тело целиком); если вы оставите их
вместе, инструменты, такие как rebase, могут воспринять это неправильно. Дальнейшие параграфы идут после пустых строк

- также можно применять маркеры списков
- обычно в качестве маркера списка используется дефис или звёздочка с одним пробелом перед ним и пустыми строками между пунктами, хотя соглашения в этом аспекте могут разниться
- в конце писать если есть с каким тикетом связана или какой тикет решён в комите `Resolve: #XXX, Relate: #XXX`

```git
Add ability to deploy on server

- Add new logic for the component
- Fix some an issue

Resolve: #777, Relate: #666
```

## Настройка GIT

```bash
# Настройка имени и почты автора
git config --global user.name "Konstantin Patsera"
git config --global user.email "konstantin.patsera@yandex.ru"


# Параметры установки окончаний строк
git config --global core.autocrlf input
git config --global core.safecrlf true
```

## NAS

```bash
git clone --bare project project.git
scp -r ./project.git konstantin@nas:/volume1/git/
git clone git@nas:/volume1/git/project.git
```

## Полезные ссылки

- [GitHub Cheat Sheet](https://github.com/tiimgreen/github-cheat-sheet#github-cheat-sheet-)

- [Git: объясните «на пальцах» разницу между rebase и cherry-pick?](https://qna.habr.com/q/28207)

- [Как в Git перенести commit из одной ветки в другую?](http://paratapok.ru/developer-tools/2593_kak-v-git-perenesti-commit-iz-odnoj-vetki-v-druguyu/)

- [How to undo (almost) anything with Git](https://github.com/blog/2019-how-to-undo-almost-anything-with-git)

- [Команда Git stash. Как прятать изменения в Git](https://pingvinus.ru/git/1718)