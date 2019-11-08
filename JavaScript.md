# JavaScript Note
##### 2019/11/2 下午2:46:21

## Binding
+ declare a Binding
```
let ten = 10;
```

+ When a binding points at a value, that does not mean it is tied to that value forerver.
+ The ```=```operator can be used at any time on existing binding to disconnect them from their current value and have them point to a new one.
```
 let mood = 'light'
//light
 mood = 'dark'
//dark
```

+ If you ask for the value of an empty binding, you will get the value ```undefined```.
+ The word ```const``` stands for constant. It defines a constant binding, which points at the same value for as long as it lives.
+ Binding name must not start with a digit.
+ The collection of bindings and their values that exist at a given time is called the environment.
+ Executing a function is called ```invoking, calling, applying```,value given to the functions are called ```arguments```.
+ ```console.log``` to output values.
+ When a function produces a value, it is said to ```return``` that value.
+ ```Number``` function converts a value to a number.
+ The ```{}``` braces can be used to group any number of statements into a single statement, called a ```block```.
+ A ```do``` loop is a control structure similar to a ```while``` loop. It differs only on one point: a ```do``` loop always executes its body at least once.



## Function
+ A pure function is a specified kind of value-producing function that not only has no side effects but also doesn't rely on side effects from other code.
+ when called with the same arguments, it always produces the same value.
+ A key aspect in understanding functions is understanding scopes. Each block creates a new scope.

## Objects and arrays
+ objects allow us to group values including other objects to build more complex structure.
+ array is written as a list of values between square brackets, separated by commas. ```[v1,v2,v3]```
+ create an object is by using braces as an expression. ```{k1:v1,k2:v2}```
+ reading a property that doesn't exist will give you the value ```undefined```.
+ it is possible to assign a value to a property expression with the ```=``` operator. This will replace the property;s value if it already existed or create a new property on the object if it didn't.
+ the ```delete``` operator cuts off a tentacle from such an octopus. It is a unary operator, when applied to an object property, will remove the named property from the object.
+ the binary ```in``` operator, when applied to a string and an object, tell you whether that object has a property with that name.
+ to find out what properties an object has, use the ```Object.keys``` function.
+ ```Object.assign``` function that copies all properties from one object into another.
```
let obA = {a:1,b:2};
Object.assign(obA,{b:3,c:4});
// {a:1,b:3,c:4}
```
+ we saw that object value can be modified. The types of values, such as numbers, strings, and booleans, are all ```immutable```.
+ objects work differently. change their properties, causing a single object value to have different content at different times.
+ there is a difference between having two reference to the same object and having two different objects that contain the same properties.
```
let ob1 = {v:10};
let ob2 = ob1;
let ob3 = {v:10};
// ob1 == ob2
// ob1 != ob3
ob1.v = 15;
// ob2.v = 15
// ob3.v = 10
```
+ the binding ```ob3``` points to different object.
+ the ```const``` binding to an object can itself not be changed and will continue to point at the same object, the content of that object might change.
+ when comparing objects with ```==``` operator, if compares by identity:it will produce ```true``` only if both objects are precisely the same value.
+ comparing different objects will return ```false```, even if they have identical properties.
+ if a property name in brace notation isn't followed by a value, its value is taken from the binding with the same name.
+ arrays have an ```includes``` method that check whether a given value exists in the array.
+ adding and removing things at the start of an array are called ```unshift``` and ```shift```.
+ to search for a specific value, array provide an ```indexOf``` method. To search from the end instead of start, ```lastIndexOf``` method.
+ Both ```indexOf``` and ```lastIndexOf``` take an optional second arguments that indicates where to start searching.
+ ```slice``` method takes start and end indices and returns an array that has only the elements between them. The start index is inclusive, the end index exclusive.
+ ```concat``` method can be used to glue arrays together to create a new array.

## Strings and properties
+ value of type string, number, boolean are not objects.
+ ```trim``` method removes whitespace.
+ ```padStart``` method and takes the desired length and padding character as arguments.
+ split a string on every occurrence of another string with ```split``` and join it again with ```join```.
```
let sentence = "Secret specialize in stomping";
let words = sentence.split(" ");
console.log(words);
// ["Secret","specialize","in","stomping"]
console.log(words.join(". "));
// Secret. specialize. in. stomping
```
+ A string can be repeated with the ```repeat``` method.

## Rest parameter
+ put three dots before the function's last parameter. When such a function is called, the rest parameter is bound to an array containing all further arguments.
```
function max(...numbers){
  let result = -Infinity;
  for(let number of numbers){
     if(number < result) result = number;
  }
  return result;
}
```
+ use a similar three-dot notation to call a function with an array of arguments.
```
let arr = [5,1,7];
console.log(max(...arr));
```
+ square bracket array notation similarly allows the triple-dot operator to spread another array into the new array.
```
let words = ["never","fully"];
console.log(["will",...words,"understand"]);
// ["will","never","fully","understand"]
```
+ ```Math.floor``` method rounds down to the nearest whole number on the result of ```Math.random```.
```
console.log(Math.floor(Math.random()*10));
```
+ ```Math.ceil``` method rounds up to a whole number.
+ ```Math.round``` method to the nearest whole number.
+ ```Math.abs``` method takes the absolute value of a number.

## Destructuring
+ you directly try to access a property of those values.
```
let {name,age} = {name:"Faraji", age:23};
console.log(name);
// Faraji
console.log(age);
// 23
```

## JSON
+ all property names have to be surrounded by double quotes, and only simple data expression are allowed.
+ functions ```JSON.stringify``` and ```JSON.parse``` to convert data to and from this format.

## HIGHER-ORDER FUNTION
+ There is a built-in array method,```forEach```,that provides something like a ```for/of``` loop as a higher-order function.
```
["A","B"].forEach(element => console.log(element));
// A
// B
```
+ filter
```
function filter(array, test){
  let passed = [];
  for(let element of array){
    if(test(element)){
      passed.push(element);
    }
  }
  return passed;
}
```
+ map
```
function map(array,function){
  let mapped = [];
  for(let element of array){
    mapped.push(transform(element));
  }
  return mapped;
}
```
+ reduce
```
function reduce(array,combine,start){
  let current = start;
  for(let element of array){
    current = combine(current,element);
  }
  return current;
}
```
+ some
```
function some(array,test){
  for(let element of array){
    if(test(element)){
      return true;
    }
  }
  return false;
}
```
+ JavaScript strings are encoded as a sequence of 16-bit numbers(UTF-16).
+ JavaScript ```charCodeAt``` method gives you a code unit, not a full character code.
+ The ```codePointAt``` method, added later, does give a full Unicode character.
+ findIndex
```
fucntion findIndex2(array,findFun){
  let count = 0;
  for(let element of array){
    if(findFun(element)){
      return count;
    }
    count++;
  }
}
```
+ use ```forEach```  to loop over the elements in an array.
+ use ```filter``` method returns a new array containing only the elements that pass the predicate function.
+ use ```map``` to transform an array by putting each element through a function is done.
+ use ```reduce``` to combine all the elements in an array into a single value.
+ use ```some``` method tests whether any element matches a given predicate function.
+ use ```findIndex``` finds the position of the first element that matches a predicate.
