# OOP Cheatsheet
- Created by: danny-dang
- Last edit: Oct 31 2021
- Language: vi

## OOP là gì?
Object Oriented Programing (OOP) - Lập trình hướng đối tượng - là kỹ thuật lập trình mà các đối tượng (object) tương tác với nhau 

## Ví dụ tại sao cần OOP

Giả sử muốn tạo 1 loại các objects để lưu các thông tin, tính năng của 1 phương tiện như sau (các object này có thể sử dụng với các mục đích khác nhau ở chỗ khác):
```javascript
const cb300r = {
    name: 'Honda CB300R',
    brand: 'Honda'
	displacement: 286,
	fuelGauge: 10,
	weight: 147,
	engineStart: () => {
        console.log('Honda CB300R starts brr...')
    },
    brake: () => {
        console.log('Honda CB300R brakes')
    }
}
const mt03 = {
    name: 'Yamaha MT-03',
    brand: 'Yamaha'
	displacement: 321,
	fuelGauge: 14,
	weight: 168,
	engineStart: () => {
        console.log('Yamaha MT-03 starts brr...')
    },
    brake: () => {
        console.log('Yamaha MT-03 brakes')
    }
}
const z300 = {
    name: 'Kawasaki Z300',
    brand: 'Kawasaki'
	displacement: 296,
	fuelGauge: 17,
	weight: 168,
	engineStart: () => {
        console.log('Kawasaki Z300 starts brr...')
    },
    brake: () => {
        console.log('Kawasaki Z300  brakes')
    }
}
```
Nhận thấy, ta phải tạo các object mình muốn một cách lặp đi lặp lại, và các object này, đều có các properties giống nhau. Việc define các object như thế rất thừa thãi và mất thời gian. 

Với việc sử dụng OOP, vấn đề trên đc tối ưu hơn như sau:
```javascript
// Đây được gọi là constructor function
const Motor = (name, brand, displacement, fuelGauge, weight) => { 
    this.name = name
    this.brand = brand
    this.displacement = displacement
    this.fuelGauge = fuelGauge
    this.weight = weight

	this.engineStart = () => {
        console.log(this.name + ' start brr...')
    } 
    this.brake = () => {
        console.log(this.name + ' brakes')
    } 
}

const cb300r = new Motor('Honda CB300R', 'Honda', 296, 10, 148)
const mt03 = new Motor('Yamaha MT03', 'Yamaha', 296, 14, 168)
const z300 = new Motor('Kawasaki Z300', 'Kawasaki', 296, 17, 168)
```

## Constructor function

Trong OOP, constructor function giống như là một bản "blueprint" để ta có thể xây dựng các object khác. 

Ví dụ: 
```javascript
function Person(first, last, age, eyeColor) {  
    this.firstName = first;  
    this.lastName = last;  
    this.age = age;  
    this.eyeColor = eyeColor;  
}
```
Naming convention đối với constructor function là PascalCase, viết hoa chữ cái đầu tiên và viết hoa đối với các từ tiếp theo

Chú ý: các arguments truyền vào function này không nhất thiết phải giống với tên của các property. Miễn là thứ tự các parameters mình truyền vào lúc khởi tạo object đúng thứ tự mình mong muốn. Việc đặt tên arguments giống với properties chỉ là 1 convention đc sử dụng rộng rãi
```javascript
function Person(arg1, arg2, arg3) {  
    this.firstName = arg1;  
    this.lastName = arg2;  
    this.age = arg3;   
}

const myFather = new Person('John', 'Doe', 49)
console.log(myFater.age) //-> 49
```

**Prototype**

> Prototype là cơ chế mà các object trong javascript kế thừa các tính năng từ một object khác. Tất cả các object trong javascript đều có một prototype, và các object này kế thừa các thuộc tính (properties) cũng như phương thức (methods) từ prototype của mình

Có thể sử dụng prototype để dịnh nghĩa các methods trong 1 constructor function sẽ giúp tiết kiệm memory.
VD:
```javascript
function Person(name) {
     this.name = name;
     this.sayHi = function(){
          return “Hi” + this.name;
     }
}
dang = new Person(“John”);
dang.sayHi(); // Hi John
// -> mỗi lần tạo object mới sử dụng từ khóa "new" sẽ define 1 method sayHi mới, do đó sẽ tốn nhiều bộ nhớ hơn
```
```javascript
function Person(name){
     this.name = name:
}
Person.prototype.sayHi = function(){
     return “Hi” + this.name;
}
dang = new Person(“Dang”);
dang.sayHi(); // Hi Dang

// ->  mỗi lần tạo new object với keyword new sẽ không define 1 method sayHi mới, nhưng các object này có thể reference đến method sayHi của Person, nhờ vậy đỡ tốn memory
```

## 4 tính chất của OOP

### Tính đóng gói (Encapsulation)
Đôi khi ta không muốn các object được tạo từ constructor function có thể dễ dàng lấy hoặc thay đổi giá trị của các property. Hoặc là đôi khi ta muốn ẩn các thông tin đó.

```javascript
function Employee(name, age, username, password) {
    // Đây là các public property, có thể dễ dàng retrieve, modify từ các object đc khởi tạo  
    this.name = name;  
    this.age = age;
    // đây là các private property, chỉ đc gán giá trị ban đầu khi khởi tạo object. Các objects đc khởi tạo không thể tự ý thay đổi giá trị này
    let username = username;
    let password = password;
    let lastDateRequestForMessage = null;
    let secretMessage = 'This message is secret';

	this.login = (user, pass) => {
        if(username === user && password === pass) {
            return 'Login successfully'
        } else {
            return 'Incorrect username or password'
        }
    }
    
    this.changePassword = (oldPass, newPass) => {
        if(oldPass === password) {
            password = newPass
            return 'Changed password successfully'
        } else {
            return 'Incorrect old password'
        }
    }
    
    this.getSecretMessage = () => {
        let today = new Date()
        lastDateRequestForMessage = today
        return secretMessage
    }
    
    this.checkLastSecretRequest = () => {
        return lastDateRequestForMessage 
    }
    
}

const employee_1 = new Employee('John', 49, 'johndoe', '123456abc')

//Có thể retrieve các public properties
console.log(employee_1.name) // --> 'John'
console.log(employee_1.age) // --> 49

//Có thể modify các public properties dễ dàng
employee_1.name = 'John Doe'

//Không thể tự ý retrieve các private properties
console.log(employee_1.username) // --> undefined
console.log(employee_1.age) // --> undefined
console.log(employee_1.secretMessage) // --> undefined

//Được sử dụng các methods đã định nghĩa để lấy các giá trị của private properties
console.log(employee_1.getSecretMessage()) 
// --> 'This message is secret'
console.log(employee_1.checkLastSecretRequest())
// --> 'Sun Oct 31 2021 21:27:04 GMT+0700'

//Được sử dụng các methods đã định nghĩa để thay đổi các private properties
employee_1.changePassword('123456abc', 'newpass')
```

### Tính kế thừa (Inheritance)
Đôi khi, ta muốn 1 đối tượng kế thừa các properties hoặc methods từ 1 đối tượng khác để nhằm mục đích hạn chế việc viết đi viết lại code khi viết các Constructor functions cho các đối tượng khởi tạo.

```javascript
function Animal(name, numLegs, numEyes) {
    this.name = name 
    this.numLegs = numLegs
    this.numEyes = numEyes

	this.walk = () => {
        return this.name + ' walks....'
    }
}

//object Dog kế thừa Animal
function Dog(name, numLegs, numEyes, numTail) {
    Animal.call(this, name, numLegs, numEyes);
    this.numTail = numTail

    this.bark = () => {
        return this.name + ' sủa gâu gâu'
    }
}

let shiba = new Dog('Shiba', 4, 2, 1)
shiba.name // --> 'Shiba'
shiba.numTail //-> 1
shiba.walk() // 'Shiba walks...'
shiba.bark () // 'Shiba sủa gâu gâu'
```
Việc sử dụng từ khóa `.call(this, ...args)` bên trong object `Dog` sẽ khởi tạo object `Animal` bên trong `Dog` cùng với các argument đc truyền vào. Nhờ đó, object `Dog` cũng sẽ có các properties từ object `Animal` cùng với các methods trong `Animal`. Hay nói cách khác, `Dog` kế thừa `Animal`.

Hiểu thêm về từ khóa `.call()`: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call?retiredLocale=vi


### Tính đa hình (Polymorphism)

Khi các đối tượng kế thừa lẫn nhau, đôi khi có những trường hợp các đổi tượng có các methods tên giống nhau. Vì vậy tính đa hình sẽ quyết định methods nào được sử dụng trong trường hợp này. Tính đa hình có 2 loại:
 - Nạp chồng (overloading): Khi 2 methods ở đối tượng cha và con giống tên nhau, nhưng mà số lượng arguments lại khác nhau. Tùy thuộc vào số lượng parameters được sử dụng để truyền vào lúc gọi method, sẽ quyết định method nào đc sử dụng.
 - Ghi đè (overridding): Khi 2 methods ở đối tượng cha và con giống tên nhau, bất kể số lượng arguments. Method ở đối tượng con sẽ đc sử dụng. 

Lưu ý: Trong Javascript, chỉ có thuộc tính ghi đè (overrdding). Tức là method ở đối tượng con luôn luôn ghi đè method ở đối tượng cha
```javascript
function Animal(name, numLegs, numEyes) {
    this.name = name 
    this.numLegs = numLegs
    this.numEyes = numEyes

	this.walk = () => {
	    console.log('called in Animal')
        return this.name + ' walks....'
    }
}

function Dog(name, numLegs, numEyes, numTail) {
    Animal.call(this, name, numLegs, numEyes);
    this.numTail = numTail

    this.walk = () => {
        console.log('called in Dog')
        return this.name + ' walks using 4 legs'
    }
}

let shiba = new Dog('Shiba', 4, 2, 1)
shiba.walk () 
// 'Called in Dog'
// 'Shiba walks using 4 legs'
```
### Tính trừu tượng (Abstraction)
Là bản nâng cấp của tính đóng gói (Encapsulation). Tính trừu tượng dùng để ẩn những chi tiết không cần thiết của ứng dụng khỏi người dùng, Từ đó, ta có thể để giảm độ phức tạp và tăng hiệu quả sử dụng của phần mềm. 

Tuy nhiên, trong Javascript chưa hỗ trợ tính năng này

## Class

Class được giới thiệu từ phiên bản Javascript ES6 (hay còn gọi là ES2015) vào năm 2015. Class là tính năng sinh ra đễ hỗ trợ việc viết OOP một cách rõ ràng và dễ dàng hơn.

### Constructor
Định nghĩa 1 class sử dụng từ khóa `class`:
```javascript
class Person {
     constructor(firstName, lastName, favoriteNum){
          this.firstName = firstName;
          this.lastName = lastName;
          this.favoriteNum = favoriteNum;
     }
     multiplyFavoriteNum(num){  
          return num * this.favoriteNum;
     }
};
let newPerson = new Person("John","Doe",7);
newPerson.multiplyFavoriteNum(10) ;
```

Cách viết tương đương trong ES5:

```javascript
function Person(firstName, lastName, favoriteNum) { //this is a constructor function
    this.firstName = firstName;
    this.lastName = lastName;
    this.favoriteNum = favoriteNum;
}
Person.prototype.multiplyFavoriteNum = function(num){
    return num*this.favoriteNum;
}
var newPerson = new Person("John","Doe",7);
newPerson.multiplyFavoriteNum(10) ;
```

Ta có thể viết bất cứ thứ gì ở trong constructor function. Tức là, constructor function sẽ không bị giới hạn bằng việc chỉ để sử dụng  để define các properties.
```javascript
class Person {
     constructor(firstName, lastName, favoriteNum){
          this.firstName = firstName;
          this.lastName = lastName;
          this.favoriteNum = favoriteNum;
          const someFunction = () => {
               console.log('someFunction called')
               return 'I can do anything here'
          }
          this.message = someFunction()
     }
     multiplyFavoriteNum(num){  
          return num * this.favoriteNum;
     }
};
let newPerson = new Person("John","Doe",7);
newPerson.multiplyFavoriteNum(10) ;
```

### Static function
`Static` là function chỉ có thể đc gọi bơi `class` object. Các object đc tạo từ `class` sẽ không thể gọi  `static` function:

```javascript
class Person {
     constructor(firstName,lastName,favoriteNumber){
          this.firstName = firstName;
          this.lastName = lastName;
          this.favoriteNum = favoriteNum;
     }
     multiplyFavoriteNum(num){   
          return num * this.favoriteNum;
     }
     static callStatic(){
          return "static has been called"
     }
};

var newPerson = new Person("John","Doe",7);
newPerson.callStatic();
    //-->ERROR
Person.callStatic() ;
    //-->"static has bene called"
```

### Inheritance
A class can inherit another class using `extends` key word and `super` keyword:

```javascript
class Animal {
    constructor(name, numLegs, numEyes) {
        this.name = name 
        this.numLegs = numLegs
        this.numEyes = numEyes
    }

	walk() {
        return this.name + ' walks....'
    }
}

//class Dog kế thừa Animal
class Dog extends Animal {
    constructor(name, numLegs, numEyes, numTail) {
        super(name, numLegs, numEyes)
        this.numTail = 1
    }
    
    bark() {
        return this.name + ' sủa gâu gâu'
    }
}
const shiba = new Dog('Shiba', 4, 2, 1)
shiba.bark() // -> 'Shiba sủa gâu gâu'
```
