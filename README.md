#Preconditions Library
####Support for Guava like Precondition error checking in Node.js

## ***** Release 1.0.2 has an npm install bug and has been deprecated!  Please update *****

<p>Ensuring a fail fast development environment can help developers find bugs quicker and easier.  
Ensuring all invariants are true at an initial point of contact will help you ensure this fail fast environment.  
This Preconditions library will assist you in doing just that by immediately throwing an Error
if any of your invariants fail.</p>

###High Level Info
There are three functions that are exposed from the library.
1. instance() - Create a testing suite passing in a single object.  Run a single, or multiple tests on the passed in object.
2. constructor() - Get the constructor function so you can extend the Preconditions library (see below for example).
3. singleton() - Get a singleton to verify a single value.

###Install
<pre>
    <code>
        npm install preconditions    
   </code>
</pre>

###Source Code
<pre>
    <code>
        https://github.com/corybill/Preconditions   
   </code>
</pre>

###Examples Using Instances (.instance())

####Setup Instance
1. Require the preconditions library - require("preconditions")
2. Call the .instance() function on the required preconditions.
3. Make an API call passing in a string representation of the variable you'd like to verify.
4. "String Representation" - Means using dot notation (i.e. "foo.deep.stringValue").
5. Looking at the object in the first code example, foo.deep.stringValue = "FOO"

Create and build the preconditions validator.
<pre>
    <code>
        var preconditions = require("preconditions").instance({
          foo: {
            deep: {
              stringValue: "FOO",
              numberValue: 10,
              functionValue: function () {},
              nonEmptyArray: ["some", "values"],
              emptyArray: [],
              finiteValue: -100,
              infiniteValue: Infinity,
              trueValue: true,
              falseValue: false,
              NaNValue: NaN,
              nullValue: null,
              regExpValue: /moe/,
              dateValue: new Date()
            }
          }
        });
        preconditions.shouldBeDefined("foo.deep.stringValue");
        preconditions.shouldBeDefined("foo.deep.emptyArray");
        preconditions.shouldBeUndefined("foo.deep.someValue");
        preconditions.shouldBeFunction("foo.deep.functionValue");
   </code>
</pre>
   
We can chain calls too.
<pre>
    <code>
        var preconditions = require("preconditions").instance(this);
        
        preconditions.shouldBeDefined("foo.deep.stringValue")
            .shouldBeDefined("foo.deep.emptyArray")
            .shouldBeUndefined("foo.deep.someValue")
            .shouldBeFunction("foo.deep.functionValue");
   </code>
</pre>

You can use the default error messages or you can pass in your own error message.  If you do not pass in an error message, then the default will be thrown.
<pre>
    <code>
        var preconditions = require("preconditions").instance(this);
                
        preconditions.shouldBeDefined("foo.deep.stringValue", "Custom error message.")
            .shouldBeDefined("foo.deep.emptyArray")
            .shouldBeUndefined("foo.deep.someValue", "Custom error message.")
            .shouldBeFunction("foo.deep.functionValue");
   </code>
</pre>

###Examples Using The Constructor (.constructor())

The Preconditions object itself is exposed so that you can extend the Preconditions class.
<pre>
    <code>
        var Preconditions = builder.constructor();
        
        function ChildClass(someObjectToTest) {
          Preconditions.call(this, someObjectToTest);
        }
        ChildClass.prototype = Object.create(Preconditions.prototype);
        ChildClass.prototype.shouldBeTrue = function (value, message) {
          var msg = message || this.ShouldBeTrue;
          if (value !== true) {
            throw new Error(msg);
          }
        };
  
        this.ShouldBeTrue = "ShouldBeTrue";
        this.childSut = new ChildClass(this.out);
        
        this.childSut.shouldNotBeFalsey(stringValue)
              .shouldBeDefined(stringValue)
              .shouldBeString(stringValue)
              .shouldNotBeFalsey(numberValue)
              .shouldBeDefined(numberValue)
              .shouldBeNumber(numberValue)
              .shouldBeTrue(false, "Value should be true (I am a custom error message).");
   </code>
</pre>

###Examples Using the Singleton (.singleton())

You can use a static instance to verify a single value.
<pre>
    <code>
        var preconditions = require("preconditions").singleton();
                
        preconditions.shouldBeDefined(someObj.valueOne, "Custom error message.")
            .shouldBeDefined(someObj.valueTwo)
            .shouldBeUndefined(someObj.valueThree, "Custom error message.")
            .shouldBeFunction(someObj.valueOne);
   </code>
</pre>

###API
####Guava API
| Signature (not including extra args)               | Default Error Message                                                                       |
|----------------------------------------------------|---------------------------------------------------------------------------------------------|
| checkArgument(configPath, message)                 | Illegal Argument.                                                                           |
| shouldBeDefined(T)                                 | Variable should be defined.                                                                 |
| checkState(boolean)                                | Illegal State.                                                                              |
| checkElementIndex(int index, int size)             | Index should be between between 0 (inclusive) and size (exclusive).                         |
| checkPositionIndex(int index, int size)            | Index should be between between 0 (inclusive) and size (inclusive).                         |
| checkPositionIndexes(int start, int end, int size) | Start and End should be between valid sub range between 0 (inclusive) and size (inclusive). |


####Convenience Functions
| Signature (not including extra args)       | Default Error Message                                                                                                                         |
|--------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| shouldBeDefined(configPath, message)       | Variable should be defined.                                                                                                                   |
| shouldBeUndefined(configPath, message)     | Variable should be undefined.                                                                                                                 |
| shouldBeNonEmptyArray(configPath, message) | Variable should be of type Array.                                                                                                             |
| shouldBeArray(configPath, message)         | Variable should NOT be of type Array.                                                                                                         |
| shouldNotBeArray(configPath, message)      | Variable should be of type Object.                                                                                                            |
| shouldBeObject(configPath, message)        | Variable should NOT be of type Object.                                                                                                        |
| shouldNotBeObject(configPath, message)     | Array or object should be empty.                                                                                                              |
| shouldBeEmpty(configPath, message)         | Array or object should NOT be empty.                                                                                                          |
| shouldNotBeEmpty(configPath, message)      | Variable should be a Function.                                                                                                                |
| shouldBeFunction(configPath, message)      | Variable should NOT be a Function.                                                                                                            |
| shouldNotBeFunction(configPath, message)   | Variable should be a String.                                                                                                                  |
| shouldBeString(configPath, message)        | Variable should NOT be a String.                                                                                                              |
| shouldNotBeString(configPath, message)     | Variable should be a Number.                                                                                                                  |
| shouldBeNumber(configPath, message)        | Variable should NOT be a Number.                                                                                                              |
| shouldNotBeNumber(configPath, message)     | Variable should NOT be a Number.                                                                                                              |
| shouldBeFinite(configPath, message)        | Variable should be Finite (i.e. not infinity).                                                                                                |
| shouldBeInfinite(configPath, message)      | Variable should be Infinite.                                                                                                                  |
| shouldBeBoolean(configPath, message)       | Variable should be a Boolean.                                                                                                                 |
| shouldNotBeBoolean(configPath, message)    | Variable should NOT be a Boolean.                                                                                                             |
| shouldBeDate(configPath, message)          | Variable should be a Date.                                                                                                                    |
| shouldNotBeDate(configPath, message)       | Variable should NOT be a Date.                                                                                                                |
| shouldBeRegExp(configPath, message)        | Variable should be a RegExp.                                                                                                                  |
| shouldNotBeRegExp(configPath, message)     | Variable should NOT be a RegExp.                                                                                                              |
| shouldBeFalsey                             | Variable should be falsey. (Means that the value should be NaN || Null || undefined.)                                                         |
| shouldNotBeFalsey                          | Variable should NOT be falsey. (Means that the value should not be NaN &amp;&amp; should not be Null &amp;&amp; and should not be undefined.) |

###Missing API or Bugs
Please reach out to me (Cory Parrish) if you would like a new precondition added or if you think you have found bug.