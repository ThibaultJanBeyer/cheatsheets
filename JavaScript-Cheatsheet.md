[back to overwiev](/../..)

#JavaScript Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Arrays & Objects](#arrays--objects)  
[Random](#random)  
[Loops](#loops)  
[Logical Operators](#logical operators)  
[User Imput](#user-imput)  

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
var myObj = new Object(); // object constructor
myObj["key"] = "value"; // or myObj.key = "value";
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
while(true){ }; // while a condition is true
do{ }while(true); // does x while a condition is true (runs x at least once)
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
