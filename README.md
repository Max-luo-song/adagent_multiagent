# Nexus 多车交互与轨迹生成

本仓库保留 Nexus 的本地轨迹生成与交互改进，基于 OpenDriveLab/Nexus。原始说明、引用及第三方许可证见 `README_UPSTREAM.md` 和对应目录。

## 包含内容

- 道路与车辆历史条件、条件掩码、指定事件车未来轨迹。
- 推理期碰撞、道路与车辆运动学约束，巡航保速和跟车响应。
- 多车切入、跟驰传播和变道意图引导。
- 急刹、前车变道暴露静止车、后车快速逼近、对向侵入，以及事件触发重规划。
- 轨迹导出、物理与响应审计、BEV 示意和运动视频。

这些改进主要在推理与引导阶段，不表示重新训练了 Nexus 权重；意图引导后的行为不能直接作为原始模型自主决策的证据。

## 主要入口

| 内容 | 入口 |
| --- | --- |
| 原版安装与使用 | [原版说明](README_UPSTREAM.md)、[安装](docs/install.md)、[演示](docs/demo.md) |
| 本地复现与环境 | [复现说明](docs/reproduction_scene_gen.md)、`alf_compat_changes.json` |
| 交互与物理引导 | `nuplan_extent/planning/training/modeling/models/diffusion/omega_*.py` |
| 最新事件准备与生成 | `scripts/pipeline/prepare_hazard_response_v2.py`、`scripts/pipeline/run_hazard_response_v2.py` |
| 事件响应引导 | `scripts/pipeline/hazard_response_refiner.py` |
| 事件验收与示意视频 | `scripts/pipeline/audit_hazard_response_v2.py`、`scripts/pipeline/report_hazard_response_v2.py` |
| 多车变道生成 | `scripts/pipeline/run_nexus_lane_response_v2.py` |

## 交付范围

**3DGS 构建部分不在当前交付范围内。** Omniscenegraph 及其依赖、三维高斯重建/训练、资产处理、渲染脚本和对应文档已从当前版本移除。

`nexus_3dgs/` 沿用历史包名，当前仅保留 Nexus 主流程正在复用的状态编解码、坐标计算、条件编译及碰撞/交互审计工具；这些工具不需要高斯构建或渲染后端。保留原名称是为了兼容现有脚本导入。

## 环境与验证

按安装和复现说明准备 Nexus 环境、nuPlan 数据、地图、缓存和 checkpoint。历史脚本包含原服务器路径及固定案例，迁移后需要配置输入路径。环境、数据、权重、认证配置、密钥、API Token 和视频均未纳入代码交付。

在 Nexus 环境中可运行：

```bash
python -B scripts/pipeline/check_nexus_input_modes.py
python -B scripts/pipeline/check_nexus_timed_event.py
python -B scripts/pipeline/check_nexus_cruise_guidance.py
```

验证记录见 `VALIDATION.json`；第三方来源见 `DEPENDENCIES.lock.json`；当前文件哈希与来源见 `SOURCE_MANIFEST.json`、`SOURCE_ORIGINS.json`。源码与少量局部检查不替代完整数据集评估。
