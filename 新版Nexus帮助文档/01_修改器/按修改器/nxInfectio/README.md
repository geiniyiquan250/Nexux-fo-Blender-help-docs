# nx 感染（nxInfectio）

当前文档版本：NeXus `2026.0.0`

`nx 感染（nxInfectio）`让一种状态从种子位置进入粒子群，再由已感染粒子传播给邻居。粒子依次经历未感染、潜伏中和已感染阶段，可用于感染扩散、颜色生长、结晶前沿和逐点传播效果。

## 添加和起步

1. 先建立一批距离足够接近的粒子。
2. 从添加（Add） ▸ INSYDIUM NeXus ▸ `nx 感染（nxInfectio）`创建修改器。
3. 插件会自动创建第一个种子空物体，把它移动到传播起点。
4. 先保持默认搜索半径（Search Radius）和最大感染数（Max Infected），播放时间线观察颜色前沿。
5. 传播停住时提高搜索半径；传播过快时降低最大感染数或潜伏倍增（Incubation Multiplier）。

没有至少一个已启用的有效种子时，感染不会开始。

## 已启用（Enabled）

顶部已启用（Enabled）控制整套传播。种子（Seeds）列表中的已启用（Enabled）只控制当前起点，可以临时关闭某个种子而保留设置。

## 视口可见（Visible in Editor）

视口可见（Visible in Editor）控制种子范围等辅助图形。关闭只隐藏视口显示，感染计算仍会继续。

## 种子（Seeds）

每个种子条目对应一个作为修改器子级的空物体。移动空物体可以改变感染开始位置；添加多个种子可以让传播从多处同时展开。

### 半径（Radius）

半径（Radius）控制种子的初始感染范围，默认值是 `0.1 m`。提高后，开始时有更多粒子进入感染；降低后，感染从更小区域起步。

### 阈值（Threshold）

阈值（Threshold）控制种子触发粒子时需要达到的潜伏水平，默认值是 `0`。提高后，需要更高潜伏水平才会由该种子推进；降低后更容易触发。

### 颜色（Color）

种子中的颜色（Color）只改变视口辅助球体的显示颜色，方便区分多个起点。它不会改变感染粒子的颜色。

## 颜色

### 色彩模式（Color Mode）

色彩模式（Color Mode）默认是固定值（Fixed Value）。

- 固定值（Fixed Value）：潜伏中和已感染分别使用固定颜色
- 梯度（Gradient）：按潜伏进度从潜伏梯度左端过渡到右端
- 使用组（Use Groups）：状态变化时把粒子切换到指定 `nx 组（nxGroup）`
- 无颜色变化（No Color Change）：保留原粒子颜色

### 潜伏中（Incubating）和已感染（Infected）

两项只在固定值（Fixed Value）下出现，默认分别是蓝色和橙色。它们让潜伏阶段与完成感染阶段形成清楚的两段颜色。

### 潜伏梯度（Incubation Gradient）

潜伏梯度（Incubation Gradient）只在梯度（Gradient）下出现。左端对应刚开始潜伏，右端对应进入已感染，编辑中间色标可以改变传播前沿的颜色过渡。

### 潜伏组（Incubating Group）和感染组（Infected Group）

两项只在使用组（Use Groups）下出现。粒子开始潜伏时加入潜伏组，完成潜伏后加入感染组。两个组都需要指定，空缺的状态不会获得对应组颜色和后续组筛选。

### 组颜色更改（Group Color Change）

组颜色更改（Group Color Change）只在使用组（Use Groups）下出现，默认是两个阶段（Both Stages）。还可以只在未感染到潜伏阶段（Uninfected to Incubated Stage）或从潜伏阶段到感染阶段（Incubated to Infected Stage）更新组颜色，或选择无颜色变化（No Color Changes）。

## 搜索

### 搜索半径（Search Radius）

搜索半径（Search Radius）控制已感染粒子寻找未感染邻居的距离，默认值是 `0.1 m`。过小时可能找不到下一位邻居，传播会停住；提高后能跨过更大间隙，过大时感染会迅速穿过整个粒子群。

### 最大感染数（Max Infected）

最大感染数（Max Infected）限制每个已感染粒子在一步中最多传给多少个未感染粒子，默认值是 `3`。提高后传播分支增多、扩散更快；降低后前沿推进更慢。

### 感染寿命（Infected Lifespan）

感染寿命（Infected Lifespan）设置粒子进入已感染后获得的寿命，默认值是 `300`帧，可切换为秒。寿命结束后粒子死亡。

缩短后，感染前沿经过的位置会逐渐清空，形成移动环带；设为 `0`时不限制寿命，已感染粒子会保留并填满经过区域。

### 仅搜索一次最近项（Search for Nearest Once）

仅搜索一次最近项（Search for Nearest Once）默认开启。每颗已感染粒子只搜索一次邻居，计算更快，适合静止粒子。

移动粒子的邻居关系会持续变化，关闭后可以让后来进入范围的粒子继续被找到，同时增加搜索计算量。

### 约束搜索（Constrain Search）和限制（Limit）

约束搜索（Constrain Search）默认关闭。开启后，限制（Limit）分别约束 X、Y、Z 三轴的传播范围，默认都是 `1`，表示不限制。

降低某个轴的值会压缩该方向的搜索范围，使感染形成较扁或更定向的传播形状。

## 潜伏（Incubation）

### 潜伏模式（Incubation Mode）

潜伏模式（Incubation Mode）决定粒子潜伏速度从哪里取得，默认是使用粒子颜色（Use Particle Color）。

- 使用粒子颜色（Use Particle Color）：颜色亮度决定速度，黑色不会完成感染，白色完成得快
- 从潜伏率设置（Set From Incubation Rate）：全部粒子使用潜伏率（Incubation Rate）和变化（Variation）
- 粒子半径（Particle Radius）：把半径映射到最小值（Min Value）与最大值（Max Value）之间
- 粒子质量（Particle Mass）：把质量映射到最小值（Min Value）与最大值（Max Value）之间

### 最小值（Min Value）和最大值（Max Value）

两项只在粒子半径（Particle Radius）或粒子质量（Particle Mass）下出现，默认分别是 `0`和 `10`。属性处于最小值或更低时潜伏率为 `0`，粒子保持潜伏而不会完成感染；达到最大值或更高时潜伏率为 `100%`。

缩小两值间距会让属性变化更快覆盖完整潜伏率范围；扩大后不同粒子之间的速度过渡更缓。

### 潜伏率（Incubation Rate）和变化（Variation）

两项只在从潜伏率设置（Set From Incubation Rate）下出现，默认分别是 `50%`和 `25%`。提高潜伏率会缩短潜伏阶段；降低后粒子更久才变成已感染。提高变化会让各粒子的完成时间更分散。

### 潜伏倍增（Incubation Multiplier）

潜伏倍增（Incubation Multiplier）根据周围额外已感染邻居增加潜伏速度，默认值是 `1`。提高后，被多个感染邻居包围的粒子会更快完成潜伏；降低到 `1`以下会减慢这种邻居叠加效果。

### 反转（Invert）

反转（Invert）在潜伏模式（Incubation Mode）使用粒子颜色、粒子半径或粒子质量时出现，默认关闭。开启后，原本潜伏最快的属性值会变成最慢，原本最慢的会变成最快。

## 免疫（Immunity）

### 使用免疫（Use Immunity）

使用免疫（Use Immunity）默认关闭。开启后，免疫水平（Immunity Level）开始生效。

### 免疫水平（Immunity Level）

免疫水平（Immunity Level）把粒子的潜伏率作为抵抗判断，默认值是 `0`。潜伏率低于该水平的粒子不会被感染。提高后免疫粒子增加；降低后更多粒子可以参与传播。

## 组（Groups Affected）

组（Groups Affected）可以把传播限制到指定 `nx 组（nxGroup）`中的粒子。列表为空时处理全部粒子。

## 映射（Mapping）

映射（Mapping）可以使用粒子数据驱动感染参数，让不同粒子获得不同传播和潜伏行为。

## 衰减（Falloff）

衰减（Falloff）可以通过 `nx 衰减（nxFalloff）`限制感染传播的空间区域。

## 常见问题

### 感染从种子开始后很快停住

检查邻近粒子间距是否超过搜索半径（Search Radius）。移动粒子还应检查仅搜索一次最近项（Search for Nearest Once）是否需要关闭。

### 感染扩散过快

先降低最大感染数（Max Infected），再减小搜索半径（Search Radius）或潜伏倍增（Incubation Multiplier）。

### 粒子一直保持潜伏颜色

检查潜伏模式（Incubation Mode）对应的输入。使用粒子颜色（Use Particle Color）时黑色潜伏率为 `0`；半径或质量低于最小值（Min Value）时也不会完成感染。
