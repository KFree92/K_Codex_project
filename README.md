# K_Codex_project

## AI Pose Guide Generator

This project is intended to become a local-first 3D pose guide generator for AI image workflows. The goal is not to recreate a detailed artist reference app like Magic Poser. Instead, the app should quickly generate AI-friendly pose reference images such as 3D mannequin views, skeleton overlays, depth maps, silhouettes, and hybrid guides.

## Development direction

- No external AI API dependency for the core workflow.
- Object-oriented architecture with isolated modules.
- Feature-level enable and disable controls.
- Shared 3D joint graph as the source of truth.
- Separate editing renderer and export renderers.
- Outputs optimized for AI image generation reference workflows.

See [docs/architecture.md](docs/architecture.md) for the initial modular architecture plan.

## If you are new to the project

Build this in milestones instead of requesting the entire app at once. Start with a running 3D web prototype, then add pose editing, exports, and optional advanced systems one by one.

Recommended next request:

> Build Milestone 1: scaffold a React + TypeScript + Vite app, add Three.js, create a modular `Application`, `FeatureRegistry`, `PoseScene`, `CameraController`, and render a simple procedural mannequin in T-pose with orbit camera controls. Keep the architecture object-oriented and make the mannequin feature toggleable.

See [docs/next-steps.md](docs/next-steps.md) for the beginner-friendly roadmap and request templates.

See [docs/progress.md](docs/progress.md) for the current progress summary and first implementation approval gate.
