
# 单例

### 单线程环境

```java
public class Singleton {

    private static Singleton instance;

    /**
     * 私有构造
     */
    private Singleton() {
    }

    /**
     * 只适用单线程环境的创建方式
     * @return
     */
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

}
```

### 静态方法，饿汉式

```java
public class SingletonHungry {

    // 利用静态构造只会初始化一次
    private static SingletonHungry instance = new SingletonHungry();

    private SingletonHungry() {
    }

    public static SingletonHungry getInstance() {
        return instance;
    }
}
```

### 同步加锁，懒汉式

```java
public class SingletonLazy {

    private static SingletonLazy instance;

    private SingletonLazy(){
    }

    public static synchronized SingletonLazy getInstance() {
        if (instance == null) {
            instance = new SingletonLazy();
        }
        return instance;
    }
}
```

### 双重校验锁，懒汉式优化

```java
public class SingletonTwice {

    //private static SingletonTwice instance;
    private static volatile SingletonTwice instance;

    private SingletonTwice() {
    }

    /**
     * 减小锁粒度，到真正创建对象的时候才使用同步
     * @return
     */
    public static SingletonTwice getInstance() {
        if (instance == null) {
            synchronized (SingletonTwice.class) {
                if (instance == null) {
                    instance = new SingletonTwice();
                }
            }
        }
        return instance;
    }

}
```

双检锁在多线程环境下还是可能出现空指针问题。创建对象不是原子操作，分三步：
- 分配内存空间
- 初始化对象
- 对象引用指向分配的地址
由于 CPU 可能的优化排序，第三步可能会先于第二步执行，这时其他线程读到就会有问题。解决这个问题只须加上 volatile 关键字，禁止指令重排序即可。

### 静态内部类

```java

```

### 枚举

```java
public class SingletonEnum {

    private SingletonEnum() {
    }

    private static enum Singleton {
        INSTANCE;
        private SingletonEnum singletonEnum;

        private Singleton() {
            singletonEnum = new SingletonEnum();
        }

        public SingletonEnum getInstance() {
            return singletonEnum;
        }
    }

    public static SingletonEnum getInstance() {
        return Singleton.INSTANCE.getInstance();
    }`

}
```

枚举类型是线程安全的，并且只会装载一次。
