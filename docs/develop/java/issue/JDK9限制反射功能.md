
# JDK9 限制反射导致动态代理异常

```java
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.design.proxy.dynamic.cglib.CglibProxy.getProxy(CglibProxy.java:26)
	at com.design.proxy.dynamic.cglib.CglibProxy.main(CglibProxy.java:37)
Caused by: net.sf.cglib.core.CodeGenerationException: java.lang.reflect.InaccessibleObjectException-->Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain) throws java.lang.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module @1376c05c
	at net.sf.cglib.core.ReflectUtils.defineClass(ReflectUtils.java:464)
	at net.sf.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:339)
	at net.sf.cglib.core.AbstractClassGenerator$ClassLoaderData$3.apply(AbstractClassGenerator.java:96)
	at net.sf.cglib.core.AbstractClassGenerator$ClassLoaderData$3.apply(AbstractClassGenerator.java:94)
	at net.sf.cglib.core.internal.LoadingCache$2.call(LoadingCache.java:54)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at net.sf.cglib.core.internal.LoadingCache.createEntry(LoadingCache.java:61)
	at net.sf.cglib.core.internal.LoadingCache.get(LoadingCache.java:34)
	at net.sf.cglib.core.AbstractClassGenerator$ClassLoaderData.get(AbstractClassGenerator.java:119)
	at net.sf.cglib.core.AbstractClassGenerator.create(AbstractClassGenerator.java:294)
	at net.sf.cglib.core.KeyFactory$Generator.create(KeyFactory.java:221)
	at net.sf.cglib.core.KeyFactory.create(KeyFactory.java:174)
	at net.sf.cglib.core.KeyFactory.create(KeyFactory.java:153)
	at net.sf.cglib.proxy.Enhancer.<clinit>(Enhancer.java:73)
	... 2 more
Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain) throws java.lang.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module @1376c05c
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:354)
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:297)
	at java.base/java.lang.reflect.Method.checkCanSetAccessible(Method.java:199)
	at java.base/java.lang.reflect.Method.setAccessible(Method.java:193)
	at net.sf.cglib.core.ReflectUtils$1.run(ReflectUtils.java:61)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:569)
	at net.sf.cglib.core.ReflectUtils.<clinit>(ReflectUtils.java:52)
	at net.sf.cglib.core.KeyFactory$Generator.generateClass(KeyFactory.java:243)
	at net.sf.cglib.core.DefaultGeneratorStrategy.generate(DefaultGeneratorStrategy.java:25)
	at net.sf.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:332)
	... 14 more
```

JDK9以上模块不能使用反射去访问非公有的成员/成员方法以及构造方法,除非模块标识为opens去允许反射访问。

解决方法是在 IDEA 中设置 VM options 参数

```bash
--illegal-access=deny --add-opens java.base/java.lang=ALL-UNNAMED
```

执行会看到警告：OpenJDK 64-Bit Server VM warning: Ignoring option --illegal-access=deny; support was removed in 17.0

要去掉只添加 --add-opens java.base/java.lang=ALL-UNNAMED 也就行了。

Maven 打包也可以设置同样的 VM Options 参数。

Gradle 需要在 build.gradle 中添加参数。

```bash
test {
    useJUnitPlatform()
    jvmArgs('--illegal-access=deny')
    jvmArgs('--add-opens', 'java.base/java.lang.invoke=ALL-UNNAMED')
}
```

参考：[解决JDK9以上的非法反射访问警告](https://blog.csdn.net/qq_27525611/article/details/108685030)