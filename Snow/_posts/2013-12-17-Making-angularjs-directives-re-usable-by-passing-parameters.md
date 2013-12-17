---
layout: post
title: Making angularjs directives re-usable by "Passing parameters"
categories: AngularJS
published: true
---
To make software components re-usable, it's required that no data be hard-coded. This can be achieved by passing parameters. There are different ways to do this and in angularjs directives one way is by binding variables to the scope. There are three different ways to bind, so called "Binding strategies". They are the following:
    
    @ : Binds to the value as a string
    = : Binds to a property
    & : Binds to a function that can be called later
    
If binding from and to the same name, the name can be omitted like so:

    name: '@name'
    name: '@

I've created a menu directive which we will refactor into utilizing all of these binding techniques. The menu has some hard-coded values namely *a title, two menu items with a text and a url, and an action that triggers when clicking an item.* The example is structured into four components, a view, a controller, a directive and a template. We will only be modifying the view, controller and directive in this example. Starting out they look like the following:

**View:**

    <div ng-controller="AngularMenuCtrl">
        <menu />
    </div>
    
**Controller:**

    module.controller('AngularMenuCtrl', function () {
    });

**Template:**

    <div class="menu">
        <span>{{title}}</span>
        <ul>
            <li ng-repeat="item in items" 
                ng-class="{selected: $index==selectedIndex}" 
                ng-click="Click($index)">{{item.Text}}>
            </li>
        </ul>
    </div>
    
**Directive:**

    app.directive("menu", function () {
        return {
            restrict: 'E',
           	templateUrl: '/Menu.html',
            link: function (scope) {
                scope.title= 'Menu';
    
                scope.items= [
                		{ Text: 'Item 1', Value: 'Url1' },
                		{ Text: 'Item 2', Value: 'Url2' }
                ];
    
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    alert("Transitioning to " + scope.items[index].Value);
                };
             }
         };
    });

Let's start off with having the title be specified in the view by adding an html attribute called **title**.

    <menu title="Menu"/>
    
We then remove the title assignment in the link function of our directive. And add the scope object to our directive and bind the title property to the string representation of our html attribute 
*title* using the **@** binding. Remember that **since we bind the html attribute 'title' to the directives' scope property 'title', we can omit the name and just use @ instead of @title**

    app.directive("menu", function () {
        return {
            restrict: 'E',
            templateUrl: '/Menu.html',
            scope: { title: '@' },
    		link: function (scope) {
				scope.items= [
                		{ Text: 'Item 1', Value: 'Url1' },
                		{ Text: 'Item 2', Value: 'Url2' }
                ];
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    alert("Transitioning to " + scope.items[index].Value);
                };
             }
         };
    });

Since the menu entries will almost always be different, let's go ahead and make them configurable by moving them to our menu controller. That way different menus can have different surrounding menu controllers. We'll do this by introducing a scope property named *items* and bind our html attribute with the same name to this new property. 

    <menu title="Menu" items="items"/>
    
    module.controller('AngularMenuCtrl', function ($scope) {
         $scope.items = [
               { Text: 'Item 1', Value: 'Url1' },
               { Text: 'Item 2', Value: 'Url2' }];
    });
    
We then bind this new attribute to our directives' scope using **=**. And also remove the existing hard-coded menu items in the directive.

    app.directive("menu", function () {
        return {
            restrict: 'E',
            templateUrl: '/Menu.html',
            scope: {
                title: '@',
                items: '='
            },
            link: function (scope) { 
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    alert("Transitioning to " + scope.items[index].Value);
                };
             }
         };
    });

With two out of three objectives achieved the only thing left in our example is the hard-coded click functionality. For the click functionality we want the selection of the active menu item (and any other core menu functionality) to be re-used and at the same time allow for some custom user specified functionality to be run (in our case: alerting which url we are transitioning to).

Let's take the same approach we took for our menu items: We first create an *AlertTransition* function on the controller scope and move the alert functionality over from the directive:

	module.controller('AngularMenuCtrl', function ($scope) {
    
        $scope.items = [
               { Text: 'Item 1', Value: 'Url1' },
               { Text: 'Item 2', Value: 'Url2' }
        ];
    
        $scope.AlertTransition = function(index) {
            alert("Transitioning to " + $scope.items[index].Value);
        };
    });

We then bind this in our view, but note that this time the name of the html attribute and scope property are not the same, showing that they can be different.

	<menu title="Menu" items="items" click="AlertTransition(index)"/>

We then bind the click html attribute to our directives' scope using the **&** binding. This will pass a reference to a function to our directive that we can then invoke in our menu click function. 
    
       app.directive("menu", function () {
        return {
            restrict: 'E',
            templateUrl: '/Menu.html',
            scope: {
                title: '@',
                items: '=',
                click: '&'
            },
            link: function (scope) {
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    scope.click({ index: index });
                };
            }
        };
    });

One tricky thing when invoking a function bound using **&**, is that the parameters have to be passed in the form of an object:

	{ parameterName: value }
    
And with this last step we have a fully working Menu directive ready for re-use :)  
[Get the code for this sample](https://github.com/Dashue/Blogging/tree/master/Making_angularjs_directives_re_usable_by_passing_parameters)