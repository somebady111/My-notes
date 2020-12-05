# UE4之FPS游戏制作

[TOC]



## Student Html

https://docs.unrealengine.com/zh-CN/Engine/QuickStart/index.html

```reStructuredText
Lumen



Lumen  是一套全动态全局光照解决方案，能够对场景和光照变化做出实时反应，且无需专门的光线追踪硬件。该系统能在宏大而精细的场景中渲染间接镜面反射和可以无限反弹的漫反射；小到毫米级、大到千米级，Lumen都能游刃有余。美术师和设计师们可以使用Lumen创建出更动态的场景，例如改变白天的日照角度，打开手电或在天花板上开个洞，系统会根据情况调整间接光照。Lumen的出现将为美术师省下大量的时间，大家无需因为在虚幻编辑器中移动了光源再等待光照贴图烘焙完成，也无需再编辑光照贴图UV。同时光照效果将和在主机上运行游戏时保持完全一致。

Nanite

Nanite  虚拟微多边形几何体可以让美术师们创建出人眼所能看到的一切几何体细节。Nanite虚拟几何体的出现意味着由数以亿计的多边形组成的影视级美术作品可以被直接导入虚幻引擎——无论是来自Zbrush的雕塑还是用摄影测量法扫描的CAD数据。Nanite几何体可以被实时流送和缩放，因此无需再考虑多边形数量预算、多边形内存预算或绘制次数预算了；也不用再将细节烘焙到法线贴图或手动编辑LOD，画面质量不会再有丝毫损失。
```



## 1.一个工程准备的迁移文件工作

```reStructuredText
1.迁移目标地图
a.打开目标地图文件(686MB)，目标为清理目标文件不需要的配置（初学者内容），将所需地图迁移到新的文件夹
b.新建一个工程，选择blueprint-No starter content，选择最小配置的工程配置，为工程起名
c.打开工程，选择所要迁移level右键-asset actions-migrate，选择ok，选择所要迁移的工程（这里是新建工程）的content文件夹下，然后打开新建工程查看所迁移的内容，此时的工程大小为93.2MB
d.迁移所需的资源。方法同上。选中任意一个动画序列（animation sequence），按住shift，选中第一个，选择所有相关动画以及所依赖的文件，进行迁移。
```

## 2.创建蓝图、制造可交互的对象

```reStructuredText
titile：创建一个行为函数
step1：设置键位，命名移动函数
step2：进入蓝图类写函数体，Event Graph->右键，寻找step1命名的函数，对动作方向、距离进行编辑。
step3：编译，实际测试。

title：行为函数的数值类型转换(actator数值类型和与vecotor数值类型)
step1:创建rotator函数体返回值为rotator类型
step2：使用get forward vector函数进行数值转换
step3：确认转动方向

title：对人物进行Rotator
step1:get controller yaw input
step2:get controller pitch input
step3:get controller roll input

title：以转向方向代替世界蓝图方向
step1:get Control Rotation
step2：转换Rotator类型数值
step3：代替世界坐标轴方向
step4：任务侧向移动，在step1的基础上进行add controller（Yaw即z轴）旋转90°，结合combineRotator函数，在进行转换数值。
math2：
step1：获得人物朝向get control rotation
step2：Get Right Actor 获得朝向角度的右转方向

title：人物进行pitch方向旋转时的转动问题
step1：进入蓝图
step2：对左上角的FPPshotter（self）进行属性use controller rotation pitch为True 
question：相机、人物结合
step1：增加自己的一个相机，在add component中search camera
step2：在viewpoint中调整相机位置
details：
1.进行上下看时需注意朝向的规定正方向问题，一个设置为1，一个设置为-1

title：增加挂点
step1：迁移资源
step2：增加挂点，要以一个动作为基础
step3：打开动作，切换到skeleton Tree 骨骼树
step4：寻找挂点需要挂载的地方，右键选择add socket，起名
step5：挂点预览，在挂点的地方右键，add preview asset
step6:选中挂点，进行weapon旋转
step7：保存，选中骨骼，save

title：增加weapon
step1：在蓝图中add component search 'skeletal Mesh' - 起名
step2：选中weapon骨骼模型在他的detail中找mesh-选择武器
step3：选中weapon骨骼模型，在detail找sockets选择挂点
step4：使用挂点：将weapon骨骼模型移动到人物mesh下
step5：调整weaponmash的location和rotation为0
title：动态调整模型
step1：在世界大纲中选中weapon mash名称进行小窗口调整
step2：拖出蓝图，实时调整视角

title：weapon在行走时的动作->动画混合空间
step1：create blueprint and say name:animation - Blend space 1D->select skeletal mesh
step2:拖动右下角的动画节点，制作混合动画
step3：时间设定，根据模型的运动时间（detail中的最大运动属性），设置动画混合时间
step4：使用混合空间：打开动画蓝图
step5：anim graph中用制作的混合动画代替原先的动画
step6：使用右下角寻找制作的混合动画代替原有动画

title：动态获得人物速度
step1：获取动画蓝图的所有者，获得所控制的角色Try get pawn owner
step2：获取目标的速度：Get velocity（此处的速度为一个向量，所以下一步要获取向量的长度）
step3：获取目标的行动向量长度：vectorlength
```

```reStructuredText
相关函数:
1.Add Movement Input 移动函数
2.make rotator 返回值rotator类型
3.get forward vector rotator和actor的转换
4.Add Controller 转向控制函数
5.Get Control Rotation 获得人物朝向函数,返回类型Rotator，可使用转换函数来转换类型
6.combine Rotator 相加函数
7.get right actor 朝向的右转方向
8.get controller yaw input 进行z轴旋转
9.get controller pitch input 进行y轴旋转
10.Try Get Pown Owner 获取所控制的角色，返回值为Pown
11.Get velocity 获取角色速度
```

## 3.加入开枪动画（动画蒙太奇），time is 12.1 8：13

```reStructuredText
titile:动画蒙太奇（加入开枪动画）
step1：进入blueprints，新建蓝图animation->animation montage
step2：在右下角asset Browser中添加开枪动画到中间位置的montage
step3：在项目设置中进行设计动作按键设置action mappings（action是两个动作的执行，存在两个执行按钮：按下和松开）
step4：播放动画蒙太奇函数：play anim montage
step5：建立函数：将设计逻辑和停止设计的逻辑写入到函数体中
```

```reStructuredText
相关函数：
1.play anim montage 播放蒙太奇动画
2.stop anim montage 停止蒙太奇动画
```

## 4.加入AI机器人，并向人物进行自动追踪、追踪方式、Robot响应射击。It's time 12.2 7：42

```reStructuredText
title:设置机器人停止距离
step1：在RobotControl蓝图中修改AI Move To函数中的acceptance radius参数范围

title：设置机器人继续追踪
step1：当AI Move To成功条件输出下，调用延迟函数Delay，继续追踪人物目标

title：设置机器人动作：Blend Space 1D
step1：建立新的蓝图类型：Blueprints->Animation->Blend Space 1D->起名（这里是RobotMove_BS）
step2：左上角设置Axis Settings中的Horizontal Axis属性：Name、M A Value
step3：右下角拖动动作进入中间区域制作动作：Idle、Run_Fwd
step4：更改动画效果：进入RobotShooter_AnimBP蓝图中（预先第一步设置的机器人动画），将RobotMove_BS的动画从右下角加入蓝图，连接执行按钮
step5：获取人物速度（参考第二节）连接RobotMove_BS
step6：在UE4-4.23版本中解决多线程不安全的问题（直接获取角色速度），解决方案为：1.创建speed变量为float类型2.将获取角色速度的函数体写入Evenet Graph中将获取速度向量的值付给speed

title：角色射击（在此之前的鼠标左键按钮事件只做了播放动画效果，并无实际的射击），假设weapon每个0.2s射击一发子弹，从而射击整个连续射击的效果
step1：在蓝图FPPShouter蓝图中创建函数ShootOnce
step2：调用函数ShootOnce，在函数StartFire函数调用函数ShootOnce：1.使用Sequence（按住s点鼠标左键）进行开火动画逻辑的隔离2.使用sequence函数选择执行函数体。startfire连接sequence，其中，Then0：先执行事件1 Then1：再执行事件二 3.重复调用ShootOnce：先调用函数ShootOnce，再使用函数Set Timer By Function Name（一段时间后执行函数即填写的函数名），设置Time属性值以及looping（是否循环）
step3：编写ShootOnce函数体（在StartFire中只是调用，现在是正式编辑）：函数Line Trace ByChannel（直线追踪）
step4：确认起点：连接LTBC,获取相机，then找到中间位置（函数GetWorldLocation），then连接begin。
step5：确认终点：获取人物朝向（GetControlRotation），then转换值类型（Rotator->vector）函数（GetForwardVector），计算终点距离：GFV获取单位向量或使用Normalize转换为单位向量。终点，方向*射程，实现方式：函数（vector*float）。填写射程（float数值）
step6：起点：相机的位置向量是世界坐标的向量，而，终点：射程*vector（朝向）。并不是正确的射击方向，需要将起点和终点的向量相加得到的向量（vector+vector），也就是说：将原本人物朝向*射距的向量平移到以视野相机为起点的方向，获取射击的向量终点
Q：至此，设计了开始射击，但是停止射击还需要编写
step7：停止射击：停止循环调用ShootOnce。在EndFire中添加sequence，在Then1后添加停止调用函数（ClearTimerByFunctionName），填写要停止的函数名称（这里是ShootOnce）
Q：这里实际换不能射中Robot
step8：射中机器人：Robot无响应的原因是对射击的响应忽略，在Robotshooter中进行construction script脚本进行编写
step9：添加函数（Set Collision Response to Channel),设置channel类型为visibility，New Response响应为Block（穿过物体之后再不能穿过其他物体）。至此，Robot响应射击的完毕。响应1：Mash（模型）响应 响应2：CapsuleComponent（胶囊体）CC响应是一个实体并没有细节的区分二M响应则细致区分
（未解决问题：胶囊体和Robot存在一起，看似射中了Robot其实是射击的胶囊体）
```

```reStructuredText
函数说明：
1.LTBC（Line Trace ByChannel）：设置一个起点，设置一个终点，起点和终点连线，当遇到一个物体时，判定为击中True，否则为False。参数：1.Trace Channel：物体类别2.actors to ignore可忽略的元素组3.Draw Debug Type调试（调试射线）4.起点（相机的中间位置，而非枪口位置）和终点（在View Point中）

```

```reStructuredText
小技巧：
1.ALT+鼠标左键：取消蓝图中的连线

```



## 5.让机器人受伤 time：12.3 PM21:25

```reStructuredText
Title1:让机器人受伤
logic:计算受伤函数（Take Damage）,创建变量血量（health），给函数创建变量Damage（受到的伤害），剩余生命值set（set设定的health值） = health（获取get得到health值） - 受到的伤害（Damage）；将剩余的生命值赋值给health（set）
step1：打开robotshotter蓝图，创建health变量，同时创建函数Take Damage（处理受到伤害逻辑），创建函数中的参数Damage。
step2：set一个health值，使用logic中的逻辑进行伤害计算，运用到的函数-（float）
step3：使用编写的Take Damage函数。
step4：在FPPshotter蓝图中Shoot Once函数中，LineTranceByChannel蓝图节点后判断返回值是否为True，如果为True（这里使用branch函数），则表示射击命中；判断射中物体是否为Robot（类型判断），LTBC的输出Outhit使用BreakHitResult函数（击中结果展示），判断HitActor是否为机器人：调用函数cast to robotshotter转换hitactor类型为robotshotter类型进行判断 
执行Take Damage函数；如果未命中False，则什么也不做。

# Day:12.5 time: 22:03 
Title2:打印伤害
step1:切换到robotshooter蓝图中，在TakeDamage函数里，SET后添加蓝图节点print string（作用：打印字符串，测试用）
step2：转换Damage数值类型，转换函数（ToString）转换为字符串类型，然后连接print string节点，编译、测试。
说明：测试使用的的是Damage，也可以替换为赋值的Health，显示剩余生命值

Title3：第二种选择方法（类选择法）
说明：在Title1中step4的判断也可以如此方法
step1：在FPPShotter蓝图中ShootOnce函数的HitActor判别如下
step2：获取类（getclass函数），如果类的类别为robotshooter（==查找Equal boolean函数）
step3：如果返回值为True则，使用Branch函数执行。 
step4：返回值为True执行TakeDamage函数。

Title4：机器人死亡
前提了解：
1.有限状态机FSM,Finite State Machine,表示有限个状态以及在这些状态之间转移和动作的数学模型
2.动画状态机
step1：在RobotShooter_AnimBP蓝图中添加死亡状态动画
step2：添加动画LocoMotion（Add New State Machine函数），连接最终状态，编辑新建的动画
step3：在开始的Empty后添加Addstate，存活Alive 死亡Dead
step4：在Alive状态中编辑存活状态，将原先编写的函数逻辑复制过去；编写Dead状态动画
step5：编写中间转换状态条件：1.获取robotshooter 2.不是获取血量（因为觉得不同函数之间调用变量是不好的。。。），而是在蓝图Robotshooter中添加一个函数（判断对象是否血量为0） 3.判断血量为0转换Dead状态
step6：在step5中1的方法：先建一个函数GetShooter。引入函数TryGetPawnOwner并转换为Robotshooter类型
step7：给Get Shooter添加输出值（返回值），返回类型为robotshooter类型
step8：设置GetShooter函数为纯函数（将属性中的pure属性为True）
step9：在转换函数中调用编写的GetShooter函数，通过getShooter函数获取状态，在robotshooter函数中添加一个函数IsDead判断health变量是否<=0，给IsDead函数添加返回值dead。
step10：将IsDead函数设为纯函数
Q：1.这里同样会出现一个线程不安全调用的问题 2.死亡动画循环 3.死亡后自动寻路逻辑依然执行
R：2.Dead死亡状态动画设置中关闭loop animation 解决。3.调用一个函数，在死亡后不再受控制
M：3的执行方案：在Take Damage函数中判断health。调用IsDead函数，使用Branch来判断死亡，然后使用函数（Detach from controller pending destroy）来脱离控制。
Then：Robot就死透了！
```

```reStructuredText
说明：
step4说明：判断返回值为True或False，使用branch函数（按住B+鼠标左键）
```

