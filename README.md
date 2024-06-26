# Lessie 桌面双轮平衡小车

TODO:放机器人照片

要做机器人开发，你除了写控制算法程序之外，还需要学多少技能？硬件设备选型和购买、设备连接和集成、连接线和连接端子的制作、了解各种硬件接口的通讯、编写硬件驱动代码、3D外壳设计和制作、物理引擎模拟环境搭建。这些工作对大部分算法工程师来说显得过于繁琐，本文将为大家提供一个一站式的方案，你不需要过多操心硬件方面的细节，让你把注意力集中在算法上。你只需要根据指引把包装里的各个模块组装起来，并安装好相应的编程工具(vscode)，与你的树莓派建立ssh连接，你就能在你的电脑上把驱动程序和你的控制代码一起安装到你的树莓派上运行。你的控制程序将通过我们提供的高级程序接口和传感器和电机进行交互，你不需要关心串口、GPIO、PWM等硬件细节，只需要处理线速度、角速度、姿态角等你关心的数据。你可以使用你最熟悉的python进行代码编写，让你把注意力集中在你最擅长的领域。

# 1. 机器人的装配
十分建议您在看完这分说明再开始安装机器人。这是一个双轮平衡小车机器人，非常适合入门机器人开发。整体结构完整紧凑，比起市面上的一些半成品的机器人（可以到某东上搜一下）来说。。。更好看？当然不止是这样，训练机器人的时候，你的机器人可能会一下从地板上狂飙出去。有了紧凑的外壳，你不用担心电线脱落，或者电子元器件被摔坏。所有内部线路都已经互相连接好，你只要把接口和芯片的排针连上，最大限度减少误接的风险。你绝对不想你接通电路的那一瞬间看到你的电路板冒出一团白烟。
TODO:【放一张接线图】
所有元器件都有专属的固定卡口，无论你的机器人怎么翻滚他们都不会掉出来。
TODO:【放卡口照片】

完整的安装指引在这里：TODO:【放安装指引链接】

# 2. 点亮机器人
现在我们假设你已经把机器人组装起来了，你可以把电源接通，经过焚香祷告之后，用你的发财小手打开开关，如果一切顺利，你会看到几个地方的led灯亮起来了：

 1. 你的树莓派，
 2. 把你的机器人反转过来，在两个有刷减速电机之间，那是陀螺仪atk_ms901m模块以及两个电机的11排线霍尔编码器。
 TODO:【放一张机器人点亮图】

什么？你已经把整个机器人都封住了？那你当然看不到这些灯，这就是为什么我们一直强调要先看完说明书再动手！

这个时候，我们可以尝试连接你的树莓派。
TODO:【放树莓派ssh设置指引链接】


# 3. 开发你的控制算法

## 3.1. 安装驱动程序及python依赖
 1. 克隆代码库：`git clone https://github.com/marlinlm/lessie.git`,
 2. 进入lessie目录：`cd lessie`,
 3. (a) 本地编译并上传到树莓派：`make upload pi@my-raspi-ip`，这个命令会利用bazel这个构建工具构建驱动程序并上传到`pi@my-raspi-ip`，这是树莓派的ssh用户名和地址。建议可以直接把树莓派的ssh访问方式写进~/.ssh/config里边，这样就可以直接把配置名作为upload的参数。例如：`make upload my-raspi-conf`。这里`my-raspi-conf`就是~/.ssh/config里的一个指向树莓派的配置名。
 3. (b) 直接在树莓派上编译的话：`make build`
 4. 此时ssh连接到树莓派，可以看到有一个`~/lessie`目录。可以看到`~/lessie`内生成了bazel-bin等几个目录，驱动程序就构建在bazel-bin里边。我们需要运行这个程序：`sudo ~/lessie/bazel-bin/spines/lessie_spine`。
 5. lessie_spine启动之后，你可以开启另一个窗口连接树莓派并运行控制程序，示例控制程序放在`~/lessie/lessie_balancer`目录下，大家可以先在树莓派内安装venv：`sudo apt install python3.8-venv`，然后创建一个虚拟环境：`python3 -m venv ~/venvs/lessie`，本文示例的虚拟环境创建在`~/venvs/lessie`目录下，可以通过命令`sudo ~/venvs/lessie/bin/python3 ~/lessie/lessie_balancer/main.py`启动示例控制程序。如果遇到缺乏依赖包的提示，可以通过`sudo ~/venvs/lessie/bin/python3 -m pip install 包名`来安装依赖包。

请注意，由于示例的控制程序只是非常简单地通过陀螺仪传感器的pitch值控制电机的输出，因此一定要小心在机器人pitch角度太大（倒地）的情况下飞出去的情况。机器人驱动中设置了pitch角超过60度自动把电机输出归零。所以倒下后机器人的轮子是不会动的。

# 4. 模拟环境
TODO: 还没搞好模拟环境。敬请期待。

# 5. 更进一步
 - 你也想要一个？欢迎到【放下单地址】下单购买机器人套件。
 - 如果在开发过程中遇到任何问题，欢迎到github上提issue或者直接甩pr。【放github项目地址】
 - 商务合作，如果您有什么商业化提案，欢迎发邮件到406043808@qq.com与我取得联系。

# 6. 补充说明
 1. 本项目底层代码基于开源项目[upkie](https://github.com/upkie) ，不过upkie项目绑定的机电系统死贵一般人买不起。这个项目修改了upkie中的硬件绑定，使用了更便宜的国产系统，缩小了机器人的尺寸，以便更小动力的电机能够驱动，这样可以帮助算法工程师以最低成本进入机器人控制算法开发领域，本项目除了3D模型之外所有代码都开源。
 2. 如果本人能坚持下去，将会继续推出更多低门槛的平衡类的机器人。
 3. 估计你已经看出来了，没错，这个机器人的外壳全部通过3D打印制作，非常的Geek。采用的是某竹P1P系列3D打印机，采用的是PETG或者PLA材料，效果就是两个字：抗摔。
 4. 这个项目所有电子元器件来自某东，大家有需要可自行购买同型号进行替换。采购价格也一并附上，大家如果有更好或者更便宜的产品欢迎联系我，如果采纳，机器人的售价也会相应降低。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/2678dd9471a44f2a8d76f3e340af3671.png)