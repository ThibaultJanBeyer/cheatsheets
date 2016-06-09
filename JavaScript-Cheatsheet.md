[back to overwiev](/../..)

#JavaScript Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Arrays & Objects](#arrays--objects)    
[Functions](#functions)  
[Recursive Functions](#recursive-functions)  
[Methods](#methods)  
[Predefined Functions/Methods](#predefined-functionsmethods)  
- [On Strings](#on-strings)  
- [On Arrays](#on-arrays)  
[Custom Constructors / Classes](#custom-constructors--classes)  
[Random](#random)  
[Loops](#loops)  
[Logical Operators](#logical-operators)
- [Conditional Operator](#conditional-operator)  
[User Imput](#user-imput)  
[Const, Let](#const-let)  
[JSON](#json)  

##Basics
**&&** –– and  **||** –– or  **!** –– not  
1 = true  
0 = false  
**increments:**  
i++ , i-- , i += x , i -= x  
**isNaN('berry');** –– true –– returns true when not a number  
**isNaN(42);**  –– false   
x**.length** –– Returns the length of a string or the amount of items in a variable
**typeof** variable –– returns object, number, string  
**Z < a** –– true, uppercase < lowercase  
```javascript
// In browsers, the global scope object is stored in the window variable:
var myVar = 10; // global variable
console.log("myVar" in window); // true
console.log(window.myVar); // 10
```

##Arrays & Objects:
```javascript
var newArray = []; // could do variable = [1,2,3] as well 
newArray.push('hello'); 
newArray[0]; // equals  ‘hello;
var newObject = {
  key: value,
  key: value
} // object literal notation
// Each piece of information we include in an object is known as a property. Each Property has a Value
var myObj = new Object(); // object constructor
myObj["key"] = "value"; // or myObj.key = "value";

delete newObject.key; // removes the key (very uncommon)
console.log("key" in anObject); // "in" returns a Boolean value that indicates whether that object has that property

myObj.hasOwnProperty('name') // returns true if myObj has a name property
```

##Functions:  
```javascript
var newFunction = function(argument,argument) { }; // or function newFunction(){} = function declarations = not part of the regular top-to-bottom flow = only use this form in the outermost block of a function or program. 
newFunction(x,y);
// below are 3 new things for functions in JavaScript 6 (experimental for now)
argument => argument + 1; // is equal to function(argument){ return argument + 1; };
function(x = 7, y = 42) { } // to set default values
function(x, y, ...a) { } // ...a will store all the  
var newFunction = function() { return "hi" };
newFunction // returns the function while newFunction() would return the functions outcome.
```

##Recursive Functions
```javascript
// Functions calling themselves (create a kindof finite loop)
function power(base, exponent) {
  if (exponent == 0)
    return 1;
  else
    return base * power(base, exponent - 1); // don’t forget to return your recursion else, you’ll get an undefined.
}

console.log(power(2, 3));
// → 8
/*
 * Whenever a function is called, a special variable named arguments is added to the environment in which the function body runs.
 */
function argumentCounter() {
  console.log("You gave me", arguments.length, "arguments.");
}
argumentCounter("Straw man", "Tautology", "Ad hominem"); // You gave me 3 arguments.
```

##Methods:  
```javascript
var bob = new Object();
bob.age = 17; // or: bob["age"] = 17; this way you can use variables as well: bob[variable];
bob.setAge = function (newAge){ bob.age = newAge; };
bob.setAge(5); // would set the Age Property of bob to 5 using the respective function
// here we define our method using "this", before we even introduce bob
var setAge = function (newAge) { this.age = newAge; };
var bob = { age: 25, setAge: setAge }
bob.setAge(50);
// writing an object constructor or object literal notation – both work the same
function someObject() {
  this.stuff = "bla"
  this.someMethod = function() { console.log(this.stuff) };
}
```

##Predefined Functions/Methods
###On Strings
```javascript
/*
 * toUpperCase, toLowerCase, trim, charAt
 */
"Doh".toUpperCase() // "DOH"
"Doh".toLowerCase() // "doh"
"  okay \n ".trim() // "okay"
"bla".charAt(0) // b –– identical to "bla"[0]
```
###On Arrays
```javascript
/*
 * Push, Pop, shift and unshift
 */
var mack = [];
mack.push("Mack");
mack.push("the", "Knife");
mack; // → ["Mack", "the", "Knife"]
mack.join(" "); // → Mack the Knife
mack.pop(); // → Knife
mack; // → ["Mack", "the"]

// push and pop add or remove elements at the end of an array
// shift and unshift add or remove elements at the start of an array

/*
 * indexOf, lastIndexOf
 */
[1, 2, 3, 2, 1].indexOf(2); // → 1
[1, 2, 3, 2, 1].lastIndexOf(2); // → 3

/*
 * slice & concat
 */
[0, 1, 2, 3, 4].slice(2, 4); // → [2, 3] (positions 2,4 – 2 included 4 excluded)
[0, 1, 2, 3, 4].slice(2); // → [2, 3, 4]
["a", "b"].concat("c", 1, "d") // → ["a", "b", "c", 1, "d"]

/*
 * filter
 */
function isBigEnough(value) { return value >= 10; }
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough); // → filtered is [12, 130, 44]

/*
 * map
 */
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt); // roots is now [1, 2, 3] (1*1=1, 2*2=4, 3*3=9), numbers is still [1, 4, 9]

/*
 * reduce
 */
var total = [0, 1, 2, 3].reduce(function(a, b) { return a + b; }); // total == 6

/*
 * forEach
 */
function logArrayElements(element, index, array) { console.log('a[' + index + '] = ' + element); }
[2, 5, , 9].forEach(logArrayElements); // → a[0] = 2 // → a[1] = 5 // → a[3] = 9
// Notice that index 2 is skipped since there is no item at that position in the array.

/*
 * every & some
 */
function isBigEnough(element, index, array) { return element >= 10; }
[12, 5, 8, 130, 44].every(isBigEnough);   // false
[12, 5, 8, 130, 44].some(isBigEnough);   // true

```

##Custom Constructors / Classes:  
```javascript
// new Object(); is a predefined constructor by js that creates an empty object, we can create our own class constructors like so:
function Person(name,age) {
  this.name = name;
  this.age = age;
}
var bob = new Person("Bob Smith", 30); // creates an object/class Person with the keys/properties specified in our constructor
// here is where a constructor notation makes sence

function Dog (breed) {
  this.breed = breed;
};

var buddy = new Dog("golden Retriever");
Dog.prototype.bark = function() { console.log("Woof"); }; // prototype adds that method to the constructor. Instead of buddy.bark = ... 
buddy.bark();
var snoopy = new Dog("Beagle");
snoopy.bark(); // is also able to bark.

function Penguin(breed) {
    this.breed = breed;
    var priv = "Private Variable"; // a variable that is only accessible within the constructor
    this.getPriv = function() { return priv; }; // to be able to access a private variable from outside
}
Penguin.prototype = new Dog(); // Penguins will inherit from Dog and also be able to bark. A Prototype Chain.
var penguin = new Penguin("penguin"); // create an instance of the Penguin class.
penguin.bark(); // Woof

/*
 * defineProperty & hasOwnProperty
 */
Object.prototype.nonsense = "hi"; // hi will be an enumerable property, hence show up in any for .. in .. loop on any object. To prevent that use defineProperty 
for (var name in map) console.log(name); // → pizza // → touched tree // → nonsense
delete Object.prototype.nonsense;
Object.defineProperty(Object.prototype, "hiddenNonsense", {enumerable: false, value: "hi"});
for (var name in map) console.log(name); // → pizza // → touched tree
console.log(map.hiddenNonsense); // → hi
// alternatively:
for (var name in map) {
  if (map.hasOwnProperty(name)) {
    // ... this is an own property
  }
}

```
 
##Random:
```javascript
Math.random() // random Number between 0 and 1 but never 0 nor 1
Math.floor(Math.random() * 2) // Random Number between 0-1
Math.floor(Math.random()*5 + 1) // Random Number between 1-5
// *x sets the width and +y the range of the randomness
```

##Loops:
```javascript
for(var i = 0; i < 6; i++){ }; // for a number of time
for(var p in obj){ } // for in iterates a specified variable p over all the enumerable properties of an object
while(true){ }; // while a condition is true
do{ }while(true); // does x while a condition is true (runs x at least once)
break; // terminate a loop, switch, or in conjunction with a label statement.
["a", "b", "c"].forEach(function(entry) { console.log(entry); }); //although move convenient, forEach is ~30% slower than a normal for loop
```


##Logical Operators
```javascript
if(true){ /* */ } else if (true){ /* */ } else { /* */ }
```
```javascript
switch(variable){ case 'option1': /* */ break; case 'option2': /* */ break; default: /* */ }
```

####Conditional Operator
```javascript
console.log(true ? 1 : 2); // → 1
console.log(false ? 1 : 2); // → 2
// The value before ? “picks” which of the other two values will come out. When true, the first value is chosen, and when false, the value on the right comes out.
```

##User Imput
```javascript
var name = prompt("who're you?");
```

##Const, Let
// these are experimental and not fully supported yet.   
**let** my_var –– creates a variable as VAR would do but limit its "scope" to the respective code block instead of the whole function   
**const** my_var = xy –– creates a variable as with VAR but make it a constant, you can not change its value later. However, you're still able to add attributes to objects.   



http://es6-features.org/ for all ECMAScript 6 suggestions (JavaScript 6+)   
https://kangax.github.io/compat-table/es6/ support Table for 6   


##JSON
```javascript
// JSON.stringify takes a JavaScript value and returns a JSON-encoded string
var string = JSON.stringify({name: "X", born: 1980});
console.log(string); // → {"name":"X","born":1980}
// JSON.parse takes such a string and converts it to the value it encodes
console.log(JSON.parse(string).born); // → 1980
```
