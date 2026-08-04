# nx 覆盖（nxCover）

当前文档版本：NeXus `2026.0.0`

`nx 覆盖（nxCover）`把粒子引导到指定物体的表面、顶点、边、内部或粒子运动路径与物体的交点，并控制粒子到达后怎样停留和释放。它适合制作粒子逐渐包覆模型、附着到表面、填入物体内部或沿网格结构排列的效果。

## 添加和起步

1. 从添加（Add） ▸ INSYDIUM NeXus ▸ `nx 覆盖（nxCover）`创建修改器。
2. 在物体（Objects）列表中加入一个网格（Mesh）、曲线（Curve）或曲线（Curves）对象。
3. 先保持默认的多边形面积（Polygon Area）和吸引（Attract），播放时间线观察粒子是否向物体表面移动并停留。
4. 粒子穿过目标或在表面附近来回越过时，开启使用制动（Use Braking）。
5. 需要覆盖多个目标时，再设置对象模式（Object Mode）和各目标条目的操作（Operation）。

物体（Objects）列表为空时，修改器没有覆盖目标，粒子不会被引导到物体上。

## 已启用（Enabled）

已启用（Enabled）控制整个修改器。关闭后，粒子会脱离覆盖作用继续运动，物体列表和参数仍会保留。

## 对象模式（Object Mode）

对象模式（Object Mode）决定物体（Objects）列表中有多个目标时，每颗粒子被分配到哪个目标，默认是序列（Sequence）。

- 序列（Sequence）：按列表顺序依次覆盖目标
- 最近对象（Nearest Object）：每颗粒子前往离自己最近的目标
- 最远对象（Furthest Object）：每颗粒子前往离自己最远的目标
- 对象索引（Object Index）：所有粒子前往对象索引（Object Index）指定的目标
- 随机对象（Random Object）：每颗粒子从列表中随机选择目标

### 对象索引（Object Index）

对象索引（Object Index）只在对象模式（Object Mode）为对象索引（Object Index）时出现，默认值是 `0`。列表第一项的索引是 `0`，提高数值会选择列表中更靠后的目标。

### 随机种子（Random Seed）

随机种子（Random Seed）只在对象模式（Object Mode）为随机对象（Random Object）时出现，默认值是 `0`。改变数值会重新分配粒子与目标的随机对应关系，粒子数量和目标列表不会改变。

### 循环（Cycle）

循环（Cycle）只在序列（Sequence）和随机对象（Random Object）下出现，默认关闭。开启后，全部目标完成一轮覆盖后会继续从列表中分配目标；关闭后，完成一轮后停止继续循环分配。

## 物体（Objects）

物体（Objects）列表支持网格（Mesh）、曲线（Curve）和曲线（Curves）对象。选中列表条目后，下方显示该目标自己的落点、移动、保持、朝向和颜色设置。

每个条目右侧都有已启用（Enabled）开关。关闭后可以临时跳过该目标，无需把它从列表中删除。

## 物体设置（Object Settings）

### 操作（Operation）

操作（Operation）决定粒子在当前目标上怎样选择覆盖位置，默认是多边形面积（Polygon Area）。

- 多边形面积（Polygon Area）：按多边形面积加权，把粒子均匀分散到整个表面；大多边形会分到更多粒子
- 多边形中心（Polygon Center）：把粒子放到各多边形中心，覆盖结果会显出网格面片排列
- 点（Points）：把粒子放到网格顶点，结果会显出顶点拓扑
- 边（Edges）：把粒子分布到网格边上，结果会沿线框结构排列
- 体积（Volume）：把粒子分布到网格内部
- 射线相交（Ray Intersection）：让粒子落到自身运动路径与网格相交的位置，粒子需要先朝目标方向运动才容易形成交点

### 强度（Strength）

强度（Strength）控制粒子被拉向覆盖位置的力度，默认值是 `100%`。降低后，粒子会保留更多原有运动，贴近目标的速度和紧密程度都会减弱；提高后更积极地靠近分配位置。

### 容差（Tolerance）

容差（Tolerance）控制粒子距离覆盖位置多近时算作已经到达，默认值是 `0.1 m`。

提高后，粒子可以在离目标较远的位置提前进入保持状态，覆盖会显得较松。降低后，粒子需要更接近精确位置才会进入保持；数值过小时，高速粒子可能多次越过目标才被判定到达。

### 继承父级（Inherit Parent）

继承父级（Inherit Parent）默认关闭。开启后，当前条目读取列表第一项的后续设置，对齐/颜色（Alignment / Color）、运动（Movement）、保持（Holding）和制动（Braking）会隐藏。

多个目标需要相同运动和停留行为时，可以把完整设置放在第一项，再让其他条目继承。操作（Operation）、强度（Strength）和容差（Tolerance）仍保留在当前条目中。

## 对齐/颜色（Alignment / Color）

### 随对象旋转（Rotate With Object）

随对象旋转（Rotate With Object）默认关闭。开启后，粒子会跟随覆盖物体的旋转，目标转动时粒子继续保持在对应表面位置。

随对象旋转（Rotate With Object）与法线对齐（Align to Normals）互斥。开启其中一项后，另一项会变为不可用。

### 法线对齐（Align to Normals）

法线对齐（Align to Normals）默认关闭。开启后，粒子的向上方向会逐渐对齐覆盖位置的表面法线，让具有朝向的粒子实例贴合表面方向。

### 对齐强度（Alignment Strength）

对齐强度（Alignment Strength）只在法线对齐（Align to Normals）开启时生效，默认值是 `10%`。提高后朝向更快贴合表面法线；降低后转向过程更缓慢，粒子会保留更多原有朝向。

需要在实例模型上看到朝向结果时，让 `nx 生成器（nxGenerator）`读取粒子（Particle）旋转。

### 改变颜色（Change Color）

改变颜色（Change Color）默认关闭。开启后，粒子在覆盖目标时逐渐变为覆盖颜色（Cover Color）。

### 覆盖颜色（Cover Color）

覆盖颜色（Cover Color）只在改变颜色（Change Color）开启时生效，默认是橙色。它决定粒子覆盖目标时最终采用的颜色。

### 颜色时序（Color Timing）

颜色时序（Color Timing）只在改变颜色（Change Color）开启时生效，默认值是 `100%`。降低后，粒子需要更长时间才会变成覆盖颜色；提高后，颜色变化更快完成。

### 改变释放颜色（Change Release Color）和释放颜色（Release Color）

改变释放颜色（Change Release Color）默认关闭。开启后，粒子从目标释放时变为释放颜色（Release Color），默认同样是橙色。

颜色变化写入粒子颜色。需要在渲染实例上显示这份颜色时，可以让 `nx 生成器（nxGenerator）`的颜色来源（Color Source）读取粒子（Particle），并使用能够读取逐实例颜色的材质。

## 运动（Movement）

运动（Movement）控制粒子从当前位置前往覆盖位置的过程。

### 速度模式（Speed Mode）

速度模式（Speed Mode）默认使用速度（Use Speed）。

- 使用速度（Use Speed）：按粒子速度模式（Particle Speed Mode）决定移动速度，每颗粒子按自身距离到达
- 到达目标时间（Time to Target）：让粒子在指定覆盖时间（Time to Cover）内到达，距离不同的粒子会使用不同移动速度

### 粒子速度模式（Particle Speed Mode）

粒子速度模式（Particle Speed Mode）只在速度模式（Speed Mode）为使用速度（Use Speed）时生效，默认是粒子（Particle）。

- 粒子（Particle）：沿用粒子进入修改器时的自身速度
- 固定（Fixed）：全部使用固定速度（Fixed Speed）
- 力（Force）：持续向覆盖位置施加力，粒子速度会在运动中逐渐改变

### 固定速度（Fixed Speed）

固定速度（Fixed Speed）只在粒子速度模式（Particle Speed Mode）为固定（Fixed）时生效，默认值是 `1.5 m/s`。提高后粒子更快到达目标，也更容易越过很小的容差范围；降低后覆盖过程更慢。

### 覆盖时间（Time to Cover）

覆盖时间（Time to Cover）只在速度模式（Speed Mode）为到达目标时间（Time to Target）时生效，默认值是 `1`帧。提高后粒子会用更长时间移动到表面；降低后会更快到达。

### 时间变化量（Time Variation）

时间变化量（Time Variation）只在到达目标时间（Time to Target）下生效，默认值是 `0`帧。提高后，各粒子的到达时间会分散开；保持 `0`时，粒子按相同覆盖时间到达。

### 释放时间（Release Time）

释放时间（Release Time）控制粒子到达后保持多久再释放，默认值是 `60`帧。提高后覆盖层在表面停留更久；降低后粒子更早离开。

### 释放变化量（Release Variation）

释放变化量（Release Variation）默认值是 `0`帧。提高后，各粒子的释放时间会分散，覆盖层逐渐脱落；保持 `0`时，同一时间到达的粒子更容易一起释放。

## 保持（Holding）

保持（Holding）控制粒子被判定到达覆盖位置后的运动方式。

### 保持模式（Holding Mode）

保持模式（Holding Mode）默认是吸引（Attract）。

- 吸引（Attract）：持续把粒子拉回覆盖位置，允许粒子在目标附近运动
- 自由（Free）：到达后立即恢复自由运动，适合只让粒子短暂经过覆盖位置
- 弹簧（Spring）：用弹簧把粒子连接到覆盖位置，粒子可以偏离并回弹
- 粘附（Stick）：把粒子固定在覆盖位置，适合保持稳定的包覆层

### 模式（Mode）

模式（Mode）只在保持模式（Holding Mode）为吸引（Attract）时生效，默认是速度（Velocity）。

- 速度（Velocity）：直接控制粒子朝覆盖位置移动的速度，响应更直接
- 加速度（Acceleration）：向覆盖位置施加加速度，粒子会逐渐改变现有速度

### 到达模式（Arrive Mode）

到达模式（Arrive Mode）只在吸引（Attract）下生效，默认是减速（Slowdown）。

- 减速（Slowdown）：粒子进入距离（Distance）范围后逐渐减速，更容易停在目标附近
- 吸引（Attract）：持续拉向目标，不主动减速，粒子可能在覆盖位置两侧往返

### 距离（Distance）

距离（Distance）只在吸引（Attract）和减速（Slowdown）同时使用时生效，默认值是 `0.5 m`。提高后粒子会更早开始减速，接近过程更平缓；降低后粒子到很靠近目标时才减速。

### 最小速度（Min Speed）和最大速度（Max Speed）

最小速度（Min Speed）与最大速度（Max Speed）只在吸引（Attract）下生效，默认分别是 `0.01 m/s`和 `1.5 m/s`。

最小速度（Min Speed）提高后，粒子接近目标时仍会保持较明显移动；数值过高时更容易越过目标。最大速度（Max Speed）提高后，较远粒子可以更快靠近；降低后吸引过程的最高速度会受限。

### 弹簧长度（Spring Length）

弹簧长度（Spring Length）只在保持模式（Holding Mode）为弹簧（Spring）时生效，默认值是 `0.005 m`。提高后，粒子可以在离覆盖位置更远处保持弹簧静止长度；降低后会被拉得更贴近目标。

### 刚度（Stiffness）

刚度（Stiffness）只在弹簧（Spring）下生效，默认值是 `1.5`。提高后粒子偏离时更快弹回；降低后覆盖层更松软，摆动范围更大。

### 阻尼（Damping）

阻尼（Damping）只在弹簧（Spring）下生效，默认值是 `5%`。提高后弹簧摆动更快停下；降低后粒子会在覆盖位置附近来回振荡更久。

## 制动（Braking）

制动（Braking）只在速度模式（Speed Mode）为使用速度（Use Speed）时可用。它让粒子接近覆盖位置时降低速度，减少高速粒子越过目标。

### 使用制动（Use Braking）

使用制动（Use Braking）默认关闭。开启后，制动距离（Braking Distance）、最小速度（Min Speed）、最大速度（Max Speed）和制动（Braking）曲线开始生效。

### 制动距离（Braking Distance）

制动距离（Braking Distance）默认值是 `0.5 m`。提高后粒子会在更远处开始减速；降低后会保持原速度到更靠近目标的位置。

### 最小速度（Min Speed）和最大速度（Max Speed）

制动中的最小速度（Min Speed）默认值是 `0.05 m/s`，决定接近目标时允许减到的最低速度。降低后更容易缓慢贴近；提高后到达更快，也更容易越过较小容差范围。

最大速度（Max Speed）默认值是 `1.5 m/s`，决定从多高的速度范围开始应用制动。需要处理更快粒子时提高；只想限制普通速度粒子时可以降低。

### 制动（Braking）曲线

制动（Braking）曲线控制粒子穿过制动距离时怎样从较高速度过渡到较低速度。曲线前段提高会让粒子更早明显减速；后段提高会让更多速度保留到靠近目标的位置。

## 组（Groups Affected）

组（Groups Affected）可以把覆盖作用限制到指定 `nx 组（nxGroup）`中的粒子。列表为空时处理全部粒子；加入组后，只处理这些组内的粒子。

## 映射（Mapping）

映射（Mapping）可以使用粒子数据驱动覆盖参数，让不同粒子获得不同强度、速度或保持行为。固定参数已经能完成目标时，可以保持映射为空。

## 衰减（Falloff）

衰减（Falloff）可以通过一个或多个 `nx 衰减（nxFalloff）`限制覆盖作用的空间范围。只有进入衰减区域的粒子会被当前修改器引导。

## 常见问题

### 粒子接近表面后反复越过目标

先开启使用制动（Use Braking），再适当提高制动距离（Braking Distance）。如果仍然越过目标，可以降低固定速度（Fixed Speed）或提高容差（Tolerance）。

### 粒子到达表面后马上离开

检查保持模式（Holding Mode）是否设为自由（Free），并检查释放时间（Release Time）是否过短。需要固定覆盖层时使用粘附（Stick）；需要轻微回弹时使用弹簧（Spring）。

### 多个目标没有按预期分配粒子

先检查对象模式（Object Mode），再检查各物体条目的已启用（Enabled）状态。使用对象索引（Object Index）时，列表第一项对应 `0`。
