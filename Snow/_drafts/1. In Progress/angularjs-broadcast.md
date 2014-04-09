---
layout: post
title: AngularJs EventBus
categories: Javascript
published: draft
---

A hidden little gem of angularjs is it's bultin *EventBus* functionality.
Messages can be passed between controllers with ease.
There is currently two ways of passing messages: **Broadcast** and **Emit**.
The difference between them is the direction the message takes in the scope hierarchy.

By using **Emit** a nessage is transmitted upwards to parent scopes, but by using rootscope, there is no scope above, hence no bubbling and the standard functionality of a flat EventBus is mimicked.

	$rootScope.$$on("EventName", function(event, data){});
	$rootScope.$emit("EventName", data);

By using **BroadCast** a message is transmitted downwards to all child scopes and performance decreases with number of child scopes.
	
	$scope.$on("EventName", function(event, data){});
	$rootScope.$broadcast("EventName", data);

Since a flat EventBus is faster than potential propagation through child scopes, **Emit is the recommended way**

Example using Emit

	<!DOCTYPE html>
	<html>
	<head>
	  <script src="http://code.angularjs.org/1.2.15/angular.js"></script>
	</head>
	
	<body ng-app="MessageBusApp">
	  <div ng-controller="MainCtrl">
	    <div ng-controller="ChildCtrl">
	      Name:
	      <input type="text" ng-model="Name" />
	      <button ng-click="Go(Name)">Go!</button>
	    </div>
	  </div>
	
	  <script type="text/javascript">
	    angular.module("MessageBusApp", [])
	      .controller("MainCtrl", function($rootScope) {
	        $rootScope.$on('SayHello', function(event, clientDataObject) {
	          alert("Hello " + clientDataObject + "!")
	        });
	      })
	      .controller("ChildCtrl", function($rootScope, $scope) {
	        $scope.Go = function(name) {
	          $rootScope.$emit('SayHello', name);
	        }
	      });
	  </script>
	</body>
	</html>

Note:
