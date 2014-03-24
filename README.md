InheritanceMixinFactory
=======================

Produces a new constructor function object that inherit the superclass that does not have naming override issue.


```js

"use strict";
//Kudos
/*
MDN 
Performance
The lookup time for properties that are high up on the prototype chain can have a negative impact on performance, and this may be significant in code where performance is critical. Additionally, trying to access nonexistent properties will always traverse the full prototype chain.

AddyOsmani JS Patterns
Mixin Pattern

Tony Parisi
Sim.js
*/


(function(){
	//Techniques to generate a new constructor without having naming override issue.
	//So that the object would be named according to the variable's name.
	//1. Aynomomous function as a constructor template
	//2. Use function argument approach instead of a variable for referencing the aynom constructor function.
	/*
		mode 0 & 2 call parent constructor once only.
		mode 1 calls constructor twice.
		
		Both, 0 & 1 works with variable naming
	*/
	function assignPrototype(ConsFunc, SuperClassInstance){
		ConsFunc.prototype = SuperClassInstance;
		ConsFunc.prototype.constructor = ConsFunc;
		return ConsFunc;
	}
	function constructorInheritanceFactory(SuperClass, mode){
		//classical inhritance
		if (typeof mode === 'undefined' || mode === 0){
			//for every new constructor
			return assignPrototype(
				//allocate a new aynom constructor function object
				function() {}, //fast access parent property
				//allocate a new parent object instance as prototype.
				new SuperClass()
			);
		}else if (mode === 1){
			//approach in Sim.js
			return assignPrototype(
				//allocate a new aynom constructor function object
				function() { SuperClass.call(this); }, //fast access parent items
				//allocate a new parent object instance as prototype.
				new SuperClass()
			);
		}else if (mode === 2){
			//approach in MDN, and Three.js
			return assignPrototype(
				//allocate a new aynom constructor function object
				function() { SuperClass.call(this); }, //fast access parent items
				//allocate a new parent object instance as prototype.
				Object.create(SuperClass.prototype) //slow creation
			);
		}

	}
	//export
	window.inheritFactory = constructorInheritanceFactory;
})();

```
