# 1如何创建本地的子线程

1. 由于创建C层新线程时，VM尚不知道它的存在，没有替他创建专属的JNIEnv对象，无法调用到Java层函数,此时，可以向VM登记而取得JNIEnv对象后，此线程就能进入Java层了  

## 在C函数里创建子线程
```c++
int h1 = pthread_create(&thread,NULL,trRun,NULL);

void* trRun(void*){
  //...
}
```
使用pthread_create()函数创建本地的子线程  

# 2Native线程进图Java线程
//Java
```java
public class CounterNative{
  private static Handler h;
  static{
    System.loadLibrary("MyJI002");
  }

  public CounterNative(){
    init();
    h = new Handler(){
      public void handlerMessage(Message msg){
        ac01.ref.setTitle("Hello ...");
      }
    };
  }

  private static void callback(int a){
    Message m = h.obtainMessage(1,a,3,null);
    h.sendMessage(m);
  }
  private native void init();
  private native void execute(int numb);
}
```
//C++
```c++
#include <stdio.h>
#include <pthread.h>
#include <jni.h>
jmethodID mid;
jclass mClass;
JavaVM* jvm;
pthread_t thread;
int n,sum;
void* trRun(void*);

void JNICALL xxx_nativeSetup(JNIEnv* env,jobject thiz){
  jclass clazz = env->GetObjectClass(thiz);
  mClass = (jclass)env->NewObjectRef(clazz);
  mid = env->GetStaticMethodID(mclass,"callback","(I)V");
}

void JNICALL xxx_nativeExec(JNIEnv* env,jobject thiz,jint numb){
  n = numb;
  pthread_create(&thread,NULL,trRun,NULL);
}

void* trRun(void*){
  int status;
  JNIEnv* env;
  bool isAttached = false;
  status = jvm->GetEnv((void**)&env,JNI_VERSION_1_4);
  if(status){
    status = jvm->AttachCurrentThread(&env,NULL);
    if(status)
      return NULL;
    isAttached = true;
  }
  sum = 0;
  for(int i = 0; i <= n; i++){
    sum += i;
  }
  env->CallstaticVoidMethod(mClass,mid,sum);
  if(isAttached)
    jvm->DetachCurrentThread();
  return null;
}

static const char* classPathName = "xxx";
static JNINativeMethod methods[] = {
  {"init","()V",(void*)Java_com_xxx_nativeSetup},
  {"execute","(I)V",(void*)Java_com_xxx_nativeExec}
};
static int registerNativeMathods(JNIEnv* env,const char* className,JNINativeMethod* gMethods,int numMethods){
  jclass clazz = env->FindClass(className);
  env->RegisterNative(clazz,gMethods,numMethods);
  return JNI_TRUE;
}
static int registerNatives(JNIEnv* env){
  registerNativeMathods(env,classPathName,methods,sizeof(methods)/sizeof(methods[0]));
  return JNI_TRUE;
}

jint JNI_Onload(JavaVM* vm,void* reserved){
  JNIEnv* env;
  jvm = vm;
  if(registerNatives(env) != JNI_TRUE)
    return -1;
  return JNI_VERSION_1_4;
}

```

# Native多线程安全
```c++
env->MonitorEnter(jobject);
env->MonitorExit(jobject);
```
