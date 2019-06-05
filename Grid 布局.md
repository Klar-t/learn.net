<center><h1>Grid 布局</h1></center>

- grid-template-columns 属性 定义每一列的列宽

- grid-template-rows 属性 定义每一行的行高

  - ```css
    .container {
      display: grid;
      grid-template-columns: 100px 100px 100px;
      grid-template-rows: 100px 100px 100px;
    }
    repeat(x,y)
    x:表示重复的次数
    y:表示重复的值
    
    repeat(auto-fill,20px)
    auto-fill 关键字 表示自动填充
    
    fr 关键字 
    .container {
      display: grid;
      grid-template-columns: 1fr 2fr;
    }
    表示后者是前者的两倍
    
    grid-template-columns: 1fr 1fr minmax(100px, 1fr);
    minMax（）函数产生一个长度范围，表示长度就在这个范围之中，它接受两个参数，分别为最小值和最大值
    minmax(100px,1fr)表示列宽不小于100px,不大于1fr
    
    grid-row-gap属性设置行与行的间隔（行间距），grid-column-gap属性设置列与列的间隔（列间距）。
    
    grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"。
    
    justify-items属性设置单元格内容的水平位置（左中右），align-items属性设置单元格内容的垂直位置（上中下）。
    
    place-items属性是align-items属性和justify-items属性的合并简写形式。
    
    justify-content属性是整个内容区域在容器里面的水平位置（左中右），align-content属性是整个内容区域的垂直位置（上中下）。
    place-content属性是align-content属性和justify-content属性的合并简写形式。
    
    grid-template属性是grid-template-columns、grid-template-rows和grid-template-areas这三个属性的合并简写形式。
    
    grid属性是grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow这六个属性的合并简写形式。
    
    grid-column属性是grid-column-start和grid-column-end的合并简写形式，grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。
    
    .item-1 {
      grid-column: 1 / 3;
      grid-row: 1 / 2;
    }
    /* 等同于 */
    .item-1 {
      grid-column-start: 1;
      grid-column-end: 3;
      grid-row-start: 1;
      grid-row-end: 2;
    }
    
    
    ```

    - ![1559718584489](C:\Users\zxh01\AppData\Roaming\Typora\typora-user-images\1559718584489.png)

  ```css
  justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。
  
  align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。
  
  
  ```

  [阮一峰 文档参考]()

  [例子参考](<http://chris.house/blog/building-a-home-page-with-grid/>)