# 回归问题

## 单个神经元

``` py
import torch

# 准备数据
x_data = torch.Tensor([[1.0], [2.0], [3.0]])
y_data = torch.Tensor([[2.0], [4.0], [6.0]])

# 构建模型
class LinearModel(torch.nn.Module):
    def __init__(self):
        super(LinearModel, self).__init__()
        # 单个神经元，输入维度和输出维度均为1
        self.linear = torch.nn.Linear(1, 1)
		
		# 前向传播计算y_pred
    def forward(self, x):
        y_pred = self.linear(x)
        return y_pred

# 构建模型示例
model = LinearModel()

# 构建损失函数和优化器，平均平方损失函数(参数表示不进行平均)
criterion = torch.nn.MSELoss(size_average=False)
# 参数表示将来会更新模型中所有的权重，学习率为0.01
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

# 训练模型，周期为100次
for epoch in range(100):
	
	# 正向传播过程
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print(epoch, loss.item())

	# 梯度清零
    optimizer.zero_grad()
    
    # 反向传播
    loss.backward()
    
    # 更新权重
    optimizer.step()

print('w=', model.linear.weight.item())
print('b=', model.linear.bias.item())

x_test = torch.Tensor([[4.0]])
y_test = model(x_test)
print('y_pred=', y_test.data)
print('y_pred=', y_test.item())
```



# 分类问题

## 二分类

### 输入为1个特征值

```python
import torch
import torch.nn.functional as F
import numpy as np
import matplotlib.pyplot as plt

x_data = torch.Tensor([[1.0], [2.0], [3.0]])
y_data = torch.Tensor([[0.0], [0.0], [1.0]])

class LogisticRegressionModel(torch.nn.Module):
    def __init__(self):
        super(LogisticRegressionModel,self).__init__()
        self.linear = torch.nn.Linear(1, 1)

    def forward(self, x):
        y_pred = F.sigmoid(self.linear(x))
        return y_pred

model = LogisticRegressionModel()

criterion = torch.nn.BCELoss(size_average=False)
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

for epoch in range(10000):
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print(epoch, loss.item())

    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

print('w=', model.linear.weight.data)
print('b=', model.linear.bias.data)

# x_test = torch.tensor([[1.0], [2.0], [3.0], [10.0]])
# y_test = model(x_test)
# print('predict=', y_test.data)

x_test = np.linspace(0, 10, 200)
x_test = torch.Tensor(x_test).view((200, 1))
y_test = model(x_test)
y = y_test.data.numpy()
plt.plot(x_test, y)
plt.grid()
plt.show()
```



### 输入为多个特征值

> 使用糖尿病人的数据集

- batch gradient descent版本

```python
import torch
import numpy as np

np.set_printoptions(threshold = np.inf)   #数组过大时，输出不用省略号显示
xy = np.loadtxt(r'C:\Users\Administrator\Desktop\diabetes\data\diabetes.csv', delimiter=',', dtype=np.float32)
#print(xy)
x_data = torch.Tensor(xy[:, :-1])
y_data = torch.Tensor(xy[:, [-1]])
k = 1
class LogisticRegressionModel(torch.nn.Module):
    def __init__(self):
        super(LogisticRegressionModel,self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()
    def forward(self, x):
        # print('X1维度：', x.data.shape)
        # print('Z1维度：', self.linear1(x).data.shape)
        y1_pred = self.sigmoid(self.linear1(x))
        y2_pred = self.sigmoid(self.linear2(y1_pred))
        y_pred = self.sigmoid(self.linear3(y2_pred))
        return y_pred

model = LogisticRegressionModel()
#
criterion = torch.nn.BCELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
#
for epoch in range(100):
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print(epoch, loss.item())

    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

# print('W1维度：', model.linear1.weight.data.shape)
print('w1=', model.linear1.weight.data)
print('b1=', model.linear1.bias.data)
print(model.linear1.bias.data.shape)
print('w2=', model.linear2.weight.data)
print('b2=', model.linear2.bias.data)
```



- mini-batch gradient descent版本

``` python
import torch
import numpy as np
from torch.utils.data import Dataset
from torch.utils.data import DataLoader
import matplotlib.pyplot as plt

# 自定义数据集类，继承Dataset抽象类,实现以下3个方法
class DiabetesDataset(Dataset):
    # 用来载入数据
    def __init__(self, filePath):
        self.xy = np.loadtxt(filePath, delimiter=',', dtype=np.float32)
        self.x_data = torch.from_numpy(self.xy[:, :-1])
        self.y_data = torch.from_numpy(self.xy[:, [-1]])
       # print(self.xy.shape)  (768, 9)

    # dataset[index]就是调用该方法
    def __getitem__(self, index):
        return  self.x_data[index], self.y_data[index]

    # 返回数据集的长度
    def __len__(self):
        return self.xy.shape[0]

# 设计的模型类都必须继承torch.nn.Module类，该类是所有神经网络类的基类
# 里面有很多方法可供我们使用
# pytorch中只要类中的方法所做的操作要构建计算图，则该类必须继承torch.nn.Module
class LogisticRegression(torch.nn.Module):

    # 在该方法进行模型的搭建
    def __init__(self):
        super(LogisticRegression, self).__init__()
        # Linear对象包含权重参数：weight bias
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()

    # 相当于方法的重写，进行前向传播
    def forward(self, x):
        y1 = self.sigmoid(self.linear1(x))
        y2 = self.sigmoid(self.linear2(y1))
        y_pred = self.sigmoid(self.linear3(y2))
        return y_pred

# 准备数据
dataset = DiabetesDataset(r'C:\Users\Administrator\Desktop\diabetes\data\diabetes.csv')
#print(dataset[0])

# DataLoader主要用来进行Shuffle和数据的批次分割
train_loader = DataLoader(dataset=dataset, batch_size=32, shuffle=True)
#print(train_loader)


# 建立模型
model = LogisticRegression()

# 损失函数和优化器
criterion = torch.nn.BCELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.02)

loss_list = []

# 训练过程

for epoch in range(1000):
    i = 0
    total_loss = 0
    # print('第'+str(epoch)+'轮')
    for data in train_loader:
        batch_input, batch_label = data
        # print(batch_input.shape)
        # print(batch_label.shape)
        y_pred = model(batch_input)    # 调用的上述模型类从Module中继承的__call__方法，该方法有调用重写的forward方法
        loss = criterion(y_pred, batch_label)  # 求损失值
        total_loss += loss.item()
        i += 1
        optimizer.zero_grad()  # 梯度清零
        loss.backward()  # 反向传播
        optimizer.step()  # 更新梯度
    loss_list.append(total_loss/i)
    print('loss='+str(total_loss/i))

plt.plot(list(range(len(loss_list))), loss_list)
plt.show()
```



## 多分类

- example1：坐标象限分类

```python
import numpy as np
import torch
import matplotlib.pyplot as plt

xy = np.loadtxt(r'C:\Users\Administrator\Desktop\diabetes\data\coordinate.txt', dtype=np.float32, delimiter=',')
x_data = torch.Tensor(xy[:, :-1])

# 标签必须为n维向量，使用ravel()进行扁平化
y_data = torch.LongTensor(xy[:, [-1]].ravel())

class MoreClassify(torch.nn.Module):
    def __init__(self):
        super(MoreClassify, self).__init__()
        self.linear1 = torch.nn.Linear(2, 50)
        self.linear2 = torch.nn.Linear(50, 4)
        self.sigmoid = torch.nn.Sigmoid()

    def forward(self, x):
        y1 = self.sigmoid(self.linear1(x))
        y_pred = self.linear2(y1)
        return y_pred

model = MoreClassify()

# 损失函数使用交叉熵损失 = softmax-log + NLLoss
criterion = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

loss_list = []

for epoch in range(1000):
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print('loss:'+str(loss.item()))
    loss_list.append(loss.item())
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

softmax_func=torch.nn.Softmax(dim=1)
x_test = torch.Tensor([[-1, 10], [10, 10]])
print(softmax_func(model(x_test.data)).data)
```



- example2：使用MNIST数据集进行手写数字识别

``` python
import torch
import matplotlib.pyplot as plt
import torch.nn.functional as F
from torchvision import datasets
from torchvision import transforms
from torch.utils.data import DataLoader
import torch.optim as optim

# 载入数据集(train:60000, test:10000)
batch_size = 64

# 1、将图片维度从W*H*C 转换为 C*W*H，灰度值映射为[0,1]；2、并服从正态分布
transform = transforms.Compose([transforms.ToTensor(),
                                 transforms.Normalize((0.1307, ), (0.3018, ))])
train_dataset = datasets.MNIST(root='../dataset/mnist/', train=True,
                               download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size)

test_dataset = datasets.MNIST(root='../dataset/mnist', train=False,
                              download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=True, batch_size=batch_size)

# for data in train_loader:
#     input_data, labels = data
#     print('in:', input_data, 'label:', labels)
#     print(input_data.shape, labels.shape)
#     break

class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.linear1 = torch.nn.Linear(784, 512)
        self.linear2 = torch.nn.Linear(512, 256)
        self.linear3 = torch.nn.Linear(256, 128)
        self.linear4 = torch.nn.Linear(128, 64)
        self.linear5 = torch.nn.Linear(64, 10)
        self.relu = torch.nn.ReLU()

    def forward(self, x):
        x = x.view(-1, 784)  # 将输入的张量形状(N,1,28,28)变为(N, 784)
        y1 = self.relu(self.linear1(x))
        y2 = self.relu(self.linear2(y1))
        y3 = self.relu(self.linear3(y2))
        y4 = self.relu(self.linear4(y3))
        y_pred =self.linear5(y4)
        return y_pred

model = Net()

# 损失函数和优化器
criterion = torch.nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.5)

def train(epoch):
    total_loss = 0.0
    i = 0
    for data in train_loader:
        input_data, labels = data
        y_pred = model(input_data)
        loss = criterion(y_pred, labels)
        total_loss += loss.item()
        i += 1
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    print('第'+str(epoch)+'轮：loss='+str(total_loss/i))


for epoch in range(100):
    train(epoch)
```



# CNN训练MNIST

```python
import torch
from torchvision import datasets
from torchvision import transforms
from torch.utils.data import DataLoader
import torch.optim as optim
import matplotlib.pyplot as plt
import cv2
import numpy as np

batch_size = 32

# 1、将图片维度从H*W*C 转换为 C*H*W，灰度值映射为[0,1]；2、并服从正态分布
transform = transforms.Compose([transforms.ToTensor(),
                                 transforms.Normalize((0.1307, ), (0.3018, ))])
train_set = datasets.MNIST(root='../dataset/mnist', train=True, download=True,
                           transform=transform)

test_set = datasets.MNIST(root='../dataset/mnist', train=False, download=True,
                          transform=transform)


train_loader = DataLoader(train_set, shuffle=True, batch_size=batch_size, num_workers=2)
test_loader = DataLoader(test_set, shuffle=True, batch_size=batch_size, num_workers=2)

class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.cov1 = torch.nn.Conv2d(1, 10, kernel_size=5)
        self.cov2 = torch.nn.Conv2d(10, 20, kernel_size=5)
        self.pooling = torch.nn.MaxPool2d(2)
        self.fc = torch.nn.Linear(320, 10)
        self.activateF = torch.nn.ReLU()
    def forward(self, x):
        batch_size = x.size(0)
        x = self.pooling(self.activateF(self.cov1(x)))
        x = self.pooling(self.activateF(self.cov2(x)))
        x = x.view(batch_size, -1)
        x = self.fc(x)
        return x

model = Net()
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
model.to(device)

# 损失函数和优化器
criterion = torch.nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.5)

def train(epoch):
    total_loss = 0.0
    i = 0
    for data in train_loader:
        input_data, labels = data
        input_data, labels = input_data.to(device), labels.to(device)
        # if(i == 0):
        #     print(labels)
        y_pred = model(input_data)
        loss = criterion(y_pred, labels)
        total_loss += loss.item()
        i += 1
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    print('第' + str(epoch) + '轮：loss=' + str(total_loss / i))


def test():
    correct = 0.0
    total = 0
    with torch.no_grad():
        for data in test_loader:
            test_input, test_label = data
            # print(test_label)  (32,)
            test_input, test_label = test_input.to(device),test_label.to(device)
            output = model(test_input)
            predicted = output.argmax(dim=1)  #行最大值下标
            total += test_label.size(0)
            correct += (test_label == predicted).sum().item()
    print('Accuracy on test %lf' %(correct / total))

if __name__ == '__main__':
    for epoch in range(100):
        train(epoch)
        test()
    torch.save(model.state_dict(), r'../model_para.pt')
```

