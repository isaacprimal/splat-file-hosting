# 3D Gaussian Splat Sequence Viewer

A WebGL-based viewer for animated 3D Gaussian Splat sequences, featuring real-time frame loading, interactive camera controls, and optimized rendering performance.

## 🌟 Features

- **Real-time Splat Rendering**: Direct display of splat content without loading animations
- **Frame Sequence Playback**: Smooth playback through hundreds of .splat files
- **Interactive Camera Controls**: Mouse, touch, keyboard, and gamepad support
- **Optimized Loading**: Aggressive preloading with intelligent caching
- **Configurable Scenes**: JSON-based configuration for camera positioning and playback settings
- **CORS-Enabled**: Served via GitHub Pages with proper headers for web applications

## 🚀 Live Demo

**View the Ripley character sequence**: [https://isaacprimal.github.io/splat-file-hosting/viewer.html](https://isaacprimal.github.io/splat-file-hosting/viewer.html)

## 📁 Project Structure

```
splat-file-hosting/
├── main.js                    # Core splat viewer engine
├── index.html                 # Main viewer interface
├── viewer.html                # Production viewer page
├── splats/
│   ├── scene.json            # Scene configuration
│   ├── frame_00001.splat     # First frame
│   ├── frame_00002.splat     # Second frame
│   └── ...                   # Additional frames (299 total)
├── clean_alpha.png           # TechniSync logo
└── README.md                 # This documentation
```

## ⚙️ Configuration

### Scene Configuration (`splats/scene.json`)

```json
{
  "ver": "1.0",
  "owner": "Primal Sphere",
  "scene": "Ripley",
  "framerate": 60,
  "numFrames": 299,
  "carouselOnLoad": false,
  "center": [0, 0, 0],
  "rotationOffset": 0,
  "viewMatrix": [0, 0, 1, 0, 0, 1, 0, 0, -1, 0, 0, 0, 0, 0, 4, 1],
  "baseUrl": "./splats/",
  "framePrefix": "frame_",
  "frameExtension": ".splat",
  "camera": {
    "id": 0,
    "img_name": "00001",
    "width": 1920,
    "height": 1080,
    "position": [0, 0, 4],
    "rotation": [[1, 0, 0], [0, 1, 0], [0, 0, 1]],
    "fy": 1000,
    "fx": 1000
  }
}
```

#### **COMPLETE CONFIGURATION CONTROL** 🎯
The `scene.json` file now has **FULL CONTROL** over all application parameters. No hardcoded values or URL parameters can override these settings.

#### Configuration Options

| Property | Type | Description | **NEW** |
|----------|------|-------------|---------|
| `ver` | string | Configuration version | |
| `owner` | string | Scene owner/creator | |
| `scene` | string | Scene name | |
| `framerate` | number | Target FPS for playback | |
| `numFrames` | number | Total frames in sequence | |
| `carouselOnLoad` | boolean | Auto-rotate camera on load | |
| `center` | array | Scene center point [x, y, z] | |
| `rotationOffset` | number | Initial rotation offset (degrees) | |
| `viewMatrix` | array | 4x4 camera transformation matrix | |
| `baseUrl` | string | **🆕 Base URL for splat files** | ✅ |
| `framePrefix` | string | **🆕 Filename prefix (e.g., "frame_")** | ✅ |
| `frameExtension` | string | **🆕 File extension (e.g., ".splat")** | ✅ |
| `camera` | object | **🆕 Camera configuration object** | ✅ |

#### **NEW: File Loading Control** 🆕
- **`baseUrl`**: Controls where splat files are loaded from
  - Default: `"./splats/"`
  - Can be any URL: `"https://example.com/myfiles/"`
- **`framePrefix`**: Filename prefix before frame numbers
  - Default: `"frame_"`
  - Example: `"sequence_"` → `sequence_00001.splat`
- **`frameExtension`**: File extension for splat files
  - Default: `".splat"`
  - Can be: `".ply"`, `".splatdata"`, etc.

#### **NEW: Camera Configuration** 🆕
The `camera` object provides complete control over camera parameters:
```json
"camera": {
  "id": 0,
  "img_name": "00001",
  "width": 1920,          // Viewport width
  "height": 1080,         // Viewport height  
  "position": [0, 0, 4],  // Camera position [x, y, z]
  "rotation": [           // 3x3 rotation matrix
    [1, 0, 0],
    [0, 1, 0], 
    [0, 0, 1]
  ],
  "fy": 1000,            // Focal length Y
  "fx": 1000             // Focal length X
}
```

### **🚫 REMOVED CONFLICTS**
The following features have been **REMOVED** to ensure scene.json has complete control:

❌ **Hardcoded camera arrays** - Now scene.json controls camera
❌ **URL parameter overrides** - No more `?url=` or `?f=` parameters
❌ **Hardcoded splat URLs** - Now configurable via `baseUrl`/`framePrefix`/`frameExtension`
❌ **Keyboard camera switching** - Number keys no longer switch cameras

### **✅ CONFIGURATION EXAMPLES**

#### Local Development:
```json
{
  "baseUrl": "./splats/",
  "framePrefix": "frame_",
  "frameExtension": ".splat"
}
```

#### Remote Server:
```json
{
  "baseUrl": "https://example.com/sequences/character1/",
  "framePrefix": "seq_",
  "frameExtension": ".ply"
}
```

#### Different Naming Convention:
```json
{
  "baseUrl": "./data/",
  "framePrefix": "ripley_animation_",
  "frameExtension": ".splatdata"
}
```
Result: Loads `./data/ripley_animation_00001.splatdata`, etc.

### Camera View Matrix

The `viewMatrix` is a 4x4 transformation matrix defining camera position and orientation:

```javascript
// Format: [row-major order]
[
  Xx, Xy, Xz, 0,    // X-axis (right)
  Yx, Yy, Yz, 0,    // Y-axis (up)  
  Zx, Zy, Zz, 0,    // Z-axis (forward)
  Tx, Ty, Tz, 1     // Translation (position)
]
```

#### Common Camera Angles

**Portrait View (current):**
```json
[0.866, 0, 0.5, 0, -0.25, 0.866, 0.433, 0, -0.433, -0.5, 0.75, 0, 0, 1, 4, 1]
```

**Side View:**
```json
[0, 0, 1, 0, 0, 1, 0, 0, -1, 0, 0, 0, 0, 0, 4, 1]
```

**Top-Down View:**
```json
[1, 0, 0, 0, 0, 0, 1, 0, 0, -1, 0, 0, 0, 4, 0, 1]
```

## 🎮 Controls

### Mouse/Trackpad
- **Left Click + Drag**: Orbit around subject
- **Right Click + Drag**: Pan camera
- **Scroll Wheel**: Zoom in/out
- **Shift + Scroll**: Pan horizontally/vertically
- **Ctrl/Cmd + Scroll**: Move forward/backward

### Keyboard
- **Arrow Keys**: Move camera (Shift for vertical)
- **WASD**: Rotate camera view
- **QE**: Roll camera left/right
- **Space**: Play/pause animation
- **Comma/Period**: Step frame by frame
- **P**: Toggle carousel mode
- **V**: Export current view matrix
- **Z**: Show camera position
- **+/-**: Switch camera presets (if available)

### Touch (Mobile)
- **Single Touch**: Orbit camera
- **Two Finger Pinch**: Zoom
- **Two Finger Rotate**: Roll camera

### Gamepad
- **Left Stick**: Move camera
- **Right Stick**: Rotate view
- **Triggers**: Roll camera
- **D-Pad**: Movement
- **Shoulder Buttons**: Frame stepping
- **A Button**: Play/pause
- **Y Button**: Carousel mode

## 🔧 Technical Implementation

### Core Technologies
- **WebGL 2.0**: GPU-accelerated rendering
- **Web Workers**: Background splat processing and sorting
- **Fetch API**: Streaming frame loading
- **Canvas API**: Interactive display surface

### Performance Optimizations

1. **Aggressive Preloading**: All frames loaded in background
2. **Worker-Based Processing**: Splat data processing off main thread
3. **Depth Sorting**: Real-time depth-based rendering order
4. **Texture Streaming**: Efficient GPU memory usage
5. **Frame Caching**: Smart memory management for 299 frames

### Recent Improvements

#### ✅ Fixed Blue Cube Loading Issue
- **Problem**: Splats stuck behind blue loading animation
- **Solution**: Fixed worker message handling and immediate spinner removal
- **Result**: Splats visible immediately as they load

#### ✅ Relative Path Loading
- **Problem**: Hardcoded GitHub URL prevented local testing
- **Solution**: Changed to `./splats/scene.json` relative path
- **Result**: Works both locally and on GitHub Pages

#### ✅ Optimized Camera Positioning
- **Problem**: Default camera angle poor for character models
- **Solution**: Implemented portrait-style viewing angle
- **Result**: Perfect viewing angle for Ripley character

#### ✅ Enhanced Frame Management
- **Problem**: Inefficient frame loading and caching
- **Solution**: Implemented smart preloading and cache system
- **Result**: Smooth 60fps playback through 299 frames

## 🚀 Deployment

### GitHub Pages Setup
1. Push changes to `main` branch
2. GitHub Pages automatically builds and deploys
3. Access at: `https://[username].github.io/splat-file-hosting/viewer.html`

### Local Development
```bash
# Start local server
python -m http.server 8000

# Access locally
http://localhost:8000/viewer.html
```

### CORS Configuration
The `_headers` file ensures proper CORS headers for cross-origin access:
```
/*
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: GET, POST, OPTIONS
  Access-Control-Allow-Headers: *
```

## 📊 File Specifications

### .splat File Format
- **Binary format**: Optimized 3D Gaussian data
- **Size**: ~1.8-2.0MB per frame
- **Total sequence**: ~600MB for 299 frames
- **Encoding**: Float32 positions, Uint8 colors and rotations

### Supported Formats
- **.splat**: Primary format (binary Gaussian splat data)
- **.ply**: Secondary format (ASCII point cloud data)

## 🎯 Use Cases

- **Character Animation**: View animated 3D character sequences
- **Product Visualization**: Interactive 3D product demos
- **Scientific Visualization**: Volumetric data display
- **Educational Content**: Interactive 3D learning materials
- **Entertainment**: Immersive 3D experiences

## 🔮 Future Enhancements

- [ ] Multi-sequence support
- [ ] Real-time splat streaming
- [ ] VR/AR compatibility
- [ ] Advanced lighting controls
- [ ] Export capabilities (video, images)
- [ ] Performance analytics
- [ ] Mobile optimization

## 📄 License

This project is part of the TechniSync Frame Viewer system.
Copyright 2024 TechniSync Corporation.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Make improvements
4. Test thoroughly
5. Submit pull request

---

**Built with ❤️ for the 3D Gaussian Splatting community** 