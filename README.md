

深 圳 大 学 实 验 报 告


      课程名称：¬            计算机网络                       

      实验项目名称：        路由器实验                       
               
学院：             计算机与软件学院                   
         
      专业：                软件工程                                   
         
      指导教师：              崔来中                             
            
      报告人：黄皓恒 学号：2021150109 班级： 软件工程01                 
       
      实验时间：            2023/5/10                                             

      实验报告提交时间：     2023/5/16                           

教务部制
实验目的与要求：

    实验目的及要求：
•	了解华为Quidway AR28系列路由器的构成、工作原理、基本配置；
•	学习使用路由器进行初步网络配置和网络管理，并根据需要进行简单网络的规划和设计。

实验环境：
•	Quidway AR2811路由器2台
•	Quidway S3900交换机1台
•	PC机4台
•	Console线缆1条（用于配置路由器与交换机）
•	双绞线若干 
方法、步骤：

步骤1. 配置 VLAN；
步骤2. 连接路由器；
步骤3. 登录并命名路由器 A；
步骤4. 配置路由器 A WAN 口；
步骤5. 配置路由器 A LAN 口和路由表；
步骤6. 登录并命令路由器 B；
步骤7. 配置路由器 B WAN 口；
步骤8. 配置路由器 B LAN 口和路由表；
步骤9. 检测配置是否成功


实验过程及内容：
步骤1. 配置 VLAN
首先将四台主机分别连接交换机的四个接口，pc1所用交换机的接口为 E 0/0/2，pc2 为E 0/0/4，pc3为 E 0/0/6，pc为 E 0/0/8。如下图1.1所示：
 
图1.1 接口连接对应
然后我们逐步配置每个主机的VLAN状态，这里我们选择用 pc1来控制交换机，打开超级终端进行操作，按下图1.2进行配置：
 
图1.2 配置VLAN
首先进入接口E 0/0/2，如图1.3
 
图1.3进入接口E 0/0/2
设置接口状态为Access，如图1.4
 
图1.4 设置接口状态
将VLAN2视图加入到E 0/0/2接口中，如图1.5
 
图1.5 连接E 0/0/2接口
VLAN2配置E0/0/4和VLAN 3配置E 0/0/6、E 0/0/8的操作同上，这里不再放过多冗杂的图展示。
执行完以上配置操作后，我们可以通过interface Ethernet 0/0/(2,4,6,8)来查看刚刚配置的对应接口的信息，如图1.6、1.7、1.8、1.9所示：
 
图1.6 Ethernet 0/0/2接口信息
 
图1.7 Ethernet 0/0/4接口信息
 
图1.8 Ethernet 0/0/6接口信息
 
图1.9 Ethernet 0/0/8接口信息
接下来我们需要为每台pc主机配置ip地址等信息，以pc1的配置为例展示，如图1.10所示：
 
图1.10 ip地址配置，选择ipv4
配置完四个主机的ip地址和网关等信息后，我们需要分别测试 VLAN2和VLAN3 是否是配置成功的状态。因此执行ping pc1和pc2、ping pc3和pc4来检查。
ping pc1和pc2的结果如图1.11所示：
 
图1.11 对VLAN2配置的检查
ping pc3和pc4的结果如图1.12所示：
 
图1.12 对VLAN3配置的检查
通过查看以上结果，可以发现pc1、2之间ping通了，pc3、4之间也ping通了，说明VLAN2和VLAN3都配置成功了。
而pc1和pc3是无法ping通的，因为他们属于不同类型的VLAN接口，如图1.13所示：
 
图1.13 pc1、pc3之间无法ping通


配置完VLAN后，接下来我们进入配置路由器的操作。
步骤2：连接路由器
    首先我们需要按照指导图2.1所示，将交换机的两个接口分别与两台路由器的 LAN 口相连。可以看到接口为GE 0/0/1分别和E 0/0/1与E 0/0/3相连：
 
图2.1 路由器连接指导图
    然后我们需要将路由器的两个 GE 口也连在一起。从指导图2.2中可以看出该接口号为GE 0/0/0：
 
图2.2  将路由器的两个 GigabitEthernet 口相连
在第一步配置VLAN时我们知道，由于pc1和pc3不属于同一个VLAN，二者不能相互ping通，因此我们接下来还需要配置路由器，以配置E 0/0/1 接口加入 vlan2为例：
进入Ethernet 0/0/1接口：interface Ethernet 0/0/1 
设置接口类型为access：port link-type access
Quit
Vlan2
将VLAN2加入到接口中：port Ethernet 0/0/1
quit
然后重复上面相似的操作，同样地将E 0/0/3 接口也加入 vlan3。
步骤3：登录路由器
完成后，我们需要通过 Console 口连接并登录路由器 A，然后清除原有配置，因为路由器可能被别人使用过。
清除原有配置：Reset saved-configuration
重启路由器：Reboot
完成上述操作后就可以登录上路由器了，如图3.1所示：
 
图3.1 登录路由器
然后我们输入命令<Quidway> system-view进入系统视图
同时修改路由器名字：[Quidway] sysname RouterA
步骤4：配置路由器A的WAN口
首先输入命令display ip routing-table查看路由器A的路由表信息，如图4.1所示：
 
图4.1 A的路由表信息
接下来根据实验指导图中的网络结构示意图进行WAN口配置，如图4.2：
 
图4.2 网络结构图示例
可以看到WAN口的示例ip地址为10.1.0.2 24，我们按照指导地址进行配置即可。输入以下命令进行配置操作：
进入 WAN 口视图：interface GigabitEthernet 0/0/0
设置 IP 地址：ip address 10.1.0.2 24
开启当前接口：undo shutdow

步骤5：配置路由器 A 以太网口和路由表
进入接口视图： interface GigabitEthernet 0/0/1
设置 IP 地址： ip address 10.1.20.1 24
设置静态路由：ip route-static 10.1.20.0 24 GigabitEthernet0/0/1
              ip route-static 10.1.30.0 24 10.1.0.3
执行完以上操作后，我们可以得到路由器A的网络结构如图5.1所示：
 
图5.1：路由器A的网络结构
实现以上所有操作后，对路由器A的配置就完成了，仿照上面对路由器 A 的全部操作，我们还需要对路由器B进行配置。如接下来的步骤6,7,8展示…(简略描述)
步骤6登录并命名路由器 B
通过 Console 口连接并登录路由器 B，清除原有配置，并重启路由器：
Reset saved-configuration
Reboot
进入系统视图：
system-view
修改路由器名字：
sysname RouterB
步骤7：配置路由器 B WAN 口
同样的，首先查看路由表信息，如图7.1所示：
display ip routing-table
 
图7.1 路由器B的路由表信息
进入 WAN 接口视图
interface GigabitEthernet 0/0/0
为 WAN 口设置 IP 地址。
ip address 10.1.0.3 24
开启当前接口。
undo shutdown

步骤8 配置路由器 B 以太网口和路由表
进入接口视图：
interface GigabitEthernet 0/0/1
设置IP 地址： 
ip address 10.1.30.1 24
设置静态路由：
ip route-static 10.1.30.0 24 GigabitEthernet0/0/1
ip route-static 10.1.20.0 24 10.1.0.2（10.1.0.2 是下一跳路由器的地址）

对比 RouterA：将 10.1.20.0 换成了10.1.30.0，10.1.30.0 24 换成了10.1.20.0
24,10.1.0.3 换成 10.1.0.2
步骤9检测配置是否成功
当路由器AB按照以上步骤配置完成后，四台主机pc1234实际上都是完全互通的。为了验证这一结论，对这四个主机，不同VLAN下进行ping操作进行验证：
pc1与pc3之间互相ping，结果如图9.1、9.2所示：
 
图9.1 pc1ping pc3，成功ping通
 
图9.2 pc3 ping pc1，成功ping通
至此，我们完成了路由器的所有相关操作。
实验分析：


深圳大学学生实验报告用纸

实验结论：














指导教师批阅意见：










成绩评定：






                                                 指导教师签字：
                                                    年    月    日
备注：
注：1、报告内的项目或内容设置，可根据实际情况加以调整和补充。
    2、教师批改学生实验报告时间应在学生提交实验报告时间后10日内。
