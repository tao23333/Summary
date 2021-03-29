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



---

### FlowLayout

> 在FlowLayout布局管理器中，组件像流水一样向某方向流动（排列），遇到障碍（边界）就会折回，重头开始排列。默认情况下,==FlowLayout布局管理器从左向右排列所有的组件，遇到边界就会折回下一行重新开始==

|                 构造方法                  |                     方法功能                      |
| :---------------------------------------: | :-----------------------------------------------: |
|               FlowLayout()                | 使用默认的对齐方式及默认的垂直/水平间距创建管理器 |
|           FlowLayout(int align)           |                   指定对齐方式                    |
| FlowLayout(int align, int hgap, int vgap) |         指定对齐方式及指定的水平/垂直间距         |

- 对齐方式：FlowLayout.LEFT  FlowLayout.CENTER  FlowLayout.RIGHT
- FlowLayout中组件之间的间距通过整数设置、单位是像素，默认是5个像素



示例代码：

```java
// 创建Window窗口(由于出window容器之外的其他容器不能单独存在)
Frame frame = new Frame("测试程序");

frame.setLayout(new FlowLayout(FlowLayout.LEFT));

for(int i=0;i<100;i++)
{
    frame.add(new Button("按钮"+(i+1)));
}

// 将窗口调整至最佳大小
frame.pack();

// 设置窗口可见
frame.setVisible(true);
```



### BorderLayout

> BorderLayout将容器分为EAST SOUTH NORTH CENTER 5个区域，普通组件可以被放置在5个区域的任意一个中。

BorderLayout的示意图：

![1342932338_7774](0-AWT编程.assets/1342932338_7774.png) 



当改变使用BorderLayout布局管理器的容器大小时，NORTH SOUTH CENTER区域水平调整，而EAST WEST和CENTER区域垂直调整。使用BorderLayout有如下几个注意点：

- 当向使用BorderLayout布局管理器的容器添加组件时，需要指定要添加到哪个区域中，如果没有指定添加到哪个区域中，则默认添加到中间区域
- 如果向同一个区域中添加多个组件时，后放入的组件会覆盖先放入的组件
- 如果不向某个区域添加组件，那么该区域不会空出来，而是会被其他区域所占用



|             构造方法             |                     方法功能                      |
| :------------------------------: | :-----------------------------------------------: |
|          BorderLayout()          | 使用默认的水平/垂直间距创建BorderLayout布局管理器 |
| BorderLayout(int hgap, int vgap) | 使用指定的水平/垂直间距创建BorderLayout布局管理器 |



### GrideLayout

> ​	GridLayout布局管理器将容器分割成纵横线分隔的网格，每个网格所占用的区域大小相同。当向使用GridLayout布局管理器的容器中添加组件时，默认==从左向右、从上向下==依次添加到网格中。与FlowLayout不同的是，放置在GridLayout布局管理器中的各组件的大小由组件所处的区域决定（每个组件将自动占满整个区域）

|                      构造方法                      |                           方法功能                           |
| :------------------------------------------------: | :----------------------------------------------------------: |
|           GridLayout(int rows, int cols)           | 采用指定的行数，列数以及默认的横向间距，纵向间距将容器分隔成多个网格 |
| GridLayout(int rows, int cols, int hgap, int vgap) | 采用指定的行数，列数，横向间距，纵向间距将容器分隔成多个网格 |

示例程序：

![2021-03-28_174957](0-AWT编程.assets/2021-03-28_174957.png) 



### CardLayout

> CardLayout布局管理器以时间而非空间来管理它里面的组件，它将加入容器的所有组件看成一叠卡片，每次只有最上面的那个组件才可见。

|              方法名称               |            方法功能            |
| :---------------------------------: | :----------------------------: |
|            CardLayout()             | 创建默认的CardLayout布局管理器 |
|    CardLayout(int hgap,int vgap)    |            指定边距            |
|       first(Container target)       |   显示target容器的第一张卡片   |
|       last(Container target)        |  显示target容器的最后一张卡片  |
|     previous(Container target)      |   显示target容器的上一张卡片   |
|       next(Container target)        |   显示target容器的下一张卡片   |
| show(Container target, String name) |  显示target容器指定名字的卡片  |

示例程序：

```java
public static void main(String[] args) {
    // TODO Auto-generated method stub
    Frame frame = new Frame("测试");

    // 创建一个panel用来存放多张卡片(组件)
    Panel p1 = new Panel();

    // 设置panel的布局管理器为卡片布局
    CardLayout cardLayout = new CardLayout();
    p1.setLayout(cardLayout);

    // 卡片名字
    String[] names = {"第一张","第二张","第三张","第四张","第五张"};

    for(int i=0;i<5;i++) {
            p1.add(names[i],new Button(names[i]));
        }

    // 将panel放到frame的中间区域
    frame.add(p1);

    // 创建另外一个p2，用来存储多个按钮组件
    Panel p2 = new Panel();

    // 创建5个按钮
    Button b1 = new Button("上一张");
    Button b2 = new Button("下一张");
    Button b3 = new Button("第一张");
    Button b4 = new Button("最后一张");
    Button b5 = new Button("第三张");

    // 创建事件监听器
    ActionListener listener = new ActionListener() {

        @Override
        public void actionPerformed(ActionEvent e) {
            // TODO Auto-generated method stub
                String name = e.getActionCommand();  // 返回的字符串就是所点击按钮的label
                switch(name) {
                    case "上一张":
                        cardLayout.previous(p1);
                        break;
                    case "下一张":
                        cardLayout.next(p1);
                        break;
                    case "第一张":
                        cardLayout.first(p1);
                        break;
                    case "最后一张":
                        cardLayout.last(p1);
                        break;
                    case "第三张":
                        cardLayout.show(p1, "第三张");
                        break;
                }
            }

        };

    // 按钮与事件绑定
    b1.addActionListener(listener);
    b2.addActionListener(listener);
    b3.addActionListener(listener);
    b4.addActionListener(listener);
    b5.addActionListener(listener);

    // 将按钮添加到p2
    p2.add(b1);
    p2.add(b2);
    p2.add(b3);
    p2.add(b4);
    p2.add(b5);


    // 将p2添加到frame的南部区域
    frame.add(p2, BorderLayout.SOUTH);

    frame.pack();
    frame.setVisible(true);
}

```

![2021-03-29_195147](0-AWT编程.assets/2021-03-29_195147.png) 



### BoxLayout

> 为了简化开发，Swing引入了一个新的布局管理器：BoxLayout。BoxLayout可以在垂直和水平两个方向摆放GUI组件，BoxLayout提供了如下一个简单的构造器：

|               方法名称                |                           方法功能                           |
| :-----------------------------------: | :----------------------------------------------------------: |
| BoxLayout(Container target, int axis) | 指定创建基于target容器的BoxLayout布局管理器，该布局管理器里的组件按axis方向排列。其中axis有BoxLayout.X_AXIS（横向）和BoxLayout.Y_AXIS（纵向）两个方向 |

![2021-03-29_201834](0-AWT编程.assets/2021-03-29_201834.png) 



## Box容器

> 在java.swing包中，提供了一个新的容器Box，该容器的默认布局管理器就是BoxLayout，大多数情况下，使用Box容器去容纳多个GUI组件，然后再把Box容器作为一个组件添加到其他的容器中，从而形成整体窗口布局。



|             方法名称             |           方法功能            |
| :------------------------------: | :---------------------------: |
| static Box createHorizontalBox() | 返回一个水平排列组件的Box容器 |
|  static Box createVerticalBox()  | 返回一个垂直排列组件的Box容器 |

 ![2021-03-29_202729](0-AWT编程.assets/2021-03-29_202729.png)



### 添加间隔

在需要有间隔的地方添加间隔组件，该组件没有内容，仅仅起到一种分隔作用。

Box类中，提供了5个方便的静态方法来生成这些间隔组件

|                     方法名称                      |                    方法功能                    |
| :-----------------------------------------------: | :--------------------------------------------: |
|      static Component createHorizontalGlue()      | 创建一条水平Glue(可在两个方向上同时拉伸的间距) |
|       static Component createVerticalGlue()       | 创建一条垂直Glue(可在两个方向上同时拉伸的间距) |
| static Component createHorizontalStrut(int width) |           创建一条固定宽度的水平间距           |
| static Component createVerticalStrut(int height)  |           创建一条固定高度的垂直间距           |



## AWT中常用组件

### 基本组件

|    组件名     |                             功能                             |
| :-----------: | :----------------------------------------------------------: |
|    Button     |                             按钮                             |
|    Canvas     |                        用于绘图的画布                        |
|   Checkbox    |               复选框组件（亦可作为单选框使用）               |
| CheckboxGroup | 用于将多个Checkbox组件组合成一组，一组Checkbox组件将只有一个可以被选中，即全部变成单选框组件 |
|    Choice     |                          下拉选择框                          |
|     Frame     |              窗口，在GUI程序中通过该类创建窗口               |
|     Label     |                  标签类，用于放置提示性文本                  |
|     List      |                 列表框组件，可以添加多项条目                 |
|     Panel     |         不能单独存在基本容器类，必须添加到其他容器中         |
|  ScrollPane   |                 带水平及垂直滚动条的容器组件                 |
|   TextArea    |                          多行文本域                          |
|   TextField   |                          单行文本框                          |

```java
// 单选
Checkbox checkbox = new Checkbox("是否...");

// 多个单选
CheckboxGroup cbg = new CheckboxGroup();
Checkbox male = new Checkbox("男",cbg,true);  // 分为cbg这组，默认选中
Checkbox female = new Checkbox("女",cbg,false);

// 下拉菜单
Choice choice = new Choice();
choice.add("red");
choice.add("blue");

// 列表框
List colorList = new List(6,true); // 行数为6，可以多选
colorList.add("red");
colorList.add("blue");
```



### 对话框Dialog

> Dialog是Window类的子类，是一个==容器类==，属于特殊组件。对话框是可以独立存在的顶级窗口，因此用法与普通窗口的用法几乎完全一样，但是使用对话框需要注意以下两点：
>
> - 对话框通常依赖于其他窗口，就是通常需要有一个父窗口
> - 对话框有非模式（non-modal）和模式（modal）两种，当某个模式对话框被打开后，该模式对话框总是位于它的父窗口智商，在模式对话框被关闭之前，父窗口无法获得焦点

|                     方法名称                     |                           方法功能                           |
| :----------------------------------------------: | :----------------------------------------------------------: |
| Dialog(Frame owner, String title, boolean modal) | 创建一个对话框对象；owner：当前对话框的父窗口；<br />title：当前对话框的标题；<br />modal：当前对话框是否是模式对话框，true/false |



### FileDialog

> Dialog类还有一个子类：FileDialog，它代表一个文件对话框，用来打开或者保存文件，需要注意的是FileDialog无法指定模态或者非模态，这是因为FileDialog依赖于运行平台的实现，如果运行平台的文件对话框是模态的，那么FileDialog也是模态的；否则就是非模态的。



|                     方法名称                     |                           方法功能                           |
| :----------------------------------------------: | :----------------------------------------------------------: |
| FileDialog(Frame parent, String title, int mode) | 创建一个对话框；<br />parent：指定父窗口；<br />title：对话框标题；<br />mode：文件对话框类型，如果指定为FileDialog.LOAD,用于打开文件，如果指定为FileDialog.SAVE,用于保存文件 |
|              String getDirectory()               |                获取被打开或保存文件的绝对路径                |
|                 String getFile()                 |                获取被打开或者保存文件的文件名                |



































































