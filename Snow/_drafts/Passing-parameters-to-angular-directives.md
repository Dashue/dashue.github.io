---
layout: post
title: "Passing parameters" to angular directives
categories: Angularjs
published: draft
---
To maximize the power and re-usability of directives, it's often desirable to "pass parameters" to them or rather bind variables so that the directive can access them. There are three ways (Binding strategies) to achieve this.
    
    @ : Passes the attribute as a string
    = : Data binds the property to a property in your directive´s scope.
    & : Passes in a function that can be called later
    
    If binding from and to the same name in both template and directive, then the name can be omitted.

    <myDirective name='MyName' /&gt;
    These are equal:
    name: '@name',
    name: '@
    
    This is not
    myDirectiveScopeName: '@name'
    I've created a menu directive which we can refactor into utilizing all of these three binding techniques. Starting out, we have four components, a view, a controller, a directive and a template.

    <div ng-controller="AngularMenuCtrl"&gt;
        <menu /&gt;
    </div&gt;
    
    module.controller('AngularMenuCtrl', function () {
    });
    
    <div class="menu"&gt;
        <span&gt;{{title}}</span&gt;
        <ul&gt;
            <li ng-repeat="item in items" 
                ng-class="{selected: $index==selectedIndex}" 
                ng-click="Click($index)"&gt;{{item.Text}}
            </li&gt;
        </ul&gt;
    </div&gt;
    
    app.directive("menu", function () {
        return {
            restrict: 'E',
            replace: true,
            transclude: false,
            templateUrl: '/Views/Menu.html',
            link: function (scope) {
                scope.title= 'My Menu';
    
                scope.items= [
                { Text: 'Menu1', Value: 'Url1' },
                { Text: 'Menu2', Value: 'Url2' }
                ];
    
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    alert("Transitioning to " + scope.items[index].Value);
                };
             }
         };
    });
    You've correctly  noticed that there isn't much re-usability in this directive: title, items and click action is hard-coded. Let's refactor it to provide the re-usability we are looking for. Let's start off with having the title be specified in the view. We bind this to the string representation using the **@** binding.

    <menu title="My Menu"/&gt;
    
    app.directive("menu", function () {
        return {
            restrict: 'E',
            replace: true,
            transclude: false,
            templateUrl: '/Views/Menu.html',
            scope: { title: '@' },
            link: function (scope) {
    
                scope.items= [
                { Text: 'Menu1', Value: 'Url1' },
                { Text: 'Menu2', Value: 'Url2' }
                ];
    
                scope.Click = function (index) {
                    scope.selectedIndex = index;
                    alert("Transitioning to " + scope.items[index].Value);
                };
             }
         };
    });
    The menu items would be more suitable to have in a controller in order to allow for reusing the directive with different MenuCtrls. To solve this we need to utilize the **=** binding.

    <menu title="My Menu" items="items"/&gt;
    
    module.controller('AngularMenuCtrl', function ($scope) {
         $scope.items = [
               { Text: 'Page One', Value: 'Url1' },
               { Text: 'Page Two', Value: 'Url2' }];
    });
    
    app.directive("menu", function () {
        return {
            restrict: 'E',
            replace: true,
            transclude: false,
            templateUrl: '/Views/Menu.html',
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
    There we go. For the click functionality we want every instance to re-use the selected menu item functionality (and any other menu base functionality) while at the same time have the ability to hook in their own custom click logic. We can achieve this by using the **&** binding to pass a reference to a function that we later invoke.

    <menu title="My Menu" items="items" click="Click(index)"/&gt;
    
    module.controller('AngularMenuCtrl', function ($scope) {
    
        $scope.items = [
               { Text: 'Menu1', Value: 'Url1' },
               { Text: 'Menu2', Value: 'Url2' }
        ];
    
        $scope.Click = function(index) {
            alert("Transitioning to " + $scope.items[index].Value);
        };
    });
    app.directive("menu", function () {
        return {
            restrict: 'E',
            replace: true,
            transclude: false,
            templateUrl: '/Views/Menu.html',
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
    When calling functions bound using **&** binding, the parameters of the function needs to be sent in the form of an object { parameterName: value }.
    
    And with this we have a fully working Menu directive ready for being re-used and expanded upon. Hope it was helpful!