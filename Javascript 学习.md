<center><h1>Javascript 学习</h1></center>

- 调试技术
  - 

- 执行环境及作用域
  - 

- 引用类型

  - Array类型

    - 检测数组 Array.isArray()

    - 转换方法 tostring（）、valueof（）、tolocalestring（）

    - 栈方法 

      - push() 方法可以接收任意数量的参数，吧他们逐个添加到数组末尾，并返回修改后数组的长度

      - pop() 方法从数组末尾移除最后一项，减少数组的length值，返回移除的项

      - ```js
        var colors=new Array();
        var count=colors.push("red","green");
        alert(count);//2
        
        count=colors.push("black");
        alert(count);//3
        
        var item=colors.pop();
        alert(item);//"black"
        alert(colors.length);//2
        ```

    - 队列方法

      - ```js
        
        ```

    - 重排序方法

      - reverse() 反转数组项的顺序
      - sort() 方法按升序排列数组项——即最小的值位于最前面，最大的值排在最后面。sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以
        确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串

    - 操作方法

      - concat() 方法可以 基于当前数组中的所有项创建一个新数组
      - slice() 基于当前数组中的一或多个项创建一个新数组。接受一个活两个参数，即要返回项的起始和结束位置。在只有一个参数的情况系下，返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的 项， 但不包括结束位置的项。slice（）方法不会影响原始数组