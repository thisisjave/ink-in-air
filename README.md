# Ink-in-Air Gesture Drawing System

Advanced AI-Powered Spatial Art Suite - A professional-grade, real-time interactive drawing application that allows users to create digital art in 3D space using natural hand gestures. Leveraging state-of-the-art hand tracking and AI-powered shape recognition, Ink-in-Air transforms hand movements into precise digital artwork.

## Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Usage](#usage)
- [Controls and Gestures](#controls-and-gestures)
- [Technical Details](#technical-details)
- [Credits](#credits)

## Features

### Dual-Hand Interaction Model

Experience a natural workstation workflow by separating creative and technical tasks:

- **Right Hand (The Artist)**: Point with only your index finger (pinky folded) to Draw/Erase. Extend both your index and pinky fingers (like the "rock on" sign) to Hover (move the cursor without drawing).
- **Left Hand (The Controller)**: Manage your digital studio. Use your left hand to switch colors, adjust brush sizes, lock/unlock thickness, and trigger system actions without interrupting your right-hand drawing.

### Premium Virtual UI (Glassmorphism)

- Modern HUD with sleek, semi-transparent heads-up display featuring rounded pill-shaped buttons
- Visual feedback with "glow" highlights for active tools and selected colors
- Real-time status indicators tracking hand position to show current modes (DRAWING, HOVERING, ERASING)

### Multi-Shape & Polygon Snapping

The AI understands your intent. Draw a rough approximation of a shape, and the system snaps it to clean geometry:

- **Straight Lines**: Snaps any straight-ish line drawn between two points into a perfect straight line
- **Circles & Ellipses**: Snaps circular curves and elongated shapes into mathematically fitted ellipses
- **Rotated Rectangles**: Snaps rectangles at any angle using rotated bounding boxes (cv2.minAreaRect)
- **Polygons**: Snaps 3, 5, and 6-sided polygons cleanly
- **Convex Hull Smoothing**: Smoothes out tremors and automatically closes imperfectly drawn shapes
- **Handwriting Protection**: Rejects small strokes and short corrections to preserve intentional drawing
- **Zero-Overlap Snapping**: Eliminates rough hand-drawn trajectories, drawing only clean shapes

### Left-Hand Pinky Size Lock

- **Adjusting Mode**: Extend pinky finger on left hand, keep middle and ring fingers folded. Pinch/spread thumb and index to adjust brush size (HUD turns cyan)
- **Locked Mode**: Fold pinky finger along with middle and ring fingers. Sizing locks at current value (HUD turns green with LOCKED indicator)

### Advanced Tracking & Smoothing

- High-fidelity 21-point hand landmark tracking via MediaPipe Tasks API
- Tuned 1 Euro Filter for adaptive smoothing (min_cutoff=0.6, beta=0.05)
- Eliminates tremors while maintaining ultra-low latency for writing and drawing
- Increased distance connection threshold (d < 400px) prevents line breaking during rapid movements
- Sub-pixel anti-aliasing for smooth, professional-grade strokes
- Spatial handedness filter restricts settings adjustments to left side of screen

## Project Structure

```
ink-in-air/
├── drawing.py                 # Main application - dual-hand logic, UI rendering, gesture processing
├── euro_filter.py             # Implementation of adaptive 1 Euro Filter for jitter-free tracking
├── test_cameras.py            # Utility to identify and preview available camera indices
├── hand_landmarker.task       # Pre-trained MediaPipe model for hand tracking
├── screenshots/               # Auto-generated folder for saved artwork
└── README.md
```

## Technologies Used

- **OpenCV**: Real-time image processing and UI rendering
- **MediaPipe**: Hand landmark detection and tracking (21-point hand tracking)
- **NumPy**: Mathematical operations and point processing
- **Python 3.x**: Core application logic and development language

## Dependencies

The project uses `uv` package manager for high-performance dependency management.

### Core Dependencies

- `opencv-python` - Computer vision and image processing
- `mediapipe` - Hand tracking and gesture recognition
- `numpy` - Numerical computing and array operations

### System Requirements

- Python 3.8 or higher
- Webcam or camera input device
- 4GB RAM minimum (8GB recommended)
- Modern multi-core processor

## Installation

### Prerequisites

Ensure you have Python 3.8+ and pip/uv installed on your system.

### Setup Steps

1. Clone the repository:
```bash
git clone https://github.com/thisisjave/ink-in-air.git
cd ink-in-air
```

2. Install the uv package manager (if not already installed):
```bash
pip install uv
```

3. Install project dependencies:
```bash
uv add opencv-python mediapipe numpy
```

4. Download the MediaPipe hand tracking model:
   - Obtain `hand_landmarker.task` from the [Google MediaPipe Tasks Model Repository](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker/model_selection)
   - Place the file in the project root directory

5. Run the application:
```bash
uv run drawing.py
```

## Usage

### Starting the Application

```bash
uv run drawing.py
```

The application will launch with your webcam feed and a virtual UI at the top of the screen.

### Saving Your Artwork

Artwork is automatically saved to the `screenshots/` folder when you use the SAVE function or press 's'.

## Controls and Gestures

### Hand Roles & Interactions

| Hand | Role | Primary Actions |
|------|------|-----------------|
| Right Hand | Artist | Drawing (Index up, Pinky folded), Hovering (Index + Pinky up), Shape Creation |
| Left Hand | Controller | Palette selection, Brush sizing, Pinky Lock, Invert Toggle |

### Left-Hand Brush Sizing & Lock

- **Adjust Brush Size**: Middle + Ring fingers folded, Pinky extended. Pinch/spread thumb and index finger to increase/decrease size
- **Lock Size**: Middle + Ring fingers folded, Pinky folded to lock current size

### Virtual Tool Palette (Top Bar)

- **Colors**: RED, GREEN, BLUE, YELLOW
- **ERASER**: 2x thick brush to clear canvas areas
- **SAVE**: Capture artwork to screenshots/ folder
- **INVERT**: Swap Left and Right hand roles (useful for left-handed artists)

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| z | Undo last stroke/shape |
| c | Clear entire canvas |
| s | Save drawing |
| q | Quit application |

## Technical Details

### Hand Tracking

- Uses MediaPipe Tasks Hand Landmarker API for real-time hand detection
- Tracks 21 hand landmarks per hand with high accuracy
- Processes 30+ FPS for smooth, responsive drawing experience

### Shape Recognition & Snapping

The system uses computer vision algorithms including:
- Contour detection and convex hull computation
- Polygon approximation using Douglas-Peucker algorithm
- Ellipse fitting with cv2.fitEllipse()
- Rotated rectangle detection with cv2.minAreaRect()

### Smoothing Algorithm

The 1 Euro Filter implementation provides:
- Adaptive smoothing based on signal velocity
- Configurable cutoff frequency and beta parameter
- Eliminates jitter while preserving natural hand movements

### Hand Classification

- Automatically identifies right and left hands
- Enforces spatial handedness constraints
- Prevents accidental mode switches from misclassification

## Credits

Developed with modern computer vision and gesture recognition technologies:

- **OpenCV** - Image processing and UI rendering library
- **MediaPipe** - Google's framework for building perception pipelines
- **NumPy** - Fundamental package for numerical computing
- **Python** - High-level programming language

---

**Version**: 1.0  
**License**: MIT  
**Author**: thisisjave

For issues, feature requests, or contributions, please visit the [project repository](https://github.com/thisisjave/ink-in-air).
