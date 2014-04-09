 javascript
 
 toequaldata
 beforeEach(inject(function($rootScope, $controller) {
        this.addMatchers({
            toEqualData: function (expected) {
                return angular.equals(this.actual, expected);
            }
        });
