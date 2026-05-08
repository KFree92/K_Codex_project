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
