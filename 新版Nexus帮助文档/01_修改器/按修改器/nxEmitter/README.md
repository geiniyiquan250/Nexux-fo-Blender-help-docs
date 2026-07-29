# nx 发射器（nxEmitter）

当前文档版本：NeXus `2026.0.0`

`nx 发射器（nxEmitter）` 是粒子的出生源。它决定粒子从哪里出现、何时出现，以及出生时的速度、半径、质量和寿命。

其他 NeXus 修改器会继续处理这批粒子。场景里没有粒子、出生位置不对、发射时间不对或数量异常时，先检查这里。

## 界面结构

`nx 发射器（nxEmitter）` 的设置位于物理属性（Physics Properties），分为五个页签：

- 物体属性（Object Properties）：设置发射形状，并在发射（Emission）和运动继承（Motion Inheritance）两个子页签中控制粒子出生。
- 显示（Display）：控制发射器和粒子在视口中的显示方式，不改变模拟结果。
- 导出（Export）：把实时粒子转换成 Blender 点云（Point Cloud），供渲染、几何节点和着色器节点使用。
- 组（Groups）：把出生粒子分配到一个或多个 `nx 组（nxGroup）`。
- 修改器（Modifiers）：指定哪些 NeXus 修改器可以影响这个发射器产生的粒子。

## 物体属性（Object Properties）

### 已启用（Enabled）

已启用（Enabled）控制整个发射器是否工作。关闭后会停止产生粒子，同时保留当前设置和对象。

### 子帧发射（Subframe Emit）

子帧发射（Subframe Emit）默认开启。它把每帧产生的粒子随机分散到这一帧所覆盖的时间区间内，减少高速连续粒子流中一排一排的间隔。发射器自身有动画时，这个时间分布更重要。

关闭后，同一帧的粒子会集中在该帧起点出生，快速移动时更容易看到分层或断续。常规连续发射建议保持开启。

## 形状（Shape）

形状（Shape）决定粒子的出生区域，默认值为矩形（Rectangle）。

- 矩形（Rectangle）：从平面矩形区域发射。宽度（Width）和高度（Height）默认均为 `1 m`；增大后出生区域变宽或变高。
- 圆盘（Disc）：从平面圆形区域发射。半径（Radius）默认值为 `0.5 m`；增大后出生圆盘变大。
- 球体（Sphere）：从球形体积发射。半径（Radius）默认值为 `0.5 m`；增大后出生体积变大。
- 盒体（Box）：从盒形体积发射。大小（Size）的三个轴默认均为 `1 m`；分别控制盒体在三个方向上的范围。
- 物体（Object）：从列表中的网格、曲线或发丝曲线对象发射，也可以把 `nx 流体（nxFluids）` 域用作体积源。

矩形（Rectangle）、圆盘（Disc）、球体（Sphere）和盒体（Box）的黄色视口控制柄可以直接调整尺寸。

### 平面（Plane）

平面（Plane）出现在物体（Object）以外的形状中，决定粒子出生时沿发射器的哪根轴获得初始速度，默认值为 `Y正轴（Y+）`。它会改变粒子的真实出生速度方向。

实例模型的真实旋转由 `nx 自旋（nxSpin）` 控制。模型需要持续指向行进方向时，在 `nx 自旋（nxSpin）` 中使用切向（Tangential）层。

### 角度（Angle）

角度（Angle）只在矩形（Rectangle）和圆盘（Disc）中出现，默认值为 `0°`。`0°` 时粒子沿平面（Plane）方向直线发出；绝对值增大后，发射方向会扩展成更宽的锥形范围。

### 方向（Direction）

方向（Direction）出现在球体（Sphere）和盒体（Box）中，默认值为法线方向（Normal）。

- 法线方向（Normal）：沿出生位置的表面法线向外发射。
- 随机（Random）：给粒子随机发射方向。
- 六个轴向选项：统一沿选定轴发射。

### 仅边缘、仅原点和仅表面

- 仅边缘（Edge Only）：矩形（Rectangle）和圆盘（Disc）只从外边缘出生粒子。它只在随机（Random）发射模式下可用，并且需要关闭仅原点（Origin Only）。
- 仅原点（Origin Only）：矩形（Rectangle）和圆盘（Disc）的粒子全部从发射器原点出生。它只在随机（Random）发射模式下可用，并且需要关闭仅边缘（Edge Only）。
- 仅表面（Surface Only）：球体（Sphere）和盒体（Box）只从体积表面出生粒子。关闭后，粒子可以分布在体积内部；它只在随机（Random）发射模式下可用。

## 物体（Object）形状

形状（Shape）设为物体（Object）后，需要在物体（Objects）列表中加入发射来源。每个来源都有独立设置：

- 已启用（Enabled）：临时启用或停用这一项来源。
- 选集（Selection）：网格对象可以使用顶点组（Vertex Group）或点域浮点属性（Attribute）提供权重。权重达到阈值（Threshold）的位置才会参与发射。
- 区域（Region）：包含（Include）会从该物体产生粒子；排除（Exclude）会移除落在该物体内部的候选粒子。默认值为包含（Include）。
- 发射来源（Emit From）：选择多边形中心（Polygon Center）、多边形面积（Polygon Area）、点（Points）、边（Edges）或体积（Volume）。多边形面积（Polygon Area）会让较大的多边形产生更多粒子；规则（Regular）和六边形（Hexagonal）发射模式只使用体积（Volume）。
- 每元素一个（One per Element）：在多边形中心（Polygon Center）、点（Points）和边（Edges）来源中可用。开启后，每个多边形、顶点或边产生一个粒子。
- 阈值（Threshold）：范围为 `0` 到 `1`，默认值为 `0.5`。提高后，需要更高的顶点组或属性权重才能发射，实际发射区域会收缩到高权重位置；降低后，更多低权重位置会参与。
- 方向（Direction）：决定粒子离开来源表面的方向，默认值为法线方向（Normal），还可以选择随机（Random）、Phong 法线方向（Phong Normal）或六个轴向。

曲线对象使用边（Edges）方式发射。液体发射类型会使用体积（Volume）作为来源。

## 发射（Emission）

### 发射类型（Emission Type）

发射类型（Emission Type）决定粒子随时间产生的方式，默认值为速率（Rate）。

- 速率（Rate）：按出生率（Birthrate）持续发射。
- 脉冲（Pulse）：重复发射一段时间，再间隔一段时间。
- 单次发射（Shot）：在指定时间释放一批固定数量的粒子。
- 液体填充（Liquid Fill）：在开始时一次性填充液体体积，适合静置后继续受力的液体主体。
- 液体流动（Liquid Flow）：持续向流体域注入液体，适合水流或入口。

### 发射模式（Emission Mode）

发射模式（Emission Mode）出现在速率（Rate）和单次发射（Shot）中，决定粒子如何排列，默认值为随机（Random）。

- 随机（Random）：粒子位置随机，局部会有密集和空缺。
- 规则（Regular）：按整齐网格排列，出生时不会互相重叠。
- 六边形（Hexagonal）：按错开的六边形行列排列，同一范围内通常能容纳更多粒子。

规则（Regular）和六边形（Hexagonal）使用间距（Spacing）和抖动（Jitter）控制排布，并替代随机模式中的出生率（Birthrate）或数量（Count）。原始形状会在面板中显示每次发射的预计粒子数量。

- 间距（Spacing）：按粒子直径的百分比设置粒子中心间距，默认值为 `100%`。提高后粒子之间空隙增加，实际数量减少；降低后排布更紧密。
- 抖动（Jitter）：在三个轴上给每个粒子的出生位置增加随机偏移，默认均为 `0`。提高后网格规律被打散，过大时会破坏整齐排布。

### 单次发射（Shot）

- 数量（Count）：只在随机（Random）模式中出现，默认值为 `100`，最小值为 `1`。提高后同一批粒子更多，降低后更少。
- 开始（Start）：控制这一批发射开始的时间，默认值为第 `2` 帧。第 `1` 帧通常用于建立初始模拟状态；设为第 `1` 帧时可能看不到这批粒子，先恢复为第 `2` 帧检查。
- 持续时间（Duration）：默认值为 `1`。设为 `1` 时整批粒子在单帧释放；提高后，同一批粒子会分散到更长时间内出生。

### 脉冲（Pulse）

- 开始（Start）：脉冲周期开始时间，默认值为 `0`。
- 长度（Length）：每次持续发射的时间，默认值为 `10`。
- 间隔（Interval）：两次脉冲之间的停发时间，默认值为 `10`。提高后每次发射之间停顿更久；需要只出现一次脉冲时，可把它设到场景时长之外。

### 出生率（Birthrate）

出生率（Birthrate）控制每秒或每帧产生的粒子数量，默认值为 `1000`。数值旁的单位开关可以在每秒和每帧之间切换。

它出现在随机（Random）模式下的速率（Rate）和脉冲（Pulse）中。速率（Rate）把它作为持续发射量；脉冲（Pulse）把它作为每次脉冲的发射量。提高后粒子流更密，降低后更稀。

旁边的变化（Variation）默认值为 `0`，会给发射数量加入随机波动。提高后各帧或各次脉冲的数量差异更明显。

## 液体（Liquid）发射

液体填充（Liquid Fill）和液体流动（Liquid Flow）会产生紧密排布的粒子，并连接到 `nx 流体（nxFluids）` 求解。

- 流体（Fluid）：指定接收液体粒子的 `nx 流体（nxFluids）` 域。没有目标域时，液体无法进入对应流体模拟。
- 求解器（Solver）：目标为 `nx 流体（nxFluids）` 时显示，可以直接选择该流体域使用的求解器。
- 分辨率（Resolution）：流体域最长边上的体素数量，默认值为 `50`，最小值为 `10`。提高后粒子间距更小，能够表现更细的液体结构，同时粒子数量和计算量会增加；降低后模拟更轻，细小水花和边缘细节会减少。
- 填充高度（Fill Level）：只在液体填充（Liquid Fill）中出现，范围为 `0%` 到 `100%`，默认值为 `30%`。提高后体积中初始液面更高，降低后初始液体更浅。
- 显示填充高度（Show Fill Level）：只在液体填充（Liquid Fill）中出现。开启后会在视口显示预设液面，方便求解前确认高度。
- 速度（Speed）：只在液体流动（Liquid Flow）中出现，决定液体注入速度。提高后液流离开发射器更快，降低后流入更慢。

液体流动（Liquid Flow）还会显示所有帧发射（Emit All Frames）、开始发射（Start Emit）和结束发射（End Emit），用于控制注入时间。

## 粒子出生属性

- 速度（Speed）：粒子的初始速度，默认值为 `1.5 m/s`。提高后粒子出生时移动更快；降到 `0` 后，粒子出生时停在原位，后续仍可受其他修改器影响。旁边的变化（Variation）会让粒子获得不同的初始速度。
- 半径（Radius）：粒子的物理半径，默认值为 `0.03 m`。提高后碰撞和实体显示形状中的粒子更大；降低后粒子更小。旁边的变化（Variation）会产生不同粒径。
- 质量（Mass）：每个粒子的质量，默认值为 `1`。提高后粒子携带的动量更大，`nx 拖拽（nxDrag）` 和 `nx 风（nxWind）` 改变其运动所需时间更长；降低后这些力更快改变粒子速度。旁边的变化（Variation）会产生不同质量。

## 发射时间与寿命

- 所有帧发射（Emit All Frames）：默认开启，从场景开始后持续允许发射。关闭后，开始发射（Start Emit）和结束发射（End Emit）会启用。
- 开始发射（Start Emit）和结束发射（End Emit）：默认值分别为第 `1` 帧和第 `2` 帧，共同限定允许发射的时间窗口。结束时间提前会缩短粒子流，延后会延长粒子流。
- 完整生命（Full Lifespan）：默认开启，粒子不会因年龄被移除。关闭后，寿命（Lifespan）和变化（Variation）会启用。
- 寿命（Lifespan）：默认值为 `90`。粒子年龄超过该值后会被移除；提高后粒子保留更久，降低后更早消失。变化（Variation）会打散粒子的消失时间。

时间参数旁的单位开关可以在帧（Frames）和秒（Seconds）之间切换。出生率（Birthrate）旁的单位开关可以在每秒和每帧之间切换。

## 运动继承（Motion Inheritance）

运动继承（Motion Inheritance）让粒子在出生时获得发射器自身的移动速度或旋转带来的速度。发射器静止时，这组设置没有可见效果。

- 使用运动继承（Use Motion Inheritance）：默认关闭。开启后，下面三项才会生效。
- 强度（Strength）：继承比例，范围为 `0%` 到 `100%`，默认值为 `100%`。`0%` 等同于没有继承；提高后发射器运动对初始速度的影响更强。
- 线性方向（Linear Direction）：默认开启，继承发射器位置移动产生的速度。
- 旋转方向（Rotational Direction）：默认开启，继承发射器旋转产生的速度。

## 显示（Display）

显示（Display）只改变视口反馈，不修改模拟数据。

- 视口可见（Visible in Editor）：显示或隐藏发射器形状的视口标记。
- 显示粒子（Show Particles）：默认开启，显示或隐藏视口粒子。
- 色彩模式（Color Mode）：默认单一颜色（Single Color）。梯度（Gradient）按粒子参数映射颜色；噪波（Noise）按出生位置的噪波图案着色；纹理（Texture）通过紫外映射（UV Map）从图像取色；材质（Material）从来源物体的材质取色。
- 粒子颜色（Particle Color）：只在单一颜色（Single Color）中出现，设置全部粒子的视口颜色。

梯度（Gradient）模式中的参数（Parameter）选择速度、年龄、半径等驱动数据。方向（Direction）或旋转（Rotation）作为驱动时，轴（Axis）选择具体分量。自动缩放（Autoscale）默认开启，会根据当前粒子数据调整颜色范围；关闭后，最小值（Min）和最大值（Max）手动定义映射区间。区间变窄时，较小的数据差异也会产生明显颜色变化。

噪波（Noise）模式提供噪波类型（Noise Type）、颜色通道（Color Channel）、随机种（Seed）、缩放（Scale）、持续度（Persistence）、间隙度（Lacunarity）、频率（Frequency）、倍频（Octaves）、低裁切（Low Clip）、高裁切（High Clip）、亮度（Brightness）和对比度（Contrast）。随机种（Seed）会更换图案；缩放（Scale）和频率（Frequency）改变图案尺寸与变化密度；倍频（Octaves）增加后细节层级更多；裁切、亮度和对比度用于收紧或调整最终颜色范围。

### 显示模式（Mode）

显示模式（Mode）决定每个粒子在视口中的形状，默认值为点（Points）。还可以选择方形（Square）、线（Line）、三维盒体（Box 3D）、圆形（Circle）、金字塔（Pyramid）、箭头（Arrow）、坐标轴（Axis）、球体（Sphere）、屏幕空间流体（Screen Space Fluid）或无（None）等模式。

- 点（Points）使用粒子大小（Particle Size）控制像素尺寸，范围为 `0` 到 `20`，默认值为 `3`。
- 线（Line）和箭头（Arrow）可以按速度、半径或固定值决定长度。固定长度（Fixed Length）直接设置长度；限制长度（Clamp Length）开启后，最小长度（Min Length）和最大长度（Max Length）限制可见线段范围。
- 三维形状模式会显示旋转（Rotation）和向上矢量（Up Vector）设置。这些设置控制视口标记如何画出朝向，粒子的真实旋转仍由 `nx 自旋（nxSpin）` 提供。
- 屏幕空间流体（Screen Space Fluid）把粒子在加速视口中显示为连续液体表面。它属于视口显示，不会创建可渲染网格；需要可渲染液体表面时使用 `nx 网格化（nxMesher）`。
- 无（None）隐藏粒子绘制。

显示约束（Display Constraints）默认关闭。开启后会绘制 `nx 约束（nxConstraints）` 产生的粒子连接线，并可分别设置出生（Birth）、距离（Distance）、自定义（Custom）和粘度（Viscosity）连接的颜色。

## 导出（Export）

- 创建点云（Create Point Cloud）：默认关闭。开启后，每帧根据实时粒子重建 Blender 点云（Point Cloud），用于渲染、几何节点和粒子信息节点（Particle Info Node）。
- 传递属性（Transfer Properties）：默认基础（Basic），传递速度（Velocity）、颜色（Color）、半径（Radius）和旋转（Rotation）。全部（All）传递所有可用通道；自定义（Custom）会显示逐项开关。

点云（Point Cloud）是 NeXus 粒子进入 Blender 渲染和节点流程的桥梁。只打开显示粒子（Show Particles）仍属于视口预览，不会自动创建这个点云对象。

## 组（Groups）

组（Groups）页签把出生粒子分配给列表中的 `nx 组（nxGroup）`，方便其他修改器只处理指定粒子。

- 随机（Random）：每个粒子随机进入列表中的一个组。
- 连续（Sequential）：粒子依次循环进入各组。
- 仅第一组（First Group Only）：所有粒子都进入列表第一项。

列表旁的创建按钮可以新建 `nx 组（nxGroup）` 并立即加入列表。

## 修改器（Modifiers）

修改器（Modifiers）列表限定哪些 NeXus 修改器可以影响这个发射器产生的粒子。场景有多个发射器时，可以用它让同一个力或处理只作用于其中一部分发射器。

## 常见问题

### 场景里没有粒子

依次检查已启用（Enabled）、当前时间、发射类型（Emission Type）、开始（Start）或开始发射（Start Emit）、显示粒子（Show Particles）和显示模式（Mode）。单次发射（Shot）设为第 `1` 帧后没有结果时，先恢复默认第 `2` 帧。

### 修改参数后仍看到旧结果

检查 `nx 缓存（nxCache）` 是否正在回放旧缓存。发射参数改变后，需要重新构建受影响的缓存。

### 粒子已经转弯，实例模型仍朝原方向

添加 `nx 自旋（nxSpin）` 并使用切向（Tangential）层。平面（Plane）和方向（Direction）只提供出生速度方向，后续路径改变后需要 `nx 自旋（nxSpin）` 更新粒子朝向。
