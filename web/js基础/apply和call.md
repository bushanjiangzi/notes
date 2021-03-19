## apply和call
  - 每个函数都包含两个非继承而来的方法：call()和apply()；在JavaScript中，call和apply作用是一样的，都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部this的指向。

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

## bind()
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

## this指向
  
  - this对象分类：全局对象，当前对象，任意对象

  - 判断处于哪种this对象，这个取决于函数的调用方式。而JavaScript中的函数调用方式又分为四种：函数调用，作为对象的方法调用，构造函数调用，call 、apply 、bind

  - https://juejin.cn/post/6844903843512205326
