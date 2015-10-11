
```
db.posts.find({},{date:true})
```

## Стадия $match

```
db.posts.aggregate([ 
  { $match: {date: {$gte: "2015", $lt: "2016"}} } 
]).pretty()
```

## Стадия $project

```
db.posts.aggregate([ 
  {$match: { date: {$gte: "2015"}, date: {$lt: "2016"} }},
  {$project: {title: "$title", isDraft: "$options.isDraft"}} 
])
```

###Операции для чисел.
Перевод температуры из Цельсия в Фарингейт.
Формула перевода tF = tC * 1.8 + 32

```
db.weather.aggregate([{
  $project: {
    tF: {
      $add: [32, {$multiply: ["$temperature", 1.8]}]}
    }
}])
```

###Операции для строк
```
db.weather.aggregate([
  { $project: {date: {$concat: [ {$substr: ["$year",0,4]}, '-', {$substr: ["$month",0,2]}, '-', {$substr: ["$day",0,2]} ]}}}
])
```

#Выбор данных

##Группировка

###Максимальная температура по годам
```
db.weather.aggregate([
  {$group: {_id: "$year", maxT: {$max: "$temperature"}}}
])	
```

###Максимальная температура по месяцам
```
db.weather.aggregate([
  {$group: {_id: "$month", maxT: {$max: "$temperature"}}}
])	
```

### Массив измерений по дням

```
db.weather.aggregate([
  {$group: {_id: {year: "$year", month: "$month", day: "$day"}, t: {$push: "$temperature"}}}
])
```

### Самый холодный день в году
```
// такой метод не работает
db.weather.aggregate([
  {$group: {_id: false, t: {$min: "$temperature"}}}
])	

db.weather.aggregate([
  {$sort: {temperature: 1}},
  {$group: {_id: false, year: {$first: "$year"}, month: {$first: "$month"}, day: {$first: "$day"}, temperature: {$first: "$temperature"} }}])
```

##unwind

###какой тег встречается чаще всего

```
db.posts.aggregate([
  {$project: {tags: true}}, 
  {$unwind: "$tags"}, 
  {$group: {_id: "$tags", sum: {$sum: 1}}}, 
  {$sort: {sum: -1}}, 
  {$group: {_id: null, tag: {$first: "$_id"}, count: {$first: "$sum"}}}
])
```
