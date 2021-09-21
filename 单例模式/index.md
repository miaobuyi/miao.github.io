# 设计模式


<!--more-->

## 单例模式

> 饿汉式单例

```java
//饿汉式单例
public class Hungry {

    //可能会浪费空间
    private byte[] datal = new byte [1024*1024];
    private byte[] data2 = new by mte [1024*1024];
    private byte[] data3 = new byte [1024*1024];
    private byte[] data4 = new byte [1024*1024];
    
    private Hungry(){
        
    }
    
    private fina1 static Hungry HUNGRY = new Hungry();
    
    public static Hungry getInstance() {
    	return HUNGRY;
    }
}

```

> DCL懒汉式

``` java
//懒汉式单例
public class LazyMan {
    
    private static boolean qinjiang = false;
    
	private LazyMan(){
		synchronized (LazyMan. class){
            if (qinjiang == false){
            	qinjiang = true;
            }else {|
				throw new RuntimeException("不 要试图使用反射破坏异常");
		}
}

	}
        
	private volatile static LazyMan LazyMan;
	//双重检测锁模式的懒汉式单例DCL 懒汉式
	public static LazyMan getInstance( ){
		if (LazyMan==nu1l){
			synchronized (LazyMan. class){
				if (LazyMan==nu1l){
					lazyMan = new LazyMan(); //不是一个原子性操作
				}
			}
		}
		return LazyMan;
	}

	//反射!
	public static void main(String[] args) throws Exception {
		LazyMan instance = LazyMan.getInstance();
        Field qinjiang = LazyMan.class.getDeclaredField( name: "qinjiang");
        qinjiang. setAccessible(true);
        Constructor<LazyMan> declaredConstructor = LazyMan. class. getDeclaredConstructor(null)
        declaredConstructor . setAccessible(true);
        LazyManτ instance = declaredConstructor.newInstance();
        qinjiang. set(instance,false);
        LazyMan instance2 = declaredConstructor.newInstance();
        System.out.print1n(instance);
        System.out.print1n(instance2);
}


```

> 静态内部类

```java
    // 静态内部类
public class Holder {
    private Holder(){

    }
    pub1ic static Holder getInstace(){
        return InnerClass. HOLDER;
    }

        
    pub1ic static class InnerClass{
        private static final Holder HOLDER = new Holder();
}

```

> 单例不安全，反射

> 枚举

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
//enum是一个什么?本身也是 一个CLass类
public enum EnumSingle {
    INSTANCE;
    public EnumSingle getInstance(){
    return INS TANCE ;
    class Test{
        public static void main(String[] args) throws Exception{
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(String.class,int.class)
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor . newInstance();
        // NoSuchMethodException: com. kuang. single. EnumSingle. <init>()
        System.out.println( instance1);
        System.out.println( instance2);
    }
}

```

枚举的最终反编译

``` java
pub1ic static Enumsing1e[] values(){
    return (EnumSingle[]) $VALUES. clone();
}

public static EnumSingle valueOf (String name){
    return (EnumS ing1e)Enum. valueOf (com/kuang/s ing1e/Enumsingle, name);
}
private Enumsingle(String S，int i){
    super(s，i);
}
public EnumSingle getInstance(){
    return INSTANCE;
}
public static final EnumSingle INSTANCE;
private static final EnumSingle $VALUES[];
static{
    INSTANCE = new EnumSingle("INSTANCE"，0);
    $VALUES = (new EnumSing1e[] {
        INSTANCE
    });
}

```


