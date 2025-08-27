# Nihai JavaScript Cheatsheet'i

**Strict Mode Hakkında:**

Bu cheatsheet'teki örneklerin çoğu modern JavaScript (`"use strict";`) standartlarına göre yazılmıştır. Strict mode JavaScript'in daha güvenli ve hata ayıklaması kolay bir versiyonunu sağlar.

```javascript
"use strict"; // Dosyanın başında veya fonksiyon içinde

// Strict mode avantajları:
// - Sessiz hataları görünür yapar
// - Değişken tanımlamadan kullanmayı engeller
// - Güvenli JavaScript kodu yazmayı teşvik eder
// - Bazı performans optimizasyonlarını mümkün kılar

// Örnek:
function strictExample() {
  "use strict";
  // myVar = 5; // ReferenceError: myVar is not defined
  let myVar = 5; // Doğru kullanım
}
```

## İçindekiler Tablosu

1. [Temel Kavramlar](#temel-kavramlar)
   - [Değişkenler](#değişkenler)
   - [Veri Türleri](#veri-türleri)
   - [Operatörler](#operatörler)
   - [Tür Zorlaması ve Dönüşümü](#tür-zorlaması-ve-dönüşümü)
   - [Kapsam ve Hoisting](#kapsam-ve-hoisting)
   - [Hata Yönetimi](#hata-yönetimi)
2. [Kontrol Akışı](#kontrol-akışı)
   - [Koşullu İfadeler](#koşullu-ifadeler)
   - [Döngüler](#döngüler)
3. [Fonksiyonlar](#fonksiyonlar)
   - [Callback Fonksiyonları](#callback-fonksiyonları)
   - [Closure](#closure)
   - [This Bağlamı](#this-bağlamı)
   - [Fonksiyon Bildirimleri](#fonksiyon-bildirimleri)
   - [Arrow Fonksiyonları](#arrow-fonksiyonları)
   - [IIFE](#iife)
4. [Nesneler ve Diziler](#nesneler-ve-diziler)
   - [Nesne Manipülasyonu](#nesne-manipülasyonu)
   - [Dizi Metodları](#dizi-metodları)
   - [Destructuring](#destructuring)
   - [Spread ve Rest Operatörleri](#spread-ve-rest-operatörleri)
5. [Asenkron JavaScript](#asenkron-javascript)
   - [Promises](#promises)
   - [Async/Await](#asyncawait)
6. [Nesne Yönelimli Programlama](#nesne-yönelimli-programlama)
   - [Class Sözdizimi](#class-sözdizimi)
   - [Prototype ve Prototypal Inheritance](#prototype-ve-prototypal-inheritance)
   - [Inheritance](#inheritance)
7. [ES6+ Özellikleri](#es6-özellikleri)
   - [Template Literals](#template-literals)
   - [Modüller](#modüller)
8. [DOM Manipülasyonu](#dom-manipülasyonu)
   - [Element Seçme](#element-seçme)
   - [Element Manipülasyonu](#element-manipülasyonu)
   - [Olay Yönetimi](#olay-yönetimi)
9. [Sık Kullanılan Dahili Nesneler](#sık-kullanılan-dahili-nesneler)
   - [Regular Expressions (RegExp)](#regular-expressions-regexp)
   - [JSON Metodları](#json-metodları)
   - [Timer Fonksiyonları](#timer-fonksiyonları)
   - [String Metodları](#string-metodları)
   - [Number Metodları](#number-metodları)
   - [Math Nesnesi](#math-nesnesi)
   - [Date Nesnesi](#date-nesnesi)
   - [Console Metodları](#console-metodları)
   - [Performance API](#performance-api)
   - [Local Storage ve Session Storage](#local-storage-ve-session-storage)
   - [Fetch API](#fetch-api)
10. [İleri Seviye JavaScript](#i̇leri-seviye-javascript)
   - [Generator Fonksiyonları](#generator-fonksiyonları)
   - [Set ve Map Koleksiyonları](#set-ve-map-koleksiyonları)
   - [WeakMap ve WeakSet](#weakmap-ve-weakset)
   - [Iterators ve Iterables](#iterators-ve-iterables)
   - [Proxy ve Reflect](#proxy-ve-reflect)
   - [Polyfill ve Transpiling](#polyfill-ve-transpiling)

---

## Temel Kavramlar

### Değişkenler

**var, let, const** arasındaki farklar ve kapsam özellikleri:

```javascript
// var - Fonksiyon kapsamlı, hoisting var
var name = "JavaScript";

// let - Blok kapsamlı, hoisting yok
let age = 25;

// const - Blok kapsamlı, değişmez, hoisting yok
const PI = 3.14159;
```

**Kapsam Örnekleri:**

```javascript
function scopeExample() {
  var varVariable = "Function scoped";
  let letVariable = "Block scoped";
  const constVariable = "Block scoped";
  
  if (true) {
    var varVariable2 = "Still function scoped";
    let letVariable2 = "Block scoped only";
    const constVariable2 = "Block scoped only";
  }
  
  console.log(varVariable2); // Erişilebilir
  // console.log(letVariable2); // ReferenceError
}
```

### Veri Türleri

**Primitif Türler:**

```javascript
// String
let text = "Hello World";

// Number
let number = 42;
let decimal = 3.14;

// Boolean
let isTrue = true;
let isFalse = false;

// Undefined
let undefinedVariable;

// Null
let nullValue = null;

// Symbol (ES6)
let symbol = Symbol('unique');
let symbol2 = Symbol('unique'); // Her symbol benzersizdir
console.log(symbol === symbol2); // false

// Symbol kullanım alanları
let obj = {
  name: "John",
  [Symbol('id')]: 123,
  [Symbol.for('userId')]: 456 // Global symbol
};

// Symbol property'ler Object.keys()'te görünmez
console.log(Object.keys(obj)); // ["name"]

// Symbol property'lere erişim
console.log(obj[Symbol.for('userId')]); // 456

// Well-known symbols
let myArray = [1, 2, 3];
console.log(myArray[Symbol.iterator]); // Array iterator function

// Custom iterator
let range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    return {
      current: this.from,
      last: this.to,
      next() {
        if (this.current <= this.last) {
          return {done: false, value: this.current++};
        } else {
          return {done: true};
        }
      }
    };
  }
};

for (let num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}

// BigInt (ES2020)
let bigNumber = 123n;
let anotherBig = BigInt(456);
let maxSafe = BigInt(Number.MAX_SAFE_INTEGER);

// BigInt detaylı kullanım
console.log(9007199254740991n + 1n); // 9007199254740992n
console.log(BigInt(Number.MAX_SAFE_INTEGER) + 1n); // Safe integer limitini aş

// BigInt işlemleri
let big1 = 123456789012345678901234567890n;
let big2 = 987654321098765432109876543210n;

console.log(big1 + big2); // Büyük sayı toplama
console.log(big1 * big2); // Büyük sayı çarpma
console.log(big2 / big1); // 8n (tam bölme)
console.log(big2 % big1); // BigInt modül

// BigInt ve regular number karşılaştırması
console.log(10n == 10);  // true (tür zorlaması)
console.log(10n === 10); // false (farklı türler)
console.log(10n > 5);    // true

// BigInt dönüştürme
console.log(Number(123n)); // 123 (küçük BigInt'ler için)
console.log(String(123n)); // "123"
console.log(Boolean(0n));  // false
console.log(Boolean(1n));  // true
```

**Non-Primitif Türler:**

```javascript
// Object
let person = {
  name: "John",
  age: 30
};

// Array
let numbers = [1, 2, 3, 4, 5];

// Function
function greet() {
  return "Hello!";
}
```

### Operatörler

**Aritmetik Operatörler:**

```javascript
let a = 10, b = 3;

console.log(a + b); // 13 (Toplama)
console.log(a - b); // 7 (Çıkarma)
console.log(a * b); // 30 (Çarpma)
console.log(a / b); // 3.33 (Bölme)
console.log(a % b); // 1 (Modül)
console.log(a ** b); // 1000 (Üs alma)
console.log(++a); // 11 (Pre-increment)
console.log(b--); // 3, sonra 2 (Post-decrement)
```

**Atama Operatörleri:**

```javascript
let x = 10;

x += 5;  // x = x + 5  (15)
x -= 3;  // x = x - 3  (12)
x *= 2;  // x = x * 2  (24)
x /= 4;  // x = x / 4  (6)
x %= 4;  // x = x % 4  (2)
x **= 3; // x = x ** 3 (8)

console.log(x); // 8

// String ile
let str = "Hello";
str += " World"; // "Hello World"

// Array ile
let arr = [1, 2];
arr[0] += 10; // [11, 2]
```

**Karşılaştırma Operatörleri:**

```javascript
let x = 5, y = "5";

console.log(x == y);  // true (Tür dönüşümü ile)
console.log(x === y); // false (Sıkı eşitlik)
console.log(x != y);  // false
console.log(x !== y); // true
console.log(x > 3);   // true
console.log(x <= 5);  // true
```

**Mantıksal Operatörler:**

```javascript
let t = true, f = false;

console.log(t && f); // false (VE)
console.log(t || f); // true (VEYA)
console.log(!t);     // false (DEĞİL)
```

**Ternary Operatör:**

```javascript
let age = 20;
let message = age >= 18 ? "Yetişkin" : "Çocuk";
console.log(message); // "Yetişkin"
```

**Bitwise Operatörler:**

```javascript
let a = 5;  // Binary: 101
let b = 3;  // Binary: 011

console.log(a & b);   // 1 (AND - 001)
console.log(a | b);   // 7 (OR - 111)
console.log(a ^ b);   // 6 (XOR - 110)
console.log(~a);      // -6 (NOT)
console.log(a << 1);  // 10 (Left shift - 1010)
console.log(a >> 1);  // 2 (Right shift - 10)
console.log(a >>> 1); // 2 (Unsigned right shift)
```

**Typeof ve Instanceof:**

```javascript
// typeof operatörü
console.log(typeof 42);          // "number"
console.log(typeof "hello");     // "string"
console.log(typeof true);        // "boolean"
console.log(typeof undefined);   // "undefined"
console.log(typeof null);        // "object" (JavaScript'in bilinen bir hatası)
console.log(typeof {});          // "object"
console.log(typeof []);          // "object"
console.log(typeof function(){}); // "function"

// instanceof operatörü
let arr = [1, 2, 3];
let date = new Date();
let regex = /abc/;

console.log(arr instanceof Array);    // true
console.log(date instanceof Date);    // true
console.log(regex instanceof RegExp); // true
console.log(arr instanceof Object);   // true (Array extends Object)
```

**Modern Operatörler (ES2020):**

```javascript
// Nullish Coalescing (??) - null veya undefined kontrolü
let name = null;
let defaultName = name ?? "Varsayılan İsim";
console.log(defaultName); // "Varsayılan İsim"

let count = 0;
let defaultCount = count ?? 10;
console.log(defaultCount); // 0 (çünkü 0 falsy ama null/undefined değil)

// Optional Chaining (?.) - güvenli property erişimi
let user = {
  name: "John",
  address: {
    street: "123 Main St",
    city: "New York"
  }
};

console.log(user?.name);                    // "John"
console.log(user?.address?.street);         // "123 Main St"
console.log(user?.address?.zipCode);        // undefined
console.log(user?.phoneNumber?.home);       // undefined (hata vermez)

// Method çağrılarında optional chaining
console.log(user?.getName?.());             // undefined (method yok)
console.log(user?.address?.getFullAddress?.()); // undefined

// Array'lerde optional chaining
let users = [{name: "John"}, {name: "Jane"}];
console.log(users?.[0]?.name);              // "John"
console.log(users?.[5]?.name);              // undefined
```

### Tür Zorlaması ve Dönüşümü

**Otomatik Tür Zorlaması:**

```javascript
console.log("5" + 3);    // "53" (string concatenation)
console.log("5" - 3);    // 2 (numeric subtraction)
console.log("5" * "2");  // 10 (numeric multiplication)
console.log(true + 1);   // 2 (boolean to number)
```

**Manuel Tür Dönüşümü:**

```javascript
// String'e dönüştürme
String(123);        // "123"
(123).toString();   // "123"

// Number'a dönüştürme
Number("123");      // 123
parseInt("123px");  // 123
parseFloat("12.34"); // 12.34

// Boolean'a dönüştürme
Boolean(0);         // false
Boolean(1);         // true
!!("hello");        // true
```

### Kapsam ve Hoisting

**Hoisting Örneği:**

```javascript
console.log(hoistedVar); // undefined (not error)
var hoistedVar = "I'm hoisted";

// console.log(notHoisted); // ReferenceError
let notHoisted = "I'm not hoisted";

// Fonksiyon hoisting
sayHello(); // Çalışır

function sayHello() {
  console.log("Hello!");
}

// Arrow fonksiyonlar hoist edilmez
// sayGoodbye(); // TypeError
var sayGoodbye = () => console.log("Goodbye!");
```

### Hata Yönetimi

**Try/Catch/Finally:**

```javascript
// Temel try/catch
try {
  let result = riskyOperation();
  console.log(result);
} catch (error) {
  console.error('Hata yakalandı:', error.message);
} finally {
  console.log('Bu blok her zaman çalışır');
}

// Belirli hata türlerini yakalama
try {
  JSON.parse('invalid json');
} catch (error) {
  if (error instanceof SyntaxError) {
    console.log('JSON syntax hatası');
  } else if (error instanceof ReferenceError) {
    console.log('Reference hatası');
  } else {
    console.log('Bilinmeyen hata:', error);
  }
}

// Hata fırlatma
function divide(a, b) {
  if (b === 0) {
    throw new Error('Sıfıra bölme hatası');
  }
  return a / b;
}

try {
  let result = divide(10, 0);
} catch (error) {
  console.log(error.message); // "Sıfıra bölme hatası"
}

// Custom Error sınıfları
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = 'CustomError';
  }
}

try {
  throw new CustomError('Özel hata mesajı');
} catch (error) {
  console.log(error.name);    // "CustomError"
  console.log(error.message); // "Özel hata mesajı"
}
```

---

## Kontrol Akışı

### Koşullu İfadeler

**if/else Yapısı:**

```javascript
let score = 85;

if (score >= 90) {
  console.log("A Grade");
} else if (score >= 80) {
  console.log("B Grade");
} else if (score >= 70) {
  console.log("C Grade");
} else {
  console.log("F Grade");
}
```

**switch Yapısı:**

```javascript
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Pazartesi");
    break;
  case "Tuesday":
    console.log("Salı");
    break;
  case "Wednesday":
    console.log("Çarşamba");
    break;
  default:
    console.log("Bilinmeyen gün");
}
```

### Döngüler

**for Döngüsü:**

```javascript
// Klasik for döngüsü
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// for...in (nesne özellikleri için)
let person = {name: "John", age: 30, city: "New York"};
for (let key in person) {
  console.log(key + ": " + person[key]);
}

// for...of (iterable öğeler için)
let colors = ["red", "green", "blue"];
for (let color of colors) {
  console.log(color);
}
```

**while Döngüsü:**

```javascript
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}

// do...while
let j = 0;
do {
  console.log(j);
  j++;
} while (j < 3);
```

---

## Fonksiyonlar

### Callback Fonksiyonları

**Temel Callback Kullanımı:**

```javascript
// Callback fonksiyon örneği
function processData(data, callback) {
  console.log('Veri işleniyor:', data);
  let result = data * 2;
  callback(result);
}

function displayResult(result) {
  console.log('Sonuç:', result);
}

processData(5, displayResult); // "Veri işleniyor: 5" -> "Sonuç: 10"

// Anonymous callback
processData(10, function(result) {
  console.log('Anonymous callback sonucu:', result);
});

// Arrow function callback
processData(15, (result) => {
  console.log('Arrow callback sonucu:', result);
});
```

**Array metodlarında callback:**

```javascript
let numbers = [1, 2, 3, 4, 5];

// map ile callback
numbers.map(function(num) {
  return num * 2;
}); // [2, 4, 6, 8, 10]

// filter ile callback
numbers.filter(num => num % 2 === 0); // [2, 4]

// Event listener'da callback
// button.addEventListener('click', function() {
//   console.log('Button clicked!');
// });
```

### Closure

**Closure Kavramı:**

```javascript
// Temel closure örneği
function outerFunction(x) {
  // Bu fonksiyon kapsamındaki değişken
  let outerVariable = x;
  
  // İç fonksiyon (closure)
  function innerFunction(y) {
    // Dış fonksiyonun değişkenine erişim
    console.log(outerVariable + y);
  }
  
  return innerFunction;
}

let myClosure = outerFunction(10);
myClosure(5); // 15 (10 + 5)

// Closure ile private değişkenler
function createCounter() {
  let count = 0; // Private değişken
  
  return {
    increment: function() {
      count++;
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    getCount: function() {
      return count;
    }
  };
}

let counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// console.log(counter.count);    // undefined (private)
```

**Pratik Closure Örnekleri:**

```javascript
// Module pattern ile closure
let myModule = (function() {
  let privateVar = 0;
  
  function privateFunction() {
    console.log('Bu private bir fonksiyon');
  }
  
  return {
    publicMethod: function() {
      privateVar++;
      privateFunction();
      return privateVar;
    },
    getPrivateVar: function() {
      return privateVar;
    }
  };
})();

console.log(myModule.publicMethod()); // "Bu private bir fonksiyon" -> 1
console.log(myModule.getPrivateVar()); // 1

// Closure ile factory function
function createMultiplier(multiplier) {
  return function(x) {
    return x * multiplier;
  };
}

let double = createMultiplier(2);
let triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### This Bağlamı

**This Kullanımı:**

```javascript
// Global context'te this
console.log(this); // Window object (browser) veya global (Node.js)

// Object method'unda this
let person = {
  name: 'John',
  age: 30,
  greet: function() {
    console.log(`Merhaba, ben ${this.name}`); // this -> person object
  },
  getInfo: function() {
    return `${this.name} - ${this.age} yaşında`;
  }
};

person.greet(); // "Merhaba, ben John"

// Constructor function'da this
function Person(name, age) {
  this.name = name; // this -> yeni oluşturulan instance
  this.age = age;
  this.greet = function() {
    console.log(`Merhaba, ben ${this.name}`);
  };
}

let john = new Person('John', 30);
john.greet(); // "Merhaba, ben John"

// Arrow function'da this binding yok
let obj = {
  name: 'JavaScript',
  regularFunction: function() {
    console.log(this.name); // "JavaScript"
    
    let arrowFunction = () => {
      console.log(this.name); // "JavaScript" (parent scope'tan alır)
    };
    arrowFunction();
  },
  arrowMethod: () => {
    console.log(this.name); // undefined (global this)
  }
};

obj.regularFunction();
obj.arrowMethod();
```

**Call, Apply, Bind Metodları:**

```javascript
let person1 = {name: 'John'};
let person2 = {name: 'Jane'};

function greet(greeting, punctuation) {
  console.log(`${greeting}, ben ${this.name}${punctuation}`);
}

// call() - argumentleri ayrı ayrı geçir
greet.call(person1, 'Merhaba', '!'); // "Merhaba, ben John!"
greet.call(person2, 'Selam', '.'); // "Selam, ben Jane."

// apply() - argumentleri array olarak geçir
greet.apply(person1, ['Hey', '?']); // "Hey, ben John?"

// bind() - yeni fonksiyon döndürür
let boundGreet = greet.bind(person2, 'Hola');
boundGreet('!!!'); // "Hola, ben Jane!!!"

// Event handler'da this
let button = document.createElement('button');
button.textContent = 'Click me';

let handler = {
  count: 0,
  handleClick: function() {
    this.count++;
    console.log(`Clicked ${this.count} times`);
  }
};

// bind kullanarak this'i koruyal
button.addEventListener('click', handler.handleClick.bind(handler));
```

### Fonksiyon Bildirimleri

**Function Declaration:**

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(5, 3)); // 8
```

**Function Expression:**

```javascript
const multiply = function(a, b) {
  return a * b;
};

console.log(multiply(4, 6)); // 24
```

**Varsayılan Parametreler:**

```javascript
function greet(name = "Dünya") {
  return `Merhaba ${name}!`;
}

console.log(greet());       // "Merhaba Dünya!"
console.log(greet("Ali"));  // "Merhaba Ali!"
```

**Rest Parametreleri:**

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

### Arrow Fonksiyonları

**Temel Arrow Fonksiyon Sözdizimi:**

```javascript
// Tek satır, otomatik return
const square = x => x * x;

// Çoklu parametreler
const add = (a, b) => a + b;

// Blok body
const complexFunction = (x, y) => {
  let result = x * y;
  return result + 10;
};

// this binding farkı
const obj = {
  name: "JavaScript",
  regular: function() {
    console.log(this.name); // "JavaScript"
  },
  arrow: () => {
    console.log(this.name); // undefined (global scope)
  }
};
```

### IIFE

**Immediately Invoked Function Expression:**

```javascript
// IIFE Klasik Syntax
(function() {
  console.log("IIFE çalışıyor!");
})();

// IIFE Arrow Function
(() => {
  console.log("Arrow IIFE çalışıyor!");
})();

// Parametreli IIFE
(function(name) {
  console.log(`Merhaba ${name}!`);
})("JavaScript");
```

---

## Nesneler ve Diziler

### Nesne Manipülasyonu

**Nesne Oluşturma ve Erişim:**

```javascript
// Nesne literal
let person = {
  name: "John",
  age: 30,
  "full-name": "John Doe" // Boşluklu özellik adı
};

// Nokta notasyonu
console.log(person.name); // "John"

// Köşeli parantez notasyonu
console.log(person["age"]); // 30
console.log(person["full-name"]); // "John Doe"

// Dinamik özellik erişimi
let property = "name";
console.log(person[property]); // "John"

// Özellik ekleme
person.city = "Istanbul";
person["country"] = "Turkey";
```

**Nesne Metodları:**

```javascript
let person = {
  name: "John",
  age: 30,
  greet: function() {
    return `Merhaba, ben ${this.name}`;
  },
  // ES6 method syntax
  introduce() {
    return `${this.name}, ${this.age} yaşında`;
  }
};

console.log(person.greet()); // "Merhaba, ben John"

// Object.keys(), Object.values(), Object.entries()
let user = {
  name: "John",
  age: 30,
  city: "Istanbul"
};

let keys = Object.keys(user);
console.log(keys); // ["name", "age", "city"]

let values = Object.values(user);
console.log(values); // ["John", 30, "Istanbul"]

let entries = Object.entries(user);
console.log(entries); // [["name", "John"], ["age", 30], ["city", "Istanbul"]]

// entries ile iteration
for (let [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}

// Object.assign() - nesneleri birleştir
let target = {a: 1, b: 2};
let source1 = {b: 3, c: 4};
let source2 = {c: 5, d: 6};

let result = Object.assign(target, source1, source2);
console.log(result); // {a: 1, b: 3, c: 5, d: 6}
console.log(target === result); // true (target değişti)

// Shallow copy için
let copy = Object.assign({}, user);

// Object.freeze() - nesneyi donduir
let frozenObj = Object.freeze({name: "John", age: 30});
frozenObj.age = 31; // Sessizce ignore edilir (strict mode'da hata)
console.log(frozenObj.age); // 30

// Object.seal() - yeni property eklenmesini engelle
let sealedObj = Object.seal({name: "John", age: 30});
sealedObj.age = 31; // Bu çalışır
sealedObj.city = "Istanbul"; // Bu çalışmaz
console.log(sealedObj); // {name: "John", age: 31}
```

### Dizi Metodları

**Temel Dizi Metodları:**

```javascript
let fruits = ["apple", "banana", "orange"];

// Ekleme/Çıkarma
fruits.push("grape");      // Sona ekle
fruits.unshift("mango");   // Başa ekle
let last = fruits.pop();   // Sondan çıkar
let first = fruits.shift(); // Baştan çıkar

// Splice ve Slice
let numbers = [1, 2, 3, 4, 5];
numbers.splice(2, 1, 10); // Index 2'den 1 eleman sil, 10 ekle
console.log(numbers); // [1, 2, 10, 4, 5]

let sliced = numbers.slice(1, 4); // Index 1-4 arası kopyala
console.log(sliced); // [2, 10, 4]

// Concat - dizileri birleştir
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = arr1.concat(arr2);
console.log(combined); // [1, 2, 3, 4, 5, 6]

let multiConcat = arr1.concat(arr2, [7, 8], 9);
console.log(multiConcat); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// Join - diziyi string'e çevir
let colors = ["red", "green", "blue"];
console.log(colors.join()); // "red,green,blue"
console.log(colors.join(" - ")); // "red - green - blue"
console.log(colors.join("")); // "redgreenblue"

let numbers = [1, 2, 3, 4, 5];
console.log(numbers.join(" + ")); // "1 + 2 + 3 + 4 + 5"
```

**Yüksek Seviye Dizi Metodları:**

```javascript
let numbers = [1, 2, 3, 4, 5];

// map - her elemana işlem uygula
let doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - koşula uyan elemanları filtrele
let evens = numbers.filter(num => num % 2 === 0);
console.log(evens); // [2, 4]

// reduce - tek değere indirge
let sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15

// find - koşula uyan ilk elemanı bul
let found = numbers.find(num => num > 3);
console.log(found); // 4

// forEach - her eleman için işlem yap
numbers.forEach((num, index) => {
  console.log(`Index ${index}: ${num}`);
});

// every - tüm elemanlar koşulu sağlıyor mu?
let allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true

// some - en az bir eleman koşulu sağlıyor mu?
let hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven); // true

// Array.from() - iterable'ı array'e çevir
let nodeList = document.querySelectorAll('div'); // NodeList
let divArray = Array.from(nodeList);

let string = "hello";
let charArray = Array.from(string);
console.log(charArray); // ['h', 'e', 'l', 'l', 'o']

// Mapping function ile
let numberStrings = ['1', '2', '3'];
let numbers = Array.from(numberStrings, x => parseInt(x));
console.log(numbers); // [1, 2, 3]

// Length ile boş array oluştur
let emptyArray = Array.from({length: 5}, (_, i) => i * 2);
console.log(emptyArray); // [0, 2, 4, 6, 8]

// Array.of() - argumentlardan array oluştur
let arr1 = Array.of(7); // [7]
let arr2 = Array(7);    // [empty × 7]
let arr3 = Array.of(1, 2, 3); // [1, 2, 3]
console.log(arr1.length); // 1
console.log(arr2.length); // 7
```

### Destructuring

**Dizi Destructuring:**

```javascript
let colors = ["red", "green", "blue", "yellow"];

// Temel destructuring
let [first, second] = colors;
console.log(first);  // "red"
console.log(second); // "green"

// Elemanları atlama
let [, , third] = colors;
console.log(third); // "blue"

// Rest ile kalan elemanlar
let [primary, ...others] = colors;
console.log(primary); // "red"
console.log(others);  // ["green", "blue", "yellow"]

// Varsayılan değerler
let [a, b, c, d, e = "purple"] = colors;
console.log(e); // "purple"
```

**Nesne Destructuring:**

```javascript
let person = {
  name: "John",
  age: 30,
  city: "Istanbul",
  country: "Turkey"
};

// Temel destructuring
let {name, age} = person;
console.log(name); // "John"
console.log(age);  // 30

// Yeniden adlandırma
let {name: fullName, age: years} = person;
console.log(fullName); // "John"
console.log(years);    // 30

// Varsayılan değerler
let {name, age, profession = "Developer"} = person;
console.log(profession); // "Developer"

// Rest ile kalan özellikler
let {name, ...details} = person;
console.log(details); // {age: 30, city: "Istanbul", country: "Turkey"}
```

### Spread ve Rest Operatörleri

**Spread Operatörü (...):**

```javascript
// Dizi spread
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Nesne spread
let obj1 = {a: 1, b: 2};
let obj2 = {c: 3, d: 4};
let merged = {...obj1, ...obj2};
console.log(merged); // {a: 1, b: 2, c: 3, d: 4}

// Fonksiyon argümanları
function sum(a, b, c) {
  return a + b + c;
}
let numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

**Rest Operatörü (...):**

```javascript
// Fonksiyon parametrelerinde
function multiply(multiplier, ...numbers) {
  return numbers.map(num => num * multiplier);
}
console.log(multiply(2, 1, 2, 3, 4)); // [2, 4, 6, 8]

// Destructuring'de
let [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest);  // [2, 3, 4, 5]
```

---

## Asenkron JavaScript

### Promises

**Promise Oluşturma:**

```javascript
// Temel Promise
let promise = new Promise((resolve, reject) => {
  let success = true;
  
  if (success) {
    resolve("İşlem başarılı!");
  } else {
    reject("İşlem başarısız!");
  }
});

// Promise kullanımı
promise
  .then(result => {
    console.log(result); // "İşlem başarılı!"
  })
  .catch(error => {
    console.log(error);
  })
  .finally(() => {
    console.log("İşlem tamamlandı");
  });
```

**Promise Metodları:**

```javascript
// Promise.all - Tüm promise'ler tamamlanana kadar bekle
let promise1 = Promise.resolve(3);
let promise2 = new Promise(resolve => setTimeout(() => resolve('foo'), 1000));
let promise3 = Promise.resolve(42);

Promise.all([promise1, promise2, promise3])
  .then(values => {
    console.log(values); // [3, "foo", 42]
  });

// Promise.race - İlk tamamlanan promise'i döndür
Promise.race([promise1, promise2, promise3])
  .then(value => {
    console.log(value); // 3 (en hızlı tamamlanan)
  });

// Promise.allSettled - Tüm sonuçları döndür (başarılı/başarısız)
Promise.allSettled([promise1, promise2, Promise.reject("error")])
  .then(results => {
    console.log(results);
    // [
    //   {status: "fulfilled", value: 3},
    //   {status: "fulfilled", value: "foo"},
    //   {status: "rejected", reason: "error"}
    // ]
  });

// Promise.any() - ilk başarılı promise'i döndür (ES2021)
let promise1 = Promise.reject('Error 1');
let promise2 = Promise.reject('Error 2');
let promise3 = Promise.resolve('Success!');

Promise.any([promise1, promise2, promise3])
  .then(value => {
    console.log(value); // "Success!"
  })
  .catch(error => {
    console.log(error); // AggregateError (tümü reject olursa)
  });
```

### Async/Await

**Async Function Sözdizimi:**

```javascript
// Temel async function
async function fetchData() {
  return "Veri alındı!";
}

fetchData().then(data => console.log(data)); // "Veri alındı!"
```

**Await Kullanımı:**

```javascript
// Promise'leri await ile bekle
async function processData() {
  try {
    let data1 = await fetch('/api/data1');
    let data2 = await fetch('/api/data2');
    
    let result1 = await data1.json();
    let result2 = await data2.json();
    
    return {result1, result2};
  } catch (error) {
    console.error('Hata:', error);
  }
}

// Paralel işlemler için
async function parallelRequests() {
  try {
    let [data1, data2] = await Promise.all([
      fetch('/api/data1'),
      fetch('/api/data2')
    ]);
    
    let results = await Promise.all([
      data1.json(),
      data2.json()
    ]);
    
    return results;
  } catch (error) {
    console.error('Hata:', error);
  }
}
```

**Hata Yönetimi:**

```javascript
async function handleErrors() {
  try {
    let response = await fetch('/api/data');
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    let data = await response.json();
    return data;
  } catch (error) {
    if (error instanceof TypeError) {
      console.error('Network error:', error);
    } else {
      console.error('Other error:', error);
    }
    throw error; // Hatayı üst seviyeye ilet
  } finally {
    console.log('Cleanup işlemleri');
  }
}
```

---

## Nesne Yönelimli Programlama

### Class Sözdizimi

**Temel Class:**

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  // Method
  greet() {
    return `Merhaba, ben ${this.name}`;
  }
  
  // Getter
  get info() {
    return `${this.name} - ${this.age} yaş`;
  }
  
  // Setter
  set age(newAge) {
    if (newAge > 0) {
      this._age = newAge;
    }
  }
  
  get age() {
    return this._age;
  }
  
  // Static method
  static species() {
    return 'Homo sapiens';
  }
}

// Gelişmiş Getters ve Setters
class Temperature {
  constructor(celsius = 0) {
    this._celsius = celsius;
  }
  
  // Getter
  get celsius() {
    return this._celsius;
  }
  
  // Setter
  set celsius(value) {
    if (typeof value !== 'number' || value < -273.15) {
      throw new Error('Invalid temperature');
    }
    this._celsius = value;
  }
  
  // Computed properties with getters
  get fahrenheit() {
    return (this._celsius * 9/5) + 32;
  }
  
  set fahrenheit(value) {
    this.celsius = (value - 32) * 5/9;
  }
  
  get kelvin() {
    return this._celsius + 273.15;
  }
  
  set kelvin(value) {
    this.celsius = value - 273.15;
  }
  
  // Getter ile validation
  get description() {
    if (this._celsius < 0) return 'Freezing';
    if (this._celsius < 20) return 'Cold';
    if (this._celsius < 30) return 'Warm';
    return 'Hot';
  }
}

let john = new Person('John', 30);
console.log(john.greet()); // "Merhaba, ben John"
console.log(john.info);    // "John - 30 yaş"
console.log(Person.species()); // "Homo sapiens"
```

### Prototype ve Prototypal Inheritance

**Prototype Temelleri:**

```javascript
// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Prototype'a method ekleme
Person.prototype.greet = function() {
  return `Merhaba, ben ${this.name}`;
};

Person.prototype.getAge = function() {
  return this.age;
};

let john = new Person('John', 30);
console.log(john.greet()); // "Merhaba, ben John"
console.log(john.getAge()); // 30

// Prototype chain kontrolü
console.log(john.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
```

**Prototypal Inheritance:**

```javascript
// Parent constructor
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} bir ses çıkarıyor`;
};

// Child constructor
function Dog(name, breed) {
  Animal.call(this, name); // Parent constructor'ı çağır
  this.breed = breed;
}

// Inheritance kurma
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Child'a özel method
Dog.prototype.bark = function() {
  return `${this.name} havlıyor: Woof!`;
};

// Parent method'unu override
Dog.prototype.speak = function() {
  return `${this.name} köpek sesi çıkarıyor`;
};

let myDog = new Dog('Buddy', 'Golden Retriever');
console.log(myDog.speak()); // "Buddy köpek sesi çıkarıyor"
console.log(myDog.bark());  // "Buddy havlıyor: Woof!"

// Prototype chain
console.log(myDog instanceof Dog);    // true
console.log(myDog instanceof Animal); // true
console.log(myDog instanceof Object); // true
```

**Modern Prototype Metodları:**

```javascript
// Object.create() ile inheritance
let animal = {
  type: 'Animal',
  speak: function() {
    return `${this.name} konuşuyor`;
  }
};

let dog = Object.create(animal);
dog.name = 'Rex';
dog.bark = function() {
  return `${this.name} havlıyor`;
};

console.log(dog.speak()); // "Rex konuşuyor"
console.log(dog.bark());  // "Rex havlıyor"

// Object.setPrototypeOf() ve Object.getPrototypeOf()
let cat = {name: 'Whiskers'};
Object.setPrototypeOf(cat, animal);
console.log(cat.speak()); // "Whiskers konuşuyor"
console.log(Object.getPrototypeOf(cat) === animal); // true

// hasOwnProperty vs prototype properties
console.log(dog.hasOwnProperty('name')); // true (kendi property'si)
console.log(dog.hasOwnProperty('speak')); // false (prototype'dan geliyor)
console.log('speak' in dog); // true (prototype chain'de var)
```

### Inheritance

**Class Inheritance:**

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} bir ses çıkarıyor`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Parent constructor'ı çağır
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} havlıyor: Woof!`;
  }
  
  // Ek method
  wagTail() {
    return `${this.name} kuyruğunu sallıyor`;
  }
}

let myDog = new Dog('Buddy', 'Golden Retriever');
console.log(myDog.speak());   // "Buddy havlıyor: Woof!"
console.log(myDog.wagTail()); // "Buddy kuyruğunu sallıyor"
```

**Private Fields (ES2022):**

```javascript
class BankAccount {
  #balance = 0; // Private field
  
  constructor(initialBalance) {
    this.#balance = initialBalance;
  }
  
  deposit(amount) {
    this.#balance += amount;
    return this.#balance;
  }
  
  withdraw(amount) {
    if (amount <= this.#balance) {
      this.#balance -= amount;
      return this.#balance;
    }
    throw new Error('Yetersiz bakiye');
  }
  
  get balance() {
    return this.#balance;
  }
}

let account = new BankAccount(1000);
console.log(account.deposit(500)); // 1500
// console.log(account.#balance); // SyntaxError: Private field
```

---

## ES6+ Özellikleri

### Varsayılan Parametreler

**Function Default Parameters:**

```javascript
// ES5 yöntemi
function greetOld(name) {
  name = name || "Dünya";
  return "Merhaba " + name + "!";
}

// ES6+ varsayılan parametreler
function greet(name = "Dünya", greeting = "Merhaba") {
  return `${greeting} ${name}!`;
}

console.log(greet()); // "Merhaba Dünya!"
console.log(greet("Ali")); // "Merhaba Ali!"
console.log(greet("Ayşe", "Selam")); // "Selam Ayşe!"

// Varsayılan değer olarak expression
function createUser(name, role = "user", id = Date.now()) {
  return { name, role, id };
}

// Varsayılan değer olarak function call
function getDefaultRole() {
  return "guest";
}

function createGuestUser(name, role = getDefaultRole()) {
  return { name, role };
}

// Destructuring ile varsayılan değerler
function configure({host = "localhost", port = 3000, ssl = false} = {}) {
  return `${ssl ? "https" : "http"}://${host}:${port}`;
}

console.log(configure()); // "http://localhost:3000"
console.log(configure({host: "example.com", ssl: true})); // "https://example.com:3000"
```

### Template Literals

**Temel Template Literals:**

```javascript
let name = 'JavaScript';
let version = 'ES6';

// String interpolation
let message = `Merhaba ${name}! ${version} kullanıyoruz.`;
console.log(message); // "Merhaba JavaScript! ES6 kullanıyoruz."

// Çok satırlı string
let multiline = `
  Bu bir
  çok satırlı
  string örneğidir.
`;

// Expression'lar
let a = 5, b = 10;
let result = `${a} + ${b} = ${a + b}`;
console.log(result); // "5 + 10 = 15"

// Function call'lar
function greet(name) {
  return `Merhaba ${name}!`;
}
let greeting = `${greet('Ali')} Nasılsın?`;
console.log(greeting); // "Merhaba Ali! Nasılsın?"
```

**Tagged Template Literals:**

```javascript
function highlight(strings, ...values) {
  return strings.reduce((result, string, i) => {
    let value = values[i] ? `<mark>${values[i]}</mark>` : '';
    return result + string + value;
  }, '');
}

let name = 'JavaScript';
let type = 'programming language';
let highlighted = highlight`${name} is a ${type}`;
console.log(highlighted); // "<mark>JavaScript</mark> is a <mark>programming language</mark>"
```

### Modüller

**Export Örnekleri:**

```javascript
// math.js - Named exports
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export const PI = 3.14159;

// Default export
export default function subtract(a, b) {
  return a - b;
}

// Alternative export syntax
function divide(a, b) {
  return a / b;
}

function power(a, b) {
  return a ** b;
}

export { divide, power };
```

**Import Örnekleri:**

```javascript
// app.js - Import kullanımı

// Default import
import subtract from './math.js';

// Named imports
import { add, multiply, PI } from './math.js';

// Import with alias
import { divide as div, power as pow } from './math.js';

// Import everything
import * as MathUtils from './math.js';

// Mixed import
import subtract, { add, multiply } from './math.js';

console.log(add(5, 3));        // 8
console.log(subtract(10, 4));  // 6
console.log(PI);               // 3.14159
console.log(div(15, 3));       // 5
console.log(MathUtils.power(2, 3)); // 8
```

### Generator Fonksiyonları

**Temel Generator Kullanımı:**

```javascript
// Generator function tanımı
function* simpleGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

let gen = simpleGenerator();
console.log(gen.next()); // {value: 1, done: false}
console.log(gen.next()); // {value: 2, done: false}
console.log(gen.next()); // {value: 3, done: false}
console.log(gen.next()); // {value: undefined, done: true}

// for...of ile generator
for (let value of simpleGenerator()) {
  console.log(value); // 1, 2, 3
}
```

**Gelişmiş Generator Örnekleri:**

```javascript
// Sonsuz sayı üretici
function* infiniteNumbers() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

let numbers = infiniteNumbers();
console.log(numbers.next().value); // 0
console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 2

// Fibonacci generator
function* fibonacci() {
  let [prev, curr] = [0, 1];
  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

let fib = fibonacci();
for (let i = 0; i < 10; i++) {
  console.log(fib.next().value);
}

// Generator ile veri işleme
function* processData(data) {
  for (let item of data) {
    yield item.toUpperCase();
  }
}

let words = ['hello', 'world', 'javascript'];
for (let processed of processData(words)) {
  console.log(processed); // HELLO, WORLD, JAVASCRIPT
}

// Generator delegation (yield*)
function* gen1() {
  yield 1;
  yield 2;
}

function* gen2() {
  yield 3;
  yield 4;
}

function* combinedGen() {
  yield* gen1();
  yield* gen2();
}

console.log([...combinedGen()]); // [1, 2, 3, 4]
```

### Set ve Map Koleksiyonları

**Set Kullanımı:**

```javascript
// Set oluşturma
let mySet = new Set();

// Değer ekleme
mySet.add(1);
mySet.add(2);
mySet.add(2); // Duplicate, ignore edilir
mySet.add('hello');

console.log(mySet); // Set {1, 2, 'hello'}
console.log(mySet.size); // 3

// Değer kontrolü
console.log(mySet.has(1)); // true
console.log(mySet.has(3)); // false

// Değer silme
mySet.delete(2);
console.log(mySet.has(2)); // false

// Array'den Set oluşturma
let arr = [1, 2, 2, 3, 3, 4];
let uniqueSet = new Set(arr);
console.log([...uniqueSet]); // [1, 2, 3, 4]

// Set iteration
let colorSet = new Set(['red', 'green', 'blue']);
for (let color of colorSet) {
  console.log(color);
}

// Set metodları
colorSet.forEach(color => console.log(color));

// Set'i array'e çevirme
let setArray = Array.from(colorSet);
let spreadArray = [...colorSet];
```

**Map Kullanımı:**

```javascript
// Map oluşturma
let myMap = new Map();

// Key-value ekleme
myMap.set('name', 'John');
myMap.set('age', 30);
myMap.set(1, 'number key');
myMap.set(true, 'boolean key');

console.log(myMap.get('name')); // "John"
console.log(myMap.get(1)); // "number key"
console.log(myMap.size); // 4

// Key kontrolü
console.log(myMap.has('age')); // true

// Key silme
myMap.delete('age');
console.log(myMap.has('age')); // false

// Array'den Map oluşturma
let entries = [['a', 1], ['b', 2], ['c', 3]];
let mapFromArray = new Map(entries);

// Object'den Map oluşturma
let obj = {x: 10, y: 20};
let mapFromObj = new Map(Object.entries(obj));

// Map iteration
for (let [key, value] of myMap) {
  console.log(key, value);
}

// Keys, values, entries
for (let key of myMap.keys()) {
  console.log('Key:', key);
}

for (let value of myMap.values()) {
  console.log('Value:', value);
}

for (let [key, value] of myMap.entries()) {
  console.log(key, '=', value);
}

// Map'i Object'e çevirme
let mapObj = Object.fromEntries(myMap);
```

### WeakMap ve WeakSet

**WeakMap:**

```javascript
// WeakMap sadece object key'leri kabul eder
let wm = new WeakMap();

let obj1 = {id: 1};
let obj2 = {id: 2};

wm.set(obj1, 'data for obj1');
wm.set(obj2, 'data for obj2');

console.log(wm.get(obj1)); // "data for obj1"
console.log(wm.has(obj2)); // true

// Object referansı silinirse WeakMap'teki entry de silinir
obj1 = null; // obj1 artık garbage collect edilebilir

// WeakMap private data için kullanılabilir
let privateData = new WeakMap();

class User {
  constructor(name) {
    this.name = name;
    privateData.set(this, {password: 'secret123'});
  }
  
  getPassword() {
    return privateData.get(this).password;
  }
}

let user = new User('John');
console.log(user.getPassword()); // "secret123"
```

**WeakSet:**

```javascript
// WeakSet sadece object değerleri kabul eder
let ws = new WeakSet();

let obj1 = {name: 'Object 1'};
let obj2 = {name: 'Object 2'};

ws.add(obj1);
ws.add(obj2);

console.log(ws.has(obj1)); // true

ws.delete(obj2);
console.log(ws.has(obj2)); // false

// WeakSet tracking için kullanılabilir
let visitedObjects = new WeakSet();

function process(obj) {
  if (visitedObjects.has(obj)) {
    return; // Zaten işlenmiş
  }
  
  visitedObjects.add(obj);
  // Object'i işle...
}
```

---

## DOM Manipülasyonu

### Element Seçme

**Element Seçim Metodları:**

```javascript
// ID ile seç
let element = document.getElementById('myId');

// Class ile seç
let elements = document.getElementsByClassName('myClass');
let firstElement = elements[0];

// Tag ile seç
let paragraphs = document.getElementsByTagName('p');

// CSS selector ile seç (ilk eşleşen)
let firstDiv = document.querySelector('.myClass');
let firstInput = document.querySelector('input[type="text"]');

// CSS selector ile seç (tüm eşleşenler)
let allDivs = document.querySelectorAll('.myClass');
let allInputs = document.querySelectorAll('input');

// NodeList'i Array'e çevir
let divsArray = Array.from(allDivs);
```

### Element Manipülasyonu

**İçerik Değiştirme:**

```javascript
let element = document.getElementById('myElement');

// Text içeriği
element.textContent = 'Yeni metin';
element.innerText = 'Görünen metin';

// HTML içeriği
element.innerHTML = '<strong>Kalın metin</strong>';

// Attribute'ler
element.setAttribute('class', 'newClass');
element.setAttribute('data-id', '123');
let className = element.getAttribute('class');

// Class manipülasyonu
element.classList.add('newClass');
element.classList.remove('oldClass');
element.classList.toggle('active');
let hasClass = element.classList.contains('myClass');

// Style değiştirme
element.style.color = 'red';
element.style.backgroundColor = 'yellow';
element.style.fontSize = '20px';
```

**Element Oluşturma ve Ekleme:**

```javascript
// Yeni element oluştur
let newDiv = document.createElement('div');
newDiv.textContent = 'Yeni div';
newDiv.className = 'created-element';

// Element ekle
let container = document.getElementById('container');
container.appendChild(newDiv);

// Belirli pozisyona ekle
container.insertBefore(newDiv, container.firstChild);

// innerHTML ile ekle
container.innerHTML += '<p>Yeni paragraf</p>';

// Element sil
let elementToRemove = document.getElementById('removeMe');
elementToRemove.remove(); // Modern yöntem
// elementToRemove.parentNode.removeChild(elementToRemove); // Eski yöntem
```

### Olay Yönetimi

**Event Listener Ekleme:**

```javascript
let button = document.getElementById('myButton');

// Click event
button.addEventListener('click', function() {
  console.log('Butona tıklandı!');
});

// Arrow function ile
button.addEventListener('click', () => {
  console.log('Arrow function ile click!');
});

// Event object kullanma
button.addEventListener('click', function(event) {
  event.preventDefault(); // Default davranışı engelle
  console.log('Event target:', event.target);
  console.log('Mouse position:', event.clientX, event.clientY);
});

// Multiple events
function handleInput(event) {
  console.log('Input değeri:', event.target.value);
}

let input = document.getElementById('myInput');
input.addEventListener('input', handleInput);
input.addEventListener('focus', () => console.log('Input focused'));
input.addEventListener('blur', () => console.log('Input blurred'));
```

**Event Types ve Detayları:**

```javascript
// Mouse Events
button.addEventListener('click', (e) => {
  console.log('Clicked at:', e.clientX, e.clientY);
  console.log('Button:', e.button); // 0: left, 1: middle, 2: right
});

button.addEventListener('dblclick', (e) => {
  console.log('Double clicked');
});

element.addEventListener('mouseenter', (e) => {
  console.log('Mouse entered');
});

element.addEventListener('mouseleave', (e) => {
  console.log('Mouse left');
});

element.addEventListener('mousedown', (e) => {
  console.log('Mouse pressed');
});

element.addEventListener('mouseup', (e) => {
  console.log('Mouse released');
});

// Keyboard Events
document.addEventListener('keydown', (e) => {
  console.log('Key pressed:', e.key);
  console.log('Key code:', e.keyCode); // Deprecated
  console.log('Code:', e.code);
  
  // Modifier keys
  if (e.ctrlKey) console.log('Ctrl was held');
  if (e.shiftKey) console.log('Shift was held');
  if (e.altKey) console.log('Alt was held');
  if (e.metaKey) console.log('Meta was held'); // Windows key / Cmd
});

document.addEventListener('keyup', (e) => {
  console.log('Key released:', e.key);
});

// Form Events
let form = document.querySelector('form');
let input = document.querySelector('input');

form.addEventListener('submit', (e) => {
  e.preventDefault(); // Form gönderimini engelle
  console.log('Form submitted');
});

input.addEventListener('input', (e) => {
  console.log('Input changed:', e.target.value);
});

input.addEventListener('change', (e) => {
  console.log('Input value changed:', e.target.value);
});

input.addEventListener('focus', (e) => {
  console.log('Input focused');
  e.target.style.borderColor = 'blue';
});

input.addEventListener('blur', (e) => {
  console.log('Input blurred');
  e.target.style.borderColor = '';
});

// Window Events
window.addEventListener('load', () => {
  console.log('Page fully loaded');
});

window.addEventListener('resize', () => {
  console.log('Window resized:', window.innerWidth, window.innerHeight);
});

window.addEventListener('scroll', () => {
  console.log('Page scrolled:', window.scrollY);
});

// Custom Events
let customEvent = new CustomEvent('myCustomEvent', {
  detail: { message: 'Hello from custom event!' }
});

element.addEventListener('myCustomEvent', (e) => {
  console.log('Custom event received:', e.detail.message);
});

element.dispatchEvent(customEvent);

// Touch Events (Mobile)
element.addEventListener('touchstart', (e) => {
  console.log('Touch started');
  e.preventDefault();
});

element.addEventListener('touchmove', (e) => {
  console.log('Touch moved');
  e.preventDefault();
});

element.addEventListener('touchend', (e) => {
  console.log('Touch ended');
});
```

**Event Delegation:**

```javascript
// Container'a tek listener ekle, child elementler için de çalışır
let container = document.getElementById('container');

container.addEventListener('click', function(event) {
  if (event.target.classList.contains('button')) {
    console.log('Button clicked:', event.target.textContent);
  }
  
  if (event.target.tagName === 'LI') {
    console.log('List item clicked:', event.target.textContent);
  }
});
```

### Iterators ve Iterables

**Iterator Protocol:**

```javascript
// Custom iterator oluşturma
function createIterator(array) {
  let index = 0;
  
  return {
    next: function() {
      if (index < array.length) {
        return { value: array[index++], done: false };
      } else {
        return { done: true };
      }
    }
  };
}

let iterator = createIterator([1, 2, 3]);
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { done: true }
```

**Iterable Protocol:**

```javascript
// Custom iterable object
let myIterable = {
  data: ['a', 'b', 'c'],
  [Symbol.iterator]: function() {
    let index = 0;
    let data = this.data;
    
    return {
      next: function() {
        if (index < data.length) {
          return { value: data[index++], done: false };
        } else {
          return { done: true };
        }
      }
    };
  }
};

// for...of ile kullanım
for (let item of myIterable) {
  console.log(item); // a, b, c
}

// Spread operator ile
console.log([...myIterable]); // ['a', 'b', 'c']

// Array.from ile
console.log(Array.from(myIterable)); // ['a', 'b', 'c']
```

**Built-in Iterables:**

```javascript
// String iterable
let str = "hello";
for (let char of str) {
  console.log(char); // h, e, l, l, o
}

// Array iterable
let arr = [1, 2, 3];
let arrIterator = arr[Symbol.iterator]();
console.log(arrIterator.next()); // { value: 1, done: false }

// Map iterable
let map = new Map([['a', 1], ['b', 2]]);
for (let [key, value] of map) {
  console.log(key, value); // a 1, b 2
}

// Set iterable
let set = new Set([1, 2, 3]);
for (let value of set) {
  console.log(value); // 1, 2, 3
}
```

### Proxy ve Reflect

**Proxy Kullanımı:**

```javascript
// Temel proxy
let target = {
  name: "John",
  age: 30
};

let proxy = new Proxy(target, {
  get: function(target, property) {
    console.log(`Getting ${property}`);
    return target[property];
  },
  
  set: function(target, property, value) {
    console.log(`Setting ${property} = ${value}`);
    target[property] = value;
    return true;
  }
});

console.log(proxy.name); // "Getting name" -> "John"
proxy.age = 31; // "Setting age = 31"
```

**Gelişmiş Proxy Örnekleri:**

```javascript
// Validation proxy
function createValidatedUser(userData) {
  return new Proxy(userData, {
    set(target, property, value) {
      if (property === 'age' && (typeof value !== 'number' || value < 0)) {
        throw new Error('Age must be a positive number');
      }
      
      if (property === 'email' && !value.includes('@')) {
        throw new Error('Invalid email format');
      }
      
      target[property] = value;
      return true;
    }
  });
}

let user = createValidatedUser({});
user.age = 25; // OK
// user.age = -5; // Error: Age must be a positive number

// Property access logging
let loggingProxy = new Proxy({}, {
  get(target, property) {
    console.log(`Accessed property: ${property}`);
    return target[property];
  },
  
  has(target, property) {
    console.log(`Checked existence of property: ${property}`);
    return property in target;
  },
  
  ownKeys(target) {
    console.log('Listed all properties');
    return Object.keys(target);
  }
});

// Array-like object with proxy
function createArrayLike() {
  return new Proxy({}, {
    get(target, property) {
      if (property === 'length') {
        return Object.keys(target).length;
      }
      return target[property];
    },
    
    set(target, property, value) {
      target[property] = value;
      return true;
    }
  });
}
```

**Reflect API:**

```javascript
let obj = { name: "John", age: 30 };

// Reflect.get() - property alma
console.log(Reflect.get(obj, 'name')); // "John"
console.log(Reflect.get(obj, 'city', 'default')); // undefined

// Reflect.set() - property ayarlama
Reflect.set(obj, 'city', 'Istanbul');
console.log(obj.city); // "Istanbul"

// Reflect.has() - property varlığı kontrolü
console.log(Reflect.has(obj, 'name')); // true
console.log(Reflect.has(obj, 'salary')); // false

// Reflect.deleteProperty() - property silme
Reflect.deleteProperty(obj, 'age');
console.log('age' in obj); // false

// Reflect.ownKeys() - tüm key'leri alma
console.log(Reflect.ownKeys(obj)); // ["name", "city"]

// Reflect with Proxy
let proxyWithReflect = new Proxy(target, {
  get(target, property, receiver) {
    console.log(`Getting ${property}`);
    return Reflect.get(target, property, receiver);
  },
  
  set(target, property, value, receiver) {
    console.log(`Setting ${property} = ${value}`);
    return Reflect.set(target, property, value, receiver);
  }
});
```

---

## Sık Kullanılan Dahili Nesneler

### Regular Expressions (RegExp)

**Temel Regex Kullanımı:**

```javascript
// Regex oluşturma yolları
let regex1 = /pattern/flags;
let regex2 = new RegExp('pattern', 'flags');

// Temel pattern matching
let text = "JavaScript çok güçlü bir programlama dilidir";
let pattern = /JavaScript/;
console.log(pattern.test(text)); // true
console.log(text.match(pattern)); // ["JavaScript"]

// Case insensitive arama
let caseInsensitive = /javascript/i;
console.log(caseInsensitive.test(text)); // true

// Global arama (tüm eşleşmeleri bul)
let emails = "john@example.com, jane@test.org, admin@site.net";
let emailPattern = /\w+@\w+\.\w+/g;
console.log(emails.match(emailPattern)); 
// ["john@example.com", "jane@test.org", "admin@site.net"]
```

**Regex Flags:**

```javascript
// i - case insensitive
// g - global (tüm eşleşmeler)
// m - multiline
// s - dotall (. karakteri \n ile eşleşir)
// u - unicode
// y - sticky

let text = "Line 1\nLine 2\nLine 3";
let pattern = /^Line/gm; // Her satırın başındaki "Line"
console.log(text.match(pattern)); // ["Line", "Line", "Line"]
```

**Yaygın Regex Patterns:**

```javascript
// Email validation
let emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailRegex.test("user@example.com")); // true

// Phone number (Türkiye)
let phoneRegex = /^(\+90|0)?[0-9]{10}$/;
console.log(phoneRegex.test("05551234567")); // true
console.log(phoneRegex.test("+905551234567")); // true

// URL validation
let urlRegex = /^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)$/;

// Sadece sayılar
let numbersOnly = /^\d+$/;
console.log(numbersOnly.test("12345")); // true
console.log(numbersOnly.test("123a5")); // false

// Türkçe karakterler dahil kelimeler
let turkishWord = /^[a-zA-ZçğıöşüÇĞIİÖŞÜ\s]+$/;
console.log(turkishWord.test("Merhaba Dünya")); // true
```

**Regex Metodları:**

```javascript
let text = "Telefon: 0555-123-4567, Email: user@example.com";

// test() - boolean döndürür
let phonePattern = /\d{4}-\d{3}-\d{4}/;
console.log(phonePattern.test(text)); // true

// exec() - detaylı bilgi döndürür
let match = phonePattern.exec(text);
console.log(match); // ["0555-123-4567", index: 9, input: "...", groups: undefined]

// String metodları ile regex
console.log(text.match(/\d{4}-\d{3}-\d{4}/)); // ["0555-123-4567"]
console.log(text.search(/Email/)); // 23 (index)
console.log(text.replace(/\d{4}-\d{3}-\d{4}/, "XXX-XXX-XXXX")); 
// "Telefon: XXX-XXX-XXXX, Email: user@example.com"

// split ile regex
let sentence = "word1,word2;word3 word4";
console.log(sentence.split(/[,;\s]/)); // ["word1", "word2", "word3", "word4"]
```

**Capturing Groups:**

```javascript
let dateText = "Bugün 15-03-2024 tarihi";
let datePattern = /(\d{2})-(\d{2})-(\d{4})/;
let match = dateText.match(datePattern);

console.log(match[0]); // "15-03-2024" (tam eşleşme)
console.log(match[1]); // "15" (gün)
console.log(match[2]); // "03" (ay)
console.log(match[3]); // "2024" (yıl)

// Named groups (ES2018)
let namedPattern = /(?<day>\d{2})-(?<month>\d{2})-(?<year>\d{4})/;
let namedMatch = dateText.match(namedPattern);
console.log(namedMatch.groups.day);   // "15"
console.log(namedMatch.groups.month); // "03"
console.log(namedMatch.groups.year);  // "2024"
```

### JSON Metodları

**JSON.stringify() ve JSON.parse():**

```javascript
// Object'i JSON string'e çevirme
let person = {
  name: "John",
  age: 30,
  city: "Istanbul",
  hobbies: ["reading", "swimming"]
};

let jsonString = JSON.stringify(person);
console.log(jsonString); 
// '{"name":"John","age":30,"city":"Istanbul","hobbies":["reading","swimming"]}'

// JSON string'i object'e çevirme
let parsedObject = JSON.parse(jsonString);
console.log(parsedObject.name); // "John"

// Pretty printing (formatting)
let prettyJson = JSON.stringify(person, null, 2);
console.log(prettyJson);
/* 
{
  "name": "John",
  "age": 30,
  "city": "Istanbul",
  "hobbies": [
    "reading",
    "swimming"
  ]
}
*/
```

**JSON.stringify() ile Filtering:**

```javascript
let user = {
  id: 1,
  name: "John",
  password: "secret123",
  email: "john@example.com",
  age: 30
};

// Belirli property'leri hariç tut
let safeJson = JSON.stringify(user, ['id', 'name', 'email']);
console.log(safeJson); // '{"id":1,"name":"John","email":"john@example.com"}'

// Replacer function ile custom filtering
let customJson = JSON.stringify(user, function(key, value) {
  if (key === 'password') {
    return undefined; // Bu property'i hariç tut
  }
  if (typeof value === 'string') {
    return value.toUpperCase(); // String'leri büyük harfe çevir
  }
  return value;
});
```

**JSON Hata Yönetimi:**

```javascript
// Geçersiz JSON parse etme
try {
  let result = JSON.parse('{"name": "John", "age": 30,}'); // Sonda fazla virgül
} catch (error) {
  console.log('JSON parse hatası:', error.message);
  // "JSON parse hatası: Unexpected token } in JSON at position 27"
}

// Circular reference hatası
let obj1 = {name: "Object 1"};
let obj2 = {name: "Object 2"};
obj1.ref = obj2;
obj2.ref = obj1; // Circular reference

try {
  JSON.stringify(obj1);
} catch (error) {
  console.log('Stringify hatası:', error.message);
  // "Stringify hatası: Converting circular structure to JSON"
}
```

### Timer Fonksiyonları

**setTimeout ve clearTimeout:**

```javascript
// Tek seferlik gecikme
console.log("Başlangıç");

let timeoutId = setTimeout(function() {
  console.log("3 saniye sonra çalışır");
}, 3000);

// setTimeout'u iptal etme
// clearTimeout(timeoutId);

// Arrow function ile
setTimeout(() => {
  console.log("Arrow function ile timeout");
}, 1000);

// Parametre geçirme
function greet(name, greeting) {
  console.log(`${greeting}, ${name}!`);
}

setTimeout(greet, 2000, "Ali", "Merhaba");
```

**setInterval ve clearInterval:**

```javascript
// Tekrarlayan işlem
let counter = 0;
let intervalId = setInterval(function() {
  counter++;
  console.log(`Sayaç: ${counter}`);
  
  if (counter >= 5) {
    clearInterval(intervalId);
    console.log("Interval durduruldu");
  }
}, 1000);

// Saat örneği
function showTime() {
  let now = new Date();
  let timeString = now.toLocaleTimeString('tr-TR');
  console.log(timeString);
}

let clockId = setInterval(showTime, 1000);

// 10 saniye sonra saati durdur
setTimeout(() => {
  clearInterval(clockId);
  console.log("Saat durduruldu");
}, 10000);
```

**Modern Timer Alternatives:**

```javascript
// Promise tabanlı delay
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Async/await ile kullanım
async function delayedLog() {
  console.log("Başlangıç");
  await delay(2000);
  console.log("2 saniye sonra");
  await delay(1000);
  console.log("1 saniye daha sonra");
}

delayedLog();

// requestAnimationFrame (browser'da animasyonlar için)
function animate() {
  // Animation logic here
  console.log("Animation frame");
  requestAnimationFrame(animate);
}

// animate(); // Bu satırı uncomment ederseniz sürekli çalışır
```

### String Metodları

**Temel String İşlemleri:**

```javascript
let text = "  JavaScript Programming  ";

// Uzunluk
console.log(text.length); // 25

// Boşlukları temizle
console.log(text.trim());     // "JavaScript Programming"
console.log(text.trimStart()); // "JavaScript Programming  "
console.log(text.trimEnd());   // "  JavaScript Programming"

// Büyük/küçük harf
console.log(text.toLowerCase());  // "  javascript programming  "
console.log(text.toUpperCase());  // "  JAVASCRIPT PROGRAMMING  "

// Arama
console.log(text.indexOf('Script'));     // 6
console.log(text.lastIndexOf('a'));      // 18
console.log(text.includes('Java'));      // true
console.log(text.startsWith('  Java'));  // true
console.log(text.endsWith('ing  '));     // true

// Kesme ve ayırma
console.log(text.slice(2, 12));          // "JavaScript"
console.log(text.substring(2, 12));      // "JavaScript"
console.log(text.substr(2, 10));         // "JavaScript" (deprecated)

let words = text.trim().split(' ');
console.log(words); // ["JavaScript", "Programming"]

// Değiştirme
console.log(text.replace('JavaScript', 'JS'));     // Ilk eşleşeni değiştir
console.log(text.replaceAll('a', '@'));           // Tüm eşleşenleri değiştir

// Tekrar etme
console.log('Ha'.repeat(3)); // "HaHaHa"

// Padding
console.log('5'.padStart(3, '0'));  // "005"
console.log('5'.padEnd(3, '0'));    // "500"
```

### Number Metodları

**Number İşlemleri:**

```javascript
let num = 3.14159;
let str = "123.45px";

// Parsing
console.log(parseInt(str));       // 123
console.log(parseFloat(str));     // 123.45
console.log(Number(str));         // NaN (çünkü px var)
console.log(Number("123.45"));    // 123.45

// Formatting
console.log(num.toFixed(2));      // "3.14"
console.log(num.toPrecision(4));  // "3.142"
console.log(num.toExponential(2)); // "3.14e+0"

// Checking
console.log(Number.isInteger(5));     // true
console.log(Number.isInteger(5.0));   // true
console.log(Number.isInteger(5.1));   // false
console.log(Number.isNaN(NaN));       // true
console.log(Number.isFinite(100));    // true
console.log(Number.isFinite(Infinity)); // false

// Constants
console.log(Number.MAX_VALUE);        // En büyük sayı
console.log(Number.MIN_VALUE);        // En küçük pozitif sayı
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(Number.POSITIVE_INFINITY); // Infinity
```

### Math Nesnesi

**Math Metodları:**

```javascript
// Temel işlemler
console.log(Math.abs(-5));        // 5
console.log(Math.ceil(4.2));      // 5
console.log(Math.floor(4.8));     // 4
console.log(Math.round(4.6));     // 5
console.log(Math.trunc(4.9));     // 4

// Min/Max
console.log(Math.min(1, 2, 3));   // 1
console.log(Math.max(1, 2, 3));   // 3

// Power ve kök
console.log(Math.pow(2, 3));      // 8
console.log(Math.sqrt(16));       // 4
console.log(Math.cbrt(27));       // 3

// Rastgele sayı
console.log(Math.random());       // 0-1 arası
console.log(Math.floor(Math.random() * 10)); // 0-9 arası

// Trigonometrik
console.log(Math.sin(Math.PI / 2)); // 1
console.log(Math.cos(0));           // 1
console.log(Math.tan(Math.PI / 4)); // 1

// Logaritma
console.log(Math.log(Math.E));      // 1
console.log(Math.log10(100));       // 2

// Constants
console.log(Math.PI);    // 3.141592653589793
console.log(Math.E);     // 2.718281828459045
```

### Date Nesnesi

**Date İşlemleri:**

```javascript
// Date oluşturma
let now = new Date();
let specificDate = new Date('2024-01-15');
let dateWithTime = new Date(2024, 0, 15, 10, 30, 0); // Ay 0-11 arası

console.log(now);          // Current date and time
console.log(specificDate); // Mon Jan 15 2024...

// Get metodları
console.log(now.getFullYear());  // 2024
console.log(now.getMonth());     // 0-11 (Ocak = 0)
console.log(now.getDate());      // 1-31
console.log(now.getDay());       // 0-6 (Pazar = 0)
console.log(now.getHours());     // 0-23
console.log(now.getMinutes());   // 0-59
console.log(now.getSeconds());   // 0-59
console.log(now.getTime());      // Timestamp (milisaniye)

// Set metodları
let date = new Date();
date.setFullYear(2025);
date.setMonth(11); // Aralık
date.setDate(25);  // 25. gün
console.log(date); // Dec 25, 2025

// Formatting
console.log(date.toString());       // Tam format
console.log(date.toDateString());   // Sadece tarih
console.log(date.toTimeString());   // Sadece saat
console.log(date.toISOString());    // ISO format
console.log(date.toLocaleDateString('tr-TR')); // Türkçe format

// Date aritmetiği
let tomorrow = new Date();
tomorrow.setDate(tomorrow.getDate() + 1);

let oneWeekAgo = new Date();
oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);

// İki tarih arasındaki fark (milisaniye)
let diff = tomorrow.getTime() - now.getTime();
let daysDifference = diff / (1000 * 60 * 60 * 24);
console.log(`Fark: ${daysDifference} gün`);
```

### Console Metodları

**Temel Console Metodları:**

```javascript
// Temel loglar
console.log("Normal log mesajı");
console.info("Bilgi mesajı");
console.warn("Uyarı mesajı");
console.error("Hata mesajı");

// Farklı veri türleri
console.log("String:", "Hello World");
console.log("Number:", 42);
console.log("Boolean:", true);
console.log("Object:", {name: "John", age: 30});
console.log("Array:", [1, 2, 3, 4]);

// Multiple değerler
console.log("Name:", "John", "Age:", 30);

// Template strings ile
let name = "John";
console.log(`User: ${name}`);
```

**Gelişmiş Console Metodları:**

```javascript
// Tablo formatında görüntüleme
let users = [
  {name: "John", age: 30, city: "New York"},
  {name: "Jane", age: 25, city: "Los Angeles"},
  {name: "Bob", age: 35, city: "Chicago"}
];
console.table(users);

// Gruplayarak loglar
console.group("User Details");
console.log("Name: John");
console.log("Age: 30");
console.log("City: New York");
console.groupEnd();

// Collapsed group
console.groupCollapsed("Debug Info");
console.log("Internal data 1");
console.log("Internal data 2");
console.groupEnd();

// Time measurement
console.time("Operation Timer");
// Uzun süren işlem
for (let i = 0; i < 1000000; i++) {
  // Some operation
}
console.timeEnd("Operation Timer"); // Operation Timer: 2.345ms

// Count tracking
console.count("Button clicks");
console.count("Button clicks");
console.count("Button clicks");
console.countReset("Button clicks");

// Assert - condition false ise log yapar
let x = 10;
console.assert(x > 5, "x should be greater than 5");
console.assert(x > 15, "x should be greater than 15", {x: x});

// Clear console
// console.clear();

// Trace - call stack gösterir
function functionA() {
  functionB();
}

function functionB() {
  console.trace("Call stack trace");
}

functionA();

// Custom CSS styling (browser only)
console.log("%cStyled message", "color: blue; font-size: 20px; font-weight: bold;");
console.log("%cRed %cGreen %cBlue", 
  "color: red;", 
  "color: green;", 
  "color: blue;"
);
```

### Performance API

**Performance Measurement:**

```javascript
// Performance timing
let startTime = performance.now();
// Your code here
let endTime = performance.now();
console.log(`Operation took ${endTime - startTime} milliseconds`);

// Mark and measure
performance.mark('start-operation');
// Your code here
performance.mark('end-operation');
performance.measure('operation-duration', 'start-operation', 'end-operation');

let measures = performance.getEntriesByType('measure');
console.log(measures[0].duration);

// Navigation timing
console.log('Page load time:', performance.timing.loadEventEnd - performance.timing.navigationStart);
console.log('DOM ready time:', performance.timing.domContentLoadedEventEnd - performance.timing.navigationStart);

// Memory usage (Chrome)
if (performance.memory) {
  console.log('Memory usage:', {
    used: performance.memory.usedJSHeapSize,
    total: performance.memory.totalJSHeapSize,
    limit: performance.memory.jsHeapSizeLimit
  });
}

// Performance observer
if ('PerformanceObserver' in window) {
  const observer = new PerformanceObserver((list) => {
    for (const entry of list.getEntries()) {
      console.log(`${entry.name}: ${entry.duration}ms`);
    }
  });
  
  observer.observe({entryTypes: ['measure']});
}
```

### Local Storage ve Session Storage

**Local Storage:**

```javascript
// Veri kaydetme
localStorage.setItem('username', 'John');
localStorage.setItem('preferences', JSON.stringify({
  theme: 'dark',
  language: 'tr'
}));

// Veri okuma
let username = localStorage.getItem('username');
console.log(username); // "John"

let preferences = JSON.parse(localStorage.getItem('preferences'));
console.log(preferences.theme); // "dark"

// Veri silme
localStorage.removeItem('username');

// Tüm veriyi temizleme
localStorage.clear();

// Storage eventini dinleme (diğer tab'lerden değişiklikleri yakala)
window.addEventListener('storage', function(event) {
  console.log('Storage değişti:', event.key, event.newValue);
});

// Mevcut tüm key'leri görme
for (let i = 0; i < localStorage.length; i++) {
  let key = localStorage.key(i);
  let value = localStorage.getItem(key);
  console.log(key, value);
}
```

**Session Storage:**

```javascript
// Session Storage (tab kapanınca silinir)
sessionStorage.setItem('sessionData', 'Bu tab kapanınca silinir');
let sessionData = sessionStorage.getItem('sessionData');

// Storage kapasitesi kontrolü
function getStorageSize() {
  let total = 0;
  for (let key in localStorage) {
    if (localStorage.hasOwnProperty(key)) {
      total += localStorage[key].length;
    }
  }
  return total;
}

console.log('Storage boyutu:', getStorageSize(), 'karakter');
```

**Storage Utility Functions:**

```javascript
// Storage wrapper ile error handling
const storage = {
  set: function(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch (error) {
      console.error('Storage set error:', error);
      return false;
    }
  },
  
  get: function(key, defaultValue = null) {
    try {
      let item = localStorage.getItem(key);
      return item ? JSON.parse(item) : defaultValue;
    } catch (error) {
      console.error('Storage get error:', error);
      return defaultValue;
    }
  },
  
  remove: function(key) {
    try {
      localStorage.removeItem(key);
      return true;
    } catch (error) {
      console.error('Storage remove error:', error);
      return false;
    }
  }
};

// Kullanım
storage.set('user', {name: 'John', age: 30});
let user = storage.get('user', {});
console.log(user.name); // "John"
```

### Fetch API

**Temel Fetch Kullanımı:**

```javascript
// GET request
fetch('https://jsonplaceholder.typicode.com/users')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Fetch error:', error));

// Async/await ile
async function fetchUsers() {
  try {
    let response = await fetch('https://jsonplaceholder.typicode.com/users');
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}
```

**HTTP Methodları ile Fetch:**

```javascript
// POST request
async function createPost() {
  try {
    let response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer your-token-here'
      },
      body: JSON.stringify({
        title: 'Yeni Post',
        body: 'Post içeriği',
        userId: 1
      })
    });
    
    let data = await response.json();
    console.log('Created:', data);
  } catch (error) {
    console.error('POST error:', error);
  }
}

// PUT request
async function updatePost(id) {
  let response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      id: id,
      title: 'Güncellenmiş başlık',
      body: 'Güncellenmiş içerik',
      userId: 1
    })
  });
  
  let data = await response.json();
  console.log('Updated:', data);
}

// DELETE request
async function deletePost(id) {
  let response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: 'DELETE'
  });
  
  if (response.ok) {
    console.log('Post silindi');
  }
}
```

**Gelişmiş Fetch Özellikleri:**

```javascript
// Response handling
async function handleResponse() {
  try {
    let response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
    
    // Status kontrolü
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    // Response headers
    console.log('Content-Type:', response.headers.get('content-type'));
    
    // Response types
    let textData = await response.text(); // String olarak
    // let jsonData = await response.json(); // JSON olarak
    // let blobData = await response.blob(); // Blob olarak
    // let arrayBuffer = await response.arrayBuffer(); // ArrayBuffer olarak
    
    console.log(textData);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

// Request timeout
async function fetchWithTimeout(url, options = {}, timeout = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);
  
  try {
    const response = await fetch(url, {
      ...options,
      signal: controller.signal
    });
    clearTimeout(timeoutId);
    return response;
  } catch (error) {
    clearTimeout(timeoutId);
    if (error.name === 'AbortError') {
      throw new Error('Request timeout');
    }
    throw error;
  }
}

// File upload
async function uploadFile(file) {
  let formData = new FormData();
  formData.append('file', file);
  formData.append('description', 'File açıklaması');
  
  try {
    let response = await fetch('/upload', {
      method: 'POST',
      body: formData // headers otomatik set edilir
    });
    
    let result = await response.json();
    console.log('Upload successful:', result);
  } catch (error) {
    console.error('Upload error:', error);
  }
}
```

## İleri Seviye JavaScript

**Polyfill Kavramı:**

```javascript
// Array.includes polyfill (eski tarayıcılar için)
if (!Array.prototype.includes) {
  Array.prototype.includes = function(searchElement, fromIndex) {
    'use strict';
    if (this == null) {
      throw new TypeError('Array.prototype.includes called on null or undefined');
    }
    
    let O = Object(this);
    let len = parseInt(O.length) || 0;
    if (len === 0) {
      return false;
    }
    
    let n = parseInt(fromIndex) || 0;
    let k;
    if (n >= 0) {
      k = n;
    } else {
      k = len + n;
      if (k < 0) {
        k = 0;
      }
    }
    
    while (k < len) {
      if (searchElement === O[k]) {
        return true;
      }
      k++;
    }
    
    return false;
  };
}

// Promise polyfill basit örnek
if (typeof Promise === 'undefined') {
  window.Promise = function(executor) {
    // Basitleştirilmiş Promise implementasyonu
    // Gerçek polyfill çok daha karmaşıktır
  };
}

// Object.assign polyfill
if (typeof Object.assign !== 'function') {
  Object.assign = function(target, ...sources) {
    if (target == null) {
      throw new TypeError('Cannot convert undefined or null to object');
    }
    
    let to = Object(target);
    
    for (let source of sources) {
      if (source != null) {
        for (let key in source) {
          if (Object.prototype.hasOwnProperty.call(source, key)) {
            to[key] = source[key];
          }
        }
      }
    }
    
    return to;
  };
}
```

**Modern JavaScript Development:**

```javascript
// Transpiling örneği (Babel ile ES6+ → ES5)

// ES6+ kod:
const greet = (name = 'World') => `Hello ${name}!`;
class User {
  constructor(name) {
    this.name = name;
  }
  
  async getData() {
    const response = await fetch('/api/user');
    return response.json();
  }
}

// Transpiled ES5 kod:
var greet = function greet(name) {
  if (name === undefined) name = 'World';
  return 'Hello ' + name + '!';
};

var User = function() {
  function User(name) {
    this.name = name;
  }
  
  User.prototype.getData = function getData() {
    var response = fetch('/api/user');
    return response.then(function(res) {
      return res.json();
    });
  };
  
  return User;
}();

// Feature detection
function supportsES6() {
  try {
    new Function('const arrow = () => {};');
    return true;
  } catch (e) {
    return false;
  }
}

if (supportsES6()) {
  console.log('ES6 destekleniyor');
} else {
  console.log('ES6 desteklenmiyor, polyfill gerekli');
}

// Progressive enhancement
function enhanceIfSupported() {
  if ('serviceWorker' in navigator) {
    // Service Worker kullan
    navigator.serviceWorker.register('/sw.js');
  }
  
  if ('geolocation' in navigator) {
    // Geolocation kullan
    navigator.geolocation.getCurrentPosition(successCallback);
  }
  
  if (window.IntersectionObserver) {
    // Intersection Observer kullan
    const observer = new IntersectionObserver(entries => {
      // Handle intersections
    });
  }
}
```

---

Bu cheatsheet, JavaScript'in temel ve ileri seviye özelliklerini kapsamlı bir şekilde içerir. Hızlı başvuru için tasarlanmış ve her konuda pratik örnekler sunmaktadır. Modern JavaScript geliştirme için gerekli tüm bilgileri içerir ve sürekli güncellenen dil özelliklerini takip eder.

**Kapsanan Ana Konular:**
- ✅ Temel kavramlar (değişkenler, veri türleri, operatörler, hoisting, hata yönetimi)
- ✅ Kontrol akışı (koşullar, döngüler)  
- ✅ Fonksiyonlar (callback, closure, this, arrow functions, generators)
- ✅ Nesneler ve diziler (metodlar, destructuring, spread/rest)
- ✅ Asenkron JavaScript (promises, async/await)
- ✅ Nesne yönelimli programlama (classes, prototype, inheritance)
- ✅ ES6+ özellikleri (template literals, modules, default parameters)
- ✅ DOM manipülasyonu (element seçme, event handling)
- ✅ Modern JavaScript (Set/Map, WeakSet/WeakMap, Proxy/Reflect, Iterators)
- ✅ Browser API'ları (Local Storage, Fetch API, Console, Performance)
- ✅ Geliştirme araçları (Polyfills, Transpiling, Feature Detection)

Bu kaynak modern JavaScript geliştiricisi için eksiksiz bir başvuru kaynağıdır.