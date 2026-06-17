### Зачем нужен useEffect

- Асинхронные действия - запросы, таймеры
- Добавление/снятие обработчиков событий/подписок
- Манипуляции с рефами на DOM-элементы
- Асинхронная отправка метрик
- Асинхронная подгрузка статики - стили, переводы, скрипты

### Когда useEffect не нужен

- Трансформация данных для рендеринга
- Обработка пользовательских действий
- Получение данных с библиотеками TanStack Query, SWR, RTK Query, Apollo, Relay и пр.

### Анатомия useEffect

```javascript
useEffect(
  () => {
    // Эффект

    return () => {}; //cleanUp функция
  },
  [/* Зависимости */], // Помним про мемоизацию
);
```

```javascript
useEffect(() => {
  const handleEscapeKey = () => {};
  document.addEventListener("keyup", handleEscapeKey);

  return () => {
    document.removeEventListener("keyup", handleEscapeKey);
  };
}, []);
```

### componentDidMount

> [!NOTE]
> В режиме StrictMode вызывается дважды

```mermaid
graph LR
A[componentDidMount] --> B([Вызов эффекта])
B --- C([Возвращение cleanUp функции])
C --> D[end]
```

### componentDidUpdate

> [!NOTE]
> Для использования в чистом виде, нужен паттерн isMounted

```mermaid
graph LR
A[componentDidUpdate] --> B{Обновились<br>зависимости?}
B -->|Нет| C[end]
B -->|Да| D{Возвращалась ли<br>функция cleanUp?}
D -->|Нет| E([Вызов эффекта<br>+<br>Возвращение cleanUp])
D -->|Да| F([Вызов<br>cleanUp])
F --> E
E --> C
```

### componentWillUnmount

```mermaid
graph LR
A[componentWillUnmount] --> B{Возвращалась ли<br>функция cleanUp?}
B -->|Да| C([Вызов<br>cleanUp])
B -->|Нет| D[end]
C --> D
```

Источник https://www.youtube.com/watch?v=ivtRckdgfts
