[back to overwiev](/../..)

#Angular Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Loop](#loop)  
[Html](#html)  
[Directives](#directives)  
[Services](#services)  
[Routing](#routing)  
[Filters](#filters)  

##Basics
**Setup**  
0. You can use the official [Angular Seed Project](https://github.com/angular/angular-seed) for quick startup.  
1. Create a new module named myApp
```javascript
// ja > App.js
var app = angular.module("myApp", []);
```
2. add a directive: it tells AngularJS that the myApp module will live within the `<body>` scope (or the whole page, `<head>`) 
```html
<!-- index.html -->
<head ng-app="myApp"> <!-- or -->
<body ng-app="myApp">
```
See: [more info on ng-app](https://docs.angularjs.org/api/ng/directive/ngApp)  
This is called “Bootstrapping”. Sometimes you want to [do it manually](https://docs.angularjs.org/guide/bootstrap#manual-initialization) (if you have multiple Angular apps for instance).
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

###Namings
**Binding** –– {{ ... }}  
**Expressions** –– something + '!'  
**Directives** ––  
**Template** –– (the part of the view containing the bindings and presentation logic) acts as a blueprint for how our data should be organized and presented to the user.  
**Controller** –– provides the context in which the bindings are evaluated and applies behavior and logic to our template.  
**Model** –– What the user sees after the page is fully rendered  
**Module** –– Child on Angular (usually it is the App in an Angular point of view)  
**Components** –– A combination of template + controller with an isolated scope (= no prototypal inheritance and no risk of our component affecting other parts of the application or vice versa) ([Docs](https://docs.angularjs.org/guide/component))  


###misc  
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

##Directives
in js/directives/appInfo.js
```javascript
app.directive('appInfo', function() { 
  return { 
    restrict: 'E', // specifies how directive will be used in view. 'E' means it will be used as a new HTML element.
    scope: { 
      info: '=' // specifies that we will pass information into this directive through an attribute named info. The = tells the directive to look for an attribute named info in the <app-info> element, like this: <app-info info="shutterbugg"></app-info>
    },          // The data in info becomes available to use in the template given by templateURL
    templateUrl: 'js/directives/appInfo.html', // specifies the HTML to use in order to display the data in scope.info. Here we use the HTML in js/directives/appInfo.html.
    link: function(scope, element, attrs) { // scope refers to the directive's scope. Any new properties attached to $scope will become available to use in the directive's template. element refers to the directive's HTML element. attrs contains the element's attributes. 
            scope.buttonText = "Install", // property buttonText
            scope.installed = false, // property installed

            scope.download = function() { // function download() 
              element.toggleClass('btn-active'); 
              if(scope.installed) { 
                scope.buttonText = "Install"; 
                scope.installed = false; 
              } else { 
                scope.buttonText = "Uninstall"; 
                scope.installed = true; 
              } 
            } 
          }

  }; 
});
```
in js/directives/appInfo.html
```html
<!-- define HTML to display details about app. Use expressions and filters to display data. -->
<img ng-src="{{ info.icon }}"> 
<h2>{{ info.title }}</h2> 
<p>{{ info.developer }}</p> 
<p>{{ info.price | currency }}</p>
<!-- part of link: function -->
<button ng-click="download()"> 
  {{ buttonText }} 
</button>
```
in index.html
```html
<!-- pass in objects from the controller's scope ($scope.shutterbugg) into the <app-info> element's info attribute so that it displays. -->
<app-info info="shutterbugg"></app-info>
```

##Services
**js > services > forecast.js**
```javascript
app.factory('forecast', ['$http', function($http) { // app.factory to create a new service named forecast + AngularJS's built-in $http to fetch JSON from the server
  return $http.get('https://s3.amazonaws.com/codecademy-content/courses/ltp4/forecast-api/forecast.json') // $http construct an HTTP GET request for the weather data (a json list from codecademy).
            .success(function(data) { // If the request succeeds, the weather data is returned; 
              return data; 
            }) 
            .error(function(err) { // otherwise the error info is returned.
              return err; 
            }); 
}]);
```
**js > controllers > MainController.js**
```javascript
app.controller('MainController', ['$scope', 'forecast', function($scope, forecast) { // add forecast into MainController as dependency so that it's available to use.
  forecast.success(function(data) { // to asynchronously fetch the weather data from server
    $scope.fiveDay = data; // store it into $scope.fiveDay 
  }); 
}]);
// any properties attached to $scope become available to use in the view
```
**index.html**
```html
<div class="main" ng-controller="MainController">
  <h1>{{ fiveDay.city_name }}</h1>
</div>
```

##Routing
**js > app.js**
```javascript
app.config(function ($routeProvider) { 
  $routeProvider // to define the application routes
    .when('/', { // to map the URL / to
      controller: 'HomeController', // to the controller HomeController 
      templateUrl: 'views/home.html' // and the template home.html
    })
    .when('/photos/:id', { // mapp URL to PhotoController and photo.html. + variable part named id to the URL
  		controller: 'PhotoController',
    	templateUrl: 'views/photo.html'
  	})
    .otherwise({ // Otherwise if a user accidentally visits a URL other than /
      redirectTo: '/' // we just redirect to /
    }); 
});
```
**js > controllers > HomeController**
```javascript
app.controller('HomeController', ['$scope', 'photos', function($scope, photos) { // use photos service
  photos.success(function(data) {
    $scope.photos = data;
  });
}]);
```
**js > controllers > PhotoController**
```javascript
// $routeParams to retrieve id from the URL by using $routeParams.id
// Notice injected both $routeParams and the photos service into the dependency array to make them available to use inside the controller.
app.controller('PhotoController', ['$scope', 'photos', '$routeParams', function($scope, photos, $routeParams) {
  photos.success(function(data) { // photos service to fetch the array of photos from the server
    $scope.detail = data[$routeParams.id]; // $routeParams.id to access the specific photo by its index
  });
}]);
```
**js > services > photos**
```javascript
// fetch the array of all photos and stores it into $scope.photos
app.factory('photos', ['$http', function($http) {
  return $http.get('https://s3.amazonaws.com/codecademy-content/courses/ltp4/photos-api/photos.json')
         .success(function(data) {
           return data;
         })
         .error(function(data) {
           return data;
         });
}]);
```
**views > home.html**
```html
<div class="item col-md-4" ng-repeat="photo in photos">
  <a href="#/photos/{{$index}}">
    <img class="img-responsive" ng-src="{{ photo.url }}">
    <p class="author">by {{ photo.author }}</p>
  </a>
</div>
```
**views > photo.html**
```html
<img ng-src="{{ detail.url }}">
```
**index.html**
```html
<!-- ow when a user visits /, a view will be constructed by injecting home.html into the <div ng-view></div> -->
<div ng-view></div>
```

##Filters
```javascript
detail.upvotes | number // 1,266
detail.pubdate | date // Oct 18, 2014 

```
**Filter an Array**  
```html
Search: <input type="text" data-ng-model="$ctrl.query">
Search by Name: <input type="text" data-ng-model="$ctrl.query.name"> <!-- save input in $ctrl.query -->
<p>Total number of phones: {{$ctrl.phones.length}}</p>
<ul class="phones">
  <li data-ng-repeat="phone in $ctrl.phones | filter:$ctrl.query"> <!-- use $ctrl.query as filter to only show matching elements -->
    <span>{{phone.name}}</span>
    <p>{{phone.snippet}}</p>
  </li>
</ul>
```
```html
<p>
  Sort by:
  <select ng-model="$ctrl.orderProp"> <!-- save selection in $ctrl.orderProp -->
    <option value="name">Alphabetical</option> <!-- save selection in $ctrl.orderProp.name -->
    <option value="age">Newest</option> <!-- save selection in $ctrl.orderProp.age -->
    <option value="-age">Oldest</option> <!-- place - in front to reverse -->
  </select>
</p>
<ul class="phones">
  <li data-ng-repeat="phone in $ctrl.phones | orderBy:$ctrl.orderProp"> <!-- use $ctrl.orderProp as filter to order the list -->
    <!-- if a value is not available it will return to the default -->
    <span>{{phone.name}}</span>
    <p>{{phone.snippet}}</p>
  </li>
</ul>
```
