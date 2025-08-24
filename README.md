# Полная шпаргалка по JavaScript для Java-разработчика

## Содержание
1. [Типы данных](#типы-данных)
2. [Переменные: var, let, const](#переменные-var-let-const)
3. [Функции](#функции)
4. [Объекты и ООП](#объекты-и-ооп)
5. [Прототипное наследование](#прототипное-наследование)
6. [Классы в ES6+](#классы-в-es6)
7. [Области видимости и контекст this](#области-видимости-и-контекст-this)
8. [Замыкания](#замыкания)
9. [Методы функций: bind, call, apply](#методы-функций-bind-call-apply)
10. [Асинхронное программирование](#асинхронное-программирование)
11. [Коллекции](#коллекции)
12. [Обработка исключений](#обработка-исключений)
13. [Управление памятью и сборка мусора](#управление-памятью-и-сборка-мусора)
14. [Event Loop](#event-loop)
15. [Работа с DOM](#работа-с-dom)
16. [Web Workers](#web-workers)
17. [Web Storage API](#web-storage-api)
18. [WebSockets](#websockets)
19. [Node.js](#nodejs)
20. [JSDoc и типизация](#jsdoc-и-типизация)
21. [Тестирование: Jest и Mocha](#тестирование-jest-и-mocha)
22. [Современные возможности JavaScript](#современные-возможности-javascript)

---

## Типы данных

В JavaScript есть 8 основных типов данных:

### Примитивные типы:
1. **Number** - числа (целые и с плавающей точкой)
2. **BigInt** - произвольно большие целые числа
3. **String** - строки
4. **Boolean** - логический тип (true/false)
5. **null** - специальное значение, обозначающее "пусто"
6. **undefined** - специальное значение, обозначающее "не определено"
7. **Symbol** - уникальные идентификаторы

### Объектный тип:
8. **Object** - объекты, массивы, функции и т.д.

```javascript
// Примеры типов данных
const num = 42;
const bigInt = 9007199254740991n; // BigInt
const str = "Привет, мир!";
const bool = true;
const nullValue = null;
let undefinedValue;
const sym = Symbol("id");
const obj = { name: "John", age: 30 };

// Проверка типа
console.log(typeof num);        // "number"
console.log(typeof bigInt);     // "bigint"
console.log(typeof str);        // "string"
console.log(typeof bool);       // "boolean"
console.log(typeof nullValue);  // "object" (это историческая ошибка в JS)
console.log(typeof undefinedValue); // "undefined"
console.log(typeof sym);        // "symbol"
console.log(typeof obj);        // "object"
console.log(typeof function(){}); // "function"

// Преобразование типов
console.log(String(42));        // "42"
console.log(Number("42"));      // 42
console.log(Boolean(1));        // true
console.log(Boolean(0));        // false
console.log(Boolean(""));       // false
console.log(Boolean("0"));      // true (непустая строка)
```

## Переменные: var, let, const

### var
- Имеет функциональную область видимости
- Поднимается (hoisting) - объявление переменной перемещается в начало функции/скрипта
- Можно переопределять в той же области видимости

### let
- Имеет блочную область видимости
- Поднимается, но нельзя использовать до объявления (временная мертвая зона)
- Нельзя переопределять в той же области видимости

### const
- Имеет блочную область видимости
- Поднимается, но нельзя использовать до объявления
- Нельзя переопределять значение
- Для объектов и массивов можно изменять внутреннее содержимое

```javascript
// var
function example() {
    console.log(x); // undefined (hoisting)
    var x = 10;
    var x = 20; // Можно переопределить
    console.log(x); // 20
}

// let
function example2() {
    // console.log(y); // ReferenceError: Cannot access 'y' before initialization
    let y = 10;
    // let y = 20; // SyntaxError: Identifier 'y' has already been declared
    y = 20; // Можно изменить значение
    console.log(y); // 20
    
    if (true) {
        let y = 30; // Другая переменная в другой области видимости
        console.log(y); // 30
    }
    
    console.log(y); // 20
}

// const
function example3() {
    const z = 10;
    // z = 20; // TypeError: Assignment to constant variable
    
    const obj = { name: "John" };
    obj.name = "Jane"; // Можно изменять свойства объекта
    console.log(obj); // { name: "Jane" }
    
    // obj = {}; // TypeError: Assignment to constant variable
}
```

## Функции

В JavaScript функции являются объектами первого класса и могут быть:
- Присвоены переменным
- Переданы как аргументы
- Возвращены из других функций

### Виды функций:

#### 1. Объявление функции (Function Declaration)
```javascript
function add(a, b) {
    return a + b;
}
```

#### 2. Функциональное выражение (Function Expression)
```javascript
const subtract = function(a, b) {
    return a - b;
};
```

#### 3. Стрелочные функции (Arrow Functions)
```javascript
const multiply = (a, b) => a * b;

// Многострочные стрелочные функции
const divide = (a, b) => {
    if (b === 0) throw new Error("Деление на ноль");
    return a / b;
};
```

#### 4. Методы объектов
```javascript
const calculator = {
    add: function(a, b) {
        return a + b;
    },
    // Сокращенный синтаксис методов (ES6)
    subtract(a, b) {
        return a - b;
    }
};
```

#### 5. Конструкторы функций
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return `Привет, меня зовут ${this.name}`;
    };
}

const john = new Person("John", 30);
```

#### 6. Генераторы
```javascript
function* idGenerator() {
    let id = 1;
    while (true) {
        yield id++;
    }
}

const gen = idGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```

### Параметры функций:

#### Параметры по умолчанию
```javascript
function greet(name = "Гость") {
    return `Привет, ${name}!`;
}
```

#### Rest-параметры
```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}
```

#### Деструктуризация параметров
```javascript
function displayPerson({ name, age, job = "Не указано" }) {
    console.log(`${name}, ${age} лет, ${job}`);
}

displayPerson({ name: "Анна", age: 28 });
```

## Объекты и ООП

### Создание объектов

```javascript
// Литерал объекта
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 30,
    greet() {
        return `Привет, меня зовут ${this.firstName} ${this.lastName}`;
    }
};

// Конструктор Object
const car = new Object();
car.make = "Toyota";
car.model = "Corolla";
car.year = 2020;

// Object.create()
const animal = {
    type: "Неизвестно",
    makeSound() {
        console.log("Какой-то звук");
    }
};

const dog = Object.create(animal);
dog.type = "Собака";
dog.makeSound = function() {
    console.log("Гав");
};
```

### Свойства объектов

```javascript
// Геттеры и сеттеры
const person = {
    firstName: "John",
    lastName: "Doe",
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(value) {
        [this.firstName, this.lastName] = value.split(" ");
    }
};

console.log(person.fullName); // "John Doe"
person.fullName = "Jane Smith";
console.log(person.firstName); // "Jane"

// Object.defineProperty
const product = {};
Object.defineProperty(product, "price", {
    value: 100,
    writable: true,
    enumerable: true,
    configurable: true
});

// Дескрипторы свойств
Object.defineProperty(product, "discount", {
    get() {
        return this._discount || 0;
    },
    set(value) {
        if (value < 0 || value > 100) {
            throw new Error("Скидка должна быть от 0 до 100");
        }
        this._discount = value;
    }
});
```

## Прототипное наследование

В JavaScript наследование основано на прототипах, а не на классах (хотя классы в ES6+ - это синтаксический сахар над прототипами).

```javascript
// Базовый объект
function Animal(name) {
    this.name = name;
}

Animal.prototype.makeSound = function() {
    console.log("Какой-то звук");
};

// Наследование
function Dog(name, breed) {
    Animal.call(this, name); // Вызов конструктора родителя
    this.breed = breed;
}

// Установка прототипа
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // Восстановление конструктора

// Переопределение метода
Dog.prototype.makeSound = function() {
    console.log("Гав");
};

// Добавление нового метода
Dog.prototype.fetch = function() {
    console.log(`${this.name} принес палку`);
};

const rex = new Dog("Рекс", "Овчарка");
rex.makeSound(); // "Гав"
rex.fetch(); // "Рекс принес палку"
console.log(rex instanceof Dog); // true
console.log(rex instanceof Animal); // true
```

## Классы в ES6+

ES6 ввел синтаксис классов, который упрощает работу с прототипным наследованием.

```javascript
// Базовый класс
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    makeSound() {
        console.log("Какой-то звук");
    }
    
    // Статический метод
    static isAnimal(obj) {
        return obj instanceof Animal;
    }
}

// Наследование
class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Вызов конструктора родителя
        this.breed = breed;
    }
    
    // Переопределение метода
    makeSound() {
        console.log("Гав");
    }
    
    // Новый метод
    fetch() {
        console.log(`${this.name} принес палку`);
    }
}

const rex = new Dog("Рекс", "Овчарка");
rex.makeSound(); // "Гав"
console.log(Animal.isAnimal(rex)); // true
```

### Приватные поля и методы (ES2020+)

```javascript
class BankAccount {
    // Приватное поле
    #balance = 0;
    
    // Приватный метод
    #validateAmount(amount) {
        if (amount <= 0) {
            throw new Error("Сумма должна быть положительной");
        }
    }
    
    constructor(owner, initialBalance = 0) {
        this.owner = owner;
        if (initialBalance > 0) {
            this.#balance = initialBalance;
        }
    }
    
    deposit(amount) {
        this.#validateAmount(amount);
        this.#balance += amount;
        return this.#balance;
    }
    
    withdraw(amount) {
        this.#validateAmount(amount);
        if (amount > this.#balance) {
            throw new Error("Недостаточно средств");
        }
        this.#balance -= amount;
        return this.#balance;
    }
    
    get balance() {
        return this.#balance;
    }
}

const account = new BankAccount("John", 1000);
console.log(account.balance); // 1000
account.deposit(500);
console.log(account.balance); // 1500
// console.log(account.#balance); // SyntaxError: Private field '#balance' must be declared in an enclosing class
```

### Статические поля и методы

```javascript
class MathUtils {
    // Статическое поле
    static PI = 3.14159;
    
    // Статический метод
    static square(x) {
        return x * x;
    }
    
    // Статический приватный метод
    static #calculateArea(radius) {
        return this.PI * radius * radius;
    }
    
    static circleArea(radius) {
        if (radius <= 0) {
            throw new Error("Радиус должен быть положительным");
        }
        return this.#calculateArea(radius);
    }
}

console.log(MathUtils.PI); // 3.14159
console.log(MathUtils.square(5)); // 25
console.log(MathUtils.circleArea(2)); // ~12.57
```

## Области видимости и контекст this

### Области видимости

1. **Глобальная область** - переменные, доступные везде
2. **Функциональная область** - переменные, доступные внутри функции
3. **Блочная область** - переменные, доступные внутри блока (let, const)

```javascript
// Глобальная область
const globalVar = "Я глобальная";

function outerFunction() {
    // Функциональная область
    const outerVar = "Я из внешней функции";
    
    function innerFunction() {
        // Вложенная функциональная область
        const innerVar = "Я из внутренней функции";
        console.log(globalVar); // Доступна
        console.log(outerVar);  // Доступна
        console.log(innerVar);  // Доступна
    }
    
    innerFunction();
    console.log(globalVar); // Доступна
    console.log(outerVar);  // Доступна
    // console.log(innerVar);  // ReferenceError: innerVar is not defined
}

if (true) {
    // Блочная область
    let blockVar = "Я из блока";
    console.log(blockVar); // Доступна
}
// console.log(blockVar); // ReferenceError: blockVar is not defined
```

### Контекст this

Значение `this` зависит от того, как вызывается функция:

1. **Глобальный контекст**: `this` указывает на глобальный объект (в браузере - `window`, в Node.js - `global`)
2. **Метод объекта**: `this` указывает на объект, у которого вызван метод
3. **Конструктор**: `this` указывает на создаваемый экземпляр
4. **Явное указание**: через `call`, `apply`, `bind`
5. **Стрелочные функции**: `this` берется из внешней (лексической) области видимости

```javascript
// Глобальный контекст
console.log(this); // window в браузере

// Метод объекта
const user = {
    name: "John",
    greet() {
        console.log(`Привет, я ${this.name}`);
    }
};
user.greet(); // "Привет, я John"

// Потеря контекста
const greet = user.greet;
// greet(); // "Привет, я undefined" (this = window)

// Конструктор
function User(name) {
    this.name = name;
    this.sayHi = function() {
        console.log(`Привет, я ${this.name}`);
    };
}
const john = new User("John");
john.sayHi(); // "Привет, я John"

// Стрелочные функции сохраняют this
const obj = {
    name: "Object",
    regularMethod: function() {
        console.log(`Regular: ${this.name}`);
        
        // this из regularMethod
        setTimeout(() => {
            console.log(`Arrow: ${this.name}`);
        }, 100);
        
        // this = window
        setTimeout(function() {
            console.log(`Regular timeout: ${this?.name || 'undefined'}`);
        }, 100);
    }
};
obj.regularMethod();
// "Regular: Object"
// "Arrow: Object"
// "Regular timeout: undefined"
```

## Замыкания

Замыкание - это функция, которая запоминает свою лексическую область видимости, даже когда выполняется вне этой области.

```javascript
// Простое замыкание
function createCounter() {
    let count = 0;
    
    return function() {
        return ++count;
    };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

// Замыкание с параметрами
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Практическое применение: приватные переменные
function createBankAccount(initialBalance = 0) {
    let balance = initialBalance;
    
    return {
        deposit(amount) {
            balance += amount;
            return balance;
        },
        withdraw(amount) {
            if (amount > balance) {
                throw new Error("Недостаточно средств");
            }
            balance -= amount;
            return balance;
        },
        getBalance() {
            return balance;
        }
    };
}

const account = createBankAccount(100);
account.deposit(50);
console.log(account.getBalance()); // 150
account.withdraw(30);
console.log(account.getBalance()); // 120
// balance недоступен извне
```

## Методы функций: bind, call, apply

Эти методы позволяют явно указать значение `this` при вызове функции.

### call

Вызывает функцию с указанным значением `this` и аргументами, перечисленными через запятую.

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "John" };

greet.call(person, "Привет", "!"); // "Привет, John!"
```

### apply

Похож на `call`, но принимает аргументы в виде массива.

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "John" };

greet.apply(person, ["Здравствуйте", "."]);  // "Здравствуйте, John."
```

### bind

Создает новую функцию с привязанным значением `this` и, опционально, с предустановленными аргументами.

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "John" };

const greetJohn = greet.bind(person);
greetJohn("Привет", "!"); // "Привет, John!"

const sayHiToJohn = greet.bind(person, "Привет");
sayHiToJohn("?"); // "Привет, John?"

const sayHiToJohnExcitedly = greet.bind(person, "Привет", "!!!");
sayHiToJohnExcitedly(); // "Привет, John!!!"
```

## Асинхронное программирование

### Callbacks

Самый старый подход к асинхронному программированию в JavaScript.

```javascript
function fetchData(callback) {
    setTimeout(() => {
        const data = { name: "John", age: 30 };
        callback(null, data);
    }, 1000);
}

fetchData((error, data) => {
    if (error) {
        console.error("Ошибка:", error);
        return;
    }
    console.log("Данные:", data);
});
```

### Promises

Promises (обещания) - это объекты, представляющие результат асинхронной операции.

```javascript
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = true;
            if (success) {
                resolve({ name: "John", age: 30 });
            } else {
                reject(new Error("Не удалось получить данные"));
            }
        }, 1000);
    });
}

fetchData()
    .then(data => {
        console.log("Данные:", data);
        return data.name;
    })
    .then(name => {
        console.log("Имя:", name);
    })
    .catch(error => {
        console.error("Ошибка:", error);
    })
    .finally(() => {
        console.log("Операция завершена");
    });

// Параллельное выполнение промисов
Promise.all([
    fetchData(),
    fetchData()
])
    .then(([data1, data2]) => {
        console.log("Все данные:", data1, data2);
    })
    .catch(error => {
        console.error("Хотя бы один промис отклонен:", error);
    });

// Выполнение первого успешного промиса
Promise.race([
    fetchData(),
    new Promise((resolve) => setTimeout(() => resolve("timeout"), 500))
])
    .then(result => {
        console.log("Первый результат:", result);
    });

// Выполнение всех промисов (без отклонения при ошибке)
Promise.allSettled([
    fetchData(),
    Promise.reject(new Error("Ошибка"))
])
    .then(results => {
        results.forEach(result => {
            if (result.status === "fulfilled") {
                console.log("Успешно:", result.value);
            } else {
                console.log("Ошибка:", result.reason);
            }
        });
    });

// Возвращает первый успешный промис
Promise.any([
    Promise.reject(new Error("Ошибка 1")),
    fetchData(),
    Promise.reject(new Error("Ошибка 2"))
])
    .then(result => {
        console.log("Первый успешный результат:", result);
    })
    .catch(error => {
        console.log("Все промисы отклонены:", error);
    });
```

### Async/Await

Синтаксический сахар над промисами, делающий асинхронный код похожим на синхронный.

```javascript
async function getData() {
    try {
        const data = await fetchData();
        console.log("Данные:", data);
        
        const name = data.name;
        console.log("Имя:", name);
        
        return name;
    } catch (error) {
        console.error("Ошибка:", error);
        throw error; // Пробрасываем ошибку дальше
    } finally {
        console.log("Операция завершена");
    }
}

// Вызов асинхронной функции
getData()
    .then(name => {
        console.log("Итоговое имя:", name);
    })
    .catch(error => {
        console.error("Ошибка в getData:", error);
    });

// Параллельное выполнение с async/await
async function getMultipleData() {
    try {
        // Запускаем оба запроса параллельно
        const dataPromise1 = fetchData();
        const dataPromise2 = fetchData();
        
        // Ждем выполнения обоих
        const [data1, data2] = await Promise.all([dataPromise1, dataPromise2]);
        
        console.log("Данные 1:", data1);
        console.log("Данные 2:", data2);
    } catch (error) {
        console.error("Ошибка при получении данных:", error);
    }
}
```

## Коллекции

### Array (Массивы)

```javascript
// Создание массива
const numbers = [1, 2, 3, 4, 5];
const empty = new Array(5); // [empty × 5]
const filled = Array(5).fill(0); // [0, 0, 0, 0, 0]

// Доступ к элементам
console.log(numbers[0]); // 1
numbers[1] = 10;
console.log(numbers); // [1, 10, 3, 4, 5]

// Основные методы
numbers.push(6); // Добавляет в конец
numbers.pop(); // Удаляет с конца
numbers.unshift(0); // Добавляет в начало
numbers.shift(); // Удаляет с начала
numbers.splice(2, 1, 'a', 'b'); // Удаляет 1 элемент с индекса 2 и вставляет новые
numbers.slice(1, 3); // Возвращает новый массив [индекс1, индекс2)
numbers.concat([7, 8]); // Объединяет массивы

// Поиск элементов
numbers.indexOf(3); // Индекс первого вхождения или -1
numbers.lastIndexOf(3); // Индекс последнего вхождения или -1
numbers.includes(3); // true/false
numbers.find(x => x > 3); // Первый элемент, удовлетворяющий условию
numbers.findIndex(x => x > 3); // Индекс первого элемента, удовлетворяющего условию

// Итерация
numbers.forEach(x => console.log(x));

// Преобразование
const doubled = numbers.map(x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, x) => acc + x, 0);
const allPositive = numbers.every(x => x > 0);
const someNegative = numbers.some(x => x < 0);

// Сортировка
numbers.sort((a, b) => a - b); // По возрастанию
numbers.reverse(); // Обращение порядка

// Преобразование в строку
numbers.join(', '); // "1, 10, 3, 4, 5"

// Деструктуризация
const [first, second, ...rest] = numbers;

// Spread оператор
const numbersCopy = [...numbers];

// Новые методы (ES2022+)
numbers.at(-1); // Последний элемент (5)
```

### Map

Коллекция пар ключ-значение с произвольными типами ключей.

```javascript
// Создание Map
const userRoles = new Map();

// Добавление элементов
userRoles.set('john', 'admin');
userRoles.set('jane', 'editor');
userRoles.set('bob', 'viewer');

// Инициализация с массивом пар
const userRoles2 = new Map([
    ['john', 'admin'],
    ['jane', 'editor'],
    ['bob', 'viewer']
]);

// Получение значений
console.log(userRoles.get('john')); // "admin"
console.log(userRoles.has('jane')); // true
console.log(userRoles.size); // 3

// Удаление
userRoles.delete('bob');
// userRoles.clear(); // Удаляет все элементы

// Итерация
for (const [user, role] of userRoles) {
    console.log(`${user}: ${role}`);
}

userRoles.forEach((role, user) => {
    console.log(`${user}: ${role}`);
});

// Получение ключей, значений, пар
const users = [...userRoles.keys()];
const roles = [...userRoles.values()];
const entries = [...userRoles.entries()];

// Объекты как ключи
const userMap = new Map();
const user1 = { name: 'John' };
const user2 = { name: 'Jane' };

userMap.set(user1, 'admin');
userMap.set(user2, 'editor');

console.log(userMap.get(user1)); // "admin"
console.log(userMap.get({ name: 'John' })); // undefined (другой объект)
```

### Set

Коллекция уникальных значений любого типа.

```javascript
// Создание Set
const uniqueNumbers = new Set([1, 2, 3, 3, 4, 4, 5]);
console.log(uniqueNumbers); // Set(5) {1, 2, 3, 4, 5}

// Добавление элементов
uniqueNumbers.add(6);
uniqueNumbers.add(1); // Игнорируется, т.к. 1 уже есть

// Проверка наличия
console.log(uniqueNumbers.has(3)); // true
console.log(uniqueNumbers.size); // 6

// Удаление
uniqueNumbers.delete(4);
// uniqueNumbers.clear(); // Удаляет все элементы

// Итерация
for (const num of uniqueNumbers) {
    console.log(num);
}

uniqueNumbers.forEach(num => {
    console.log(num);
});

// Преобразование в массив
const numbersArray = [...uniqueNumbers];

// Практическое применение: удаление дубликатов из массива
const withDuplicates = [1, 2, 2, 3, 4, 4, 5];
const withoutDuplicates = [...new Set(withDuplicates)];
```

### WeakMap и WeakSet

Специальные версии Map и Set с "слабыми" ссылками на объекты.

```javascript
// WeakMap
const weakMap = new WeakMap();
let obj = { name: "John" };

weakMap.set(obj, "metadata");
console.log(weakMap.get(obj)); // "metadata"

obj = null; // Теперь объект может быть собран сборщиком мусора

// WeakSet
const weakSet = new WeakSet();
let user = { id: 1 };

weakSet.add(user);
console.log(weakSet.has(user)); // true

user = null; // Теперь объект может быть собран сборщиком мусора

// Отличия от Map и Set:
// 1. Ключами могут быть только объекты
// 2. Нет методов keys(), values(), entries(), size, forEach()
// 3. Нельзя перебрать содержимое
// 4. Объекты удаляются автоматически при отсутствии других ссылок
```

### Array Buffer и Typed Arrays

Для работы с бинарными данными.

```javascript
// ArrayBuffer - непрерывная область памяти фиксированной длины
const buffer = new ArrayBuffer(16); // 16 байт

// TypedArray - представление ArrayBuffer как массива определенного типа
const int32View = new Int32Array(buffer);
int32View[0] = 42;
console.log(int32View[0]); // 42

// Разные представления одного буфера
const int8View = new Int8Array(buffer);
console.log(int8View[0]); // 42 (младший байт от 42)

// Типы типизированных массивов:
// Int8Array, Uint8Array, Uint8ClampedArray
// Int16Array, Uint16Array
// Int32Array, Uint32Array
// Float32Array, Float64Array
// BigInt64Array, BigUint64Array
```

## Обработка исключений

```javascript
// try-catch-finally
try {
    // Код, который может вызвать ошибку
    const result = riskyOperation();
    console.log(result);
} catch (error) {
    // Обработка ошибки
    console.error("Произошла ошибка:", error.message);
    
    // Можно проверить тип ошибки
    if (error instanceof TypeError) {
        console.error("Ошибка типа");
    } else if (error instanceof ReferenceError) {
        console.error("Ошибка ссылки");
    }
} finally {
    // Выполняется всегда, независимо от ошибки
    console.log("Завершение операции");
}

// Создание собственных ошибок
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateUser(user) {
    if (!user.name) {
        throw new ValidationError("Имя пользователя обязательно");
    }
    if (user.age < 18) {
        throw new ValidationError("Пользователь должен быть старше 18 лет");
    }
}

try {
    validateUser({ name: "John", age: 16 });
} catch (error) {
    if (error instanceof ValidationError) {
        console.error("Ошибка валидации:", error.message);
    } else {
        console.error("Неизвестная ошибка:", error);
        throw error; // Пробрасываем неизвестные ошибки дальше
    }
}

// Асинхронная обработка ошибок
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Ошибка при получении данных:", error);
        throw error; // Пробрасываем ошибку дальше
    }
}
```

## Управление памятью и сборка мусора

JavaScript автоматически управляет памятью с помощью сборщика мусора, который освобождает память, занятую объектами, когда они становятся недоступными.

```javascript
// Создание объекта
let obj = { data: "some data", size: 1024 };

// Объект доступен через переменную obj
console.log(obj.data);

// Перезаписываем ссылку
obj = null;

// Теперь исходный объект недоступен и может быть собран сборщиком мусора
```

### Утечки памяти

Распространенные причины утечек памяти:

1. **Нежелательные замыкания**

```javascript
function createLeak() {
    const largeData = new Array(1000000).fill('x');
    
    return function() {
        // Использует только один элемент, но хранит всю структуру
        console.log(largeData[0]);
    };
}

const leakyFunction = createLeak(); // largeData остается в памяти
```

2. **Забытые обработчики событий**

```javascript
function setupHandler() {
    const largeData = new Array(1000000).fill('x');
    
    document.getElementById('button').addEventListener('click', function() {
        // Обработчик сохраняет ссылку на largeData
        console.log(largeData[0]);
    });
}

// Даже если setupHandler завершится, обработчик и largeData останутся в памяти
```

3. **Циклические ссылки**

```javascript
function createCycle() {
    const obj1 = {};
    const obj2 = {};
    
    obj1.ref = obj2;
    obj2.ref = obj1;
    
    return function() {
        // Функция, которая ничего не делает, но сохраняет
        // доступ к циклически связанным объектам
    };
}
```

### WeakMap и WeakSet для предотвращения утечек

```javascript
// Использование WeakMap для кеширования без утечек памяти
const cache = new WeakMap();

function processUser(user) {
    if (cache.has(user)) {
        return cache.get(user);
    }
    
    const result = expensiveOperation(user);
    cache.set(user, result);
    return result;
}

let user = { name: "John" };
processUser(user);

// Если user больше не используется, он и его данные в кеше
// могут быть автоматически удалены сборщиком мусора
user = null;
```

## Event Loop

Event Loop (цикл событий) - это механизм, который позволяет JavaScript выполнять неблокирующие операции, несмотря на то, что JavaScript однопоточный.

### Компоненты Event Loop:

1. **Call Stack** - стек вызовов функций
2. **Heap** - область памяти для объектов
3. **Queue** - очередь задач:
   - **Macrotask Queue** - setTimeout, setInterval, I/O, UI рендеринг
   - **Microtask Queue** - Promise callbacks, queueMicrotask, MutationObserver

### Порядок выполнения:

1. Выполнить весь код в стеке вызовов
2. Выполнить все микрозадачи
3. Выполнить одну макрозадачу
4. Повторить с шага 1

```javascript
console.log('1 - Синхронный код');

setTimeout(() => {
    console.log('4 - Макрозадача (setTimeout)');
}, 0);

Promise.resolve()
    .then(() => {
        console.log('2 - Микрозадача (Promise)');
    })
    .then(() => {
        console.log('3 - Микрозадача (Promise, следующий then)');
    });

console.log('5 - Синхронный код');

// Вывод:
// 1 - Синхронный код
// 5 - Синхронный код
// 2 - Микрозадача (Promise)
// 3 - Микрозадача (Promise, следующий then)
// 4 - Макрозадача (setTimeout)
```

### queueMicrotask

```javascript
console.log('1 - Начало');

queueMicrotask(() => {
    console.log('3 - Микрозадача');
});

console.log('2 - Конец');

// Вывод:
// 1 - Начало
// 2 - Конец
// 3 - Микрозадача
```

## Работа с DOM

DOM (Document Object Model) - это программный интерфейс для HTML и XML документов.

### Выбор элементов

```javascript
// По ID
const element = document.getElementById('myId');

// По классу
const elements = document.getElementsByClassName('myClass');

// По тегу
const divs = document.getElementsByTagName('div');

// CSS-селекторы
const element2 = document.querySelector('.myClass'); // Первый найденный
const allElements = document.querySelectorAll('div.myClass'); // NodeList всех найденных

// Относительная навигация
const parent = element.parentNode;
const children = element.childNodes; // Включая текстовые узлы
const elementChildren = element.children; // Только элементы
const firstChild = element.firstChild;
const lastChild = element.lastChild;
const nextSibling = element.nextSibling;
const previousSibling = element.previousSibling;
```

### Изменение DOM

```javascript
// Создание элементов
const div = document.createElement('div');
const textNode = document.createTextNode('Привет, мир!');

// Добавление элементов
div.appendChild(textNode);
document.body.appendChild(div);

// Вставка элементов
const parent = document.getElementById('container');
const referenceNode = document.getElementById('reference');
parent.insertBefore(div, referenceNode);

// Современные методы вставки
parent.append(div, textNode); // Добавляет в конец
parent.prepend(div); // Добавляет в начало
referenceNode.before(div); // Вставляет перед
referenceNode.after(div); // Вставляет после
referenceNode.replaceWith(div); // Заменяет

// Удаление элементов
parent.removeChild(div);
div.remove(); // Современный метод

// Клонирование
const clone = div.cloneNode(true); // true - глубокое клонирование с потомками
```

### Работа с атрибутами

```javascript
// Стандартные атрибуты
element.id = 'newId';
element.className = 'class1 class2';
element.href = 'https://example.com';

// Произвольные атрибуты
element.setAttribute('data-id', '123');
const value = element.getAttribute('data-id');
element.removeAttribute('data-id');

// Проверка наличия атрибута
const hasAttr = element.hasAttribute('data-id');

// data-атрибуты
element.dataset.id = '123'; // Устанавливает data-id="123"
console.log(element.dataset.id); // Получает значение data-id
```

### Работа с классами

```javascript
// Добавление/удаление классов
element.classList.add('newClass');
element.classList.remove('oldClass');
element.classList.toggle('active'); // Добавляет, если нет; удаляет, если есть
element.classList.contains('active'); // Проверка наличия класса
element.classList.replace('oldClass', 'newClass'); // Заменяет один класс другим
```

### Работа со стилями

```javascript
// Встроенные стили
element.style.color = 'red';
element.style.backgroundColor = 'blue'; // camelCase для CSS-свойств
element.style.cssText = 'color: red; background-color: blue;'; // Несколько стилей сразу

// Получение вычисленных стилей
const computedStyle = getComputedStyle(element);
console.log(computedStyle.color);
```

### События

```javascript
// Добавление обработчика
function handleClick(event) {
    console.log('Клик!', event.target);
    event.preventDefault(); // Предотвращает действие по умолчанию
    event.stopPropagation(); // Останавливает всплытие
}

element.addEventListener('click', handleClick);

// Удаление обработчика
element.removeEventListener('click', handleClick);

// Делегирование событий
document.getElementById('list').addEventListener('click', function(event) {
    if (event.target.tagName === 'LI') {
        console.log('Клик по элементу списка:', event.target.textContent);
    }
});

// Создание и отправка событий
const customEvent = new CustomEvent('myEvent', {
    detail: { data: 'Произвольные данные' },
    bubbles: true,
    cancelable: true
});
element.dispatchEvent(customEvent);

// Обработка пользовательского события
element.addEventListener('myEvent', function(event) {
    console.log('Пользовательское событие:', event.detail.data);
});
```

## Web Workers

Web Workers позволяют выполнять JavaScript-код в фоновом потоке, не блокируя основной поток.

### Основной поток (main.js)

```javascript
// Создание Worker
const worker = new Worker('worker.js');

// Отправка сообщения Worker'у
worker.postMessage({ command: 'start', data: [1, 2, 3, 4, 5] });

// Получение сообщения от Worker'а
worker.onmessage = function(event) {
    console.log('Результат от Worker:', event.data);
};

// Обработка ошибок
worker.onerror = function(error) {
    console.error('Ошибка в Worker:', error.message);
};

// Завершение Worker'а
function terminateWorker() {
    worker.terminate();
}
```

### Worker (worker.js)

```javascript
// Получение сообщения от основного потока
self.onmessage = function(event) {
    const { command, data } = event.data;
    
    if (command === 'start') {
        // Выполнение тяжелых вычислений
        const result = heavyComputation(data);
        
        // Отправка результата обратно
        self.postMessage({ result });
    }
};

function heavyComputation(data) {
    // Пример: вычисление суммы квадратов
    return data.map(x => x * x).reduce((sum, x) => sum + x, 0);
}

// Обработка ошибок
self.onerror = function(error) {
    console.error('Ошибка в Worker:', error.message);
};

// Самозавершение
function closeWorker() {
    self.close();
}
```

### Shared Workers

Позволяют нескольким вкладкам/фреймам использовать один и тот же Worker.

```javascript
// Основной поток
const sharedWorker = new SharedWorker('shared-worker.js');

// Подключение к порту
sharedWorker.port.start();

// Отправка сообщения
sharedWorker.port.postMessage({ command: 'getData' });

// Получение сообщения
sharedWorker.port.onmessage = function(event) {
    console.log('Данные от Shared Worker:', event.data);
};
```

```javascript
// shared-worker.js
const connections = [];

// Обработка подключений
self.onconnect = function(event) {
    const port = event.ports[0];
    connections.push(port);
    port.start();
    
    port.onmessage = function(event) {
        const { command } = event.data;
        
        if (command === 'getData') {
            port.postMessage({ result: 'Данные для всех подключений' });
        } else if (command === 'broadcast') {
            // Отправка сообщения всем подключениям
            connections.forEach(connection => {
                connection.postMessage({ broadcast: 'Сообщение для всех' });
            });
        }
    };
};
```

## Web Storage API

### LocalStorage

Хранит данные без ограничения по времени, доступен только в рамках одного домена.

```javascript
// Сохранение данных
localStorage.setItem('username', 'John');
localStorage.setItem('preferences', JSON.stringify({ theme: 'dark', fontSize: 14 }));

// Получение данных
const username = localStorage.getItem('username');
const preferences = JSON.parse(localStorage.getItem('preferences'));

// Удаление данных
localStorage.removeItem('username');

// Очистка всех данных
localStorage.clear();

// Количество элементов
console.log(localStorage.length);

// Получение ключа по индексу
const key = localStorage.key(0);
```

### SessionStorage

Похож на localStorage, но данные сохраняются только на время сессии (пока открыта вкладка).

```javascript
// Работает аналогично localStorage
sessionStorage.setItem('tempData', 'Временные данные');
const tempData = sessionStorage.getItem('tempData');
sessionStorage.removeItem('tempData');
sessionStorage.clear();
```

### События хранилища

```javascript
// Отслеживание изменений в localStorage/sessionStorage в других вкладках
window.addEventListener('storage', function(event) {
    console.log('Изменение в хранилище:', {
        key: event.key,
        oldValue: event.oldValue,
        newValue: event.newValue,
        url: event.url,
        storageArea: event.storageArea // localStorage или sessionStorage
    });
});
```

## WebSockets

WebSocket - это протокол, обеспечивающий полнодуплексную связь между клиентом и сервером.

### Клиентская часть

```javascript
// Создание соединения
const socket = new WebSocket('wss://example.com/socket');

// Обработка событий
socket.onopen = function(event) {
    console.log('Соединение установлено');
    
    // Отправка сообщения серверу
    socket.send(JSON.stringify({ type: 'greeting', content: 'Привет, сервер!' }));
};

socket.onmessage = function(event) {
    // Получение сообщения от сервера
    const message = JSON.parse(event.data);
    console.log('Сообщение от сервера:', message);
};

socket.onerror = function(error) {
    console.error('Ошибка WebSocket:', error);
};

socket.onclose = function(event) {
    console.log('Соединение закрыто:', event.code, event.reason);
};

// Отправка данных
function sendMessage(content) {
    if (socket.readyState === WebSocket.OPEN) {
        socket.send(JSON.stringify({ type: 'message', content }));
    } else {
        console.error('Соединение не установлено');
    }
}

// Закрытие соединения
function closeConnection() {
    socket.close(1000, 'Нормальное закрытие');
}
```

### Серверная часть (Node.js с библиотекой ws)

```javascript
const WebSocket = require('ws');

// Создание WebSocket-сервера
const wss = new WebSocket.Server({ port: 8080 });

// Обработка подключений
wss.on('connection', function connection(ws) {
    console.log('Новое подключение');
    
    // Отправка приветствия
    ws.send(JSON.stringify({ type: 'welcome', content: 'Добро пожаловать!' }));
    
    // Получение сообщений
    ws.on('message', function incoming(message) {
        const data = JSON.parse(message);
        console.log('Получено сообщение:', data);
        
        // Эхо-ответ
        ws.send(JSON.stringify({ type: 'echo', content: data.content }));
    });
    
    // Обработка закрытия соединения
    ws.on('close', function() {
        console.log('Соединение закрыто');
    });
});

// Рассылка сообщения всем клиентам
function broadcast(message) {
    wss.clients.forEach(function each(client) {
        if (client.readyState === WebSocket.OPEN) {
            client.send(JSON.stringify(message));
        }
    });
}
```

## Node.js

Node.js - это среда выполнения JavaScript на основе движка V8, позволяющая запускать JavaScript на сервере.

### Модульная система

#### CommonJS (традиционная для Node.js)

```javascript
// math.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};

// Или экспорт отдельных функций
// exports.add = add;
// exports.subtract = subtract;
```

```javascript
// app.js
const math = require('./math');
// Или с деструктуризацией
// const { add, subtract } = require('./math');

console.log(math.add(5, 3)); // 8
console.log(math.subtract(5, 3)); // 2
```

#### ES Modules (поддерживается в новых версиях Node.js)

```javascript
// math.mjs
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

// Или экспорт по умолчанию
// export default { add, subtract };
```

```javascript
// app.mjs
import { add, subtract } from './math.mjs';
// Или импорт всего модуля
// import * as math from './math.mjs';

console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
```

### Основные модули Node.js

#### Файловая система (fs)

```javascript
const fs = require('fs');

// Синхронное чтение
try {
    const data = fs.readFileSync('file.txt', 'utf8');
    console.log(data);
} catch (err) {
    console.error('Ошибка чтения файла:', err);
}

// Асинхронное чтение с коллбэком
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error('Ошибка чтения файла:', err);
        return;
    }
    console.log(data);
});

// Асинхронное чтение с промисами
const fsPromises = require('fs').promises;

async function readFileAsync() {
    try {
        const data = await fsPromises.readFile('file.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error('Ошибка чтения файла:', err);
    }
}

// Запись в файл
fs.writeFile('output.txt', 'Привет, мир!', 'utf8', (err) => {
    if (err) {
        console.error('Ошибка записи файла:', err);
        return;
    }
    console.log('Файл успешно записан');
});

// Работа с директориями
fs.mkdir('new-dir', { recursive: true }, (err) => {
    if (err) {
        console.error('Ошибка создания директории:', err);
        return;
    }
    console.log('Директория создана');
});

// Чтение содержимого директории
fs.readdir('.', (err, files) => {
    if (err) {
        console.error('Ошибка чтения директории:', err);
        return;
    }
    console.log('Файлы:', files);
});
```

#### HTTP-сервер

```javascript
const http = require('http');

// Создание простого HTTP-сервера
const server = http.createServer((req, res) => {
    // Получение информации о запросе
    const { method, url, headers } = req;
    console.log(`${method} ${url}`);
    
    // Обработка разных URL
    if (url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Главная страница');
    } else if (url === '/api/data') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Данные API' }));
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Страница не найдена');
    }
});

// Запуск сервера
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Сервер запущен на порту ${PORT}`);
});
```

#### Работа с путями (path)

```javascript
const path = require('path');

// Объединение путей
const fullPath = path.join(__dirname, 'subfolder', 'file.txt');
console.log(fullPath);

// Нормализация пути
console.log(path.normalize('/folder//subfolder/file.txt'));

// Получение компонентов пути
console.log(path.basename('/folder/file.txt')); // file.txt
console.log(path.dirname('/folder/file.txt')); // /folder
console.log(path.extname('/folder/file.txt')); // .txt

// Разбор пути
const pathInfo = path.parse('/folder/file.txt');
console.log(pathInfo);
// { root: '/', dir: '/folder', base: 'file.txt', ext: '.txt', name: 'file' }
```

#### События (EventEmitter)

```javascript
const EventEmitter = require('events');

// Создание эмиттера событий
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();

// Регистрация обработчика события
myEmitter.on('event', (arg1, arg2) => {
    console.log('Событие произошло:', arg1, arg2);
});

// Регистрация обработчика, который сработает только один раз
myEmitter.once('oneTimeEvent', () => {
    console.log('Это событие сработает только один раз');
});

// Генерация события
myEmitter.emit('event', 'arg1', 'arg2');
myEmitter.emit('oneTimeEvent');
myEmitter.emit('oneTimeEvent'); // Обработчик не сработает

// Удаление обработчика
function handler() {
    console.log('Обработчик события');
}
myEmitter.on('removeMe', handler);
myEmitter.removeListener('removeMe', handler);
// Или myEmitter.off('removeMe', handler); в новых версиях

// Получение списка слушателей
console.log(myEmitter.listeners('event'));

// Установка максимального количества слушателей
myEmitter.setMaxListeners(20); // По умолчанию 10
```

## JSDoc и типизация

JSDoc - это система документирования кода JavaScript с помощью комментариев, которая также может использоваться для статической типизации.

```javascript
/**
 * Представляет пользователя системы.
 * @typedef {Object} User
 * @property {string} id - Уникальный идентификатор пользователя
 * @property {string} name - Имя пользователя
 * @property {number} age - Возраст пользователя
 * @property {string[]} roles - Список ролей пользователя
 */

/**
 * Создает нового пользователя.
 * @param {string} name - Имя пользователя
 * @param {number} age - Возраст пользователя
 * @param {string[]} [roles=['user']] - Список ролей пользователя (по умолчанию ['user'])
 * @returns {User} Созданный пользователь
 * @throws {Error} Если возраст меньше 18
 */
function createUser(name, age, roles = ['user']) {
    if (age < 18) {
        throw new Error('Пользователь должен быть совершеннолетним');
    }
    
    return {
        id: generateId(),
        name,
        age,
        roles
    };
}

/**
 * Генерирует уникальный идентификатор.
 * @returns {string} Уникальный идентификатор
 * @private
 */
function generateId() {
    return Math.random().toString(36).substr(2, 9);
}

/**
 * Класс для работы с коллекцией пользователей.
 */
class UserService {
    /**
     * @type {User[]}
     * @private
     */
    #users = [];
    
    /**
     * Добавляет пользователя в коллекцию.
     * @param {User} user - Пользователь для добавления
     * @returns {void}
     */
    addUser(user) {
        this.#users.push(user);
    }
    
    /**
     * Находит пользователя по идентификатору.
     * @param {string} id - Идентификатор пользователя
     * @returns {User|null} Найденный пользователь или null
     */
    findById(id) {
        return this.#users.find(user => user.id === id) || null;
    }
    
    /**
     * Находит пользователей по роли.
     * @param {string} role - Роль для поиска
     * @returns {User[]} Список пользователей с указанной ролью
     */
    findByRole(role) {
        return this.#users.filter(user => user.roles.includes(role));
    }
}

/**
 * Функция высшего порядка для фильтрации пользователей.
 * @callback UserFilterCallback
 * @param {User} user - Пользователь для проверки
 * @returns {boolean} True, если пользователь соответствует критерию
 */

/**
 * Фильтрует пользователей по заданному критерию.
 * @param {User[]} users - Массив пользователей
 * @param {UserFilterCallback} filterFn - Функция фильтрации
 * @returns {User[]} Отфильтрованный массив пользователей
 */
function filterUsers(users, filterFn) {
    return users.filter(filterFn);
}

/**
 * Перечисление возможных статусов задачи.
 * @enum {string}
 */
const TaskStatus = {
    /** Задача создана */
    CREATED: 'created',
    /** Задача в процессе выполнения */
    IN_PROGRESS: 'in_progress',
    /** Задача завершена */
    COMPLETED: 'completed',
    /** Задача отменена */
    CANCELLED: 'cancelled'
};

/**
 * @template T
 * @param {T} value - Значение для обработки
 * @returns {T} То же значение
 */
function identity(value) {
    return value;
}

// Использование с JSDoc-типами
const user = createUser('John', 25, ['admin', 'moderator']);
const service = new UserService();
service.addUser(user);

const admins = service.findByRole('admin');
const youngAdmins = filterUsers(admins, user => user.age < 30);

const numberIdentity = identity(42); // T = number
const stringIdentity = identity('hello'); // T = string
```

### Интеграция с TypeScript

Современные IDE (VSCode, WebStorm) могут использовать JSDoc для проверки типов даже без TypeScript:

```javascript
// @ts-check

/**
 * @typedef {Object} Point
 * @property {number} x - Координата X
 * @property {number} y - Координата Y
 */

/**
 * Вычисляет расстояние между двумя точками.
 * @param {Point} p1 - Первая точка
 * @param {Point} p2 - Вторая точка
 * @returns {number} Расстояние между точками
 */
function distance(p1, p2) {
    return Math.sqrt(Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2));
}

// Ошибка будет подсвечена в IDE
distance({ x: 0, y: 0 }, { x: '1', y: 0 }); // Ошибка: Type 'string' is not assignable to type 'number'
```

## Тестирование: Jest и Mocha

### Jest

Jest - полноценный фреймворк для тестирования JavaScript с встроенным средством создания моков, ассертами и покрытием кода.

#### Установка

```bash
npm install --save-dev jest
```

#### Базовые тесты

```javascript
// math.js
function sum(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

module.exports = { sum, multiply };
```

```javascript
// math.test.js
const { sum, multiply } = require('./math');

test('сложение 1 + 2 равно 3', () => {
    expect(sum(1, 2)).toBe(3);
});

test('умножение 2 * 3 равно 6', () => {
    expect(multiply(2, 3)).toBe(6);
});

// Группировка тестов
describe('математические функции', () => {
    test('сложение отрицательных чисел', () => {
        expect(sum(-1, -2)).toBe(-3);
    });
    
    test('умножение отрицательных чисел', () => {
        expect(multiply(-2, -3)).toBe(6);
    });
});
```

#### Асинхронное тестирование

```javascript
// api.js
async function fetchUser(id) {
    const response = await fetch(`https://api.example.com/users/${id}`);
    if (!response.ok) {
        throw new Error('Ошибка получения пользователя');
    }
    return response.json();
}

module.exports = { fetchUser };
```

```javascript
// api.test.js
const { fetchUser } = require('./api');

// С async/await
test('получение пользователя', async () => {
    const user = await fetchUser(1);
    expect(user).toHaveProperty('name');
    expect(user.id).toBe(1);
});

// С Promise
test('получение пользователя (Promise)', () => {
    return fetchUser(1).then(user => {
        expect(user).toHaveProperty('name');
    });
});

// Проверка ошибок
test('ошибка при получении несуществующего пользователя', async () => {
    expect.assertions(1); // Убедимся, что асинхронный код выполнится
    try {
        await fetchUser(999);
    } catch (e) {
        expect(e.message).toMatch(/ошибка/i);
    }
});
```

#### Моки и шпионы

```javascript
// user-service.js
const api = require('./api');

class UserService {
    async getUser(id) {
        const user = await api.fetchUser(id);
        return {
            ...user,
            fullName: `${user.firstName} ${user.lastName}`
        };
    }
}

module.exports = UserService;
```

```javascript
// user-service.test.js
const UserService = require('./user-service');
const api = require('./api');

// Мок всего модуля
jest.mock('./api');

test('getUser возвращает пользователя с полным именем', async () => {
    // Настройка мока
    api.fetchUser.mockResolvedValue({
        id: 1,
        firstName: 'John',
        lastName: 'Doe'
    });
    
    const userService = new UserService();
    const user = await userService.getUser(1);
    
    // Проверки
    expect(api.fetchUser).toHaveBeenCalledWith(1);
    expect(user.fullName).toBe('John Doe');
});

// Шпион для отдельного метода
test('шпион для метода', () => {
    const calculator = {
        add: (a, b) => a + b
    };
    
    // Создаем шпиона
    const spy = jest.spyOn(calculator, 'add');
    
    calculator.add(2, 3);
    
    expect(spy).toHaveBeenCalledWith(2, 3);
    expect(spy).toHaveBeenCalledTimes(1);
    
    // Восстанавливаем оригинальную реализацию
    spy.mockRestore();
});
```

#### Снимки (Snapshots)

```javascript
// component.js
function generateUserProfile(user) {
    return {
        header: `Профиль: ${user.name}`,
        content: `Возраст: ${user.age}, Email: ${user.email}`,
        footer: `Последний вход: ${user.lastLogin}`
    };
}

module.exports = { generateUserProfile };
```

```javascript
// component.test.js
const { generateUserProfile } = require('./component');

test('генерирует правильный профиль пользователя', () => {
    const user = {
        name: 'John Doe',
        age: 30,
        email: 'john@example.com',
        lastLogin: '2023-01-15'
    };
    
    const profile = generateUserProfile(user);
    
    // Создает или сравнивает со снимком
    expect(profile).toMatchSnapshot();
});
```

### Mocha

Mocha - гибкий фреймворк для тестирования JavaScript, который обычно используется с библиотекой утверждений, например, Chai.

#### Установка

```bash
npm install --save-dev mocha chai
```

#### Базовые тесты

```javascript
// math.js
function sum(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

module.exports = { sum, multiply };
```

```javascript
// math.test.js
const { expect } = require('chai');
const { sum, multiply } = require('./math');

describe('Математические функции', () => {
    it('сложение 1 + 2 равно 3', () => {
        expect(sum(1, 2)).to.equal(3);
    });
    
    it('умножение 2 * 3 равно 6', () => {
        expect(multiply(2, 3)).to.equal(6);
    });
    
    // Вложенные группы
    describe('с отрицательными числами', () => {
        it('сложение отрицательных чисел', () => {
            expect(sum(-1, -2)).to.equal(-3);
        });
        
        it('умножение отрицательных чисел', () => {
            expect(multiply(-2, -3)).to.equal(6);
        });
    });
});
```

#### Асинхронное тестирование

```javascript
// api.js
async function fetchUser(id) {
    const response = await fetch(`https://api.example.com/users/${id}`);
    if (!response.ok) {
        throw new Error('Ошибка получения пользователя');
    }
    return response.json();
}

module.exports = { fetchUser };
```

```javascript
// api.test.js
const { expect } = require('chai');
const { fetchUser } = require('./api');

describe('API', () => {
    // С async/await
    it('получение пользователя', async () => {
        const user = await fetchUser(1);
        expect(user).to.have.property('name');
        expect(user.id).to.equal(1);
    });
    
    // С Promise
    it('получение пользователя (Promise)', () => {
        return fetchUser(1).then(user => {
            expect(user).to.have.property('name');
        });
    });
    
    // С done
    it('получение пользователя (callback)', (done) => {
        fetchUser(1)
            .then(user => {
                expect(user).to.have.property('name');
                done();
            })
            .catch(done);
    });
    
    // Проверка ошибок
    it('ошибка при получении несуществующего пользователя', async () => {
        try {
            await fetchUser(999);
            // Если дошли сюда, то тест должен провалиться
            expect.fail('Должна была возникнуть ошибка');
        } catch (e) {
            expect(e.message).to.match(/ошибка/i);
        }
    });
});
```

#### Моки с Sinon

```bash
npm install --save-dev sinon
```

```javascript
// user-service.js
const api = require('./api');

class UserService {
    async getUser(id) {
        const user = await api.fetchUser(id);
        return {
            ...user,
            fullName: `${user.firstName} ${user.lastName}`
        };
    }
}

module.exports = UserService;
```

```javascript
// user-service.test.js
const { expect } = require('chai');
const sinon = require('sinon');
const UserService = require('./user-service');
const api = require('./api');

describe('UserService', () => {
    // Восстанавливаем оригинальные функции после каждого теста
    afterEach(() => {
        sinon.restore();
    });
    
    it('getUser возвращает пользователя с полным именем', async () => {
        // Создаем заглушку
        const stub = sinon.stub(api, 'fetchUser').resolves({
            id: 1,
            firstName: 'John',
            lastName: 'Doe'
        });
        
        const userService = new UserService();
        const user = await userService.getUser(1);
        
        // Проверки
        expect(stub.calledWith(1)).to.be.true;
        expect(user.fullName).to.equal('John Doe');
    });
    
    // Шпион
    it('шпион для метода', () => {
        const calculator = {
            add: (a, b) => a + b
        };
        
        // Создаем шпиона
        const spy = sinon.spy(calculator, 'add');
        
        calculator.add(2, 3);
        
        expect(spy.calledWith(2, 3)).to.be.true;
        expect(spy.callCount).to.equal(1);
    });
});
```

## Современные возможности JavaScript

### Оператор опциональной последовательности (?.)

```javascript
const user = {
    profile: {
        address: {
            city: 'Москва'
        }
    }
};

// Старый способ
const city1 = user && user.profile && user.profile.address && user.profile.address.city;

// Новый способ
const city2 = user?.profile?.address?.city;

// Работает с методами
const result = user.getDetails?.(); // Не вызовет ошибку, если метода нет

// Работает с массивами
const firstItem = array?.[0];
```

### Оператор нулевого слияния (??)

```javascript
// Старый способ (проблема: 0 и '' считаются ложными)
const value1 = someValue || 'значение по умолчанию';

// Новый способ (null и undefined заменяются на значение по умолчанию)
const value2 = someValue ?? 'значение по умолчанию';

// Примеры
const count = 0;
console.log(count || 10); // 10 (0 считается ложным)
console.log(count ?? 10); // 0 (0 не является null или undefined)

const name = '';
console.log(name || 'Гость'); // "Гость" (пустая строка считается ложной)
console.log(name ?? 'Гость'); // "" (пустая строка не является null или undefined)
```

### Логическое присваивание

```javascript
// Логическое И (&&=)
let x = 1;
x &&= 2; // Эквивалентно: x = x && 2
console.log(x); // 2

let y = 0;
y &&= 2; // Эквивалентно: y = y && 2
console.log(y); // 0 (т.к. 0 && 2 = 0)

// Логическое ИЛИ (||=)
let a = 0;
a ||= 1; // Эквивалентно: a = a || 1
console.log(a); // 1

let b = 2;
b ||= 1; // Эквивалентно: b = b || 1
console.log(b); // 2 (т.к. 2 || 1 = 2)

// Нулевое слияние (??=)
let c = null;
c ??= 'значение'; // Эквивалентно: c = c ?? 'значение'
console.log(c); // "значение"

let d = 0;
d ??= 'значение'; // Эквивалентно: d = d ?? 'значение'
console.log(d); // 0 (т.к. 0 ?? 'значение' = 0)
```

### Приватные поля и методы классов

```javascript
class BankAccount {
    // Приватное поле (с #)
    #balance = 0;
    
    // Приватный метод
    #validateAmount(amount) {
        if (amount <= 0) {
            throw new Error('Сумма должна быть положительной');
        }
    }
    
    constructor(initialBalance = 0) {
        if (initialBalance > 0) {
            this.#balance = initialBalance;
        }
    }
    
    deposit(amount) {
        this.#validateAmount(amount);
        this.#balance += amount;
        return this.#balance;
    }
    
    get balance() {
        return this.#balance;
    }
    
    // Статический приватный метод
    static #generateAccountNumber() {
        return Math.floor(Math.random() * 1000000);
    }
    
    static createAccount(initialBalance) {
        const account = new BankAccount(initialBalance);
        account.accountNumber = this.#generateAccountNumber();
        return account;
    }
}
```

### Top-level await

В современных модулях ES можно использовать await на верхнем уровне, без оборачивания в async-функцию.

```javascript
// module.js
const response = await fetch('https://api.example.com/data');
const data = await response.json();

export { data };
```

### Метод at() для индексации массивов

```javascript
const array = [10, 20, 30, 40, 50];

// Доступ к последнему элементу
const last = array[array.length - 1]; // Старый способ
const last2 = array.at(-1); // Новый способ: 50

// Доступ к предпоследнему элементу
const secondLast = array.at(-2); // 40

// Работает и с положительными индексами
const first = array.at(0); // 10
```

### Метод replaceAll()

```javascript
const string = 'Привет, мир! Привет, JavaScript!';

// Старый способ (с регулярным выражением)
const newString1 = string.replace(/Привет/g, 'Здравствуй');

// Новый способ
const newString2 = string.replaceAll('Привет', 'Здравствуй');
```

### Promise.any и AggregateError

```javascript
// Promise.any возвращает первый успешно выполненный промис
try {
    const result = await Promise.any([
        fetch('https://api1.example.com/data').then(r => r.json()),
        fetch('https://api2.example.com/data').then(r => r.json()),
        fetch('https://api3.example.com/data').then(r => r.json())
    ]);
    
    console.log('Первый успешный результат:', result);
} catch (error) {
    // AggregateError содержит все ошибки, если все промисы отклонены
    console.error('Все запросы завершились с ошибкой:', error.errors);
}
```

### Numeric Separators

```javascript
// Большие числа стали более читаемыми
const billion = 1_000_000_000;
const binary = 0b1010_0001_1000_0101;
const hex = 0xFF_EC_DE_5E;
```

### Финализаторы для классов

```javascript
class ResourceHandler {
    constructor() {
        this.resource = acquireResource();
    }
    
    // Метод, вызываемый перед сборкой мусора
    // Обратите внимание: нет гарантии, что он будет вызван!
    [Symbol.for('nodejs.util.inspect.custom')] = () => {
        return 'ResourceHandler';
    };
}
```

### Глобальный this

```javascript
// Работает в любом окружении (браузер, Node.js, WebWorker)
const globalObject = globalThis;

// В браузере это window
// В Node.js это global
// В WebWorker это self
```

### Intl API для интернационализации

```javascript
// Форматирование дат
const date = new Date();
const formatter = new Intl.DateTimeFormat('ru', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
});
console.log(formatter.format(date)); // "понедельник, 15 января 2023 г."

// Форматирование чисел
const number = 123456.789;
console.log(new Intl.NumberFormat('ru').format(number)); // "123 456,789"
console.log(new Intl.NumberFormat('ru', { style: 'currency', currency: 'RUB' }).format(number)); // "123 456,79 ₽"

// Сравнение строк с учетом локали
const collator = new Intl.Collator('ru');
['я', 'б', 'в', 'а'].sort(collator.compare); // ["а", "б", "в", "я"]

// Относительное время
const rtf = new Intl.RelativeTimeFormat('ru');
console.log(rtf.format(-1, 'day')); // "1 день назад"
console.log(rtf.format(2, 'month')); // "через 2 месяца"

// Списки
const listFormat = new Intl.ListFormat('ru', { style: 'long', type: 'conjunction' });
console.log(listFormat.format(['яблоки', 'груши', 'бананы'])); // "яблоки, груши и бананы"
```

---
