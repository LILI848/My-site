---
title: "[OOP_1] Creating an Object"
date: 2022-04-10T04:38:01+01:00
draft: false
tags: ["javascript", "OOP"]
categories: ["技术随感 tech"]
summary: an approach to orgnize our code
---

# Why OOP?

### How to structure complex data

- using a paradigm(an organizing structure) to:

1. saving data;
2. apply functionality to _use/change/render(display)_ the data;
3. combine the data to its functionalities; bundled together;

### The goal of a paradigm

1.  make it easy to add features and functionality;
2.  easy for developers to reason about;
3.  no cost of performance (efficient in terms of memory);

### How to do that?

- Just put data in an object.

# \* Three ways to create obj

# Method 1 - creating an object literal

![OOP1](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/1-obj.png?raw=true)

- key: bundle the data and its functionality using an object.

```Js
const user1={
name: "Phil";//注意引号
score: 4;
increment: function(){
user1.score++; //注意dot
}
}

user1.increment();
```

# Method 2 - assign properties to an object literal

1. create an empty obj
2. assign properties to this obj using dot notation

```Js
const user2={}; // first create an empty object
user2.name="Julia"; // assign properties to that object
user2.score=5;
user2.increment=function(){
user2.score++;
}
```

- verbalize the code and map it out:

```Js
const user2={};
// first creating a constant called user2 in memory, which is an object literal;
// to which we are gonna stick some properties;

user2.name="Julia";
// assign properties to the object;
// first create a name property with the value of "Julia";

user2.score=5;
// and then create a score property with a value of 5;

user2.increment=function(){
// attach the pertinent (relevant) functionality which we wanna hava available on the obj;
// which is the increment function;

user2.score++;
}

```

- what is constant?

  we can't switch the object out, which means we can change properties, but we can't get rid of the whole object from memory cause its a constant/ it got the same position in memory.

# Method 3 - Object.create() with a bonus feature

- when coding is getting repetitive, use Object.creat(); whatever we pass to that, it returns out an empty object;
- the bonus feature depends on what we pass to it (other than null);

```js
const user3 = Object.create(null);

user3.name = "Eva";
user3.score = 9;
user3.increment = function () {
  user3.score++;
};
```

# How to generate objects using a function

```js
function userCreation(name, score) {
  const newUser = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function () {
    newUser.socre++;
  };
  return newUser;
}

const user1 = userCreation("Phil", 4);
const user2 = userCreation("Julia", 5);
user1.increment();
```

- verbalize the code:

```js
function userCreation(name, score) {
  //1.declare a function called userCreation and save it in memory ;
  //4. call the function;
  //5. create an execution context to run the function's code;
  // this context which means we are no longer running code outside the function + a mini-memory to save the stuff gets declared inside(local memory);
  //6. the two parameters;

  const newUser = {};
  //7. create a const called newUser which is an empty object(object literal);

  newUser.name = name;
  //8. we are dynamically assigning a name property to the const newUser;
  //9. the argument phil is passed in;
  // the name is the parameter, and the argument is the one we pass in;

  newUser.score = score;
  //10. the argument 4 is passed in/ assigned to the score property;

  newUser.increment = function () {
    // 11. we add a property called increment and declare it as a function;
    // this is known as method in an object;

    newUser.socre++;
  };
  return newUser;
  //12. we need to return it out of the function + assign it to user1 in the global execution context;
}

const user1 = userCreation("Phil", 4);
//2. create a new constant called user1;
//3. it stores the return value of calling userCreation;
//note: the const label remains uninitialized/not declared until we get the return value;

const user2 = userCreation("Julia", 5);

//13. create a new constant called user2 which stores the retun value of calling userCreation;

user1.increment();
//14.find the user1 in the global memory, and find the increment function stored in the global memory;
```

## map it out:

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/2.png?raw=true)

**note: the two parameters should be the first thing stored in the local memory and then the newUser object.**

## Problem:

- This is untenable so never do it in practice:
- The increment method is stored individually on each object(user1,user2...user100); It stored this function individually and repeatedly, making it unefficient when there are so many different functions stored in our computer's memory.

## solution

- We only need one copy of increment method, and JS can store it somewhere else, in another object.
- The prototype chain.
