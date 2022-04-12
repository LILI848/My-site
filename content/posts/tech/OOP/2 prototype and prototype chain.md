---
title: "[OOP_2] The prototype in JS"
date: 2022-04-10T04:40:01+01:00
draft: false
tags: ["jsscript", "OOP"]
categories: ["技术随感 tech"]
summary: Class notes of FrontendMaster's  course- OOP JS part 2
---

# What is prototype?

1. Create two objects which are unlinked:

```js
const functionStore={
increment: function() {this.score++;}
login: function(){console.log("You are loggedin")}
}

const user1={
name:"Phil";
socre:4;
}

user1.name;//"Phil"
user1.increment// Error!
```

2. Then make the link with Object.create();

```js
const user1 = Object.create(functionStore);
user1; // { }
```

3. solution:

```Js
const functionStore={
increment: function() {this.score++;},
login: function(){console.log("You are loggedin")}
}
const user1=Object.create(functionStore);
user1.name="Phil";
user1.score=4;
user1.increment();
```

4. map it out
   ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/4.jpg?raw=true)

# **proto**

- Creating objects using functions, without copying functions in every object repeatedly;

```js
function userCreator(name, score) {
  const newUser = Object.create(userFunctionStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}
const userFunctionStore = {
  increment: function () {
    this.score++;
  },
  login: function () {
    console.log("You are loggedin");
  },
};

const user1 = userCreator("Phil", 4);
const user2 = userCreator("Julia", 5);
user1.increment();
```

- verbalize the steps:

```js
function userCreator(name,score){
// 1.create a new function stored in memory
//parameters which are name and score, in order to receive arguments(the actual values we pass in later)
//6. create a local execution context;
//7. in the local memory, the first parameter is assigned the argument "Phil";
//8. pass in the second argument 4;

const newUser= Object.create(userFunctionStore);
//9. whatever pass in, it would be an empty object first;
//now this object has a bond to the userFunctionStore in the global memory;
//add a hidden property underscore underscore proto (__proto__) to this object;


newUser.name=name;
newUser.score=score;
//10. add property of name contains value of the name parameter; same as score property;

return newUser;

//11. return the object out without the name(just the value of the newUser with no name) into the global constant; or,  assign this object to a new global label called user1;
};


const userFunctionStore={
//2. declare a constant stored two properties;

increment: function(){this.score++;}
//3. first property is a function; in orther words, the object has a method called increment on it;

login:function(){console.log("You are loggedin");}
//4. second property is a function; or, the object has a method called login on it;
};

const user1= userCreator("Phil", 4);
//5. create a new constant called user1 stored the return value of calling the function userCreator with inputs "Phil" and 4;

const user2= userCreator("Julia",5);
//12. the return value of calling the function; //an object with a property name which would be "Julia" + a property score which would be 5 + a hidden bond to userFunctionStore;

user1.increment();
// 13. find th user1 in the global memory; not find the increment method in the user1 object;
// then find the hidden bond to userFunctionStore, find the increment method in the object;
```

- map it out
  ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/3.jpg?raw=true)

# How to make this process more efficient?

- Using new keyword to automate our manual work;
- call the constructor function with new, we automate 2 things:

  1. create a new user obj;
  2. return a new user obj;
     ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/6.png?raw=true)
     - map it out
       ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/7.jpg?raw=true)

  **Note: the userCreator.prototype is a hidden property of userCreator called prototype.**

# How does the new keyword work under the hood?

### Function-object combos

- meaning: all functions are both objects and functions;
- example:

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/8.png?raw=true)

- we can access the object bit of a function and access the function bit at the same time, add property to the function does not overwrite it to be only object.
- We can use a dot notation to access its object bit in its function-object combo.
- prototype is a regular property of the function in its object form/ on a function in its object format, which is automatically an object.

### Example using new and this

```js
function UserCreator(name, score){
this.name=name;
this.score=score;
}

UserCreator.prototype.increment={
this.score++;
}
UserCreator.prototype.login={
console.log("login");
}

const user1= new UserCreator("Eva", 9);
user1.increment();
```

- verbalize the code in detail:

```js
function UserCreator(name, score){
//1. create a new function with two parameters
//*true story: UserCreator is declared as a function-object combo
//and this object has by default a property called prototype which is an object automatically;
//the function bit can be accessed using parentheses in order to run the function;
// and the object bit can be accessed using dot notation;
// this object is used to store the single version of each of the functions we want;

//5.create local context and assign two arguements to the parameters;
//*then what the new key do automatically:
//-create an empty object and assign it to the label `this` in the local memory;
//-add a hidden property called proto, which has a bond to prototype/which is a reference to the prototype object/ a link to shared functions;

this.name=name;
this.score=score;
//6. find this, this is an auto created object,
//and assign the arguements to the property name and score;

//7. the new keyword auto-return the object,
// or, return the object with properties name and score out into the global label `user1` with a hidden bond to the prototype of UserCreator;

}

UserCreator.prototype.increment={
//2. JS looks for the UserCreator object first,
// then the property called prototype on the object form of this function, or this object-function combo,
//find the prototype property which is an object,
//and assign increment to this object, which is a method(function);


this.score++;
}


UserCreator.prototype.login={
//3. add a property in the prototype property on the function in its object format;

console.log("login");
}

const user1= new UserCreator("Eva", 9);
//4. declare a constant called user1 and the user1 is uninitialized for now,
//it gonna store the returning value of function called UserCreator with two inputs,
// while calling the UserCreator function, the new keyword mutates the execution context of the function,
//this context is gonna have a ton of stuff done automatically inside of it;



user1.increment();
//8. finally, the data and the functionality bundled together;
//9. find the user1 in global memory,
//but not find the property increment in user1 itself;
//10. look up in the proto property, which is a link to the prototype object on the UserCreator object(or function-object combo),
// then find the increment method in the prototype property of UserCreator;


```

- map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/9.jpg?raw=true)

# `this` keyword scoping issues

Define a new inner function to organize the code further- using traditional function expression or declaration.
![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/10.png?raw=true)

- this is WRONG
- this.score = window.score.

### Solution:

- using arrow functions to fix it

  ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/11.png?raw=true)

# Problem explanation

### `this`: an implicit parameter

- `this`: allow us to refer to the pertinent object at hand;

- how to call the prototype method

```js
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increment = function () {
  this.score++;
};
UserCreator.prototype.login = function () {
  console.log("You are loggedin!");
};

const user1 = new UserCreator("Eva", 9);
const user20 = new UserCreator("Jeff", 8);

user1.increment();
user20.increment();
```

- map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/12-.png?raw=true)

## Rule of `this`

- a different `this`: **auto-created object**.
  When we create the user1 with the keyword `new`, the object was auto-created, and it got the label `this`, and then the object was returned out into user1, and `this`was gone, cause its execution context was deleted;
- rule of using `this`: **the object at hand**.
  Whenever I call a method, a function on an object, or I call a function to the right-hand side of the dot, as soon as that function's execution context opens, `this` will automatically point to the object on the left-hand side of the dot.
  - `this ` , known as the implicit parameter in the local context, will be assigned to its implicit argument, meaning the object on the left-hand side of the dot.

## Define a new inner function

- the traditional way: using function expression or function declaration;
- neither of which can give us the right `this` assignment inside (this way `this` points to window).

```js
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increment = function () {
  function add1() {
    this.score++;
  }
  // const add1= function(){this.score++;}

  //函数表达式等号赋值给变量，函数声明没有。两者区别是函数声明会提升作用域，函数表达式需要等赋值完毕才能调用。

  add1();
};

UserCreator.prototype.login = function () {
  console.log("You are loggedin!");
};

const user1 = new UserCreator("Eva", 9);

user1.increment();
```

- map it out
  ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/13.png?raw=true)

# Solution explanation

### Arrow function: a new way of declaring functions

- Solving the bug in OOP: calling functions inside a method and the `this` inside refers to nothing we wanted, `this` being miss assigned.

- benefits of Arrows: `this` assignment can be automatically static or lexical scoped, which means `this` can automatically refers to the `this` from where it was born (where it is saved in the local memory/ at hand).

## With arrow functions- bind `this` lexically

```js
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increment = function () {
  const add1 = () => {
    this.score++;
  };
  add1();
};

UserCreator.prototype.login = function () {
  console.log("login");
};

const user1 = new UserCreator("Eva", 9);

user1.increment();
```

## verbalize the code

- The first thing in the local execution context is assigning the implicit parameter `this`;
- `this` is assigned to what `this` was when function `add1` was defined, meaning `add1` was saved as a label in memory for that function code with hidden stuff in it, including a hidden property saying when `add1` is called, `this` assignment is to the `this` when `add1 `was saved.

- Lexical static scoping: where I was born, where I am saved, determines sth about me when I get called, like `this` assignment.
- lexical means positioning on the page, where I was positioned.

## map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/14.png?raw=true)
