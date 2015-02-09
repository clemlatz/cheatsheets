Angular JS
==========

Docs:
https://docs.angularjs.org/api

## Modules

### Create a *store* module with *dependencies* array

    var app = angular.module('store', [dependencies])

### Include the *store* module at page loading

    <html ng-app="store">

### Print *Hello World* in the pae

    <p>{{ "Hello "+"world" }}</p>


## Controllers

### Create a controller

    app.controller('StoreController', function() {
        this.products = [{
                name: "Le Petit Prince",
                price: 10,
                canPurchase: true
            },
            {
                name: "Lolita",
                price: 15,
                canPurchase: false
            }
        
        ];
    }); 
    
### Use the controller in the HTML

    <div ng-controller="StoreController as store">
        <h1> {{ store.products[0].name }} </h1>
        <h2> ${{ store.products[0].price }} </h2>
        <p> {{ store.products[0].description }} </p>
    </div>
    
    
## Directives

### ng-init
Initializes a variable

    <ul ng-init="tab = 1"></ul>

### ng-show
Show an element if value is true

    <p ng-show="store.product.canPurchase">Will show</p>
    <li ng-show="tab === 1">Tab 1</li>
    
### ng-hide
Hide an element if value is true

### ng-repeat
Iterate through each element of an array

    <div ng-controller="StoreController as store">
        <div ng-repeat="product in store.products">
            <h1> {{ product.name }} </h1>
            <h2> ${{ product.price }} </h2>
            <p> {{ product.description }} </p>
        </div>
    </div>

### ng-src
To load an image from a variable

    <img ng-src="{{ product.image }}">
    
### ng-click
Executes an expression on click

    <li ng-click="tab = 1"></li>
    <li ng-click="tab = 2"></li>
    <li ng-click="tab = 3"></li>

### ng-change
Executes an expression on input value changes

### ng-class
Toggles a class if expression is true

    <li ng-class="{ active:tab === 1 }">Tab 1</li>

### ng-model
Binds form element to variables

    <input name="title" ng-model="article.title">
    
### ng-submit
Binds submit event of a form to a method of the controller

### ng-include
Includes a template

	<div ng-include="'included.html'">

### Custom element directives

	<product-title></product-title>
	
	app.directive('productTitle', function() {
		return {
			restrict: 'E',
			templateUrl: 'productTitle.html',
			controller: function() {},
			controllerAs: 'panels'
		};
	});

### Custom attribute directives

	<h3 product-title></h3>
	
	app.directive('productTitle', function() {
		return {
			restrict: 'A',
			templateUrl: 'productTitle.html',
			controller: function() {},
			controllerAs: 'panels'
		};
	});

## Filters

    {{ data|filter:options }}

### uppercase / lowercase
Changes case

### limitTo:{length}
Truncates a string or an array

    <p>{{ 'abcdefijkl'|limit:8 }}</p>

### date:{format}
Formats a date

    <p>{{ timestamp|date:'dd/MM/yyyy h:mm' }}</p>

### currency:{symbol}
Formats a price

    <p>{{ product.price|currency:'€' }}</p>

### orderBy:{key}
Sort items in an array by key or '-key' to descending order

    <p>{{ products|orderBy:'-price' }}


## Services

### Dependency injection

    app.controller('SomeController', ['$serviceA', '$serviceB', function($serviceA, $serviceB) {
        
    }])

### $http
Fetches JSON data from a web service

    $http({ method: 'GET', url: '/products.hson' });
    $http.get('/products.json', { apiKey: 'myApiKey' });

Returns a promise object with success() & error() functions

    $http(...).success( function(data) {
        
    })

## $log
Logs messages to the JavascriptConsole

## $filter
Filters an array
