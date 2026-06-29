# Repository Analysis

Use this reference when the user asks to read a repository, module, file, class, function, variable, thread model, dependency, performance path, or debugging path.

## Repository Triage

Inspect in this order:

1. Top-level files: README, docs, package manifests, build scripts, launch/config files, `CMakeLists.txt`, `package.xml`, `setup.py`, `.repos`, Docker files.
2. Directory roles: source, include, nodes, algorithms, messages, launch, config, tests, examples, third-party, tools.
3. Entrypoints: `main`, ROS/ROS2 nodes, nodelets/components, launch files, executables in CMake, scripts, benchmark/test binaries.
4. Domain anchors: estimator, mapping, tracker, frontend, backend, optimizer, preprocessor, synchronizer, calibrator, planner, controller.
5. Data contracts: messages, structs, state classes, residual blocks, factor types, callbacks, queues, buffers, configs.

When the repository is large, produce a staged reading plan and analyze only the first target unless the user requested a complete survey.

## Repository Report

Use this shape for broad analysis:

```markdown
## System Overview
- 一句话定位
- 输入/输出
- 运行形态
- 核心算法

## Architecture
Mermaid graph:
graph TD
  Sensor --> Preprocess
  Preprocess --> Estimator
  Estimator --> Map

## Module Relationship
| Module | Responsibility | Key Files | Inputs | Outputs |

## Call Graph
Explain who calls whom and why.

## Data Flow
Explain payload, timestamp, coordinate frame, queue/buffer, and ownership.

## Control Flow
Explain lifecycle, callbacks, loops, initialization, reset, failure recovery.

## Key State
Explain physical meaning, update timing, frame, units, and consumers.

## Reading Roadmap
Give the next 3-7 files/functions in order.
```

## Module Analysis

For a module, answer:

- What problem does it solve in the whole system?
- Which upstream modules feed it?
- Which downstream modules consume it?
- Which state does it own?
- Which state does it only borrow or cache?
- What is its lifecycle: initialize, update, reset, shutdown?
- What invariants must remain true?
- What assumptions are hidden in configs or comments?
- Where can it fail?

Prefer a short table for files:

```markdown
| File | Role | Read First? | Why |
```

## Class Analysis

For a class, explain:

- Responsibility: what concept it represents.
- Ownership: what resources, buffers, threads, handles, maps, or factors it owns.
- State: member variables grouped by semantic role, not declaration order.
- Construction: required dependencies, default values, config loading.
- Public API: commands, queries, callbacks, lifecycle methods.
- Invariants: valid ranges, frame conventions, sorted queues, initialized flags.
- Failure modes: stale data, null pointers, time jumps, race conditions, numerical instability.

Use this compact template:

```markdown
## Class: `Name`
一句话作用：

### Position In System

### State Groups
| Members | Meaning | Lifecycle | Updated By | Used By |

### Main Methods
| Method | Intent | Calls | Risk |

### Design Notes

### Debug Notes
```

## Function Analysis

For a function, explain logical blocks instead of every line:

1. Intent: one sentence.
2. Inputs/outputs: include ownership, mutation, frame, unit, timestamp.
3. Preconditions: required initialized state, sorted buffers, valid calibration, config.
4. Algorithm blocks: group loops and branches into meaningful phases.
5. Math meaning: residual, Jacobian, optimization, prediction, matching, interpolation, transform.
6. Complexity: rough time and memory complexity.
7. Calls: upstream caller and downstream callees.
8. Failure modes: empty input, numerical degeneracy, bad timestamp, race, allocation spikes.
9. Next reading: the best next symbol/file and why.

Template:

```markdown
## Function: `foo`
一句话作用：

### Inputs And Outputs

### Algorithm Blocks
1. ...

### Mathematical Meaning

### Engineering Meaning

### Complexity

### Failure Modes

### Next Reading
```

## Variable Analysis

Never stop at type names. For every important variable or member, explain:

- Semantic meaning: physical quantity, cache, handle, config, flag, residual, Jacobian.
- Coordinate frame: world, map, odom, body, IMU, lidar, camera, sensor, local tangent.
- Units: meters, radians, seconds, covariance, pixels, normalized coordinates.
- Lifecycle: initialized where, updated where, reset where, consumed where.
- Ownership and mutability: owned, borrowed, shared, view, pointer, reference.
- Validity: range, empty state, timestamp ordering, frame consistency.

Example:

```markdown
`R_w_i` is not "a matrix"; it is the rotation from IMU frame to world frame. It is predicted by IMU integration, corrected by the estimator update, and consumed by point undistortion and pose publishing.
```

## Debug Reading

Map debug questions with:

```markdown
Symptom -> Likely Causes -> Evidence To Collect -> Code Locations -> Checks -> Fix Direction
```

Common robotics symptoms:

- Localization diverges: timestamp sync, extrinsics, IMU noise, bias initialization, wrong gravity, wrong frames, degenerate geometry.
- Map blurs: motion compensation, transform direction, lidar time field, voxel/downsample settings.
- Optimizer fails: bad residual scale, wrong Jacobian sign, robust loss, ill-conditioned Hessian, insufficient constraints.
- Realtime drops: nearest-neighbor search, repeated allocation, large copies, logging, lock contention, visualization.
- Intermittent jumps: race condition, stale transform, delayed callback, queue overflow, reset path.

## Staged Reading Plan

When asked to read a large project, propose stages:

1. Build/run surface and entrypoints.
2. Data ingestion and synchronization.
3. State representation and coordinate frames.
4. Frontend measurement construction.
5. Estimator/optimizer update.
6. Map/state publication.
7. Debug/performance path.
8. Extension or paper mapping.
