# 前言

> - 使用LAKKA必须有屏幕，在屏幕上进行WIFI的连接和SSH服务、SAMBA服务的开启
> - 使用手柄可进行相应的选择
> - 是用手柄可以调键位，左侧十字键控制方向，右侧ABXY键控制动作和技能
> - LAKKA不同于Raspbian，需要修改配置文件输出声音
> - 玩游戏主要有两部分，游戏本身ROM和游戏运行所需环境（模拟器）
>   - ROM相当于卡带
>   - 模拟器相当于插卡带的键盘
> - 每个游戏都有对应的模拟器，一个游戏可在模拟器A可以打开，可能在模拟器B打不开



具体教程参见pdf文档



恢复系统默认设置

```python
systemctl stop retroarch

rm .config/retroarch/retroarch.cfg

reboot
```

