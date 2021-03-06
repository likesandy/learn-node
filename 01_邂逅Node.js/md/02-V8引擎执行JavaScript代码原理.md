# V8引擎

- 首先先看一下官方是如何定义的
  + V8是用C ++编写的Google开源高性能JavaScript和WebAssembly引擎。它用于Chrome和Node.js等
  + 它实现ECMAScript和WebAssembly，并在Windows 7或更高版本，macOS 10.12+和使用x64，IA-32，ARM或MIPS处理器的Linux系统上运行
  + V8可以独立运行，也可以嵌入到任何C ++应用程序中。

  ![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/623f961ce0274cfa9351a66845ed4a9d~tplv-k3u1fbpfcp-watermark.image)

# V8引擎的原理

- V8引擎本身的源码非常复杂，大概有超过100w行C++代码，但是我们可以简单了解一下它执行JavaScript代码的原理：

- Parse模块会将JavaScript代码转换成AST（抽象语法树），这是因为解释器并不直接认识JavaScript代码； 
  + 如果函数没有被调用，那么是不会被转换成AST的； 
  + Parse的V8官方文档：https://v8.dev/blog/scanner

- Ignition是一个解释器，会将AST转换成ByteCode（字节码）
  + 同时会收集TurboFan优化所需要的信息（比如函数参数的类型信息，有了类型才能进行真实的运算）；
  + 如果函数只调用一次，Ignition会执行解释执行ByteCode；
  + Ignition的V8官方文档：https://v8.dev/blog/ignition-interpreter

- TurboFan是一个编译器，可以将字节码编译为CPU可以直接执行的机器码；
  + 如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过TurboFan转换成优化的机器码，提高代码的执行性能；
  + 但是，机器码实际上也会被还原为ByteCode，这是因为如果后续执行函数的过程中，类型发生了变化（比如sum函数原来执行的是number类型，后
来执行变成了string类型），之前优化的机器码并不能正确的处理运算，就会逆向的转换成字节码；
  + TurboFan的V8官方文档：https://v8.dev/blog/turbofan-jit

- 上面是JavaScript代码的执行过程，事实上V8的内存回收也是其强大的另外一个原因，不过这里暂时先不展开讨论：
  + Orinoco模块，负责垃圾回收，将程序中不需要的内存回收；
  + Orinoco的V8官方文档：https://v8.dev/blog/trash-talk

> 这里不就细谈V8引擎了,主要了解V8引擎是什么就可以了,V8引擎暂时还不是很清楚,以后清楚了再专门出关于V8引擎原理的仓库