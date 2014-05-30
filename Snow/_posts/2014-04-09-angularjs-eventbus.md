---
layout: post
title: AngularJs EventBus
categories: Javascript, AngularJs
published: true
---

---excerpt
This post introduces the EventBus functionality of angularjs and explains two ways of using it.
---end

## Background
A hidden little gem of angularjs is its bultin *EventBus* functionality.
Messages can be passed between controllers with ease.
There is currently two ways of passing messages: **Broadcast** and **Emit**.
The difference between them is the direction the message takes in the scope hierarchy.

## Using BroadCast
By using BroadCast a message is transmitted downwards to all child scopes. Every scope is a child scope of root scope and because of this, performance quickly takes a hit.
	
	$scope.$on("EventName", function(event, data){});
	$rootScope.$broadcast("EventName", data);

## Using Emit
By using Emit a nessage is transmitted upwards to parent scopes. When using rootscope there is no parent scope, hence no bubbling and the standard functionality of a flat EventBus is achieved.

	$rootScope.$$on("EventName", function(event, data){});
	$rootScope.$emit("EventName", data);

## Example Using Emit

	<!DOCTYPE html>
	<html>
	<head>
    	<script src="http://code.angularjs.org/1.2.15/angular.js"></script>
	</head>

	<body ng-app="MessageBusApp">
		<div ng-controller="MainCtrl">
	    	Name: <span>{{ConfiguredName}}</span>
	    	<br/>
	    	
			<div ng-controller="ChildCtrl">
	        	Set Name: <input type="text" ng-model="Name"/> <button ng-click="Go(Name)">Go!</button>
	    	</div>
		</div>

		<script type="text/javascript">
    	angular.module("MessageBusApp", [])
            .controller("MainCtrl", function ($rootScope, $scope) {
                $rootScope.$on('SayHello', function (event, name) {
                    $scope.ConfiguredName=name;
                });
            })
            .controller("ChildCtrl", function ($rootScope, $scope) {
                $scope.Go = function (name) {
                    $rootScope.$emit('SayHello', name);
                }
            });
		</script>
	</body>
	</html>

## Conclusion
Since a flat EventBus is faster than potential propagation through child scopes, **Emit is the recommended way**.