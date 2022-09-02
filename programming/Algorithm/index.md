# 埃拉托斯特尼筛法（素数筛）

```javascript
let countPrimes = function (n) {
  let count = 0;
  //定义一个指定长度，每一项的值为0的数组
  let signs = new Uint8Array(n);

  for (let i = 2; i < n; i++) {
    if (!signs[i - 1]) {
      //计数
      count++;
      //输出该素数
      console.log(i);

      for (let j = i * i; j <= n; j += i) {
        signs[j - 1] = true;
      }
    }
  }

  return count;
};
```





