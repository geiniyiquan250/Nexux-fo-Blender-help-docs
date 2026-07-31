# nx 缩放（nxScale）

当前文档版本：NeXus `2026.0.0`

`nx 缩放（nxScale）`用分层方式改变粒子缩放、半径或质量。它可以让实例模型随年龄长大，让粒子半径按噪波变化，让质量随速度或加速度改变，也可以读取顶点组控制局部大小。

新建修改器会自动创建一个已启用的范围（Range）层，默认目标是粒子半径（Particle Radius）。层从列表顶部向下计算，每一层与上方结果混合，因此交换层顺序会改变最终结果。

## 先分清三个目标

- 粒子缩放（Particle Scale）：控制粒子实例几何体的 X、Y、Z 三轴大小。`nx 生成器（nxGenerator）`实例化到粒子上的模型会读取这个值。
- 粒子半径（Particle Radius）：控制 NeXus 粒子自身的半径，常见于粒子显示、邻域和模拟计算。
- 粒子质量（Particle Mass）：控制粒子对 `nx 重力（nxGravity）`、`nx 阻力（nxDrag）`等力的响应。

想让模型看起来变大时，选择粒子缩放（Particle Scale）。想改变粒子本身的计算尺寸时，选择粒子半径（Particle Radius）。想改变受力惯性时，选择粒子质量（Particle Mass）。

## 层（Layers）和共有设置

层（Layers）列表可添加、删除、排序和单独启用。新层先选择层类型（Layer Type）和粒子数据（Particle Data），再调该类型的专用参数。列表右侧已启用（Enabled）只关闭当前层。

混合（Blend）默认法线方向（Normal）。添加（Add）会叠加当前层，减去（Subtract）会削弱上方结果，相乘（Multiply）会放大或压低上方值；最小（Min）、最大（Max）、屏幕（Screen）、叠加（Overlay）和差值（Difference）适合需要特殊合成关系的多层效果。只使用一层时保持法线方向（Normal）最容易判断。

强度（Strength）默认 `100%`，控制当前层混入上方结果的比例。设为 `0%` 时该层不产生可见变化；降低后可以逐步减弱当前层，不必直接删除。

## 范围（Range）

范围（Range）是默认层类型，让目标从起始值过渡到结束值。

时序（Timing）默认粒子寿命（Particle Age）。出生时（On Birth）只写入结束值；粒子寿命（Particle Age）按每颗粒子的出生时间过渡；帧时间（Frame Time）按时间线统一过渡；衰减（Falloff）按修改器衰减值过渡。

起始缩放、起始半径或起始质量（Start）在出生时（On Birth）模式下隐藏；结束缩放、结束半径或结束质量（End）始终存在。提高结束值会让粒子最终更大、更粗或更重；降低后结果更小、更轻。变化（Variation）给起始和结束值加入随机差异，避免所有粒子同步变化。

起始时间（Start Time）与结束时间（End Time）只在粒子寿命（Particle Age）和帧时间（Frame Time）下出现，决定过渡开始和结束的时间窗口。拉长窗口后变化更慢；缩短窗口后变化更快。缓动（Ease）曲线决定这段过渡是匀速、先慢后快或先快后慢。

## 噪波（Noise）

噪波（Noise）用空间噪波让不同位置的粒子获得不同缩放、半径或质量。噪波类型（Noise Type）默认沃罗诺伊噪波（VoroNoise），还可选择单纯形（Simplex）、卷曲（Curl）、湍流（Turbulence）、波浪湍流（Wavy Turbulence）、分形布朗运动（FBM）和立方（Cubic）。

起始值（Start）与结束值（End）决定噪波输出映射到的范围；变化（Variation）继续增加随机差异。随机种（Seed）更换噪波图案，不直接增强效果。对比度（Contrast）曲线重映射噪波结果，能让大粒子集中到少数区域，或让大小变化更平均。

缩放（Scale）默认 `100%`，提高后噪波区域变大，较远粒子更容易拥有相近大小；降低后大小变化更密集。持续度（Persistence）默认 `100%`，控制后续倍频的细节强度；降低后大尺度形状更明显。间隙度（Lacunarity）默认 `1`，提高后各倍频间的细节尺度差异增大。频率（Frequency）默认 `100%`，提高后噪波变化更密集。倍频（Octaves）默认 `1`，提高后叠加更多细节，也会让大小分布更复杂。

## 随时间改变数值（绝对）（Change Value Over Time (Absolute)）

这一层每一步给目标加上固定数值。改变（Change）为正时持续长大、变粗或变重；使用负值时持续缩小、变细或变轻。粒子缩放（Particle Scale）可以分别设置 X、Y、Z 三轴，因此可以形成拉长或压扁的实例模型。变化（Variation）让不同粒子的改变速度不同。

## 随时间改变数值（相对）（Change Value Over Time (Relative)）

这一层按当前数值的百分比持续改变，默认百分比（Percentage）是 `5%`。正值会形成逐步放大的累积变化，负值会形成逐步缩小的累积变化。数值越大，后期变化越快，适合指数式生长或衰减。

## 设置数值（Set Value）

设置数值（Set Value）直接把目标固定为数值（Value）。提高数值后，粒子立刻使用更大、更粗或更重的结果；降低后立刻变小、更细或更轻。变化（Variation）可给固定值加入随机范围。

## 由衰减设置（Set by Falloff）

由衰减设置（Set by Falloff）读取修改器衰减（Falloff）页的值来改变目标。粒子进入衰减区域时会按区域强度改变大小、半径或质量。

重映射衰减值（Remap Falloff Value）默认关闭。开启后，衰减值会扩展到 `-1` 至 `1` 的范围，区域外的粒子也会缩小，区域内粒子会长大。该层需要先在修改器衰减（Falloff）页加入 `nx 衰减（nxFalloff）`。

## 按速度缩放（Scale by Speed）和按加速度缩放（Scale by Acceleration）

按速度缩放（Scale by Speed）使用粒子当前速度作为改变（Change）的倍率。移动越快，大小、半径或质量改变得越明显。

按加速度缩放（Scale by Acceleration）使用粒子速度每帧的变化量作为倍率。加速、减速、碰撞或强力转向时更容易产生变化。两者都使用改变（Change）、变化（Variation）和限制（Limits）。

## 按映射缩放（Scale by Map）

按映射缩放（Scale by Map）读取网格或曲线对象的顶点组权重，按粒子附近的权重改变目标。

顶点组对象（Vertex Group Object）指定权重来源对象；设置后才显示顶点组（Vertex Group）。最大距离（Max Distance）默认 `0.5`，超过这个距离的粒子不会继续采样该对象。结束值（End）决定权重映射到的最大结果，变化（Variation）可打散结果。起始时间（Start Time）、结束时间（End Time）和缓动（Ease）与范围（Range）层的时间控制相同。

## 限制（Limits）和时间范围外行为

随时间改变数值（绝对）（Change Value Over Time (Absolute)）、随时间改变数值（相对）（Change Value Over Time (Relative)）、设置数值（Set Value）、由衰减设置（Set by Falloff）、按速度缩放（Scale by Speed）和按加速度缩放（Scale by Acceleration）提供限制（Limits）。

限制开关会随粒子数据（Particle Data）显示为限制到缩放上限（Clamp to Scale Limit）、限制到半径上限（Clamp to Radius Limit）或限制到质量上限（Clamp to Mass Limit），默认开启。对应的缩放下限与缩放上限、半径下限与半径上限、质量下限与质量上限会阻止数值继续超出范围。提高上限允许更大结果，提高下限可避免粒子缩小到不可见。范围内限制（Clamp Within Range）默认关闭；开启后，每颗粒子在上下限之间使用不同随机限制值，最终大小会保留差异。由衰减设置（Set by Falloff）不显示范围内限制（Clamp Within Range）。

缓动（Ease）曲线的范围外模式（Clamp Mode）默认钳制（Clamp）：窗口前保持起始值，窗口后保持结束值。范围内钳制（Clamp In Range）只在时间窗口内作用；重复（Repeat）循环曲线；继续（Continue）让曲线延续到边界之外。

## 组（Groups Affected）、映射（Mapping）和衰减（Falloff）

组（Groups Affected）可以限制哪些 `nx 组（nxGroup）`受到缩放。映射（Mapping）可以用粒子数据驱动层参数。衰减（Falloff）限制整个修改器的空间作用范围，并为由衰减设置（Set by Falloff）提供读取值。

## 常见问题

### 实例模型大小没有变化

检查粒子数据（Particle Data）是否设为粒子缩放（Particle Scale）。新建范围（Range）层默认影响粒子半径（Particle Radius），它不会直接改变 `nx 生成器（nxGenerator）`实例模型的尺寸。

### 粒子一直变大或一直缩小

检查随时间改变数值（绝对）（Change Value Over Time (Absolute)）的改变（Change），以及随时间改变数值（相对）（Change Value Over Time (Relative)）的百分比（Percentage）。需要阻止数值继续变化时，开启当前粒子数据对应的限制开关并设置上下限。

### 由衰减设置没有结果

先在修改器衰减（Falloff）页添加并启用 `nx 衰减（nxFalloff）`。没有可用衰减对象时，这一层没有可读取的空间值。
