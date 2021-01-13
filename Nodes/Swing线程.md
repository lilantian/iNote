[Swing线程之SwingUtilities.invokeLater解释](https://blog.csdn.net/paullinjie/article/details/52002139)

在官方的文档里：[https://docs.oracle.com/javase/tutorial/uiswing/painting/step1.html](https://docs.oracle.com/javase/tutorial/uiswing/painting/step1.html)告诉我们如何创建一个gui。

事件分发线程：  

<font color=red>Swing中事件处理和绘画代码都在一个单独的线程中执行，这个线程就叫做事件分发线程。</font>这就确保了事件处理器都能串行的执行，并且绘画过程不会被事件打断。为了避免死锁的可能，你必须极度小心从事件分发线程中创建、修改、查询Swing组件以及模型。

注意：我们过去常说只要你没有修改已经实现过的组件，你就能在主进程中创建GUI。<font color=red>[补充：下面页注中的红色字体。] 已实现过的意思是组件已经在屏幕上描绘出来或是准备描绘了。方法setVisible(true)和pack可以实现一个窗口，反过来又可以实现该窗口内 包含的组件。</font>尽管这对大多数应用程序都管用，但这种做法在某些情况下会引起一些问题。在Swing Tutorial的所有示例中，我们只在ComponentEventDemo中遇到一个问题。在那个样例中，有时候当你载入样例后，它并不会启动。因为 如果在文本域还没实现的时候就去更新会出现死锁，但是其他的时候没有意外的话它也是会正常启动。  
为了避免线程问题，建议你使用invokeLater在事件分发线程中为所有新应用程序创建GUI。如果你的现有程序能工作正常，那你可能就会让它保持下去；然而，如果改造起来方便的话，还是希望你能改造一下。

你可能已经注意 到，大部分教程中的例子都使用一个标准的主函数，即SwingUtilities的函数invokeLater来保证GUI在事件分发线程中创建。这里有 一个从FocusConceptsDemo例子中提取的主函数的样例。我们还将处理创建GUI事件的主函数都要调用的一个私有静态方法，即 createAndShowGUI 的源代码包含进来了。

```java
/**
 * Create the GUI and show it. For thread safety,
 * this method should be invoked form the
 * event-dispatching thread.
 */
private static void createAndShowGUI() {
	//Make sure we have nice window decorations.
	JFrame.setDefaultLookAndFeelDecorated(true);

	//Create and set up the window.
	frame = new JFrame("FocusConceptsDemo");
	frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

	//Create and set up the content pane.
	JComponent newContentPane = new FocusConceptsDemo();
	newContentPane.setOpaque(true);//content panes must be opaque
	frame.setContentPane(newContentPane);

	//Display the window.
	frame.pack();
	frame.setVisible(true);
}

public static void main(String[] args) {
	//Schedule a job for the event-dispatching thread:
	//creating and showing this application's GUI.
	javax.swing.SwingUtilities.invokeLater( new Runnable() {
		public void run() {
			createAndShowGUI();
		}
	});
}
```

## 使用invokeLater方法
你可以从任何线程中调用invokeLater来请求事件分发线程以运行某段代 码。<font color=red>你必须将这段代码放入一个Runnable对象的run方法中，并将该指定Runnable对象作为参数传递给invokeLater。</font> invokeLater函数会立即返回，不会等到事件分发线程执行完这段代码。这里有一个使用invokeLater的例子：
```java
Runnable updateAComponent = new Runnable() {
	@Override
    public void run() {
    	component.doSomthing();
    }
};
SwingUtilities.invokeLater(updateAComponent);
```
invokeLater必须放在run()方法体内。

## 使用invokeAndWait方法

invokeAndWait方法和 invokeLater方法一样，<font color=red>除了invokeAndWait是直到事件分发线程已经执行了指定代码才返回。</font>任何可能的时候，你都应当使用 invokeLater而不是invokeAndWait——因为invokeAndWait很容易引起死锁。如果你使用invokeAndWait，要 保证调用invokeAndWait的线程不持有任何其他线程在调用时刻也会需要的锁。

这里有一个使用invokeAndWait的例子：
```java
void showHelloThereDialog() throws Exception {
	Runnable showModalDialog = new Runnable() {
		@Override
        public void run() {
            JOprionPane.showMessaeDialog(myMainframe);
        }
	};
	SwingUtilities.invokeLater(showModalDialog);
}
```
类似地，一个需要获取GUI状态的线程也需要类似的处理，比如包含两个文本域的组件，可能会有如下代码：
```java

void printTextField() throws Exception {
     final String[] myStrings = new String[ 2 ];
 
     Runnable getTextFieldText = new Runnable() {
         public void run() {
             myStrings[ 0 ] = textField0.getText();
             myStrings[ 1 ] = textField1.getText();
         }
     };
     SwingUtilities.invokeAndWait(getTextFieldText);
 
     System.out.println(myStrings[ 0 ] + " " + myStrings[ 1 ]);
}

```

## 使用线程提高性能

使用恰当的话，线程会是 一个很有用的工具。然而，当在一个Swing程序中使用线程时，你必须谨慎处理。虽然有危险，但线程还是很有用的。你可以使用线程提高你程序的响应性能。 而且，线程有时还能简化程序的代码或结构。这里有一些使用线程的特殊场景：

- 将一个比较消耗时间的初始化任务移出主线程，可以使GUI出现得更快。耗时任务包括做额外的计算任务以及网络阻塞或硬盘I/O（比如，载入图片）。
- 将耗时任务移出事件分发线程，从而使GUI同时继续响应用户操作。
- 为了能重复执行一个操作，通常还需要在操作之间设置一个预置时间段（定时器）。
- 等待其他程序的消息。

如果你需要创建一个线程，可以通过使用工具类如SwingWorker或是Timer类中的一个实现该线程，这样一来能够避免一些常见的陷阱。SwingWorker对象创建一个线程以执行一个耗时操作。

当该操作结束后，SwingWoker会在事件分发线程中给你提供执行额外代码的选项。Timer类则适合重复执行或需要一段延时执行的操作。如果你需要实现自己的线程，可以在Concurrency中找到相关信息。

可以使用一些技巧来使多线程的Swing程序有更好的性能：

- 如果当你需要更新一个组件但事件监听器中的代码并没有执行的时候，可以考虑使用这两个方法，SwingUtilities的invokeLater（优先选项）或invokeAndWait方法。
- 如果你不能确定事件监听器中的代码 是否已经执行，那你就应当分析程序代码和线程中每个函数的调用文件。如果还是不行，可以使用SwingUtilities的 isEventDispatchThread方法。当该方法在事件分发线程中执行时返回true。你能在任何线程中安全地调用invokeLater，但 invokeAndWait会在调用线程不是事件分发线程时抛出异常。
- 如果你需要在一段延迟之后更新组件（不论你的代码目前是否正在一个事件监听器中运行），那就使用定时器timer吧。
- 如果你需要在没经过一段规律的时间间隔后更新组件，使用定时器timer。

使用timer定时器的信息和例子，请见如何使用Swing定时器。

## 使用SwingWorker类

注意：SwingWorker类的实现已经有过两次更新了，最近的一次是2000年的2月。第一次更新（在1999年1月）允许程序安全地中断工作线程。最近的一次更新（称作“SwingWorker 3”）修正了一个会引起空指针异常NullPointerException的隐藏线程bug。

类SwingWorker在SwingWorker.java中 实现，它没有在Swing包中发布。使用SwingWorker类之前，需要先创建一个SwingWorker的子类。子类必须要实现构造函数使它能包含 执行耗时操作的代码。当初始化SwingWorker子类的时候，SwingWorker类创建了一个线程但没启动它（截至SwingWorker 3）。然后才是调用SwingWorker对象的启动start方法来启动线程，就是调用构造函数。

这里有一个例子，使用SwingWorker类将耗时任务从动作事件监听器中移动到后台线程中，从而使GUI能保持响应。

```java
//OLD CODE:
public void actionPerformed(ActionEvent e) {
     ...
     //...code that might take a while to execute is here...
     ...
}
 
//BETTER CODE:
public void actionPerformed(ActionEvent e) {
     ...
     final SwingWorker worker = new SwingWorker() {
         public Object construct() {
             //...code that might take a while to execute is here...
             return someValue;
         }
     };
     worker.start();  //required for SwingWorker 3
     ...
}
```
构造函数的返回值可以是任意的对象。想获取该返回值，可以调用SwingWorker对象的get函数。对于get函数要小心使用。因为它会阻塞，会引起死锁。如果有必要的话，可以调用SwingWorker的中断函数interrupt中断线程（引起函数返回）。

如果你想在耗时 任务完成的时候更新GUI，可以调用get函数（这个正如需要注意的那样，有些危险）或是重写你SwingWorker子类的finished函数。 Finished函数会在构造函数返回后执行。因为finished函数在事件分发线程中执行，你能安全地使用它更新Swing组件。当然，你最好不要把 耗时操作放进finished函数中。

下面实现finished函数的例子是从IconDemoApplet.java文件中拿来的。若想充分地讨论这个applet，包括如何使用后台线程载入图片以提高响应性能，请看如何使用Icons。

```java
public void actionPerformed(ActionEvent e) {
     ...
     if (icon == null ) {     //haven't viewed this photo before
         loadImage(imagedir + pic.filename, current);
     } else {
         updatePhotograph(current, pic);
     }
}
...
//Load an image in a separate thread.
private void loadImage( final String imagePath, final int index) {
     final SwingWorker worker = new SwingWorker() {
         ImageIcon icon = null ;
 
         public Object construct() {
             icon = new ImageIcon(getURL(imagePath));
             return icon; //return value not used by this program
         }
 
         //Runs on the event-dispatching thread.
         public void finished() {
             Photo pic = (Photo)pictures.elementAt(index);
             pic.setIcon(icon);
             if (index == current)
                 updatePhotograph(index, pic);
         }
     };
     worker.start();
}
```
更多使用SwingWorker的例子，请看How to Monitor Progress。还有，TumbleItem.java，在How to Make Applets中讨论过的，既使用了SwingWorker，还使用了计时器Timer。

讨论：[http://www.ime.uerj.br/javatutor/uiswing/misc/threads.html](http://www.ime.uerj.br/javatutor/uiswing/misc/threads.html)

官方解释：
```java
public static void invokeLater(Runnable doRun)
```

函数public static void invokeLater(Runnable doRun)引起函数doRun.run()在AWT的事件分发线程中异步执行。这个函数应该在程序线程需要更新GUI的时候调用。下面的例子中，invokeLater调用事件分发线程的Runnable对象doHelloWorld到队列中，然后打印一条信息。

```java
Runnable doHelloWorld = new Runnable() {
     public void run() {
         System.out.println( "Hello World on " + Thread.currentThread());
     }
};
```
如果是从事件分发线程调用的invokeLater——比如，从一个JButton的监听器中——doRun.run()会一直延迟到事件队列中的所有待处理事件都处理完才 执行。要注意，当doRun.run()抛出一个未捕获的异常时，事件分发线程会释放掉（不是当前线程）。

至于1.3部分，这个函数是由java.awt.EventQueue.invokeLater()包装而来。

不像Swing的其他函数，这个函数可以从任意线程中调用。











