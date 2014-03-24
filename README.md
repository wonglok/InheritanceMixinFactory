InheritanceMixinFactory
=======================

Produces a new constructor function object that inherit the superclass that does not have naming override issue.


```js
"use strict";
//Kudos
/*
MDN 
Inheritance Performance **Heads up!**
The lookup time for properties that are high up on the prototype chain can have a negative impact on performance, and this may be significant in code where performance is critical. Additionally, trying to access nonexistent properties will always traverse the full prototype chain.

AddyOsmani JS Patterns
Mixin Pattern

Tony Parisi
Sim.js
*/

/*
//usage:
var EarthApp = inheritFactory(Sim.App,2);
console.log((new EarthApp()) instanceof Sim.App);
*/

//MDN Polyfill
if (typeof Object.create != 'function') {
    (function () {
        var F = function () {};
        Object.create = function (o) {
            if (arguments.length > 1) { throw Error('Second argument not supported');}
            if (o === null) { throw Error('Cannot set a null [[Prototype]]');}
            if (typeof o != 'object') { throw TypeError('Argument must be an object');}
            F.prototype = o;
            return new F();
        };
    })();
}


(function(){
	//Techniques to generate a new constructor without having naming override issue.
	//So that the object type would be named according to the variable's name.
	//1. Aynomomous function as a constructor template
	//2. Use function argument approach instead of a variable for referencing the aynom constructor function.
	
	function assignPrototype(ConsFunc, SuperClassInstance){
		ConsFunc.prototype = SuperClassInstance;
		ConsFunc.prototype.constructor = ConsFunc;
		return ConsFunc;
	}
	function constructorInheritanceFactory(SuperClass, mode){
		//classical inhritance
		if (typeof mode === 'undefined' || mode === 0){
			//Classical inhritance, MDN
			//works with variable naming
			//Calls SuperClass constructor function once.
			return assignPrototype(
				//allocate a new aynom constructor function object
				function() {}, //fast access parent property
				//allocate a new parent object instance as prototype.
				new SuperClass()
			);
		}else if (mode === 1){
			//MDN, and Three.js approach
			//This mode doesn't now work with variable based object type naming
			//Calls SuperClass constructor function once.
			//Assign the parent's item into
			return assignPrototype(
				function() { SuperClass.call(this); },
				Object.create(SuperClass.prototype) //slow creation
			);
		}else if (mode === 2){
			//Approach in Sim.js
			//Works with variable naming
			//Calls SuperClass constructor function twice.
			return assignPrototype(
				function() { SuperClass.call(this); },
				new SuperClass()
			);
		}
	}
	//export
	window.inheritFactory = constructorInheritanceFactory;
})();
```
