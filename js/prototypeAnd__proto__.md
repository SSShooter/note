原链接http://rockyuse.iteye.com/blog/1426510
__proto__是原型
prototype是模板
每个对象都会在其内部初始化一个属性，就是__proto__，当我们访问一个对象的属性 时，如果这个对象内部不存在这个属性，那么他就会去__proto__里找这个属性，这个__proto__又会有自己的__proto__，于是就这样 一直找下去，也就是我们平时所说的原型链的概念。
其实prototype只是一个假象，他在实现原型链中只是起到了一个辅助作用，换句话说，他只是在new的时候有着一定的价值，而原型链的本质，其实在于__proto__


其他相关
三张图搞懂JavaScript的原型对象与原型链
http://gold.xitu.io/post/5835853f570c35005e413b19?utm_source=gold_browser_extension
