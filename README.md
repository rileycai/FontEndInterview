# FontEndInterview
Front-end interview resources

##  bilibili 面试 
不考虑兼容性且不能更改dom结构，需求如下：

  1.完成经典的上 header ，下 footer ，左边是侧边栏，右边是内容。
  
  2.去掉列表前面的 · ，列表项水平排列，注意里面的br标签需要换行，同时每两个li后有一条竖线。
  
  3.点击列表项不跳转，弹出href内的内容

[来源](https://juejin.im/post/5c878397f265da2dde07293b)  [题目](https://github.com/zhenzhencai/FontEndInterview/blob/master/Questions.html)  [答案](https://github.com/zhenzhencai/FontEndInterview/blob/master/Answer.html)
    
<!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
      * { 
        margin: 0; 
        padding: 0; 
      }
      html, 
      body, 
      #app { 
        margin: 0; 
        padding: 0; 
        height: 100%; 
      }
      #header, #footer {
        height: 50px;
        line-height: 50px;
        text-align: center;
        background: #555;
        color: #fff;
      }
      #side { 
        width: 200px; 
        background: #eee; 
      }

      /* css here */
    </style>
  </head>
  <body>
    <div id="app">
      <header id="header">header</header>
      <aside id="side">side</aside>
      <div id="main">
        <ul>
          <li><a href="https://www.bilibili.com/1">Link1</a></li>
          <li><a href="https://www.bilibili.com/1">Link2</a></li>
          <li><a href="https://www.bilibili.com/1">Link3</a></li>
          <br>
          <li><a href="https://www.bilibili.com/1">Link4</a></li>
          <li><a href="https://www.bilibili.com/1">Link5</a></li>
        </ul>
      </div>
      <footer id="footer">footer</footer>
    </div>
    <script>
      // JS here
    </script>
  </body>
</html>
    
    个

