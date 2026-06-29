# Examples And Roadmap

Use this reference for project-specific routing and staged development of the skill itself.

## Example Project Focus

FAST-LIO2 / FAST-LIVO2:

- Start from launch/config and `laserMapping`/mapping entrypoints.
- Focus on lidar preprocessing, point undistortion, ESKF update, ikd-tree map maintenance, nearest-neighbor search, residual construction, realtime constraints.
- Explain why ikd-tree is used and how map insertion/deletion keeps latency bounded.

Basalt:

- Focus on VIO pipeline, IMU preintegration, optical flow, sliding-window optimization, marginalization, FEJ/consistency, bundle adjustment.
- Map paper symbols to residual blocks and state variables.

VINS / VINS-Fusion:

- Focus on feature tracking, initialization, IMU preintegration, visual-inertial alignment, marginalization, loop closure.
- Explain observability and gauge freedom carefully.

LIO-SAM / Cartographer:

- Focus on factor graph / pose graph, loop closure, scan matching, IMU/GPS factors, map optimization.

Autoware / Apollo:

- Focus on system architecture, ROS/ROS2 graph, localization/perception/planning/control boundaries, message contracts, realtime and deployment concerns.

GTSAM / Ceres:

- Focus on factor/cost construction, variables, residuals, Jacobians, local parameterization, solver configuration, marginalization/fixed-lag behavior.

## User Prompt Examples

Prompts that should trigger this skill:

- "分析这个仓库，像教材一样讲清楚。"
- "解释 FAST-LIO2 的 laserMapping.cpp。"
- "阅读 `src/vio.cpp`，在 `fast_livo2_learning` 生成一个 Markdown blog，要求脱离代码讲清楚 VIO。"
- "这个类每个成员变量为什么存在？"
- "把这段 ESKF 更新代码和论文公式对应起来。"
- "定位发散，帮我从源码角度排查。"
- "帮我生成这个模块的高级面试题。"
- "给我下一步阅读顺序。"
- "分析 Autoware 的 localization 架构。"

## Skill Expansion Roadmap

Maintain this skill in stages:

1. System: keep `SKILL.md` concise and route to references.
2. Output: add or refine report/debug/interview templates.
3. Repository: expand module/class/function/thread/performance reading rules.
4. C++: add deeper C++20, templates, memory model, concurrency, STL notes.
5. Math: add linear algebra, probability, optimization, Lie group, Jacobian notes.
6. SLAM: add ESKF, FEJ, marginalization, observability, IMU preintegration, ICP, NDT.
7. Frameworks: add ROS2, Eigen, Ceres, GTSAM, PCL, OpenCV.
8. Examples: add project-specific examples after analyzing real code.
9. Interview: add graded question banks derived from real repositories.

When adding new material, avoid duplicating content already present in `SKILL.md`; place details in the most specific reference file.
