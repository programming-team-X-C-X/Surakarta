# Team 1 Surakarta第一阶段报告
## ——written by rxy

- ### Part 1：Rulemanager
- #### 简介
在这一部分里，我们在Rule_manager.cpp文件中实现了简单逻辑的判断（如non_capture_move,not_piece等）Rule_manager.h文件中实现了本阶段最恶心（没有之一）的旋吃部分。~~由于简单逻辑真的很水~~,所以这一部分我主要介绍我们用来判断是否合法吃子的思路。
- #### 实现思路
我们可以观察到（或者从潘大哥后来给的提示中）棋盘是由若干条轨道组成的，而一个棋子只要确定了运动方向，就可以推导出整个运动轨迹。
由此，我们先通过找规律的手法，将每个子的坐标与他所在的轨道对应起来，具体而言，每一颗子的轨道序号只可能是:
**"move.from.x"**
**"move.from.y"**  
**"board_size-move.from.x-1"**  
**"board_size-move.from.y-1"**  
在此基础上，我们完善了left,right,up,down四个函数，分别对应棋子向各个方向走的情况，并且对move.from.x/move.from.y进行分类讨论，以board_size/2为分界线分别用循环遍历起行走路线（然后就堆了8层结构完全一致的sh*t mountain），然后如果在过程中碰到其他棋子（非终点棋子）就return 0，最后将各函数return值进行加和以得出有无合法吃子路径。
为什么要重点写吃子呢？因为逻辑上来讲写完not_board,not_piece等判别和L**i**gal_capture_move后，剩下的就都可以是Ill**i**gal_capture_move了

- p.s. 听说在.h文件里实现函数是一种极其不规范的行为（~~逃~~）

#### 对于Part 1 遇到的一些问题

首先，我们在第一次写完movereason之后碰到了一个非常烦人的问题:segmentation fault(core dumped).真的很烦人（确信）。后来发现是简单函数判断中move.from和move.to没有卡好范围，导致测试样例中一些超出棋盘的样例进入了数组导致数组越界。

但是解决完上述问题之后，由于我们组使用了address sanityzer来调试seg问题，然后又因为一些不知名原因导致add爆炸了，在运行过程中本应该正常出现的Test1. passed那个滚动显示变成了 deadly signal。但是由于我们当时不知道这个点，以为是原先的movereason不可救药了，于是选择重写

后来手搓GDB，发现原来方案也可行（恼）

- ### Part 2: ~~Easy~~ AI
  - 首先，目前（2024.3.19）的agent_mine只是一个初始版本的ai，采用了我们所认为的贪心算法，而在后续的大作业过程中我们将会对其进行完善。
  
  就目前版本而言，我们采用了人类下棋学习，然后利用贪心实现的方式，将我们从下棋过程中获取的一些经验写入程序中，主要体现为：

  - **1.保命要紧，能不主动出击就不主动出击**
  - **2.如果路径上有能吃的棋子，就一定要吃**
  - **3.在能吃棋子的基础上，尽量保全自己**
  - **4.如果没有符合以上条件的走法，则尽可能让自己走到一个不容易被吃的位置**

而正是目前的4这个点为我们带来了改进的可能（to be continue）
**一个比较重要的问题是得出棋盘位置上的价值权重**

（~~A Little Complain~~）其实本来贪心的算法就是能吃就吃，没有考虑自己活不活，但是发现random_ai采用的正是这个思路。。原来我和random的智力没有区别（

## update at 5.24 by rxy
  距离我写好MCTS的~~Artificial Idiot~~已经过去一周了，不得不说在Surakarta游戏下，我认为mcts的有限次模拟算法是难以比上保命优先且能吃则吃的贪心AI的（不排除是因为苯人太菜的原因）
  MTCS综合胜率维持在46%-52%，效果劣于stage1时写的贪心，文件已上传至stage1文件夹
