### React

- **Declarative**: 

- **Component-Based**: 

  ​

#### Declarative Programming

> We say the operation is declarative if, whenever called with the same arguments, it returns the same results independent of any other **computation state**.

- **independent**: it computes irrespective of other computation state
- **stateless**: each action is unrelated to any previous action
- **deterministic**: the same inputs will return the same outputs
- **referentially transparent**: the expression can be replaced with its value without changing the meaning of the program



![More is not better or worse than less, just different.](https://dl.dropboxusercontent.com/u/1401061/paradigms.jpg)



> In programming, you keep decomposing a problem until you reach the level of detail that you can deal with, solve each subproblem in turn, and recompose the solutions bottom-up. 



> The biggest difference between the local and the global approach is in their treatment of space and, more importantly, time. The local approach embraces the immediate gratification of here and now, whereas the global approach takes a long-term static view, as if the future had been preordained, and we were only analyzing the properties of some eternal universe.





现在的感觉是：Declarative Programming 是在编程语言层面的，How 和 What 都是指程序提供的语言接口。

C 系的命令式编程都是面向 **计算机硬件** 的抽象，有变量、赋值语句、表达式等等。命令式程序就是一个冯诺依曼机的指令序列。

而函数式编程是面向数学的抽象，将计算描述为一种表达式求值。这里的函数也是指数学上的自变量映射。

语言层面上提供了不同的接口，也影响程序员编码时的思考习惯，这也就是 How 和 What 的由来。



from wikipedia

> Declarative programming is a programming paradigm - a style of building the structure and elements of computer programs - that expresses the logic of a computation without describing its control flow.

对应到语言中，SQL 就不会提供控制流 if for 等。

from [Difference between declarative and imperative in React.js](http://stackoverflow.com/a/33656983/3873366)

> React is able to be declarative because it "knows how to make chicken", for example. Compared to backbone, which only knows how to interface with the kitchen.

