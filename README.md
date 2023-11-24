# Домашняя работа по теме "Базовые понятия MongoDB, CRUD, фильтры"
В качестве dataset используется [Earth Meteorite Landings](https://data.nasa.gov/resource/y77d-th95.json)
### Создать новую коллекцию
![New Collection](./01.png?raw=true)
### Импортируем данные через MongoDB Compass
![MongoDB Compass](./02.png?raw=true)
### Прблема, которая возникла, - некорректный поиск по строковому значению в поле mass. Чтобы избежать этого, изменим тип на double:
```
db.meteorites.updateMany(
  {mass: {$gte: ''}},
  [{$set: {
      mass: {
        $toDouble: '$mass'
      }
  }}]
)
```
## Запросы на выборку:
### Вывести все метеориты, упавшие с (>=) 2010 по (<) 2020 годы:
```
db.meteorites.find( { $and: [ {"year": { $gte: '2010' } }, { "year": { $lt: '2020'} } ] } )
```
![DB](./03.png?raw=true)
### Подсчитать количество таких метеоритов:
```
db.meteorites.countDocuments( { $and: [ {"year": { $gte: '2010' } }, { "year": { $lt: '2020'} } ] } )
```
![DB](./04.png?raw=true)
### Найти количество метеоритов, упавших до 2000 года и массой от 200 до 400 кг включительно.
```
db.meteorites.countDocuments( { $and: [ {"year": { $lt: '2000' }, $and: [ {"mass": { $gte: 200 } }, { "mass": { $lte: 400} } ] } ] } )
```
![DB](./05.png?raw=true)
### Найти сумму масс всех метеоритов, упавших на Землю:
```
db.meteorites.aggregate( [ { $group: { _id: null, sum_mass: { $sum: "$mass" } }  } ] )
```
### Отсортировать вывод метеоритов, упавших с (>=) 2010 по (<) 2020 годы по возрастанию массы:
```
db.meteorites.find( { $and: [ {"year": { $gte: '2010' } }, { "year": { $lt: '2020'} } ] } ).sort({"mass": 1})
```
## Запросы на обновление
### Поменять массу у метеорита
```
db.meteorites.update( { "name": { $regex: "^Chelyabinsk" } }, { $set: { "mass": 203203 } }, { multi: true } )
```
![DB](./06.png?raw=true)
## Запросы на удаление

### Удалить метеориты, массой меньше одной тонны
```
db.meteorites.deleteMany( {"mass": { $lte: 1000 } } )
```
![DB](./07.png?raw=true)
