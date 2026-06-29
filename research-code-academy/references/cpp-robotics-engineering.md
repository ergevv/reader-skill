# C++ Robotics Engineering

Use this reference for C++/ROS2 engineering analysis, Eigen, Ceres, GTSAM, PCL, OpenCV, threading, memory, realtime performance, build structure, and production robotics trade-offs.

## C++ Reading Checklist

Explain only the parts that matter to the user request:

- Ownership: value, pointer, reference, `shared_ptr`, `unique_ptr`, raw pointer, view/span.
- Lifetime: construction, initialization order, reset path, callback lifetime, thread lifetime.
- Mutability: `const`, mutable buffers, shared state, atomics, locks.
- Allocation: per-frame allocation, reserve/preallocate, copies of point clouds/images/matrices.
- Templates: why generic, compile-time constraints, type aliases, numeric scalar type.
- Error handling: return status, exceptions, assertions, logging, fallback behavior.
- API boundaries: public interface, private helpers, dependency injection, config contracts.
- Build/runtime constraints: CMake targets, ROS packages, launch/config files, hardware assumptions.

## ROS/ROS2 Reading Checklist

For ROS/ROS2 code, identify:

- Nodes/components and their responsibilities.
- Topics, services, actions, parameters, TF frames.
- Callback groups, executors, timers, subscriptions, publishers.
- QoS and queue sizes.
- Message timestamp semantics.
- Synchronization strategy: approximate/exact sync, custom queues, interpolation, locks.
- Launch/config path that changes behavior.
- Runtime failure modes: dropped messages, stale TF, queue overflow, callback starvation.

## Eigen

When Eigen appears, check:

- Matrix/vector dimensions and semantic meaning.
- Fixed-size vs dynamic-size allocation.
- Alignment issues for Eigen types in STL containers.
- `.noalias()`, expression templates, temporary creation.
- Quaternion normalization and coefficient order.
- Transform direction and multiplication order.

Explain Eigen code by math intent first, then implementation detail.

## Ceres

When Ceres appears, identify:

- Parameter blocks and their physical meaning.
- Residual blocks and measurement source.
- Local parameterization/manifold for rotations.
- Loss function and why it is used.
- Solver options: linear solver, trust region, iteration limits, threading.
- Cost scaling and unit consistency.
- Analytic vs automatic differentiation and debugging strategy.

Common Ceres risks:

- Wrong residual units or weight.
- Missing local parameterization for quaternion/SE3.
- Parameter block lifetime invalid.
- Over-parameterization or gauge freedom.
- Poor initial guess.

## GTSAM

When GTSAM appears, identify:

- Variables and keys.
- Factor types.
- Noise models.
- Between/Prior/IMU/Projection factors.
- `Values` initialization and update.
- Batch vs incremental solver.
- Marginalization or fixed-lag smoother behavior.

Explain factor graph meaning before API names.

## PCL And Point Clouds

Check:

- Point type fields: xyz, intensity, ring, time, normal, semantic label.
- Frame of the cloud.
- Motion distortion handling.
- Downsampling/voxel filters.
- Nearest-neighbor search structure.
- Map insertion/deletion.
- Copy and conversion costs.
- Invalid point filtering and NaN handling.

## OpenCV And Vision

Check:

- Image encoding and color space.
- Camera model and distortion.
- Feature tracking/matching.
- Pixel coordinate vs normalized coordinate.
- Timestamp sync with IMU/lidar.
- Memory copies between ROS/OpenCV/GPU.

## Performance And Realtime Analysis

Use this structure:

```markdown
## Performance Path
Hot path:

Complexity:

Allocation:

Threading/locks:

I/O/logging:

Expected bottleneck:

Measurement plan:
```

Common robotics bottlenecks:

- Nearest-neighbor search over growing maps.
- Per-point transforms without vectorization or downsampling.
- Repeated allocation of point clouds, vectors, matrices.
- Copying large images/clouds between callbacks.
- Holding locks during heavy computation.
- Visualization/logging in realtime loops.
- Solver iteration count under degenerate measurements.

## Architecture Analysis

When asked for architecture, explain:

- Boundaries: frontend/backend/map/IO/visualization/config.
- Data ownership: who owns state and map.
- Timing: realtime loop, async callbacks, batch jobs.
- Fault isolation: reset, degraded mode, sensor dropout.
- Extensibility: adding sensors, loop closure, relocalization, cloud map, multi-robot.
- Trade-offs: accuracy vs latency, global consistency vs realtime local odometry, memory vs map quality.

## Production Robotics Questions

For production-readiness, inspect:

- Deterministic startup and calibration validation.
- Config sanity checks.
- Logging and observability.
- Metrics for latency, queue size, dropped frames, residual statistics.
- Thread safety and shutdown.
- Dataset replay and regression tests.
- Failure recovery: relocalization, map reset, sensor dropout handling.
