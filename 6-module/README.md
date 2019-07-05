# Mongoose и MongoDB

## MongoDB

MongoDB это популярная документо-ориентированная база данных. Для упрощения можно представить что данные там храняться в виде JSON-a. На самом деле это не совсем так, и данные храняться несколько иначе, но в данный момент это не важно. 
Существует 2 версии - Community Edition и Enterprise Edition. Вторая отличается большим количеством возможностей и поддержкой от разработчиков, но при этом она платная. Для наших задач (как и в принципе многих задач реальных проектов) возможностей бесплатной версии хватает с головой
Разработчики MongoDB предоставляют библиотеки для работы с базой данных для популярных языков программирвоания, в т. ч. и для NodeJS

Документация:
- Официальная документация https://docs.mongodb.com/manual/
- Инструкция по установке: https://docs.mongodb.com/manual/administration/install-community/ \
  _Кроме официального способа установки, когда сервер базы данных устанавливается в операционную систему, вы можете использовать либо [облачное решение](https://mlab.com/) (бесплатно предоставляется 500МБ хранилища) или установить базу данных используя Docker-образ_ 
- Документация по основным командам БД https://docs.mongodb.com/manual/crud/
- Документация по родной библиотеке для работы с БД: http://mongodb.github.io/node-mongodb-native/3.1/quick-start/quick-start/
    
## Mongoose

В рамках курса мы будем работать с базой данных используя Object Document Mapper (ODM) Mongoose.
Это специальная библиотека, которая позволяет вам работать с записями базы данных как с объектами, добавлять методы для работы с этими данными прямо в модель данных и сохранять. 

Документацию по Mongoose вы можете найти тут: https://mongoosejs.com/

# Примеры использования Mongoose

Для использования Mongoose необходимо сначала подключить библиотеку и создать подключение к базе данных.

_Тут предполагается что сервер базы данных уже у вас установлен и запущен, база данных находится по адресу `localhost:27017` (адрес по умолчанию при установке базы данных в систему)_

```js
const mongoose = require('mongoose');
const connection = mongoose.createConnection('mongodb://localhost:27017', {userNewUrlParser: true});
```

Функцию `createConnection` достаточно вызвать 1 раз при создании приложения и в дальнейшем использовать созданное соединение. 

_Параметр `userNewUrlParser` указыватет для низкоуровневой библиотеки работы с MongoDB использовать новую версию парсера 
адрессной строки. Если этот параметр не указать, то в консоли будет показано предупреждение, но на работоспособность 
пока это не повлияет_

## Декларация схемы и модели данных

Для начала объявляется схема модели данных, где описывается какие есть аттрибуты и какие типы данных, а так же можно 
добавить дополнительные опции в виде индексов, валидации и прочее - https://mongoosejs.com/docs/guide.html

```js
const userSchema = new mongoose.Schema({
  // простое указание свойства и его типа, когда нет необходимости указывать другие парамтеры
  name: String, 
  dateOfBirth: Date, 
  
  // расширенная форма описания свойства модели, когда кроме типа нужно указать другие параметры
  // в данном случае unique:true создаст в MongoDB уникальный индекс по полю и база не позволит создать 
  // 2 записи с одинаковым заначением поля `login` 
  login: { 
    type: String,
    unique: true,
    required: true
  },
  
  // так же можно указать что поле является массивом в данном случае предполагается, 
  // что роль пользователя может описываться массивом значений, например, ['admin', 'author']
  role: [String],
  
  // модель может иметь и вложенные объекты, которые описываются 
  // по такому же принципу как и основная модель
  blocked: { 
    // для аттрибутов модели (в т. ч. и для вложенных документов можно указать значения по умолчанию
    isBlocked: {
      type: Boolean,
      default: false,
    },
    date: Date,
  }
  
})
```
Более полную информацию о типах данных, которые вы можете использовать при описании схемы, и их опциях вы можете найти [тут](https://mongoosejs.com/docs/schematypes.html)

После того, как схема определена необходимо создать модель

```js
const User = connection.model('User', userSchema);
```

В дальнейшем все взаимодействие с моделью, как с объектом должны происходить через `User`

## Создание записи и сохранение в БД

Для создания записи существует 2 способа:

1. Создать объект пользователя явно указать аттрибуты, которые необходимо изменить
```js
const user = new User();
user.name = "Jon";
user.dateOfBirth = new Date(1980, 1);
user.login = "Jon_Snow";
user.role = ['user'];
user.blocked.isBlocked = false;
```

2. Передать необходимые поля в конструктор
```js
const user = new User({
    name: "Jon",
    login: "Jon_Snow",
    role: ["user"],
    blocked: {
      isBlocked: false,
    }
});
```

После того, как объект создан в памяти приложения и надполненб необходимо его сохранить.
Для этого у экземпляра класса модели есть метод `save`. Его можно использовать двумя способами: через callback или через Promise

1. Callback
```js
  user.save((err, user) => {
    // err - если произошла ошибка, первым аргументом будет объект ошибки, иначе - undefined
    // user - сохраненный объект пользователя 
  });
```

2. Promise
```js
  user.save()
  .then(user => {
    // ...
  })
  .catch(err => {
    // ...
  });
  
  // или
  async function createUser() {
    //...
    await user.save()
    // ... 
  }
```

Все методы, которые работают с базой данных поддерживают как и Callback API, так и Promise API
В дальнейшем все примеры будут с использованием Promise API.

### ID записи

В MongoDb идентификатором записи по умолчанию является автоматически сгенерированный `ObjectId`, 
который выглядит примерно так `4d5ea62fd76aa1136000000c`, каким образом формируется это значение
можно почитать [тут](https://docs.mongodb.com/manual/reference/bson-types/#objectid)
В документе в MongoDB идентификатор записи сохраняется в поле `_id` и имеет внутренний тип `ObjectId`

После того как объект создан вы можете получить значение его `id` следующим образом:

```js
const stiringUserId = user.id; // в таком случае вам вернется строка со значением идентификатора
const userObjectId = user._id; // а в таком - объект класса ObjectId, который хранит в себе идентфикатор записи
```
Кроме случаев, когда вам надо явно оперировать с `ObjectId`, рекомендую использовать свойство `id`, 
что бы не столкнуться с неправильно работающим сравнением объектов по `id`

Так же хочу обратить внимание, что идентификатор генерируется в библиотеке и вам не надо сохранять запись что бы его получить.

Вы можете в качестве ID использовать свое значение, указав его в аттрибуте `_id`. 
В таком случае необходимо следить что бы значения были уникальными и всегда присутствовало (иначе MongoDB автоматически добавит ObjectId)

## Поиск записи в БД

В Mongoose существует 3 метода для поиска записей в базе данных

### `Model.find(condition, projection, options, callback)`:
Наиболе общий метод, позволяет задать условия поиска, количество возвращаемых записей, условия сортировки и так далее.

#### `condition`
В этом параметре указывается условие поиска, в виде обычного JavaScript объекта, например:
```js
    { 
        name: "Jon"
    }
```
вернет все записи где поле `name` равно `"Jon"`

Кроме того есть ряд операторов MongoDB, которые так же можно использовать при составлении запроса, обычно они начинаются с символа $:
```js
    {
        dateOfBirth: { 
            $gte: new Date(1990, 0) 
        }
    }
```
Информацию про доступные операторы вы можете найти [тут](https://docs.mongodb.com/manual/reference/operator/query/)

#### `projection`
Строка или объект который определяет какие атрибуты исключить из ответа из базы данных, или какие получить
Строка:
`name login`: означает выбрать только атрибуты `name` и `login`, остальные атрибуты из базы не будут получены
`-name -login`: будет получено из базы все кроме `name` и `login`

Объект:
`{name:1, login:1}`: аналогично `name login`:
`{name:0, login:0}`: аналогично `-name -login`: 

Необходимо указывать либо поля которые добавить, либо которые исключить, смешивать нельзя.

#### `options`
Тут вы можете указать ряд параметров запроса, например такие как `sort`, `limit`, `skip` и [другие](https://mongoosejs.com/docs/api.html#query_Query-setOptions)

#### `callback`
Функция обратного вызова, которая будет вызвана при получении ответа от базы данных 


Например следующий запрос вернет 10 пользователей, которые имеют роль `user` начиная с 11-й записи, 
отсортированных в порядке убывания даты рождения 

```js
User.find({role:'user'}, 'name', {sort:{dateOfBirth:1}, limit: 10, skip: 10})
.then(users => {
  // ...
})
```

### `Model.findOne(condition, projections, options, callback)`
Этот метод аналогичен предыдущему методу, за исключением того, что он возвращает первую найденную запись, 
которая соответствует условиям поиска

### `Model.findById(id, projection, options, callback)`
Это сокращенный метод для поиска записи по ее идентификатору. В качестве аргумента `id` можно передать строку, число или `ObjectId`

```js
User.findById('4d5ea62fd76aa1136000000c')
.then(user => {
  // ...
})
```

## Изменение свойств записи

### Pre- post- хуки

## Работа со связанными моделями

## Работа с индексами

## Удаление записи

## Дополнительные ссылки 
