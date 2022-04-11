---
title: "[OOP_2] The prototype chain"
date: 2022-04-10T04:40:01+01:00
draft: false
tags: ["javascript", "OOP"]
categories: ["技术随感 tech"]
summary: to solve the problem in creating an object
---

# What is prototype?

- 1. Create two objects which are unlinked:

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

- 2. Then make the link with Object.create();

```js
const user1 = Object.create(functionStore);
user1; // { }
```

- 3. solution:

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

- 4. map it out
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
8. pass in the second argument 4;

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

  ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/5.jpg?raw=true)

# How to make this process more efficient?

- Using new keyword to automate our manual work;
- call the constructor function with new, we automate 2 things:

  1. create a new user obj;
  2. return a new user obj;
     ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/6.jpg?raw=true)
     - map it out
       ![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/7.jpg?raw=true)

  **Note: the userCreator.prototyp is a hidden property of userCreator called prototype.**

# How does the new keyword work under the hood?
