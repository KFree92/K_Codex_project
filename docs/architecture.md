# AI Pose Guide Generator Architecture

## Product focus

This project is a local-first 3D pose guide generator for AI image workflows. It does not depend on external AI APIs. The app's primary job is to let users rapidly create pose reference outputs such as mannequin renders, skeleton overlays, depth maps, silhouettes, and hybrid guide images.

The application should prioritize clear pose structure, camera perspective, depth, and fast export over artist-facing realism, lighting, clothing, or props.

## Architecture principles

1. **Object-oriented modules**: each major capability is represented by a class or service with a narrow responsibility.
2. **Feature isolation**: each feature can be enabled, disabled, tested, or replaced without rewriting the whole application.
3. **Shared scene state, separate behaviors**: modules communicate through typed state, events, and service contracts rather than directly mutating each other's internals.
4. **Renderer separation**: editing view rendering and export rendering are separate so UI helpers do not pollute generated reference images.
5. **Local-first operation**: pose data, presets, and exports must work without authentication, cloud storage, or external APIs.
6. **AI-reference-first output**: render modes should generate clean conditioning images that are easy for AI image models and ControlNet-style workflows to interpret.

## High-level module map

```txt
Application
├─ FeatureRegistry
├─ EventBus
├─ PoseScene
│  ├─ JointGraph
│  ├─ MannequinBuilder
│  ├─ ConstraintSystem
│  └─ CustomGuideJointSystem
├─ Control Layer
│  ├─ FKController
│  ├─ IKController
│  ├─ TransformGizmoController
│  └─ CameraController
├─ Render Layer
│  ├─ EditorRenderer
│  ├─ MannequinRenderMode
│  ├─ SkeletonRenderMode
│  ├─ DepthRenderMode
│  ├─ NormalRenderMode
│  ├─ SilhouetteRenderMode
│  └─ HybridRenderMode
├─ Export Layer
│  ├─ ExportManager
│  ├─ PngExporter
│  ├─ PoseJsonSerializer
│  └─ BatchExportService
└─ Preset Layer
   ├─ PosePresetLibrary
   ├─ CameraPresetLibrary
   └─ PoseRandomizer
```

## Feature flag model

Every major capability should be registered as a feature. A feature can be enabled or disabled independently.

```ts
export type FeatureId =
  | 'fk-controls'
  | 'ik-controls'
  | 'custom-guide-joints'
  | 'skeleton-export'
  | 'depth-export'
  | 'normal-export'
  | 'hybrid-export'
  | 'pose-randomizer'
  | 'batch-export';

export interface Feature {
  id: FeatureId;
  label: string;
  enabled: boolean;
  initialize(app: AppContext): void;
  dispose(): void;
}
```

The `FeatureRegistry` owns feature lifecycle. If an experimental module causes problems, it can be disabled without removing its code.

## Core objects

### AppContext

`AppContext` is the dependency container passed to features and services. It avoids global imports and prevents modules from secretly depending on each other.

```ts
export interface AppContext {
  events: EventBus;
  scene: PoseScene;
  camera: CameraController;
  features: FeatureRegistry;
  exporters: ExportManager;
}
```

### EventBus

Modules should communicate through domain events where possible.

Example events:

- `joint:selected`
- `joint:rotated`
- `joint:moved`
- `pose:changed`
- `camera:changed`
- `render-mode:changed`
- `export:requested`
- `feature:enabled`
- `feature:disabled`

This keeps controllers, UI, renderers, and exporters loosely coupled.

### JointGraph

`JointGraph` is the source of truth for pose structure. Meshes and render outputs are derived from it.

```ts
export interface JointNode {
  id: string;
  label: string;
  parentId: string | null;
  childrenIds: string[];
  position: Vector3Tuple;
  rotation: QuaternionTuple;
  length: number;
  radius: number;
  constraints?: JointConstraint;
  metadata?: Record<string, unknown>;
}
```

The graph should support both built-in body joints and user-added guide joints such as wings, tails, weapons, hair flow, cloth flow, gaze direction, and composition markers.

## Controller responsibilities

### FKController

Forward kinematics handles explicit joint rotation. It should be simple, stable, and available early in development.

Responsibilities:

- Select a joint.
- Rotate the selected joint.
- Apply parent-child transforms.
- Emit `pose:changed`.
- Respect optional joint constraints.

### IKController

Inverse kinematics is a separate feature because it is more complex and may need iteration.

Responsibilities:

- Move wrist and ankle handles.
- Solve two-bone arm and leg chains.
- Support pole controls for elbow and knee direction.
- Respect joint limits where practical.
- Fall back cleanly if disabled.

### CameraController

Camera state affects all AI reference outputs, so it should be managed centrally.

Responsibilities:

- Orbit, pan, and zoom.
- Perspective and orthographic modes.
- FOV and focal length control.
- Front, side, three-quarter, top, low-angle, and high-angle presets.
- Emit `camera:changed`.

## Render mode responsibilities

Each render mode must implement a shared interface.

```ts
export interface RenderMode {
  id: string;
  label: string;
  render(input: RenderInput): Promise<RenderOutput> | RenderOutput;
}
```

Recommended render modes:

1. **MannequinRenderMode**: gray or colored 3D mannequin with clear body volume.
2. **SkeletonRenderMode**: projected joints and bone lines for pose conditioning.
3. **DepthRenderMode**: grayscale depth image where near and far body parts are distinguishable.
4. **NormalRenderMode**: RGB surface normal output for direction and volume guidance.
5. **SilhouetteRenderMode**: clean body shape mask.
6. **HybridRenderMode**: mannequin plus joint and bone overlay for maximum readability.

## Export responsibilities

The export layer should not know how users edit poses. It only consumes current scene, camera, and render settings.

Required exports:

- `mannequin.png`
- `skeleton.png`
- `depth.png`
- `silhouette.png`
- `hybrid.png`
- `pose.json`

Future exports:

- `normal.png`
- multi-resolution export packs
- ControlNet-style presets
- ComfyUI helper bundles
- batch variations

## Failure isolation strategy

The project should assume some modules will be experimental. For example, IK, normal map rendering, and custom guide joints may need several iterations.

To avoid blocking the whole app:

1. Keep each experimental capability behind a feature flag.
2. Make every feature expose `initialize()` and `dispose()`.
3. Route cross-module updates through events.
4. Keep pose data serializable even when optional features are disabled.
5. Allow the UI to hide disabled feature panels automatically.
6. Prefer degraded behavior over runtime crashes. For example, if IK is disabled, FK rotation should still work.

## Recommended MVP sequence

1. Build `PoseScene`, `JointGraph`, and procedural mannequin rendering.
2. Add `CameraController` with orbit, zoom, FOV, and view presets.
3. Add `FKController` for joint rotation.
4. Add `PoseJsonSerializer` for save and load.
5. Add `PngExporter` for mannequin export.
6. Add `SkeletonRenderMode` and `DepthRenderMode`.
7. Add `HybridRenderMode`.
8. Add feature flags and settings UI.
9. Add `IKController` as an optional feature.
10. Add pose presets, randomizer, and batch export.
