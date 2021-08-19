## 初识javaScript
- `<script>`  
  - `<script>`标签的位置  
`<script>`标签可以在页面的任何位置，同其他html标签一样等待浏览器从上至下执行。
将`<script>`放在`<body>`之前需要先下载和执行玩标签内的内容才会开始页面内容的渲染，会导致DOM节点的捕捉失败，因为此时DOM树还未建立。建议放在`<body>`之后，或将`<script>`内的代码全放在`window.onload`事件中。
  - `<script>` 属性  
  async 表示立即下载脚本，只针对外部脚本文件  
  charset  
  defer  脚本延迟到文档完全解析和显示后再执行  
  integrity  允许验证签名和资源完整性  
  language  代码块中脚本语言如Javascript，已废弃  
  src  外部文件的url  
  type  代替language，值通常为"text/javascript"  
  - 使用`<script>`的方式  
嵌入网页中：直接将代码放置在`<script>`标签中  
通过`src`属性引入外部脚本  
引入外部脚本的`<script>`标签内部的代码会被忽略
  - `<noscript>`标签  
用于浏览器不支持js或禁用js的情形，此时浏览器会渲染`<noscript>`标签中的内容，标签中可以包含除`<script>`标签外的任何HTML标签。 

- 标识符
    - 标识符命名规则使用驼峰大小写形式，要求以非数字字符开头
    - 属性和变量命名用名词，方法和函数命名用动词，返回`Boolean`的函数使用is开头
    - 类命名要求以大写字母开头

- 严格模式
- 启用严格模式
整个脚本启用，在脚本开头加上一行：  
`"use strict"`
单独指定函数:  

    ```language
    function (){
        "use strict";  
        //函数体  
    }
    ```  



- `typeof` 和 `instanceof`
  - `typeof` 操作符用于确定任意变量的数据类型，其返回值为下列字符串之一：  
`undefined  boolean  string  number object  function  symbol`   
`typeof null`的返回值为`object`
  - `a instanceof b` 判断`a`是否是`b`的实例，返回一个`Boolean`值  
---
## 简单数据类型（原始数据类型）6种 和复杂数据类型（引用数据类型）
1. `Undefined`   
只有一个值`undefined`，使用`var`和`let`声明了变量但没初始化时，就相当于于给变量赋值`undefined`  
2. `Null`  
只有一个值`null`，表示空对象指针。  
**用`==`比较`undefined`和`null`会返回`true`**  
3. `Boolean`  
两个值`true`和`false`  
方法：`Boolean()`转型函数           


    | 数据类型  | 转换为true的值       | 转换为false的值           |
    | --------- | -------------------- | ------------------------- |
    | Boolean   | true                 | false                     |
    | String    | 非空字符串           | ""(不包括 " " 空格字符串) |
    | Number    | 非零数值（含无穷值） | 0、NaN                    |
    | Object    | 任意对象             | null（不含 { }）          |
    | Undefined |                      | undefined                 |
4. `Number`  
   - `Number`类型使用IEEE754格式表示整数和浮点值，也叫双精度值。  
所有的`Number`类型数值都使用 IEEE754 64位格式存储，但是对`Number`类型数值使用位操作时，会先把值转换位32位整数，再进行位操作，之后再将结果转换为64位。
        ```
            八进制
            let octalNum1 = 070//八进制的56
            let octalNum2 = 079//无效的八进制，当成79处理
            严格模式下，应使用前缀0o;
            let octalNum3 = 0o79//八进制的56
            十六进制
            let octalNum4 = 0x1f;
            浮点值的定义中必须包含小数点
            let octalNum5 = 0.1//浮点值0.1
            let octalNum6 = 1.0//当成整数1
            科学计数法表示浮点值
            let octalNum7 = 3.125e7//31250000 
            注： 
            console.log((0.1+0.2)==0.3)//false        
        ```

 

   -  值的范围   
 `Number.MIN_VALUE`内存储了最小值`5e-324`，`Number.MAX_VALUE`内存储了最大值`1.7976931348623157e+318` ，超出了最大值的会自动转换为一个特殊的`Infinity`,可以通过`isFinite()`判断一个数是否有限大。  

        ```language
            let result = Number.MIN_VAULE + Number.MIN_VAULE;
            console.log(isFinite(result));//false  
        ```
        
   -  NaN  
`NaN`表示本来要返回数值的操作失败了（不是抛出错误）；存在以下属性：  
任何涉及`NaN`的操作返回`NaN`  
`NaN`不等于包括`NaN`在内的任何值  
存在一个判断返回数值是否是`NaN`的方法`isNaN()`


         ```    
             console.log(0/0);//NaN
             console.log(5/0);//Infinity
             console.log(NaN == NaN);//false
             console.log(NaN === NaN);//false
             console.log(isNaN(NaN));//true
         ```
   -  数值转换  
3个转型函数 `Number()`、`parseInt()`、`parseFloat()`
         -  `Number()`函数转换规则  
  布尔值，`true => 1  false => 0`  
  数值，直接返回  
  `null => 0`  
  `undefined => NaN`  
  字符串
             ```
             如果字符串包含数值字符，包括前面带加减号的情况，转换为一个10进制数     值；
             如果字符串包含有效的浮点值格式如"1.1"，返回相应的浮点值，（忽略前面    的零）；
             如果字符串包含有效的十六进制格式如"0xf",返回十六进制值对应的十进制     整数值；
             如果是空字符串，返回0；
             如果字符串包括上述之外的情况，返回NaN；
             console.log(Number(true));//1
             console.log(Number(false));//0
             console.log(Number(null));//0
             console.log(Number(undefined));//NaN
             console.log(Number("-10"));//-10
             console.log(Number("1.1"));//1.1
             console.log(Number("0xf"));//15
             console.log(Number(""));//0
             console.log(Number("string"));//NaN
             ```
         -  parseInt()  
            ```
            let num1 = parseInt("1234blue");//1234   
            let num2 = parseInt("");//NaN
            let num3 = parseInt("0xA");//10 
            let num4 = parseInt("22.5");//22 
            let num5 = parseInt("0xAF",16);//175 
            let num6 = parseInt("AF",16);//175 
            let num7 = parseInt("AF");//NaN
            let num7 = parseInt("1e3");//1
            ```
        - parseFloat()  
            ```
            let num1 = parseFloat("1234blue");//1234，按整数解析       
            let num2 = parseFloat("0xA");//0
            let num3 = parseFloat("22.34.5");//22.34
            let num4 = parseFloat("3.125e7");//31250000
            ```
5. `String`  
  字符串可以用使用单引号（ ‘ ）、双引号（ " ）和反引号（ \` ）标识。  
  字符串是不可变的，一旦创建，其值就不能改变。  
  `let lang = 'Java';  lang = lang + "Script"`
  对与上一行的代码，变量`lang`一开始先创建并初始化为`Java`；紧接着，`lang = lang + "Script"`时，先为变量分配一个足够容纳10个字符的空间，然后填充上`Java`和`Script`，再将原始的字符串`Java`和`Script`删除。  
     - 转换为字符串  
       -  `.toString()`  
        `.toString()`方法可以用于数值、布尔值、对象和字符串值，`null`和`undefined`没有`.toString()`方法。  
             ```
             console.log((10).toString());//"10"
             console.log(("string").toString());//"string"
             console.log((true).toString());//"true"
             console.log(({age:12,name:'nike'}).toString())//"[object Object]"
             ```  


        
             `.toString()`方法只在数值调用时，接收一个底数参数：  
             `console.log((10).toString(2));//"1010"`  
       - String( )函数  
            如果值有`.toString()`方法，则调用该方法。  
            `null => "null";  undefined => "undefined" `
      - 模板字面量  
        模板字面量可以定义字符串，并保留换行字符和格式。
        模板字面量还支持字符串插值,模板字面量中的`${}`内的内容会进行计算，并将结果调用`String()`转换为字符串插入。
         ```
         let str = `first line
                    second line`;//换行符后有19个空格
         console.log(str.length)//41       
  
         let value = 5;
         let result = value * value;
         let print = `${value} to the second power is ${result}`;
         console.log(print);//5 to the second power is 25
         ```     

        

      -  标签函数

          ```
          let a = 6, b = 9;
          function simpleTag(strings, aValExprebValExpression,            sumExpression)  {
              console.log(strings);
              console.log(avalExpression);
              console.log(bvalExpression);
              console.log(sumExpression);
              return "foobar";
          }
          let untaggedResult = `${a} + ${b} = ${a+b}`;
          let taggedResult = simpleTag`${a} + ${b} = ${a+b}`;
          // ["", " + ", " = ", "", raw: Array(4)]
          //6
          //9
          //15 
          console.log(untaggedResult);
          console.log(taggedResult);
          //6 + 9 = 15
          //foobar

          ```
6. `Symbol`  
    `Symbol`是ES6新增的数据类型，是原始值，且符号实例是唯一的、不可变的。  
    符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。  

        ```
        //普通符号实例的创建,Symbol()不能与new一起使用
            let sym = Symbol();//Symbol()
            let sym2 = Symbol("foo");//Symbol(foo)
            let sym3 = Symbol("foo");//Symbol(foo)
            console.log(sym2 == sym3);//false


            //全局符号注册表，全局符号实例
            let sym4 = Symbol.for("foo");//创建一个新的全局符号
            let sym5 = Symbol.for("foo");//返回已有的全局符号
            console.log(sym4 == sym5);//true
            console.log(sym2 == sym4);//false


            //查询全局注册表
            console.log(Symbol.keyFor(sym5));//foo


            //使用符号作为属性
            let obj = {
                baz:'baz val'
                [sym]:'foo val'
            }
            Object.defineProperties(obj, {
                [sym2]: { value: 'baz val' },
                [sym4]: { value: 'qux val' }
            })
            obj[sym2] = 'baz val 2';//无法修改已有属性
            obj[sym4] = 'qux val 2';//无法修改已有属性
            obj[sym5] = 'qux val 3';//无法修改已有属性
            console.log(obj);
            //{Symbol(): "foo val", Symbol(foo): "baz val", Symbol(foo): "qux val"}
            console.log(typeof obj[sym4]);//string
            console.log(obj[sym2]);//baz val

            //Object获取key
            console.log(Object.getOwnPropertyNames(obj));
            console.log(Object.getOwnPropertySymbols(obj));
            console.log(Object.getOwnPropertyDescriptors(obj));
            console.log(Reflect.ownKeys(obj));
            //["baz"]
            //[Symbol(), Symbol(foo), Symbol(foo)]
            //{baz: {…}, Symbol(): {…}, Symbol(foo): {…}, Symbol(foo): {…}}
            //["baz", Symbol(), Symbol(foo), Symbol(foo)]

        ```
7. `Object`  
    每个`Object`实例都有如下属性和方法,这些属性和方法位于原型上，每个实例都可通过原型链访   问。  
    `constructor` => `Object()`函数  
    `hasOwnProperty(propertyName)`判断当前对象实例上（不延伸原型）上是否有给定的属性，    `propertyName`为字符串或者符号。  
    `isPrototypeOf(object)`判断当前对象是否为另一个对象的原型。  
    `propertyIsEnumerable(propertyName)`判断给定的属性是否可以使用`for-in`枚举，    `propertyName`必须为字符串。  
    `toLocalString()`返回对象的字符串表示,反应对象所在的本地化执行环境。  
    `toString()`返回对象的字符串表示，不同环境可以通用。
***
## 操作符 
- `==` 和 `===`
  - `==` 操作符会先进行类型转换再确定操作数是否相等。  
转换操作数的类型时，遵循如下规则：  
    - 如过任一操作数是布尔值，则将操作数转换为数值再比较是否相等，`true => 1, false => 0`；   
    - 如果一个操作数是字符串，另一个是数值，则将字符串转换为数值再比较。  
    - 如果一个操作数是对象，另一个操作数不是，则调用`valueOf()`方法，取得原始值再根据之前的规则比较。  

      进行比较时会遵循如下规则：  
    - `null` 和 `undefined`相等；  
    - `null` 和 `undefined` 不能转换为其他类型的值再进行比较。  
    - 如果任一操作数是`NaN`，则返回`false`；`NaN == NaN`同样返回`false`；
    - 如果两个操作数都是对象，比较它们是不是同一个对象，如果两个操作数指向同一个对象，则返回`true`，否则返回`false`;
  - `===` 操作符比较时不执行类型转换，只有两个操作数在不转换的前提下相等才返回`true`。  
  `NaN === NaN => false`  
  `null === undefined => false`

- 逻辑与 `&&` 和逻辑或 `||`   
  - 逻辑与操作符可用于任何类型的操作数，不限于布尔值，如果有操作数不是布尔值，则逻辑与不一定返回布尔值而是遵循如下规则：  
    - 逻辑与是一种短路操作符,逻辑与操作会先对第一个操作数进行`Boolean()`转换；
    - 如果转换结果为`false`，直接返回转换前的第一个操作数，而不再理会第二个操作数，即使第二个操作数存在错误也不会报错。
    - 如果转换结果为`true`，直接返回第二个操作数，如果第二个操作数存在错误则会报错。
    ```language
      console.log(0 && { age: 12 });
      console.log(typeof (0 && { age: 12 }));
      console.log(2 && { age: 12 });
      console.log(true && null)；
      let found = false;
      let result = (found && undefinedValue);//不会报错
      let found = true;
      let result = (found && undefinedValue);//会报错
    ```
  - 逻辑或操作与逻辑与操作类似，如果有操作数不是布尔值，则逻辑与不一定返回布尔值而是遵循如下规则：
    - 逻辑或同样是一种短路操作符,逻辑或操作也会先对第一个操作数进行Boolean()转换。
    - 如果转换结果为`true`，直接返回转换前的第一个操作数，而不再理会第二个操作数，即使第二个操作数存在错误也不会报错。
    - 如果转换结果为`false`，直接返回第二个操作数，如果第二个操作数存在错误则会报错。
***
## 变量、作用域与内存 
- 传递参数  
ECMAscript 中所有的参数都是按值传递的。如果参数是原始值，则传递参数的副本，函数内部修改参数，不会影响函数外部的原始值。如果参数是引用值，则传递的是参数所对应的对象的地址，也就是传递指针，在函数内部修改参数，会影响外部的引用值，因为两者指向的是同一个对象。
- `var let const`
  - var 可以冗余声明变量，不会报错，在全局作用域中定义的变量会成为`window`的属性,let则不能冗余声明变量，全局作用域中声明的变量也不会变成`window`的属性。
  - var 定义的变量会被提升到作用域的最顶端，let声明不会提升，存在暂时性死区，即不能再声明前使用变量。
  - 在使用`with`增强作用域链时,在`with`块中使用的`var`定义的变量可以在块外访问,而`let`定义的变量则不能;  

    ```
     let qs =  "?debug=true";
     with(location){
         var url = href + qs;
         let url2 = href + qs;
         console.log(url);
         console.log(url2);
     }
    console.log(url);
    console.log( url2);
    //http://127.0.0.1:5501/background%E5%B1%9E%E6%80%A7.html?debug=true
    //http://127.0.0.1:5501/background%E5%B1%9E%E6%80%A7.html?debug=true
    //http://127.0.0.1:5501/background%E5%B1%9E%E6%80%A7.html?debug=true
    //Uncaught ReferenceError: url2 is not defined
    ```
  - 函数内定义变量时，省略关键字会定义全局变量
  - for循环使用var声明循环变量时，会渗透到循环体外部，而let不会
  - const声明变量的同时必须初始化变量，不能对const声明的变量进行赋值，但const声明的对象可以修改内部的属性和方法，这是由于const锁定的是变量所保存的对象的地址，而不是地址所对应的内容。
  - 声明变量优先使用const、let，尽量不适用var。 
- 执行上下文与作用域
  - 变量或函数的上下文决定了它们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象，这个上下文中定义的所有变量和函数都存在于这个对象上。
  - 全局上下文是最外层的上下文，在浏览器中，就是`window`对象。  
  每个函数调用都有自己的上下文，当代码执行流进入函数时，函数的上下文被推到一个上下文栈上，函数执行完毕后，上下文栈会弹出该函数上下，将控制权返回之前执行的上下文。
  - 上下文中的代码在执行时会创建变量对象的一个作用域链，正在执行的上下文的变量对象始终位于作用域链的最前端，全局上下文位于作用域链的最后端。如果上下文是函数，则其活动对象用做变量对象, 活动对象最初只有一个定义变量:`arguments`(全局上下文中没有这个变量),下一个对象来自包含上下文的变量对象,在下一个对象来自在下一个包含上下文.以此类推,直至全局上下文的变量对象.
    ```
      var color = 'blue';
        function changeColor(color) {
            let anotherColor = "red";
            function swapColors(){
                let tempColor = anotherColor;
                anotherColor = color;
                color = tempColor;
                // 这里可以访问 color anotherColor 和 tempColor 
            }
            // 这里可以访问 color anotherColor, 不可访问 tempColor
            swapColors();           
        }
        // 这里只能访问 color
        changeColor();    
    ```

    以上代码涉及3个上下文:全局上下文,`changeColor()`的局部上下文和`swapColors()`的局部上下文.其中`swapColors()`的局部上下文的作用链中有3个对象:`swapColors()`的变量对象, `changeColor()`的变量对象和全局变量对象.`changeColor()`的局部上下文的作用链中有2个对象:`changeColor()`的变量对象和全局变量对象.
- 作用域链增强  
  某些语句会导致再作用域链前端临时添加一个上下文:
  - `try/catch`语句的`catch`块
  - `with`语句
  
- 垃圾回收  
  JavaScript是使用垃圾回收的语言,执行环境负责在代码执行时管理内存,通过自动内存管理实现内存分配和闲置资源回收.垃圾回收程序周期性运行,确定哪个变量不会使用后,释放它占用的内存.  
  垃圾回收在标记未使用的变量时有两种主要的策略:标记清理和引用计数.
  - 标记清理  
   当变量进入上下文,如在函数内部声明一个变量时,这个变量会被加上存在与上下文中的标记,而当变量离开上下文时,也会加上离开上下文的标记.  
   垃圾回收程序运行时,会标记内存中所有存储的变量,然后将所有在上下文中的变量和被上下文中的变量引用的变量的标记去掉,之后再被加上标记的变量就是待删除的.  
   - 引用计数 
  对每个值都记录它被引用的次数,声明变量并赋一个引用值时,引用数为1,如果同一个值又被赋给另一个变量,引用数加1,如果保存对该值引用的变量被其他值覆盖,引用数减1,引用数为0时,下次垃圾回收程序运行时,就会释放该内存.  
  循环引用:  
     ```
     function problem(){
         let objA = new Object();
         let objB = new Object();
         objA.otherObj = objB;
         objB.otherObj = objA;
     }
     //objA和objB的引用数都为2,它们的引用数永远不会变成0;
     ```

  现代垃圾回收程序会基于对JavaScript运行时的环境的探测来决定何时运行,一般都是根据已分配对象的大小和数量来判断.
- 内存管理  
  优化内存占用的最佳手段就是保证在执行代码时,只保存必要的数据,如果数据不再必要,那么把它设置为`null`.  
  - 隐藏类和删除操作  
    Chrome浏览器使用V8 JavaScript引擎,运行期间V8会将创建的对象与"隐藏类"关联起来,以跟踪它们的属性特征.   
    隐藏类:具备相同属性和方法的一类对象.
    ```
    function Article(){
        this.title = 'title';
        this.version = 1921;
    }
    let a1 = new Article();
    let a2 = new Article();
    let a3 = new Article();
    let a4 = new Article();
    //此时a1 a2 a3和a4共享相同的隐藏类
    a2.author = 'Nike';
    //为a2动态赋值构造函数之外属性后,a1和a2对应两个不同的隐藏类;
    delete a3.version;
    //使用delete删除共同的属性或方法后,a1和a3对应两个不同的隐藏类;
    a4.version = null;
    //将共同的属性赋值为null时,a1和a4仍共享相同的隐藏类;
    //因此,最佳实践是将不想要的属性设置为null;
    ```  
    浏览器决定何时运行垃圾回收程序的一个标准就是对象更替的速度,可以通过创建对象池和静态分配的方法减少垃圾回收程序的频率,改善浏览器的性能.

- 内存泄露  
  常见情况:  
  声明局部变量时,忘记加上关键字,声明了全局变量;  
  定时器的回调通过闭包引用了外部的变量;
    ```
    let name = 'nike';
    setInterval(()=>{
        console.log(name);
    },100);
    ```
***
## 基本引用类型  
引用值(或者对象)是某个特定引用类型的实例.  
引用类型是把数据和功能组织到一起的结构,经常被人错误地称为"类";  
### `Date ` 
   - `Date`类型将日期保存为自UTC时间1970年1月1日零时至今所经过的毫秒数.  
   创建Date类型实例:  
   `let now = new Date();`  
   不给`Date`构造函数传参数的情况下,创建的对象将保存当前日期和时间.  
   要基于其他时间创建日期对象,必须传入其毫秒表示,传入其他表示日期的字符串,`Date`会在后台调用`Date.parse()`进行解析.  
   -  `Date.parse()`接受一个表示日期的字符串,返回日期所对应的毫秒数.  
   "月/日/年",  
   "2019-05-23T00:00:02"  
   "May / 23, 2019 00:00:01"  
   "5 23, 2019 00:00:01"   
   "2019 05 23 00:00:01"  
   "05/23/ 00:00:01"    
   - `Date.UTC()`方法与`Date.parse()`相同,但月数从0开始,返回的是本地时间而不是GMT时间.  
   - `Date.now()`方法,返回表示方法执行时日期和时间的毫秒数.

### `RegExp`
   - 创建正则表达式  
        ```
        let exp = /pattern/flag;
        let exp2 = new RegExp("pattern",flag);  
        ```   
        `pattern`:表示要匹配的正则表达式;
        正则表达式中的特殊字符 ：  
        - \b：字符边界
        - \B：非字符边界  
        - \s：空格  
        - /^a/：表示匹配输入的开始的a。如果多行标志被设置为 true，那么也匹配换行符后紧跟的位置;  
        - [^xyz]：反向字符集， 它匹配任何没有包含在方括号中的字符;
        - [xyz]：一个字符集合，匹配方括号中的任意字符；
        - /t$/：匹配输入结束的t,如果多行标志被设置为 true，那么也匹配换行符前的位置;
        - \*：匹配前一个表达式 0 次或多次。等价于 { 0 , }；
        - \+：匹配前面一个表达式 1 次或者多次。等价于 { 1 , }；
        - ?：匹配前面一个表达式 0 次或者 1 次。等价于 {0,1}；  
  如果紧跟在任何量词 *、 +、? 或 {} 的后面，将会使量词变为非贪婪（匹配尽量少的字符），和缺省使用的贪婪模式（匹配尽可能多的字符）正好相反。例如，对 "123abc" 使用 /\d+/ 将会匹配 "123"，而使用 /\d+?/ 则只会匹配到 "1"。  
        - [\b]：匹配一个退格(U+0008)
        - \d：匹配一个数字。等价于[0-9]；
        - \D：匹配一个非数字字符。等价于[^0-9]；

          
        `flag`:表示匹配模式的一个字符;  
        - `g`:全局模式,表示查找所有字符串的全部内容;    
        - `i`:忽略大小写;  
        - `m`:多行模式,匹配到一行文本末尾时会继续查找;   
        - `u`:启动`Unicode`匹配  
        - `s`:`dotAll`模式,表示元字符`.`可以匹配任何字符(包括\n或\r);  
   - RegExp实例的方法；
     - `array = reg.exec(str)`  
       - `reg`：正则表达式；  
       - `str`：用于匹配的字符串；
       - `array`：返回的一个数组，包含两个额外的属性；  
  `array.input === str`；  
  `array.index`：匹配字符串的起始位置；  
  数组的第一个元素是匹配整个模式的字符串，其他元素是与表达式中的捕获组匹配的字符串。

        ```language
        let text = "mom and dad and baby";
        let pattern = /mom( and dad( and baby)?)?/gi;
        let matches = pattern.exec(text);
        console.log(matches.index);
        console.log(matches.input);
        console.log(matches[0]);
        console.log(matches[1]);
        console.log(matches[2]);
        //0
        //mom and dad and baby
        //mom and dad and baby
        //and dad and baby
        //and baby
        ```  
        - 正则表达式如果没有设置全局模式，则每次只会返回第一个匹配项的信息,此时`lastIndex`始终保持为0。如果设置了全局模式，每次调用`exec()`会向前搜索下一个匹配项。   
         ```
        let text = "cat, bat, sat, fat";
        let pattern = /.(a)(t)/g;
        let matches = pattern.exec(text);
        console.log(matches.index);//0
        console.log(matches[0]);//cat
        console.log(pattern.lastIndex);//3
        matches = pattern.exec(text);
        console.log(RegExp.input);//cat, bat, sat, fat
        console.log(RegExp.lastMatch);//bat
        console.log(RegExp.leftContext);//cat,
        console.log(RegExp.rightContext);//, sat, fat
        console.log(RegExp.lastParen);//t
        console.log(RegExp["$_"]);//cat, bat, sat, fat
        console.log(RegExp["$&"]);//bat
        console.log(RegExp["$`"]);//cat,
        console.log(RegExp["$'"]);//, sat, fat
        console.log(RegExp["$+"]);//t
        console.log(matches.index);//5
        console.log(matches[0]);//bat
        console.log(pattern.lastIndex);//8
        matches = pattern.exec(text);
        console.log(matches.index);//10
        console.log(matches[0]);//sat
        console.log(pattern.lastIndex);//13
        ```  
      - `boolean = pattern.test(text)`  
  用于测试字符串`text`是否与`pattern`相匹配，如果匹配则返回`true`；
      - `RegExp`构造函数的属性：
        

        | 全名                  | 简写                                           | 说明                                       |
        | --------------------- | ---------------------------------------------- | ------------------------------------------ |
        | `RegExp.input`        | $_                                             | 最后搜索的字符串                           |
        | `RegExp.lastMatch`    | $&                                             | 最后匹配的文本                             |
        | `RegExp.lastParen`    | $+                                             | 最后匹配的捕获组                           |
        | `RegExp.leftContext`  | $\`|`input`字符串中出现在`lastMatch`前面的文本 |
        | `RegExp.rightContext` | $'                                             | `input`字符串中出现在`lastMatch`后面的文本 |
### 原始值包装类型   
   为了方便操作原始值，ECMAScript提供了三种特殊的引用类型：Boolean、Number 和 String，每当用到某个原始值的方法或者属性时，后台都会创建一个相应原始值包装的对象，调用相关的方法或者属性，整个过程分为3步：  
   （1）创建一个原始包装类型的实例；  
   （2）调用实例上的特定方法；   
   （3）销毁实例。   
   引用类型与原始值包装类型的主要区别在于对象的生命周期。通过`new`实例化后，一般引用类型的实例会在离开作用域时被销毁，而原始值包装类型的实例只存在于原始值调用属性或者方法的那行代码执行期间。

- `Boolean`  
     ```language
          let objFalse = new Boolean(false);//本质上是对象
          let result = falseFalse && true;
          console.log(result);//true;
          let falseValue = false;
          let result = falseFalse && true;
          console.log(result);//false;

          console.log(typeof objFalse);//object
          console.log(typeof falseValue);//boolean
          console.log(objFalse instanceof Boolean);//true
          console.log(falseValue instanceof Boolean);//false
     ```
- `Number`   
      - 与`Boolean`原始值包装类型一样，`Number`的实例本质上也是`object`; 
      - `number.toFixed(num)`返回`num`位小数位数的数值；
      - `number.toExponential(num)`返回`num`位小数位数的科学记数法形式的数值；
      - `number.toPrecision(num)`返回`num`位总位数（不含指数）的数值，可能是固度，也可能是科学记数法形式；本质上还是调用`number.toFixed(num)`或者`numbetoExponential(num)`;
      ```
      console.log((10.005).toFixed(2));//10.01
      console.log((10).toExponential(1));//1.0e+1
      console.log((99).toPrecision(1));//1e+2
      console.log((99).toPrecision(2));//99
      console.log((99).toPrecision(3));//99.0
      
      ```

      - `Number.isInteger(number)`方法，用于判断`number`是否是一个整数。  
            `console.log(Number.isInteger(1.00));//true`  
      - `Number.isSafeInteger(number)`方法，用与判断`number`是否在`Number.MIN_SAFE_INTEGER(-2^53+1)到Number.MAX_SAFE_INTEGER(2^53-1)`之间。 
- `String`   
    - 与前面两种原始值包装类型一样，`String`的实例本质上也是`object`; 
    - `.length`属性，返回字符串中字符的数量，字符串中的双字节字符也会按照单字符来计数。  
          `console.log("我的".length);//2`;
     - `JavaScript`字符  
            JavaScript字符串使用了两种Unicode编码混合的策略：UCS-2和UTF-16；对于可以采用16位编码的字符（U+0000~U+FFFF），两种编码一致。对于增补平面的字符，每个字符使用两个16位码元，称为**代理对**；  
        - `str.charAt(num)`，返回字符串`str`中索引为`num`的字符；
        - `str.charCodeAt(num)`，返回字符串`str`中索引为`num`的字符的Unicode编码；  
        - `String.fromCharCode(num)`，返回Unicode编码为`num`的字符；   
          上述的方法和属性都只针对U+0000~U+FFFF范围内的字符，对于存在代理对的字符串会出现问题，因为这些方法都是针对16位码元一个字符的情形。  
          对于存在代理对的字符串使用如下方法：
        - `str.codePointAt(num)`,类似`str.charCodeAt(num)`，只是会识别代理对。
        - `String.fromCodePoint(num)`，类似`String.fromCharCode(num)`，只是会识别代理对。
        - `str.normalize()`方法，对字符串进行规范化，接收一个代表规范化形式的字符串。Unicode提供了四种规范化形式：NFD,NFC,NFKD,NFKC;
            ```
            let message = "ab😊de";
            console.log(message.length)//6
            console.log(message.charAt(1));//b
            console.log(message.charAt(2));//<?>
            console.log(message.charAt(3));//<?>

            console.log(message.charCodeAt(1));//98
            console.log(message.charCodeAt(2));//55357
            console.log(message.charCodeAt(3));//56842
            console.log(message.codePointAt(2));//😊

            console.log(String.fromCharCode(0x1F60A));//            
            console.log(String.fromCodePoint(0x1F60A));//😊
            
            //迭代字符串可以智能识别代理对
            console.log([..."ab😊de"]);//["a","b","😊","d","e"]  

            //normalize();
            //有些字符存在多种Unicode编码。
            console.log(String.fromCharCode(0x00c5));//Å
            console.log(String.fromCharCode(0x212B));//Å
            console.log(String.fromCharCode(0x0041,0x030A));//Å
            //U+00C5是对U+212B进行NFC/NFKC规范之后的结果；
            //U+0041/U+030A是对U+212B进行NFD/NFKD规范之后的结果；

            console.log(String.fromCharCode(0x00c5) == String.fromCharCode(0x212B));//false;

            console.log(String.fromCharCode(0x00c5) == (String.fromCharCode(0x212B).normalize("NFC")));//true;   
            ```
    - 字符串操作方法  
      1. `str.concat(substr)`,返回`str`和`substr`拼串的结果，原字符串`str`不变；该方法可以接收多个字符串参数，并将所有的字符串拼接起来。
          ```language
          let str = 'hello ';
          let result = str.concat("world!");
          console.log(result);//"hello world!"
          console.log(str);//"hello "
          console.log(str.concat("world","!"));//"hello world!"
          ```

             
      2. `str.slice(), str.substr(), str.substring()`，这三个方法都返回调用方法的字符串的子串，且都接收一个或者两个参数，第一个参数表示子字符串开始的位置，对`str.slice(), str.substring()`而言，第二个参数是提取结束的位置，对`str.substr()`，第二个参数是子字符串的长度。如果省略第二个参数，三个方法都会提取到字符串的末尾。  
      三个方法都不会对原始字符串进行任何修改；  
      当某个参数是负值时，`str.slice()`方法会将所有负值参数都当成原字符串长负值，`str.substr()`方法将第一个负参数值当成原字符串长度加上负值，将负参数当成0；`str.substring()`会将所有的负参数值都当成0。
           ```
           let str = 'hello world';
           console.log(str.slice(3));//"lo world"
           console.log(str.substr(3));//"lo world"
           console.log(str.substring(3));//"lo world"
           
           console.log(str.slice(3,7);//"lo w"
           console.log(str.substr(3,7);//"lo w"
           console.log(str.substring(3,7);//"lo w
           console.log(str.slice(-3));//"rld"
           console.log(str.substring(-3));//"rld"
           console.log(str.substr(-3));//"rld"
           console.log(str.slice(3,-4));//"lo w"
           console.log(str.substring(3,-4));//"hel" 
           //str.substring()方法会比较两个参数大小，并调整位置
           console.log(str.substr(3,-4));//""              
           ```  

      3.  `str.indexOf()和str.lastIndexOf()`，两个方法都用于在`str`中搜索子串，并返回子串在原串中的起始字符的索引，未找到则返回-1，`str.indexOf()`从前往后搜索，`str.lastIndexOf()`从后往前搜索。两个方法都可以接收第二个参数，表示开始搜索的位置。  
          ```
          let str = 'hello world';
          console.log(str.indexOf("o"));//4
          console.log(str.lastIndexOf("o"));//7
          
          console.log(str.indexOf("o",6));//7
          console.log(str.lastIndexOf("o",6     4                            
          ```

        4. `str.startsWith()、str.endsWith()和str.includes()`，这三个方法都用于判断字符串中是否包含另一个字符串，并返回一个布尔值。`str.startsWith()和str.includes()`方法接收可选的第二个参数，表示开始搜索的位置。 `str.endsWith()`，接收可选的第二个参数，表示当作字符串末尾的位置。
              ```
              let str = "foobarbaz";
              console.log(str.startsWith("foo"));//true
              console.log(str.includesWith("bar"));//true
              console.log(str.endsWith("baz"));//true
              console.log(str.startsWith("foo",1));//false
              console.log(str.includesWith("bar",4));//false              
              console.log(str.endsWith("bar",6));//true             
              ```  

        1. `str.trim()`，创建一个字符串的副本，删除前、后所有的空格，再返回结果；  
                `str.trimLeft()`和`str.trimRight()`，分别用于从字符串开始和末尾清理空格符；

        2. `str.repeat(num)`,表示要将字符串复制`num`次并拼接返回结果。  
                `str.padStart()和str.padEnd()`方法，复制字符串，如果字符串长度小于指定长度则再相应一边填充,如果指定长度小于原字符串长度，默认返回原字符串。接收两个参数，第一个参数是指定长度，第二个参数是可选的填充字符串，默认为空格。
              ```
              console.log(("foo").padStart(6)); //"   foo"
              console.log(("foo").padStart(9,".")); //"......foo"
              console.log(("foo").padEnd(6)); //"foo   "
              console.log(("foo").padEnd(6,".")); //"foo..."
              console.log(("foo").padStart(8,"bar")); //"barbafoo"
              console.log(("foo").padEnd(2,"bar")); //"foo"
              ```  
        3. 字符串迭代与解构  
              ```
              let str = "abc";
              //手动迭代
              let strIterator = str[Symbol.iterator]();
              console.log(strIterator.next());//{value:"a"done:false}            
              console.log(strIterator.next());//{value:"b"done:false}            
              console.log(strIterator.next());//{value:"c"done:false}            
              console.log(strIterator.next());//{value:undefineddone:true}
              //for-of
              for( const char of str){
                console.log(char);
              } 
              //a
              //b
              //
              //解构
              console.log([...str]);//["a","b","c"]                
              ```

        1. `str.match(pattern)`，本质上跟RegExp对象的`exec()`方法相同，只接受一个参数，正则表达式字符串或者RegExp对象。
              ```
              let str = "cat, bat, sat, fat";
              let pattern = /.at/;
              let matches = str.match(pattern);
              console.log(maches.index);//0;
              console.log(maches.[0]);//"cat";
              console.log(maches.lastIndex);//0              
              ```  

             `str.search(pattern)` 这个方法返回模式第一个匹配的位置索引，没找到则返回-1；  
             `str.replace()`，用于子字符串替换操作；方法接收两个参数，第一个参数可以是字符串或者正则表达式，第二个参数可以是一个字符串或者函数。如果第一个参数是字符串，则只会替换第一个匹配的子字符串，要想替换所有匹配的位置，第一个参数必须是全局模式的正则表达式。  
            第二个参数是字符串的情况下，有几个特殊的字符序列，可以用来则表达式操作的值。 
            | 字符序列 | 替换文本                                          |
            | -------- | ------------------------------------------------- |
            | $&       | 匹配整个模式的子字符串，同RegExp.lastMatch        |
            | $`       | 匹配的子字符串之前的字符串，同RegExp.leftContext  |
            | $'       | 匹配的子字符串之后的字符串，同RegExp.rightContext |
            | $n       | 匹配第n个捕获组的字符串，n为0~9                   |
            | $nn      | 匹配第nn个捕获组的字符串，nn为00~99               |
            
 
            第二个参数是函数时，函数会收到所有的捕获组的字符串和三个参整个模式匹配的字符串、匹配项在字符串中的开始位置，以及整串。 

              ```
              let text = "cat, bat, sat, fat";
              let result = text.replace("at", "ond");
              console.log(result);//cond, bat, sat, fat
              result = text.replace(/at/g, "ond");
              console.log(result);//cond, bond, sond, fond

              result = text.replace(/(.at)/g, "word($1)");
              console.log(result);//word(cat), word(bat), word(sat), word(fat)

              text = "aaaa2bbbb";
              result = text.replace(/(a)([1-9])(b)/g, function (match, pos, originalText) {
                  console.log(arguments);
                  //Arguments(6) ["a2b", "a", "2", "b", 3, "aaaa2bbbb", callee: ƒ, Symbol(Symbol.iterator): ƒ]
                  return 0;
              });
              console.log(result);//aaa0bbb
              ```
              
          

            `str.split()`方法会根据传入的分隔符将字符串拆分成数组，作为分隔符的参数   可以是字符串，也可以是RegExp对象，还可以传入第二个参数，确保返回的数组不   会超过指定大小。  
             ```
            let colorText = "red,blue,green,yellow";
            console.log(colorText.split(","));
            //["red", "blue", "green", "yellow"]
            console.log(colorText.split(",", 2));
            //["red", "blue"]
            console.log(colorText.split(/[^,]+/));
            //["", ",", ",", ",", ""]
             ```

        1. `str.localeCompare(subStr)`，这个方法比较两个字符串，返回如下3个值中的一个。
              - 如果按照字母表顺序，字符串应该排在字符串参数前头，则返值；
              - 如果字符串与字符串参数相等，则返回0；
              - 如果按照字母表顺序，字符串应该排在字符串参数后头，则返值；

              ```
              let str = "yellow";
              console.log(str.localeCompare("brick"));//1
              console.log(str.localeCompare("yellow"));//0
              console.log(str.localeCompare("zoo"));//-1              
              ```

      

                


                     



              
           






  


  




