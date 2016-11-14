##SASS用法指南

学过[CSS](https://zh.wikipedia.org/wiki)的人都知道,它不是一种编程语言。

你可以在用它开发网页样式，但是没法用它编程。也就是说，CSS基本上是设计师的工具，不是程序员的工具。在程序员眼里，CSS是一件很麻烦的东西。它没有变量，也没有条件语句，只是一行行单纯的描述，写起来相当费事。

![pic](http://image.beekka.com/blog/201206/bg2012061901.jpg)

很自然地，有人就开始为CSS加入编程元素，这被叫做“CSS与处理器”(css preprocessor)。他的基本思想是，用一种专门的编程语言，进行网页样式设计，然后再编译成正常的CSS文件。

各种“CSS预处理器”之中，我自己最喜欢[SASS](https://www.baidu.com)，觉得它有很多优点，打算以后都用它来写CSS。下面是我整理的用法总结，供自己开发时参考，相信对其他人也有用。

＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝

###SASS用法指南

![pic2](http://image.beekka.com/blog/201206/bg2012061902.png)

**一、什么是SASS**

[SASS](https://www.baidu.com)是一种CSS的开发工具，提供了许多便利的写法，大大节省了设计者的时间，使得CSS的开发，变的简单和可维护。

本文总结了SASS的主要用法。我的目标是，有了这篇文章，日常的一般使用久不需要去看[官方文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)了。

**二、安装和使用**

**2.1安装**

SASS是Ruby语言写的，但是两者的语法没有关系。不懂Ruby，照样使用。只是必须先[安装Ruby](http://www.ruby-lang.org/zh_cn/downloads/)，然后再安装SASS。

假定你已经安装好了Ruby，接着在命令行输入下面的命令：

```

	gem install sass
	
```

在windows系统中，会出现安装不上的现象，原因是gem源（https://rubygems.org/）不起作用（被国内墙了），解决方法如下：

```

    先移除原来的源：
	gem sources --remove https://rubygems.org/
	
	再添加一个国内可用的源：
	gem sources -a http://gem.ruby-china.org/
	
	添加完成后，可以用一下命令查看添加的源有没有添加上且是唯一的：
	gem sources -l
	
	如果是，则可以输入安装sass的命令，若不是则重复上述的步骤。
	

```

在Mac OSX系统中，一般情况下会涉及到权限问题，因此在安装时要添加sudo命令：

```

	sudo gem install sass
	

```

回车会提示输入密码（用户密码，即开机密码），就可以进行安装。

若遇到安装不上的现象，可以参照windows系统的操作进行。


**2.2使用**

SASS文件就是普通的文本文件，里面可以直接食用CSS语法。文件后缀名是.scss，意思为Sassy CSS。

下面的命令，可以在屏幕上显示.scss文件转化的css代码。（假设文件名test。）

```

	gem test.scss
	

```

如果要将显示结果保存成文件，后面再跟一个.css文件名。

```

	gem test.scss test.css 或
	gem test.scss:test.css
	

```

**SASS**提供四个[编译风格](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#output_style)的选项：

```

	* nested：嵌套缩进的css代码，它是默认值。
	
	* expanded：没有缩进的、扩展的css代码。
	
	* compact：简洁格式的css代码。
	
	* compressed：压缩后的css代码。
	

```
生产环境当中，一般使用最后一个选项。

```

	sass --style compressed test.sass test.css
	

```

你也可以让SASS监听某个文件或目录，一旦源文件有变动，就自动生成编译后的版本。

```

	// watch a file 
    
    sass --watch input.scss:output.css
    
    // watch a directory

    sass --watch app/sass:public/stylesheets
	

```

**SASS**的官方网站，提供一个[在线转换器](http://sass-lang.com/try.html)。你可以在那里，试运行下面的各种例子。

**三、基本用法**

**3.1 数据类型**

**'Null'**
	null 是Sass中最基本的数据类型，它既不是true也不是false，而表示的是空。它没有任何值。任何变体的null，甚至是一个字母都不存在的情况也不会视为空。这也意味着NULL或Null实际上是null，他们都是字符串。尽管null表示什么都没有，但是使用length（..）还是会返回length为1.这是因为null仍然表示的是一个真实存在的实体，只不过它不代表任何东西。同样，你不能使用null来连接其他字符串，出于这个原因，你要是使用text＋null，在编译Sass的时候将会报错。
	
**'booleans'**
	这个数据类型只有两个值：true和false。在Sass中，只有自身是false和null才会返回false，其他一切都将返回true。
	
**'Number'**
	数字在CSS中使用很广泛，大部分都是结合CSS的单位一起使用，但是在技术上而言它依然算是数字。同样的，在Sass中也是有数字类型（Number）。如果一个数字和一个字符串相加，所得到的还是一个字符串，而若是把这相加的结果当作设置值，在编译时就会报错。

**'Strings'** 
	在CSS中字符串常常用于字体样式或其他的属性的样式。Sass中的字符串和CSS一样，在Sass中，使用单引号（‘’）或双引号（“”）包裹的都是字符串，就是他们包裹的是一个空格，那也是字符串。不过需要特别注意的是，如果没有使用‘’或“”在Sass中不会被认为是字符串，在实际使用中将会造成一定的错误。如果你想在一个字符串时使用一个变量，而你又不想让这个变量直接变成字符串，那就得使用到一个被称为变量插值，简单点说，就是使用#{}来包裹着歌变量，例如：

```

	$name: 'gajendar';
	$author: 'Author : $name';//'Author : $name'
    
    $author: 'Author : #{$name}';//'Author : gajendar'
   
```
插值方法在一些条件语句中使用变量的时候变得尤其的方便。

**'Colors'**
	CSS颜色表达式也是属于颜色数据类型，比如颜色的十六进制符号、rgb、rgba、hsl、hsla和使用关键词（如pink、blue）等等。Sass主要是给你提供一些额外的功能，这样你就可以更有效的使用颜色。比如你可以在Sass中添加颜色值。Sass具有分离颜色的所有通道，然后对应的颜色通道进行运算。比如，red颜色对应的十六进制值是#ff0000，green颜色对应的十六进制值是#008000，把他们添加到一起就是#ff8000。另外不能使用这样的方法来极损颜色的透明（α）通道的值。

**'Lists'**
	列表，其实就是Sass中的数组，它可以包含零个、一个或多个值，甚至是还可以包含多个子列表。在列表中创建不同的值时，你只需要使用空格或逗号分隔开就行，如下所示：
	
```

$font-list: 'Raleway','Dosis','Lato'; //Three comma separated elements

$pad-list: 10px 8px 12px; //Three space separated elements

$multi-list: 'Roboto',15px 1.3em; //This multi-list has two lists.


```
列表中可以存储多种类型的值，上述示例中，前两个列表各有三个元素，而后面的$multi-list列表中两个元素用逗号分隔开，这意味着，第一个元素时字符串Roboto，而第二个元素是另一个列表（$multi-list 的子列表），并且这个子列表包含两个元素，这两个元素是使用空格符分隔开的。也就是说，使用不同的分隔符在同一个列表中可以创建一个嵌套列表。当列表和循环一起使用的时候，可以证明列表是非常有用的。

**'Maps'**
	Sass中的Map其实就是类似于关联数组，常常以key/value对键值出现。Map必须用括号（ （） ）括起来，每对键值之间是用逗号分隔。在Map中，一个给定的key只能有一个相关的value，但是一个给定的value可以被映射到许多不同的key上。另外，在Map中映射给key的值value可以是任何数据类型，包括Map。

**3.2 变量**

**SASS**允许使用变量，所以变量以$开头。

```

	$blue : #1875e7; 
    
    div {
      color : $blue;
    }
	

```

如果变量需要镶嵌在字符之中，就必须需要写在**#{}**之中。

```

	$side : left; 
    
    .rounded {
      border-#{$side}-radius : 5px;
    }
	

```

**3.3 计算功能**

**SASS**允许在代码中使用算式：

```

    $var :x;
    body {
       magin: (14px/2);
       top: 50px + 100px;
       right: $var * 10%;
    }
	

```

**3.4 嵌套**

**SASS**允许选择器嵌套。比如，下面的**CSS**代码：

```

    div h1 {
       color: red;
    }
	

```

可以写成：

```

    div  {
       h1 {
           color: red;
       }
	}
    
```

属性也可以嵌套，比如**border-color**属性，可以写成：

```

    p {
       border: {
           color: red;
       }
	}
    
```

注意，**border**后面必须加上冒号。

在嵌套的代码块内，可以使用**&**引用父元素。比如**a:hover**伪类，也可以写成：

```

    a {
       &:hover { color: #ffb3ff; }
	}
    
```

**3.5 注释**

**SASS**共有两种注释风格。

标准的**CSS**注释**/* comment */**,会保留到编译后的文件。

单行注释 // comment，只保留在**SASS**源文件中，编译后被省略。

在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。


```

    /*!
       重要注释
    */

 
```

**四、代码的重用**

**4.1 继承**

**SASS**允许一个选择器，继承另一个选择器。比如，现有class1:


```

    .class1 {
       border: 1px solid #ddd;
	}
    
```

class2要继承class1,就要使用@extend：

```

    .class2 {
       @extend .class1;
       font-size:120%;
	}
    
```

**4.2 Mixin**

**Mixin**有点像C语言的宏（macro），是可以重用的代码块。

使用@mixin命令，定义一个代码块。


```

    @mixin left {
       float: left;
       margin-left: 10px;
	}
    
```

使用@include命令，调用这个mixin。

```

    div {
       @include left;
	}
    
```

**mixin**的强大之处，在于可以指定参数和缺省值。


```

    @mixin left($value: 10px) {
       float: left;
       margin-left: $value;
	}
    
```

使用的时候，根据需要加入参数：

```

    div {
       @include left（20px）;
	}
    
```

下面是一个mixin的实例，用来生成浏览器前缀。

```

    @mixin rounded($vert,$horz,$radius: 10px) {
       border-#{$vert}-#{$horz}-radius: $radius;
       -moz-border-radius-#{$vert}-#{$horz}:$radius;
       -webkit-border-#{$vert}-#{$horz}-radius: $radius;
	}
    
```

使用的时候，可以像下面这样调用：

```

    #navbar li { @include rounded(top, left); }
    #footer { @include rounded(top, left, 5px); }
    
```

**4.3 颜色函数**

**SASS**提供了一些内置的颜色函数，以便生成系列颜色。

```

   lighten(#cc3, 10%) // #d6d65c
   darken(#cc3, 10%) // #a3a329
   grayscale(#cc3) // #808080
   complement(#cc3) //#33c
       
```

**4.4 插入文件**

@import命令，用来插入外部文件。

```

	@import "path/filename.scss";
	    
```

如果插入的是.css文件，则等同于css的import命令。

```

	@import "foo.css";
	    
```

**五、高级用法**

**5.1 条件语句**

@if可以用来判断：


```

	p {
	   @if 1 + 1 == 2 { border: 1px solid; }
	   @if 5 < 3 { border: 2px dotted; }
	}
	    
```

配套的还有@else命令：

```
    
    $i-am-true: true;		
	 body {
		 @if not $i-am-true {
		   background: rgba(255, 0, 0, 0.6);
		 } @else {
		   background: rgba(0, 0, 255, 0.6); // expected
		 }
	 }//@if not 等价于if(!xxx)
		
    @if lightness($color) > 30% {
        background-color: #000;
    } @else {
        background-color: #fff;
    }
   
```

**5.2 循环语句**

**SASS**支持for循环：

```

	@for $i from 1 to 10 {
		.border-#{$i} {
           border: #{$i}px solid blue;
        }
    }
            
```

也支持while循环：

```

	$i: 6;
	
	@while $i > 0 {
	    .item-#{$i} { width: 2em * $i; }
	    $i: $i - 2;
	}  
	    

```

each命令，作用与for类似：

```

    @each $member in a, b, c, d {
        .#{$member} {
            background-image: url("/image/#{$member}.jpg");
         }
    }


```

**5.3 自定义函数**

**SASS**允许用户编写自己的函数。

```

    @function double($n) {
        @return $n * 2;
    }
    
    #sidebar {
    	width: double(5px);
    }


```

参考资料：

[sass官方文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

[sass用法指南 --阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/06/sass.html)

[w3cplus](http://www.w3cplus.com/blog/tags/302.html)











