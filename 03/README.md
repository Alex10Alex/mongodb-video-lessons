



###Операторы для чисел.
Перевод температуры из Цельсия в Фарингейт.
Формула перевода tF = tC * 1.8 + 32

`db.weather.aggregate([{
`  $project: {
`    tK: {
`      $add: [32, {$multiply: ["$temperature", 1.8]}]}
`    }
`}])


###Операторы для дат
`db.posts.aggregate([{$project: {date: {$dayOfWeek: "$date"}}}])

###Операторы для строк
`db.posts.aggregate([{$project: {text: {$concat: ["Заголовок: <", "$title", ">. ", 'Содержание : ', "$body"]}}}])

#Выбор данных

##Группировка

###Средняя температура по месяцам
```
db.weather.aggregate([{
  $group: {
    _id: {year: "$year", month: "$month"}, 
    temperature: {$avg: "$temperature"}
  }
}])
```
#Средняя температура по месяцам летом

```
db.weather.aggregate([
  { 
    $group: { 
      _id: {year: "$year", month: "$month"}, 
      temperature: {$max: "$temperature"} }
  }, 
  { 
    $match: {"_id.year": 2014, "_id.month": {$in: [6,7,8]}}
  } 
])
```

#Правда ли что средняя температура весной больше средней температуры осенью

```
db.weather.aggregate([
  { $match: {month: {$in: [3,4,5,9,10,11]}} }, 
  { $project: {isSpring: {$or: [{$lte: ["$month", 5]}]}, temperature: true } },
  { $group: {_id: "$isSpring", t: {$avg: "$temperature"}} }
])
```

###unwind

#какой тег встречается чаще всего

```
db.posts.aggregate([
  {$project: {tags: true}}, 
  {$unwind: "$tags"}, 
  {$group: {_id: "$tags", sum: {$sum: 1}}}, 
  {$sort: {sum: -1}}, 
  {$group: {_id: null, tag: {$first: "$_id"}, count: {$first: "$sum"}}}
])
```
