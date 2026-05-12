# Progress and Approval Plan

This document records how work should proceed before each implementation milestone. The project owner is new to the codebase, so every milestone should start with a clear summary, a concrete plan, and an explicit approval request.

## Current status

### What has been done

1. The product direction has been defined as a local-first AI pose guide generator.
2. The core workflow has been scoped to generating AI-friendly reference images, not running AI image generation itself.
3. External AI APIs are not required for the core application.
4. The architecture direction has been defined as object-oriented, modular, and feature-flag-driven.
5. The initial roadmap has been split into small milestones so the project can grow safely.

### What has not been done yet

1. No runnable web application has been scaffolded yet.
2. No Three.js scene exists yet.
3. No procedural mannequin exists yet.
4. No feature registry exists in code yet.
5. No export, pose editing, FK, IK, or depth rendering features exist yet.

## Working agreement

Before starting each milestone, Codex should summarize:

1. **What has been done so far.**
2. **What will be done next.**
3. **What the target feature is for.**
4. **How Codex will implement it.**
5. **What risks or tradeoffs exist.**
6. **What approval is needed before implementation begins.**

Codex should not jump straight into large implementation work when the owner asks for a planning gate. It should present the plan first and wait for approval wording such as `승인`, `진행해`, or `Milestone 1 진행`.

## First implementation gate: Milestone 1

### What will be built

Milestone 1 will create the first runnable browser prototype.

The goal is to open the app locally and see a simple 3D mannequin in a Three.js scene. The code should already follow the modular architecture, even if the visible feature is still simple.

### Why this feature matters

This milestone proves the foundation of the product:

- The project can run as a web app.
- Three.js rendering works.
- The app has a place to attach future pose, camera, render, and export modules.
- The mannequin feature can be toggled instead of being hardcoded into one large mixed file.

### Planned implementation

Codex will implement Milestone 1 by creating:

1. A React, TypeScript, and Vite application scaffold.
2. A Three.js dependency and browser rendering entry point.
3. An `Application` class that owns startup and shutdown.
4. A `FeatureRegistry` class that enables and disables feature modules.
5. A `PoseScene` class that owns the Three.js scene, renderer, and render loop.
6. A `CameraController` class for orbit camera setup.
7. A procedural mannequin feature that renders a simple T-pose body from basic shapes.
8. A minimal UI panel showing which features are active.

### Expected file shape

The exact file names can change during implementation, but the intended shape is:

```txt
src/
├─ main.tsx
├─ App.tsx
├─ styles.css
├─ core/
│  ├─ Application.ts
│  ├─ Feature.ts
│  └─ FeatureRegistry.ts
├─ scene/
│  ├─ PoseScene.ts
│  └─ CameraController.ts
└─ features/
   └─ mannequin/
      ├─ MannequinFeature.ts
      └─ ProceduralMannequin.ts
```

### Success criteria

Milestone 1 is complete when:

1. The project installs dependencies successfully.
2. The development server starts successfully.
3. The browser shows a simple 3D mannequin.
4. Orbit camera controls work.
5. The mannequin is registered as a feature.
6. The UI shows whether the mannequin feature is enabled.
7. Disabling the mannequin feature removes or hides the mannequin without breaking the scene.

### Risks and tradeoffs

1. The first mannequin will be simple and not anatomically polished.
2. FK, IK, depth export, skeleton export, and PNG export are intentionally deferred.
3. The priority is clean structure and a working base, not final visual quality.

## Approval request

Approval needed before Milestone 1 implementation:

> Do you approve starting Milestone 1 as described above?

Suggested approval reply:

> 승인. Milestone 1 진행해.
