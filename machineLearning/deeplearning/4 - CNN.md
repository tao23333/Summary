> 图像的输入数据要将其格式变为（batch，channel，width，height）
>
> 用Pytorch读入图像，默认为W,H,C，要用transform将其转为C,W,H；还要将格式转为Tensor，默认读进来时PIL Image

- 卷积层的卷积核数量决定了输出的channel数量，每个卷积核层数与输入channel相同，每个卷积核大小(w,h)决定了输出的(w,h)[可以设置padding，可使得图像输出与输出w,h不变]，stride是卷积的步长，设置stride之后，可以不加池化层

  - `self.cov2 = torch.nn.Conv2d(1, 1, kernel_size=3)`

    - Conv2d参数必须有input_channel，output_channel，kernel_size；

      可选择bias=False，padding=1，stride=2

    - 默认卷积核权值参数是随机初始化，也可以指定：

      `self.cov2.weight.data = torch.Tensor([1,2,3,4,5,6,7,8,9].view(1,1,3,3).data)`

      1,1,3,3分别为output_channel,input_channel,w,h

- 池化层一般为最大池化和平均池化。图像经过池化后，通道channel不变，大小w,h要变