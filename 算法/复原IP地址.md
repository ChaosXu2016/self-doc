## 复原IP地址

### 题目描述

原题目地址：[复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

```
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```

### 分析

1. 什么是有效的ip地址？

   首先ip地址是0～255之间的数字，这里要注意数字除0以外，第一个字符不能为0，因此我们可以写出下面的校验函数

   ```javascript
   const isValidAddress = address => {
     return !(
       (address.length > 1 && address[0] === '0') ||
       Number(address) > 255
     )
   }
   ```

2. 如何复原？

   以`"25525511135"`为例，我们分别用`2|25|255`来作为第一个ip地址，来尝试下一个地址是否有效，一直到第四个。这时候我们遇到不符合条件的分支则舍弃。

3. 如何判断不符合条件？

   + 假设我们已经复原了`k`个地址，这时候还剩字符串的长度为`l`，如果`l > (4 - k) * 3`，这时候我们不论怎么组合，都用不完当前的字符串，所以检测到这种情况的时候，我们便不需要往下遍历了。
   + 还没有复原出4个地址的时候，字符串已经用完了，这种分支也可以舍弃
   + 当前地址不是有效的ip地址时，也需要舍弃
   + 没有用完当前字符串的分支，也需要舍弃。这实际上是对第一种情况的补充。这时候我们只需要这么判断即可：当已经复原了3个地址，则最后一个地址肯定是我们没用完的字符串，这时候我们只需要判断剩下的字符串是不是有效的地址即可。

### 实现

```javascript
/**
 * @param {string} s
 * @return {string[]}
 */

const isValidAddress = address => {
  return !(
    (address.length > 1 && address[0] === '0') ||
    Number(address) > 255
  )
}

const dfsAddress = (s, i, lastAddresses, addressesList) => {
  const restLength = s.length - i
  let addresses = []
  if(restLength > 0 && restLength <= (4 - lastAddresses.length) * 3) {
    if(lastAddresses.length === 3) {
      const nextAddress = s.slice(i)
      if(isValidAddress(nextAddress)) {
        addresses = [...lastAddresses, nextAddress]
      }
    } else {
      for(let n = 1; n <= restLength && n <= 3; n++) {
        const nextAddress = s.slice(i, i + n)
        if(isValidAddress(nextAddress)) {
          addresses = [...lastAddresses, nextAddress]
          dfsAddress(s, i + n, addresses, addressesList)
        }
      }
    }
  }
  addresses.length === 4 && addressesList.push(addresses.join('.'))
}

var restoreIpAddresses = function(s) {
  const addressesList = []
  dfsAddress(s, 0, [], addressesList)
  return addressesList
};
```

