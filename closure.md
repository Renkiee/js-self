 # 闭包 一个函数作用域内部的变量的函数被另一个函数持有，所以一个函数
 执行完因为内部变量被其它函数持有，所以不能立即清空函数上下文。

 原因
 1. js允许函数内部定义新的函数
 2. js允许函数作为返回值
 3. 可以在内部函数中访问父函树内部定义的变量，

 栈：内存中一块连续的内存空间，体积小，速度快，执行完立即释放，所以函数上下文变量都是存放在栈里面的
 闭包被引用的变量不会立即释放所以他其实是存放在堆里面的


 逃逸分析是实现闭包的基础，在js解析的时候会进行变量提升分配内存，语法检查，所以他会知道哪些变量
 v8执行js代码会经过编译执行两个阶段，编译阶段是将js代码转换成为字节码或者机器码，执行阶段指cpu执行机器码
 v8不会将所有代码全部编译成中间码，因为等待时间过长，并且占用内存，所以v8采用了延迟解析，对不是立即执行
 的函数代码会跳过函数内部代码，只为函数声明生成中间码
 但是这样就有一个问题，不编译函数内部代码就没有办法知道函数内部是否引用了另一个函数的变量，导致函数执行完成之后
 不能正确释放上下文，所以v8同时还引入了逃逸分析也叫预解析，当解析代码遇到一个函数时，v8会对该函数快速的做一次
 预解析，1.首先判断是否有语法错误 2.然后检查内部是否引用了外部的变量，如果发现有引用外部的变量，会把栈中的变量复制到堆中，这样下次执行就使用堆里面的引用，这样就解决闭包带了的问题

 带变量的方法
```js
function bibao (){
  let a = 12313
  return function(){
    console.log(a)
  }
}
bibao()
```
# 事件委托 事件代理
基本功能就是用来为dom元素绑定事件，可以同时绑定多个事件会按照顺序触发
由于事件冒泡的机制，可以把子元素的事件绑定到父元素身上，等冒泡触发父元素的事件，然后执行回调，函数是需要占内存的，使用事件代理可以减少内存占用，优化性能，而且对于新加的子元素不需要手动的绑定事件