---
title: "[OOP_4] factory function & sub factory"
date: 2022-04-10T06:40:01+01:00
draft: false
tags: ["jsscript", "OOP"]
categories: ["技术随感 tech"]
summary: Class notes of FrontendMaster's course- OOP JS part4
---

# Subclassing: inheritance

- meaning: passing knowledge or work that's been previously written down
- principle: Function.prototype and Array.prototype

- example of passing it down

  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/19.png?raw=true)

- base code: function expression+arrow functions

```js
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increaseScore = function () {
  const add1 = () => {
    this.score++;
  };
  add1();
};

const user1 = new UserCreator("Will", 3);
user1.increaseScore();
```

- base code: using class keyword

```js
class userCreator {
constructor (name, score){
this.name=name;
this.score=score;
}
}
increaseScore (){
this.score++;
}

const user1=new userCreator("Will",3);
user1.increaseScore();
```

# Create object with factory function

- factory function: like userCreator; because it produces new functions alike.

### Challenge:

- First create general users, then create a more specific type of users: paid users, and paid users have a new shared function, increase balance, which allows them to increase balance;

### Solution - 1

- without using new keyword, we create a function and its functionality, and then bond it using Object.create(). Making it have a bond to shared functions.
- creating this base class first, and then its subclass.

![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/21.png?raw=true)

```js
function userCreator(name, score) {
  //two parameters, local parameter-argument combos/ paring up;

  const newUser = Object.create(userFunctions);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}

userFunctions = {
  sayName: function () {
    console.log("I'm " + this.name);
  },
  increment: function () {
    this.score++;
  },
};
const user1 = userCreator("Phil", 5);
//don't say variable user1, just say create a new constant user1, which is uninitialized for now;

user1.sayName();

function paidUserCreator(paidName, paidScore, accountBalance) {
  const newPaidUser = userFunctions(paidName, paidScore);

  //bond master version of functions

  Object.setPrototypeOf(newPaidUser, paidUserFunctions);

  //bond new version of shared functions

  newPaidUser.accountBalance = accountBalance;

  //add new property on the object

  return newPaidUser;
}

const paidUserFunctions = {
  // add new shared functions

  increaseBalance: function () {
    this.accountBalance++;
  },
};

Object.setPrototypeOf(paidUserFunctions, userFunctions);

//setPrototypeOf(obj, prototype);
//meaning set the obj's prototype to a new object,setting the property proto to find its prototype's address and change it.
//setPrototypeOf is actually changing the __proto__ property;
//bond new shared functions with old shared functions;
//linking the new to the old functions is called sub-factory functions;

const paidUser1 = paidUserCreator("Alyssa", 8, 25);

paidUser1.increaseBalance();
paidUser1.sayName();
```

![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/20.png?raw=true)

- Solution principle: first set a master version of data+functionality bond; then create a new version of data+functionality bond;
  in this new version, first create a factory function, producing 1)the same old version of object, 2) link this old version to the new shared functions, 3)add new properties, 4)return resulting object out;
  the new shared functions should be declared as an object, and link this object to the old version using prototype chain.

- OOP: first a master version of sth, a master class, then more specific versions.

# map it out

![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/22.png?raw=true)

- This is how the new keyword working under the hood with subclassing. Without the help of new and class automating a bunch of work, this is the actual process of how it works in villina JS.

# call+apply

- approach to executing a function which allows us to control the assignment of `this`

```js
const obj = {
  num: 3,
  increment: function () {
    this.num++;
  },
};

const otherObj = {
  num: 10,
};
obj.increment();

obj.increment.call(otherObj);

//when I run a function using the call approach,
//the assignment of `this` would be the argument that is passed into it.
```

- map it out
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/23.png?raw=true)

- `this`:
  1.  just automatically refer to the object left-side of the dot;
  2.  with the arrow functions, it refers to whatever this was where the function was defined;
  3.  with apply/call, we can manually take control what the `this` refers to;

# subclass: with new keyword

- First build out our base class which produces other subclasses;
- Then using `call` to subclass;
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/24.png?raw=true)

- verbalize the base class

```js
function UserCreator(name, score) {
  //the name parameter is locally available inside a userCreator as the name argument;
  //the score parameter is locally available inside a userCreator as the score arguement;

  this.name = name;

  this.score = score;
}

UserCreator.prototype.sayName = function () {
  console.log("I'm " + this.name);
};
UserCreator.prototype.increment = function () {
  this.score++;
};

const user1 = new UserCreator("Phil", 5);
const user2 = new userCreator("Tim", 4);
user1.sayName();
```

- map it out
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/25.jpg?raw=true)

# subclass with constructor+call

![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/24.png?raw=true)

- base class

```js
function UserCreator(name, score) {
  this.name = name;

  this.score = score;
}

UserCreator.prototype.sayName = function () {
  console.log("I'm " + this.name);
};
UserCreator.prototype.increment = function () {
  this.score++;
};

const user1 = new UserCreator("Phil", 5);
const user2 = new userCreator("Tim", 4);
user1.sayName();
```

- subclass: paid user

```js
fucntion paidUserCreator(paidName, paidScore, accountBalance){
UserCreator.call(this, paidName, paidScore);
//Normally, `this` inside UserCreator is set to the new auto created object using `new` keyword.
//But here we control it with passing in `this`,
//we refer to `this` object one layer out.
//we want `this` in the local memory of userCreator to refer to the object one layer out
//which is our auto created object, = paidUser1,
//which is the return value of running paidUserCreator with the help of a `new `keyword.
// the value of `this` inside UserCreator  is gonna be the value of `this` outside/one layer out.

//userCreator.call() has no return it out because no `new` keyword's auto-return.
//No need to return it out, because `this` inside =`this` outside/ one layer out,
//we refer to the object one layer out,
//so `this.name="Alyssa"` is automatically add property `name` and `"Alyssa"` to `this` object outside.
//this is called pseudo-return, return like a side-effect.

//Also can use: UserCreator.apply(this, [paidName, paidScore ])

// Finally, the execution context of userCreator.call() is gone.

 this.accountBalance=accountBalance;
}

paidUserCreator.prototype= Object.create(userCreator.prototype);
// Object.create() returns out a brand new object
//it overwrites the old object completely,
//and now it has the proto property set to userCreator.prototype.

//Another way: //Object.setPrototypeOf(paidUserCreator.prototype, UserCreator.prototype);

paidUserCreator.prototype.increaseBalance=function(){
this.accountBalance++;
}

const paidUser1= new paidUserCreator("Alyssa", 8, 25);


paidUser1.increaseBalance();
paidUser1.sayName();

// we do our lookup check using the prototype chain.
```

- map it out
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/25.png?raw=true)

- try it out

```js
function userCreator(name, score) {
  this.name = name;
  this.score = score;
}

userCreator.prototype.increment = function () {
  this.score++;
};

const user1 = new userCreator("Phil", 5);

user1.increment();

function paidUserCreator(paidName, paidScore, accountBalance) {
  userCreator.call(this, paidName, paidScore);
  this.accountBalance = accountBalance;
}

paidUserCreator.prototype.increaseBalance = function () {
  this.accountBalance++;
};

paidUserCreator.prototype = Object.create(userCreator.prototype);

const paidUser1 = new paidUserCreator("Allysa", 8, 25);

paidUser1.increment();
```

# Class: subclass & extend

- class: the way JS can emulate an object orient language within a prototypal environment.
- class VS constructor approach
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/28.png?raw=true)
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/27.png?raw=true)

### what `extends` can do:

1. set **proto** in the prototype object of paidUserCreator: as a reference up to the master version of class's prototype object.(in this case, userCreator.prototype)
2. set **proto** in the paidUserCreator itself: as a reference up to the master class itself, which in this case, is the userCreator function+object combo./ Normally this proto property is just linking up to the ultimate Function.prototype where all the shared functions/ whatever each function has access to will be there.

### what `super` can do:

1. call super inside, it is heading to proto link in the subclass, and look up userCreator class combo.
2. super = let's go run userCreator() with new keyword, creating a new object inside the userCreator, instead of paidUserCreator;

- code in full
  ![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/26.png?raw=true)

- verbalize the code

```js
class userCreator {
  constructor(name, score) {
    this.name = name;
    this.score = score;
  }
  sayName() {
    console.log("I'm " + this.name);
  }
  increment() {
    this.score++;
  }
}

const user1 = new userCreator("Phil", 4);
user1.sayName;

class paidUserCreator extends userCreator {
  //1. extends will add 1)proto in prototype object; 2)proto in itself;

  //We've set up our whole paidUser class with an extension of userCreator;

  constructor(paidName, paidScore, accountBalance) {
    //2. add parameter+argument combos;
    //3. add new keyword stuff, but with extends, only one, this;
    //new will add a `this`, but uninitialized;
    //the object will be born in userCreator constructor function, instead of paidUserCreator;
    //and it is auto-return out and assigned to the `this` keyword in paidUserCreator;
    //4. after `this`, immediately run super function;

    super(paidName, paidScore);
    //5. this=super();
    //super("Alyssa", 8)= new userCreator("Alyssa", 8);

    //this=super() because of `extends`.
    //using reflect.construct();
    // the first paramete is the function that's creating the object;
    //look up the proto property and find the userCreator class,
    //the second paramete is "Alyssa",
    //the third is the returned object that comes out of super pointing to; which is paidUserCreator;
    //so super is set to userCreator;

    //super = let's go run userCreator() with new keyword;
    //it is like super() means userCreator();
    //super("Alyssa", 8)= new userCreator("Alyssa", 8);

    // this=super()=new userCreator("Alyssa", 8);;

    // 6. the proto should link to userCreator.prototype, but it is not
    // it is overwritted by `super`;
    // super sets this object's proto to paidUserCreator.prototype object;
    // because of reflect.construct() pass those parameters in and change the returned object proto to other object;

    this.accountBalance = accountBalance;
  }

  increaseBalance() {
    this.accountBalance++;
  }
}

const paidUser1 = new paidUserCreator("Alyssa", 8, 25);

paidUser1.sayName();
```

- map it out

![OOP4](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/29.png?raw=true)
