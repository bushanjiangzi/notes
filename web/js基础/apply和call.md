## this指向
  - [this, call, apply 和 bind](https://juejin.cn/post/6844903843512205326)

  - this对象分类：全局对象，当前对象，任意对象

  - 判断处于哪种this对象，这个取决于函数的调用方式。而JavaScript中的函数调用方式又分为四种：函数调用，作为对象的方法调用，构造函数调用，call 、apply 、bind

    1. 函数调用：在独立调用中，this在非严格模式下指向全局对象window.在严格模式下this是 undefined。在这里，用var声明一个变量和给this以及window添加属性是等价的。被(…)();这样形式(IIFE**立即调用的函数表达式**)封装的函数会立即被调用，也就是说等同于被独立调用，因此它内部的this是全局变量，所以输出的是全局变量。
    ```
    var a = 11;
    function foo() {
      var a = 22;
      console.log(this.a);//11
    }
    foo();
    console.log(this.a);//11

    function foo() {
      console.log( a ); 
    }
    function bar() {
      var a = 3;
      foo();
    }
    var a = 2;
    bar();//2

    var myname = "princess";
    console.log(this.myname); //princess
    console.log(window.myname); //princess

    // delelte删除不了变量、删除不了原型链中的变量、可以删掉对象属性！
    var foo = 123;//foo 是变量
    window.bar = 345;//bar 是属性
    delete foo;
    delete bar;
    console.log(this.foo,this.bar);//123,undefined
    ```

    2. 作为对象的方法调用：this指向该对象(这里即对象o)
    ```
    var b = 33;
    var o = {
      b: 44,
      f: function() {
        return this.b;
      }
    };
    console.log(o.f()); // 44

    // 但是当对象的方法被赋予给一个变量时，其相当于函数触发，此时的this为 window (非严格模式下)或者 undefined(严格模式下)。
    var b = 33;
    var o = {
      b: 44,
      f: function() {
        return this.b;
      }
    };
    console.log(o.f()); // 44
    var c = o.f;    // this == window || undefined
    c(); // 33
    ```
    3. 构造函数调用：**this指向该实例对象**。new 方法创建对象的时候，会经过如下过程：创建对象，将this值赋予新的对象 -> 调用构造函数，为this添加属性和方法 -> 返回this给当前的对象
    ```
    function Person(name, age) {
      this.name = name;
      this.age = age;
      return {
        name: 'Mary'
      };
    }
    var person1 = new Person('Jack');
    console.log(person1.name); // 'Mary'
    console.log(person1.age); // undefined

    function Person(name, age) {
      this.name = name;
      this.age = age;
      return this;
    }
    var person1 = Person('Bob', 18);
    console.log(person1.name); // Bob
    console.log(window.name); // Bob
    console.log(person1 === window); // true
    ```

## apply、call和bind

  - 每个函数都包含两个非继承而来的方法：call()和apply()；在JavaScript中，call和apply作用是一样的，都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部this的指向。call 和apply均可以改变this指向，差别在于传入参数的不同；如果第一个参数为null或者undefined，那么就非严格模式下默认为此时的this为window对象，严格模式下为undefined。而

  - bind函数返回的是一个新的函数，即方法，不是立即执行函数，需要手动去调用bind()。使用 bind()方法创建的上下文，其为永久的上下文环境，不可修改，即使是使用 call 或者 apply方法，也无法修改 this 所指向的值。

## 应用例子讲述apply和call
  ```
  const cat = {
    // 它的名字叫 cat
    name: 'cat',
    // 它可以跑
    run() {
      console.log(`${this.name} can run`)
    },
    // 还可以跳(包括跳墙是可以的)
    jump() {
      console.log(`${this.name} can jump`)
    }
  }
  const dog = {
    // 汪星人叫 dog
    name: 'dog',
    // 会跑
    run() {
      console.log(`${this.name} can run`)
    },
    // 还会吠
    bark() {
      console.log(`${this.name} bark loudly`)
    },
    // 马戏团的, 还会做算数
    count(a, b) {
      console.log(`${a} + ${b} = ${a + b}`)
    }
  }
  ```
  - apply和call
  ```
  cat.jump.apply(dog) // 'dog can jump'
  dog.bark.apply(cat) // 'cat bark loudly'
  dog.count.apply(cat, [1, 2]) // 1 + 2 = 3
  dog.count.call(cat, 1, 2) // 1 + 2 = 3
  ```

## bind
  - bind() 方法创建一个新函数，在调用时，将其 this 关键字设置为所需的值。
  ```
  var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
      var fullname = this.firstname + ' ' + this.lastname;
      return fullname;
    }
  };
  var pokemonName = function() {
    console.log(this.getPokeName() + 'I choose you!');
  };
  var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon. 'this' of pokemon === pokemon now
  logPokemon(); // 'Pika Chu I choose you!'
  ```
  -  bind() 一个值后，我们可以像使用任何其他正常函数一样使用该函数。我们甚至可以更新函数来接受参数，并像这样传递它们
  ```
  var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
      var fullname = this.firstname + ' ' + this.lastname;
      return fullname;
    }
  };

  var pokemonName = function(snack, hobby) {
    console.log(this.getPokeName() + 'I choose you!');
    console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
  };
  var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon. 'this' of pokemon === pokemon now
  logPokemon('sushi', 'algorithms');
  // Pika Chu I choose you!
  // Pika Chu  loves sushi and algorithms
  ```
  - bind() 和 call() 之间的主要区别在于 call() 方法：支持接受其他参数；当它被调用的时候，立即执行函数；call() 方法不会复制正在调用它的函数。

