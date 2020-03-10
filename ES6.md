#  ES6

## Datatypes

`let` & `const` (let,const and var) is used to declare variables. <u>let</u> variables can be modified and <u>const</u> variables cannot be modified.

*const can be used for objects and arrays as well and we can't change the type in this case but we can modify the attributes in the objects and values in the arrays*

```javascript
let x = 1;
const y = 'test';
```



## String substitute

String substitute is simplest way to substitute dynamic values in a string.

 *Along with variables even functions can be placed in the string.*

```javascript
`${name}`; //***
console.log(`${name}`);
```



## Functions

- **Defaults** 

  Now we can provide default values incase it is missing from the calling function. So if values is not passed the default values will be used

  *We can make functions as defaults and we can make other parameters also as defaults. Temporary dead zone(TDZ), make sure default parameters is used in proper order.*

  ```javascript
  function asyncFun(url, timeout = 2000) { //***
      // code here
  }
  ```

  

- **Arguments**

  All the arguments to the function can be accessed using `arguments` array.

  *Updating the parameters will update the arguments array. But same thing is not true in strict mode and when default parameters is used.* 

  ```javascript
  function asyncFun(url, timeout) {
      console.log(arguments[0]); // https://google.com
      console.log(arguments[1]); // 2000
  }
  asyncFun('https://google.com', 2000)
  ```

  

- **Rest parmeters**

  The <u>rest parameter</u> syntax allows us to represent an indefinite number of arguments as an array.

  *Extra parameters can be managed well using rest parameters. You can't put the rest parameter in the middle, it has to be in last always.*

  ```javascript
  //1
  function pick(object, ...keys){ //***
      // code here
  }
  //2
  function sum(...theArgs) { //***
    return theArgs.reduce((previous, current) => {
      return previous + current;
    });
  }
  
  console.log(sum(1, 2, 3)); // 6
  ```

  

- **Spread operator**

  It is just opposite of rest parameters, it is to used send array of objects separately.

  *It need not be the last parameter.*

  ```javascript
  let numArr = [25,30,40]; 
  console.log(Math.max(...numArr)); //***
  ```

  

- **Constructor functions** 

  Technically they are regular functions. There are two conventions though:

  1. They are named with capital letter first.
  2. They should be executed only with `"new"` operator. 

  *We can use rest operators with constructor functions also.*

  ```javascript
  //1
  let func = new Function('first','second','return first + second'){ //***
      // code here
  } 
  //2
  function User(name) {
    this.name = name;
    this.isAdmin = false;
  }
  
  let user = new User("Jack"); //***
  ```

  

- **Name property** 

  We can use name property to get the function name.

  ```javascript
  let doSomething = function(){
      // code here
  } 
  console.log(doSomething.name); // *** // doSomething
  ```

  

- **new.target** 

  This pseudo-property lets you detect whether a function or constructor was called using the [new](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) operator.

  In constructors and functions invoked using the [new](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) operator, `new.target` returns a reference to the constructor or function. In normal function calls, `new.target` is [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined).

  ```javascript
  function Foo() {
    if (!new.target) throw 'Foo() must be called with new'; //***
  }
  
  try {
    Foo();
  }
  catch(e) {
    console.log(e);
    // expected output: "Foo() must be called with new"
  }
  ```

  

- **Arrow functions** allow us to write shorter function syntax

  *No `this`, super, arguments object and `new.target` bindings. Cannot be called with <u>new</u>. Can't change `this`. No duplicate named parameters. It will have access to the parent this and arguments object.*

  

- **Tail call**  The last call is function call inside a function



## Objects

- **Literal Shorthand Syntax** 

  No need to assign the value if parameter and attribute name is same.

  ```javascript
  function makePerson(name, age){  
      return { name,age}; //***
  }
  ```

  

- **Concise  Method Syntax** 

  No need to specify : `function()` while defining the methods inside objects. We will have access to `super`.

  ```javascript
  const person = { 
      name: 'Andrew',
      sayName() {  //***
          console.log('Hi')
      } 
  };
  ```

  

- **Computed property names**

  We can add variables in property names and perform calculations in the expressions.

  ```javascript
  let firstName = "first name";
  let secondName="second name";
  let suffix ="name";
  const person = { "first name": 'Zack', [secondName]: 'attack', ['third '+ suffix]: 'Shack'};/***
  ```

  

- **Object.is()**

  It mostly works similar to `===`, along with it tries to solve some quirky behavior of JavaScript.

  ```javascript
  Object.is(+0,-0);//*** //false
  Object.is(NaN,NaN);//*** //true
  ```

  

- **Object.assign()**

  Mixin. It is way to create new object  from existing object.

  *We can pass multiple objects while creating new one, it will create a new one combining all the objects.*

  ```js
  const person ={name:'Jack'}; 
  Object.assign({}, person); //***
  ```

  

- **Object.setPrototypeOf()**

  We have direct control over the prototype of our objects so we can manipulate the chain.

  ```js
  let person =  {calling:'hello'};
  let animal = {calling:'owww'};
  let friend = Object.create(person);
  console.log(friend.calling); // hello 
  Object.setPrototypeOf(friend, animal); //***
  console.log(friend.calling) // owww
  ```

  

- **Object.create(null)** will create object without inheriting any property.

  

- **Duplicate property**

  It will not throw an error instead the last property name listed will be applied for the object. 

  ```js
  const car = {make: 'Ford', make: 'Chevy'};//***  //{make:'Chevy'}
  ```

  

- **Property enumeration order**

  Property enumeration order. All numeric keys in ascending order. All string & symbol keys in the order in which were added to the object. 

   *It will effect how the getOwnPropertyNames & Reflect.keys work.*

  ```js
  const obj = {0:'0',b:'b',1:'1',a:'a'};
  console.log(Object.getOwnPropertyNames(obj)); //['0','1','b','a']
  ```

  

- **Super**
  It can be used to called the parent function in the prototype chain.

  ```js
   let person = {
       greet() { 
           return 'hello';
       }
   }; 
  let animal = { 
      greet() {
          return 'owww';
      }
  }; 
  let friend = { 
      greet() { 
          return super.greet()+ ' there'; //***
      }
  };
  Object.setPrototypeOf(friend, person); 
  console.log(friend.greet()); // hello there
  ```

  

- **Configurable property** 

  **configurable** property controls at the same time whether the property can be deleted from the object and whether its attributes (other than value and writable) can be changed. If configurable is `false`, you can't delete the property. By default it is `false`. `Object.defineProperties()` & `Object.defineProperty()` can be used to set this property. 

  ```js
  let o = {}; 
  Object.defineProperty(o, 'a', {
      value: 37,
    writable: true,
      enumerable: true,
      configurable: true //***
  });
  ```
  
  

## Destructuring

1. ### Object

   - **Basic object destructuring**

     Simplifies assigning the variables from the object. Variables should have same name as the object parameters.

     ```js
     const example = { save: true, reload: true};
     let {save,reload} = example; //***
     console.log(save); //true 
     console.log(reload);//true
     ```

     

   - **Destructuring assignment**

     While reassigning the variables it should be enclosed within parenthesis`()`.

     ```js
     const example = { save: true, reload: true};
     let save = false, reload=false;
     ({save,reload} = example); //***
     console.log(save);//true 
     console.log(reload);//true
     ```

     

   - **Default values**

     We can have default values in destructuring so if there is no value provided then the default value will be applied.

     ```js
     const obj = {name:'foo', identifier: 'the best'};
     let {name, identifier, cup = 'coffee cup'} = obj; //***
     console.log(name,identifier,cup) // foo the best coffee cup
     ```

     

   - **Declaring different variable names**

     We can achieve different variable names/alias in destructuring. 

     ```js
     const drink = {temprature: 'hot',name:'coffee'}; 
     let {
         temperature: localTemp,//***
         name: localName,//***
         flavorRating: localRating = 9 //***
     } = drink;
     console.log(localTemp,localName, localRating); // hot coffee 9
     ```

     

   - **Destructuring nested object**

     ```js
     const user = {
         name:'Johnny',
         age:34,
         bestFriend: { 
             name:'Malcom',
             age:32,
             tempremant:'docile'
         }
     };
     let {bestFriend: {tempremant}} = user; //*** 
     console.log(tempremant); //docile
     ```

     

2. ### Array

   - **Basic array destructuring**

     We can do the same for array like object. Here it works on the position. 

     ```js
     let friends = ['John', 'Jessica', 'Jimmy'];
     let [firstFriend, secondFriend] = friends; //***
     console.log(firstFriend, secondFriend); // John Jessica 
     let [,,thirdFriend] = friends; //***
     console.log(thirdFriend); // Jimmy
     ```

     

   - **Array assignment**

     ```js
     let colors = ['red', 'blue', 'green'], 
         firstColor ='purple',
         secondColor = 'gray'; 
     console.log(firstColor,secondColor); // purple gray
     [firstColor, secondColor] = colors; //***
     console.log(firstColor,secondColor); // red blue; 
     ```

     

   - **Value swapping**

     We can swap values in simpler way without temp. 

     ```js
     let a=1,b=2; 
     console.log(a,b) // 1 2
     [b, a] = [a,b]; //***
     console.log(a,b) // 2 1; 
     ```

     

   - **Default values**

     We can have default values in destructuring so if there is no value provided then the default value will be applied.

     ```js
     let color = ['red']; 
     let [firstColor, secondColor = 'green'] = color; //***
     console.log(secondColor); // green
     ```

     

   - **Destructuring nested arrays**

     ```js
     let colors = ['red', ['skyblue', 'dodgerblue', 'navy'], 'green'];
     let [firstColor, [secondColor, , thirdColor], fourthColor] = colors; //***
     console.log(firstColor, secondColor, thirdColor, fourthColor); // red skyblue navy green
     ```

     

   - **Rest items**

     We can use rest variable while destructuring. We can also use it to copy of the array.

     *It has to be the last item.*

     ```js
     let colors = ['red', 'blue', 'green'];
     let [firstColor, ...restColors] = colors; //***
     console.log(firstColor, restColors); // red ['blue', 'green']
     ```

     

   - **Mixing type**

     We can destructure different types (Array &  Object)  at the same time. 

     ```js
     let user = { 
         name: 'John',
         age: 37,
         favoriteColors: ['green','purple', 'blue'],
         bestFriend: {
             name:'Malcolm',
             temprament:'chill'
         }
     };  
     let {name: localName,
          favoriteColors: [, localColor],
          bestFriend:{
              temprament: localTemprament
          }
         } = user; 
     console.log(localName, localColor, localTemprament); // John purple chill
     ```

     

3. ### Parameters

   - **Basic destructuring**

     We can destructure at the parameter level itself. If you destructure at the parameter level then it will be required in JavaScript, if we omit while call function then it will throw error. 

     ```js
     function setCookie(name, value, {secure, domain, path, expires}){//***
         console.log(expires);  
     } 
     setCookie('type', 'js', {secure: true, expires: 5000}); // 5000
     ```

     *Other than secure and expires others will be undefined.*

   - **Default values**

     We can provide default values individually or together for whole object. 

     ```js
     function setCookie(name, value, {secure = true, domain, path = '/foo', expires = 2000} = {}){ //***
     	console.log(secure, domain, path, expires);
     } 
     setCookie('type', 'js', {domain: 'www'}); // true www /foo 2000 
     ```

     

## Sets

Similar to an array but can't contain duplicates. If you try to add duplicates it will be ignored

- **Creation**

  ```js
  let set = new Set(); //***
  set.add("5");
  set.add(5);
  console.log(set.size); // 2 
  ```

  *"5" & 5 are not duplicates in Set.* 

  

- **Initialization**

  We can initialize Set by passing array.

  ```js
  const arr = [1,2,3,3,3,4,4,4,5,5,5]; 
  let arrSet = new Set(arr); //***
  console.log(arrSet.size) // 5 
  console.log(arrSet.has(5)) // true
  ```

  

- **Cohercing**

  It will not coherce the objects.

  ```js
  let set = new Set();
  const key1 = {}, key2={};
  set.add(key1);
  set.add(key2);
  console.log(set.size);//***  // 2  
  ```

  *In the js simulation of set, it would have considered as a single value `[object, object]`  (`object` is the key) and the size would be 1. Set will treat this as separate value so the size is 2. In array all the keys are turned into string,"5" & 5 keys will be same.*

  

- **Available methods**

  1. size - Using `size` we can get size of the Set. 

  2. delete - Using `delete` we can delete the entry

  3. has - using `has`  we can check if the value is available or not

  4. clear - using `clear` we can clear the Set.

  5. get - using `get` we can get required value.

     ```js
     let arrSet = new Set([1,2,3,4,5]);
     console.log(arrSet.size); // 5
     arrSet.delete(4);
     console.log(arrSet.has(4)); // false
     arrSet.clear();
     console.log(arrSet.size); // 0
     ```

     

- **Sets to array**

  ```js
  let set = new Set([1, 2]), arr = [...set]); //***
  ```

  

- **Strong Sets**
  Strong Sets will store the object reference, if we even hollow out the object(changing the value of the object) it will still hold the value. All the above examples are strong Sets. 

  ```js
  let set = new Set(), key = {present: true};
  set.add(key);
  key = null;
  let key2 = [...set][0];
  console.log(key2); //*** // {present: true} 
  ```

  

- **Weak Sets**

   It can story only weak object references and can't store primitive values. A weak reference to an object does not prevent garbage collection if it is the only remaining reference.

  *The add method throws an error when passed a non-object.(`has()` and `delete()` always return false for non-object arguments). Weak sets are not iterables and therefore cannot be used in a for-of loop, so there is no way to programmatically determine the contents of a weak set. Weak sets do not have size property.*

  ```js
  let ws = new WeakSet(), key1 = {};
  ws.add(key1);
  console.log(ws.has(key1)); // true  
  key1 = null;
  console.log(ws.has(key1))//*** //false 
  ```

  

## Maps

An ordered list of key-value pairs. Both key and value can have any type.

- **Creation**

  ```js
  let map = new Map();
  map.set("name", "Andrew");
  map.set("drink", "water");
  console.log(map.get("name")); // Andrew
  console.log(map.size); // 2 
  ```

- **Initialization**

  We can initialize Map by passing array. 

  ```js
let map = new Map ([ ["name", "Andrew"], ["age", 39] ]);
  console.log(map.get("age"));  // 39
console.log(map.size)  // 2 
  ```

  
  
- **Available methods**

  1. size - Using `size` we can get size of the Map. 
2. delete - Using `delete` we can delete the entry
  3. has - using `has`  we can check if the value is available or not
4. clear - using `clear` we can clear the Map.
  5. get - using `get` we can get required value.

  ```js
let map = new map();
  map.set("name", "Andrew");
  map.set("age", 74);
  console.log(map.has('name')); //true
  console.log(map.delete('name')); // true
  ```

  *If you try to get the value which is not available it will return `undefined`.*

  

- **Weak Maps**

  Weak Maps are similar to weak Sets. It will help avoid memory leak, this can be used if we want keep the reference of the `dom` element. In case if we want to keep track of the `dom` elements, removing the `dom` element will remove the key references in the map. It will have `has`, `get` and `delete` methods. Since we can't iterate so it doesn't have `size` & `clear` method. 

  ```js
  let weakmap = new WeakMap(), 
      elm = document.querySelector('.element'); 
  weakmap.set(elm, 'original element');
  elm.parentNode.removeChild(element);
  console.log(weakmap.get(elm)); // original element 
  elm = null;
  console.log(weakmap.get(elm)); // undefined 
  ```

  *In stronger maps the reference will still be available after removing hollow out the object.*



## Iterators

Objects with an interface designed for iteration. 

- Iterator objects have a `next()` method, it returns a result object. It has two properties value, which is the next `value` and `done`, which is a **boolean** that's `true` when there are no more values to return. Iterator is invoked only on the call of next method, so that means it stops after it yields, it doesn't loop over all the values.

  

- **Generators** 

  A function that returns an iterator. *keyword makes a function a Generator function. 

  **yield** can be used only in a Generator function.
  
  *We can't make the arrow function as Generator function*
  
  ```js
  // 1
  function *makeIterator() { //***
      yield 1;
      yield 2;
      yield 3;
  } 
  let iterator= makeIterator();
  console.log(iterator.next().value); // 1
  console.log(iterator.next().value); // 2
  console.log(iterator.next().value); // 3 
  console.log(iterator.next().done); // true 
  
  //2
  function *createIterator(items) { //***
      for(let i =0; i < items.length; i++) {
          yield items[i];
      } 
  }
  const items = [1,2,3];
  let iterator= createIterator(items);
console.log(iterator.next().value); // 1 
  console.log(iterator.next().value); // 2 
  console.log(iterator.next().value); // 3 
  console.log(iterator.next().done); // true
  
  //3 Generator function with anonymous function
  let makeIterator = function *(items) {
      for(let i =0; i < items.length; i++) {
          yield items[i];
      } 
  }
  const items = [1,2,3];
  let iterator= createIterator(items);
  console.log(iterator.next().value); // 1 
  console.log(iterator.next().value); // 2 
  
  //4 We cannot make arrow function as Generator function
  *() => {   // This will fail
      
  }
  ```
  
- **Generator Methods**

  ```js
  let obj = {
      *makeIterator(items) {
         for(let i =0; i < items.length; i++) {
          	yield items[i];
      	}  
      }
  }
  ```

  

- **Iterable**

  Object with a `Symbol.iterator` property specifies that it returns an iterator for the object

  Arrays, Sets and Maps, as well as Strings are iterable by default. `for of` will not work if it is not iterable.

  ```js
  let drinks = ['cofee', 'milk', 'tea'];
  let iterator = drinks[Symbol.iterator](); //***
  console.log(iterator.next()); // {value: 'coffee', done: false}
  
  // We can check if it is iterable by checking typeof of Symbol.iterator, it should be a function if it is iteratable
  function isIterable(obj) {
      return typeof obj[Symbol.iterator] === "function";
  }
  console.log(isIterable([1,2])); // true
  console.log(isIterable(new Map())); // true
  console.log(isIterable(new Set())); // true
  console.log(isIterable(new WeakMap())); // false
  console.log(isIterable(new WeakSet())); // false
  console.log(isIterable({})); // false
  ```

  Objects by default are not iterable, we can't use `for of` on object. We can use **Generator functions** to make it iterable.

  ```js
  let collection = {
  	items: [],
      *[Symbol.iterator]() { // *** 
          for(let item of this.items) {
          	yield item;
      	} 
      }
  }
  collection.items.push(1);
  collection.items.push(2);
  collection.items.push(3);
  
  for(let val of collection) { //*** We could use the for of because we used generator function
      console.log(val); // 1  // 2 // 3
  }
  ```

  

- **Built-in Iterators**

  1. entries() - Returns an iterator whose values are a key-value pair.
  2. values() - Returns an iterator whose values are the values of the collection
  3. keys() - Returns an iterator whose values are the keys contained in the collection.

  By default, Arrays and Sets uses `values()` iterator. Maps uses `entries()` iterator.

  *WeakMap & WeakSet is not iterable because they don't have a built-in iterator. The reason for this is, it is almost impossible to know how many values are available in that data collection. To use a iterator you need know how many values in that collection.*

  ```js
  let colors = ['red', 'blue', 'green'];
  let tracking = new Set([45, 56, 82, 23]);
  let data = new Map();
  data.set('name', 'Andrew');
  data.set('age', 90);
  
  // entries()
  for(let entry of colors.entries()) {
      console.log(entry); // [0, "red"] // [1, "blue"] // [2, "green"] 
      //*** key-value pairs // index-value
  }
  for(let entry of tracking.entries()) {
      console.log(entry); // [45, 45] // [56, 56] // [82, 82]  // [23, 23]
      //*** key-value pairs // value-value
  }
  for(let entry of data.entries()) {
      console.log(entry); // ["name", "Andrew"] // ["age", 90] 
      //*** key-value pairs // key-value
  }
  
  // values()
  for(let entry of colors.values()) {
      console.log(entry); // red // blue // green
      //*** only values
  }
  for(let entry of tracking.values()) {
      console.log(entry); // 45 // 56 // 82 // 23
  }
  for(let entry of data.values()) {
      console.log(entry); // Andrew // 90 
  }
  
  // keys()
  for(let entry of colors.keys()) {
      console.log(entry); // 0 // 1 // 2
      //*** only values
  }
  for(let entry of tracking.keys()) {
      console.log(entry); // 45 // 56 // 82 // 23
  }
  for(let entry of data.keys()) {
      console.log(entry); // name // age 
  }
  ```

  Destructuring Maps

  ```js
  for(let [key, value] of data.keys()) {
      console.log(`${key} ${value}`); // name Andrew // age 90
  }
  ```

  

- **String Iterators**

  Strings are almost behaving like arrays, we can simply loop through the strings to find the characters.

  ```js
  let str = 'abc';
  console.log(str[1]); // b
  ```

  Most of the **english** character are denoted by1 char and some of **japanese** and **chinese** characters are denoted by 2 char. Some of japanese are represented in 1 char. So while iterating you might not see the actual character, because the it needs both bytecode to signify the character. This can be mitigated by using `for of`

  ```js
  let str = 'ab🚲c';
  for(let i=0;i< str.length;i++){ 
      console.log(str[i]); // a // b // � // � // c
  };
  for(let char of str) { //***
      console.log(char); // a // b // 🚲 // c
  }
  ```

  

- **NodeList Iterators**

  It seems to look like NodeList iterators behave like Arrays but the mechanism is completely different. With ES6 nodeList iteration is simplified.

  ```js
  let divList = document.querySelectorAll('div');
  for(let div of divList) {
  	console.log(div); // <div> </div> // <div> </div> // <div> </div>
  }
  ```

  

- **Spread Operator with Iterables**

  When use spread operator it uses it is default iterator.  If we use **Set**, then it will use `values()` iterator and if you use **Map**, then it will use `entries()` iterator.

  ```js
  let set = new Set([1,2,3]),
  	arr = [...set];
  console.log(arr); // [1,2,3]; //***
  
  let map = new Map([['name', 'Andrew'], ['age', 33]]),
      arr2 = [...map];
  console.log(arr2); // [['name', 'Andrew'], ['age', 33]] //***       
  
  //2
  let bigNumbers = [455, 899, 9000, 333],
      smallNumbers = [33, 44, 55];
  let bigArray = [0, ...bigNumbers, ...map, 33, ...smallNumbers, ...set]; //***
  console.log(bigArray); // [0, 455, 899, 9000, 333, ['name', 'Andrew'], ['age', 33], 33, 33, 44, 55 , 1, 2, 3]
  ```

  

- **Passing Args to Iterators**

  Now we will be able to pass parameters to iterator functions.

  ```js
  function *makeIterator() {
      let first = yield 1;
      let second = yield first + 2;
      yield second + 3;
  }
  
  let iterator = makeIterator();
  console.log(iterator.next()); // {value: 1, done: false} // first yield
  console.log(iterator.next(4));// {value: 6, done: false} // second yield //***
  console.log(iterator.next(5));// {value: 8, done: false} // third yield //***
  console.log(iterator.next());// {value: undefined, done: true}
  ```

  *On the first `next()` the first yield is called and stopped. On the second `next(4)`  variable `first` is assigned with value `4` and the second yield is called and stopped. On the third `next(5)` variable `second` is assigned with `5` and the third yield is called and stopped.  On the forth `nex()`,  since no yield is left, it returns `undefined`*.

  

- **Error Handling**

  ```js
  //1 throw
  function *makeIterator() {
      let first = yield 1;
      let second = yield first + 2;
      yield second + 3;
  }
  
  let iterator = makeIterator();
  console.log(iterator.next()); // {value: 1, done: false} // first yield
  console.log(iterator.next(4));// {value: 6, done: false} // second yield 
  console.log(iterator.throw(new Error('Stop!'))); // Uncaught Error: Stop! //***
  
  //2 try catch
  function *makeIterator() {
      let first = yield 1;
      let second;
      try {
          second = yield first + 2;
      }
      catch {
          second = 6;
      }
      yield second + 3;
  }
  
  let iterator = makeIterator();
  console.log(iterator.next()); // {value: 1, done: false} // first yield
  console.log(iterator.next(4));// {value: 6, done: false} // second yield 
  console.log(iterator.throw(new Error('Stop!'))); //{value: 9, done: false} // No error this time, Error is caught in catch
  console.log(iterator.next(5));// {value: undefined, done: true} 
  ```

  *In the second example the last yield is not `8` because before the last `next(5)` call we are throwing an error and in the **catch** block `second=6 `. So the next yield is `6+3` which is `9`*

  

- **Generator Returns**

  It will be help full if you want to return some values from iterator.

  ```js
  // 1
  function *makeIterator() {
      yield 1;
      return; //***
      yield 2;
  }
  let iterator = makeIterator();
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: undefined, done: true} // Because of return it will never go to second yield
  console.log(iterator.next()); // {value: undefined, done: true} 
  
  //2
  function *makeIterator() {
      yield 1;
      yield 2;
      yield 3;
      return 'final value'; //***
  }
  let iterator = makeIterator();
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: 2, done: false}
  console.log(iterator.next()); // {value: 3, done: false}
  console.log(iterator.next()); // {value: "final value", done: true} 
  ```

  

- **Delegating Generator**

  We can have Generator to delegate to other generators. 

  ```js
  function *makeColorIterator() {
      yield 'red';
      yield 'blue';
  }
  function *makeNumberIterator() {
      yield 1;
      yield 2;
  }
  function *makeCombinedIterator() {
      yield *makeNumberIterator(); //***
      yield *makeColorIterator(); //***
      yield true;
  }
  let iterator = makeCombinedIterator();
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: 2, done: false}
  console.log(iterator.next()); // {value: 'red', done: false}
  console.log(iterator.next()); // {value: 'blue', done: false}
  console.log(iterator.next()); // {value: true, done: false}
  console.log(iterator.next()); // {value: undefined, done: true} 
  
  ```

  Delegating with return example.

  ```js
  function *makeNumberIterator() {
      yield 1;
      yield 2;
      return 3;
  }
  function *makeRepeatingIterator(counter) {
     for(let i =0; i < counter; i++) {
          	yield 'repeat';
       }  
  }
  function *makeCombinedIterator() {
      let result = yield *makeNumberIterator(); 
      yield *makeRepeatingIterator(result);  //***
      yield true;
  }
  let iterator = makeCombinedIterator();
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: 2, done: false}
  console.log(iterator.next()); // {value: 'repeat', done: false}
  console.log(iterator.next()); // {value: 'repeat', done: false}
  console.log(iterator.next()); // {value: 'repeat', done: false}
  console.log(iterator.next()); // {value: true, done: false}
  console.log(iterator.next()); // {value: undefined, done: true} 
  ```

  Asynchronous Task running with Generator example.

  ```js
  function run(taskDef) {
      let task = taskDef();
      let result = task.next();
      console.log(result); // {value: 1, done: false}
      function step() {
          if(!result.done) {
              result = task.next();
              console.log(result); // {value: 2, done: false} // {value: 3, done: false} // {value: undefined, done: true} 
              step();
          }
      }
  }
  
  run(function *() {
      yield 1;
      yield 2;
      yield 3;
  });
  
  //2 //passing value
  
  function run(taskDef) {
      let task = taskDef();
      let result = task.next();
      console.log(result); // {value: 1, done: false}
      function step() {
          if(!result.done) {
              result = task.next(result.value);
              console.log(result); // {value: 3, done: false} // {value: undefined, done: true} 
              step();
          }
      }
  }
  
  run(function *() {
      let value = yield 1;
      value = yield value + 2;
  });
  
  ```

  

## Classes

- **Difference b/w Class & Custom types**

  1. Class declarations, unlike function declarations, are not hoisted. Class declarations act like let declarations and so exist in the temporal dead zone until execution reaches the declaration
  2. All code inside of class declarations run in strict mode automatically.
  3. All methods are non-enumerable.
  4. All methods lack an internal **Construct** method and will throw an error if you try to call them with `new`.
  5. Calling the class constructor without `new` throws an error.
  6. Attempting to overwrite the class name within a class method throws an error.

  ```js
  // ES5 downward
  // Custom types
  function PersonType(name) {
      this.name = name;
  }
  
  PersonType.prototype.sayName = function() {
      console.log(this.name);
  }
  const person = new PersonType('Ricky');
  person.sayName(); // Ricky
  
  // ES6 onward
  // Class
  class PersonClass {
      constructor(firstName) {
          this.name = firstName;
      }
      sayName() {
          console.log(this.name);
      }
  }
  const anotherPerson = new PersonType('Jim');
  anotherPerson.sayName(); // Jim
  ```

  

- **Class Expressions**

  It is same as the class declaration, you can use class expression when you want to pass class to another function.

  ```js
  let PersonClass = class  { //***
      constructor(firstName) {
          this.name = firstName;
      }
      sayName() {
          console.log(this.name);
      }
  }
  const anotherPerson = new PersonType('Jim');
  anotherPerson.sayName(); // Jim
  ```

  *You can also give class name instead of anonymous class, like `let PersonClass = class PersonClass2` you will not be able to access the PersonClass2 from outside but you can access within the class*

  

- **First-Class Classes**

  We can treat something as first-class citizen if we can treat it as **value**. In js, functions are first-class citizen. It means you can pass function as arguments, we can return functions from another function and we can assign functions to variables. We can do the same with classes.

  ```js
  function makeObject(objDef) {
      return new objDef();
  }
  
  const obj = makeObject(class {
      sayHi() {
          console.log('hello');
      }
  })
  obj.sayHi(); // hello
  ```

  

- **Singletons**

  Classes which can be instantiated only once.

  ```js
  const person = new class {
      constructor(firstName) {
          this.name = firstName;
      }
      sayName() {
          console.log(this.name);
      }
  }('Nico');
  person.sayName(); // Nico
  ```

- **Access Properties**

  Getter and setter in class.

  ```html
  <div class="element">
      <span>Andrew</span>
  </div>
  ```

  ```js
  class CustomHtmlElement {
      constructor(element) {
          this.element = element;
      }
      get html() {
          return this.element.innerHTML;
      }
      set html(value) {
          this.element.innerHTML = value;
      } 
  }
  let div = document.querySelector('.element');
  const customeEl = new CustomHTMLElement(div);
  console.log(customeEl.html); // <span>Andrew</span>
  
  const list = `
  <ul>
  	<li>Hello</li>
  	<li>Hello</li>
  </ul>`;
  customEl.html = list;
  console.log(customeEl.html); //<ul> <li>Hello</li><li>Hello</li> </ul>
  ```

- **Computed Names**

  You can give computed names for methods, getter and setter.

  ```js
  let name = 'sayName';
  class PersonClass {
      constructor(firstName) {
          this.name = firstName;
      }
      [name]() { //***
          console.log(this.name);
      }
  }
  
  //2
  let name2 = 'html' ;
  class CustomHtmlElement {
      constructor(element) {
          this.element = element;
      }
      get [name2]() { //***
          return this.element.innerHTML;
      }
      set [name2](value) { //***
          this.element.innerHTML = value;
      } 
  }
  ```

  

- **Generator Class Method**

  It will be useful if you have collection like an object to represent values, array, sets and maps have built-in iterator. Object don't have built-in iterators

  ```js
  class GenClass {
       *makeIterator() {
           yield 1;
           yield 2;
           yield 3;
       }
  }
  const instance = new GenClass();
  const iterator = instance.makeIterator();
  console.log(iterator.next()); // {value:1, done: false}
  ```

  

- **Default Iterators**

  ```js
  class Collection {
  	constructor() {
          this.values = [];
      }
      *[Symbol.itertor]() { //***
          yield *this.values;
      }
  }
  
  const collection = new Collection();
  collection.values.push(1);
  collection.values.push(2);
  
  for(let item of collection) {
      console.log(item); // 1 // 2
  }
  ```

  

- **Static methods**

  It will be useful for Utility functions, you don't have to instantiate to call this functions.

  ```js
  // ES5
  function Calculate() {
      
  }
  Calculate.add = function(val1, val2) {
      console.log(val1 + val2);
  }
  Calculate.add(1,2); //3
  
  //ES6
  class Calculate {
      constructor {
          
      }
  	static area(length, width) { //***
          console.log(lengh * width);
      }
  }
  
  Calculate.area(2, 4); // 8
  ```

   
  
- **Inheritance**

  It will help in the re-use of the code. The derived class will inherit all the properties of parent class.

  *Parent class **constructor** will be called automatically if you don't define **constructor** in the child/derived class. If you define **constructor** is child class you need call `super()` before performing anything.*

  ```js
  class Person {
      constructor(strength, speed, intelligence) {
          this.strength = strength;
          this.speed = speed;
          this.intelligence = intelligence;
      }
  }
  
  class Wrestler extends Person {
      constructor(strength, speed, intelligence, grappling, takedowns) {
          super(strength, speed, intelligence); //***
          this.grappling = grappling;
          this.takedowns = takedowns;
      }
  }
  
  const wrestler = new Wrestler(23, 45, 67, 87, 29);
  console.log(wrestler); // {strength: 23, speed: 45, intelligence: 67, grappling: 87, takedowns: 29}
  ```

  

- **Shadow Methods**

  Derived class methods can shadow the methods of parent class methods. It looks for methods in prototypic chain.

  

- **Inherit Methods**

  All methods will be inherited, even the **static** methods.

  ```js
  class Rectangle {
      constructor(length, width) {
          this.length = length;
          this.width = width;
      }
      getArea() {
          console.log(this.length * this.width);
      }
      static create(length, width) {
          return new Rectangle(length, width);
      }
  }
  
  class Square extends Rectangle {
      getArea() { // Shadow method
          //console.log(`The area is ${this.length * this.width}`);
          super.getArea(); // you can call parent function using super. If you don't define the method with same name, it will call the next function which is parent function in the prototypic chain.
      }
  }
  
  const square = new Square(4, 4);
  const rectangle = new Square.create(2, 2);//*** // Static method is inherited
  console.log(rectangle); // Rectangle {length: 2, width: 2}
  console.log(square); // Square {length: 4, width: 4}
  square.getArea(); // 16
  ```

  

- **Derived Classes from Expressions**

  We can derive classes from expressions.

  ```js
  function Rectangle() { //***
          this.length = length;
          this.width = width;
  }
  
  Rectangle.prototype.getArea = function() {
      return this.width * this.length;
  }
  
  class Square extends Rectangle { //***
      constructor(length){
          super(length, length);
      }
  }
  
  const square = new Square(4);
  console.log(square); // Square {length: 4, width: 4}
  ```

  

- **Dynamic Class Bases**

  Deriving from expression opens up lot more options.

  ```js
  function Rectangle() { 
          this.length = length;
          this.width = width;
  }
  
  Rectangle.prototype.getArea = function() {
      return this.width * this.length;
  }
  
  function getBase() { //***
      return Rectangle; 
  }
  
  class Square extends getBase() { 
      constructor(length){
          super(length, length);
      }
  }
  
  const square = new Square(4);
  console.log(square); // Square {length: 4, width: 4}
  ```

  

- **Inheriting from Built-in types**

  We can inherit the built-in types. We can create custom classes with all the capability of the built-in types and add our custom capability to it.

  Built-in types

  1. Array
  2. ArrayBuffer
  3. Map
  4. Promise
  5. RegExp
  6. Set
  7. Typed Arrays

  ```js
  class MyArray extends Array {
      //empty
  }
  
  const arr = new MyArray();
  arr[0] = 1;
  console.log(arr[0]); // 1
  console.log(arr.length); // 1
  arr.length = 0;
  console.log(arr[0]); // undefined
  ```

  

- **Symbol.species**

  Symbol.species is used to define a static accessor property that returns a function.

  ```js
  // 1
  class MyClass {
      static get [Symbol.species]() {
          return this;
      }
      constructor(value) {
          this.value = value;
      }
      clone() {
          // this.constructor[Symbol.species] = returns class
          return new this.constructor[Symbol.species](this.value);
      }
  }
  
  //2
  class MyArray extends Array {
      //empty
  }
  
  let arr = new Myarray(1,2,3,4),
      arr2 = arr.splice(1, 3);
  
  console.log(arr instanceof MyArray); //*** // true
  console.log(arr2 instanceof MyArray); //*** // true
  
  ```

  Overriding `Symbol.species` : We can override the instance, we can change whatever we what it to be.

  ```js
  class MyClass {
      static get [Symbol.species]() {
          return this;
      }
      constructor(value) {
          this.value = value;
      }
      clone() {
          // this.constructor[Symbol.species] = returns class
          return new this.constructor[Symbol.species](this.value);
      }
  }
  
  class DerivedClass1 extends MyClass {
      
  }
  
  class DerivedClass2 extends MyClass {
      static get [Symbol.species]() { //***
          return MyClass;
      }
  }
  
  const instance1 = new DerivedClass1(1),
        clone1 = instance1.clone('clone1'),
        instance2 = new DerivedClass2(1),
        clone2 = instance2.clone('clone2');
  
  console.log(clone1 instanceof MyClass); // true
  console.log(clone1 instanceof DerivedClass1); // true
  console.log(clone2 instanceof MyClass); // true
  console.log(clone2 instanceof DerivedClass2); // false
  ```

  We can override in built-in types too..

  ```js
  class MyArray extends Array {
     static get [Symbol.species]() { //***
          return Array;
      }
  }
  
  let arr = new Myarray(1,2,3,4),
      arr2 = arr.splice(1, 3);
  
  console.log(arr instanceof MyArray); //*** // true
  console.log(arr2 instanceof MyArray); //*** // false
  ```

- **new.target**

  ```js
  class Rectangle {
      constructor(length, width) {
          console.log(new.target === Rectangle); //***
          this.length = length;
          this.width = width;
      }
  }
  const rect = new Rectangle(4,4); // true
  
  class Square extends Rectangle {
      constructor(length) {
          super(length, length);
      }
  }
  
  const sqr = new Square(2); // false
  ```

  

## Arrays

- **Creating Arrays**

  Arrays can be created using the below ways:

  1. Array.of(1, 2, 3)
  2. [1, 2, 3]
  3. new Array(1, 2, 3);

  There is a quirky behavior in creating the array with option 2 and option 3, when the array is created with one number it shows the length as that number. This can be solved if create arrays using `Array.of()`

  ```js
  // Quirky behavior
  let arr1 = new Array(1, 2),
      arr2 = new Array(5), //*** //Quirky behavior
      arr3 = new Array("2");
  console.log(arr1.length); // 1
  console.log(arr2.length); //*** // 5
  console.log(arr3.length); // 1
  
  // Solved using Array.of()
  let arr1 = Array.of(1, 2),
      arr2 = Array.of(5), //***
      arr3 = Array.of("2");
  console.log(arr1.length); // 1
  console.log(arr2.length); // 1
  console.log(arr3.length); // 1
  
  ```

  

- **Transforming Array-like objects to Array**

  ```js
  function foo() {
      const arr1 = Array.prototype.splice.call(arguments); // old way
      const arr2 = Array.from(arguments); // new way
      console.log(arr1); // [1,2]
      console.log(arr2); // [1, 2]
      
      //manipulation while convering to array
      const arr3 = Array.from(arguments, (value) => value + 2); // new way
      console.log(arr3); // [3, 4]
  }
  
  foo(1, 2);
  ```

  We can transform the object with iterables also.

  

- **Methods**

  1. find - Returns the value
  2. findIndex - Returns the index
  3. fill(value, startIndex, endIndex) - Fills the value in the array.
  4. copyWithin - It takes the value within the array and fills the values of the array.

  ```js
  //1
  const numbers = [23, 45, 67, 89];
  const result = numbers.find(function(number, i ,arr) { //***
      return number > 70;
  });
  console.log(result); // 89
  //2
  const resultIndex = numbers.findIndex(function(number, i ,arr) { //***
      return number > 70;
  });
  console.log(resultIndex); // 3
  //3
  numbers.fill(1); 
  console.log(numbers); // [1,1,1,1]
  
  numbers = [23, 45, 67, 89];
  numbers.fill(1, 2); 
  console.log(numbers); // [23, 45, 1, 1]
  
  numbers = [23, 45, 67, 89];
  numbers.fill(1, 1, 3); 
  console.log(numbers); // [23, 1, 1, 89]
  
  // 4
  numbers = [23, 45, 67, 89];
  console.log(numbers); //[23, 45, 67, 89]
  //parameters
  //where to start pasting
  //where to start copying
  //where to stop copying
  numbers.copyWithin(2, 0);
  console.log(numbers); //[23, 45, 23, 45]
  
  numbers = [1, 2, 3, 4];
  console.log(numbers); //[1, 2, 3, 4]
  numbers.copyWithin(2, 0, 1);
  console.log(numbers); //[1, 2, 1, 4]
  ```

  