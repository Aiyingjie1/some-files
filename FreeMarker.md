```html
<h1>Welcom ${user} <#if user == "Boss"> our leader
  <#else> not boss
  </#if></h1>
```

判断用#if,符合条件会输出<#if>中的信息;<#else>在<#if>中使用。

取值直接$取。



~~~html
<table>
  <tr>
    <th>name</th>
  </tr>
  <#list lists as persion>
    <tr><td>${persion.name}</td></tr>
  </#list>
</table>
~~~

使用list遍历，这里的lists是list的名称。





~~~html
<#include "/footer.html">
~~~

在一个页面中调用<#include>可以将另一个页面包含。



FreeMarker中不能出现不存在（null也不能出现）的变量，否则会报错。

~~~html
<h1>Welcom ${user!"AI"}</h1>    ${(user.name)!"AI"}--这样做防止user也不存在的情况
~~~

!"默认值"   处理了这个user这个值如果丢失，则会有默认值。

~~~html
<#if user??><h1>Welcom ${user}</h1></#if>
~~~

使用“对象”？？判断一个对象是否存在。

~~~html
<!--对于对象多级判断-->
<h1>${user.name.first}</h1> 需要进行空值处理
<h1><#if user.name.first!0>${user.name.first}</#if></h1>这样处理会存在问题，如果user.name不存在的话模版也会报错，然后停下。
<!--应该使用-->
<h1><#if (user.name.first)!0>${user.nemg.first}</#if></h1>此时就不会出现未定义变量而报错
<!--或者使用-->
<h1><#if user.name.first??>${user.nemg.first}</#if></h1>判断是存在。但未指定默认值
~~~





~~~html
<#list ["winter","spring","summer","autumn"] as X>
${X}  
</#list>
~~~

遍历一个序列。



~~~html
<#assign ages={"Joe":23,"Fred":25}+{"Joe":30,"Julia":18}>
  Joe is ${ages.Joe}
  Fred is ${ages.Fred}
  Julia is ${ages.Julia}
~~~

哈希表连接（有点像json）





test是一个字符串：dog&cat

~~~html
${test?upper_case?html}
~~~

FreeMarker内建函数，使用?代替.的方式调用方法。

输出为：DOG  "&""amp;" JERRY     &被HTML标准化输出

FreeMarker调用方法

~~~html
${repeat("What",3)}   ---repeat是一个方法，what是传入的一个字符串参数，3是一个int类型参数，
~~~

输出是：whatwhatwhat





~~~html
<#macro greet person color> ---在定义时可以指定默认值，例如在这里写person="A1"后面调用时会覆盖这个值
  <font0 size="+2" color="${color}">Hello ${person}</font>
</#macro>
~~~

使用#macro定义一个宏，其中greet是宏的名字。person，color是参数。

可以通过如下方式调用

~~~html
<@greet person="Fred" color="black"></@greet>
~~~

使用这个宏，@greet调用。传入参数person=“Fred” color="black"

不能传入定义宏时没有定义的参数。顺序不重要

宏与宏之间可以互相嵌套。







~~~html
<#macro do_thrice>
 <#nested>
 <#nested>
 <#nested>
</#macro>
   
   <@do_thrice>
      Anything  
   </@do_thrice>
~~~

使用<#nested>，当在使用宏，并在宏中插入了值，则会被显示。

即上面的显示结果是	Anything

​				  	 Anything

​				  	 Anything





