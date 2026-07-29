# nx 缓存（nxCache）

当前文档版本：NeXus `2026.0.0`

`nx 缓存（nxCache）` 把 NeXus 模拟结果记录到磁盘，并在播放时直接读取已存数据。缓存完成后可以前后拖动时间线，重新打开工程时也能继续读取同一结果。

缓存记录的是进入它的 NeXus 对象结果。发射、碰撞、流体或其他上游参数改变后，需要重新构建缓存，才能看到新结果。

## 已启用（Enabled）

已启用（Enabled）关闭后，`nx 缓存（nxCache）` 会停止读取和记录缓存，已有缓存设置和文件会继续保留。

## 界面结构

`nx 缓存（nxCache）` 位于物理属性（Physics Properties），包含三个页签：

- 构建（Build）：设置记录模式、文件格式、目录和数据通道，并执行构建或清空。
- 播放（Playback）：控制缓存如何随时间线回放。
- 包含（Inclusion）：限定哪些 NeXus 对象进入这份缓存。

已启用（Enabled）关闭后，缓存会停止读取和记录，同时保留对象与设置。

## 构建（Build）

### 缓存模式（Cache Mode）

缓存模式（Cache Mode）默认值为关闭（Off）。

- 关闭（Off）：停止读取和写入缓存。
- 播放（Playback）：从磁盘读取已缓存数据，播放时不重新求解这部分模拟。
- 记录（Record）：场景播放时把模拟数据写入缓存。

点击构建缓存（Build Cache）会自动完成整段记录，并在结束后切换到播放（Playback）。通常直接使用构建缓存（Build Cache）即可，无需先手动切到记录（Record）。

### 格式（Format）和缩放（Scale）

格式（Format）决定粒子缓存的文件格式，默认值为 NeXus 原生格式（NeXus）。当前插件还支持 Alembic 格式（Alembic）、X-Particles 格式（X-Particles）、Houdini 点缓存格式（BGEO/GEO）、粒子数据库格式（PDB/PDB32/PDB64/PDA）、RenderMan 点云格式（PTC）、Maya 点缓存格式（PDC）、Krakatoa 粒子格式（PRT）和 RealFlow 粒子格式（BIN）。

在 NeXus 内继续使用缓存时，优先选择 NeXus 原生格式（NeXus）。其他格式用于与对应外部软件交换数据。

缩放（Scale）默认值为 `1.0`，在读写粒子位置和速度时应用单位比例。导入外部软件后尺寸偏大时降低它，尺寸偏小时提高它；NeXus 内部读写通常保持 `1.0`。

### 记录粒子数据（Record Particle Data）

记录粒子数据（Record Particle Data）决定缓存保留哪些粒子通道，默认值为基础（Basic）。

- 基础（Basic）：记录位置、速度和基础粒子数据，适合常规回放。
- 全部（All）：记录所有可用通道，文件会更大。
- 自定义（Custom）：逐项选择需要的速度（Velocity）、颜色（Color）、质量（Mass）、燃料（Fuel）、旋转（Rotation）、时间（Time）、显示（Display）、组（Group）、温度（Temperature）、烟雾（Smoke）、缩放（Scale）、生命（Life）、标识（ID）、半径（Radius）和距离（Distance）数据。自定义模式默认只开启速度（Velocity）。

后续节点、实例或外部软件需要某个属性时，必须在构建前记录对应通道。缺失通道需要启用后重新构建。

### EFX 格式和通道

EFX 格式（EFX Format）用于缓存 ExplosiaFX 体素数据，默认值为 NeXus 原生格式（NeXus），还可以选择 OpenVDB 格式（OpenVDB）或 X-Particles EFX 格式（X-Particles EFX）。

EFX 缩放（EFX Scale）默认值为 `1.0`，控制体素缓存与外部软件之间的单位比例。

EFX 通道（EFX Channels）设置要写出的体素通道和通道名称（Channel Name）。烟雾（Smoke）和温度（Temperature）默认开启；燃料（Fuel）、速度（Velocity）和颜色（Color）默认关闭。导出 OpenVDB 格式（OpenVDB）时，可以把通道名称改成目标软件或渲染器要求的网格名称。

### 目录（Directory）

目录（Directory）决定缓存文件写入的位置，默认使用 NeXus 偏好设置中的缓存目录。

修改上游参数后仍看到旧结果时，先确认当前目录和缓存标识是否仍指向旧文件，再重新构建。工程需要移动到其他电脑或网络渲染时，也要确认这些机器能够访问同一目录。

### 文件命名（File Naming）

文件命名（File Naming）默认折叠，展开后包含：

- 缓存标识（Cache ID）：当前缓存的唯一子目录名。新建 `nx 缓存（nxCache）` 时会自动生成；旁边的刷新按钮会生成新的标识，避免与已有缓存共用文件。
- 扩展分隔符（Ext Separator）：放在对象名称与帧序号之间，默认值为下划线 `_`。
- 索引位数（Index Padding）：帧序号补零后的位数，范围为 `1` 到 `12`，默认值为 `6`。例如六位序号会显示成 `000001`。

### 压缩和内存

- 压缩缓存（Compress Cache）：默认开启，用额外的压缩与解压计算换取较小的磁盘文件。粒子数量很大时，构建和读取可能明显变慢；磁盘空间充足且读写速度更重要时，可以关闭后重新构建比较。
- 内存限制（Memory Limit (MB)）：缓存允许使用的最大内存，默认值为 `100 MB`，最小值为 `0`。提高后缓存可以在内存中保留更多数据，同时占用更多系统内存。

### 构建缓存（Build Cache）

构建缓存（Build Cache）会从头运行当前帧范围并记录结果，进度窗口会显示构建状态。缓存对象处于停用状态时，点击该按钮会先自动启用。完成后缓存模式（Cache Mode）会切换到播放（Playback）。

目标目录已有数据时，会出现三个选项：

- 覆盖（Overwrite）：替换现有缓存并重新记录。上游模拟参数已经改变时使用。
- 继续（Continue）：保留已有缓存，只补充缺失帧。已缓存帧不会按新参数重算。
- 取消（Cancel）：停止本次构建。

### 清空缓存（Empty Cache）

清空缓存（Empty Cache）会在确认后删除全部缓存帧并移除磁盘文件，操作无法撤销。来源（Sources）中已经锁定的项目也会被清空。

需要保留现有结果时，先改用新的缓存标识（Cache ID）或目录（Directory），再构建新缓存。

### 信息（Info）和来源（Sources）

信息（Info）显示最近一次构建的已缓存帧（Cached Frames）、已用内存（Memory Used）、压缩大小（Compressed Size）、未压缩大小（Uncompressed Size）、压缩比例（Compression Ratio）和完成时间（Time to Complete）。压缩大小与压缩比例只在压缩缓存（Compress Cache）开启时有意义。

来源（Sources）列表由 NeXus 根据进入缓存的对象自动生成：

- 已启用（Enabled）关闭后，这一来源不参与缓存。
- 已锁定（Locked）开启后，重新构建时保留这一来源已有的缓存数据，不重新记录。清空缓存（Empty Cache）仍会删除它。

## 播放（Playback）

### 重定时（Retiming）

重定时（Retiming）改变场景时间与缓存时间的对应关系，不会改写磁盘中的原始缓存，默认值为禁用（Disabled）。

- 禁用（Disabled）：按记录时序播放，并显示错位（Offset）和缩放（Scale）。
- 帧（Frame）：直接指定当前要读取的缓存帧。给帧（Frame）设置关键帧，可以暂停、跳帧、倒放或改变速度。
- 时间（秒）（Time(s)）：以秒指定当前缓存时间。给时间（Time）设置关键帧，可以按秒控制回放。
- 自定义（Custom）：使用时序（Timing）曲线。横轴是场景时间，纵轴是要读取的缓存帧；曲线上升更快时回放加速，水平段会停住，向下时会倒放。

### 错位（Offset）和缩放（Scale）

错位（Offset）和缩放（Scale）只在重定时（Retiming）设为禁用（Disabled）时出现。

- 错位（Offset）：默认值为 `0`，按帧整体移动缓存开始时间。提高后缓存结果在时间线上更晚出现，降低后更早出现。
- 缩放（Scale）：默认值为 `100%`。`200%` 会以两倍速度播放，`50%` 会以一半速度播放。

### 自动（Auto）和播放范围

自动（Auto）默认开启，直接使用场景的帧范围。关闭后可以手动设置：

- 开始（Start）：缓存开始参与播放的帧，默认值为 `0`。
- 停止（Stop）：缓存停止参与播放的帧，默认值为 `250`。

时间线超出这段范围后，缓存不再提供结果。

## 包含（Inclusion）

包含（Inclusion）页签中的包含/排除（Include/Exclude）列表限制哪些 NeXus 对象进入当前缓存。把对象加入列表后，可以用每项的已包含（Included）开关控制是否缓存该对象。

场景中只需要缓存部分发射器、求解器或生成结果时，在这里明确范围，可以减少无关数据。

## 常见问题

### 修改模拟参数后画面没有变化

检查缓存模式（Cache Mode）是否为播放（Playback）。旧缓存仍在回放时，使用覆盖（Overwrite）重新构建；继续（Continue）只会补缺失帧。

### 时间线拖动后没有缓存结果

检查已缓存帧（Cached Frames）、自动（Auto）或手动开始（Start）/停止（Stop）范围，以及当前缓存标识（Cache ID）和目录（Directory）。

### 缓存文件过大

先检查记录粒子数据（Record Particle Data）是否选了全部（All），再改为基础（Basic）或自定义（Custom）并只保留后续需要的通道。需要进一步节省磁盘时，开启压缩缓存（Compress Cache）后重新构建。
