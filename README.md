# Повна шпаргалка JavaScript для Java-розробника

## Зміст
1. [Типи даних](#типи-даних)
2. [Змінні: var, let, const](#змінні-var-let-const)
3. [Функції](#функції)
4. [Об'єкти та ООП](#объекты-и-ооп)
5. [Прототипне успадкування](#прототипне успадкування)
6. [Класи в ES6+](#класи-в-es6)
7. [Області видимості та контекст this](#області-видимості-і-контекст-this)
8. [Замикання](#замикання)
9. [Методи функцій: bind, call, apply](#методи-функцій-bind-call-apply)
10. [Асинхронне програмування](#асинхронне-програмування)
11. [Колекції](#колекції)
12. [Обробка винятків](#обробка-виключень)
13. [Управління пам'яттю та складання сміття](#управління-пам'яттю-і-складання-сміття)
14. [Event Loop](#event-loop)
15. [Робота з DOM](#робота-з-dom)
16. [Web Workers](#web-workers)
17. [Web Storage API](#web-storage-api)
18. [WebSockets](#websockets)
19. [Node.js](#nodejs)
20. [JSDoc та типізація](#jsdoc-і-типізація)
21. [Тестування: Jest та Mocha](#тестування-jest-і-mocha)
22. [Сучасні можливості JavaScript](#сучасні-можливості-javascript)

---

## Типи даних

JavaScript має 8 основних типів даних:

### Примітивні типи:
1. **Number** - числа (цілі та з плаваючою точкою)
2. **BigInt** - довільно великі цілі числа
3. **String** - рядки
4. **Boolean** - логічний тип (true/false)
5. **null** - спеціальне значення, що означає "порожньо"
6. **undefined** - спеціальне значення, що означає "не визначено"
7. **Symbol** - унікальні ідентифікатори

### Об'єктний тип:
8. **Object** - об'єкти, масиви, функції тощо.

```javascript
// Приклади типів даних
const num = 42;
const bigInt = 9007199254740991n; // BigInt
const str = "Привіт, мир!";
const bool = true;
const nullValue = null;
let undefinedValue;
const sym = Symbol("id");
const obj = {name: "John", age: 30};

// Перевірка типу
console.log(typeof num); // "number"
console.log(typeof bigInt); // "bigint"
console.log(typeof str); // "string"
console.log(typeof bool); // "boolean"
console.log(typeof nullValue); // "object" (це історична помилка в JS)
console.log(typeof undefinedValue); // "undefined"
console.log(typeof sym); // "symbol"
console.log(typeof obj); // "object"
console.log(typeof function(){}); // "function"

// Перетворення типів
console.log(String(42)); // "42"
console.log(Number("42")); // 42
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("")); // false
console.log(Boolean("0")); // true (непорожній рядок)
````

## Змінні: var, let, const

### var
- має функціональну область видимості
- Піднімається (hoisting) - оголошення змінної переміщується на початок функції/скрипту
- Можна перевизначати у тій самій області видимості

### let
- має блокову область видимості
- Піднімається, але не можна використовувати до оголошення (тимчасова мертва зона)
- Не можна перевизначати у тій же області видимості

### const
- має блокову область видимості
- Піднімається, але не можна використовувати до оголошення
- Не можна перевизначати значення
- Для об'єктів та масивів можна змінювати внутрішній вміст

```javascript
// var
function example() { 
console.log(x); // undefined (hoisting) 
var x = 10; 
var x = 20; // Можна перевизначити 
console.log(x); // 20
}

// let
function example2() { 
// console.log(y); // ReferenceError: Cannot access 'y' before initialization 
let y = 10; 
// let y = 20; //SyntaxError: Identifier 'y' already been declared 
y = 20; // Можна змінити значення 
console.log(y); // 20 

if (true) { 
let y = 30; // Інша змінна в іншій області видимості 
console.log(y); // 30 
} 

console.log(y); // 20
}

// const
function example3() { 
const z = 10; 
// z = 20; // TypeError: Assignment to constant variable 

const obj = {name: "John"}; 
obj.name = "Jane"; // Можна змінювати властивості об'єкта 
console.log(obj); // { name: "Jane" } 

// obj = {}; // TypeError: Assignment to constant variable
}
````

## Функції

JavaScript функції є об'єктами першого класу і можуть бути:
- присвоєно змінним
- Передано як аргументи
- Повернені з інших функцій

### Види функцій:

#### 1. Оголошення функції (Function Declaration)
```javascript
function add(a, b) { 
return a + b;
}
````

#### 2. Функціональний вираз (Function Expression)
```javascript
const subtract = function(a, b) { 
return a - b;
};
````

#### 3. Стрілецькі функції (Arrow Functions)
```javascript
const multiply = (a, b) => a * b;

// Багаторядкові стрілочні функції
const divide = (a, b) => { 
if (b === 0) throw new Error("Поділ на нуль"); 
return a/b;
};
````

#### 4. Методи об'єктів
```javascript
const calculator = { 
add: function(a, b) { 
return a + b; 
}, 
// Скорочений синтаксис методів (ES6) 
subtract(a, b) { 
return a - b; 
}
};
````

#### 5. Конструктори функцій
```javascript
function Person(name, age) { 
this.name = name; 
this.age = age; 
this.greet = function() { 
return `Привіт, мене звуть ${this.name}`; 
};
}

const john = New Person ("John", 30);
````

#### 6. Генератори
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
````

### Параметри функцій:

#### Параметри за замовчуванням
```javascript
function greet(name = "Гість") { 
return `Привіт, ${name}!`;
}
````

#### Rest-параметри
```javascript
function sum(...numbers) { 
return numbers.reduce((total, num) => total + num, 0);
}
````

#### Деструктуризація параметрів
```javascript
function displayPerson({ name, age, job = "Не вказано" }) { 
console.log(`${name}, ${age} років, ${job}`);
}

displayPerson({ name: "Ганна", age: 28});
````

## Об'єкти та ОВП

### Створення об'єктів

```javascript
// Літерал об'єкта
const person = { 
firstName: "John", 
lastName: "Doe", 
age: 30, 
greet() { 
return `Привіт, мене звуть ${this.firstName} ${this.lastName}`; 
}
};

// Конструктор Object
const car = новий Object();
car.make = "Toyota";
car.model = "Corolla";
car.year = 2020;

// Object.create()
const animal = { 
type: "Невідомо", 
makeSound() { 
console.log("Якийсь звук"); 
}
};

const dog = Object.create(animal);
dog.type = "Собака";
dog.makeSound = function() { 
console.log("Гав");
};
````

### Властивості об'єктів

```javascript
// Геттери та сетери
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

// Дескриптори властивостей
Object.defineProperty(product, "discount", { 
get() { 
return this._discount || 0; 
}, 
set(value) { 
if (value < 0 || value > 100) { 
throw new Error("Знижка повинна бути від 0 до 100"); 
} 
this._discount = value; 
}
});
````

## Прототипне успадкування

У JavaScript успадкування ґрунтується на прототипах, а не на класах (хоча класи в ES6+ – це синтаксичний цукор над прототипами).

```javascript
// Базовий об'єкт
function Animal(name) { 
this.name = name;
}

Animal.prototype.makeSound = function() { 
console.log("Якийсь звук");
};

// Спадкування
function Dog(name, breed) { 
Animal.call(this, name); // Виклик конструктора батька 
this.breed = breed;
}

// Установка прототипу
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // Відновлення конструктора

// Перевизначення методу
Dog.prototype.makeSound = function() { 
console.log("Гав");
};

// Додавання нового методу
Dog.prototype.fetch = function() { 
console.log(`${this.name} приніс палицю`);
};

const rex = new Dog ("Рекс", "Вівчарка");
rex.makeSound(); // "Гав"
rex.fetch(); // "Рекс приніс палицю"
console.log(rex instanceof Dog); // true
console.log(rex instanceof Animal); // true
````

## Класи в ES6+

ES6 ввів синтаксис класів, який полегшує роботу з прототипним успадкуванням.

```javascript
// Базовий клас
class Animal { 
constructor(name) { 
this.name = name; 
} 

makeSound() { 
console.log("Якийсь звук"); 
} 

//Статичний метод 
static isAnimal(obj) { 
return obj instanceof Animal; 
}
}

// Спадкування
class Dog extends Animal { 
constructor(name, breed) { 
super(name); // Виклик конструктора батька 
this.breed = breed; 
} 

// Перевизначення методу 
makeSound() { 
console.log("Гав"); 
} 

//Новий метод 
fetch() { 
console.log(`${this.name} приніс палицю`); 
}
}

const rex = new Dog ("Рекс", "Вівчарка");
rex.makeSound(); // "Гав"
console.log(Animal.isAnimal(rex)); // true
````

### Приватні поля та методи (ES2020+)

```javascript
class BankAccount { 
// Приватне поле 
# Balance = 0; 

// Приватний метод 
#validateAmount(amount) { 
if (amount <= 0) { 
throw new Error("Сума має бути позитивною"); 
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
throw new Error("Недостатньо коштів"); 
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
// console.log(account.#balance); // SyntaxError: Приватна філія '#balance' повинна бути зафіксована в an enclosing class
````

### Статичні поля та методи

```javascript
class MathUtils { 
//Статичне поле 
static PI = 3.14159; 

//Статичний метод 
static square(x) { 
return x * x; 
} 

// Статичний приватний метод 
static #calculateArea(radius) { 
return this.PI * radius * radius; 
} 

static circleArea(radius) { 
if (radius <= 0) { 
throw new Error("Радіус має бути позитивним"); 
} 
return this.#calculateArea(radius); 
}
}

console.log(MathUtils.PI); // 3.14159
console.log(MathUtils.square(5)); // 25
console.log(MathUtils.circleArea(2)); // ~12.57
````

## Області видимості та контекст this

### Області видимості

1. **Глобальна область** - змінні, доступні скрізь
2. **Функціональна область** - змінні, доступні всередині функції
3. **Блочна область** - змінні, доступні всередині блоку (let, const)

```javascript
// Глобальна область
const globalVar = "Я глобальна";

function outerFunction() { 
// Функціональна область 
const outerVar = "Я із зовнішньої функції"; 

function innerFunction() { 
// Вкладена функціональна область 
const innerVar = "Я із внутрішньої функції"; 
console.log(globalVar); // Доступна 
console.log(outerVar); // Доступна 
console.log(innerVar); // Доступна 
} 

innerFunction(); 
console.log(globalVar); // Доступна 
console.log(outerVar); // Доступна 
// console.log(innerVar); // ReferenceError: innerVar is not defined
}

if (true) { 
//Блочна область 
let blockVar = "Я з блоку"; 
console.log(blockVar); // Доступна
}
// console.log(blockVar); // ReferenceError: blockVar is not defined
````

### Контекст this

Значення `this` залежить від того, як викликається функція:

1. **Глобальний контекст**: `this` вказує на глобальний об'єкт (у браузері - `window`, в Node.js - `global`)
2. **Метод об'єкта**: `this` вказує на об'єкт, у якого викликаний метод
3. **Конструктор**: `this` вказує на створюваний екземпляр
4. **Явна вказівка**: через `call`, `apply`, `bind`
5. **Стрілкові функції**: `this` береться із зовнішньої (лексичної) області видимості

```javascript
// Глобальний контекст
console.log(this); // window у браузері

// Метод об'єкта
const user = { 
name: "John", 
greet() { 
console.log(`Привіт, я ${this.name}`); 
}
};
user.greet(); // "Привіт, я John"

// Втрата контексту
const greet = user.greet;
// greet(); // "Привіт, я undefined" (this = window)

// Конструктор
function User(name) { 
this.name = name; 
this.sayHi = function() { 
console.log(`Привіт, я ${this.name}`); 
};
}
const john = new User("John");
john.sayHi(); // "Привіт, я John"

// Стрілецькі функції зберігають this
const obj = { 
name: "Object", 
regularMethod: function() { 
console.log(`Regular: ${this.name}`); 

// Це з regularMethod 
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
````

## Замикання

Замикання - це функція, яка запам'ятовує свою лексичну область видимості, навіть коли виконується поза цією областю.

```javascript
// Просте замикання
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

// Замикання з параметрами
function createMultiplier(factor) { 
return function(number) { 
return number * factor; 
};
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Практичне застосування: приватні змінні
function createBankAccount(initialBalance = 0) { 
let balance = initialBalance; 

return { 
deposit(amount) { 
balance + = amount; 
return balance; 
}, 
withdraw(amount) { 
if (amount > balance) { 
throw new Error("Недостатньо коштів"); 
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
// balance недоступний ззовні
````

## Методи функцій: bind, call, apply

Ці методи дозволяють явно вказати значення this при виклику функції.

### call

Викликає функцію із зазначеним значенням `this` та аргументами, перерахованими через кому.

```javascript
function greet(greeting, punctuation) { 
console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = {name: "John"};

greet.call(person, "Привіт", "!"); // "Привіт, John!"
````

### Apply

Схожий на call, але приймає аргументи у вигляді масиву.

```javascript
function greet(greeting, punctuation) { 
console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = {name: "John"};

greet.apply(person, ["Здрастуйте", "."]); // "Доброго дня, John."
````

### bind

Створює нову функцію з прив'язаним значенням `this` і, опціонально, із встановленими аргументами.

```javascript
function greet(greeting, punctuation) { 
console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = {name: "John"};

const greetJohn = greet.bind(person);
greetJohn("Привіт", "!"); // "Привіт, John!"

const sayHiToJohn = greet.bind(person, "Привіт");
sayHiToJohn("?"); // "Привіт, John?"

const sayHiToJohnExcitedly = greet.bind(person, "Привіт", "!!!");
sayHiToJohnExcitedly(); // "Привіт, John!!!"
````

## Асинхронне програмування

### Callbacks

Найстаріший підхід до асинхронного програмування JavaScript.

```javascript
function fetchData(callback) { 
setTimeout(() => { 
const data = { name: "John", age: 30}; 
callback(null, data); 
}, 1000);
}

fetchData((error, data) => { 
if (error) { 
console.error("Помилка:", error); 
return; 
} 
console.log("Дані:", data);
});
````

### Promises

Promises (обіцянки) – це об'єкти, що становлять результат асинхронної операції.

```javascript
function fetchData() { 
return new Promise((resolve, reject) => { 
setTimeout(() => { 
const success = true; 
if (success) { 
resolve({ name: "John", age: 30}); 
} else { 
reject(new Error("Не вдалося отримати дані")); 
} 
}, 1000); 
});
}

fetchData() 
.then(data => { 
console.log("Дані:", data); 
return data.name; 
}) 
.then(name => { 
console.log("Ім'я:", name); 
}) 
.catch(error => { 
console.error("Помилка:", error); 
}) 
.finally(() => { 
console.log("Операція завершена"); 
});

// Паралельне виконання промісів
Promise.all([ 
fetchData(), 
fetchData()
]) 
.then(([data1, data2]) => { 
console.log("Всі дані:", data1, data2); 
}) 
.catch(error => { 
console.error("Хоч би один проміс відхилений:", error); 
});

// Виконання першого успішного промісу
Promise.race([ 
fetchData(), 
new Promise((resolve) => setTimeout(() => resolve("timeout"), 500))
]) 
.then(result => { 
console.log("Перший результат:", result); 
});

// Виконання всіх промісів (без відхилення за помилки)
Promise.allSettled([ 
fetchData(), 
Promise.reject(new Error("Помилка"))
]) 
.then(results => { 
results.forEach(result => { 
if (result.status === "fulfilled") { 
console.log("Успішно:", result.value); 
} else { 
console.log("Помилка:", result.reason); 
} 
}); 
});

// Повертає перший успішний проміс
Promise.any([ 
Promise.reject(new Error("Помилка 1")), 
fetchData(), 
Promise.reject(new Error("Помилка 2"))
]) 
.then(result => { 
console.log("Перший успішний результат:", result); 
}) 
.catch(error => { 
console.log("Усі проміси відхилені:", error); 
});
````

### Async/Await

Синтаксичний цукор над промісами робить асинхронний код схожим на синхронний.

```javascript
async function getData() { 
try { 
const data = await fetchData(); 
console.log("Дані:", data); 

const name = data.name; 
console.log("Ім'я:", name); 

return name; 
} catch (error) { 
console.error("Помилка:", error); 
throw error; // Прокидаємо помилку далі 
} finally { 
console.log("Операція завершена"); 
}
}

// Виклик асинхронної функції
getData() 
.then(name => { 
console.log("Підсумкове ім'я:", name); 
}) 
.catch(error => { 
console.error("Помилка getData:", error); 
});

// Паралельне виконання з async/await
async function getMultipleData() { 
try { 
// Запускаємо обидва запити паралельно 
const dataPromise1 = fetchData(); 
const dataPromise2 = fetchData(); 

// Чекаємо на виконання обох 
const [data1, data2] = await Promise.all([dataPromise1, dataPromise2]); 

console.log("Дані 1:", data1); 
console.log("Дані 2:", data2); 
} catch (error) { 
console.error("Помилка при отриманні даних:", error); 
}
}
````

## Колекції

### Array (Массиви)

```javascript
// Створення масиву
const numbers = [1, 2, 3, 4, 5];
const empty = новий Array(5); //[empty × 5]
const filled = Array(5).fill(0); // [0, 0, 0, 0, 0]

// Доступ до елементів
console.log(numbers[0]); // 1
numbers[1] = 10;
console.log(numbers); // [1, 10, 3, 4, 5]

// Основні методи
numbers.push(6); // Додає до кінця
numbers.pop(); // Видаляє з кінця
numbers.unshift(0); // Додає на початок
numbers.shift(); // Видаляє з початку
numbers.splice(2, 1, 'a', 'b'); // Видаляє 1 елемент з індексу 2 та вставляє нові
numbers.slice(1, 3); // Повертає новий масив [індекс1, індекс2)
numbers.concat([7, 8]); // Об'єднує масиви

// Пошук елементів
numbers.indexOf(3); // Індекс першого входження або -1
numbers.lastIndexOf(3); // Індекс останнього входження або -1
numbers.includes(3); // true/false
numbers.find(x => x > 3); // Перший елемент, що задовольняє умову
numbers.findIndex(x => x > 3); // Індекс першого елемента, що задовольняє умову

// Ітерація
numbers.forEach(x => console.log(x));

// Перетворення
const doubled = numbers.map (x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, x) => acc + x, 0);
const allPositive = numbers.every(x => x > 0);
const someNegative = numbers.some(x => x < 0);

// Сортування
numbers.sort((a, b) => a - b); // За зростанням
numbers.reverse(); // Звернення порядку

// Перетворення на рядок
numbers.join(', '); // "1, 10, 3, 4, 5"

// Деструктуризація
const [first, second, ... rest] = numbers;

// Spread оператор
const numbersCopy=[...numbers];

// Нові методи (ES2022+)
numbers.at(-1); // Останній елемент (5)
````

### Map

Колекція пар ключ-значення з довільними типами ключів.

```javascript
// Створення Map
const userRoles = New Map();

// Додавання елементів
userRoles.set('john', 'admin');
userRoles.set('jane', 'editor');
userRoles.set('bob', 'viewer');

// Ініціалізація з масивом пар
const userRoles2 = New Map([ 
['john', 'admin'], 
['jane', 'editor'], 
['bob', 'viewer']
]);

// Отримання значень
console.log(userRoles.get('john')); // "admin"
console.log(userRoles.has('jane')); // true
console.log(userRoles.size); // 3

// Видалення
userRoles.delete('bob');
//userRoles.clear(); // Видаляє всі елементи

// Ітерація
for (const [user, role] of userRoles) { 
console.log(`${user}: ${role}`);
}

userRoles.forEach((role, user) => { 
console.log(`${user}: ${role}`);
});

// Отримання ключів, значень, пар
const users = [...userRoles.keys()];
const roles = [...userRoles.values()];
const entries = [...userRoles.entries()];

// Об'єкти як ключі
const userMap = New Map();
const user1 = {name: 'John'};
const user2 = {name: 'Jane'};

userMap.set(user1, 'admin');
userMap.set(user2, 'editor');

console.log(userMap.get(user1)); // "admin"
console.log(userMap.get({name: 'John'})); // undefined (інший об'єкт)
````

### Set

Колекція унікальних значень будь-якого типу.

```javascript
// Створення Set
const uniqueNumbers = new Set([1, 2, 3, 3, 4, 4, 5]);
console.log(uniqueNumbers); // Set(5) {1, 2, 3, 4, 5}

// Додавання елементів
uniqueNumbers.add(6);
uniqueNumbers.add(1); // Ігнорується, т.к. 1 вже є

// Перевірка наявності
console.log(uniqueNumbers.has(3)); // true
console.log(uniqueNumbers.size); // 6

// Видалення
uniqueNumbers.delete(4);
// uniqueNumbers.clear(); // Видаляє всі елементи

// Ітерація
for (const num of uniqueNumbers) { 
console.log(num);
}

uniqueNumbers.forEach(num => { 
console.log(num);
});

// Перетворення на масив
const numbersArray = [... uniqueNumbers];

// Практичне застосування: видалення дублікатів із масиву
const withDuplicates = [1, 2, 2, 3, 4, 4, 5];
const withoutDuplicates = [...new Set(withDuplicates)];
````

### WeakMap та WeakSet

Спеціальні версії Map і Set із "слабкими" посиланнями на об'єкти.

```javascript
// WeakMap
const weakMap = новий WeakMap();
let obj = {name: "John"};

weakMap.set(obj, "metadata");
console.log(weakMap.get(obj)); // "metadata"

obj = null; // Тепер об'єкт може бути зібраний збирачем сміття

// WeakSet
const weakSet = новий WeakSet();
let user = {id: 1};

weakSet.add(user);
console.log(weakSet.has(user)); // true

user = null; // Тепер об'єкт може бути зібраний збирачем сміття

// Відмінності від Map та Set:
// 1. Ключами може бути лише об'єкти
// 2. Немає методів keys(), values(), entries(), size, forEach()
// 3. Не можна перебрати вміст
// 4. Об'єкти видаляються автоматично за відсутності інших посилань
````

### Array Buffer і Typed Arrays

Для роботи з бінарними даними.

```javascript
// ArrayBuffer - безперервна область пам'яті фіксованої довжини
const buffer = новий ArrayBuffer(16); // 16 байт

// TypedArray - подання ArrayBuffer як масиву певного типу
const int32View = новий Int32Array(buffer);
int32View[0] = 42;
console.log(int32View[0]); // 42

// Різні уявлення одного буфера
const int8View = новий Int8Array(buffer);
console.log(int8View[0]); // 42 (молодший байт від 42)

// Типи типізованих масивів:
// Int8Array, Uint8Array, Uint8ClampedArray
// Int16Array, Uint16Array
// Int32Array, Uint32Array
// Float32Array, Float64Array
// BigInt64Array, BigUint64Array
````

## Обробка винятків

```javascript
// try-catch-finally
try { 
// Код, який може спричинити помилку 
const result = riskyOperation(); 
console.log(result);
} catch (error) { 
// Обробка помилки 
console.error("Відбулася помилка:", error.message); 

// Можна перевірити тип помилки 
if (error instanceof TypeError) { 
console.error("Помилка типу"); 
} else if (error instanceof ReferenceError) { 
console.error("Помилка посилання"); 
}
} finally { 
// Виконується завжди, незалежно від помилки 
console.log("Завершення операції");
}

// Створення власних помилок
class ValidationError extends Error { 
constructor(message) { 
super(message); 
this.name = "ValidationError"; 
}
}

function validateUser(user) { 
if (!user.name) { 
throw new ValidationError("Ім'я користувача обов'язково"); 
} 
if (user.age < 18) { 
throw new ValidationError("Користувач повинен бути старше 18 років"); 
}
}

try { 
validateUser({ name: "John", age: 16});
} catch (error) { 
if (error instanceof ValidationError) { 
console.error("Помилка валідації:", error.message); 
} else { 
console.error("Невідома помилка:", error); 
throw error; // Прокидаємо невідомі помилки далі 
}
}

// Асинхронна обробка помилок
async function fetchData() { 
try { 
const response = await fetch('https://api.example.com/data'); 
if (!response.ok) { 
throw new Error(`HTTP error! status: ${response.status}`); 
} 
const data = await response.json(); 
return data; 
} catch (error) { 
console.error("Помилка при отриманні даних:", error); 
throw error; // Прокидаємо помилку далі 
}
}
````

## Управління пам'яттю та складання сміття

JavaScript автоматично керує пам'яттю за допомогою збирача сміття, який звільняє пам'ять, зайняту об'єктами, коли вони стають недоступними.

```javascript
// Створення об'єкта
let obj = {data: "some data", size: 1024};

// Об'єкт доступний через змінну obj
console.log(obj.data);

// Перезаписуємо посилання
obj = null;

// Тепер вихідний об'єкт недоступний і може бути зібраний збирачем сміття
````

### Витоку пам'яті

Найпоширеніші причини витоків пам'яті:

1. **Небажані замикання**

```javascript
function createLeak() { 
const largeData = новий Array(1000000).fill('x'); 

return function() { 
// Використовує лише один елемент, але зберігає всю структуру 
console.log(largeData[0]); 
};
}

const leakyFunction = createLeak(); // largeData залишається в пам'яті
````

2. **Забуті обробники подій**

```javascript
function setupHandler() { 
const largeData = новий Array(1000000).fill('x'); 

document.getElementById('button').addEventListener('click', function() { 
// Обробник зберігає посилання на largeData 
console.log(largeData[0]); 
});
}

// Навіть якщо setupHandler завершиться, обробник і largeData залишаться у пам'яті
````

3. **Циклічні посилання**

```javascript
function createCycle() { 
const obj1 = {}; 
const obj2 = {}; 

obj1.ref = obj2; 
obj2.ref = obj1; 

return function() { 
// Функція, яка нічого не робить, але зберігає 
// Доступ до циклічно пов'язаних об'єктів 
};
}
````

### WeakMap і WeakSet для запобігання витоку

```javascript
// Використання WeakMap для кешування без витоку пам'яті
const cache = New WeakMap ();

function processUser(user) { 
if (cache.has(user)) { 
return cache.get(user); 
} 

const result = expensiveOperation(user); 
cache.set(user, result); 
return result;
}

let user = {name: "John"};
processUser(user);

// Якщо user більше не використовується, він та його дані в кеші
// можуть бути автоматично видалені збирачем сміття
user = null;
````

## Event Loop

Event Loop (цикл подій) - це механізм, який дозволяє JavaScript виконувати неблокуючі операції, незважаючи на те, що JavaScript є однопоточним.

### Компоненти Event Loop:

1. **Call Stack** - стек викликів функцій
2. **Heap** - область пам'яті для об'єктів
3. **Queue** - черга завдань: 
- **Macrotask Queue** - setTimeout, setInterval, I/O, UI рендеринг 
- **Microtask Queue** - Promise callbacks, queueMicrotask, MutationObserver

### Порядок виконання:

1. Виконати весь код у стеку дзвінків
2. Виконати всі мікрозадачі
3. Виконати одне макрозавдання
4. Повторити з кроку 1

```javascript
console.log('1 - Синхронний код');

setTimeout(() => { 
console.log('4 - Макрозадача (setTimeout)');
}, 0);

Promise.resolve() 
.then(() => { 
console.log('2 - Мікрозавдання (Promise)'); 
}) 
.then(() => { 
console.log('3 - Мікрозавдання (Promise, наступний then)'); 
});

console.log('5 - Синхронний код');

// Висновок:
// 1 - Синхронний код
// 5 - Синхронний код
// 2 - Мікрозавдання (Promise)
// 3 - Мікрозавдання (Promise, наступний then)
// 4 - Макрозадача (setTimeout)
````

### queueMicrotask

```javascript
console.log('1 - Початок');

queueMicrotask(() => { 
console.log('3 - Мікрозавдання');
});

console.log('2 - Кінець');

// Висновок:
// 1 - Початок
// 2 - Кінець
// 3 - Мікрозавдання
````

## Робота з DOM

DOM (Document Object Model) – це програмний інтерфейс для HTML та XML документів.

### Вибір елементів

```javascript
// За ID
const element = document.getElementById('myId');

// За класом
const elements = document.getElementsByClassName('myClass');

// За тегом
const divs = document.getElementsByTagName('div');

// CSS-селектори
const element2 = document.querySelector('.myClass'); // Перший знайдений
const allElements = document.querySelectorAll('div.myClass'); // NodeList всіх знайдених

// Відносна навігація
const parent = element.parentNode;
const children = element.childNodes; // Включно з текстовими вузлами
const elementChildren = element.children; // Тільки елементи
const firstChild = element.firstChild;
const lastChild = element.lastChild;
const nextSibling = element.nextSibling;
const previousSibling = element.previousSibling;
````

### Зміна DOM

```javascript
// Створення елементів
const div = document.createElement('div');
const textNode = document.createTextNode('Привіт, мир!');

// Додавання елементів
div.appendChild(textNode);
document.body.appendChild(div);

// Вставка елементів
const parent = document.getElementById('container');
const referenceNode = document.getElementById('reference');
parent.insertBefore(div, referenceNode);

// Сучасні методи вставки
parent.append(div, textNode); // Додає до кінця
parent.prepend(div); // Додає на початок
referenceNode.before(div); // Вставляє перед
referenceNode.after(div); // Вставляє після
referenceNode.replaceWith(div); // Замінює

// Видалення елементів
parent.removeChild(div);
div.remove(); // Сучасний метод

// Клонування
const clone = div.cloneNode(true); // true - глибоке клонування з нащадками
````

### Робота з атрибутами

```javascript
// Стандартні атрибути
element.id = 'newId';
element.className = 'class1 class2';
element.href = 'https://example.com';

// Довільні атрибути
element.setAttribute('data-id', '123');
const value = element.getAttribute('data-id');
element.removeAttribute('data-id');

// Перевірка наявності атрибуту
const hasAttr = element.hasAttribute('data-id');

// data-атрибути
element.dataset.id = '123'; // Встановлює data-id="123"
console.log(element.dataset.id); // Отримує значення data-id
````

### Робота з класами

```javascript
// Додавання/видалення класів
element.classList.add('newClass');
element.classList.remove('oldClass');
element.classList.toggle('active'); // Додає, якщо ні; видаляє, якщо є
element.classList.contains('active'); // Перевірка наявності класу
element.classList.replace('oldClass', 'newClass'); // Замінює один клас іншим
````

### Робота зі стилями

```javascript
// Вбудовані стилі
element.style.color = 'red';
element.style.backgroundColor = 'blue'; // camelCase для CSS-властивостей
element.style.cssText = 'color: red; background-color: blue;'; // Декілька стилів відразу

// Отримання обчислених стилів
const computedStyle = getComputedStyle(element);
console.log(computedStyle.color);
````

### Події

```javascript
// Додавання оброблювача
function handleClick(event) { 
console.log('Клік!', event.target); 
event.preventDefault(); // Запобігає дії за умовчанням 
event.stopPropagation(); // Зупиняє спливання
}

element.addEventListener('click', handleClick);

// Видалення оброблювача
element.removeEventListener('click', handleClick);

// Делегування подій
document.getElementById('list').addEventListener('click', function(event) { 
if (event.target.tagName === 'LI') { 
console.log('Клік по списку:', event.target.textContent); 
}
});

// Створення та відправка подій
const customEvent = новий CustomEvent('myEvent', { 
detail: { data: 'Довільні дані' }, 
bubbles: true, 
cancelable: true
});
element.dispatchEvent(customEvent);

// Обробка події користувача
element.addEventListener('myEvent', function(event) { 
console.log('Подія користувача:', event.detail.data);
});
````

## Web Workers

Web Workers дозволяють виконувати JavaScript код у фоновому потоці, не блокуючи основний потік.

### Основний потік (main.js)

```javascript
// Створення Worker
const worker = новий Worker('worker.js');

// Відправлення повідомлення Worker'у
worker.postMessage({ command: 'start', data: [1, 2, 3, 4, 5]});

// Отримання повідомлення від Worker'а
worker.onmessage = function(event) { 
console.log('Результат від Worker:', event.data);
};

// Обробка помилок
worker.onerror = function(error) { 
console.error('Помилка Worker:', error.message);
};

// Завершення Worker'а
function terminateWorker() { 
worker.terminate();
}
````

### Worker (worker.js)

```javascript
// Отримання повідомлення від основного потоку
self.onmessage = function(event) { 
const {command, data} = event.data; 

if (command === 'start') { 
// Виконання важких обчислень 
const result = heavyComputation(data); 

// Надсилання результату назад 
self.postMessage({result}); 
}
};

function heavyComputation(data) { 
// Приклад: обчислення суми квадратів 
return data.map(x => x * x).reduce((sum, x) => sum + x, 0);
}

// Обробка помилок
self.onerror = function(error) { 
console.error('Помилка Worker:', error.message);
};

// Самозавершення
function closeWorker() { 
self.close();
}
````

### Shared Workers

Дозволяють кільком вкладкам/фреймам використовувати той самий Worker.

```javascript
// Основний потік
const sharedWorker = новий SharedWorker('shared-worker.js');

// Підключення до порту
sharedWorker.port.start();

// Відправлення повідомлення
sharedWorker.port.postMessage({ command: 'getData' });

// Отримання повідомлення
sharedWorker.port.onmessage = function(event) { 
console.log('Дані від Shared Worker:', event.data);
};
````

```javascript
// shared-worker.js
const connections = [];

// Обробка підключень
self.onconnect = function(event) { 
const port=event.ports[0]; 
connections.push(port); 
port.start(); 

port.onmessage = function(event) { 
const {command} = event.data; 

if (command === 'getData') { 
port.postMessage({ result: 'Дані для всіх підключень'}); 
} else if (command === 'broadcast') { 
// Надсилання повідомлення всім підключенням 
connections.forEach(connection => { 
connection.postMessage({ broadcast: 'Повідомлення для всіх'}); 
}); 
} 
};
};
````

## Web Storage API

### LocalStorage

Зберігає дані без обмеження за часом, доступний лише у межах одного домену.

```javascript
//Збереження даних
localStorage.setItem('username', 'John');
localStorage.setItem('preferences', JSON.stringify({ theme: 'dark', fontSize: 14 }));

// Отримання даних
const username = localStorage.getItem('username');
const preferences = JSON.parse(localStorage.getItem('preferences'));

// Видалення даних
localStorage.removeItem('username');

// Очищення всіх даних
localStorage.clear();

// Кількість елементів
console.log(localStorage.length);

// Отримання ключа за індексом
const key = localStorage.key(0);
````

### SessionStorage

Схожий на localStorage, але дані зберігаються лише на час сесії (поки відкрито вкладку).

```javascript
// Працює аналогічно localStorage
sessionStorage.setItem('tempData', 'Тимчасові дані');
const tempData = sessionStorage.getItem('tempData');
sessionStorage.removeItem('tempData');
sessionStorage.clear();
````

### Події сховища

```javascript
// Відстеження змін до localStorage/sessionStorage в інших вкладках
window.addEventListener('storage', function(event) { 
console.log('Зміна в сховищі:', { 
key: event.key, 
oldValue: event.oldValue, 
newValue: event.newValue, 
url: event.url, 
storageArea: event.storageArea // localStorage або sessionStorage 
});
});
````

## WebSockets

WebSocket - це протокол, що забезпечує повнодуплексний зв'язок між клієнтом та сервером.

### Клієнтська частина

```javascript
// Створення з'єднання
const socket = новий WebSocket('wss://example.com/socket');

// Обробка подій
socket.onopen = function(event) { 
console.log('З'єднання встановлено'); 

// Надсилання повідомлення серверу 
socket.send(JSON.stringify({ type: 'greeting', content: 'Привіт, сервер!'}));
};

socket.onmessage = function(event) { 
// Отримання повідомлення від сервера 
const message = JSON.parse(event.data); 
console.log('Повідомлення від сервера:', message);
};

socket.onerror = function(error) { 
console.error('Помилка WebSocket:', error);
};

socket.onclose = function(event) { 
console.log('З'єднання закрито:', event.code, event.reason);
};

// Відправлення даних
function sendMessage(content) { 
if (socket.readyState === WebSocket.OPEN) { 
socket.send(JSON.stringify({ type: 'message', content })); 
} else { 
console.error('З'єднання не встановлено'); 
}
}

// Закриття з'єднання
function closeConnection() { 
socket.close(1000, 'Нормальне закриття');
}
````

### Серверна частина (Node.js з бібліотекою ws)

```javascript
const WebSocket = require('ws');

// Створення WebSocket-сервера
const wss = новий WebSocket.Server({ port: 8080 });

// Обробка підключень
wss.on('connection', function connection(ws) { 
console.log('Нове підключення'); 

// Відправлення привітання 
ws.send(JSON.stringify({ type: 'welcome', content: 'Ласкаво просимо!' }))); 

// Отримання повідомлень 
ws.on('message', function incoming(message) { 
const data = JSON.parse(message); 
console.log('Отримано повідомлення:', data); 

// Відлуння-відповідь 
ws.send(JSON.stringify({ type: 'echo', content: data.content })); 
}); 

// Обробка закриття з'єднання 
ws.on('close', function() { 
console.log('З'єднання закрите'); 
});
});

// Розсилка повідомлення всім клієнтам
function broadcast(message) { 
wss.clients.forEach(function each(client) { 
if (client.readyState === WebSocket.OPEN) { 
client.send(JSON.stringify(message)); 
} 
});
}
````

## Node.js

Node.js – це середовище виконання JavaScript на основі двигуна V8, що дозволяє запускати JavaScript на сервері.

### Модульна система

#### CommonJS (традиційна для Node.js)

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

// Або експорт окремих функцій
// exports.add = add;
// exports.subtract = subtract;
````

```javascript
// app.js
const math = require('./math');
// Або з деструктуризацією
// const {add, subtract} = require('./math');

console.log(math.add(5, 3)); // 8
console.log(math.subtract(5, 3)); // 2
````

#### ES Modules (підтримується в нових версіях Node.js)

```javascript
// math.mjs
export function add(a, b) { 
return a + b;
}

export function subtract(a, b) { 
return a - b;
}

// Або експорт за замовчуванням
// export default {add, subtract};
````

```javascript
// app.mjs
import { add, subtract } from './math.mjs';
// Або імпорт всього модуля
// import * as math from './math.mjs';

console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
````

### Основні модулі Node.js

#### Файлова система (fs)

```javascript
const fs = require('fs');

// Синхронне читання
try { 
const data = fs.readFileSync('file.txt', 'utf8'); 
console.log(data);
} catch (err) { 
console.error('Помилка читання файлу:', err);
}

// Асинхронне читання з коллбеком
fs.readFile('file.txt', 'utf8', (err, data) => { 
if (err) { 
console.error('Помилка читання файлу:', err); 
return; 
} 
console.log(data);
});

// Асинхронне читання з промісами
const fsPromises = require('fs').promises;

async function readFileAsync() { 
try { 
const data = await fsPromises.readFile('file.txt', 'utf8'); 
console.log(data); 
} catch (err) { 
console.error('Помилка читання файлу:', err); 
}
}

// Запис у файл
fs.writeFile('output.txt', 'Привіт, мир!', 'utf8', (err) => { 
if (err) { 
console.error('Помилка запису файлу:', err); 
return; 
} 
console.log('Файл успішно записаний');
});

// Робота з директоріями
fs.mkdir('new-dir', {recursive: true}, (err) => { 
if (err) { 
console.error('Помилка створення директорії:', err); 
return; 
} 
console.log('Директорія створена');
});

// Читання вмісту директорії
fs.readdir('.', (err, files) => { 
if (err) { 
console.error('Помилка читання директорії:', err); 
return; 
} 
console.log('Файли:', files);
});
````

#### HTTP-сервер

```javascript
const http = require ('http');

// Створення простого HTTP-сервера
const server = http.createServer((req, res) => { 
// Отримання інформації про запит 
const {method, url, headers} = req; 
console.log(`${method} ${url}`); 

// Обробка різних URL 
if (url === '/') { 
res.writeHead(200, {'Content-Type': 'text/plain'}); 
res.end('Головна сторінка'); 
} else if (url === '/api/data') { 
res.writeHead(200, { 'Content-Type': 'application/json'}); 
res.end(JSON.stringify({ message: 'Дані API'})); 
} else { 
res.writeHead(404, {'Content-Type': 'text/plain'}); 
res.end('Сторінку не знайдено'); 
}
});

// Запуск сервера
const PORT = process.env.PORT | 3000;
server.listen(PORT, () => { 
console.log(`Сервер запущений на порту ${PORT}`);
});
````

#### Робота з шляхами (path)

```javascript
const path = require('path');

// Об'єднання шляхів
const fullPath = path.join(__dirname, 'subfolder', 'file.txt');
console.log(fullPath);

//Нормалізація шляху
console.log(path.normalize('/folder//subfolder/file.txt'));

// Отримання компонентів шляху
console.log(path.basename('/folder/file.txt')); // file.txt
console.log(path.dirname('/folder/file.txt')); // / folder
console.log(path.extname('/folder/file.txt')); // .txt

// Розбір шляху
const pathInfo = path.parse('/folder/file.txt');
console.log(pathInfo);
// { root: '/', dir: '/folder', base: 'file.txt', ext: '.txt', name: 'file' }
````

#### Події (EventEmitter)

```javascript
const EventEmitter = require('events');

// Створення емітера подій
class MyEmitter extends EventEmitter {}
const myEmitter = New MyEmitter();

// Реєстрація оброблювача події
myEmitter.on('event', (arg1, arg2) => { 
console.log('Подія відбулася:', arg1, arg2);
});

// Реєстрація оброблювача, який спрацює лише один раз
myEmitter.once('oneTimeEvent', () => { 
console.log('Ця подія спрацює лише один раз');
});

// Генерація події
myEmitter.emit('event', 'arg1', 'arg2');
myEmitter.emit('oneTimeEvent');
myEmitter.emit('oneTimeEvent'); // Обробник не спрацює

// Видалення оброблювача
function handler() { 
console.log('Обробник події');
}
myEmitter.on('removeMe', handler);
myEmitter.removeListener('removeMe', handler);
// Або myEmitter.off('removeMe', handler); у нових версіях

// Отримання списку слухачів
console.log(myEmitter.listeners('event'));

// Встановлення максимальної кількості слухачів
myEmitter.setMaxListeners(20); // За замовчуванням 10
````

## JSDoc та типізація

JSDoc - це система документування JavaScript за допомогою коментарів, яка також може використовуватися для статичної типізації.

```javascript
/** 
* Представляє користувача системи. 
* @typedef {Object} User 
* @property {string} id - Унікальний ідентифікатор користувача 
* @property {string} name - Ім'я користувача 
* @property {number} age - Вік користувача 
* @property {string[]} roles - Список ролей користувача 
*/

/** 
* Створює нового користувача. 
* @param {string} name - Ім'я користувача 
* @param {number} age - Вік користувача 
* @param {string[]} [roles=['user']] - Список ролей користувача (за замовчуванням ['user']) 
* @returns {User} Створений користувач 
* @throws {Error} Якщо вік менше 18 
*/
function createUser(name, age, roles = ['user']) { 
if (age < 18) { 
throw new Error('Користувач повинен бути повнолітнім'); 
} 

return { 
id: generateId(), 
name, 
age, 
roles 
};
}

/** 
* Генерує унікальний ідентифікатор. 
* @returns {string} Унікальний ідентифікатор 
* @private 
*/
function generateId() { 
return Math.random().toString(36).substr(2, 9);
}

/** 
* Клас для роботи з колекцією користувачів. 
*/
class UserService { 
/** 
* @type {User[]} 
* @private 
*/ 
#users = []; 

/** 
* Додає користувача до колекції. 
* @param {User} user - Користувач для додавання 
* @returns {void} 
*/ 
addUser(user) { 
this.#users.push(user); 
} 

/** 
* Знаходить користувача за ідентифікатором. 
* @param {string} id - Ідентифікатор користувача 
* @returns {User|null} Знайдений користувач або null 
*/ 
findById(id) { 
return this.#users.find(user => user.id === id) || null; 
} 

/** 
* Знаходить користувачів по ролі. 
* @param {string} role - Роль для пошуку 
* @returns {User[]} Список користувачів із зазначеною роллю 
*/ 
findByRole(role) { 
return this.#users.filter(user => user.roles.includes(role)); 
}
}

/** 
* Функція вищого порядку для фільтрації користувачів. 
* @callback UserFilterCallback 
* @param {User} user - Користувач для перевірки 
* @returns {boolean} True, якщо користувач відповідає критерію 
*/

/** 
* Фільтрує користувачів за заданим критерієм. 
* @param {User[]} users - Масив користувачів 
* @param {UserFilterCallback} filterFn - Функція фільтрації 
* @returns {User[]} Відфільтрований масив користувачів 
*/
function filterUsers(users, filterFn) { 
return users.filter(filterFn);
}

/** 
* Перерахування можливих статусів завдання. 
* @enum {string} 
*/
const TaskStatus = { 
/** Завдання створено */ 
CREATED: 'created', 
/** Завдання у процесі виконання */ 
IN_PROGRESS: 'in_progress', 
/** Завдання завершено */ 
COMPLETED: 'completed', 
/** Завдання скасовано */ 
CANCELLED: 'cancelled'
};

/** 
* @template T 
* @param {T} value - Значення для обробки 
* @returns {T} Те саме значення 
*/
function identity(value) { 
return value;
}

// Використання з JSDoc-типами
const user = createUser('John', 25, ['admin', 'moderator']);
const service = новий UserService();
service.addUser(user);

const admins = service.findByRole('admin');
const youngAdmins = filterUsers(admins, user => user.age < 30);

const numberIdentity = identity(42); // T = number
const stringIdentity = identity('hello'); // T = string
````

### Інтеграція з TypeScript

Сучасні IDE (VSCode, WebStorm) можуть використовувати JSDoc для перевірки типів навіть без TypeScript:

```javascript
// @ts-check

/** 
* @typedef {Object} Point 
* @property {number} x - Координата X 
* @property {number} y - Координата Y 
*/

/** 
* Обчислює відстань між двома точками. 
* @param {Point} p1 - Перша точка 
* @param {Point} p2 - Друга точка 
* @returns {number} Відстань між точками 
*/
function distance(p1, p2) { 
return Math.sqrt(Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2));
}

// Помилка буде підсвічена в IDE
distance({ x: 0, y: 0 }, { x: '1', y: 0 }); // Помилка: Type 'string' is not assignable to type 'number'
````

## Тестування: Jest та Mocha

### Jest

Jest - повноцінний фреймворк для тестування JavaScript із вбудованим засобом створення моків, асертами та покриттям коду.

#### Установка

``` bash
npm install --save-dev jest
````

#### Базові тести

```javascript
// math.js
function sum(a, b) { 
return a + b;
}

function multiply(a, b) { 
return a*b;
}

module.exports = { sum, multiply };
````

```javascript
// math.test.js
const { sum, multiply } = require('./math');

test('додавання 1 + 2 дорівнює 3', () => { 
expect(sum(1, 2)).toBe(3);
});

test('множення 2 * 3 дорівнює 6', () => { 
expect(multiply(2, 3)).toBe(6);
});

// Угруповання тестів
describe('математичні функції', () => { 
test('складання негативних чисел', () => { 
expect(sum(-1, -2)).toBe(-3); 
}); 

test('множення негативних чисел', () => { 
expect(multiply(-2, -3)).toBe(6); 
});
});
````

#### Асинхронне тестування

```javascript
// api.js
async function fetchUser(id) { 
const response = await fetch(`https://api.example.com/users/${id}`); 
if (!response.ok) { 
throw new Error('Помилка отримання користувача'); 
} 
return response.json();
}

module.exports = { fetchUser };
````

```javascript
// api.test.js
const { fetchUser } = require('./api');

// З async/await
test('отримання користувача', async() => { 
const user = await fetchUser(1); 
expect(user).toHaveProperty('name'); 
expect(user.id).toBe(1);
});

// З Promise
test('отримання користувача (Promise)', () => { 
return fetchUser(1).then(user => { 
expect(user).toHaveProperty('name'); 
});
});

// Перевірка помилок
test('помилка при отриманні неіснуючого користувача', async() => { 
expect.assertions(1); // Переконаємося, що асинхронний код виконається 
try { 
await fetchUser(999); 
} catch (e) { 
expect(e.message).toMatch(/помилка/i); 
}
});
````

#### Моки та шпигуни

```javascript
//user-service.js
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
````

```javascript
//user-service.test.js
const UserService = require('./user-service');
const api = require('./api');

// Мок всього модуля
jest.mock('./api');

test('getUser повертає користувача з повним ім'ям', async() => { 
// Налаштування мока 
api.fetchUser.mockResolvedValue({ 
id: 1, 
firstName: 'John', 
lastName: 'Doe' 
}); 

const userService = новий UserService(); 
const user = await userService.getUser(1); 

// Перевірки 
expect(api.fetchUser).toHaveBeenCalledWith(1); 
expect(user.fullName).toBe('John Doe');
});

// Шпигун для окремого методу
test('шпигун для методу', () => { 
const calculator = { 
add: (a, b) => a + b 
}; 

// Створюємо шпигуна 
const spy = jest.spyOn(calculator, 'add'); 

calculator.add(2, 3); 

expect(spy).toHaveBeenCalledWith(2, 3); 
expect(spy).toHaveBeenCalledTimes(1); 

// Відновлюємо оригінальну реалізацію 
spy.mockRestore();
});
````

#### Знімки (Snapshots)

```javascript
// component.js
function generateUserProfile(user) { 
return { 
header: `Профіль: ${user.name}`, 
content: `Вік: ${user.age}, Email: ${user.email}`, 
footer: `Останній вхід: ${user.lastLogin}` 
};
}

module.exports = {generateUserProfile};
````

```javascript
// component.test.js
const { generateUserProfile } = require('./component');

test('генерує правильний профіль користувача', () => { 
const user = { 
name: 'John Doe', 
age: 30, 
email: 'john@example.com', 
lastLogin: '2023-01-15' 
}; 

const profile = generateUserProfile(user); 

// Створює або порівнює зі знімком 
expect(profile).toMatchSnapshot();
});
````

### Mocha

Mocha - гнучкий фреймворк для тестування JavaScript, що зазвичай використовується з бібліотекою тверджень, наприклад, Chai.

#### Установка

``` bash
npm install --save-dev mocha chai
````

#### Базові тести

```javascript
// math.js
function sum(a, b) { 
return a + b;
}

function multiply(a, b) { 
return a*b;
}

module.exports = { sum, multiply };
````

```javascript
// math.test.js
const {expect} = require('chai');
const { sum, multiply } = require('./math');

describe('Математичні функції', () => { 
it('додавання 1 + 2 дорівнює 3', () => { 
expect(sum(1, 2)).to.equal(3); 
}); 

it('множення 2 * 3 дорівнює 6', () => { 
expect(multiply(2, 3)).to.equal(6); 
}); 

// Вкладені групи 
describe('з негативними числами', () => { 
it('складання негативних чисел', () => { 
expect(sum(-1, -2)).to.equal(-3); 
}); 

it('множення негативних чисел', () => { 
expect(multiply(-2, -3)).to.equal(6); 
}); 
});
});
````

#### Асинхронне тестування

```javascript
// api.js
async function fetchUser(id) { 
const response = await fetch(`https://api.example.com/users/${id}`); 
if (!response.ok) { 
throw new Error('Помилка отримання користувача'); 
} 
return response.json();
}

module.exports = { fetchUser };
````

```javascript
// api.test.js
const {expect} = require('chai');
const { fetchUser } = require('./api');

describe('API', () => { 
// З async/await 
it('отримання користувача', async() => { 
const user = await fetchUser(1); 
expect(user).to.have.property('name'); 
expect(user.id).to.equal(1); 
}); 

// З Promise 
it('отримання користувача (Promise)', () => { 
return fetchUser(1).then(user => { 
expect(user).to.have.property('name'); 
}); 
}); 

// З done 
it('отримання користувача (callback)', (done) => { 
fetchUser(1) 
.then(user => { 
expect(user).to.have.property('name'); 
done(); 
}) 
. catch (done); 
}); 

// Перевірка помилок 
it('помилка при отриманні неіснуючого користувача', async() => { 
try { 
await fetchUser(999); 
// Якщо дійшли сюди, тест повинен провалитися 
expect.fail('Мала виникнути помилка'); 
} catch (e) { 
expect(e.message).to.match(/помилка/i); 
} 
});
});
````

#### Моки з Sinon

``` bash
npm install --save-dev sinon
````

```javascript
//user-service.js
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
````

```javascript
//user-service.test.js
const {expect} = require('chai');
const sinon = require('sinon');
const UserService = require('./user-service');
const api = require('./api');

describe('UserService', () => { 
// Відновлюємо оригінальні функції після кожного тесту 
afterEach(() => { 
sinon.restore(); 
}); 

it('getUser повертає користувача з повним ім'ям', async() => { 
// Створюємо заглушку 
const stub = sinon.stub(api, 'fetchUser').resolves({ 
id: 1, 
firstName: 'John', 
lastName: 'Doe' 
}); 

const userService = новий UserService(); 
const user = await userService.getUser(1); 

// Перевірки 
expect(stub.calledWith(1)).to.be.true; 
expect(user.fullName).to.equal('John Doe'); 
}); 

// Шпигун 
it('шпигун для методу', () => { 
const calculator = { 
add: (a, b) => a + b 
}; 

// Створюємо шпигуна 
const spy = sinon.spy(calculator, 'add'); 

calculator.add(2, 3); 

expect(spy.calledWith(2, 3)).to.be.true; 
expect(spy.callCount).to.equal(1); 
});
});
````

## Сучасні можливості JavaScript

### Оператор опціональної послідовності (?.)

```javascript
const user = { 
profile: { 
address: { 
city: 'Москва' 
} 
}
};

// Старий спосіб
const city1 = user && user.profile && user.profile.address && user.profile.address.city;

// Новий спосіб
const city2 = user?.profile?.address?.city;

// Працює з методами
const result = user.getDetails?.(); // Не викликає помилку, якщо методу немає

// Працює з масивами
const firstItem = array?. [0];
````

### Оператор нульового злиття (??)

```javascript
// Старий спосіб (проблема: 0 і '' вважаються хибними)
const value1 = someValue || 'за замовчуванням';

// Новий спосіб (null і undefined замінюються на значення за замовчуванням)
const value2 = someValue ?? 'за замовчуванням';

// Приклади
const count = 0;
console.log(count || 10); // 10 (0 вважається хибним)
console.log(count ?? 10); // 0 (0 не є null або undefined)

const name = '';
console.log(name || 'Гість'); // "Гість" (порожній рядок вважається хибним)
console.log(name ?? 'Гість'); // "" (порожній рядок не є null або undefined)
````

### Логічне присвоєння

```javascript
// Логічне І (&&=)
let x = 1;
x &&= 2; // Еквівалентно: x = x && 2
console.log(x); // 2

let y = 0;
y &&= 2; // Еквівалентно: y = y && 2
console.log(y); // 0 (т.к. 0 && 2 = 0)

// Логічне АБО (||=)
let a = 0;
a ||= 1; // Еквівалентно: a = a | 1
console.log(a); // 1

let b = 2;
b ||= 1; // Еквівалентно: b = b | 1
console.log(b); // 2 (т.к. 2 | | 1 = 2)

// Нульове злиття (?? =)
let c = null;
c ?? = 'значення'; // Еквівалентно: c = c ?? 'значення'
console.log(c); // "значення"

let d = 0;
d ?? = 'значення'; // Еквівалентно: d = d?? 'значення'
console.log(d); // 0 (т.к. 0 ?? 'значення' = 0)
````

### Приватні поля та методи класів

```javascript
class BankAccount { 
// Приватне поле (з #) 
# Balance = 0; 

// Приватний метод 
#validateAmount(amount) { 
if (amount <= 0) { 
throw new Error('Сума повинна бути позитивною'); 
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

// Статичний приватний метод 
static #generateAccountNumber() { 
return Math.floor(Math.random() * 1000000); 
} 

static createAccount(initialBalance) { 
const account = new BankAccount(initialBalance); 
account.accountNumber = this.#generateAccountNumber(); 
return account; 
}
}
````

### Top-level await

У сучасних модулях ES можна використовувати await на верхньому рівні, без обертання в async-функцію.

```javascript
// module.js
const response = await fetch('https://api.example.com/data');
const data = await response.json();

export {data};
````

### Метод at() для індексації масивів

```javascript
const array = [10, 20, 30, 40, 50];

// Доступ до останнього елементу
const last = array [array.length – 1]; // Старий спосіб
const last2 = array.at(-1); // Новий спосіб: 50

// Доступ до передостаннього елементу
const secondLast = array.at(-2); // 40

// Працює і з позитивними індексами
const first = array.at(0); // 10
````

### Метод replaceAll()

```javascript
const string = 'Привіт, мир! Привіт JavaScript!';

// Старий спосіб (з регулярним виразом)
const newString1 = string.replace(/Привіт/g, 'Привіт');

// Новий спосіб
const newString2 = string.replaceAll('Привіт', 'Привіт');
````

### Promise.any та AggregateError

```javascript
// Promise.any повертає перший успішно виконаний проміс
try { 
const result = await Promise.any([ 
fetch('https://api1.example.com/data').then(r => r.json()), 
fetch('https://api2.example.com/data').then(r => r.json()), 
fetch('https://api3.example.com/data').then(r => r.json()) 
]); 

console.log('Перший успішний результат:', result);
} catch (error) { 
// AggregateError містить усі помилки, якщо всі проміси відхилені 
console.error('Всі запити завершилися з помилкою:', error.errors);
}
````

### Numeric Separators

```javascript
// Великі числа стали більш читаними
const billion = 1_000_000_000;
const binary = 0b1010_0001_1000_0101;
const hex = 0xFF_EC_DE_5E;
````

### Фіналізатори для класів

```javascript
class ResourceHandler { 
constructor() { 
this.resource = acquireResource(); 
} 

// Метод, що викликається перед збиранням сміття 
// Зверніть увагу: немає гарантії, що його буде викликано! 
[Symbol.for('nodejs.util.inspect.custom')] = () => { 
return 'ResourceHandler'; 
};
}
````

### Глобальний this

```javascript
// Працює у будь-якому оточенні (браузер, Node.js, WebWorker)
const globalObject = globalThis;

// У браузері це window
// У Node.js це global
// У WebWorker це self
````

### Intl API для інтернаціоналізації

```javascript
// Форматування дат
const date = New Date();
const formatter = new Intl.DateTimeFormat('ru', { 
weekday: 'long', 
year: 'numeric', 
month: 'long', 
day: 'numeric'
});
console.log(formatter.format(date)); // "понеділок, 15 січня 2023 р."

// Форматування чисел
const number = 123456.789;
console.log(new Intl.NumberFormat('ru').format(number)); // "123 456,789"
console.log(new Intl.NumberFormat('ru', { style: 'currency', currency: 'RUB'}).format(number)); // "123 456,79 ₽"

// Порівняння рядків з урахуванням локалі
const collator = new Intl.Collator('ru');
['я', 'б', 'в', 'а']. sort(collator.compare); // ["а", "б", "в", "я"]

// Відносний час
const rtf = new Intl.RelativeTimeFormat('ru');
console.log(rtf.format(-1, 'day')); // "1 день тому"
console.log(rtf.format(2, 'month')); // "Через 2 місяці"

// Списки
const listFormat = new Intl.ListFormat('ru', { style: 'long', type: 'conjunction'});
console.log(listFormat.format(['яблука', 'груші', 'банани']))); // "яблука, груші та банани"
````
---
