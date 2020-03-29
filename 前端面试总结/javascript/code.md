## 面试写过的代码

### 二分查找

```javascript
var search = function(nums, target, left, right) {
  const l = typeof left === 'number' ? left : 0,
  r = typeof right === 'number' ? right : nums.length
  if(l <= r) {
    const middle = Math.floor((l + r) / 2)
    if (nums[middle] === target) {
      return middle
    } else if (nums[middle] > target) {
      return search(nums, target, l, middle - 1)
    } else {
      return search(nums, target, middle + 1, right)
    }
  } else {
    return -1
  }
};
```

### 柯里化

```javascript
const curryAdd = (() => {
  let cacheArgs = []
  return function add(...args) {
    if(args.length) {
      cacheArgs.push(...args)
      return add
    } else {
      let sum = cacheArgs[0] || NaN
      if(cacheArgs.length > 2) {
        sum = cacheArgs.reduce((a, b) => a + b)
      }
      cacheArgs = []
      return sum
    }
  }
})()
```

### Promise.all

```javascript
Promise.all = (promises) => {
  const result = [], count = promise.length
  return new Promise((resolve, reject) => {
    promises.forEach((promise, index) => {
      promise.then(res => {
        count--
        result[index] = res
        if(!count) {
          resolve(result)
        }
        return res
      }).cache(err => {
        result[index] = err
        reject(result)
        return Promise.reject(err)
      })
    })
  })
}
```

### Promise.race

```javascript
const _resolve = function(resolve, reject, res) {
  if(res instanceof Promise) {
    res.then(nextRes => {
      _resolve(nextRes)
    }).cache(err => reject(err))
  } else {
    resolve(res)
  }
}
Promise.race = promises => {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      promise.then(res => {
        _resolve(resolve, reject, res)
        return res
      }).cache(err => {
        reject(err)
        return Promise.reject(err)
      })
    })
  })
}
```

### debounce

```javascript
const debounce = (fn, duration) => {
  let timerId = null
  return function(...args) {
    if(timerId) {
      clearTimeout(timerId)
    } else {
      timerId = setTimeout(() => {
        fn.call(this, ...args)
      }, duration)
    }
  }
}
```

### throttle

```javascript
const throttle = (fn, duration) => {
  let isFree = true
  return function(...args) {
    if(isFree) {
      isFree = false
      setTimeout(() => { isFree = true }, duration)
      fn.call(this, ...args)
    }
  }
}
```

### 查找最长公共前缀

```javascript
const findLongestCommonPrefixByTwoString = (string1, string2) => {
  let i = 0
  while(i < string1.length && i < string2.length) {
    if(string1[i] !== string2[i]) {
      break
    } else {
      i++;
    }
  }
  return string1.slice(0, i)
}
const findLongestCommonPrefix = strings => {
  if(strings.length < 2) {
    return strings[0] || ''
  }
  return strings.reduce((s1, s2) => findLongestCommonPrefixByTwoString(s1, s2))
}
```

