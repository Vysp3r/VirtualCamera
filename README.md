# Virtual Camera

A Linux application for creating and managing PipeWire virtual cameras from windows, monitors, and screen regions.

Virtual Camera aims to provide a simple graphical interface for creating virtual camera feeds without requiring `v4l2loopback`, OBS, or manual PipeWire/GStreamer configuration.

> [!NOTE]
> Virtual Camera is currently in early development.

## Features

Planned functionality includes:

* Create virtual cameras from:

  * Application windows
  * Displays/monitors
  * Screen regions
* Use XDG Desktop Portal for secure source selection
* Preview camera output before enabling it
* Start and stop virtual cameras
* Create and manage multiple virtual cameras
* Change the source of an existing virtual camera
* Persistent virtual camera profiles
* Configure:

  * Resolution
  * Frame rate
  * Cursor visibility
  * Scaling behaviour
* PipeWire and GStreamer backend
* No `v4l2loopback` requirement
* Wayland-first design
* Multiple frontend support

## Frontends

Virtual Camera is designed with a toolkit-independent core so that multiple graphical frontends can share the same backend implementation.

Planned frontends include:

### GTK

A GTK 4 and Libadwaita frontend designed to integrate naturally with GNOME and follow the GNOME Human Interface Guidelines.

### Qt

A Qt 6 frontend intended to provide a native experience on KDE Plasma and other Qt-based desktop environments.

Both frontends will use the same underlying virtual camera, capture, profile, and pipeline implementation.

## Architecture

Virtual Camera separates multimedia and application logic from the graphical interface.

```text
VirtualCamera
├── Core
│   ├── Virtual camera management
│   ├── PipeWire integration
│   ├── GStreamer pipelines
│   ├── Portal integration
│   ├── Capture sources
│   ├── Camera profiles
│   ├── Configuration
│   └── Events
│
└── Frontends
    ├── GTK 4 / Libadwaita
    └── Qt 6
```

The core must remain independent from GTK, Qt, and other user-interface frameworks.

Frontend-specific concepts such as widgets, dialogs, windows, models, and toolkit signals should remain entirely inside their respective frontend implementations.

## Core

The core is responsible for concepts such as:

```text
CameraManager
VirtualCamera
CaptureSource
CameraProfile
Pipeline
CameraState
```

Typical operations include:

```text
create_camera()
remove_camera()
start_camera()
stop_camera()
update_camera()
list_cameras()
request_capture_source()
```

State changes are exposed through toolkit-independent events which can be adapted by each frontend.

Examples include:

```text
camera_added
camera_removed
camera_state_changed
source_changed
preview_changed
error
```

## How It Works

Virtual Camera is built around the modern Linux multimedia stack:

* **PipeWire** — media routing and capture
* **GStreamer** — video processing and pipeline management
* **XDG Desktop Portal** — secure window and screen selection
* **GTK 4 / Libadwaita** — GNOME frontend
* **Qt 6** — KDE and Qt frontend

A source is selected through the desktop portal:

```text
Window / Monitor / Region
          ↓
XDG Desktop Portal
          ↓
      PipeWire
          ↓
      GStreamer
          ↓
Virtual Camera Core
          ↓
   Virtual Camera
```

Applications can then use the resulting stream as a normal camera source where supported.

## Preview

Video previews are also kept independent from the graphical toolkit.

The core provides access to the relevant stream or pipeline while each frontend is responsible for displaying it using its own native widgets.

```text
Core preview stream
       ├── GTK preview
       └── Qt preview
```

This prevents multimedia logic from becoming coupled to either frontend.

## Virtual Camera Profiles

Virtual cameras can be saved as individual profiles.

For example:

```text
Discord Camera
└── Firefox — YouTube

Game Camera
└── Monitor 1

Presentation
└── LibreOffice Impress
```

Each camera can be started, stopped, reconfigured, or assigned a different source independently.

## Background Service

A future version may optionally expose the core through a D-Bus service.

This would allow virtual cameras to continue running independently of the graphical frontend.

Potential architecture:

```text
GTK ─┐
     │
Qt ──┼── D-Bus ── Virtual Camera Service
     │
CLI ─┘
```

This could eventually provide:

* Persistent cameras when the GUI closes
* Multiple clients controlling the same cameras
* Command-line management
* Desktop integrations
* Automation support

The initial implementation does not need to depend on this architecture.

## Design Goals

Virtual Camera should remain:

* Simple
* Desktop-native
* Secure by default
* Wayland-friendly
* PipeWire-native
* Toolkit-independent at its core
* Easy to use without understanding Linux multimedia internals
* Focused specifically on virtual cameras

The GTK frontend should follow GNOME conventions and Human Interface Guidelines.

The Qt frontend should integrate naturally with KDE Plasma and Qt-based environments.

## Non-Goals

Virtual Camera is not intended to replace OBS Studio.

Complex functionality such as:

* Scene composition
* Streaming
* Recording
* Transitions
* Overlays
* Video production workflows

is outside the primary scope of the project.

Virtual Camera instead focuses on the common use case:

> “I want this window or screen to appear as a camera.”

## Project Status

The project is currently under development.

APIs, architecture, behaviour, UI design, and implementation details may change significantly before the first stable release.

## Contributing

Contributions, bug reports, design feedback, and feature suggestions are welcome.

Before implementing major functionality, please open an issue so the proposed approach can be discussed first.

When contributing:

* Keep core logic independent from frontend frameworks
* Avoid introducing GTK or Qt dependencies into the core
* Keep multimedia logic reusable between frontends
* Follow the conventions of the frontend being modified
* Prefer simple user-facing abstractions over exposing PipeWire or GStreamer internals

## License

License information will be added as the project develops.
