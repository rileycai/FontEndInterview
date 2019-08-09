# 图森未来面试
时间： 2019年8月9日 周五

## 一面-代码面试
### 简单介绍下实习情况

### Game One

给定一个字符串，找出不含有重复字符的 最长子串 的长度。

示例：

给定 “abcabcbb” ，没有重复字符的最长子串是 “abc” ，那么长度就是3。

给定 “bbbbb” ，最长的子串就是 “b” ，长度是1。

给定 “pwwkew” ，最长子串是 “wke” ，长度是3。请注意答案必须是一个子串，“pwke” 是 子序列 而不是子串。

### Game Two

了解Promise.all吗？写一个实现吧
```javascript
// 现场版本
Promise.all = (arr) => {
    if(!Array.isArray(arr)){
        throw typeError('error!');
    }
    return new Promise((resolve,reject) => {
        let resValue = [];
        const len = arr.length;
        let count = 0;
        for (let [i,item] in arr){
            Promise.resolve(item).then(res=>{
                count++;
                resValue[i] = res;
                if (count === len) {
                    resolve(resValue);
                }
            },err=>{
                reject(err);
            })
        }
    })
}
```

### Game Three

给定一个字符串生成对应的DOM树。
PS：除了字母外，只会出现(. # > +)符号
example:
html

 input                 output
 
'div.a' =>        <div class="a"></div>
'div#a' =>        <div id="a"></div>
'div.a>span.b' =>  <div class="a"><span class="b"></span></div>
'div#a>p.b+p.c' => <div id="a">
                    <p class="b"></p>
                    <p class="c"></p>
                  </div>
         