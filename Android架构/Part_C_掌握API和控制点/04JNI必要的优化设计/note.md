# 1创建C++类对象

# 2优化目的
不依赖于C层的*.so的全局变量来存储Java层或者C++层的对象　　
依赖C层(全局或者静态变量)来存储C++对象指针，或者储存Java层对象参考，会导致系统弹性下降　　
C层的全局或者静态变量只适合于存储静态的数据，例如methodID或者fieldID  

# 3静态对静态，动态对动态的原则

# 4Java与C++对象之间的单向对称关联

```java
public class CounterNative{
  private int mObject;
  static{
    System.loadLibrary("MyCount7");
  }

  public CounterNative(int numb){
    nativeSetup(numb);
  }
  private native void nativeSetup(int n);
}
```

当你定义C++类别时，可以将它与JNI的C函数定义在同一个文件(.so)里，也可以定义在独立的档案中  

```c++
class CCounter{
  int n;
public:
  CCounter(int v){
    n = v;
  }

  int execute(){
    int i, sum = 0;
    for(int i = 0;i <= n; i++)
      sum += i;
    return sum;
  }
}
```

```c
JNIEXPORT void JNICALL xxx_nativeSetup(JNIEnv* env,jobject thiz,jint n){
  CCounter* obj = new CCounter(n);
  jclass clazz = (jclass)env->GetObjectClass(thiz);
  jfieldId fid = (jfieldId)env->GetFieldID(clazz,"mObject","I");
  env->SetIntField(thiz,fid,(jint)obj);
}

JNIEXPORT jint JNICALL xxx_nativeExec(JNIEnv* env,jclass clazz,jobject obj){
  jclass objClass = (jclass)env->GetObjectClass(obj);
  jfieldID fid = env->GetFieldID(objClass,"mObject","I");
  jlong p = (jlong)env->GetObjectField(obj,fid);
  CCounter* co = (CCounter*)p;
  return (jint)co->execute();
}
```
