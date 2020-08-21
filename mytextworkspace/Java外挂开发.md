# Java外挂开发

```java

public class Plug1 {
	
	public static void main(String[] args) throws AWTException {
		Robot robot=new Robot();
		
		
		while(true){
			//延迟时间，方便切换到需要按键的窗口
			robot.delay(3000);
			//按下某建
			robot.keyPress(KeyEvent.VK_K);
			Random random=new Random();
			
//			System.out.println();
			//模拟用户行为，网易等公司会检测用户每次按键用的时间，全部一样则封号
			int randomtime=(int)random.nextDouble()*3000;
			
			robot.delay(randomtime+1000);
			//松开某建
			robot.keyRelease(KeyEvent.VK_K);
			
			
//			按一个或多个鼠标按钮。
//			mousePress(int buttons)
			
//			
//			释放一个或多个鼠标按钮。
//			mouseRelease(int buttons)
			
			
//			在装有轮子的鼠标上旋转滚轮
//			mouseWheel(int wheelAmt)
			
		
//			返回给定屏幕坐标处的像素颜色
//			
			
		Color color=robot.getPixelColor(540, 540);
		System.out.println(color.getBlue());
		System.out.println(color.getGreen());
		System.out.println(color.getRed());
		
		//可以根据屏幕的像素位置的颜色判断其颜色，如果血条为灰色可以进行补血操作
			
//			将鼠标指针移动到给定的屏幕坐标
			robot.mouseMove(30, 0);
			
			//按下鼠标左键
			robot.mousePress(InputEvent.BUTTON1_MASK);
			
			
		}
		
		
	}

}
```



常用的外挂可以C 、C++ 、 Delphi 、vb、易语言进行开发

按键精灵，lua





使用Java语言开发外挂的利弊：

利：不容易被发现，按键精灵什么的只要启动就会被游戏公司的安全软件拦截

弊：不能发送钩子（当前焦点窗口不是具体的目标应用话使用无效果）

