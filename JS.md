短路表达式：作为"&&"和"||"操作符的操作数表达式，这些表达式在进行求值时，只要最终的结果已经可以确定是真或假，求值过程便告终止，这称之为短路求值。这是这两个操作符的一个重要属性。

```javascript
    // ||短路表达式
    var foo = a || b;
    // 相当于
    if(a){
        foo = a;
    }else{
        foo = b;
    }
    
    // &&短路表达式
    var bar = a && b;
    // 相当于
    if(a){
        bar = b;
    }else{
        bar = a;
    }
```

1、在 Javascript 的逻辑运算中，0、""、null、false、undefined、NaN 都会判定为 false ，而其他都为 true ；

2、因为 Javascript 的内置弱类型域 (weak-typing domain)，所以对严格的输入验证这一点不太在意，即便使用 && 或者 || 运算符的运算数不是布尔值，仍然可以将它看作布尔运算。虽然如此，还是建议如下：

```javascript
if(foo){ ... }     //不够严谨
if(!!foo){ ... }   //更为严谨，!!可将其他类型的值转换为boolean类型
```