# AWT图像界面编程

## 继承体系

![1192257-20171203105908679-1790600814](0-AWT编程.assets/1192257-20171203105908679-1790600814.png) 

![1192257-20171203110025991-1435166983](0-AWT编程.assets/1192257-20171203110025991-1435166983.png) 

> - Window是可以独立存在的顶级窗口，默认使用`BorderLayout`管理其内部组建布局
> - Panel可以容纳其他组件，但不能独立存在，他必须内嵌其他容器中使用，默认使用`FlowLayout`管理其内部组建布局
> - ScrollPane是一个带滚动条的容器，他也不能独立存在，默认使用`BorderLayout`管理其内部组建布局



## 常用API

Component作为基类，提供了如下常用的方法来设置组件的大小，位置，可见性等

|                    方法                     |           功能           |
| :-----------------------------------------: | :----------------------: |
|          setLocation(int x, int y)          |      设置组件的位置      |
|       setSize(int width, int height)        |      设置组件的大小      |
| setBounds(int x,int y,int width,int height) | 同时设置组件的位置和大小 |
|            setVisible(Boolean b)            |     设置组件的可见性     |



Container作为容器根类，提供了如下的方法来访问容器中的组件

|                方法                 |                             功能                             |
| :---------------------------------: | :----------------------------------------------------------: |
|    Component add(Component comp)    | 向容器中添加其他组件(该组件既可以是普通组件，也可以是容器)，并返回被添加的组件 |
| Component getComponent(int x,int y) |                        返回指定的组件                        |
|       int getComponentCont()        |                    返回该容器内组件的数量                    |
|     Component[] getComponents()     |                    返回该容器内所有的组件                    |

示例程序1：

```java
public static void main(String[] args) {
    // TODO Auto-generated method stub
    // 创建窗口
    Frame frame = new Frame("测试程序");

    // 设置窗口位置和大小
    frame.setLocation(100, 100);
    frame.setSize(400, 400);

    // 设置窗口可见
    frame.setVisible(true);
}
```



示例程序2：

```java
public static void main(String[] args) {
    // TODO Auto-generated method stub
    // 创建Window窗口(由于出window容器之外的其他容器不能单独存在)
    Frame frame = new Frame("测试程序");

    // 创建Panel
    Panel panel = new Panel();

    panel.add(new TextField("文本域"));
    panel.add(new Button("按钮"));

    // 将Panel添加到Frame中
    frame.add(panel);
    // 设置窗口位置和大小
    frame.setLocation(100, 100);
    frame.setSize(400, 400);

    // 设置窗口可见
    frame.setVisible(true);
}
```

> 补：Eclipse中文乱码解决方法：
>
> 在Arguments标签下的VM arguments中添加下面这行参数代码，然后点击应用。
>
>   ` -Dfile.encoding=GB18030`





## 布局管理器

之前，介绍了Component中有一个方法setBounds()可以设置当前容器的位置和大小，但是我们需要明确一件事，如果我们手动为组件设置了位置和大小的话，就会造成程序的不通用型，例如

```java
Label label = new Label("你好 世界")
```

创建了一个label组件，很多情况下，我们需要让label组件的宽高和“你好 丗界”这个字符串自身的宽高一致，这种大小成为最佳大小，由于OS存在差异，例如在windows上，我们要达到这样的效果，需要把该Label组件的宽和高分别设置为100 px，20 px,但是在Linux操作系统上，可能需要把宽高分别设置为120 px，24 px才能达到同样的效果。

如果要让我们的程序在不同OS下都有同样的使用体验，那么手动设置组件的位置和大小无疑是一种灾难，因为有太多的组件需要分别设置不同OS下的大小和位置。为了解决这个问题，JAVA提供了布局管理器，可以根据运行平台来自动调整组建大小，程序员不用再手动设置组件的大小和位置了，只需要为容器选择合适的布局管理器即可。



在AWT中，提供了五种布局管理器：

- **BorderLayout**

- **FlowLayout**
- **GridLayout**
- **CardLayout**
- **GridBagLayout**

























































