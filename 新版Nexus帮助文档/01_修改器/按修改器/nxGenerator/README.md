# nx 生成器（nxGenerator）

当前文档版本：NeXus `2026.0.0`

`nx 生成器（nxGenerator）`把网格模型实例放到每颗有效粒子的位置上，让粒子从视口中的点变成可以观察和渲染的模型。实例可以读取粒子的颜色、缩放和旋转，也可以在多个网格图层之间按比例混合。

## 添加和起步

1. 从添加（Add） ▸ INSYDIUM NeXus ▸ `nx 生成器（nxGenerator）`创建生成器。
2. 先把显示模式（Display Mode）保持为预览（Preview），确认粒子位置和实例分布。
3. 在生成器层（Generator Layers）中添加网格对象。
4. 需要渲染或让实例参与碰撞时，把显示模式（Display Mode）切换为几何体（Geometry）。
5. 再按需要调整颜色、缩放、旋转和动画设置。

## 生成器设置

### 已启用（Enabled）

已启用（Enabled）控制实例生成。关闭后，实例停止显示和更新，图层、源发射器和参数仍会保留。

### 显示模式（Display Mode）

显示模式（Display Mode）默认预览（Preview）。

- 预览（Preview）：只在视口中提供快速的图形处理器（GPU）预览，实例对碰撞体、其他修改器和渲染引擎不可见
- 几何体（Geometry）：把实例创建为真实的 Blender 几何体，实例可以被碰撞体和其他修改器读取，也可以进入渲染

预览（Preview）适合调位置、数量和外观，响应更快。需要生成最终画面时必须使用几何体（Geometry）。切换到几何体（Geometry）后，视口和渲染都会处理真实实例，场景负担也会增加。

## 源发射器（Source Emitters）

源发射器（Source Emitters）列表限制生成器读取哪些 `nx 发射器（nxEmitter）`的粒子。列表为空时，会读取场景中所有发射器的粒子；加入一个或多个发射器后，只处理列表中的来源。

每个源发射器条目都有已启用（Enabled）开关。关闭单个条目后，该发射器暂时不会提供实例，列表中的其他发射器继续生效。

如果场景中有多股粒子流，建议先限制源发射器（Source Emitters），再调整网格层，这样更容易判断每层实例来自哪一股粒子。

## 生成器层（Generator Layers）

生成器层（Generator Layers）中的每一项代表一种要实例化的网格。每层可以单独设置网格、生成比例、颜色、缩放、旋转和动画。

使用加号添加图层，减号删除图层，上下箭头调整列表顺序。列表中的已启用（Enabled）控制当前层是否参与；选中某层后，下方会显示该层设置。

### 网格（Mesh）

网格（Mesh）指定当前层要放到粒子上的模型。当前插件只接受网格对象。没有指定网格时，该层没有可生成的实例。

### 生成（Spawn）

生成（Spawn）是当前层分到的粒子比例。单层时会使用 `100%`；添加多个已启用层后，所有层的生成比例总和保持为 `100%`。

提高某层生成（Spawn）后，该层获得更多粒子，其他可调整层会按剩余比例重新分配。降低后，该层出现得更少。生成比例为 `0%`时，该层不会分到粒子。

### 锁定（Lock）

锁定（Lock）默认关闭。开启后，拖动其他层的生成（Spawn）滑块时，该层比例保持不变，剩余比例只在其他已启用且未锁定的层之间重新分配。仍然可以直接拖动被锁定层自己的滑块。

当某一类模型需要始终占固定比例时，可以锁定它，再调整其他层的分配。

## 颜色（Color）

颜色（Color）分组控制实例的着色方式。

### 着色（Shading）

着色（Shading）默认使用默认（Default），保留源网格每个面的平直和平滑着色标记。平直（Flat）会强制所有面使用平直着色；平滑（Smooth）会通过顶点法线平均强制平滑着色。

源模型边缘需要保持清晰时，可以使用平直（Flat）；模型需要连续高光时，可以使用平滑（Smooth）。

### 颜色来源（Color Source）

颜色来源（Color Source）默认自定义（Custom）：读取当前层的颜色（Color）。还可选择网格（Mesh）或粒子（Particle）。

- 自定义（Custom）：使用当前层设置的颜色
- 网格（Mesh）：使用源网格对象的视口颜色
- 粒子（Particle）：读取模拟中的逐粒子颜色，例如 `nx 颜色（nxColor）`输出的颜色

### 颜色（Color）

颜色（Color）默认蓝色，只在颜色来源（Color Source）为自定义（Custom）时直接决定实例颜色。切换到网格（Mesh）或粒子（Particle）后，实例颜色来自对应来源。

### 颜色变化量（Color Variation）

颜色变化量（Color Variation）默认 `0%`，为每个实例加入随机颜色变化。提高后同层实例的颜色差别更明显；保持 `0%`时所有实例使用相同基础颜色或来源颜色。

每通道颜色变化（Per-channel Color Variation）默认关闭。关闭时使用一个总变化量；开启后分别显示红、绿、蓝（RGB）三个通道的变化量，可以只改变某个颜色通道。

### 实例材质（Instance Material）

实例材质（Instance Material）指定这一层实例使用的材质。留空时沿用源网格已有材质。使用添加材质按钮可以创建一个已经连接逐实例属性的起始材质，让粒子颜色和颜色变化量在渲染中可见。

## 缩放（Scale）

缩放（Scale）分组决定实例模型的尺寸。

### 缩放源（Scale Source）

缩放源（Scale Source）默认粒子半径（Particle Radius）。可选来源包括自定义（Custom）、网格缩放（Mesh Scale）、粒子半径（Particle Radius）和粒子缩放（Particle Scale）。

- 自定义（Custom）：使用当前层的缩放值
- 网格缩放（Mesh Scale）：读取源网格对象自身的变换缩放
- 粒子半径（Particle Radius）：读取粒子半径，并在三个轴上统一使用
- 粒子缩放（Particle Scale）：读取模拟中每颗粒子的三轴缩放

需要让实例模型随 `nx 缩放（nxScale）`的粒子缩放值变化时，选择粒子缩放（Particle Scale）。只需要让模型随粒子物理半径统一变大或变小时，选择粒子半径（Particle Radius）。

### 每轴缩放（Per-axis Scale）和缩放（Scale）

每轴缩放（Per-axis Scale）只在缩放源（Scale Source）为自定义（Custom）时使用。关闭时显示一个统一缩放值；开启后分别设置 X 轴（X）、Y 轴（Y）、Z 轴（Z）三轴缩放。

统一缩放默认 `1.0`。提高后实例整体变大；降低后整体变小。三轴缩放可以把模型拉长、压扁或改变比例。

### 缩放变化量（Scale Variation）

缩放变化量（Scale Variation）默认 `0%`，给每颗粒子加入随机尺寸变化。提高后同层实例的大小差异更明显；保持 `0%`时实例尺寸一致。

每轴缩放变化（Per-axis Scale Variation）默认关闭。开启后可以分别控制 X 轴（X）、Y 轴（Y）、Z 轴（Z）的随机变化，适合让长度变化和厚度变化使用不同幅度。

## 旋转（Rotation）

旋转（Rotation）分组决定实例朝向。

### 旋转源（Rotation Source）

旋转源（Rotation Source）默认粒子（Particle），可选自定义（Custom）、网格（Mesh）和粒子（Particle）。

- 自定义（Custom）：使用当前层的旋转值
- 网格（Mesh）：读取源网格对象自身的变换旋转
- 粒子（Particle）：读取模拟中每颗粒子的旋转

发射器中的方向或旋转显示主要用于观察发射状态。需要实例在运动过程中持续改变真实朝向时，应使用 `nx 自旋（nxSpin）`写入粒子旋转，再让旋转源（Rotation Source）读取粒子（Particle）。

### 前进轴（Forward Axis）

前进轴（Forward Axis）只在旋转源（Rotation Source）为粒子（Particle）时出现，默认正向 Y 轴（+Y）。它决定网格的哪个局部轴指向粒子运动方向，可选正向 X 轴（+X）、负向 X 轴（-X）、正向 Y 轴（+Y）、负向 Y 轴（-Y）、正向 Z 轴（+Z）和负向 Z 轴（-Z）。

模型的长轴与粒子前进方向不一致时，切换前进轴（Forward Axis），例如让箭头或鱼模型的头部朝向运动方向。

### 每轴旋转（Per-axis Rotation）和旋转（Rotation）

每轴旋转（Per-axis Rotation）只在旋转源（Rotation Source）为自定义（Custom）时使用。关闭时设置一个统一旋转；开启后分别设置 X 轴（X）、Y 轴（Y）、Z 轴（Z）三轴旋转。

旋转变化量（Rotation Variation）默认 `0%`，为每个实例加入随机转向。提高后实例朝向差异更明显；保持 `0%`时同层实例使用相同来源旋转。

每轴旋转变化（Per-axis Rotation Variation）开启后，可以分别设置三个轴向的随机变化。它适合让某个方向保持稳定，同时只增加另外两个方向的随机转动。

## 动画（Animation）

### 冻结动画（Freeze Animation）

冻结动画（Freeze Animation）默认关闭。开启后，当前层会记录源网格在切换时的场景帧，并让所有实例保持这个网格姿态。源网格继续变形或播放动画时，实例不会跟随更新。

冻结帧（Frozen Frame）显示记录的帧号，当前界面中用于查看冻结结果。刷新按钮会重新读取源网格当前帧的姿态，适合在修改源模型后更新实例外形。

## 组（Groups Affected）

组（Groups Affected）可以把 `nx 生成器（nxGenerator）`限制到指定 `nx 组（nxGroup）`中的粒子。列表为空时处理所有符合源发射器（Source Emitters）条件的粒子；加入组后，只在这些组上生成实例。

## 常见问题

### 视口中有实例，渲染结果没有

检查显示模式（Display Mode）是否仍为预览（Preview）。切换到几何体（Geometry）后，实例才会成为渲染引擎可读取的真实几何体。

### 实例颜色已经改变，渲染材质仍没有颜色变化

把颜色来源（Color Source）设为粒子（Particle），并检查实例材质（Instance Material）是否读取逐实例颜色属性。`nx 颜色（nxColor）`只负责写入粒子颜色，生成器材质还需要读取这份数据。

### 实例没有朝向运动方向

确认旋转源（Rotation Source）设为粒子（Particle），前进轴（Forward Axis）与模型长轴一致，并检查模拟中是否已有 `nx 自旋（nxSpin）`提供逐帧旋转。

### 多个模型比例不符合预期

检查各层生成（Spawn）和锁定（Lock）状态。已启用且未锁定的层会共同分配剩余比例；锁定层会先占用固定比例，其他层只分配剩余部分。
