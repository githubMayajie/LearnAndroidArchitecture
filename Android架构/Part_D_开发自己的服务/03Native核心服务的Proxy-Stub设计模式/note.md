在Java层，Binder类实现了IBinder接口
在c++层，BBinder类实现了IBinder接口

如何协助产生Proxy_Stub类？
1. 在Java层使用aidl.exe工具生成proxy-Stub类  
2. 在c/C++层使用模板来协助产生proxy-Stub类

# 3使用模板，产生stub类

Android提供了BnInterface<I> 和BpInterface<I>模板
```c++
template<typename INTERFACE>
class BnInterface: public INTERFACE,public BBinder{
public:
  virtual sp<IInterface> queryLocalInterface(const String16& descriptor);
  virtual String16 getInterfaceDescriptor() const;

protected:
  virtual IBinder* onAsBinder();
}
```

基于上面的模板，定义接口
```c++
class IMySercie : public IInterface{
public:
  DECLARE_META_INTERFACE(MyService);
  virtual void sv1(...) = 0;
  virtual void sv1(...) = 0;
  virtual void sv1(...) = 0;
}
```
基于上面的模板，可产生stub类
```c++
class BnMyService : public BnInterface<IMySercie>{
  //...
}
```

# 使用模板产生proxy类
Android提供了BpInterface<T>类别模板
```c++
class BpMyService : public BpInterface<IMySercie>{
  //...
}
```
