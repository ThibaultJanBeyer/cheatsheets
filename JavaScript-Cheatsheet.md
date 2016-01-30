[back to overwiev](/../..)

#JavaScript Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Arrays & Objects](#arrays--objects)  
[Functions, Methods & Custom Constructors](#functions-methods--custom-constructors)  
[Random](#random)  
[Loops](#loops)  
[Logical Operators](#logical operators)  
[User Imput](#user-imput)  
[Const, Let](#const-let)  

##Basics
**&&** –– and  **||** –– or  **!** –– not  
1 = true  
0 = false  
**increments:**  
i++ , i-- , i += x , i -= x  
**isNaN('berry');** –– true –– returns true when not a number  
**isNaN(42);**  –– false   
**.length()** –– Returns the length of a string or the amount of items in a variable 

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
```

##Functions, Methods & Custom Constructors:
```javascript
var newFunction = function(argument,argument) { };
newFunction(x,y);
argument => argument + 1; // is equal to function(argument){ return argument + 1; };
```
**Methods**
```javascript
var bob = new Object();
bob.age = 17;
bob.setAge = function (newAge){ bob.age = newAge; };
bob.setAge(5); // would set the Age Property of bob to 5 using the respective function
// here we define our method using "this", before we even introduce bob
var setAge = function (newAge) { this.age = newAge; };
var bob = { age: 25, setAge: setAge }
bob.setAge(50);
// writing an object constructor or object literal notation – both work the same
```
**Custom Constructors**
```javascript
// new Object(); is a predefined constructor by js that creates an empty object, we can create our own constructors like so:
function Person(name,age) {
  this.name = name;
  this.age = age;
}
var bob = new Person("Bob Smith", 30); // creates an object with the keys/properties specified in our constructor
// here is where a constructor notation makes sence
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
for(var v in obj){ } // iterates a specified variable over all the enumerable properties of an object
for(var v of obj){ } // While for...in iterates over property names, for...of iterates over array elements
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

##User Imput
```javascript
var name = prompt("who're you?");
```

##Const, Let
**let** my_var –– creates a variable as VAR would do but limit its "scope" to the respective code block instead of the whole function
**const** my_var = xy –– creates a variable as with VAR but make it a constant, you can not change its value later. However, you're still able to add attributes to objects.