# Exceptions

#### Java ConcurrentModificationException####

​	对Vector、ArrayList在迭代的时候如果同时对其进行修改就会抛java.util.ConcurrentModificationException异常。

~~~java
public class Test {
    public static void main(String[] args)  {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(2);
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            Integer integer = iterator.next();
            if(integer==2)
                list.remove(integer);
        }
    }
}
~~~

​	　如果modCount不等于expectedModCount，则抛出ConcurrentModificationException异常。很显然，此时modCount为1，而expectedModCount为0，因此程序就抛出了ConcurrentModificationException异常。到这里，想必大家应该明白为何上述代码会抛出ConcurrentModificationException异常了。关键点就在于：调用list.remove()方法导致modCount和expectedModCount的值不一致。注意，像使用for-each进行迭代实际上也会出现这种问题。

~~~java
public class Test {
    public static void main(String[] args)  {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(2);
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            Integer integer = iterator.next();
            if(integer==2)
                iterator.remove();   //注意这个地方
        }
    }
}
~~~

在迭代器中如果要删除元素的话，需要调用Itr类的remove方法。