# 第一章

- 知道OO基础，并不足以让你设计出良好的OO系统。
- 良好的OO设计必须具备可复用、可扩充、可维护三个特性。
- 模式可以让我们建造出具有良好OO设计质量的系统。
- 模式被认为是历经验证的OO设计经验。
- 模式不是代码，而是针对设计问题的通用解决方案。你可把它们应用到特定的应用中。
- 模式不是被发明，而是被发现。
- 大多数的模式和原则，都是着眼于软件变化的主题。
- 大多数的模式都允许系统局部改变独立于其他部分。
- 我们常把系统中会变化的部分抽出来封装。
- 模式让开发人员之间有共享的语言，能够最大化沟通的价值。

OO基础：抽象；封装；多态；继承  
OO原则：封装变化；多用组合，少用继承；针对接口编程，不针对实现编程  
OO模式：  
策略模式——定义算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户。  

利用继承来提供Duck的行为，这会导致下来哪些缺点？（ABDF）  
A、代码在多个自来中重复  
B、运行时的行为不容易改变  
C、我们不能让鸭子跳舞  
D、很难知道所有鸭子的全部行文  
E、鸭子不能同事又飞又叫  
F、改变会牵一发动全身，造成其他鸭子不想要的改变  

软件开发的一个不变真理：CHANGE   
不管当初软件设计得多好，一段时间之后，总是需要成长与改变，否则软件就会“死亡”。  

***设计原则***：  

1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起。  
把会变化的部分取出并“封装”起来，好让其他不分不会受到影响。  
结果如何？代码变化引起的不经意后果变少，系统变得更有弹性。  
2. 针对接口编程，而不是针对实现编程。  
“针对接口编程”真正的意思是“针对超类型（supertype）编程”。
关键在于多态，利用多态，程序可以针对超类型编程，执行会根据实际状况执行到真正的行为，不会被绑死在超类型的行为上。

问：我是不是一定要先把系统做出来，再看看有哪些需要变化，然后才回头把这些地方分离&封装？  
答：不尽然。通常在你设计系统时，预先考虑到有哪些地方未来可能需要变化，于是提前在代码中加入这些弹性。你会发现、原则与模式可以应用在软件开发生命周期的任何阶段。  

问：用一个类代表一个行为，感觉似乎有点奇怪。类不是应该代表某种“东西”吗？类不是应该同时具备状态“与”行为吗？（参考Duck设计模式）  
答：在OO系统中，是的，类代表的东西一般都是既有状态（实例变量）又有方法。只是在本例中，碰巧“东西”是个行为。但是即使是行为，也仍然可以有状态和方法，例如，飞行的行为可以具有实例变量，记录飞行行为的属性（每秒翅膀拍动几下、最大高度和速度等）。

```
public abstract class Duck {
	FlyBehavior flyBehavior;
	QuackBehavior quackBehavior;
	public Duck() {
	}
	
	public abstract void display();
	
	public void performFly() {
		flyBehavior.fly();
	}
	
	public void performQuack() {
		quackBehavior.quack();
	}
	
	public void swim() {
		System.out.println("All ducks float, even decoys!");
	}
}

public interface FlyBehavior {
	public void fly();
}

public class FlyWithWings implements FlyBehavior {
	public void fly() {
		System.out.println("I'm flying!");
	}
}

public class FlyNoWay implements FlyBehavior {
	public void fly() {
		System.out.println("I can't flying!");
	}
}

public interface QuackBehavior {
	public void quack();
}

public class Quack implements QuackBehavior {
	public void quack() {
		System.out.prinln("Quack");
	}
}

public class MuteQuack implements QuackBehavior {
	public void quack() {
		System.out.prinln("《Silece》");
	}
}

public class Squack implements QuackBehavior {
	public void quack() {
		System.out.prinln("Squack");
	}
}

public class MiniDuckSimulator {
	public static void main(String[] args) {
		Duck duck = new Duck();
		duck.performQuack();
		duck.performFly();
	}
}
```
***设计原则***  
多用组合，少用继承

**策略模式**  
定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立鱼使用算法的客户。  

问：如果设计模式这么棒，为何没有人简历相关的库呢？那样我们就不必自己动手了。  
答：设计模式比库的登基更高。设计模式告诉我们如何组织类和对象以解决某种问题。而且采纳这些设计并使它们适合我们特定的应用，是我们责无旁贷的事。  

问：库和框架不也是设计模式吗？  
答：库和框架提供了我们某些特定的实现，让我们的代码可以轻易地引用，但是这并不算是设计模式。有些时候，库和框架本身会用到设计模式，这样很好，因为一旦你了解了设计模式，会更容易了解这些API是围绕着设计模式构造的。  

问：那么，没有所谓设计模式的库？  
答：没错，但是稍后你回看到设计模式类目。你可以在应用中利用这些设计模式。  

知道抽象、继承、多态这些概念，并不会马上让你变成好的面向对象设计者。设计大师关心的是建立弹性的设计，可以维护，可以应付变化。  

- OO基础  
  - 抽象
  - 封装
  - 多态
  - 继承
- OO原则
  - 封装变化
  - 多用组合，少用继承
  - 针对接口编程，不针对实现编程
- OO模式
  - 策略模式—定义算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立与使用算法的客户。

- 要点  
  - 知道OO基础，并不足以让你设计出良好的OO系统。
  - 良好的OO设计必须具备可以复用、可扩充、可维护三个特性。
  - 模式可以让我们建造出具有良好OO设计质量的系统。
  - 模式被认为是历史验证的OO设计经验。
  - 模式不是代码，而是针对设计问题的通用解决方案。你可把它们应用到特定的应用中。
  - 模式不是被发明的，而是被发现的。
  - 大多数的模式和原则，都着眼于软件变化的主题。
  - 大多数的模式都云栖系统局部改变独立鱼其他部分。
  - 我们常把系统中会变化的部分抽出来封装。
  - 模式让开发人员之间有共享的语言，能够最大化沟通的价值。

---
观察者（Observer）模式
---
认识观察者模式：  
我们看看报纸和杂事的订阅是怎么回事：  
1. 报社的业务就是初版报纸。
2. 向某家报社订阅报纸，只要他们有新报纸出版，就会给你送来。只要你是他们的订户，你就会一直收到新报纸。
3. 当你不想再看报纸的时候，取消订阅，他们就不会再送新报纸。
4. 只要报社还在运营，就会一直有人（或单位）向他们订阅报纸或取消订阅报纸。

**出版者+订阅者=观察者模式**  
出版者该称为“主题”（Subject），订阅者改称为“观察者”（Observer）。  

***观察者模式***：定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。

当两个对象之间松耦合，它们依然可以交互，但是不太轻清楚彼此的细节。  
观察者模式提供了一种对象设计，让主题和观察者之间松耦合。  

***设计原则***  
1. 为了交互对象之间的松耦合设计而努力。

松耦合的设计之所以能让我们建立有弹性的OO系统，能够应对变化，是因为对象之间的互相依赖讲到了最低。  

```
//在WeatherData中实现主题接口
、、WeatherData实现了Subject接口
public class WeatherData implements Subject {
	private ArrayList observers;
	private float temperature;
	private float humidity;
	private float presure;
	
	public WeatherData() {
		//加上一个ArrayList来记录观察者，此ArrayList是在构造器中建立的。
		observers = new ArrayList();
	}
	
	//加到ArrayList中即可注册观察者
	public void registerObserver(Observer o) {
		observers.add(o);
	}
	
	//从ArrayList中删除即可取消注册
	public void removeObserver(Observer o) {
		int i = observers.indexOf(o);
		if (i >= 0) {
			observers.remove(i);
		}
	}
	
	//在这里，我们把状态告诉每一个观察者。
	//因为观察者都实现了update()，所以我们知道如何通知它们
	public void notifyObservers() {
		for (int i = 0; i < observers.size(); i++) {
			Observer observer = (Observer)observers.get(i);
			observer.update(temperature, humidity, presure);
		}
	}
	
	//当从气象站得到更新观测值时，我们通知观察者
	public void measurementsChanged() {
		notifyObservers();
	}
	
	public void setMeasurements(float temperature, float humidity, float presure) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.presure = presure;
	}
	
	//WeatherData的其他方法
}
```
```
//===布告板===
/**
* 此布告板实现了Object接口，所以可以从WeatherData对象中获得改变
* 实现了DisplayElement接口，因为我们的API规定所有的布告板都必须实现此接口
/
public class CurrentConditionDisplay implements Observer, DisplayElement {
	private float temperature;
	private float humidity;
	private Subject weatherData;
	
	//weatherData对象（也就是主题）做为注册之用
	public CurrentConditionDisplay(Subject weatherData) {
		this.weatherData = weatherData;
		weatherData.registerObserver(this);
	}
	
	//当update()被调用时，我们把温度和湿度保存起来，然后调用display()
	public void update(float temperature, folat humidity, folat pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		display();
	}
	
	//温度和湿度显示
	public void display() {
		System.out.println("Current conditions:" + temperature + " F degrees and " + humidity + "% humidity");
	}

}
```
```
//利用内置java.util.Observerable重做气象站
import java.util.Observable;
import java.util.Observer;


```









































