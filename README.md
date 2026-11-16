# Nexus 多车交互与 3DGS

本仓库交付截至 **2026-11-16** 的 Nexus 本地改进源码，包括条件轨迹生成、推理期交互与运动学引导、危险事件响应，以及轨迹到 3DGS 多视角渲染的接入。基于 OpenDriveLab/Nexus，保留原始说明、引用及各依赖许可证。

## 改进内容

- 条件掩码与指定事件轨迹；自车和响应车辆未来可保持待生成。
- 碰撞、道路、车辆运动学约束，巡航保速与跟车响应。
- 多车切入、跟驰传播、变道意图引导。
- 急刹、前车变道暴露静止车、后车快速逼近、对向侵入等事件；支持事件触发重规划。
- 轨迹审计、三维坐标与道路适配、外部车辆高斯资产插入、车辆跟随与多视角渲染。

改进主要发生在推理与轨迹引导阶段，并不表示重新训练了 Nexus 权重；明确指定的行为意图不能当作原始模型自主决策的证据。

## 从哪里开始

| 内容 | 入口 |
| --- | --- |
| 原 Nexus 安装与说明 | [README_UPSTREAM.md](README_UPSTREAM.md)、[安装](docs/install.md)、[演示](docs/demo.md) |
| 复现环境及兼容修改 | [复现说明](docs/reproduction_scene_gen.md)、`alf_compat_changes.json` |
| 运动学、巡航、多车引导 | `nuplan_extent/planning/training/modeling/models/diffusion/omega_*.py` |
| 最新事件响应 | `scripts/pipeline/prepare_hazard_response_v2.py`、`run_hazard_response_v2.py`、`hazard_response_refiner.py`、`audit_hazard_response_v2.py` |
| 三维桥接与资产驱动 | `nexus_3dgs/`、[3DGS 环境说明](docs/nexus_3dgs.md) |
| 事件轨迹的高斯渲染 | `scripts/pipeline/prepare_response_gaussians.py`、`fit_response_gaussian_lanes.py`、`render_response_gaussians.py` |
| BEV 到 3DGS 技术路线 | [技术路线详解](docs/BEV条件到3DGS场景_技术路线详解.md) |

## 恢复与检查

按照安装和复现文档准备 Nexus 环境；3DGS 使用独立环境。依赖版本及第三方来源见 `DEPENDENCIES.lock.json`。vendored 第三方源码中的原有 LICENSE/NOTICE 保持保留，编译扩展需要在目标环境重新构建。

数据、地图、Nexus checkpoint、三维背景 checkpoint 和车辆高斯 PLY **需要另行准备**。历史实验脚本包含原服务器路径及固定案例，运行前需配置路径或重新执行准备步骤，不能只克隆源码就复现所有视频。

在配置完成的 Nexus 环境中，可运行：

```bash
python -B scripts/pipeline/check_nexus_cruise_guidance.py
python -B scripts/pipeline/check_nexus_replay.py
```

前者检查巡航保速、切入响应及条件保留；后者检查三维回放的坡面与安装旋转恢复。它们是局部回归检查，不替代完整数据集和 GPU 渲染验证。

## 代码交付范围

使用独立源码快照，不携带原仓或嵌套依赖的 Git 历史；目标为 `Max-luo-song/adagent_multiagent` 的 `main` 分支。源码包含本地未提交的开发改进，原开发工作区保持原样。

认证文件、API key、Token、私钥、环境、模型权重、数据缓存、车辆资产、视频和编译二进制不在交付范围。Notebook 仅保留去除输出后的代码。`SECURITY_AUDIT.json` 记录规则检查结果，`SOURCE_ORIGINS.json` 和 `SOURCE_MANIFEST.json` 记录来源及交付文件哈希。
