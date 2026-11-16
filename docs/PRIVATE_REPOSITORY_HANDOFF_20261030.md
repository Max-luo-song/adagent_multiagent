# 私有代码仓交接（2026-10-30）

目标仓库：`https://github.com/keepgoing-xhz/nexus-driving-interaction`。应保持Private。此文件说明交付范围，不表示已经完成远端推送或成员邀请；实际状态以交付回执为准。

## 本次冻结范围

- Nexus核心、全部本地交互引导模块、数据准备/推理/审计/展示脚本。
- Nexus到Omniscenegraph的本地3DGS接入、道路/资产适配与B车四向渲染。
- 必要第三方源码、原许可证和已安装版本的来源记录；不携带嵌套Git历史。
- 实验文档、baseline缺点总结及少量数值审计摘要。

源码来自本地工作区当前状态，包含未提交的开发改动，不等同于上游Nexus的干净版本。`DEPENDENCIES.lock.json`记录各上游提交及跟踪文件改动，`SOURCE_ORIGINS.json`记录原始源码文件哈希，`SOURCE_MANIFEST.json`记录交付文件哈希。任何敏感值清理只发生在交付副本中。完整旧实验与原数据保持本地留存。

## 不放入Git的内容

API key、Token、私钥、实际认证配置、`.env`、终端日志、命令历史、旧Git对象、模型权重、nuPlan数据与缓存、PLY资产、编译后的动态库、虚拟环境、训练结果和展示视频均排除。Notebook代码可以保留，但移除输出、执行次数和附件。代码库不要求配置商业模型API key以运行现有Nexus推理链路。

采用全新初始提交交付，不将当前上游仓历史推送到新仓。顶层和依赖目录原有LICENSE、NOTICE继续保留；私有可见性不改变第三方许可。源码筛选和敏感模式检查有明确覆盖范围，规则通过不能保证检测所有未知形式的敏感信息。

## 快速定位

| 目的 | 入口 |
| --- | --- |
| Nexus原版安装/示例 | `README_UPSTREAM.md`、`docs/install.md`、`docs/demo.md` |
| 本地复现与环境兼容 | `docs/reproduction_scene_gen.md`、`alf_compat_changes.json` |
| 条件输入模式及未来事件 | `docs/nexus_input_modes_comparison_20261026.md`、`docs/nexus_timed_event_implementation_20261026.md` |
| 三车跟驰传播 | `docs/nexus_chain_interaction_20261027.md` |
| C变道绕行 | `docs/nexus_lane_change_response_20261027.md`、`scripts/pipeline/run_nexus_lane_response_v2.py` |
| 3DGS环境与数据准备 | `docs/nexus_3dgs.md`、`scripts/pipeline/nexus_3dgs.sh` |
| 外部资产与新道路 | `docs/nexus_threecar_asset_insert_20261028.md` |
| B车四路相机 | `docs/nexus_threecar_B_multiview_20261028.md` |
| Baseline主要缺点和后续方向 | `docs/BASELINE_LIMITATIONS_20261030.md` |

## 环境、权重与数据单独恢复

原开发采用Python 3.9、CUDA版PyTorch和两个相互隔离的Nexus/3DGS环境。先阅读安装及本地复现说明；`requirements.txt`、`scripts/pipeline/nexus_3dgs_requirements.txt`与ALF兼容记录共同描述依赖。ALF、MTR等原本预编译的`.so`不交付，需要在目标机器从所保留的源码重新编译。系统动态库和PyTorch3D兼容要求见3DGS环境说明。

nuPlan数据、地图、缓存和Nexus checkpoint按原复现文档另行准备。资产插入演示还需要相应道路背景检查点和外部车辆PLY。不能只克隆代码就声称复现了包含数据和权重的视频。

许多开发脚本以仓库位置计算ROOT，但部分历史实验脚本和配置记录了原服务器路径、日期目录、固定案例索引。迁移机器时先检查路径和输入是否存在，按文档重新生成准备产物或在原机器恢复本地实验目录。历史绝对路径作为来源记录保留，不能当作其他机器上的可用下载链接。

本次不训练Nexus、不重新采样、不改变已有实验结论；未声称已在全新环境完成端到端复现测试。

## 推送与邀请

先确认目标仓库Private及服务器SSH授权可用，再将经过检查的独立快照推送到该仓库。工作区仍保留上游`OpenDriveLab/Nexus`的remote，避免在开发仓上误推全部历史、缓存或不相关改动。

SSH授权仅支持Git传输，不自动赋予GitHub网页/API管理会话。邀请成员需要仓库Owner在Settings → Collaborators添加截图中的邮箱，或通过已授权的GitHub API身份执行。成员邀请状态必须实际核实，不能将“已推送代码”当作“对方已接受邀请”。
