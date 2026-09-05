# Virtual Camera

A GNOME application for creating and managing PipeWire virtual cameras from windows, monitors, and screen regions.

Virtual Camera aims to provide a simple graphical interface for creating virtual camera feeds without requiring `v4l2loopback`, OBS, or manual PipeWire/GStreamer configuration.

> [!NOTE]
> Virtual Camera is currently in early development.

## Features

Planned functionality includes:

* Create virtual cameras from:

  * Application windows
  * Displays/monitors
  * Screen regions
* Use the desktop portal for secure source selection
* Preview camera output before enabling it
* Start and stop virtual cameras
* Create and manage multiple virtual cameras
* Change the source of an existing virtual camera
* Configure:

  * Resolution
  * Frame rate
  * Cursor visibility
  * Scaling behaviour
* Persistent virtual camera profiles
* GNOME-native interface using GTK and Libadwaita
* PipeWire and GStreamer backend
* No `v4l2loopback` requirement

## Goals

Virtual Camera is intended to make Linux virtual cameras feel like a normal desktop feature.

Instead of requiring users to work with PipeWire nodes, GStreamer pipelines, loopback devices, or complex broadcasting software, the application exposes a small set of understandable controls:

1. Create a virtual camera
2. Select what to capture
3. Preview the result
4. Start the camera
5. Use it in another application

The application follows the GNOME Human Interface Guidelines and keeps implementation details out of the main interface wherever possible.

## How It Works

Virtual Camera is planned around the Linux multimedia stack:

* **PipeWire** — media routing and capture
* **GStreamer** — processing and virtual camera pipelines
* **XDG Desktop Portal** — secure window and screen selection
* **GTK 4** — graphical interface
* **Libadwaita** — GNOME application design and widgets

A source selected through the desktop portal is captured through PipeWire, processed as required, and exposed as a virtual camera that compatible applications can use.

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

## Design Principles

Virtual Camera should remain:

* Simple
* GNOME-native
* Secure by default
* Wayland-friendly
* PipeWire-native
* Focused on virtual cameras rather than broadcasting or recording
* Accessible to users who do not understand Linux multimedia internals

The project is not intended to replace OBS Studio.

For complex scenes, compositing, streaming, recording, transitions, overlays, and other production workflows, OBS remains the appropriate tool.

Virtual Camera instead focuses on the common case of:

> “I want this window or screen to appear as a camera.”

## Project Status

The project is currently under development.

APIs, behaviour, UI design, and implementation details may change substantially before the first stable release.

## Contributing

Contributions, bug reports, design feedback, and feature suggestions are welcome.

Before implementing major functionality, please open an issue so the proposed approach can be discussed first.

When contributing UI changes, please keep the GNOME Human Interface Guidelines and Libadwaita conventions in mind.

## License

License information will be added as the project develops.
