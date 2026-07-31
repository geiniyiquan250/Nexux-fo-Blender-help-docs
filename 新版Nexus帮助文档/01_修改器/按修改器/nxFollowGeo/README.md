# nx 跟随几何体（nxFollowGeo）

当前文档版本：NeXus `2026.0.0`

`nx 跟随几何体（nxFollowGeo）`让粒子贴着网格表面爬行、沿网格边线移动，或捕捉到曲线后沿样条前进。它适合虫群爬墙、粒子沿模型边缘流动、弹道吸附到模型、光点沿路径飞行等效果。

## 一条容易跑通的起步方法

1. 使用 `nx 发射器（nxEmitter）` 产生一批具有初始速度的粒子。
2. 添加 `nx 跟随几何体（nxFollowGeo）`，并把它接入同一批粒子的修改器流程。
3. 在对象（Objects）列表加入目标网格（Mesh）。
4. 保持表面（Surface）模式，把拉力（Pull）提高到能将粒子吸住表面的程度。
5. 粒子滑得过快时，提高摩擦力（Friction）；需要留出悬浮距离时，提高偏移（Offset）。

对象（Objects）列表接受网格（Mesh）、曲线（Curve）和曲线（Curves）对象。网格与曲线（Curves）使用表面或边缘跟随；曲线（Curve）会显示专用的样条跟随设置。

## 已启用（Enabled）和对象（Objects）

已启用（Enabled）控制整个修改器。关闭后，已经被捕捉的粒子会离开目标物体并恢复自身运动，列表和参数会保留。

对象（Objects）列表中的每一项也有单独的已启用（Enabled）开关。关闭某一项可以临时取消该物体的跟随作用，无需从列表删除。切换不同列表项后，下方参数会跟随当前目标物体变化。

## 网格和曲线（Curves）跟随

### 模式（Mode）

模式（Mode）默认使用表面（Surface）。

- 表面（Surface）：粒子分布在面上并顺着表面移动
- 边缘（Edge）：粒子沿网格边线移动

边缘（Edge）模式适合轮廓线、硬表面结构和沿线爬行的效果。表面（Surface）适合覆盖整个模型。

### 连接（Connection）

连接（Connection）决定粒子怎样吸附并保持在表面上。

拉力（Pull）默认 `200%`，在距离（Distance）范围内把粒子拉向表面。提高后，粒子更容易贴紧凹凸表面；降低后，粒子更容易滑离目标。变化（Variation）会给每个粒子加入不同拉力，适合让吸附过程错开。

推力（Push）默认 `100%`，把粒子沿表面法线推开。偏移（Offset）默认 `0`，决定拉力和推力平衡后离表面的基础距离。提高偏移后，粒子会在表面上方悬浮；偏移变化（Variation）可以让悬浮高度产生随机差异。

距离（Distance）默认 `0.1`，决定拉力开始作用的最大距离。提高后，更远的粒子也能被捕捉；降低后，粒子必须更接近表面。摩擦力（Friction）默认 `0%`，提高后粒子会逐渐减速，更像在表面爬行；保持较低值时，粒子会沿表面快速滑过。

视场（FOV）默认 `270°`，决定粒子检测表面的方向范围。提高后，侧面和后方的面也更容易被捕捉；降低后，粒子主要检测前方区域。

边缘（Edge）和平滑（Smoothing）只在边缘（Edge）模式生效。提高边缘后，粒子会更紧贴网格边线；提高平滑后，粒子沿边线转弯时更连贯，减少生硬折转。

### 释放（Release）

释放模式（Release Mode）决定粒子何时离开表面。

- 无（None）：持续跟随表面
- 时间（Time）：到达释放时间（Release Time）后离开

时间（Time）模式下，时间模式（Time Mode）可选粒子寿命（Particle Age）或帧时间（Frame Time）。粒子寿命从每颗粒子出生时开始计时；帧时间按场景时间线计时。释放时间（Release Time）默认 `30` 帧，变化（Variation）会让粒子在不同时间离开，避免整批同时脱离。

## 曲线（Curve）跟随

加入曲线（Curve）对象后，界面切换为连接（Connection）、扩展数据（Extended Data）、释放（Release）和样条数据（Spline Data）四部分。当前插件会自动加入一个偏移（Offset）层，便于直接调整路径宽度。

### 连接（Connection）

方向（Direction）默认向前（Forwards），向后（Backwards）会沿相反方向前进。模式（Mode）默认引导（Guide）：曲线直接引导粒子朝向和路径；力（Force）模式把曲线作为持续作用的距离力，其他运动修改器仍会更明显地参与结果。

激活范围（Activate Range）默认 `2`，决定粒子靠近曲线多远时开始被捕捉。总强度（Strength）默认 `100%`，控制整套曲线跟随的影响量。

吸引强度（Attract Strength）把粒子拉向曲线。引导（Guide）模式默认 `10%`，力（Force）模式默认 `0.5`。提高后更快贴到曲线；变化（Variation）会让不同粒子的捕捉力度不同。吸引衰减（Attract Falloff）提高后，吸引会随距离更快减弱；吸引衰减类型（Attract Falloff Type）可选平直（Flat）、线性（Linear）、二次（Quadratic）和立方（Cubic），默认立方（Cubic）。

跟随强度（Follow Strength）推动粒子沿曲线前进。引导（Guide）模式默认 `40%`，力（Force）模式默认 `1`。提高后，粒子更明显地按曲线方向移动；跟随变化（Variation）可打散速度。跟随衰减（Follow Falloff）和跟随衰减类型（Follow Falloff Type）控制这种前进力随距离如何减弱。

对齐（Align）默认 `0%`，控制粒子运动方向向曲线切线对齐的程度。提高后，粒子路径更贴合曲线方向。实例模型需要真实旋转并朝向移动方向时，继续加入 `nx 自旋（nxSpin）`，使用切向（Tangential）层控制模型朝向。

### 扩展数据（Extended Data）

扩展数据（Extended Data）可以叠加偏移（Offset）与扭曲（Twist）层。

偏移值（Offset Value）默认 `0.2`，提高后整条粒子路径离曲线更远。偏移混合（Offset Blend）默认 `50%`，控制偏移在两条侧向轴之间怎样分配。偏移沿长度曲线可以让偏移从曲线起点到终点逐渐变化。

扭曲方向（Twist Direction）选择顺时针（Clockwise）或逆时针（Anti-clockwise）。扭曲数（Twists）默认 `0`，提高后粒子会沿曲线绕更多圈；扭曲半径（Twist Radius）默认 `0.1`，提高后螺旋离曲线中心更远。半径沿长度曲线可以让螺旋沿路径收紧或展开。

### 曲线释放（Release）

曲线的释放模式（Release Mode）默认样条末端（Spline End）。还可使用衰减（Falloff）、时间（Time）和选集（Selection）。

释放时（On Release）决定脱离后的动作：无操作（Do Nothing）会继续原有运动；循环（Loop）跳回曲线起点；反转（Reverse）沿曲线掉头；销毁（Kill）移除粒子；更改组（Change Group）把粒子移入其他组。

距离（Distance）默认 `100%`，表示粒子沿曲线走到的长度比例。降低后，粒子会在到达曲线末端前释放。时间（Time）模式下，时间模式（Time Mode）、释放时间（Release Time）和变化（Variation）的含义与表面释放相同。

### 样条数据（Spline Data）

多段（Multi Segment）控制多段曲线的选择方式：任意分段（Any Segment）、特定段（Specific Segment）、序列中的段（Segments In Sequence）和最近段（Nearest Segment）。选择特定段（Specific Segment）后，段（Segment）指定要使用的段编号。

起始模式（Starting Mode）默认最近点（Nearest Point），还可选择最近顶点（Nearest Vertex）、特定顶点（Specific Vertex）和沿样条位置（Position Along Spline）。沿样条位置会显示位置（Position）及变化（Variation）；特定顶点会显示索引（Index）。这些选项决定粒子第一次被曲线捕捉时从哪里进入路径。

## 组（Groups Affected）、映射（Mapping）和衰减（Falloff）

组（Groups Affected）可将跟随作用限制到指定的 `nx 组（nxGroup）`。列表为空时，修改器影响全部可用粒子。

映射（Mapping）可以用粒子数据动态控制跟随参数。衰减（Falloff）使用一个或多个 `nx 衰减（nxFalloff）` 限制修改器的空间范围。粒子未被目标物体捕捉时，先检查组和衰减是否把它排除在外。

## 常见问题

### 粒子没有贴到表面

检查目标对象是否已加入对象（Objects）列表，粒子是否进入距离（Distance）范围，并逐步提高拉力（Pull）。高速粒子容易在一次时间步内越过很小的捕捉范围，可以提高距离（Distance）或降低粒子速度。

### 粒子贴住后立刻停下

检查摩擦力（Friction）是否过高。降低它后，粒子会更容易沿表面继续滑动。

### 沿曲线移动方向反了

在曲线连接（Connection）中把方向（Direction）切换为向后（Backwards）。

### 曲线路径没有螺旋或偏移

检查扩展数据（Extended Data）里的对应层是否启用。偏移（Offset）需要提高偏移值（Offset Value）；扭曲（Twist）需要将扭曲数（Twists）设为大于 `0`。
