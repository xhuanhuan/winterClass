##### 原型系统

- 如果所有对象都有私有字段`[[prototype]]`，就是对象的原型。
- 读一个属性，如果对象本身没有，就会继续访问对象的原型，直至原型为空或找到为止。



##### 访问操纵原型

- `Object.create `  根据原型创建新对象，原型可为null
- `Object.getPrototypeOf `  获得一个对象的原型
- `Object.setPrototypeOf `  设置一个对象的原型

```
var cat = {
    say(){
        console.log("meow~");
    },
    jump(){
        console.log("jump");
    }
}
var tiger = Object.create(cat,  {
    say:{
        writable:true,
        configurable:true,
        enumerable:true,
        value:function(){
            console.log("roar!");
        }
    }
})

var anotherCat = Object.create(cat);
anotherCat.say();

var anotherTiger = Object.create(tiger);
anotherTiger.say();


```



##### 访问`[[class]]`属性的方法

- `Object.prototype.toString`

- `Symbol.toStringTag`可以定义`Object.prototype.toString`的行为

  ```
  var o = { [Symbol.toStringTag]: "MyObject" }
  console.log(o + ""); // [object MyObject]， 字符串加法触发了 Object.prototype.toString的调用
  ```



##### new的行为

1. 以构造器的`prototype`属性为原型，创建一个新对象；
2. 将`this`和参数传给构造器并执行；
3. 如果构造器返回了对象，则返回，否则返回第一步创建的对象



##### es6中的类

```
class Animal { 
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }
  speak() {
    console.log(this.name + ' barks.');
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```



