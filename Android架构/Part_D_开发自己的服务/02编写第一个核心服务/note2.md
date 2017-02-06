### 方案二的实现  

c层拥有控制点的必备表现是：
1. 从C创建Java对象  
2. 从C调用Java层函数

在Android中，提供了一个JNI Native模块，内涵一个javaObjectForIBinder()函数，它能协助诞生Java层的BinderProxy对象，作为BpBinder对象的分身  

```java
public class sqr05{
    static{
        System.loadLibrary("SQRS05_jni");
    }
    public native final IBinder bindCoreService();
}
```

```c++
sp<IBinder> m_ib;
JNIEXPORT jobject JNICALL xxx_bindCoreService(JNIEnv* env,jobject thiz){
    LOGE("bindCoreService");
    sp<IServiceManager> sm = defaultServiceManager();
    m_ib = sm->getService(String16("misoo,sqr"));
    LOGE("SM:getService %p\n",sm.get());
    if(m_ib == 0){
        LOGW("SQRService not publish,wait...");
        return 0;
    }
    jobject jbi = javaObjectForiBinder(env,m_ib);
    if(jbi == 0){
        return 0;
    }
    return jbi;
}

```
