# calendar

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

Необходимо разработать на vuejs v2 компонент-календарь (без копирования кода из других источников).
Нужно сделать возможность переключения месяцев, а так же выбора конкретной даты при клике.
На клик по дню повесить событие, которое будет возвращать выбранную дату.
При инициализации компонент может принимать свойство даты в формате "год-месяц-день" и переключать текущий месяц и день на нее. Если дата не передана, то берем текущий день.
Реализовать возможность смены языка (название месяцев, дней недели).
