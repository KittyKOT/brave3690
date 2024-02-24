# Инструкция в системе контроля версий GIT

## Определение GIT
**GIT** - это консольная утилита, для отслеживания и ведения истории изменения файлов, в Вашем проекте.
## Установка и настройка GIT
Скачать установочный файл Git для Windows, MAC, Linux необходимо по ссылке: [GIT Download](https://git-scm.com/downloads "Установить GIT")

При первом использовании GIT необходимо указать имя и электронную почту пользователя. В противном случае GIT не позволит записывать индексированные изменения в репозиторий Git. Для ввода данных пользователя необходимо выполнить следующие команды:
>git config --global user.name<br>
>config --global user.email

Вместо *name* в команде требуется написать имя пользователя, а вместо *email* - адрес электронной почты пользователя.
## Инициализация локального репозитория
>**git init** - создание пустого репозитория в выбранной папке. После выполнения команды в папке появится скрытый файл с наименованием *.git*

После добавляются файлы в папку (проект), изначально имеющие неотслеживаемое GIT состояние - *Untracked*.

Состояние рабочего каталога и раздела проиндексированных файлов отображает команда: 
>**git status**

## Контроль изменения версий
Основным объектом в управлении контроля версий является <u>**коммит**</u>. Каждый последующий коммит добавляется после внесения изменений и с его помощью можно отследить информацию об этих изменениях.

Добавление коммита производится в 2 этапа:

1. Добавление файла или файлов к следующему коммиту, когда изменения фиксируются для включения в коммит (состояние *staged*). Можно использовать несколько способов вызова этого действия:
- >**git add -A** - добавить все файлы в состояние staged
- >**git add .** - добавить все файлы из текущей папки и всех внутренних.
- >**git add <file_name>** - добавляет только конкретный файл. Вместо **<file_name>** нужно указать имя файла.

2. Создание коммита с помощью команды, где вместо **"message"** вводится текст комментария

    >**git commit -m "message"**

Просмотр истории коммитов в ветке (указателе коммитов) осуществляется с помощью команды:

- >**git log** - выводится полный хэш-код (идентификатор) коммита

*или*

- >**git log --oneline** - отображает идентификатор версии файла в сокращённом формате

Для просмотра различия версий файла в разных коммитах, используется команда
>**git diff**

В конце команды нужно указать идентификатор коммита, если сравниваем с текущей версией, или два идентификатора, если сраниваем определённые предыдущие версии.

Для того, чтобы переключиться в нужную версию файла, нужно найти идентификатор коммита (сокращённый или полный) и указать его в конце команды.

```sh
git checkout
```

### Использование веток при работе с GIT

#### Понятие и применение веток
**Ветка** – это последовательность коммитов. Ветку можно рассматривать как временную шкалу. Коммиты в ней – снимки интересных моментов, идущие друг за другом в хронологической последовательности. 

С точки зрения внутренней реализации Git, **ветка** – это ссылка на последний коммит в этой ветке. Ветка-ссылка указывает на коммит, который является последним в "потоке" коммитов в данной ветке. 

Частые случаи использования веток:

+ одновременная работа нескольких программистов над одним проектом / файлом не мешая друг другу.
+ тестирование экспериментальных функций, когда создается новая ветка специально для экспериментов. Если эксперимент удался, изменения с экспериментальной ветки переносятся на основную, если нет – новая ветка удаляется
+ для разных выходящих параллельно релизов одного проекта.

#### Создание веток

```sh
git branch <branch_name>
```
команда проверяет наличие ветки и в случае отсутствия ветки с указанным наименованием создаёт новую.

#### Просмотр веток

При работе с большим количеством веток, можно забыть имя нужной, а без имени ветки переключиться на нее не получится. Для таких ситуаций существует команда просмотра списка веток: 
```sh
git branch
```
По умолчанию команда выводит список локальных веток. 

С ключом _**"-r"**_ можно вывести только удаленные ветки, c ключом _**"-a"**_ - и локальные, и удалённые ветки.

#### Переключение между ветками

```sh
git checkout <Branch_Name>
```
команда для перехода к указанной ветке.

Команда **git checkout** умеет создавать ветки и сразу переключаться на них. Это намного удобнее, чем выполнять два этих действия по отдельности. Поэтому данный способ является более предпочтительным, так как задействует только одну команду **git checkout** со специальным ключом _**-b**_.

По умолчанию все коммиты добавляются в ветку *master*, но внутри любого коммита можно создавать новые ветки коммитов. Тогда необходимо использовать команду
>**git checkout -b *<Branch_Name> <hashtag_commit>***

, где вместо ***<Branch_Name>*** указываем наименование новой ветки, а вместо ***<hashtag_commit>*** идентификатор коммита, в котором будет создаваться ветка

Если необходимо переключиться к актуальному состоянию файла и продолжить работу, то требуется указать команду
>**git checkout master**

#### Слияние веток

**Сливаемая ветка** – та ветка, с которой мы берем изменения, чтобы влить их в целевую.

**Целевая ветка** – та ветка, в которую мы сливаем наши изменения.

**Слияние веток** – это перенос изменений с одной ветки на другую. При этом слияние не затрагивает сливаемую ветку, то есть она остается в том же состоянии, что позволяет нам потом продолжить работу с ней.

```sh
git merge <Merged_branch_name>
```

команда соединения изменений со сливаемой ветки в целевую.

#### Удаление веток

```sh
git branch -d <Branch_Name>
```
с ключом _**"-d"**_ команда удалит ветку только в том случае, если она полностью слита с одной из других веток. В противном случае, Git выдаст предупреждение, о том, что в ветке есть неслитые изменения, и не даст ее удалить.

```sh
git branch -D <Branch_Name>
```
с ключом _**"-D"**_ команда удалит ветку, игнорируя предупреждения Git, в любом случае, даже если в ней есть изменения, которые можно потерять.

## Справочная информация
1. [Официальная документация о системе контроля версий](https://git-scm.com/book/ru/v2/Введение-О-системе-контроля-версий "Справочное пособие на русском")
2. [Основные команды GIT и их применение](https://habr.com/ru/companies/ruvds/articles/599929/ "30 команд на хабр.ком")

![Проект GIT](git_branches_project.jpg)# Инструкция в системе контроля версий GIT

## Определение GIT
**GIT** - это консольная утилита, для отслеживания и ведения истории изменения файлов, в Вашем проекте.
## Установка и настройка GIT
Скачать установочный файл Git для Windows, MAC, Linux необходимо по ссылке: [GIT Download](https://git-scm.com/downloads "Установить GIT")

При первом использовании GIT необходимо указать имя и электронную почту пользователя. В противном случае GIT не позволит записывать индексированные изменения в репозиторий Git. Для ввода данных пользователя необходимо выполнить следующие команды:
>git config --global user.name<br>
>config --global user.email

Вместо *name* в команде требуется написать имя пользователя, а вместо *email* - адрес электронной почты пользователя.
## Инициализация локального репозитория
>**git init** - создание пустого репозитория в выбранной папке. После выполнения команды в папке появится скрытый файл с наименованием *.git*

После добавляются файлы в папку (проект), изначально имеющие неотслеживаемое GIT состояние - *Untracked*.

Состояние рабочего каталога и раздела проиндексированных файлов отображает команда: 
>**git status**

## Контроль изменения версий
Основным объектом в управлении контроля версий является <u>**коммит**</u>. Каждый последующий коммит добавляется после внесения изменений и с его помощью можно отследить информацию об этих изменениях.

Добавление коммита производится в 2 этапа:

1. Добавление файла или файлов к следующему коммиту, когда изменения фиксируются для включения в коммит (состояние *staged*). Можно использовать несколько способов вызова этого действия:
- >**git add -A** - добавить все файлы в состояние staged
- >**git add .** - добавить все файлы из текущей папки и всех внутренних.
- >**git add <file_name>** - добавляет только конкретный файл. Вместо **<file_name>** нужно указать имя файла.

2. Создание коммита с помощью команды, где вместо **"message"** вводится текст комментария

    >**git commit -m "message"**

Просмотр истории коммитов в ветке (указателе коммитов) осуществляется с помощью команды:

- >**git log** - выводится полный хэш-код (идентификатор) коммита

*или*

- >**git log --oneline** - отображает идентификатор версии файла в сокращённом формате

Для просмотра различия версий файла в разных коммитах, используется команда
>**git diff**

В конце команды нужно указать идентификатор коммита, если сравниваем с текущей версией, или два идентификатора, если сраниваем определённые предыдущие версии.

Для того, чтобы переключиться в нужную версию файла, нужно найти идентификатор коммита (сокращённый или полный) и указать его в конце команды.

```sh
git checkout
```

### Использование веток при работе с GIT

#### Понятие и применение веток
**Ветка** – это последовательность коммитов. Ветку можно рассматривать как временную шкалу. Коммиты в ней – снимки интересных моментов, идущие друг за другом в хронологической последовательности. 

С точки зрения внутренней реализации Git, **ветка** – это ссылка на последний коммит в этой ветке. Ветка-ссылка указывает на коммит, который является последним в "потоке" коммитов в данной ветке. 

Частые случаи использования веток:

+ одновременная работа нескольких программистов над одним проектом / файлом не мешая друг другу.
+ тестирование экспериментальных функций, когда создается новая ветка специально для экспериментов. Если эксперимент удался, изменения с экспериментальной ветки переносятся на основную, если нет – новая ветка удаляется
+ для разных выходящих параллельно релизов одного проекта.

#### Создание веток

```sh
git branch <branch_name>
```
команда проверяет наличие ветки и в случае отсутствия ветки с указанным наименованием создаёт новую.

#### Просмотр веток

При работе с большим количеством веток, можно забыть имя нужной, а без имени ветки переключиться на нее не получится. Для таких ситуаций существует команда просмотра списка веток: 
```sh
git branch
```
По умолчанию команда выводит список локальных веток. 

С ключом _**"-r"**_ можно вывести только удаленные ветки, c ключом _**"-a"**_ - и локальные, и удалённые ветки.

#### Переключение между ветками

```sh
git checkout <Branch_Name>
```
команда для перехода к указанной ветке.

Команда **git checkout** умеет создавать ветки и сразу переключаться на них. Это намного удобнее, чем выполнять два этих действия по отдельности. Поэтому данный способ является более предпочтительным, так как задействует только одну команду **git checkout** со специальным ключом _**-b**_.

По умолчанию все коммиты добавляются в ветку *master*, но внутри любого коммита можно создавать новые ветки коммитов. Тогда необходимо использовать команду
>**git checkout -b *<Branch_Name> <hashtag_commit>***

, где вместо ***<Branch_Name>*** указываем наименование новой ветки, а вместо ***<hashtag_commit>*** идентификатор коммита, в котором будет создаваться ветка

Если необходимо переключиться к актуальному состоянию файла и продолжить работу, то требуется указать команду
>**git checkout master**

#### Слияние веток

**Сливаемая ветка** – та ветка, с которой мы берем изменения, чтобы влить их в целевую.

**Целевая ветка** – та ветка, в которую мы сливаем наши изменения.

**Слияние веток** – это перенос изменений с одной ветки на другую. При этом слияние не затрагивает сливаемую ветку, то есть она остается в том же состоянии, что позволяет нам потом продолжить работу с ней.

```sh
git merge <Merged_branch_name>
```

команда соединения изменений со сливаемой ветки в целевую.

#### Удаление веток

```sh
git branch -d <Branch_Name>
```
с ключом _**"-d"**_ команда удалит ветку только в том случае, если она полностью слита с одной из других веток. В противном случае, Git выдаст предупреждение, о том, что в ветке есть неслитые изменения, и не даст ее удалить.

```sh
git branch -D <Branch_Name>
```
с ключом _**"-D"**_ команда удалит ветку, игнорируя предупреждения Git, в любом случае, даже если в ней есть изменения, которые можно потерять.

## Работа с удаленными репозиториями

Удалённые репозитории представляют собой версии вашего проекта, сохранённые в интернете или ещё где-то в сети. У вас может быть несколько удалённых репозиториев, каждый из которых может быть доступен для чтения или для чтения-записи. Взаимодействие с другими пользователями предполагает управление удалёнными репозиториями, а также отправку и получение данных из них.

### Добавление удалённых репозиториев

```sh
git remote add <название удаленного репозитория> <ссылка на удаленный репозиторий>
```

Имя удаленного репозитория в команде **git remote add** вы можете придумать сами. При работе с этим удаленным репозиторием, вы будете обращаться к нему по придуманному имени. 

Ссылку на удаленный репозиторий можно взять, нажав на большую зеленую кнопку Code на странице репозитория на GitHub.

### Клонирование удалённых репозиториев

Необходимость клонировать существующий удаленный репозиторий к себе на компьютер возникает в ситуациях, когда вы решаете поработать над уже существующим кодом. Для выполнения этой операции в Git предусмотрена команда **git clone**.

```sh
git clone <ссылка на удаленный репозиторий>
```
### Получение изменений из удаленного репозитория

Чтобы получать изменения и сразу обновлять рабочую копию так, чтобы она соответствовала удаленному репозиторию, нужно использовать команду **git pull**.

```sh
git pull [ключи] [имя удаленного репозитория]
```

Ключи

- >--ff – включить fast-forward, если это возможно, 
- >--no-ff – отключить fast-forward, 
- >--ff-only – остановить pull, если его невозможно сделать в fast-forward.

### Отправка изменений в удаленный репозиторий

Для загрузки изменений в удаленный репозиторий используется команда 
```sh
git push [ключи] [имя удаленного репозитория] [имя ветки]
```
Если слияние изменений в удаленном репозитории нельзя сделать в режиме fast-forward, и при этом не был передан ключ force, выполнение закончится с ошибкой.

Ключи

- >--all
Пушит все имеющиеся ветки

- >-f, --force
Перезаписывает удаленную ветку, вне зависимости от ее содержимого. Старайтесь не использовать этот флаг без крайней необходимости.

- >--force-with-lease
Удаляет все коммиты, которых нет в локальном репозитории. Если коммит, который команда соберется удалять был создан другим пользователем, то выполнение закончится с ошибкой.

### Создание форка репозитория на GitHub

_**Форк**_ (от англ. fork – вилка) – точная копия репозитория, но в вашем аккаунте. Форки нужны, чтобы вносить свои изменения в проект, к репозиторию которого у вас нет прямого доступа.

_**Пулл-реквест**_ (от англ. pull-request – запрос pull) – функция GitHub, позволяющая попросить владельца репозитория, от которого мы сделали форк, загрузить наши изменения обратно в свой репозиторий.

Форки и пулл-реквесты нужны, чтобы любой пользователь мог внести свой вклад в любой открытый проект, репозиторий которого есть на GitHub. Кроме того, перед тем как влить ваши изменения в основной репозиторий, ответственные обязательно проверят ваш код на наличие ошибок и уязвимостей. 

1. Зайдите на страницу репозитория проекта. Нажимаем на кнопку **Fork**. После этого Git создаст точную копию этого репозитория в вашем аккаунте.
2. Клонируем репозиторий к себе на компьютер командой **git clone**. Создадим файл _README.md_ с описанием проекта, чтобы другим пользователям было понятно, в чем отличие этой реализации от остальных.
3. Сделаем коммит и выполним **git push**, чтобы загрузить наши изменения в удаленный репозиторий.
4. GitHub подсказывает нам, что наша ветка опережает ветку исходного репозитория на один коммит и предлагает сделать пулл-реквест.
5. Нажимаем на кнопку **Compare** на подсказке GitHub, либо переходим на вкладку **Pull Requests** и нажимаем **New pull request**.
6. Перед нами откроется страница создания пулл-реквеста.

Здесь мы можем просмотреть внесенные изменения и выбрать две ветки: одну в исходном репозитории, на нее будут залиты наши изменения, вторую – в нашем репозитории, с нее будут скачаны изменения. Как только мы выбрали ветки и убедились, что не внесли никаких лишних изменений, нажимаем кнопку **Create pull request**.

7.  Попадаем на страницу описания наших изменений. Необходимо описать, что за изменения вы внесли и почему они были необходимы. Сообщение должно отражать суть и необходимость внесенных изменений. Как только мы закончили с описанием, можно нажимать кнопку **Create pull request**.

8. Теперь мы попадаем на страницу уже созданного пулл-реквеста в изначальном репозитоии. Именно так будет выглядеть наш пулл-реквест и для владельца репозитория. На этой странице он сможет писать комментарии, указывая на ошибки или задавая вопросы. После того, как владелец репозитория просмотрит наши изменения и убедится, что они не имеют вредоносный характер, он сможет принять наш пулл-реквест. Тогда все изменения, добавленные в этот пулл-реквест нами, будут залиты в исходный репозиторий.

## Справочная информация
1. [Официальная документация о системе контроля версий](https://git-scm.com/book/ru/v2/Введение-О-системе-контроля-версий "Справочное пособие на русском")
2. [Основные команды GIT и их применение](https://habr.com/ru/companies/ruvds/articles/599929/ "30 команд на хабр.ком")

![Проект GIT](git_branches_project.jpg)