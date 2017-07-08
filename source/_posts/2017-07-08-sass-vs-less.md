title: sass vs less
date: 2017-07-08 07:42:06
categories:
tags:
---
sass
--

2007年发布，最早的一款CSS预处理器，带来了变量、常量、嵌套、混入、函数、循环等功能，
解决了CSS不可编程的短板。由于浏览器不能直接识别Sass，所以需要预先编译。

Sass有两套语法规则：

1. 缩进式

    
    body
        color: red
        header
            background: red
    
2. 大括号

    
    body{
        color: red
        header{
            background: red
        }
    }
    
less
--

2009年，受Sass影响，但使用的CSS语法，可以快速上手。它也有上面说的一些功能。

下面对比一些常用功能：

变量
--

变量是代码最小的复用单元，可以减少硬编码。

sass

    $color: blue;
    
    body{
        $color: black;
        color: $color;
    }
    
less 

    @color: blue;
    
    body{
        @color: black;
        color: @color;
    }
    
提到变量，就不得不提作用域。

    $color: black;
    .scoped {
        $bg: blue;
        $color: white;
        color: $color;
        background-color:$bg;
    }
    .unscoped {
        color:$color;
    }
    
sass在这里还是有点小问题的:

3.4.0之前：

    .unscoped{
        color: white
    }
    
之后：

    .unscoped{
        color: black;
    }
    
后声明的变量总是会覆盖之前声明的变量，不管之前变量在何处声明。也就是变量都是全局的，谁在后面谁是老大。

3.4.0（包括）之后，修正了这个问题：

>All variable assignments not at the top level of the document are now local by default. If there’s a global variable with the same name, it won’t be overwritten unless the !global flag is used. For example, $var: value !global will assign to $var globally. This behavior can be detected using feature-exists(global-variable-shadowing).

大概：变量不再是全局，而默认是局部，除非给变量声明时，加上 !global。

less就没有这个问题，作用域很其像他编程语言。

嵌套
--

sass/less 官都提到了写出的样式和 HMTL 的结构更加相像，层级很明了。

html

    <nav>
        <ul>
            <li>
                <a></a>
            </li>
        </ul>
    </nav>

sass / less

    nav{
        ul{
            li{
               a{
                                       
               } 
            }
        }
    }
    
其实这样嵌套深了也不一样好，毕竟最后也会转换为CSS，会导致选择器很长，从性能、代码体积上都会有损失。个人认为 3 到 4 层嵌套
就可以了，或者上面的也可以写成：

    nav{
        ul{
            
        }
        
        li{
        
        }
        
        a{
        
        }
    }

混入
--

一个规则集合可以导入到另一个规则集合，或者可以说大块的代码复用。适用场景：我们在写 CSS3 时，要加很多前缀（当然现在有好多工具可以自动添加）。

sass

    @mixin border-radius($radius:10px) {
      -webkit-border-radius: $radius;
         -moz-border-radius: $radius;
          -ms-border-radius: $radius;
              border-radius: $radius;
    }
    
    .box { 
        @include border-radius(10px); 
    }
    
less

    .border-radius(@radius:10px){
      -webkit-border-radius: @radius;
         -moz-border-radius: @radius;
          -ms-border-radius: @radius;
              border-radius: @radius;
    }
    
    .box{
        .border-radius(5px);
    }

sass 有严格的声明和引用语法：@mixin，@include，而less 就松散很多，同时less还有个问题：

    .mixin{
        color:red;
    }
    
    .other-mixin(){
        background:blue;
    }
    
    .class{
        .mixin;
        .other-mixin;
    }
    
output

    .mixin{
        color:red;
    }
    
    .class{
        color:red;
        background:blue;
    }
就是你在使用 mixin 时，不加小括号会把 mixin 也输出。因为 less 的编译器不知道你是在声明
mixin 还是在声明普通的选择器。


函数
--

sass/less 都各自内置了一些函数：Color、String、List、Math、Type等

sass/less

    @base: #f04615;
    @width: 0.5;
    
    .class {
       width: percentage(@width); // returns `50%`
       color: saturate(@base, 5%);
    }
    
output

    .class {
       width: 50%;
       color: #f6430f;
    }

最好的例子应该是 REM 布局中的 px2rem 函数了：

    $baseFontSize:100px !default;
    
    @function px2rem( $px , $base-font-size: $baseFontSize ){
        @if (unitless($px)) {
            @return px2rem($px + 0px);
    
        } @else if (unit($px) == rem) {
            @return $px;
        }
    
        @return $px/($base-font-size) * 1rem;
    }
    
    
    .body{
        font-size:px2rem(100)
    }

导入
--

sass/less

     @import "reset.css"
     
     @import "reset" // reset.less / reset.scss

运算
--

四则运算

    + - * /
    
sass
    
    $conversion-1: 5cm + 10mm; // result is 6cm
    $conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

less

    @conversion-1: 5cm + 10mm; // result is 6cm
    @conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

单位也是参与计算的

继承
--

sass

    .message{
        border:1px solid #ccc;
    }
    
    .success{
        @extend .message;
        border-color:green;
    }
    
    .error{
        @extend .message;
        border-color:red;
    }

output

    .message, .success, .error {
        border: 1px solid #ccc;
    }
    
    .success {
        border-color: green;
    }
    
    .error {
        border-color: red;
    }

less （1.4.0加入）

    .message{
        border:1px solid #ccc;
    }
    
    .success{
        &:extend(.message);
        border-color:green;
    }
    
    .error{
        &:extend(.message);
        border-color:red;
    }

output


    .message, .success, .error {
        border: 1px solid #ccc;
    }
    
    .success {
        border-color: green;
    }
    
    .error {
        border-color: red;
    }

注：extend 和 mixin 有时可以达到同样的功能，但是从语法和输出后的代码上还是有些区别的。

extend生成的代码更紧凑、更符合预期。
