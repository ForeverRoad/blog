---
title: 设计模式
date: 2022-04-08 18:08:09
tags: 设计模式
categories: 设计模式
---



#### 设计模式基础概念

- 单一职责原则
	+ 根据职责划分模块
- 里氏替换原则
	+ 子类可以代替父类
- 依赖倒置原则
	+ 高级模块，低级模块不相互依赖，而都是依赖于抽象（接口化编程，降低耦合度）
	
- 开闭原则
	+ 对扩展开放，对修改关闭
	

<!-- more -->

#### 单例模式

> 确保只有一个类实例化，提供给整个系统使用

- 优点：
	+ 只有一个实例，减少了内存等资源消耗，避免了实例的创建和销毁
	+ 避免对资源的多重占用，例如一个写文件动作，由于只有一个实例存在 内存中，避免对同一个资源文件的同时写操作
- 缺点：
	+ 没有接口，不易扩展
	
- 使用场景：
	+ 全局唯一的计数或生成序列化的
	+ 创建一个对象需要消耗的资源过多，如要访问IO和数据库等资源
	


``` java

	// 饿汉式单例，一开始就创建，可能会造成资源浪费
	public class Singleton { 
		//初始化一个实例
		private static final Singleton singleton = new Singleton();  

		// 构造参数私有
		private Singleton(){ 
			
		}
		// 通过方法获取到唯一的实例
		public static Singleton getInstance(){ return singleton; }
		
		public static void say(){ System.out.println("...."); } 
	}

	// 懒汉式单例，只有在被调用的时候实例化，但是有并发问题
	public class Singleton {
		//初始化一个实例，使用volatile关键字保证其他线程可见性
		private static volatile Singleton singleton = null;  

		// 构造参数私有
		private Singleton(){ 
			
		}
		// 通过方法获取到唯一的实例
		public static Singleton getInstance(){ 
			// 通过双重检查，把锁的影响范围最小化
			if (singleton == null){
	            synchronized(Singleton.class){
	                if (singleton == null) {
	                    singleton = new Singleton();
	                }
	            }
        	}
			return singleton;
		}
		
		public static void say(){ System.out.println("...."); } 
	}





```


#### 工厂模式

> 通过工厂类决定具体实例化哪个实现类，通过封装，降低耦合性。类似于工厂的模块化，需要什么品牌的零件，就安装对应品牌的零件，不需要关心具体。

- 优点：
	+ 封装性好，解耦
	
- 缺点：
	+ 新增产品族比较困难

``` java
	// 抽象产品类
	public abstract class Product { 
	
		public abstract void say(); 
	}
	// 具体的实现产品类
	public class ConcreteProduct1 extends Product { public void say() { //业务逻辑处理 } }

	public class ConcreteProduct2 extends Product { public void say() { //业务逻辑处理 } }

	// 抽象工厂类
	public abstract class Creator { 
		/** 创建一个产品对象，其输入参数类型可以自行设置 * 通常为String、Enum、Class等，当然也可以为空 */ 
		public abstract <T extends Product> T createProduct(Class<T> c); 
	}
	// 工厂实现类
	public class ConcreteCreator extends Creator {
		// 通过传入的class决定具体实例化哪个产品
        public <T extends Product> T createProduct(Class<T> c) {
            Product product = null;
            try {
                product = (Product) Class.forName(c.getName()).newInstance();
            } catch (Exception e) { 
                //异常处理 
            }
            return (T)product; 
        } 
    }

```


#### 模板方法

> 定义一个操作中的算法的框架，而将一些步骤延迟到子类中。使得子类可以不改 变一个算法的结构即可重定义该算法的某些特定步骤。

- 基础方法：
	+ 具体的逻辑基本操作方法，被子类实现，在模板方法中被调用
- 模板方法：
	+ 可以有一个或几个，一般是一个具体方法，也就是一个框架，实现对基本方法的调度， 完成固定的逻辑。

- 优点：
	+ 父类控制行为，子类负责实现
	+ 封装不可变部分，扩展可变
	+ 提取公共代码部分，便于维护
	
``` java

	public abstract class Model { 
		// 基本方法
		public abstract void start(); 
		public abstract void say(); 
		public abstract void end(); 

		// 模板方法
		public void run(){
			this.start();
			this.say();
			this.end();
		}
	}


	public class ModelMethod extends Model{ 
		// 基本方法
		public abstract void start(){
			// do something
		} 
		public abstract void say(){
			// do something
		} 
		public abstract void end(){
			// do something
		} 
		
	}

	public static void main(String[] args) {
		ModelMethod model = new ModelMethod();
		model.run();
	}


```



#### 建造者模式


> 将一个复杂对象的构建与它的表示分 离，使得同样的构建过程可以创建不同的表示。


- 适用于多参数，或者执行顺序不固定，类似于链式调用
- 优点：
	+ 封装性，不需要关 心每一个具体的模型内部是如何实现的
	+ 建造者独立，容易扩展，builder类独立的，可以扩展不同的产品
	+ 便于控制细节风险
- 建造者模式关注的是零件类型和装配工艺（顺序），这是它与工厂方法模式最大不同的 地方，虽然同为创建类模式，但是注重点不同。


``` java

	// Product: 最终要生成的对象，例如 Computer实例。
	public class Product { 
		public void doSomething(){ 
			//独立业务处理 
		}
	}
	// Builder： 构建者的抽象基类（有时会使用接口代替）。其定义了构建Product的抽象步骤，其实体类需要实现这些步骤。其会包含一个用来返回最终产品的方法Product getProduct()。
	public abstract class Builder { 
		//设置产品的不同部分，以获得不同的产品 
		public abstract void setPart(); 
		public abstract void setName(); 
		//建造产品 
		public abstract Product buildProduct(); 
	}

	// ConcreteBuilder: Builder的实现类,具体建造者。
	public class ConcreteProduct extends Builder { 
		private Product product = new Product(); 
		//设置产品零件
		public void setPart(){ 
		/** 产品类内的逻辑处理 */ 
		}
		public void setName(){ 
		/** 产品类内的逻辑处理 */ 
		}
		//组建一个产品 
		public Product buildProduct() { 
			return product; 
		} 
	}

	// Director: 决定如何构建最终产品的算法. 其会包含一个负责组装的方法void Construct(Builder builder)， 在这个方法中通过调用builder的方法，就可以设置builder，等设置完成后，就可以通过builder的 getProduct() 方法获得最终的产品。

	public class Director { 
		private Builder builder = new ConcreteProduct(); 
		//构建不同的产品 
		public Product getAProduct(){ 
			builder.setPart(); 
			/** 设置不同的零件，产生不同的产品 */ 
			// builder.setName();
			
			return builder.buildProduct(); 
		} 
	}


```




#### 代理模式

> 为其他对象提供 一种代理以控制对这个对象的访问

- 常用于AOP，日志等不侵入业务逻辑方法而实现对象的访问控制
- 优点：
	+ 职责清晰，真实业务逻辑与代理处理无关
	+ 高扩展性，不管真实业务逻辑如何变化，代理毫无影响
	
- 使用代理必须有接口，除非使用CJLIB

``` java
	
	// 简单代理

	// 接口类
	public interface Subject { 
	//定义一个方法 
		public void request(); 
	}

	// 实现类
	public class RealSubject implements Subject{ 
		public void request(){
			// do somethings
		}
	}

	// 代理类
	public class Proxy implements Subject{ 
		//要代理哪个实现类 
		private Subject subject = null; 
		//默认被代理者 
		public Proxy(){ this.subject = new Proxy(); }
		//通过构造函数传递代理者 
		public Proxy(Object...objects ){
		}
		//实现接口中定义的方法 
		public void request() { 
			this.before();
			// 调用真实对象的方法
			this.subject.request(); 
			this.after(); 
		}
		//预处理 
		private void before(){ 
			//do something 
		}
		//善后处理 
		private void after(){ 
			//do something 
		}
	}


	// 动态代理
	// 通过InvocationHandler获取到代理对象，调用真实对象的方法

	public class MyProxy implements InvocationHandler { 
		//被代理者 
		Class cls =null; 
		//被代理的实例 
		Object obj = null; 
		//我要代理谁 
		public MyProxy(Object obj){ this.obj = obj; }
		//调用被代理的方法 
		public Object invoke(Object proxy, Method method, Object[] args) throws Throwable { 
			Object result = method.invoke(this.obj, args); 
			return result; 
		} 
	}


	public static void main(String[] args) {
		Subject subject = new RealSubject();
		InvocationHandler handler = new Proxy(subject);
		MyProxy myProxy = (MyProxy)Proxy.newProxyInstance(player.getClass().getClassLoader(),handler);
		myProxy.request();
	}

```


#### 责任链模式

> 通过链式调用，解耦了请求方和处理方,通过顶级处理方接收请求，并决定处理还是传递到下一级处理

- 性能问题，每个请求都是从链头遍历到链尾，特别 是在链比较长的时候
- 调试不很方便，特别是链条比较长， 环节比较多的时候，由于采用了类似递归的方式，调试的时候逻辑可能比较复杂

``` java

public abstract class Handler { 
	private Handler nextHandler; 
	//每个处理者都必须对请求做出处理
	public final Response handleMessage(Request request){ 
		Response response = null; 
		//判断是否是自己的处理级别 
		if(this.getHandlerLevel().equals(request.getRequestLevel())){ 
			response = this.echo(request); 
		}else{ 
			//不属于自己的处理级别 
			//判断是否有下一个处理者 
			if(this.nextHandler != null){ 
				response = this.nextHandler.handleMessage(request); 
			}else{ 
			//没有适当的处理者，业务自行处理 
			} 
		}
		return response; 
	}
	//设置下一个处理者是谁 
	public void setNext(Handler _handler){ 
	this.nextHandler = _handler; 
	}
	//每个处理者都有一个处理级别 
	protected abstract Level getHandlerLevel(); 
	//每个处理者都必须实现处理任务 
	protected abstract Response echo(Request request); 
}

```


#### 装饰模式

> 动态地给一个对象添加一些额外的职责。 就增加功能来说，装饰模式相比生成子类更为灵活

- 装饰类和被装饰类可以独立发展，而不会相互耦合。换句话说，Component类无须知 道Decorator类，Decorator类是从外部来扩展Component类的功能，而Decorator也不用知道具 体的构件。
- 装饰模式是继承关系的一个替代方案。我们看装饰类Decorator，不管装饰多少层，返回的对象还是Component，实现的还是is-a的关系。
- 装饰模式可以动态地扩展一个实现类的功能，这不需要多说，装饰模式的定义就是如此。
- 应避免多层装饰，减少问题定位的麻烦。

``` java
// 基本对象
public abstract class Component { 
	//抽象的方法 
	public abstract void operate(); 
}
public class ConcreteComponent extends Component { 
	//具体实现 
	@Override public void operate() { 
		System.out.println("do Something"); 
	} 
}

// 装饰类
public abstract class Decorator extends Component { 
	private Component component = null; 
	//通过构造函数传递被修饰者 
	public Decorator(Component _component){ 
	this.component = _component; 
	}
	//委托给被修饰者执行 
	@Override public void operate() { 
		this.component.operate(); 
	} 
}

public class ConcreteDecorator extends Decorator { 
	//定义被修饰者 
	public ConcreteDecorator1(Component _component){ 
	super(_component); 
	}
	//定义自己的修饰方法 
	private void method1(){ 
		System.out.println("method1 修饰"); 
	}
	//重写父类的Operation方法 ，加入修饰的逻辑
	public void operate(){ 
		this.method1(); 
		super.operate(); 
	} 
}

public static void main(String[] args) { 
	Component component = new ConcreteComponent(); 
	//进行修饰 
	component = new ConcreteDecorator1(component); 
	//修饰后运行 
	component.operate(); 
}


```


#### 策略模式

> 定义一组算法，将每个算法都封装起来，并且使它们之间可以互换。

- 优点：
	+ 算法可以自由切换
	+ 避免使用多重条件判断
	+ 扩展性良好

- 缺点：
	+ 策略类数量增多
	+ 所有的策略类都需要对外暴露

``` java
	// 抽象的策略类
	public interface Strategy { 
		//策略模式的运算法则 
		public void doSomething(); 
	}
	// 多种不同的策略实现
	public class ConcreteStrategy1 implements Strategy { 
		public void doSomething() { 
			System.out.println("具体策略1的运算法则"); 
		} 
	}
	public class ConcreteStrategy2 implements Strategy { 
		public void doSomething() { 
			System.out.println("具体策略1的运算法则"); 
		} 
	}
	// 调用某种策略
	public class Context { 
		//抽象策略 
		private Strategy strategy = null; 
		//构造函数设置具体策略 
		public Context(Strategy _strategy){ 
		this.strategy = _strategy; 
		}
		//封装后的策略方法 
		public void doAnythinig(){ 
			this.strategy.doSomething(); 
		} 
	}
	// 也可根据标识从注入的策略map中获取
	Map<String,Strategy> map = new HashMap<>();
	// 注入策略实现

	public class Context {
		
		//封装后的策略方法
		public void doAnythinig(String key){

			map.get(key).doSomething();
		}
	}



```

#### 适配器模式

> 将一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作

- Target目标角色:该角色定义把其他类转换为何种接口，也就是我们的期望接口
- Adaptee源角色:它是已经存在的、运行良好的类或对 象，经过适配器角色的包装，它会成为一个崭新、靓丽的角色
- Adapter适配器角色:适配器模式的核心角色,把源角色转换为目标角色


```java

public interface Target { 
	//目标角色有自己的方法 
	public void request(); 
}

//目标角色的实现类
public class ConcreteTarget implements Target { 
	public void request() { 
		System.out.println("if you need any help,pls call me!"); 
	} 
}

public class Adaptee { 
	//原有的业务逻辑 
	public void doSomething(){ 
		System.out.println("I'm kind of busy,leave me alone,pls!"); 
	} 
}

// 适配器角色
public class Adapter extends Adaptee implements Target { 
	public void request() { 
		super.doSomething(); 
	} 
}

public static void main(String[] args) { 
	//原有的业务逻辑 
	Target target = new ConcreteTarget(); 
	target.request(); 
	//现在增加了适配器角色后的业务逻辑 
	Target target2 = new Adapter(); 
	target2.request(); 
}
```


#### 观察者模式（发布订阅）

> 定义对象间一种一对多的依赖关系，使得每 当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。

``` java

// 被观察者
public abstract class Subject { 
	//定义一个观察者数组 
	private Vector<Observer> obsVector = new Vector<Observer>(); 
	//增加一个观察者 
	public void addObserver(Observer o){ 
	this.obsVector.add(o); 
	}
	//删除一个观察者 
	public void delObserver(Observer o){ 
	this.obsVector.remove(o); 
	}
	//通知所有观察者 
	public void notifyObservers(){ 
		for(Observer o:this.obsVector){ 
			o.update(); 
		} 
	} 
}
// 观察者
public interface Observer { 
	//更新方法 
	public void update(); 
}
```