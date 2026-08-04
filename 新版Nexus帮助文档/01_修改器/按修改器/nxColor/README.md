# nx 颜色（nxColor）

当前文档版本：NeXus `2026.0.0`

`nx 颜色（nxColor）`用多层规则改变 NeXus 粒子的颜色。每一层可以读取速度、年龄、距离、噪波等数据，再与上方颜色结果混合。

新建修改器会自动加入一个参数梯度（Gradient by Parameter）层，默认按速度（Speed）从渐变中取色。只需要固定颜色时，可以把层类型（Layer Type）改为设置颜色（Set Color）。

## 添加和起步

1. 从添加（Add） ▸ INSYDIUM NeXus ▸ `nx 颜色（nxColor）`创建修改器。
2. 在属性（Properties） ▸ 物理（Physics）中选择颜色层。
3. 选择层类型（Layer Type），设置这一层的颜色来源。
4. 需要组合多种效果时继续添加层，并用混合模式（Blend Mode）和强度（Strength）控制叠加结果。

## 修改器设置

### 已启用（Enabled）

已启用（Enabled）控制整个颜色修改器。关闭后，所有颜色层停止更新，层列表和参数仍会保留。

### 仅在出生时改变（Change On Birth Only）

仅在出生时改变（Change On Birth Only）默认关闭，此时颜色会在粒子生命周期内持续更新。开启后，每颗粒子只在出生时计算一次颜色，之后保持该结果。

随机颜色需要固定在每颗粒子上时，可以开启它。按速度、移动距离或到物体的距离持续变色时，应保持关闭。

### 层（Layers）

层（Layers）列表从上到下计算，每一层与上方结果混合。列表右侧的已启用（Enabled）只控制当前层；上下箭头会改变计算顺序，因此交换层顺序可能改变最终颜色。

可用层类型包括参数梯度（Gradient by Parameter）、噪波（Noise）、设置颜色（Set Color）、递增/递减（Increment/Decrement）、时间相关（Time-Dependent）、到物体的距离（Distance from Object）和到摄像机的距离（Distance from Camera）。

## 每层共有设置

### 层类型（Layer Type）

层类型（Layer Type）决定当前层执行哪种颜色操作。切换后，下方专用参数会随类型变化。

### 混合模式（Blend Mode）和强度（Strength）

混合模式（Blend Mode）默认法线方向（Normal），还可选择最小（Min）、减去（Subtract）、相乘（Multiply）、叠加（Overlay）、最大（Max）、添加（Add）、屏幕（Screen）和差值（Difference）。只使用一层时保持法线方向（Normal）最容易判断；多层组合时，混合模式决定当前层怎样改变上方颜色。

强度（Strength）默认 `100%`。降低后，当前层只给上方结果加入部分颜色；设为 `0%`时当前层不产生可见变化。设置颜色（Set Color）层在低强度下更像给原颜色加一层色调，`100%`时会完整使用设定颜色。

### 变化速率模式（Rate Mode）

变化速率模式（Rate Mode）默认即时（Instant），决定粒子多快到达当前层算出的目标颜色。

- 即时（Instant）：颜色立即改变
- 帧时间（Frame Time）：随时间逐渐接近目标颜色
- 自定义（Custom）：同样逐渐变化，并使用变化速率倍率（Rate Multiplier）控制速度

变化速率倍率（Rate Multiplier）默认 `100%`，只在自定义（Custom）模式下可编辑。提高后更快接近目标颜色；降低后颜色过渡需要更长时间。

### 阈值（Threshold）

阈值（Threshold）默认 `0%`，为每次颜色变化加入跳过概率。`0%`时每颗粒子都会执行变化；提高后会有更多粒子保留原颜色；`100%`时颜色变化会被全部跳过。它适合制作分散、缺口明显的着色结果。

## 参数梯度（Gradient by Parameter）

参数梯度（Gradient by Parameter）把每颗粒子的某项数据映射到梯度（Gradient）。最小值对应渐变左端，最大值对应右端，中间数据取两端之间的颜色。

参数（Parameter）默认速度（Speed），还可使用年龄（Age）、方向（Direction）、移动距离（Distance Traveled）、密度（Density）、生命（Life）、质量（Mass）、邻点（Neighbor）、半径（Radius）、衰减（Falloff）、随机（Random）、烟雾（Smoke）、温度（Temperature）和燃料（Fuel）。

- 速度（Speed）：移动越快，越接近最大（Max）对应的渐变颜色
- 年龄（Age）：粒子出生后逐渐沿渐变移动
- 方向（Direction）：根据粒子朝向轴取色
- 移动距离（Distance Traveled）：粒子走得越远，越接近渐变右端
- 密度（Density）和邻点（Neighbor）：按周围粒子的密集程度取色
- 生命（Life）：按粒子生命数据取色
- 质量（Mass）和半径（Radius）：按每颗粒子的物理属性取色
- 衰减（Falloff）：按修改器衰减区域提供的强度取色
- 随机（Random）：为粒子分配随机渐变位置
- 烟雾（Smoke）、温度（Temperature）和燃料（Fuel）：读取 `nx ExplosiaFX（nxExplosiaFX）`平流通道

最小（Min）和最大（Max）决定数据映射范围。低于最小值的粒子使用渐变左端颜色，高于最大值的粒子使用右端颜色。缩小两者间距会让颜色更快跨完整个渐变；扩大间距后，颜色变化覆盖更大的数据范围。

邻居半径（Neighbor Radius）只在参数（Parameter）设为邻点（Neighbor）时出现，默认 `0.25 m`。提高后会在更大范围内统计邻点，颜色更容易反映大范围密度；降低后只统计紧邻粒子，局部聚集会更明显。

使用固定点（Use Fixed Point）只在参数（Parameter）设为随机（Random）时出现，默认开启。开启后每颗粒子保留同一个随机值，颜色保持稳定；关闭后随机值可以随重新计算而变化。

轴（Axis）只在参数（Parameter）设为方向（Direction）时出现，默认 `X`。X 轴（X）读取航向，Y 轴（Y）读取俯仰，Z 轴（Z）读取侧倾。需要按实例模型的实际旋转着色时，先使用 `nx 自旋（nxSpin）`建立粒子朝向。

## 噪波（Noise）

噪波（Noise）从三维程序图案中为粒子取色，适合制作斑驳、流动或空间分区颜色。

噪波类型（Noise Type）默认沃罗诺伊噪波（VoroNoise），还可选择单纯形（Simplex）、卷曲（Curl）、湍流（Turbulence）、波浪湍流（Wavy Turbulence）、分形布朗运动（FBM）和立方（Cubic）。切换类型会改变图案结构。

时序（Timing）默认帧时间（Frame Time）。帧时间（Frame Time）让整个场景按时间线采样噪波；粒子寿命（Particle Age）让每颗粒子按自己的出生时间推进图案。

颜色通道（Color Channel）默认梯度（Gradient）。梯度（Gradient）用噪波数值在下方渐变中取色；噪波（Noise）直接生成红、绿、蓝（RGB）颜色通道。下方梯度（Gradient）只在颜色通道设为梯度（Gradient）时参与结果。

随机种（Seed）默认 `1`，修改后会换一套噪波图案。缩放（Scale）默认 `100%`，提高后形成更宽的连续色块，降低后颜色细节更密集。

持续度（Persistence）默认 `100%`，提高后保留更多小尺度细节；降低后主要留下大块颜色。间隙度（Lacunarity）默认 `1`，提高后不同倍频之间的尺寸差异更明显。频率（Frequency）默认 `100%`，提高后颜色变化更频繁。倍频（Octaves）默认 `1`，提高后叠加更多细节和层次。

低裁切（Low Clip）默认 `0%`，提高后会压掉更多低值噪波区域。高裁切（High Clip）默认 `100%`，降低后会压掉更多高值区域。两者靠近时，图案会形成更明确的分区。

亮度（Brightness）默认 `0%`，正值让噪波输出整体变亮，负值让整体变暗。对比度（Contrast）默认 `100%`，提高后明暗和颜色分界更明显；降低后颜色变化更平缓。

## 设置颜色（Set Color）

设置颜色（Set Color）把所有受影响粒子设为颜色（Color）色块中的固定颜色，默认橙色。它适合统一改色，也适合放在图层顶部作为后续混合的基础色。

当前插件会在设置颜色（Set Color）层下方同时显示梯度（Gradient）控件，这个控件不会影响该层结果。该层只读取颜色（Color）色块。

## 递增/递减（Increment/Decrement）

递增/递减（Increment/Decrement）在每次计算时分别改变红、绿、蓝三个颜色通道，让颜色随时间持续偏移。

红色变化率（Red Rate of Change）、绿色变化率（Green Rate of Change）和蓝色变化率（Blue Rate of Change）的范围为 `-100%`到 `100%`，默认均为 `0%`。正值持续增加对应颜色，负值持续减少；例如提高红色并降低蓝色，会让粒子逐渐向暖色移动。

## 时间相关（Time-Dependent）

时间相关（Time-Dependent）让粒子在一段时间内从梯度（Gradient）左端走到右端。

完成时间（Time to Completion）默认 `3 s`，决定完整走完渐变需要多久。提高后变色更慢；降低后更快到达末端。界面中的帧/秒切换可以在帧数和秒数之间选择时间单位。

完成时（On Complete）默认无操作（Do Nothing）：颜色停在渐变末端。循环至开始（Wrap to Start）会跳回渐变起点并再次播放；反转（Reverse）会从末端向起点返回，形成往返变化。

## 到物体的距离（Distance from Object）

到物体的距离（Distance from Object）按粒子与目标物体之间的距离从梯度（Gradient）取色。物体（Object）可选择网格（Mesh）、曲线（Curve）或曲线（Curves）对象。

最近距离（Nearest Distance）默认 `0 m`，对应渐变左端；最远距离（Furthest Distance）默认 `5 m`，对应渐变右端。减小最远距离后，粒子离目标较近时就会走完整个渐变；提高后颜色会在更大的空间范围内缓慢变化。

未指定物体（Object）时，这一层没有可用的距离来源。

## 到摄像机的距离（Distance from Camera）

到摄像机的距离（Distance from Camera）按粒子与指定相机（Camera）的距离从梯度（Gradient）取色，适合让近处和远处使用不同颜色。

最近距离（Nearest Distance）默认 `0 m`，最远距离（Furthest Distance）默认 `5 m`，映射规则与到物体的距离（Distance from Object）相同。

视场（FOV）默认关闭。开启后，距离判断会结合相机视场，适合让颜色分布跟随指定镜头的可见范围。未指定相机（Camera）时，这一层没有可用的距离来源。

## 组（Groups Affected）、映射（Mapping）和衰减（Falloff）

组（Groups Affected）可以把 `nx 颜色（nxColor）`限制到指定的 `nx 组（nxGroup）`。列表为空时影响所有可用粒子；加入组后，只给这些组中的粒子着色。

映射（Mapping）可以使用粒子数据动态驱动颜色修改器中的数值参数。参数梯度（Gradient by Parameter）已经直接提供常用数据着色，只有需要让粒子数据控制其他层参数时才需要继续使用映射。

衰减（Falloff）使用一个或多个 `nx 衰减（nxFalloff）`限制着色的空间范围。粒子进入有效衰减区域后才会接受颜色变化；多个衰减对象可以继续混合。

## 颜色怎样进入最终输出

`nx 颜色（nxColor）`写入逐粒子颜色。需要让后续几何体使用这份颜色时，还要在输出修改器中选择对应来源：

- `nx 生成器（nxGenerator）`的颜色来源选择粒子（Particle）后，会读取模拟中的逐粒子颜色
- `nx 拖尾（nxTrail）`的拖尾颜色模式选择粒子颜色（Particle Color）后，会使用粒子当前颜色
- `nx 网格化（nxMesher）`开启传递颜色（Transfer Color）后，会把粒子颜色写到网格顶点

视口粒子已经变色而渲染几何体仍保持原色时，先检查这些下游颜色来源和材质是否读取了传递后的颜色数据。

## 常见问题

### 随机颜色一直变化

在参数梯度（Gradient by Parameter）中选择随机（Random），并开启使用固定点（Use Fixed Point）。还可以开启修改器顶部的仅在出生时改变（Change On Birth Only），让每颗粒子的出生颜色保持不变。

### 按速度或距离着色看起来只有一种颜色

检查最小（Min）和最大（Max）是否覆盖了场景中的实际数据范围。范围过大时，所有粒子会集中在渐变的一小段；范围过小时，大量粒子会同时落到渐变端点。

### 调整变化速率倍率没有反应

把变化速率模式（Rate Mode）设为自定义（Custom）。即时（Instant）和帧时间（Frame Time）下，变化速率倍率（Rate Multiplier）处于禁用状态。

### 设置颜色层里的渐变没有效果

设置颜色（Set Color）层只读取颜色（Color）色块。需要使用渐变时，改用参数梯度（Gradient by Parameter）、时间相关（Time-Dependent）或距离类图层。
