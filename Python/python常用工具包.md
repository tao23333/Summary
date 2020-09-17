# random

> import random

## 常用函数

### random()

- 生成一个范围为[0,1)的随机浮点数

### randint(a,b)

- 生成范围为[a,b]的随机整数

### uniform(a,b)

- 生成范围为[a,b]的随机浮点数

### randrange([start],stop,[step])

- 从指定[start,stop)范围内，按step步长递增获得的集合中，获取一个随机数
- random.randrange(10, 30, 2)，结果相当于从[10, 12, 14, 16, ... 26, 28]序列中获取一个随机数

### choice(sequence)

- 从序列中获取一个随机元素
- sequence在python不是一种特定的类型，而是泛指一系列的类型。list, tuple, string都属于sequence

### shuffle(x)

- 用于将列表x中的元素打乱

### sample(sequence,k)

- 从序列中生成指定数量的随机字符，并将其组装成列表返回



