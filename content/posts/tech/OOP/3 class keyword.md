---
title: "[OOP_3] class keyword & prototype chain"
date: 2022-04-10T05:40:01+01:00
draft: false
tags: ["jsscript", "OOP"]
categories: ["技术随感 tech"]
summary: Class notes of FrontendMaster's course- OOP JS part3
---

# class

- rename the function-object combo with a prototype property

```js
class UserCreator {
  constructor(name, score) {
    this.name = name;
    this.score = score;
  }
  increment() {
    this.score++;
  }
  login() {
    console.log("login");
  }
}

const user1 = new UserCreator("Eva", 9);
user1.increment();
```

## prototype VS class

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/15.png?raw=true)

- map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/16.jpg?raw=true)

- JS is not a traditional object-oriented language, it is actually a prototypal language faking it.
- Implemention of class is just using proto bond in its nature. All objects by default have proto property.
- using proto link to give objects/functions/arrays bonus functionality.

# proto - storing functions

- all objects have this property which links to a place storing functions

- example code

```Js
const obj={
num:3
}

obj.num;
// look up obj/reference obj/ look a memory for obj
// and we find it
// and it has a property called num
// and its value is 3

obj.hasOwnProperty("num");
//it is a function that takes in an argument, the string num,
//first look for obj in the global memory,
//second look for the function called hasOwnProperty on the obj,


Object.prototype
```

- map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/17.jpg?raw=true)

# Function = a function+object combo with a prototype property

- example code

```java
function multiplyBy2(num){
return num* 2
}

multiplyBy2.toString();
//first look up the function in the global memory,
//then look up its property and not find toString,
//then look up to __proto__ property and find Function.prototype,
//remember, functions are looking up to Function. prototype first, in order to get bonus properties, including toString;


multiplyBy2.hasOwnProperty("score");
//first look up the function in the global memory and find it
//then look up hasOwnProperty on the object bit, but not find it,
//then look up to Function.prototype, but not find it
//then using Function.prototype.__proto__ in order to look up to Object.prototype, and find it.
//now __proto__ is a private variable and not exposed by default. Using Object.getPrototypeof insead.
```

- map it out

![OOP2](https://github.com/LILI848/My-site/blob/master/content/posts/tech/OOP/OOP_JS_img/18.png?raw=true)

- **prototype chain ends at:Object.proto**

- Object.proto points to null
