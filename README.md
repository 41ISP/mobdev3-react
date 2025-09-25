# React - Полная памятка разработчика

React - это JavaScript библиотека для создания пользовательских интерфейсов, разработанная Facebook. Основана на концепции компонентов и виртуального DOM.

## 🚀 Инициализация проекта через Vite

```bash
# Создать новый React проект с Vite (быстрый сборщик)
npm create vite@latest
cd my-react-app
npm install
npm run dev
```

**Зачем Vite?** - Быстрый запуск разработки, мгновенная горячая перезагрузка, оптимизированная сборка.

## 📦 Структура проекта

```
my-react-app/
├── src/
│   ├── components/          # Главный компонент приложения
│   │   └── Button/          # Компонент кнопки
│   │       ├── Button.jsx          # Компонент кнопки
│   │       └── Button.css          # Стили кнопки
│   ├── App.jsx          # Главный компонент приложения
│   ├── main.jsx         # Точка входа в приложение
│   ├── components/      # Папка для компонентов
│   └── assets/          # Статические файлы
├── public/              # Публичные файлы
├── index.html           # HTML шаблон
└── package.json         # Зависимости и скрипты
```

## 🧩 Основы компонентов

### Функциональный компонент (современный подход)

```jsx
// Простейший компонент
function Welcome() {
  return <h1>Привет, мир!</h1>;
}

// Компонент со стрелочной функцией (предпочтительно)
const Welcome = () => {
  return <h1>Привет, мир!</h1>;
};

// Экспорт компонента
export default Welcome;

// Именованный экспорт
export const Welcome = () => <h1>Привет, мир!</h1>;
```

### Создание и подключение компонентов

```jsx
// components/Button.jsx
const Button = ({ text, onClick, disabled = false }) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {text}
    </button>
  );
};

export default Button;

// Другой файл!
// App.jsx - подключение компонента
import Button from './components/Button';
import { Welcome } from './components/Welcome'; // именованный импорт

const App = () => {
  return (
    <div>
      <Welcome />
      <Button text="Нажми меня" onClick={() => alert('Клик!')} />
    </div>
  );
};
```

## 🎯 Props - передача данных в компоненты

### Обычная передача props

```jsx
const UserCard = (props) => {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>Возраст: {props.age}</p>
      <p>Email: {props.email}</p>
    </div>
  );
};

// Использование
<UserCard name="Иван" age={25} email="ivan@email.com" />
```

### Деструктуризация props (рекомендуется)

```jsx
// Деструктуризация в параметрах функции
const UserCard = ({ name, age, email, isActive = false }) => {
  return (
    <div className={isActive ? 'active' : 'inactive'}>
      <h2>{name}</h2>
      <p>Возраст: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
};

// Как передавать в компонент
const Users = () => {
  const UserData = {
    name, age, email, isActive = false }
  return (
    <Users {...
  )
};

```

## 🔄 useState - управление состоянием

### Основы useState

```jsx
import { useState } from 'react';

const Counter = () => {
  // [текущее значение, функция для обновления]
  const [count, setCount] = useState(0); // начальное значение: 0
  
  return (
    <div>
      <p>Счетчик: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
    </div>
  );
};
```

### useState с разными типами данных

```jsx
const StateExamples = () => {
  // Число
  const [number, setNumber] = useState(0);
  
  // Строка
  const [text, setText] = useState('');
  
  // Булево значение
  const [isVisible, setIsVisible] = useState(false);
  
  // Массив
  const [items, setItems] = useState(['item1', 'item2']);
  
  // Объект
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  // Null/undefined
  const [data, setData] = useState(null);
  
  return (
    <div>
      <p>Число: {number}</p>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => setIsVisible(!isVisible)}>
        {isVisible ? 'Скрыть' : 'Показать'}
      </button>
    </div>
  );
};
```

### Стрелочная функция в set функции

```jsx
const FunctionalUpdates = () => {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    // НЕПРАВИЛЬНО: может привести к проблемам при множественных обновлениях
    setCount(count + 1);
    setCount(count + 1); // count все еще 0, результат будет 1, не 2
    
    // ПРАВИЛЬНО: используем функцию, получающую предыдущее значение
    setCount(prev => prev + 1);
    setCount(prev => prev + 1); // правильно получим +2
  };
  
  // Для объектов и массивов
  const [user, setUser] = useState({ name: 'Иван', age: 25 });
  
  const updateAge = () => {
    setUser(prevUser => ({
      ...prevUser,    // копируем существующие свойства
      age: prevUser.age + 1  // обновляем только age
    }));
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment Twice</button>
      <p>{user.name}, возраст: {user.age}</p>
      <button onClick={updateAge}>Увеличить возраст</button>
    </div>
  );
};
```

## 📝 Controlled Components (Управляемые компоненты)

### Один управляемый input

```jsx
const SingleInput = () => {
  const [value, setValue] = useState('');
  
  return (
    <div>
      <input 
        type="text"
        value={value}                              // значение из state
        onChange={(e) => setValue(e.target.value)} // обновление state
        placeholder="Введите текст"
      />
      <p>Вы ввели: {value}</p>
    </div>
  );
};
```

### Форма с объектом в state

```jsx
const UserForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: '',
    gender: '',
    subscribe: false
  });
  
  // Универсальная функция для обновления любого поля
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Отправлена форма:', formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        type="text"
        value={formData.name}
        onChange={handleChange}
        placeholder="Имя"
      />
      
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      
      <input
        name="age"
        type="number"
        value={formData.age}
        onChange={handleChange}
        placeholder="Возраст"
      />
      
      <select name="gender" value={formData.gender} onChange={handleChange}>
        <option value="">Выберите пол</option>
        <option value="male">Мужской</option>
        <option value="female">Женский</option>
      </select>
      
      <label>
        <input
          name="subscribe"
          type="checkbox"
          checked={formData.subscribe}
          onChange={handleChange}
        />
        Подписаться на рассылку
      </label>
      
      <button type="submit">Отправить</button>
    </form>
  );
};
```

## ⚡ useEffect - побочные эффекты

### Основы useEffect

```jsx
import { useState, useEffect } from 'react';

const EffectExamples = () => {
  const [count, setCount] = useState(0);
  const [data, setData] = useState(null);
  
  // Выполняется после каждого рендера (аналог componentDidUpdate)
  useEffect(() => {
    document.title = `Count: ${count}`;
  });
  
  // Выполняется только один раз после первого рендера (аналог componentDidMount)
  useEffect(() => {
    console.log('Компонент смонтирован');
    
    // Функция очистки (аналог componentWillUnmount)
    return () => {
      console.log('Компонент размонтирован');
    };
  }, []); // пустой массив зависимостей
  
  // Выполняется только при изменении count
  useEffect(() => {
    if (count > 0) {
      console.log(`Count изменился: ${count}`);
    }
  }, [count]); // массив зависимостей с count
  
  // Загрузка данных
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Ошибка загрузки:', error);
      }
    };
    
    fetchData();
  }, []);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
};
```

### Очистка эффектов (cleanup)

```jsx
const TimerComponent = () => {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    
    // Очистка интервала при размонтировании компонента
    return () => clearInterval(interval);
  }, []);
  
  return <p>Прошло секунд: {seconds}</p>;
};
```

## 🗂️ Рендер списков (map)

### Простой список

```jsx
const TodoList = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Изучить React', completed: false },
    { id: 2, text: 'Написать код', completed: true },
    { id: 3, text: 'Деплой приложения', completed: false }
  ]);
  
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>  {/* key обязателен для списков */}
          <span style={{ 
            textDecoration: todo.completed ? 'line-through' : 'none' 
          }}>
            {todo.text}
          </span>
        </li>
      ))}
    </ul>
  );
};
```

### Рендер компонентов через map

```jsx
const TodoItem = ({ todo, onToggle, onDelete }) => {
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{ 
        textDecoration: todo.completed ? 'line-through' : 'none' 
      }}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>Удалить</button>
    </li>
  );
};

const TodoApp = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Изучить React', completed: false },
    { id: 2, text: 'Написать код', completed: true }
  ]);
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };
  
  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={toggleTodo}
          onDelete={deleteTodo}
        />
      ))}
    </ul>
  );
};
```

## 🎛️ Условный рендеринг

```jsx
const ConditionalRendering = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [user, setUser] = useState(null);
  
  return (
    <div>
      {/* Тернарный оператор */}
      {isLoggedIn ? (
        <p>Добро пожаловать!</p>
      ) : (
        <p>Пожалуйста, войдите</p>
      )}
      
      {/* Логическое И (&&) */}
      {user && <p>Привет, {user.name}!</p>}
      
      {/* Множественные условия */}
      {isLoggedIn && user ? (
        <UserDashboard user={user} />
      ) : isLoggedIn ? (
        <LoadingSpinner />
      ) : (
        <LoginForm />
      )}
    </div>
  );
};
```

## 🔄 Продвинутые паттерны

### Custom Hooks (пользовательские хуки)

```jsx
// Пользовательский хук для локального хранилища
const useLocalStorage = (key, initialValue) => {
  const [value, setValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });
  
  const setStoredValue = (value) => {
    try {
      setValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [value, setStoredValue];
};

// Использование
const App = () => {
  const [name, setName] = useLocalStorage('name', '');
  
  return (
    <input
      value={name}
      onChange={(e) => setName(e.target.value)}
      placeholder="Ваше имя"
    />
  );
};
```

### Context API - глобальное состояние

```jsx
import { createContext, useContext, useState } from 'react';

// Создание контекста
const ThemeContext = createContext();

// Провайдер контекста
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Хук для использования контекста
const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme должен использоваться внутри ThemeProvider');
  }
  return context;
};

// Компонент, использующий контекст
const Header = () => {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header style={{ backgroundColor: theme === 'light' ? '#fff' : '#333' }}>
      <button onClick={toggleTheme}>
        Переключить на {theme === 'light' ? 'темную' : 'светлую'} тему
      </button>
    </header>
  );
};

// Использование
const App = () => (
  <ThemeProvider>
    <Header />
  </ThemeProvider>
);
```

## 🚀 Оптимизация производительности

### React.memo - мемоизация компонентов

```jsx
import { memo } from 'react';

// Компонент будет ре-рендериться только при изменении props
const ExpensiveComponent = memo(({ data, onAction }) => {
  console.log('ExpensiveComponent рендерится');
  
  return (
    <div>
      <h3>{data.title}</h3>
      <button onClick={() => onAction(data.id)}>Действие</button>
    </div>
  );
});

// Сравнение props вручную
const SmartComponent = memo(({ user, settings }) => {
  return <div>{user.name}</div>;
}, (prevProps, nextProps) => {
  // true = не ре-рендерить, false = ре-рендерить
  return prevProps.user.id === nextProps.user.id;
});
```

### useMemo и useCallback

```jsx
import { useMemo, useCallback, useState } from 'react';

const OptimizedComponent = ({ items }) => {
  const [filter, setFilter] = useState('');
  const [sortBy, setSortBy] = useState('name');
  
  // Мемоизация вычислений
  const filteredAndSortedItems = useMemo(() => {
    return items
      .filter(item => item.name.toLowerCase().includes(filter.toLowerCase()))
      .sort((a, b) => a[sortBy].localeCompare(b[sortBy]));
  }, [items, filter, sortBy]);
  
  // Мемоизация функций
  const handleItemClick = useCallback((id) => {
    console.log('Клик по элементу:', id);
  }, []);
  
  return (
    <div>
      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Фильтр"
      />
      <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
        <option value="name">По имени</option>
        <option value="date">По дате</option>
      </select>
      
      {filteredAndSortedItems.map(item => (
        <ExpensiveComponent
          key={item.id}
          data={item}
          onAction={handleItemClick}
        />
      ))}
    </div>
  );
};
```

## 🛠️ Полезные советы

### Обработка ошибок

```jsx
import { useState, useEffect } from 'react';

const ErrorBoundary = ({ children }) => {
  const [hasError, setHasError] = useState(false);
  
  useEffect(() => {
    const handleError = (error) => {
      console.error('Поймана ошибка:', error);
      setHasError(true);
    };
    
    window.addEventListener('error', handleError);
    return () => window.removeEventListener('error', handleError);
  }, []);
  
  if (hasError) {
    return <h2>Что-то пошло не так!</h2>;
  }
  
  return children;
};
```

### Дебаунсинг в React

```jsx
import { useState, useEffect } from 'react';

const useDebounce = (value, delay) => {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
};

const SearchComponent = () => {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  
  useEffect(() => {
    if (debouncedSearchTerm) {
      // Выполнить поиск
      console.log('Поиск:', debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);
  
  return (
    <input
      type="text"
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Поиск..."
    />
  );
};
```

## ✅ Лучшие практики

1. **Всегда используйте ключи** при рендере списков
2. **Не изменяйте state напрямую**, используйте set функции
3. **Используйте функциональные обновления** для сложной логики
4. **Выносите логику в пользовательские хуки** для переиспользования
5. **Мемоизируйте тяжелые вычисления** с useMemo
6. **Деструктуризируйте props** для читаемости кода
7. **Используйте TypeScript** для больших проектов
8. **Разбивайте компоненты** на маленькие, переиспользуемые части

## 📚 Что дальше?

- Изучите React Router для навигации
- Освойте управление состоянием (Redux, Zustand)
- Изучите тестирование (Jest, React Testing Library)
- Попробуйте Next.js для production приложений
- Изучите стейт менеджеры (Redux Toolkit, Recoil)

Эта памятка покрывает основы React. Практикуйтесь, создавайте проекты, и постепенно изучайте более продвинутые концепции!
