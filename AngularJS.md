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

    app.controller('StoreController', function()) {
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
        
        ]
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

### ng-class
Toggles a class if expression is true

    <li ng-class="{ active:tab === 1 }">Tab 1</li>

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



























End of file
