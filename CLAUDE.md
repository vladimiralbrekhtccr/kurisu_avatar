# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Electron application that displays VRM (Virtual Reality Model) avatars as a transparent overlay on the desktop. The app creates a frameless, always-on-top window that can display 3D avatars using Three.js and the @pixiv/three-vrm library.

## Key Commands

- `npm start` - Run the Electron application
- `npm install` - Install dependencies (if needed)

## Architecture

### Main Components

- **main.js**: Electron main process that creates the transparent window with `nodeIntegration: true` and `contextIsolation: false`. Handles IPC communication for window movement.
- **index.html**: Single-page renderer that sets up the Three.js scene and VRM loading. Contains all UI and 3D rendering logic.
- **avatar.vrm**: The VRM model file that gets loaded and displayed

### Key Technical Details

- **Window Configuration**: Transparent, frameless, always-on-top window (800x600) with `skipTaskbar: true`
- **VRM Loading**: Uses `GLTFLoader` with `VRMLoaderPlugin` registered as a plugin. VRM model is accessed via `gltf.userData.vrm`
- **Three.js Setup**: Uses local node_modules version loaded via script tag, with VRM library loaded using `require('@pixiv/three-vrm')`
- **Controls**: Custom mouse controls for camera rotation and zoom, plus a window movement button that uses IPC
- **Security**: Intentionally uses `nodeIntegration: true` for simplicity - avoid adding preload.js or context isolation

### Dependencies

- **Core**: `electron`, `three`, `@pixiv/three-vrm`
- **VRM Loading**: Requires both GLTFLoader and VRMLoaderPlugin working together
- **Three.js Version**: Must be compatible with @pixiv/three-vrm (currently using three@0.159.0)

### Common Issues

- **Multiple Three.js Instances**: Only use one Three.js instance via script tag, load VRM via require()
- **VRM API**: Use `gltfLoader.register()` with `VRMLoaderPlugin`, not direct `VRMLoader` constructor
- **Module System**: This project uses CommonJS-style requires in renderer due to nodeIntegration, not ES modules

## Development Notes

- Always maintain the transparent window functionality
- VRM model should be placed in root directory as `avatar.vrm`
- Mouse controls include rotation (drag) and zoom (wheel)
- Window movement is handled via the top-right button using IPC
- The app shows debug info in top-left corner for troubleshooting