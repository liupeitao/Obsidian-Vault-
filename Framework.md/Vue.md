# Vue

‍

## Invoke和call有什么区别

​`call`​ 和 `apply`​ 是 JavaScript 中的两个方法，它们都是用来改变函数的 `this`​ 指向的。`call`​ 和 `apply`​ 的区别在于传入参数的方式不同：

* ​`call`​ 方法传入参数的方式是一个一个单独传入，比如 `func.call(thisArg, arg1, arg2, arg3)`​；
* ​`apply`​ 方法传入参数的方式是一个数组，比如 `func.apply(thisArg, [arg1, arg2, arg3])`​。

​`invoke`​ 不是 JavaScript 原生的方法，我不确定你指的是哪个函数。如果是指 Lodash 中的 `invoke`​ 函数，它是用来调用一个对象方法的，第一个参数是要调用方法的对象，第二个参数是要调用的方法名，后面的参数是要传递给方法的参数。它和 `call`​ 或 `apply`​ 的作用不同，因为它是调用对象的方法，而不是改变函数的 `this`​ 指向。{{
