Math对象的扩展
===
ES6在Math对象上新增了__17__个与数学相关的方法。所有这些方法都是静态方法，只能在`Math`对象上调用。

#### Math.trunc()
`Math.trunc方`法用于去除一个数的小数部分，返回整数部分。
```js
Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
```
__对于非数值，Math.trunc内部使用Number方法将其先转为数值。__
```js
Math.trunc('123.456')
// 123

// 对于空值和无法截取整数的值，返回NaN。
Math.trunc(NaN);      // NaN
Math.trunc('foo');    // NaN
Math.trunc();         // NaN
// 对于没有部署这个方法的环境，可以用下面的代码模拟。

Math.trunc = Math.trunc || function(x) {
  return x < 0 ? Math.ceil(x) : Math.floor(x);
}
```
#### Math.sign()
Math.sign方法用来判断一个数到底是正数、负数、还是零。

它会返回五种值。

- 参数为正数，返回+1；
- 参数为负数，返回-1；
- 参数为0，返回0；
- 参数为-0，返回-0;
- 其他值，返回NaN。

```js
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
Math.sign('foo'); // NaN
Math.sign();      // NaN
// 对于没有部署这个方法的环境，可以用下面的代码模拟。

Math.sign = Math.sign || function(x) {
  x = +x; // convert to a number
  if (x === 0 || isNaN(x)) {
    return x;
  }
  return x > 0 ? 1 : -1;
}
```
#### Math.cbrt()
`Math.cbrt`方法用于计算一个数的立方根。
```js
Math.cbrt(-1) // -1
Math.cbrt(0)  // 0
Math.cbrt(1)  // 1
Math.cbrt(2)  // 1.2599210498948734
// 对于非数值，Math.cbrt方法内部也是先使用Number方法将其转为数值。

Math.cbrt('8') // 2
Math.cbrt('hello') // NaN

// 对于没有部署这个方法的环境，可以用下面的代码模拟。
Math.cbrt = Math.cbrt || function(x) {
  var y = Math.pow(Math.abs(x), 1/3);
  return x < 0 ? -y : y;
};
```
#### Math.clz32()
JavaScript的整数使用32位二进制形式表示，`Math.clz32`方法返回一个数的32位无符号整数形式有多少个前导0。
```js
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1000) // 22
// 上面代码中，0的二进制形式全为0，所以有32个前导0；1的二进制形式是0b1，只占1位，所以32位之中有31个前导0；1000的二进制形式是0b1111101000，一共有10位，所以32位之中有22个前导0。

// 左移运算符（<<）与Math.clz32方法直接相关。

Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1 << 1) // 30
Math.clz32(1 << 2) // 29
Math.clz32(1 << 29) // 2

// 对于小数，Math.clz32方法只考虑整数部分。

Math.clz32(3.2) // 30
Math.clz32(3.9) // 30

// 对于空值或其他类型的值，Math.clz32方法会将它们先转为数值，然后再计算。

Math.clz32() // 32
Math.clz32(NaN) // 32
Math.clz32(Infinity) // 32
Math.clz32(null) // 32
Math.clz32('foo') // 32
Math.clz32([]) // 32
Math.clz32({}) // 32
Math.clz32(true) // 31
```
#### Math.imul()
Math.imul方法返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。
```js
Math.imul(2, 4);          // 8
Math.imul(-1, 8);         // -8
Math.imul(-2, -2);        // 4
```
如果只考虑最后32位（含第一个整数位），大多数情况下，Math.imul(a, b)与a * b的结果是相同的，即该方法等同于(a * b)|0的效果（超过32位的部分溢出）。之所以需要部署这个方法，是因为JavaScript有精度限制，超过2的53次方的值无法精确表示。这就是说，对于那些很大的数的乘法，低位数值往往都是不精确的，Math.imul方法可以返回正确的低位数值。

> (0x7fffffff * 0x7fffffff)|0 // 0

上面这个乘法算式，返回结果为0。但是由于这两个二进制数的最低位都是1，所以这个结果肯定是不正确的，因为根据二进制乘法，计算结果的二进制最低位应该也是1。这个错误就是因为它们的乘积超过了2的53次方，JavaScript无法保存额外的精度，就把低位的值都变成了0。Math.imul方法可以返回正确的值1。

> Math.imul(0x7fffffff, 0x7fffffff) // 1

#### Math.fround()
Math.fround方法返回一个数的单精度浮点数形式。
```js
Math.fround(0);     // 0
Math.fround(1);     // 1
Math.fround(1.337); // 1.3370000123977661
Math.fround(1.5);   // 1.5
Math.fround(NaN);   // NaN
```
对于整数来说，Math.fround方法返回结果不会有任何不同，区别主要是那些无法用64个二进制位精确表示的小数。这时，__Math.fround方法会返回最接近这个小数的单精度浮点数。__

对于没有部署这个方法的环境，可以用下面的代码模拟。
```js
Math.fround = Math.fround || function(x) {
  return new Float32Array([x])[0];
};
```
#### Math.hypot()
Math.hypot方法返回所有参数的平方和的平方根。
```js
Math.hypot(3, 4);        // 5
Math.hypot(3, 4, 5);     // 7.0710678118654755
Math.hypot();            // 0
Math.hypot(NaN);         // NaN
Math.hypot(3, 4, 'foo'); // NaN
Math.hypot(3, 4, '5');   // 7.0710678118654755
Math.hypot(-3);          // 3
```
上面代码中，3的平方加上4的平方，等于5的平方。

如果参数不是数值，Math.hypot方法会将其转为数值。只要有一个参数无法转为数值，就会返回NaN。

### 对数方法
ES6新增了4个对数相关方法。

#### Math.expm1()

Math.expm1(x)返回ex - 1。
```js
Math.expm1(-1); // -0.6321205588285577
Math.expm1(0);  // 0
Math.expm1(1);  // 1.718281828459045

// 对于没有部署这个方法的环境，可以用下面的代码模拟。
Math.expm1 = Math.expm1 || function(x) {
  return Math.exp(x) - 1;
};
```
#### Math.log1p()

Math.log1p(x)方法返回1 + x的自然对数。如果x小于-1，返回NaN。
```js
Math.log1p(1);  // 0.6931471805599453
Math.log1p(0);  // 0
Math.log1p(-1); // -Infinity
Math.log1p(-2); // NaN

// 对于没有部署这个方法的环境，可以用下面的代码模拟。
Math.log1p = Math.log1p || function(x) {
  return Math.log(1 + x);
};
```
####Math.log10()

Math.log10(x)返回以10为底的x的对数。如果x小于0，则返回NaN。
```js
Math.log10(2);      // 0.3010299956639812
Math.log10(1);      // 0
Math.log10(0);      // -Infinity
Math.log10(-2);     // NaN
Math.log10(100000); // 5

// 对于没有部署这个方法的环境，可以用下面的代码模拟。
Math.log10 = Math.log10 || function(x) {
  return Math.log(x) / Math.LN10;
};
```
#### Math.log2()
Math.log2(x)返回以2为底的x的对数。如果x小于0，则返回NaN。
```js
Math.log2(3)    // 1.584962500721156
Math.log2(2)    // 1
Math.log2(1)    // 0
Math.log2(0)    // -Infinity
Math.log2(-2)   // NaN
Math.log2(1024) // 10
Math.log2(1 << 29) // 29

// 对于没有部署这个方法的环境，可以用下面的代码模拟。
Math.log2 = Math.log2 || function(x) {
  return Math.log(x) / Math.LN2;
};
```
### 三角函数方法
ES6新增了6个三角函数方法。

- Math.sinh(x) 返回x的双曲正弦（hyperbolic sine）
- Math.cosh(x) 返回x的双曲余弦（hyperbolic cosine）
- Math.tanh(x) 返回x的双曲正切（hyperbolic tangent）
- Math.asinh(x) 返回x的反双曲正弦（inverse hyperbolic sine）
- Math.acosh(x) 返回x的反双曲余弦（inverse hyperbolic cosine）
- Math.atanh(x) 返回x的反双曲正切（inverse hyperbolic tangent）