##### css规则

- at规则（at-rule）
- 普通规则（qualified-rule）



##### at规则

- @charset

  提示css文件的使用的字符编码方式，使用时必须放在最前面。

  ```
  @charset "utf-8"
  ```

- @import

  引入css文件，可以引入另一个文件的全部内容（@charset规则不会被引入），支持supports和media query形式

  ```js
  @import "mystyle.css"
  @import url("mystyle.css")
  @import "mystyle.css" screen,projection
  ```

- @media  媒体查询

  ```
  @media print {
      body { font-size: 10pt }
  }
  ```

- [@page](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@page)

  用于在打印文档时修改某些CSS属性，只能修改margin,orphans,widow 和 page breaks of the document

  ```
  @page {
    size: 8.5in 11in;
    margin: 10%;
  
    @top-left {
      content: "Hamlet";
    }
    @top-right {
      content: "Page " counter(page);
    }
  }
  
  /* 设置打印时的左侧文档样式 */
  @page :left {
    margin: 2in 3in;
  }
  ```

- [@counter-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@counter-style)

  定义列表项的表现

  ```
  @counter-style circled-alpha {
    system: fixed;
    symbols: Ⓐ Ⓑ Ⓒ Ⓓ Ⓔ Ⓕ Ⓖ Ⓗ Ⓘ Ⓙ Ⓚ Ⓛ Ⓜ Ⓝ Ⓞ Ⓟ Ⓠ Ⓡ Ⓢ Ⓣ Ⓤ Ⓥ Ⓦ Ⓧ Ⓨ Ⓩ;
    suffix: " ";
  }
  .items {
      list-style: circled-alpha;
  }
  // 产生的列表style以顺序Ⓐ,..Ⓩ,27,28...
  ```

- @keyframes  定义动画关键帧

  ```
  @keyframes diagonal-slide {
    from {
      left: 0;
    }
    to {
      left: 100px;
    }
  }
  ```

- @fontface  定义字体

  ```
  @font-face {
    font-family: Gentium;
    src: url(http://example.com/fonts/Gentium.woff);
  }
  p { font-family: Gentium, serif; }
  ```

- @support 检查环境的特性，与media类似

- [@namespace](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@namespace)

  用于跟XML命名空间配合，表示内部css选择器都带上特地命名空间

  ```
  @namespace url(http://www.w3.org/1999/xhtml);
  @namespace svg url(http://www.w3.org/2000/svg);
  
  /* 匹配所有的XHTML <a> 元素, 因为 XHTML 是默认无前缀命名空间 */
  a {}
  
  /* 匹配所有的 SVG <a> 元素 */
  svg|a {}
  
  /* 匹配 XHTML 和 SVG <a> 元素 */
  *|a {}
  ```


- [@viewport](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@viewport)

  设置视口的一些特性，兼容性不佳，多数时候用`html`的`meta`标签



##### 普通规则（选择器+声明区块）

- 选择器

  ![选择器语法结构](https://static001.geekbang.org/resource/image/4f/67/4fa32e5cf47c72a58f7a8211d4e8fc67.png)

- 声明：属性+值

  - 属性：由中划线、下划线、字母等组成的标识符，支持反斜杠转义（两个中划线开头的为变量而非属性，通过`var(--xx)`来访问

    ```
    :root { --main-color: #06c; }
    h1 { color: var(--main-color); }
    ```

  - 值

    - 关键字，如`initial`，`unset`，`inherit`,任何属性都可以的关键字

    - 字符串，如`content`属性

    - URL

    - 整数、实数，如`line-height`，`flex`等属性

    - 维度，如width

    - 百分比

    - 颜色，如`#000`，`rgb`等

    - 图片

    - 2D位置

    - 函数

      1. cacl()   支持单位混合四则运算

         ```
         section {
           float: left;
           margin: 1em; border: solid 1px;
           width: calc(100%/3 - 2*1em - 2*1px);
         }
         ```

      2. max()        二者较大值

      3. min()         二者较小值

      4. clamp()     给一个值限定范围，超出时采用范围边界值

      5. toggle()     在对个值中来回切换

         ```
         ul { list-style-type: toggle(circle, square); }
         // 列表样式在圆点和方点之间切换
         ```

      6. attr() 获取某个属性的值

         ```
         <p data-foo="hello">world</p> 
         
         [data-foo]::before {
           content: attr(data-foo) " ";
         }
         ```


