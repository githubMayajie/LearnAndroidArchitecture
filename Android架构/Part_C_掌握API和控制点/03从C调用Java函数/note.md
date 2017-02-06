# 3How-to:从C调用Java函数　　

#5 从C创建Java对象

Java层创建１个对象
```java
CounterName x;
x = new CounterName();
```

创建与thiz同类的对象
1. 问这个对象thiz的类，得到clazz  
2. 问这个类里的<init>()构造式，得到methodID  
3. 基于methodID,调用构造式(创建对象)

创建与thiz不同类的对象
1. 问特定的类，得到clazz  
2. 同上　　
3. 基于methodID,调用构造式(创建对象)

```cpp
jclass clazz = (*env)->FindClass("");
jmethod initMid = (*env)->GetMethodID(env,clazz,"<init>","()V");
jobject ref = (*env)->NewObject(env,clazz,initMid);
```
