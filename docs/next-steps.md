# Beginner Next Steps

This project should be built in small, visible increments. Do not ask for a huge full application at once. Ask for one thin feature, run it, check it visually, then continue.

## How to ask for work

Use requests that include a clear target, a small scope, and an expected result.

Good examples:

- "Create the initial web app scaffold with React, TypeScript, and Three.js."
- "Add a simple 3D scene with orbit camera and a gray procedural mannequin in T-pose."
- "Add a feature registry so each module can be enabled or disabled."
- "Add FK rotation controls for one arm only, then test that the rest of the mannequin still renders."
- "Add PNG export for the current mannequin view."

Avoid requests like:

- "Make the whole app."
- "Build everything like Magic Poser."
- "Add AI support." The core app should not depend on external AI APIs.

## Recommended first milestone

The first useful milestone is not a polished product. It is a running local web app that proves the core 3D workflow.

Milestone 1 should include:

1. A React and TypeScript web app scaffold.
2. Three.js rendering through a dedicated scene module.
3. A procedural mannequin made from simple joints and limb shapes.
4. Orbit camera controls.
5. A `FeatureRegistry` that can enable or disable feature modules.
6. A minimal UI panel that shows active features.

Expected result: opening the app shows a simple 3D mannequin in the browser, and the code already has modular boundaries for future pose editing and export features.

## Suggested command for the next implementation request

Ask for this next:

> Build Milestone 1: scaffold a React + TypeScript + Vite app, add Three.js, create a modular `Application`, `FeatureRegistry`, `PoseScene`, `CameraController`, and render a simple procedural mannequin in T-pose with orbit camera controls. Keep the architecture object-oriented and make the mannequin feature toggleable.

## Roadmap after Milestone 1

### Milestone 2: Pose data model

Add `JointGraph`, built-in body joints, pose serialization, and reset pose support.

### Milestone 3: FK controls

Allow selecting a joint and rotating it with UI controls. FK should be a feature that can be disabled independently.

### Milestone 4: AI reference exports

Add PNG export modes for mannequin, skeleton, silhouette, and depth.

### Milestone 5: Hybrid guide output

Combine mannequin rendering with projected joint points and bone lines.

### Milestone 6: Optional IK

Add arm and leg two-bone IK as an optional feature. If IK breaks, FK and exports should still work.

### Milestone 7: Presets and batch workflow

Add pose presets, camera presets, random pose variations, and batch export.

## Review checklist for each milestone

Before moving to the next milestone, verify:

- The app still runs locally.
- The new feature can be disabled if it is experimental.
- Existing features still work when the new feature is disabled.
- The feature has a clear class, service, or module boundary.
- The output is useful for AI image reference workflows, not just visually decorative.
