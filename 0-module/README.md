# Введение

Добро пожаловать в задачник курса **Node.JS**!
В этом разделе мы разберемся что такое задачник и как с ним начать работать.

## Необходимые инструменты

Ниже приведен список программ, которые нужно установить и с которым вы будете работать при решении 
задач из задачника.

### Учетная запись на Github

Весь код домашних заданий, а также ваших решений будет находится в [Github](https://github.com/), 
поэтому первым делом необходимо там зарегистрироваться.

### Система контроля версий (GIT)

Git — это инструмент без которого сложно представить себе современную разработку. Если вы с ней еще 
не знакомы — ничего страшного, нам нужны только самые основные операции, которые запомнить 
достаточно просто, но которые при этом используются в 95% случаев.
Скачать и установить git можно с [официального сайта](https://git-scm.com/downloads).

### NodeJS

Мы будем изучать NodeJS, так что без нее нам никак не обойтись!
Самый простой способ установить Node.JS — скачать дистрибутив с последней версией на 
[официальном сайте](https://nodejs.org/en/download/) и запустить его.

### Консоль

Для выполнения различных команд git, а также для запуска Node.JS нам понадобится специальная 
программа, которая по умолчанию есть во всех операционных системах. В Linux и MacOS она называется 
`terminal`, в Windows - `cmd` или `командная строка`.

### Редакторы кода

Если у вас уже есть любимый редактор кода — вы можете вести разработку в нем. Ниже представлены на 
выбор несколько редакторов кода, в которых вы можете удобно решать задачи курса. Все редакторы в 
списке ниже являются кроссплатформенными, т.е. будут работать в любой операционной системе.

#### Atom (бесплатно)

Atom — бесплатный редактор кода с очень богатыми возможностями по настройке и большим количеством 
готовых плагинов. Установить можно с [официального сайта](https://atom.io/).

#### VS Code (бесплатно)

Visual Studio Code — стремительно набирающий популярность бесплатный редактор кода от Microsoft, 
которым пользуется огромное количество разработчиков по всему миру. Установить можно с 
[официального сайта](https://code.visualstudio.com/).  

#### WebStorm (бесплатно в течение 30 дней)

WebStorm — это среда разработки, предоставляющая не только редактор кода, но и встроенные утилиты 
для анализа, рефакторинга и отладки кода. Купить и установить эту программу можно на официальном 
сайте ее разработчика — [компании JetBrains](https://www.jetbrains.com/webstorm/).

## Первые шаги

На данном этапе у вас установлены все необходимые инструменты и есть учетная запись на 
[github.com](https://github.com). 

Самое время разобраться с тем как с этим начать работать!

### Форк задачника

В начале курса для каждого студента создается репозиторий с задачником на Github. Он будет содержать
все задачи курса, а также в этот репозиторий вы будете отправлять решения задач. Но напрямую с 
задачником работать не выйдет, вначале вам необходимо создать его форк в своем личном профиле, и всю
разработку вести там. 

Для создания форка достаточно нажать на кнопку `Fork`, которая находится на странице репозитория на 
Github. 

### Настройка git

На этом шаге наша задача получить fork репозитория из Github на свой компьютер для того, чтобы 
начать разработку. Для этого необходимо выполнить следующие шаги:

1. Узнать ссылку на fork репозитория. Сделать это можно в интерфейсе github нажав на кнопку 
`Clone or download`.  
2. Выбрать на компьютере папку, в которой будет находиться папка с исходным кодом.
3. В консоли перейти в эту папку и выполнить команду:
```git clone <ссылка на ваш fork>```
4. После успешного выполнения предыдущей команды в выбранной папке должна появится еще одна, 
содержащая fork
5. В консоли необходимо перейти в папку с fork'ом и выполнить команду:
```npm install```
6. Для синхронизации изменений между задачником и fork'ом вам потребуется скачивать их на свой 
компьютер. В интерфейсе github скопируйте ссылку на странице задачника (не fork'a, а оригинального 
репозитория!).
7. Далее в консоли необходимо в папке с fork'ом выполнить следующую команду:
```git remote add upstream <ссылка на репозиторий с задачником>```
8. Чтобы проверить все ли действия были выполнены правильно выполните следующую команду:
```git remote -v```
В выводе должны быть 4 строки, 2 origin и 2 upstream. У строк с origin в начале должна быть ссылка 
на fork, а у upstream - на оригинальный задачник. Например:
```
$ git remote -v
    origin	git@github.com:student/nodejs-20190329_student.git (fetch)
    origin	git@github.com:student/nodejs-20190329_student.git (push)
    upstream	git@github.com:js-tasks-ru/nodejs-20190329_student.git (fetch)
    upstream	git@github.com:js-tasks-ru/nodejs-20190329_student.git (push)
```


### Добавление задачи

Добавление задачи осуществляется со страницы задачи на сайте. Выберите интересующий вас модуль, 
далее нужную задачу и нажмите на кнопку "Добавить". Через некоторое время задача будет добавлена в 
задачник на github. Для того, чтобы начать работу над задачей вам надо "скопировать" все изменения 
из репозитория задачника в свой fork. Для этого в консоли надо перейти в папку с исходным кодом 
(которую вы создали на предыдущем шаге) и выполнить следующую команду:
```git pull upstream master```

### Решение задачи

Весь задачник разбит на папки, каждая из которых содержит информацию о соответствующем занятии. В 
каждой папке есть краткий материал по занятию и ссылки на материалы с дополнительной информацией по 
теме. Задачи находятся тут же, в папках с названием `*-task`. В папке задачи есть условие задачи с 
инструкцией по выполнению, а также файлы тестов для самопроверки. Именно в папке с задачей 
необходимо выполнять решение.

По ходу решения вы можете пользоваться автоматическими тестами, которые помогут понять удовлетворяет
ли ваше решение условиям задачи или нет. Для запуска тестов необходимо **находясь в корневой папке 
задачника** выполнить команду `npm run test:local 0-module 1-task`, где `0-module` и `1-task` - 
название модуля и задачи соответственно.

*Обратите внимание, что в ходе решения вы **не можете менять файлы тестов, а также файл с условием 
задачи**. Разрешается менять только файлы с кодом задачи, при необходимости вы можете создавать 
новые файлы.*

### Возврат решения на проверку

Если вы добились того, что команда `npm run test:local 0-module 1-task` говорит вам, что решение 
верное — самое время выкладывать его для проверки на Github.

Для этого необходимо выполнить следующие действия:

1. Добавить измененные файлы в git и создать коммит (с описанием того, что было сделано)
```git commit -am "add solution for 1-module 1-task"```
2. Отправить изменения на Github
```git push origin master```
3. Зайти на страницу репозитория в Github и **создать Pull Request**.
4. Система атоматически проверит ваше решение и если в нем нет ошибок, то все изменения попадут в 
ветку master, а на сайте задача будет помечена как решеная.

### Синхронизация fork'а и задачника

Не забывайте синхронизировать свой fork с оригинальным репозиторием задачника - после добавления задач 
выполняйте команду:
```git pull upstream master```

### Подсмотреть решение

Если вы хотите посмотреть решение добавленной задачи вы можете это сделать на странице задачи в 
учебнике, нажав на кнопку "получить решение". 
