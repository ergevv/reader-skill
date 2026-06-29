# Robotics Math And SLAM

Use this reference for ESKF/EKF, factor graphs, VIO/LIO/SLAM, IMU preintegration, Lie groups, Jacobians, marginalization, FEJ, observability, ICP/NDT, scan matching, bundle adjustment, and paper-to-code mapping.

## Math Explanation Standard

For every formula or algorithm:

1. State the problem in words.
2. Define symbols before equations.
3. State coordinate frames and perturbation convention.
4. Derive the formula at the level needed by the user.
5. Map each symbol to code variables/functions.
6. Explain physical meaning.
7. Explain numerical or engineering consequences.

Avoid dropping formulas without explaining why the system needs them.

## State And Frames

Always identify:

- State vector: pose, velocity, biases, calibration, landmarks, map parameters.
- Error state: minimal perturbation used by EKF/ESKF or optimizer.
- Nominal state: state propagated on manifold.
- Frame convention: world/map/odom/body/IMU/lidar/camera.
- Transform direction: `T_a_b` means pose of frame b in frame a unless code documents otherwise.
- Timestamp source and interpolation model.

Ask or infer carefully when naming is ambiguous.

## ESKF/EKF Reading Checklist

Explain:

- Why the state contains position, velocity, attitude, gyro bias, accel bias, gravity, or extrinsics.
- Prediction: continuous dynamics -> discretization -> nominal propagation -> covariance propagation.
- Error-state definition: additive for Euclidean variables, Lie algebra perturbation for rotation.
- Measurement: residual definition, linearization point, Jacobian, noise model.
- Update: Kalman gain, covariance update, nominal correction, error reset.
- Observability: which directions are weak or unobservable.
- Consistency risks: wrong Jacobian sign, wrong reset Jacobian, FEJ missing, frame mismatch.

Template:

```markdown
## ESKF Meaning
Nominal state:

Error state:

Prediction:

Residual:

Jacobian:

Code mapping:

Failure modes:
```

## Factor Graph Reading Checklist

Explain:

- Variables: pose, velocity, bias, landmark, calibration, time offset.
- Factors: prior, IMU, projection, lidar residual, loop closure, GPS, wheel odometry.
- Residual: measurement minus prediction or manifold log error.
- Jacobian: analytic, automatic, numerical, local parameterization.
- Linearization: current estimate, first estimate, relinearization policy.
- Solver: Gauss-Newton, Levenberg-Marquardt, Dogleg, iSAM2, sliding window.
- Marginalization: what is removed, what prior remains, where information can become inconsistent.

## Lie Group And Jacobian Notes

When code uses `Exp`, `Log`, `hat`, `vee`, `Jr`, `Jl`, `SO3`, `SE3`, explain:

- Rotations live on a manifold and cannot be updated by naive vector addition.
- `Exp` maps tangent-space perturbations to the manifold.
- `Log` maps manifold error back to tangent space.
- Right/left Jacobians appear when perturbations are composed around rotations.
- The sign and side of perturbation matter.

Useful explanation form:

```markdown
`Jr` is the right Jacobian of SO(3). It corrects the linearized relationship between a small tangent perturbation and a composed rotation. If the code uses right perturbation, changing it to left perturbation changes Jacobian signs and adjoint terms.
```

## Paper Mapping

When mapping paper to code:

1. Identify the paper section/equation/algorithm.
2. Quote or summarize only the minimum needed.
3. Map paper symbols to code symbols in a table.
4. Explain implementation differences: discretization, approximations, robust loss, thresholds, data structures.
5. Identify missing pieces: engineering shortcuts, omitted terms, hard-coded assumptions.

Template:

```markdown
| Paper Symbol | Meaning | Code Symbol | File/Function | Notes |
|---|---|---|---|---|
```

## SLAM Topic Prompts

Use these anchors when relevant:

- ICP: correspondence search -> residual -> Jacobian -> robust weighting -> pose update.
- NDT: voxel Gaussian model -> likelihood/residual -> optimization -> degeneracy.
- Scan matching: point-to-point, point-to-plane, surfel, feature-based, direct methods.
- IMU preintegration: preintegrated delta, bias correction, covariance propagation, reset.
- Bundle adjustment: reprojection residual, landmark parameterization, Schur complement.
- Marginalization: prior factor, Schur complement, fixed linearization point, consistency.
- FEJ: keep selected Jacobians at first estimate to preserve observability properties.
- Observability: global position/yaw/scale/gravity directions depending on sensor setup.
- Loop closure: place recognition, relative constraint, pose graph optimization, map correction.

## Common Mathematical Failure Modes

- Residual sign is flipped.
- Transform direction is inverted.
- Quaternion order differs (`wxyz` vs `xyzw`).
- Radians/degrees mismatch.
- Gravity sign/frame is wrong.
- Timestamp interpolation uses wrong frame.
- Jacobian is derived for left perturbation but code updates with right perturbation.
- Covariance is not reset after state correction.
- Marginalization prior is reused after changing the linearization point.
- Robust loss hides rather than fixes outliers.
