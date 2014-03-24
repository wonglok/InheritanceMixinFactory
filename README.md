InheritanceMixinFactory
=======================

Produces a new constructor function object that inherit the superclass that does not have naming override issue.


```js
(function(){
	//Techniques to generate a new constructor without having naming override issue.
	//So that the object would be named according to the variable's name.
	//1. Aynomomous function as a constructor template
	//2. Use function argument approach instead of a variable for referencing the aynom constructor function.
	function assignPrototype(ConsFunc, prototype){
		ConsFunc.prototype = prototype;
		return ConsFunc;
	}
	function constructorInheritanceFactory(SuperClass){
		//for every new constructor
		return assignPrototype(
			//allocate a new constructor function object
			function() { SuperClass.call(this); },
			//allocate a new parent object instance as prototype.
			new SuperClass()
		);
	}
	//export
	window.inheritFactory = constructorInheritanceFactory;
})();
```
