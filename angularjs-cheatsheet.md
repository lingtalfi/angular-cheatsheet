AngularJs Cheatsheet
=========================
2016-07-22




Filters
=============
https://docs.angularjs.org/api


- currency
- date
- filter
- json
- limitTo
- lowercase
- number
- orderBy
- uppercase


Directives
================

### attributes
- ng-show  true|false
- ng-hide  true|false
- ng-disabled		true|false



Events
===========

http://www.w3schools.com/angular/angular_events.asp



Important Html attributes
----------------------------
http://www.w3schools.com/angular/angular_validation.asp


- required 
- type=email 




Partials
-----------
```html
<div ng-include src="sidebarURL"></div>
```

```js
$scope.sidebarURL = "partials/airport.html";
```

Router
-------------

```html
<div ng-view></div>
```

```js
angular.module('airline', [])
.config(airlineRouter);


function airlineRouter($routeProvider){
	$routeProvider
	.when('/', {templateUrl: 'partials/destinations.html'	})
	.when('/flights', {template: '<h3>Flights</h3>'	})
	;
}
```


### Route params

```js
// router code
angular.module('airline', [])
	.config(airlineRouter);

function airlineRouter ($routeProvider) {
	$routeProvider
		.when('/', {templateUrl: 'partials/destinations.html',
		 controller: 'DestinationsCtrl'})
		.when('/airports/:airportCode', {
			templateUrl: 'partials/airport.html',
		 	controller: 'AirportCtrl'
		});
}
```

```js
// controller code
function AirportCtrl($scope, $routeParams){
	$scope.currentAirport = $scope.airports[$routeParams.airportCode]
}
```

ng-include
--------------
```html
<!-- two_airports.html -->
<div ng-include="airportTemplate" 
class="span12" ng-controller="Airport1Ctrl"></div>
<div ng-include="airportTemplate" 
class="span12" ng-controller="Airport2Ctrl"></div>
```

```js
// controllers
function AppCtrl ($scope) {
  $scope.airportTemplate = "partials/airport.html";
}  

function Airport1Ctrl ($scope, $routeParams) {
	$scope.currentAirport = $scope.airports[$routeParams.airport1];
}

function Airport2Ctrl ($scope, $routeParams) {
	$scope.currentAirport = $scope.airports[$routeParams.airport2];
}
```

```js
// router
angular.module('airline', [])
	.config(airlineRouter);

function airlineRouter ($routeProvider) {
	$routeProvider
		.when('/airports/:airport1/:airport2', {
		 templateUrl: 'partials/two_airports.html'
		});
}
```



Navigation Active
--------------------

### with no default link active
```html
<ul class="nav nav-pills">
	<li ng-class="destinationsActive">
		<a ng-click="setActive('destinations')" 
		href="#">Destinations</a>
	</li>
	<li ng-class="flightsActive">
		<a ng-click="setActive('flights')" 
		href="#/flights">Flights</a>
	</li>
	<li ng-class="reservationsActive">
		<a ng-click="setActive('reservations')" 
		href="#/reservations">Reservations</a>
	</li>
</ul>
```

```js
function AppCtrl ($scope) {
  $scope.setActive = function (type) {
    $scope.destinationsActive = '';
    $scope.flightsActive = '';
    $scope.reservationsActive = '';
    $scope[type + 'Active'] = 'active';
  }
}  
```


### with default link active

```html
<ul class="nav nav-pills">
	<li ng-class="destinationsActive">
		<a href="#">Destinations</a>
	</li>
	<li ng-class="flightsActive">
		<a href="#/flights">Flights</a>
	</li>
	<li ng-class="reservationsActive">
		<a href="#/reservations">Reservations</a>
	</li>
</ul>
```


```js
angular.module('airline', [])
	.config(airlineRouter);

function airlineRouter ($routeProvider) {
	$routeProvider
		.when('/', {templateUrl: 'partials/destinations.html', 
			controller: function($scope){
				$scope.setActive('destinations');
			}})
		.when('/flights', {template: '<h3>Flights</h3>',
			controller: function($scope){
				$scope.setActive('flights');
			}})
		.when('/reservations', {template: '<h3>Your Reservations</h3>', 
			controller: function($scope){
				$scope.setActive('reservations');
			}});
}
```


Retrieving multiple records
-------------------
Makes it possible to connect an application to a server-side data store.

```html
<head>
	<!-- ... -->
	<script type="text/javascript" src="js/lib/angular-resource.min.js"></script>
	<script type="text/javascript" src="js/services.js"></script>
</head>
```


```js
js/app.js
angular.module('airline', ['airlineServices'])
	.config(airlineRouter);
```

```js
// js/services.js
angular.module('airlineServices', ['ngResource'])
	.factory('Airport', function($resource){
			return $resource("/airports");
	})
;

function airlineRouter ($routeProvider) {
	$routeProvider
	  	// ...
		.when('/reservations', {
		 template: '<h3>Your Reservations</h3> {{airports | json}}',
		 controller: 'ReservationsCtrl'});
}
```

```js
// partials/destination.js
function DestinationsCtrl ($scope, Airport) {
	//...
  	$scope.airports = Airport.query();
}
```

Retrieving individual record
------------------------
```html
<!-- index.html -->
<html ng-app="airline">
<!-- ... -->
<script type="text/javascript" src="js/lib/angular-resource.min.js"></script>
<!-- ... -->
```

```js
// js/app.js: router 
angular.module('airline', ['airlineServices'])
	.config(airlineRouter);

function airlineRouter ($routeProvider) {
	$routeProvider
		.when('/', {templateUrl: 'partials/destinations.html',
		 controller: 'DestinationsCtrl'})
		.when('/airports/:airportCode', {
		 templateUrl: 'partials/airport.html',
		 controller: 'AirportCtrl'
		});
}
```

```js
// app/services.js
angular.module('airlineServices', ['ngResource'])
	.factory('Airport', function  ($resource) {
		return $resource('/airports/:airportCode');
	});
```

```js
// js/controllers/destinations.js
function DestinationsCtrl ($scope, Airport) {
	// ...
  	$scope.setAirport = function (code) {
    	$scope.currentAirport = Airport.get({airportCode: code});
  	};
  	$scope.airports = Airport.query();
}

```


Searching through models
---------------------------

```html
<input type="text" class="input-small" placeholder="Origin" ng-model="search.origin" />
<table class="table">
  <thead>
    <tr>
      <th>Number</th>
    </tr>
  </thead>
  <tbody>
    <tr ng-repeat="flight in flights | filter:search">
      <td>{{flight.number}}</td>
    </tr>
  </tbody>
</table>
```



Save model
--------------
```html
<div class="controls controls-row" ng-model="reserve">
  <input type="text" class="input-small" placeholder="Origin" ng-model="reserve.origin" />
  <input type="text" class="input-small" placeholder="Destination" ng-model="reserve.destination" />
</div>
<a href="" class="btn btn-primary" ng-click="reserveFlight()">Reserve</a>
```

```js
function ReservationsCtrl ($scope, Reservations) {
	// ...
	$scope.reserveFlight = function  () {
		Reservations.save($scope.reserve, function (data) {
			$scope.reserve.origin = '';
			$scope.reserve.destination = '';

			$scope.reservations.push(data);
		});
	}
}
```


Filters
-------------
```js
angular.module('airline', ['airlineServices', 'airlineFilters'])
	.config(airlineRouter);

function airlineRouter ($routeProvider) {
	$routeProvider
		.when('/', {templateUrl: 'partials/destinations.html',
		 controller: 'DestinationsCtrl'});
}

angular.module('airlineFilters', [])
	.filter('originTitle', function () {
		return function (input) {
			return input.origin + ' - ' + input.originFullName;
		};
	})
	.filter('destinationTitle', function () {
		return function (input) {
			return input.destination + ' - ' + input.destinationFullName;
		};
	});
```



Form State and Input State
------------------------------
http://www.w3schools.com/angular/angular_validation.asp



Api
--------
http://www.w3schools.com/angular/angular_api.asp

- lowercase
- uppercase
- isString
- isNumber
