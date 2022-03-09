# Factory Functions and Module Pattern.

 Note that one of the key differences between a Factory Function and a Constructor one, is the use of the word "new". While a Factory Function doesn't need this to return an object, a Constructor does. We cannot create an instance using a Constructor as a base without using the "new" keyword. Also, with a Factory Function we don't have to use "this".

 Also, a factory function may perfor slower than a constructor function in micro-optimization benchmarks.


## Constructor Function syntax:

```
function humanProto(){
};

humanProto.prototype.specie = function(){
	return "Homo Sapiens";
};

humanProto.prototype.sayHi = function(){
	return `hi, my name is ${this.firstName}, and i'm from ${this.country}... i'm a human ${this.genre}, and i'm glad to meet you...!`
};

function personConstructor(fullname, country, age, genre){
	this.firstName = fullname.split(' ')[0];
	this.secondName = fullname.split(' ')[1];
	this.country = country;
	this.genre = genre;
}

/*setting the prototype of our personConstructor() to humanProto()*/

personConstructor.prototype = Object.create(humanProto.prototype);

const johnDoe = new personConstructor("John Doe", "unknown", 32, "male");
```

## Factory Function syntax:
### Using a function factory as a prototype:

```
function humanProto(){
	function specie(){
		return "Homo Sapiens";
	}
	
	function sayHi(){
		return `hi, my name is ${this.firstName}, and i'm from ${this.country}... i'm a human ${this.genre}, and i'm glad to meet you...!`
	}
	
	return {specie, sayHi};
};

function personFactory(fullName, country, age, genre){
	const [ firstName, lastName ] = [ fullName.split(' ')[0], fullName.split(' ')[1] ];
	
	return Object.assign(Object.create(humanProto()),{firstName}, {lastName}, {age}, {genre});
};

const johnDoe = personFactory("John Doe", "Unknown", 32, "male");
```

### Using an object literal as a prototype:

```
const humanProto = {
	specie(){
		return "Homo Sapiens"
	},
	
	sayHi(){
		return `hi, my name is ${this.firstName}, and i'm from ${this.country}... i'm a human ${this.genre}, and i'm glad to meet you...!`
	},
}

function personFactory(fullName, country, age, genre){
	const [ firstName, lastName ] = [ fullName.split(' ')[0], fullName.split(' ')[1] ];
	
	return Object.assign(Object.create(humanProto), {firstName}, {lastName}, {age}, {genre});
};

const johnDoe = personFactory("John Doe", "Unknown", 32, "male");
```

## Q&A

### 1. Describe common bugs you might run into using constructors:

 One of the common bugs used to be forgetting the "**new**" keyword when invoking the constructor and in that way we would be polluting the global namespace, along with bugs that were hard to trace. But now that is not possible, if we try to replicate that behavior, it will just throw "undefined" at us when we try to call the object "made" by the constructor, meaning that without the "new" keywords, our object will not be created... 
 
 Another of the bugs is that the **constructor property** of an object is set by the engine exactly once. When we define a --constructor-- function, and then use it to define a new object, the **constructor property** of our recent create object will point to the --constructor-- function that created it. But, if we change the **prototype property** of our --constructor-- function to some other object, without explicitly setting our object **constructor property** then al the objects created by the function will have their **constructor property** set to ***Object** --which is default--.
 
#### 1.1. Without setting the prototype of our constructor function.
 
 
 ```
 function DummyPerson(name, age){
 	this.name = name;
 	this.age = age;
 };
 
 const a Doe = new DummyPerson("John Doe", 32);
 console.log(aDoe.constructor); // [Function: DummyPerson]
 ```
 
 ---
#### 1.2. Setting the prototype of our constructor function.

```
function DummyPerson(name, age){
	this.name = name;
	this.age = age;
};

const testProto = {
	sayHi(){
		return "hi";
	},
};

DummyPerson.prototype = testProto;

const aDoe = new DummyPerson("John Doe", 32);
console.log(aDoe.constructor); // [Function: Object]
```

---

### 2. Write a factory method that returns an object.

```
function newRightAngleTriangle(height, base){
	function hypotenuse(){
		return Math.sqrt( (Math.pow(height, 2) + Math.pow(base, 2)) );
	}
	
	return {height, base, hypotenuse}
}

const triangleOne = newRightAngleTriangle(2, 3);
triangleOne.hypotenuse() // 3.60555127...
```

---

### 3. Explain how scope works in JS, and what ES6 changed.

 Scope is basically the context in which an Object is created (can be a variable, function...), This context make this object available to others objects depending if they are in the same scope (in the same context, like an if statement)
or, if they are being called by an object with an "insider" scope within this context. Like for example:

```
function outsideScope(){
	const husbandName = "John";
	console.log(husbandName); //logs: "John"
	
	function insiderScope(){
		const wifeName = "Jane";
		let message = `${husbandName} is married with ${wifeName}.`;
		console.log(message) // John is married with Jane.
	}
	
	console.log(wifeName) // ReferenceError: wifeName is not defined
}
```
 Two of the features that ES6 brought were **const* and **let**. **const** allows us to create variables whose values cannot be changed once we have declared them. While "let" allows us to create variables whose values can be changed after we have declared them. 

---

### 4. Explain what Closure is and how it impacts private functions and variables.

 A **closure** is the combination of a function enclosed with references to its surrounding state, meaning, to the lexical enviroment. A closure give us access to an outer function's scope from an inner function. For example: 
 
 ```
 function favoriteBand(name){
 	let __message = `My favorite band is: ${name}`;
 	
 	function __insertDate(){
 		const day = new Date().getDate();
 		__message += ` ... Modified the day: ${day}`;
 	};
 	
 	function modifyMessage(text){
 		__message = `${text} ${name}`
 	}
 	
 	
 	function logMessage(){
 		__insertDate();
 		console.log(__message);
 	}
 	
 	console.log(day); // ReferenceError: day is not defined;
 	return {modifyMessage, logMessage}
 }
 ```
 
 in the example above we can see how each inner function can access an outer variable, but we cannot access from the outer scope one of the variables declared inside one of our inner functions, in this case. We cannot access the variable *day* from our private method *__insertDate()*.
 
---

### 5. Describe how private functions and variables are useful.

 They are a way in which we can maintain our data an methods out of the eye of the user, meaning that it cannot be change, at least, no without a method whose is in charge to do that. An example that just cross my mind right now, is if we need a function that return the product of two fibonacci numbers. To do that, we would need two functions.
 
 The first function has a **parameter n**, and its in charge of returning the fibonacci number corresponding to *n*. The second function is gonna be the one in charge of returning the product of two fibonacci numbers, so, this function will receive two parameter; **parameter a**, and **parameter b**, and as we just say, will return a * b;
 
 With the former example we can see that the first function, is an auxiliary function to our purpose, but that doesn't mean it is not important, on the contrary, without it, we could not calculate the product of two fibonacci numbers. Also, as a private function it cannot be altered by the user, while the methods that are in the same scope can access this auxiliary function.
 
 ---
 
### 6. Use inheritance in objects using the factory patter.

```
const humanProto = {
	specie(){
		return "Homo Sapiens";
	},
	
	return {specie()};
};

function newHuman(name, genre, age, country){
	return Object.assign(Object.create(humanProto), {name}, {genre}, {age}, {country});
};

const john = newHuman("John", "Male", 32, "Unknown");
john.specie() // Homo Sapiens
```

---

### 7. Explain the module pattern.

 The module pattern offers us a way in which we can organize our code and make it cleaner. One of the advantages that the module pattern presents is that it allow us to have private methods and variables along with public ones --that are the ones we are gonna return--. This also let us avoid problems with the namespace.
 
  The disadvantages of the Module pattern are that, if we want to change the visibility of a public method --or property-- we would need to change each place in which this method was used. We also cannot access private members that are added to the object after it was initialize it.
  
#### 7.1 Module Syntax:

 The concept is exactly the same as the *factory function*, but instead of creating a factory that we are gonna use to create multiple objects, the module pattern wraps the *function factory* in an IIFE.
  
  ```
const myModule = (function (){
  	//Private
  	let _counter = 0;
  	
  	const _countriesArr = [];
  	
    const _upperCase = (text) => {
  		return text.toUpperCase();
  	};
  	
  	//Public
    const addCountry = (country) => {
  		_countriesArr.push([_upperCase(country), _counter++]);
  	};
  	
    const showCountries = () => {
  		console.log(_countriesArr);
  	};
  	
    const resetCounter = () => {
  		_counter = 0;
  	};
  	
  	return {addCountry, showCountries, resetCounter};
  })()

myModule.addCountry("usa");
myModule.addCountry("argentina");
myModule.showCountries(); // [ [ 'USA', 0 ], [ 'ARGENTINA', 1 ] ]
myModule.resetCounter();
myModule.addCountry("chile");
myModule.showCountries(); // [ [ 'USA', 0 ], [ 'ARGENTINA', 1 ], [ 'CHILE', 0 ] ]
console.log(myModule._counter) // undefined

  ```

---

### 8. Describe IIFE. What does it stand for?.

 IIFE stands for **Inmmediately Invoked Function Expression**. In Javascript there are two ways to created a function: through **function declaration** or a **function expression**. 
 
 A **function declaration** is the normal way we used to create a named function.
 
 ```
 function sayHi(){
 	console.log("Hi");
 };
 ```
 
A **function expression** is when we are assigning a function to a variable or property.

```
const sayHi = function(){
	console.log("Hi");
};
```

 The thing is, that we can also convert a *function declaration* into a *function expression* without assigning it to a variable or property. We can do this wrapping our *function declaration* with parenthesis --grouping operator--.

```
(function sayHi(){
	console.log("Hi");
});
``` 

 An expression return a value. If we want to invoke this expression right away we just need to add a couple of parenthesis at the end of our expression.
 
 ```
 (function sayHi(){
 console.log("Hi");
 })(); // Hi
 ```

 Even though i named this *functions expressions* one of the use cases is that using IIFE we avoid polluting the global namespace. For example, if we need some code that will only be used once and we will never use it again, we can call it with an IIFE. Sure, we could do that with a named function, but appart from the fact that we would be polluting the global namespace, the function itself wouldn't be as self-documenting as an IIFE can be.

 Also, we could use IIFE to create private and public variables and methods. 

---

### 9. Briefly explain namespacing and how it's useful.

 Namespacing is a way in which we can avoid name collisions between multiple functions or objects with the same name. If we start our app defining all our variables and methods in the global scope soon we will reach a point in which our code not only wouldn't be as cleaner as it could be, but it will also affect the names that we use to define our objects. A namespace ensures that all our variables and methods have unique names so that they can be easily identified and cannot cause name collisions.
 
 A more simpler concepts is that namespace is a way to implement scope.
 
 We have to keep in mind that while other languages provide namespace by default, Javascript doens't. There are various patterns to implement namespacing in Javascript.
 
 1. Single global variables.
 2. Prefix namespacing.
 3. Object literal notation.
 4. Nested namespacing.
 5. Immediately-Invoke-Functions (IIFE).
 6. Expressions.
 7. Namespace injection.
 
#### 9.1. Single Global Variables.

 We can use a single global variable as our primary object of reference without polluting the global scope. The biggest challenge with this pattern is that we have to make sure that no one else has using the same global variable name as we have in the page.
 
 ```
 const myApp = (function(){
 	function sayHi(){
 		return "Hi";
 	};
 	
 	return {sayHi};
 })();
 
 myApp.sayHi() // Hi;
 ```
 
 The syntax is the same of the module pattern. The difference between this two patterns is that with this one we are going to put all our methods and properties inside one variable.
 
#### 9.2. Prefix Namespacing.

 It's a very simple concept. The idea is we select a unique prefix namespace that we are gonna use, and then define any methods, variables, or other objects after the prefix.
 
```
const myApp_propA = {};
const myApp_methodA = {};
```

 With this pattern two problems can occur: Once we reached a certain point in the development of our app we will have a large number of global objects. It can also occur that our prefix is not being use by any other developers.
 
 Keep in mind we can do this using an Object literal to organize our code and not pollute our app with a large number of global objects.
 
#### 9.3. Object Literal Notation.

 We can use an object to organize our code. This can be thought as an object containing a series of key-value pairs that represent new namespaces with its own methods and properties.
 
 ```
 const myApp = {
 	getUser: function(){
 		...
 	},
 	
 	newUser: function(){
 		...
 	},
 	
 	account: {
 		globalSettings: {
 			...
 		},
 	},
 	
 };
 ```
 
 We can also add properties directly to the namespace.
 
 ```
 myApp.logInfo = function(){
 	...
 };
 ```
 
  The advantage of Objects literals is that apart from assist in organizing our code and parameters in a logical way, we are not polluting the global namespace. We can also create easily readable structures that can be expanded to support deep nesting.
  
  It could occur that by mistake we redefine a variable, or cause a name collision. We can "implement" a test that take into account the existence of a variable by the same name, so the chances of a collision occurring are significantly reduce.
  
```
let myApp = myApp || {}; /*1*/
if(!myApp){ myApp = {} }; /*2*/
myApp = (myApp === undefined) ? {} : myApp; /*3*/
```
 
 
 
