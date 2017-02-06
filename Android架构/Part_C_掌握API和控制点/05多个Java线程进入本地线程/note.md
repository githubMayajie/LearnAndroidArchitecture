# 1认识JNI线程模式

## 线程往返Java与c/C++　　
1. 由于每一个进程都有一个主线程　　
2. 每一个进程里，都可能有Java程序码，也有c/c++本地程序码  
3. Java层的主线程进场从Java层进入JNI层的C函数里执行，此外，当反向调用Java函数时，又返回进入Java函数执行  
4. 无论主线程还是子线程，都可以从Java层进入C/C++层去执行，也能从C/C++层进入Java层　　

## VM对象与JavaVM指针　　
1. 在进程里，有一个虚拟机(简称VM)的对象，可执行Java代码，也能引导JNI本地程序的执行，实现Java与C/C++之间的沟通　　
2. 当VM执行到System.loadLibrary()函数去加载C模块时，就可立即调用JNI_OnLoad()函数。  
3. VM调用JNI_OnLoad()函数时，会将VM的指针传递给他，其参数格式位JNIVM* vm -- jint JNI_OnLoad(JavaVM* vm,void* reserved);  
4. 将传来的VM指针储存与本地模块的公共变量，让本地函数随时能够使用jvm来与VM交互。  jvm = vm;  

当创建一个本地C层的新线程时，可以使用如下指令，向VM登记，要求VM诞生JNIEnv对象，并能将其指针值存入env里。  
```c++
jvm->AttachCurrentThread(&env,NULL);
```

## 为什么需要JNIEnv对象呢？
1. 本地c函数的第一个参数就是JNIEnv对象的指针。  
2. 这是Java线程透过VM进入C函数时，VM替线程创建的对象，是该线程的专属私有变量  
3. 线程透过它来要求VM协助进入Java层去取得Java层的资源，包括，取得函数或属性ID,调用Java函数或存取Java对象属性值等.  

---
# 从Session概念认识JNIEnv对象
每一个线程进入VM都有一个私有的JNIEnv对象

# 细说JNIEnv对象

# 本地函数的线程安全
```c++
env->MonitorEnter(jobject);//锁住
env->MonitorExit(jobject);//解锁
```
