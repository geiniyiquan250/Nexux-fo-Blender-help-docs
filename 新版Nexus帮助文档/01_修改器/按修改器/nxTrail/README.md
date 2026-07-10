# nx 拖尾（nxTrail）

当前文档版本：NeXus `2026.0.7011503`

`nx 拖尾（nxTrail）` 用来把粒子的运动路径显示成拖尾。  
它既可以直接在视口里画线，也可以生成 Blender 曲线（Blender Curves）对象来继续渲染和处理。

当前版本插件里，`nx 拖尾（nxTrail）` 可以同时处理多个发射器来源。  
它可以在 `发射器（Emitters）` 列表里同时接入多个 `nx 发射器（nxEmitter）`，每个来源都能单独设置颜色、长度、连接方式、厚度和样条方式。

## 当前界面怎么理解

当前版本里，`nx 拖尾（nxTrail）` 可以先分成两层来看：

- 第一层是对象顶部的 `显示（Display）`
- 第二层是 `发射器（Emitters）` 列表里，每个来源自己的三张页

对象顶部的 `显示（Display）` 决定整套拖尾按哪种方式输出：

- `线条（Lines）`
- `样条（Splines）`

每个来源条目里，再按三张页细调：

- `常规（General）`
- `厚度（Thickness）`
- `样条（Spline）`

如果当前来源条目还没有指定发射器对象，这一条就不会有实际拖尾结果。

## 顶部这项先分清

### 显示（Display）

`显示（Display）` 决定 `nx 拖尾（nxTrail）` 用哪种形式输出。  
当前版本插件默认值是 `线条（Lines）`。

#### 线条（Lines）

`线条（Lines）` 用来直接在视口里显示拖尾线。  
这类线条属于视口预览，不会作为场景里的真实可渲染线对象被输出。
它更适合先观察路径、密度和连接逻辑。

- 优点是直接、轻、改参数后更容易马上看走势
- 限制是它属于视口显示，不会自动给你生成 Blender 曲线（Blender Curves）对象，也不能直接当成最终渲染线来用

如果你当前只是在排查拖尾有没有接对、方向对不对，通常先用 `线条（Lines）` 更省事。  
如果目标是让拖尾真正进入场景并参与渲染，需要切到 `样条（Splines）`。

#### 样条（Splines）

`样条（Splines）` 会为每个来源生成 Blender 曲线（Blender Curves）子对象。  
它更适合做真正的渲染、材质、后续曲线处理和导出。

- 切到 `样条（Splines）` 后，`样条（Spline）` 页里的设置才会更有可见意义
- 如果还停在 `线条（Lines）`，改 `样条（Spline）` 页的大多数项，通常不会带来你期待的曲线变化

如果你现在的目标是出可渲染曲线，先确认这里已经切到 `样条（Splines）`。

## 发射器（Emitters） 列表怎么理解

`发射器（Emitters）` 列表里的每一项，都代表一个拖尾来源。  
每项都可以单独：

- 开关启用
- 指定一个 `发射器（Emitter）`
- 调自己的颜色和长度
- 选自己的连接算法
- 选自己的厚度和样条方式

如果场景里有多个发射器想分别做不同拖尾，通常就会在这里分成多项。

## 常规（General）

### 色彩模式（Color Mode）

`色彩模式（Color Mode）` 决定拖尾整体颜色怎么来。  
当前版本插件默认值是 `标准（Standard）`。

可选项有：

- `标准（Standard）`
- `梯度（Gradient）`

#### 标准（Standard）

`标准（Standard）` 只使用一个固定颜色。  
切到这一项后，会出现 `颜色（Color）`。

##### 颜色（Color）

`颜色（Color）` 控制整条拖尾的固定颜色。

- 适合快速确认拖尾来源
- 颜色不会沿路径发生渐变

#### 梯度（Gradient）

`梯度（Gradient）` 会沿拖尾长度去采样颜色变化。  
它更适合做头尾过渡、冷热变化、长短衰减这类效果。

如果你已经切到 `梯度（Gradient）`，却完全看不到变化，先回头检查 `厚度（Thickness）` 页里的 `无厚度/颜色数据（No Thickness/Color Data）` 是否被开启了。  
开启后，颜色和厚度相关数据都不会继续写出。

### 长度模式（Length Mode）

`长度模式（Length Mode）` 决定拖尾长度按时间算，还是按空间距离算。  
当前版本插件默认值是 `时间（Time）`。

可选项有：

- `时间（Time）`
- `距离（Distance）`

#### 时间（Time）

切到 `时间（Time）` 后，会先看到 `整个场景（Full Scene）`。

##### 整个场景（Full Scene）

`整个场景（Full Scene）` 控制拖尾是否一直保留整个场景时长。  
当前版本插件默认开启。

- 开启后，会尽量保留整段场景里的拖尾历史
- 关闭后，下面才会出现单独的 `长度（Length）`

##### 长度（Length）

`长度（Length）` 只在 `整个场景（Full Scene）` 关闭时出现。  
当前版本插件默认值是 `30.0`。

- 值更高时，会保留更长时间的运动历史
- 值更低时，拖尾会更短，旧轨迹更快被截掉

#### 距离（Distance）

切到 `距离（Distance）` 后，`长度（Length）` 会改成按空间距离理解。  
当前版本插件默认值是 `1.0`。

- 值更高时，拖尾会延伸更远
- 值更低时，拖尾会更短

如果粒子速度变化很大，又想让每条拖尾都尽量保留相近的空间长度，这一项通常比时间更直观。

### 帧采样（Frame Sampling）

`帧采样（Frame Sampling）` 控制每隔多少帧记录一个拖尾点。  
当前版本插件默认值是 `1`。

- 值更高时，记录点更稀，拖尾会更轻，也更容易出现折线感
- 值更低时，记录点更密，拖尾更顺，也更重

如果拖尾点太密、数量太大，可以先从这里提上去一点。

### 变化（Variation）

`变化（Variation）` 控制拖尾长度的随机变化比例。  
当前版本插件默认值是 `0.0`。

- 值更高时，每条拖尾长短会更不一致
- 值更低时，拖尾长度更统一

如果想让一批拖尾不要整整齐齐一样长，可以从这里加一点变化。

### 连接（Connections）

`连接（Connections）` 是 `nx 拖尾（nxTrail）` 里最关键的一组。  
它决定拖尾到底按什么规则把粒子连起来。

当前版本插件默认值是 `无连接（No Connections）`。

可选项有：

- `无连接（No Connections）`
- `直线序列（Straight Sequence）`
- `分段序列（Segmented Sequence）`
- `多序列（Multiple Sequence）`
- `所有点（All Points）`
- `最近索引（Nearest Index）`
- `最近距离（Nearest Distance）`
- `簇（Cluster）`
- `约束（Constraints）`

#### 无连接（No Connections）

`无连接（No Connections）` 会按每个粒子自己的历史轨迹来画拖尾。  
这通常是最稳、最容易理解的起点。

如果你只是想看“每个粒子过去怎么走过来”，先保留这一项。

#### 直线序列（Straight Sequence）

`直线序列（Straight Sequence）` 会按粒子发射顺序，把相邻粒子直接串起来。  
它更像按“发射顺序编号”去拉线。

如果来源粒子本身就是有序排列，这种模式会更容易看到整齐的带状结构。

#### 分段序列（Segmented Sequence）

`分段序列（Segmented Sequence）` 会按顺序连线，但中间会主动留出间隔。  
切到这一项后，会出现：

- `段长度（Segment Length）`
- `间隙长度（Gap Length）`

##### 段长度（Segment Length）

`段长度（Segment Length）` 控制每一小段连续连接多长。  
当前版本插件默认值是 `1`。

- 值更高时，每段会连得更长
- 值更低时，每段更短

##### 间隙长度（Gap Length）

`间隙长度（Gap Length）` 控制每段之间空开多少。  
当前版本插件默认值是 `1`。

- 值更高时，断开的空隙更长
- 值更低时，各段之间更接近连成一片

如果想做断续的虚线带状拖尾，这一组最直接。

#### 多序列（Multiple Sequence）

`多序列（Multiple Sequence）` 会把来源粒子拆成多条序列来生成多条拖尾。  
切到这一项后，会出现：

- `模式（Mode）`
- `序列（Sequences）` 或 `长度（Length）`

##### 模式（Mode）

`模式（Mode）` 有两种：

- `交替（Alternating）`
- `连续（Sequential）`

`交替（Alternating）` 会轮流分配粒子给不同拖尾。  
`连续（Sequential）` 会按连续块去分配。

##### 序列（Sequences）

`序列（Sequences）` 只在 `模式（Mode）` 为 `交替（Alternating）` 时出现。  
当前版本插件默认值是 `1`。

- 值更高时，会拆成更多条交错拖尾
- 值更低时，条数更少

##### 长度（Length）

这里的 `长度（Length）` 只在 `模式（Mode）` 为 `连续（Sequential）` 时出现。  
当前版本插件默认值是 `1`。

- 值更高时，每条连续序列会吃进更多粒子
- 值更低时，每条序列更短

#### 所有点（All Points）

`所有点（All Points）` 会把来源里的点彼此都尝试连起来。  
它很容易一下变得非常密。

- 适合少量点的特殊结构
- 点数一多时，画面会很快变乱，也更重

如果当前结果像蜘蛛网一样密到看不清，先回头看是不是切到了这里。

#### 最近索引（Nearest Index）

`最近索引（Nearest Index）` 会按粒子编号顺序去找相邻连接。  
切到这一项后，会出现：

- `最大连接数（Max Connections）`
- `跳过粒子（Skip Particles）`
- `目标组（Destination Groups）`

##### 最大连接数（Max Connections）

`最大连接数（Max Connections）` 控制每个粒子最多往外连多少条。  
当前版本插件默认值是 `1`。

- 值更高时，连接会更密
- 值更低时，连接更克制

##### 跳过粒子（Skip Particles）

`跳过粒子（Skip Particles）` 控制按一定间隔跳过部分粒子的外连。  
当前版本插件默认值是 `0`。

- 值更高时，整体连线会更稀
- 值更低时，更多粒子会参与外连

#### 最近距离（Nearest Distance）

`最近距离（Nearest Distance）` 会按粒子之间的距离窗口来连线。  
切到这一项后，会出现：

- `最小距离（Min Distance）`
- `最大距离（Max Distance）`
- `最大数量（Max Number）`
- `目标组（Destination Groups）`

##### 最小距离（Min Distance）

`最小距离（Min Distance）` 控制小于多近的粒子不要连。  
当前版本插件默认值是 `0.0`。

- 值更高时，会主动避开过近的连接
- 值更低时，近处粒子也更容易互相连接

##### 最大距离（Max Distance）

`最大距离（Max Distance）` 控制超过多远的粒子不要连。  
当前版本插件默认值是 `0.1`。

- 值更高时，能跨更远去连
- 值更低时，只保留近处连接

##### 最大数量（Max Number）

`最大数量（Max Number）` 控制每个粒子最多保留多少条这类距离连接。  
当前版本插件默认值是 `0`。

- 值为 `0` 时，不额外封顶
- 值更高时，允许更多连接
- 值更低时，更容易控制密度

#### 簇（Cluster）

`簇（Cluster）` 会先按距离把粒子分成小群，再只在每个群里连线。  
切到这一项后，会出现：

- `群聚距离（Cluster Distance）`
- `簇中最小粒子数（Min Particles in Cluster）`
- `目标组（Destination Groups）`

##### 群聚距离（Cluster Distance）

`群聚距离（Cluster Distance）` 控制多近的粒子会被归到同一簇里。  
当前版本插件默认值是 `0.1`。

- 值更高时，簇更容易变大
- 值更低时，更容易拆成很多小簇

##### 簇中最小粒子数（Min Particles in Cluster）

`簇中最小粒子数（Min Particles in Cluster）` 控制一个簇至少要有多少粒子，才值得生成连线。  
当前版本插件默认值是 `2`。

- 值更高时，小簇更容易被忽略
- 值更低时，更小的簇也会开始出线

#### 约束（Constraints）

`约束（Constraints）` 会沿着 `nx 约束（nxConstraints）` 里已经存在的约束关系来画连接。  
切到这一项后，会出现 `约束类型（Constraint Type）`。

如果场景里本来就没有 `nx 约束（nxConstraints）` 关系，这一项通常不会给出你想要的结果。

##### 约束类型（Constraint Type）

`约束类型（Constraint Type）` 控制只看哪一类约束。  
当前版本插件默认值是 `所有类型（All Types）`。

可选项有：

- `所有类型（All Types）`
- `出生（Birth）`
- `距离（Distance）`
- `自定义（Custom）`
- `粘度（Viscosity）`

如果只想看某一类约束结构，先从这里筛。

### 目标组（Destination Groups）

`目标组（Destination Groups）` 只会在这些连接模式里出现：

- `最近索引（Nearest Index）`
- `最近距离（Nearest Distance）`
- `簇（Cluster）`

它决定连接时允许连向哪些组：

- `使用所有组（Use All Groups）`
- `仅同组（Only Same Group）`
- `仅不同组（Only Different Groups）`
- `特定组（Specific Group）`
- `除特定组外的全部（All Except Specific Group）`

如果切到 `特定组（Specific Group）` 或 `除特定组外的全部（All Except Specific Group）`，下面还会出现 `组（Group）`。

#### 组（Group）

`组（Group）` 用来指定上面要筛的那个 `nx 组（nxGroup）`。

如果你已经选了只连特定组，却没指定组对象，这一层筛选就没法按预期工作。

### 冻结模式（Freeze Mode）

`冻结模式（Freeze Mode）` 控制拖尾达到长度上限后，后面怎么处理。  
当前版本插件默认值是 `不冻结（Do Not Freeze）`。

可选项有：

- `不冻结（Do Not Freeze）`
- `冻结粒子（Freeze Particle）`
- `冻结拖尾（Freeze Trail）`

可以先这样理解：

- `不冻结（Do Not Freeze）`：拖尾按正常方式滚动更新
- `冻结粒子（Freeze Particle）`：到上限后，粒子本身也一起停住
- `冻结拖尾（Freeze Trail）`：拖尾停止继续记录，但粒子本身可以继续动

如果想做“拖尾留住，但粒子继续飞走”的效果，通常先看 `冻结拖尾（Freeze Trail）`。

### 冻结运动（Freeze Movement）

`冻结运动（Freeze Movement）` 只在 `冻结模式（Freeze Mode）` 设为 `冻结粒子（Freeze Particle）` 或 `冻结拖尾（Freeze Trail）` 时出现。

- 开启后，冻结时会把运动一起锁住
- 关闭后，冻结时更偏向只锁拖尾这层行为

### 冻结缩放（Freeze Scale）

`冻结缩放（Freeze Scale）` 只在 `冻结模式（Freeze Mode）` 设为 `冻结粒子（Freeze Particle）` 或 `冻结拖尾（Freeze Trail）` 时出现。

- 开启后，冻结时会把缩放变化一起锁住
- 关闭后，缩放更容易继续跟着后续状态变化

## 厚度（Thickness）

### 无厚度/颜色数据（No Thickness/Color Data）

`无厚度/颜色数据（No Thickness/Color Data）` 用来直接关闭这条来源的厚度和颜色附加数据输出。  
当前版本插件默认关闭。

- 开启后，下面的厚度和颜色相关控制都会失去主要作用
- 关闭后，下面这些设置才会继续写到拖尾里

如果你改了梯度、厚度样条、顶点颜色都没反应，先查这一项。

### 厚度模式（Thickness Mode）

`厚度模式（Thickness Mode）` 决定拖尾厚度怎么来。  
当前版本插件默认值是 `不设置厚度（Do Not Set Thickness）`。

可选项有：

- `不设置厚度（Do Not Set Thickness）`
- `从值设置（Set From Value）`
- `使用样条（Use Spline）`
- `使用当前半径（Use Radius (Current)）`
- `使用变量半径（Use Radius (Variable)）`

#### 不设置厚度（Do Not Set Thickness）

`不设置厚度（Do Not Set Thickness）` 不额外写厚度数据。  
更适合只先看路径，不先管粗细变化。

#### 从值设置（Set From Value）

`从值设置（Set From Value）` 会给整条拖尾一个统一厚度。  
切到这一项后，会出现：

- `厚度（Thickness）`
- `厚度变化（Thickness Variation）`

##### 厚度（Thickness）

`厚度（Thickness）` 控制基础拖尾粗细。  
当前版本插件默认值是 `0.01`。

- 值更高时，拖尾更粗
- 值更低时，拖尾更细

##### 厚度变化（Thickness Variation）

`厚度变化（Thickness Variation）` 控制粗细随机波动。  
当前版本插件默认值是 `0.0`。

- 值更高时，每条拖尾粗细更不一致
- 值更低时，整体更统一

#### 使用样条（Use Spline）

`使用样条（Use Spline）` 会按一条厚度曲线，沿拖尾长度去控制粗细变化。  
切到这一项后，会出现：

- 厚度样条编辑
- `样条最大值（Spline Max）`
- `样条时间（Spline Time）`

##### 样条最大值（Spline Max）

`样条最大值（Spline Max）` 控制厚度样条可达到的最大粗细。  
当前版本插件默认值是 `0.01`。

- 值更高时，样条顶部对应的粗细更粗
- 值更低时，整体粗细上限更小

##### 样条时间（Spline Time）

`样条时间（Spline Time）` 控制厚度样条沿拖尾被采样的时间范围。  
当前版本插件默认值是 `30.0`。

- 值更高时，粗细变化会摊到更长一段拖尾上
- 值更低时，粗细变化会更集中地发生

#### 使用当前半径（Use Radius (Current)）

`使用当前半径（Use Radius (Current)）` 会直接拿粒子当前半径来作为拖尾厚度。

- 更适合想让拖尾粗细直接跟着粒子当前尺寸走
- 如果粒子半径本身一直没变化，这种模式看起来会比较稳定

#### 使用变量半径（Use Radius (Variable)）

`使用变量半径（Use Radius (Variable)）` 会把粒子半径变化沿拖尾记录下来。

- 更适合粒子半径本身会在时间里变化的场景
- 如果粒子一直等大，这一项和上面那项看起来差异会更小

### 拖尾颜色模式（Trail Color Mode）

`拖尾颜色模式（Trail Color Mode）` 控制颜色按哪一层写进拖尾。  
当前版本插件默认值是 `粒子颜色（Particle Color）`。

可选项有：

- `粒子颜色（Particle Color）`
- `顶点颜色（Per-Vertex Color）`

#### 粒子颜色（Particle Color）

`粒子颜色（Particle Color）` 会直接使用粒子当前颜色。

#### 顶点颜色（Per-Vertex Color）

`顶点颜色（Per-Vertex Color）` 会把颜色按拖尾顶点去记录。  
如果后续要在曲线上读取更细的颜色变化，这一项通常更合适。

## 样条（Spline）

`样条（Spline）` 这一页主要在 `显示（Display）` 已经设成 `样条（Splines）` 时更值得看。

### 样条类型（Spline Type）

`样条类型（Spline Type）` 决定生成哪种曲线类型。  
当前版本插件默认值是 `线性（Linear）`。

可选项有：

- `线性（Linear）`
- `贝塞尔（Bezier）`
- `B 样条（B-Spline）`

可以先这样抓区别：

- `线性（Linear）`：严格按记录点连，转折更直接
- `贝塞尔（Bezier）`：更圆滑，适合明显需要柔顺曲线的时候
- `B 样条（B-Spline）`：也偏平滑，更适合连续顺滑的曲线感

如果你先想确认路径有没有偏，通常先保留 `线性（Linear）` 更直观。

### 闭合样条（Close Spline）

`闭合样条（Close Spline）` 控制生成出来的曲线是否首尾闭合。  
当前版本插件默认关闭。

- 开启后，首尾会接回去
- 关闭后，保持开口曲线

如果拖尾本来就该是开口路径，通常不要随手开这个。

### 中间点（Intermediate Points）

`中间点（Intermediate Points）` 控制是否在原始拖尾点之间继续补点。  
当前版本插件默认值是 `无（None）`。

可选项有：

- `无（None）`
- `统一（Uniform）`
- `自适应（Adaptive）`

#### 无（None）

`无（None）` 表示直接使用现有拖尾点，不额外插点。

#### 统一（Uniform）

`统一（Uniform）` 会按固定数量往每段里补点。  
切到这一项后，会出现 `数字（Number）`。

##### 数字（Number）

`数字（Number）` 控制每段补多少个中间点。  
当前版本插件默认值是 `0`。

- 值更高时，曲线点更密
- 值更低时，补点更少

#### 自适应（Adaptive）

`自适应（Adaptive）` 会更看转折角度来决定哪里补点。  
切到这一项后，会出现 `角度（Angle）`。

##### 角度（Angle）

`角度（Angle）` 控制达到多大转折时，才更积极地补点。  
当前版本插件默认值是 `15` 度。

- 值更高时，需要更明显拐弯才会重点补点
- 值更低时，较轻的转折也会开始加点

如果曲线拐角位置太硬，可以先用 `自适应（Adaptive）` 再把角度压低一点。

### 使用最大长度（Use Max Length）

`使用最大长度（Use Max Length）` 控制是否给生成曲线额外加一个长度上限。  
当前版本插件默认关闭。

- 开启后，下面会出现 `最大长度（Max Length）`
- 关闭后，不再额外截这层长度

### 最大长度（Max Length）

`最大长度（Max Length）` 只在 `使用最大长度（Use Max Length）` 开启时出现。  
当前版本插件默认值是 `1.0`。

- 值更高时，允许保留更长的曲线
- 值更低时，更早把曲线截短

如果样条输出已经太长、太乱，又不想回头改基础拖尾长度，可以先从这里加一道限制。

## 和其他常见修改器一起看时，最容易卡住的地方

### 和 `nx 发射器（nxEmitter）`

`nx 拖尾（nxTrail）` 读取的就是发射器粒子。  
如果来源发射器没有有效粒子、当前条目没指定对象，或者条目被关掉，这一条就不会有拖尾。

### 和 `nx 约束（nxConstraints）`

如果 `连接（Connections）` 设成 `约束（Constraints）`，结果会直接依赖 `nx 约束（nxConstraints）` 里已经存在的约束关系。  
前面没有约束关系时，这种模式通常不会给出预期结果。

### 和 `nx 缓存（nxCache）`

如果拖尾参数改了却完全不变，先回头看缓存是不是还停在旧结果。  
这类情况非常常见。

### 和 Blender 曲线（Blender Curves）

如果 `显示（Display）` 已经切到 `样条（Splines）`，后面看到的是 Blender 曲线（Blender Curves）对象。  
这时颜色、粗细、材质的最终观感，还会继续受曲线显示和材质读取方式影响。

## 当前版本下最常见的排查顺序

### 为什么完全没有拖尾

通常先检查：

1. `发射器（Emitters）` 列表里是否真的已经指定了来源发射器
2. 这一条来源的 `启用（Enable）` 是否还开着
3. 来源发射器当前是否真的有粒子
4. 缓存是不是还停在旧结果

### 为什么视口里有线，但渲染时没有曲线

这通常先看顶部的 `显示（Display）`。  
如果当前还是 `线条（Lines）`，它本来就偏向视口显示，不会按曲线方式给你后续渲染结果。

### 为什么改了样条页参数没有反应

通常先检查：

1. 顶部 `显示（Display）` 是否已经切到 `样条（Splines）`
2. 当前来源是否真的已经生成曲线子对象
3. 改的是不是当前来源条目的那一页

### 为什么改了梯度或厚度没有变化

通常先检查：

1. `无厚度/颜色数据（No Thickness/Color Data）` 是否被开启
2. `色彩模式（Color Mode）` 是否真的切到了 `梯度（Gradient）`
3. `厚度模式（Thickness Mode）` 是否选到了你想要的那种方式

### 为什么连接一切就变得很乱

通常先检查：

1. `连接（Connections）` 是否切到了 `所有点（All Points）`
2. `最大连接数（Max Connections）`、`最大数量（Max Number）` 是否过高
3. `最大距离（Max Distance）` 或 `群聚距离（Cluster Distance）` 是否设得太大
4. `目标组（Destination Groups）` 是否还在允许跨所有组一起连
