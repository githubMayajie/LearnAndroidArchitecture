# 1
## Content:
#### Oriented的含义
任何函数都是由对象构成   <br>


Oriented  
Base  
Driven  
Centered  

---
#### Object
特点:静态特性(Attribute) 动态行为(Behavior)  

---
#### 类的用途
```java
class Rose{
  private double price;
  private int month;
  public void say(){
    System.out.println("Color is Red!");
  }
}

public class JMain{
  public static void main(String[] args){
    Rose rose = new Rose();
    rose.say();
  }
}
```

# 2
#### 继承
```java
class Teacher extends Person{
  ...
}
class Student extends Person{
  ...
}

```
---
#### 组合
```java
class Task implements Runnable{
  public void run(){
    int sum = 0;
    for (int i = 0; i <=100; i++) {
        sum += i;
    }
    System.out.println("Resule:" + sum);
  }
}

public class JMain{
  public static void main(String[] args) {
    Thread t = new Thread(new Task());
    t.start();
  }
}

class Task extends Thread{
  public void run(){
    int sum = 0;
    for (int i = 0; i <=100; i++) {
        sum += i;
    }
    System.out.println("Resule:" + sum);
  }
}
public class JMain{
  public static void main(String[] args) {
    Thread t = new Task();
    t.start();
  }
}
```
---
# 3
#### 接口
变与不变的分离  
```cpp
class Car : Vehicle{

}
```
---

```java
class Tire extends Car{

}
```

```java
interface IShaper{
  void template_paint(Graphics gr);
}
public class Shaper implements IShaper{
  public void template_paint(Graphics gr){
    invatiant_paint(gr);
    hook_paint(gr);
  }

  private void inviriant_paint(Graphics gr){
    gr.setColor(Color.black);
  }

  abstract protected void hook_paint(Graphics gr);
}
public class Bird extends Shaper{
  private void hook_paint(Graphics gr){
    gr.setColor(Color.cyan);
  }
}
```

---
#### Ioc机制与default函数  
---
# 4
