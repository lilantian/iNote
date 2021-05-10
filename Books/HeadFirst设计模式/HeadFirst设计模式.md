第一章
---

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

```java
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

```java
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
```java
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
```java
//利用内置java.util.Observerable重做气象站
import java.util.Observable;
import java.util.Observer;

public class WeatherData extends Observable {
	private float temperature;
	private float humidity;
	private float pressure;
	
	public WeatherData() {}
	
	public void setMeasurements(float temperature, float humidity, float pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.pressure = pressure;
		measurementsChanged();
	}
	
	public float getTemperature() {
		return temperature;
	}
	
	public float getHumidity() {
		return humidity;
	}
	
	public float getPressure() {
		return pressure;
	}
}
```
```java
import java.util.Observable;
import java.util.Observer;

public class CurrentConditionsDisplay implements Observer, DisplayElement {
	Observable observable;
	private float temperature;
	private float humidity;
	
	public CurrentConditionsDisplay(Observable observable) {
		this.observable = observable;
		observable.addObserver(this);
	}
	
	public void update(Observable obs, Object arg) {
		if (obs instanceof WeatherData) {
			WeatherData weatherData = (WeatherData)obs;
			this.temperature = weatherData.getTemperature();
			this.humidity = weatherData.getHumidity();
			display();
		}
	}
	
	public void display() {
		System.out.println)("Current conditions:" + temperature + "F degrees and " + humidity + "% humidity");
	}

}
```
```java
public class SwingObserverExample {
	JFrame frame;
	
	public static void main(String[] args) {
		SwingObserverExample example = new SwingObserverExample();
		example.go();
	}
	
	public void go() {
		frame = new JFrame();
		
		JButton button = new JButton("Should I do it?");
		button.addActionListener(new AngleListener());
        button.addActionListener(new DevilListener());
        frame.getContentPane().add(BorderLayout.CENTER, button);
        //这里设置frame属性
	}
	
	class AngleListener implements ActionListener {
		public void adtionPerformed(ActionEvent event) {
			System.out.println("Don't do it, you might regret it!");
		}
	}
	
	class DevilListener implements ActionListener {
		public void adtionPerformed(ActionEvent event) {
			System.out.println("Come on, do it!");
		}/
	}
}
```

- OO基础：抽象
- OO原则
	- 封装变化
	- 多用组合，少用继承
	- 针对接口编程，不针对实现编程
	- 为交互对象之间的松耦合设计而努力
- OO模式
	- 观察者模式—在对象之间定义一对多的依赖，这样一来，当一个对象改变状态，依赖它的对象都会收到通知，并自动更新。

要点：
- 观察者定义了对象之间一对多的关系
- 主题（也就是可观察者）用一个共同的接口来更新观察者
- 观察者和可观察者之间用松耦合方式结合（loosecoupling），可观察者不知道观察者的细节，只知道观察者实现了观察者接口。
- 使用此模式时，你可从被观察者出推（push）或拉（pull）数据（然而，推的凡事被认为更“正确”）
- 有多个观察者时，不可以依赖特定的通知次序
- Java有多种观察者模式的实现，包括了通用的java.util.Observable
- 要注意java.util.Observable实现上多带来的一些问题
- 如果有必要的话，可以实现自己的Observable，这并不难，不要害怕
- Swing大量使用观察者模式，许多GUI框架也是如此。
- 此模式也被应用在许多地方，例如：JavaBeans、RMI

***设计原则***
- 找出程序中会变化的方面，然后将其和固定不变的方面相分离
- 针对接口编程，不针对实现编程
- 多用组合，少用继承

---
装饰者模式
---

***设计原则***
开放-关闭原则：类应该对扩展开放，对修改关闭。

虽然似乎有点矛盾，但是的确有一些技术可以允许在不直接修改代码的情况下对其进行扩展。  
在选择需要被扩展的代码部分时要小心。每个地方都采用开放-关闭原则，是一种浪费，也没有必要。还会导致代码变得复杂且难以理解。

**装饰者模式**动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更有弹性的替代方案。  

```java
//扩展FilterInputStream类，并覆盖read()方法
public class LowerCaseImputStream extends FilterInputStream {
	public LowerCaseInputStream(InputStream in) {
		super(in);
	}
	
	public int read() throws IOException {
		int c = super.read();
		return (c == -1 ? c : Character.toLowerCase((char)c));
	}
	
	public int read(byte[] b, int offset, int len) throws IOException {
		int result = super.read(b, offset, len);
		for (int i = offset; i < offset+result; i++) {
			b[i] = (byte)Character.toLowerCase((char)b[i]);
		}
		return result;
	}

}

//测试
//test.txt
// I know the Decorator Pattern therefore I RULE!
public class InputTest {
	public static void main(String[] args) throws IOException {
		int c;
		try {
			InputStream in = new LowerCaseInputStream(new BufferedInputStream(new FileInputStream("test.txt")));
			while((c = in.read()) >= 0) {
				Syste.out.println((char)c);
			}
			in.close();
		} catch (IOExcetion e) {
			e.printStackTrace();
		}
	}
}
//结果...
% java InputTest
i know the decorator pattern therefore i rule!
%
```

- OO基础
	- 抽象
	- 封装
	- 多型
	- 继承
- OO原则
	- 封装变化
	- 多用组合，少用继承
	- 针对接口编程，不针对实现编程
	- 为交互对象之间的松耦合设计而努力
	- 对扩展开放，对修改关闭
- OO模式
	- 策略模式
	- 观察者模式
	- 装饰者模式：动态地将责任附加到对象上。想要扩展功能，装饰者提供有别于继承的另一种选择。

要点：
- 继承属于扩展形式之一，但不见得是达到弹性设计得最佳方式
- 在我们的设计中，应该允许行为可以被扩展，而无需修改现有的代码
- 组合和委托可用于在运行时动态地加上新的行为
- 除了继承，装饰者模式也可以让我们扩展行为
- 装饰者模式意味着一群装饰者类，这些类用来包装具体组件
- 装饰者类反映出被装饰的组件类型（事实上，他们具有相同的类型，都经过接口或继承实现）
- 装饰者可以在被装饰者的行为前面与/或后面加上自己的行为，甚至被装饰者的行为整个去带掉，而达到特定的目的
- 你可以用无数个装饰者包装一个组件
- 装饰者一般对组件的客户是透明的，除非客户程序依赖于组件的具体类型
- 装饰者会导致设计中出现许多小对象，如果过度使用，会让程序变得很复杂

---
工厂模式
---

```java
public abstract class PizzaStore {

	public Pizza orderPizza(String type) {
		Pizza pizza;
		
		pizza = createPizza(type);
		
		pizza.prepare();
		pizza.bake();
		pizza.cut();
		pizza.box();
		
		return pizza;
	}
	
	protected abstract Pizza createPizza(String type);
	
	//其他方法
}
```
工厂方法用来处理对象的创建，并将这样的行为封装在子类中。这样，客户程序中关于超类的代码就和子类对象创建代码解耦了。  
```java
/*
工厂方法是抽象的，所以依赖子类来处理对象的创建
工厂方法必须返回一个产品。超类中定义的方法，通常使用到工厂方法的返回值。
工厂方法将客户（也就是超类中的代码）和实际创建具体产品的代码分割开来
*/
abstract Product factoryMethod(String type)
```
所有工厂模式都用来封装对象的创建。工厂方法模式（Factory Method Pattern）通过让子类决定该创建的对象是什么，来达到将对象创建的过程封装的目的。

***工厂方法模式***定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。  

问：当只有一个ConcreteCreator的时候，工厂方法模式有什么优点？  
答：尽管只有一个具体创建者，工厂方法模式依然很有用，因为它帮助我们将产品的“实现”从“使用”中解耦。如果增加产品或者改变产品的实现，Creator并不会受到影响（因为Creator与任何ConcreteProduct之前都不是紧耦合）。

问：工厂方法和创建者是否总是抽象的？  
答：不，可以定义一个默认的工厂方法来产生某些具体的产品，这么一来，即使创建者没有任何子类，依然可以创建产品。

问：每个商店基于传入的类型制造出不同种类的比萨。是否所有的具体创建者都必须如此？能不能只创建一种比萨？  
答：这里所采用的方式称为“参数化工厂方法”。它可以根据传入的参数创建不同的对象。然而，工厂进场只产生一种对象，不需要参数化。模式的这两种形式都是有效的。

问：利用字符串传入参数化的类型，似乎有点危险，然一吧Clam（蛤蜊）英文拼错，成了Calm（平静），要求供应“CalmPizza”，怎么办？  
答：说的很对，这样的情形会造成所谓的“运行时错误”。有几个其他更复杂的技巧可以避开这个麻烦，在便一时期就将参数上的错误挑出来。比方说，你可以创建代表参数类型的对象和使用静态常量或者Java 5所支持的enum。

问：对于简单工厂和工厂方法之间的差异，我依然感到困惑。他们看起来很类似，差别在于，在工厂方法中，返回比萨的类是子类。能解释一下吗？  
答：子类的确看起来很像简单工厂。简单工厂把全部的事情，在一个地方都处理完了，然而工厂方法却是创建一个框架，让子类决定要如何实现。比方说，在工厂方法中，orderPizza()方法提供了一般的框架，以便创建比萨，orderPizza()方法依赖工厂方法创建具体类，并制造出实际的比萨。可通过继承PizzaStore类，决定实际制造出的比萨是什么。简单工厂的做法，可以将对象的创建封装起来，单思简单工厂不具备工厂方法的弹性，因为简单工厂不能变更正在创建的产品。

**依赖倒置原则**  
***设计原则：要依赖抽象，不要依赖具体类***

几个指导方针，帮你避免在OO设计中违反依赖倒置原则：  
- 变量不可以持有具体类的引用。  
	如果使用new，就会持有具体类的引用。你可以改用工厂来避开这样的做法。
- 不要让类派生自具体类。
	如果派生自具体类，你就会依赖具体类。请派生自一个抽象（接口或抽象类）。
- 不要覆盖基类中已实现的方法。
	如果覆盖基类已实现的方法，那么你的基类就不是一个真正适合被继承的抽象。基类中已实现的方法，应该由所有的子类共享。

```java
public interface PizzaIngredientFactory {
	public Dough createDough();
	public Sauce CreateSauce();
	public Cheese createCheese();
	public Veggies[] createVeggies();
	public Pepperoni createPepperoni();
	public Clams createClam();
}

public class CheesePizza extends Pizza {
	PizzaIngredientFactory ingredientFactory;
	
	//要制作比萨，需要工厂提供原料。所以每个比萨类都需要从构造器参数中的大一个工厂，并把这个工厂存储在一个实例变量中。
	public CheesePizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}
	
	void prepare() {
		System.out.println("Preparing " + name);
		dough = ingredientFactory.createFough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
	}
}
```
```java
//1.首先我们需要一个纽约比萨店
PizzaStore nyPizzaStore = new NYPizzaStore();
//2.现在已经有个比萨店了，可以接受订单
nyPizzaStore.orderPizza("cheese");
//3.orderPizza()方法首先调用createPizza()方法
Pizza pizza = createPizza("cheese");
//4.当createPizza()方法被调用时，也就开始涉及原料工厂
Pizza pizza = new CheesePizza(nyIngredientFactory);
//5.接下来需要准备比萨。一旦调用了prepare()方法，工厂将被要求准备原料
void prepare() {
	dough = factory.createDough();
	sauce = factory.createSauce();
	cheese = factory.createCheese();
}
//6.最后，我们得到了准备好的比萨，orderPizza()就会接着烘烤、切片、装盒

```

**抽象工厂模式**：提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。  
抽象工厂允许客户使用抽象的接口来创建一组相关的产品，而不需要知道（或关心）实际产出的具体产品是什么。这样一来，客户就从具体的产品中被解耦。  

[MerMaid文档](https://mermaid-js.github.io/mermaid/#/)  

要点：
- 所有的工厂都是用来封装对象的创建。
- 简单工厂，虽然不是真正的设计模式，但仍不失为一个简单的方法，可以将客户程序从具体类解耦。
- 工厂方法使用集成：把对象的创建委托给子类，子类实现工厂方法来创建对象。
- 抽象工厂使用对象组合：对象的创建被实现在工厂接口所暴露出来的方法中。
- 所有工厂模式都通过减少应用程序和具体类之间的依赖促进松耦合。
- 工厂方法允许类将实例化延迟到子类进行。
- 抽象工厂创建相关的对象家族，而不需要依赖它们的具体类。
- 依赖倒置原则，指导我们避免依赖具体类型，而要尽量依赖抽象。
- 工厂是很有威力的技巧，帮助我们针对抽象编程，而不要针对具体类编程。

---
单件模式
---

**单件模式（Singleton Pattern）：用来创建独一无二的，只能有一个实例的对象的入场券。**  
***单件模式***确保一个类只有一个实例，并提供一个全局访问点。  

```java
public class Singleton {
	private static Singleton uniqueInstance;
	
	private Singleton() {}
	
	public static synvhronized Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	}
	
}
```

1. 如果getInstance()的性能对应用程序不是很关键，就什么别做  
没错，如果你的应用程序可以接受getInstance()造成的额外负担，就忘了这件事吧。同步getInstance()的方法既简单又有效。但是你必须知道，同步一个方法可能造成程序执行效率下降100背。因此，如果将getInstance()的程序使用在频繁运行的地方，你可能就得重新考虑了。
2. 使用“急切”创建实例，而不用延迟实例化的做法  
```java
public class Singleton {
	private static Singleton uniqueInstance = new Singleton();
	
	private Singleton() {}
	
	public static Singleton getInstance() {
		return uniqueInstance;
	}
}
```
3. 用“双重检查加锁”，在getInstance()减少使用同步  
利用双重检查加锁（double-checked locking），首先检查是否实例已经创建了，如果尚未创建，“才”进行同步。这样一来，只有第一次会同步，这正是我们想要的。  
```java
/*
volatile关键词确保：当uniqueInstance变量被初始化成Singleton实例时，
多个线程正确地处理uniqueInstance变量
*/
public class Singleton {
	private volatile static Singleton uniqueInstance;
	
	private Singleton() {}
	
	public static Singleton getInstance() {
	/*
	检查实例，如果不存在，就进入同步区块。
	只有第一次才彻底执行这里的代码。
	*/
		if (uniqueInstance == null) {
			synchronized (Singleton.class) {
				//进入区块后，再检查一次。如果仍是null，才创建实例。
				if (uniqueInstance == null) {
					uniqueInstance = new Singleton();
				}
			}
		}
		return uniqueInstance;
	}
}
```
要点
- 单件模式确保程序中一个类最多只有一个实例。
- 单件模式也提供访问这个实例的全局点。
- 在Java中实现单件模式需要私有的构造器、一个静态方法和一个静态变量。
- 确定在性能和资源上的限制，然后小心地选择适当的方案来实现单件，以解决多线程的问题（我们必须认定所有的程序都是多线程的）。
- 如果不是采用第五版的Java 2，双重检查加锁实现会失效。
- 小心，你如果使用多个类加载器，可能导致单件失效而产生多个实例。
- 如果使用JVM 1.2或之前的banner，你必须建立单件注册表，以免垃圾收集器将单件回收。

---
封装调用：命令模式
---







































































