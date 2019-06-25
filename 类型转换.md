# 类型转换

##### int[]转list

Arrays.stream(int[]).boxed().collect(Collectors.toList());

eg:

```java
int[] elementNumbers = formationService.getElementNumbers(userInSession);
List<Integer> elementList = Arrays.stream(elementNumbers).boxed().collect(Collectors.toList());
```

##### **String转换为char[]**

```java
char[] chars1 = str.toCharArray();
```

##### char[]转换为String

```java
String str1 = new String(chars);			//通过String构造函数

String str2 = String.valueOf(chars);		//通过String类型转换，实际也是new String(char[])
```

##### 数组和List

``` java
List<Character> list1 = Arrays.asList(chars); //通过Arrays的工具类

List<Character> list2 = new ArrayList<>();   //通过Collections工具类
Collections.addAll(list2,chars);
```

##### List转换为包装类型数组

```java
List<Character> list = new ArrayList<>();
		list.add('1');
		list.add('2');
Character[] chars =  list.toArray(new Character[list.size()]);//注意toArray()的参数
```

##### String转List

~~~java
String str1 = "123456";
		//方式一
		List<String>list1  = Arrays.asList(str.split("")); //str.split()返回一个String[]数组
		//方式二
		List<String>list2 = new ArrayList<>();
		Collections.addAll(list2, str.split(""))
~~~

##### List转String

~~~java
public static void listToString(){
		List<String> list = Arrays.asList("1","2","3","4");
		String str = String.join("", list); //"1234"
}
//String.join 可以添加符号
~~~

##### List和Set互转

~~~java
 List<String> list = new ArrayList<>();
 Set<String> set = new HashSet<>(list);   //直接构造函数即可
同理，set转list也这样转
当String转set时，先分割成数组，再转list,再变成set.
set转String时，使用String.join()
~~~

