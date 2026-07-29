# NeXus for Blender 中文帮助文档

这里是 `NeXus for Blender` 的新版中文帮助文档仓库。  
本文档为重新优化版本，用更直观、更通俗的方式描述每个功能和使用含义。  
当前仓库只整理新版帮助文档，不包含旧版归档、备份文件和辅助脚本。

## 当前编写基准

- 当前 NeXus 插件版本：`2026.0.0`
- 当前 Blender 便携版目录：`E:\blender-5.2.0-windows-x64`
- 当前插件本体路径：`E:\blender-5.2.0-windows-x64\portable\extensions\extensions_insydium_ltd\insydium_nexus`
- [NeXus 官方 Blender 帮助文档](https://docs.insydium.ltd/blender/)

功能名称、默认值、参数范围和条件显示会同时核对官方 Blender 帮助文档与当前插件本体。两者存在差异时，正文以当前目标版本的插件本体为准。过时内容会直接更新，不在普通用户正文中保留旧行为。

## 快速索引

- [新版总览与编写入口](./新版Nexus帮助文档/00_总览与编写入口/README.md)
- [修改器文档总目录](./新版Nexus帮助文档/01_修改器/README.md)
- [面板与功能总目录](./新版Nexus帮助文档/02_面板与功能/README.md)
- [流程与排错总目录](./新版Nexus帮助文档/03_流程与排错/README.md)
- [术语与对齐总目录](./新版Nexus帮助文档/04_术语与对齐/README.md)
- [待确认与后续补充](./新版Nexus帮助文档/05_待确认与后续补充/README.md)

## 已写正文直达

- [nx 发射器（nxEmitter）](./新版Nexus帮助文档/01_修改器/按修改器/nxEmitter/README.md)
- [nx 流场修改器（nxFlowField）](./新版Nexus帮助文档/01_修改器/按修改器/nxFlowField/README.md)
- [nx 重力（nxGravity）](./新版Nexus帮助文档/01_修改器/按修改器/nxGravity/README.md)
- [nx 风力（nxWind）](./新版Nexus帮助文档/01_修改器/按修改器/nxWind/README.md)
- [nx 湍流（nxTurbulence）](./新版Nexus帮助文档/01_修改器/按修改器/nxTurbulence/README.md)
- [nx 涡度（nxVorticity）](./新版Nexus帮助文档/01_修改器/按修改器/nxVorticity/README.md)
- [nx 阻力（nxDrag）](./新版Nexus帮助文档/01_修改器/按修改器/nxDrag/README.md)
- [nx 限制（nxLimit）](./新版Nexus帮助文档/01_修改器/按修改器/nxLimit/README.md)
- [nx 速度（nxSpeed）](./新版Nexus帮助文档/01_修改器/按修改器/nxSpeed/README.md)
- [nx 推力（nxPush）](./新版Nexus帮助文档/01_修改器/按修改器/nxPush/README.md)
- [nx 吸引（nxAttract）](./新版Nexus帮助文档/01_修改器/按修改器/nxAttract/README.md)
- [nx 避让（nxAvoid）](./新版Nexus帮助文档/01_修改器/按修改器/nxAvoid/README.md)
- [nx 爆炸（nxExplode）](./新版Nexus帮助文档/01_修改器/按修改器/nxExplode/README.md)
- [nx 方向（nxDirection）](./新版Nexus帮助文档/01_修改器/按修改器/nxDirection/README.md)
- [nx 自旋（nxSpin）](./新版Nexus帮助文档/01_修改器/按修改器/nxSpin/README.md)
- [nx 缓存（nxCache）](./新版Nexus帮助文档/01_修改器/按修改器/nxCache/README.md)
- [nx 碰撞体（nxCollider）](./新版Nexus帮助文档/01_修改器/按修改器/nxCollider/README.md)
- [nx 流体（nxFluids）](./新版Nexus帮助文档/01_修改器/按修改器/nxFluids/README.md)
- [nx 粒子-粒子碰撞（nxPPCollisions）](./新版Nexus帮助文档/01_修改器/按修改器/nxPPCollisions/README.md)
- [nx 网格化（nxMesher）](./新版Nexus帮助文档/01_修改器/按修改器/nxMesher/README.md)
- [nx 泡沫（nxFoam）](./新版Nexus帮助文档/01_修改器/按修改器/nxFoam/README.md)
- [nx 拖尾（nxTrail）](./新版Nexus帮助文档/01_修改器/按修改器/nxTrail/README.md)
- [文档（Document）- 子步（Substeps）](./新版Nexus帮助文档/02_%E9%9D%A2%E6%9D%BF%E4%B8%8E%E5%8A%9F%E8%83%BD/%E5%AF%B9%E8%B1%A1%E4%B8%8E%E4%BF%AE%E6%94%B9%E5%99%A8%E7%95%8C%E9%9D%A2/%E6%96%87%E6%A1%A3%EF%BC%88Document%EF%BC%89-%20%E5%AD%90%E6%AD%A5%EF%BC%88Substeps%EF%BC%89.md)

## 当前目录

- `新版Nexus帮助文档`
  新版帮助文档主目录。

## 新版文档结构

- `00_总览与编写入口`
  新版帮助文档的总入口和整体说明。

- `01_修改器`
  按修改器整理的正文，以及按主题整理的共性内容。

- `02_面板与功能`
  修改器之外的重要界面、页签和功能区说明。

- `03_流程与排错`
  常见使用线路、联动关系、误解点和排查思路。

- `04_术语与对齐`
  文档写作时使用的术语基准和对齐说明。

- `05_待确认与后续补充`
  仍需继续核对和补写的内容。

## 当前已编写的正文

- `nx 发射器（nxEmitter）`
- `nx 流场修改器（nxFlowField）`
- `nx 重力（nxGravity）`
- `nx 风力（nxWind）`
- `nx 湍流（nxTurbulence）`
- `nx 涡度（nxVorticity）`
- `nx 阻力（nxDrag）`
- `nx 限制（nxLimit）`
- `nx 速度（nxSpeed）`
- `nx 推力（nxPush）`
- `nx 吸引（nxAttract）`
- `nx 避让（nxAvoid）`
- `nx 爆炸（nxExplode）`
- `nx 方向（nxDirection）`
- `nx 自旋（nxSpin）`
- `nx 缓存（nxCache）`
- `nx 碰撞体（nxCollider）`
- `nx 流体（nxFluids）`
- `nx 粒子-粒子碰撞（nxPPCollisions）`
- `nx 网格化（nxMesher）`
- `nx 泡沫（nxFoam）`
- `nx 拖尾（nxTrail）`
- `文档（Document）- 子步（Substeps）`
