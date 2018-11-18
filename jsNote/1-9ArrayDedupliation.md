## 数组去重复

1. 双重循环。创建新空数组，循环数组的每一项与新数组的每一项比较，不存在就添加到新的空数组中。（回转寿司，跟自己桌子上的对比）

   * break 和标签语句 结束循环

     ```js
     var nembArray = [1, 5, 2, 12, 3, 2, 3, 3, 4, 32, 41, 2, 1, 12, 31, 23, 23, 12, 3, 23]
     var newArray = []
     for (var i = 0; i < nembArray.length; i++) {
         var flg =true;
         start : 
         for (var k = -1; k < newArray.length; k++) {
             if (newArray[k] === nembArray[i]) {
                 flg=false
                 break start;
             }
         }
         if (flg){
             newArray.push(nembArray[i])
         }
     }
     ```

   * 放到函数中用return语句结束循环

     ```js
     for (var i = 0; i < nembArray.length; i++) {
             additem(nembArray[i])
     }
     function additem(item){
         for (var k = -1; k < newArray.length; k++) {
             if (newArray[k] === item) {
                 return
             }
         }
         newArray.push(item)
     }
     ```

2. 双重循环，创建新空数组，循环数组项跟后面的数组项进行对比，后面有一样的就不做处理，后面没有的就放到新数组里面。（回转寿司，跟传送带上的对比）

   ```js
   var nembArray = [1, 5, 2, 12, 3, 2, 3, 3, 4, 32, 41, 2, 1, 12, 31, 23, 23, 12, 3, 23]
   var newArray = []
   for (var i = 0; i < nembArray.length; i++) {
       var flg =true;
       start :
       for (var k = i+1; k < nembArray.length; k++) {
           if(nembArray[k] === nembArray[i]){
               flg=false
               break start;
           }
       }
       if (flg){
           newArray.push(nembArray[i])
       }
   }
   // 简写方式
   for (var i = 0; i < nembArray.length; i++) {
       var flg =true;
       for (var k = i+1; k < nembArray.length; k++) {
           if(nembArray[k] === nembArray[i]){
               flg=false
               k=++i
           }
       }
       if (flg){
           newArray.push(nembArray[i])
       }
   }
   ```

3. 同上，但是重复的删除，剩下的就是不重复的。注意k值要减1

4. 使用对象属性名不能重复的特性去重

5. 排序后再去重，删除相邻两个重复的

6. ES5实现

   1. indexOf()  传入要对比的项，查找是否再数组中存在。
   2. indexOf()  传入要对比的项和开始对比的位置，可以查找后面是否存在。
   3. forEach()  代替for循环

7. ES6实现

   1. set对象  把set对象转化为数组用from对象 
   2. 拓展运算符... 