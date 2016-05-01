[back to overwiev](/../..)

#Angular Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Loop](#loop)  
[Html](#html)  

##Basics
**Setup**  
1. Create a new module named myApp
```javascript
// ja > App.js
var app = angular.module("myApp", []);
```
2. add a directive: tt tells AngularJS that the myApp module will live within the <body> scope  
```html
<!-- index.html -->
<body ng-app="myApp">
```
3. Create a new controller: manages the app's data.
```javascript
// js > controllers > MainController.js
app.controller('MainController', ['$scope', function($scope) { 
  $scope.title = 'Top Sellers in Books'; 
}]);
```
4. ng-controller is a directive that defines the controller scope
```html
<div class="main" ng-controller="MainController">
  <h1>{{ title }}</h1>
</div>
```
We access $scope.title using {{ title }}. That’s an expression: used to display values on the page.  

####misc
**Price**
```html
<p class="price">{{ product.price | currency }}</p>
```
AngularJS gets the value of product.price. It sends this number into the currency filter. The pipe symbol (|) takes the output on the left and "pipes" it to the right. The filter outputs a formatted currency with the dollar sign and the correct decimal places.  

**Date**
```javascript
pubdate: new Date('2014', '03', '08')
```
```html
<p class="date">{{ product.pubdate | date | uppercase }}</p>
```

##Loop
```javascript
$scope.products = [ 
  { 
    name: 'The Book of Trees', 
    price: 19, 
    pubdate: new Date('2014', '03', '08'), 
    cover: 'img/the-book-of-trees.jpg' 
  }, 
  { 
    name: 'Program or be Programmed', 
    price: 8, 
    pubdate: new Date('2013', '08', '01'), 
    cover: 'img/program-or-be-programmed.jpg' 
  }
]
```
```html
<div ng-repeat="product in products"> 
  <img ng-src="{{ product.cover }}">
  <p class="title">{{ product.name }}</p> 
  <p class="price">{{ product.price | currency }}</p> 
  <p class="date">{{ product.pubdate | date }}</p> 
  <p class="likes" ng-click="plusOne($index)">{{ product.likes }}</p>
</div>
```

##Html
```html
<!-- loops trough every product in products -->
<div ng-repeat="product in products"> 
  
<!-- include the source -->
<img ng-src="{{ product.cover }}"> 

<!-- ng-click is a directive that adds an onclick event. plusOne is the name of the function. $index passes the product number (in a loop). {{ product.likes }} displays the value -->
<p ng-click="plusOne($index)">{{ product.likes }}</p>
```
