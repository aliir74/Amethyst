# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Amethyst is a tiling window manager for macOS, inspired by xmonad. It automatically arranges application windows in configurable layouts. Written in Swift, targeting macOS 10.15+.

## Build Commands

```bash
# Install dependencies (fastlane, xcbeautify, swiftlint)
brew bundle

# Build the app (output: ./build/Amethyst.app)
fastlane

# Run tests
xcodebuild -workspace Amethyst.xcworkspace -scheme Amethyst clean test | xcbeautify

# Run tests (without xcbeautify)
xcodebuild -workspace Amethyst.xcworkspace -scheme Amethyst clean test

# Lint Swift code
swiftlint
```

## Architecture

### Core Components

- **Managers/** - Core business logic orchestrators
  - `WindowManager.swift` - Central coordinator for all windows across screens
  - `ScreenManager.swift` - Per-screen state and layout management
  - `HotKeyManager.swift` - Keyboard shortcut registration and handling
  - `AppManager.swift` - Application lifecycle tracking

- **Layout/** - Window arrangement algorithms implementing the base `Layout` class
  - 13 built-in layouts (Tall, Wide, BSP, Fullscreen, Column, Row, etc.)
  - `CustomLayout.swift` - JavaScript-based custom layouts (beta)

- **Model/** - Data models for Window, Screen, Space, Application, UserConfiguration

- **Preferences/** - Settings UI view controllers

### Key Dependencies

- **Silica** - Custom framework for window/screen management (by ianyh)
- **RxSwift** - Reactive event handling
- **Quick/Nimble** - BDD-style testing

### Design Patterns

- Manager pattern for separation of concerns
- Strategy pattern for layout algorithms
- Observer pattern via RxSwift for event handling
- Coordinators for transitions (WindowTransitionCoordinator, FocusTransitionCoordinator)

## Testing

Tests use Quick/Nimble (BDD-style). Test files are in `AmethystTests/`.

```bash
# Run a specific test class
xcodebuild -workspace Amethyst.xcworkspace -scheme Amethyst \
  -only-testing:AmethystTests/LayoutNameTests test | xcbeautify
```

## Configuration

User configuration is YAML-based at `~/.amethyst.yml` or `~/.config/amethyst/amethyst.yml`. See `.amethyst.sample.yml` for reference.

## Code Style

SwiftLint is configured via `.swiftlint.yml`:
- Line length: 200 characters (warning)
- Cyclomatic complexity: 15
- Some rules disabled: `force_cast`, `force_try`, `function_body_length`, `file_length`

## Contributing

- Branch off `development` (not `master`)
- Open PRs against `development`
- The app requires macOS Accessibility permissions to function
