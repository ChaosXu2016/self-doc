# CSS相关问题汇总

#### 如何实现水平垂直居中？

1. flex

   ```css
   /*flex*/
   .father-layout {
     display: flex;
     align-items: center;
     justify-content: center;
   }
   ```

   

2. 当子元素定宽高时，使用负margin的方法：父元素设置`position: releative;`，子元素设置`position: absolute`。`top`和`left`的值为`50%`，再用宽高的负的`1/2`为`margin-top`和`margin-left`的值即可

   ```css
   .father-layout {
     background: pink;
     width: 1000px;
     height: 200px;
     position: relative;
   }
   /*margin*/
   /*子元素需定宽高*/
   .child-layout {
     position: absolute;
     width: 100px;
     height: 50px;
     background: green;
     top: 50%;
     left: 50%;
     margin-top: -25px;
     margin-left: -50px;
   }
   ```

3. 当子元素宽高不定的时候，使用tansform：父元素设置`position: releative;`，子元素设置`position: absolute; overflow: hidden;`。`top`和`left`的值为`50%`，再设置`transform: translate(-50%, -50%);`即可。

   ```css
   .father-layout {
     background: pink;
     height: 200px;
     position: relative;
   }
   .child-layout {
     position: absolute;
     overflow: hidden;
     background: green;
     top: 50%;
     left: 50%;
     transform: translate(-50%, -50%);
   }
   ```

#### 圣杯布局和双飞翼布局

+ 需求：三列布局，中间主题内容宽度自适应，两边定宽

##### 圣杯布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    * {
      padding: 0;
      margin: 0;
    }
    .main-container {
      overflow: hidden;
      padding: 0 150px 0 200px;
    }
    .main {
      height: 500px;
      width: 100%;
      background: blue;
      float: left;
    }
    .left {
      height: 500px;
      float: left;
      background: green;
      width: 200px;
      position: relative;
      margin-left:-100%;
      left: -200px;
    }
    .right {
      height: 500px;
      float: left;
      background: red;
      width: 150px;
      position: relative;
      margin-left: -150px;
      left: 150px;
    }
  </style>
</head>
<body>
  <div class="main-container">
    <div class="main">main</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```

##### 双飞翼布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    * {
      padding: 0;
      margin: 0;
    }
    .container {
      width: 100%;
      height: 500px;
      overflow: hidden;
    }
    .main-wrap {
      float: left;
      width: 100%;
    }
    .main {
      margin: 0 150px 0 200px;
      height: 500px;
      background: blue;
    }
    .left {
      float: left;
      background: red;
      height: 500px;
      width: 200px;
      margin-left: -100%;
    }
    .right {
      float: left;
      background: green;
      width: 150px;
      height: 500px;
      margin-left: -150px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="main-wrap">
      <div class="main"></div>
    </div>
    <div class="left"></div>
    <div class="right"></div>
  </div>
</body>
</html>
```



#### BFC

> `BFC（Block Formatting Context）`块级格式化上下文

##### BFC的特点

1. BFC内部的子元素在垂直方向，边距会发生重叠。
2. BFC在页面中是独立的模块，外面的元素不会影响内部元素，反之亦然。
3. BFC区域不与float box重叠。
4. 计算BFC高度时，浮动的子元素也参与计算

##### 生成BFC

1. overflow: hidden/auto
2. float: left/right/inhert
3. position: absolute/fixed
4. display: inline-block/table-caption/table-cell/flex/inline-flex



#### 选择器权重

1. tag选择器 1
2. class、属性选择器 10
3. id选择器 100
4. 行内样式 1000
5. important最高优先级
6. 遵循后面的覆盖前面的规则的原则



#### CSS3新特性

1. border-radius
2. transform
3. shadow
4. text-shadow
5. 线性渐变
6. 旋转

