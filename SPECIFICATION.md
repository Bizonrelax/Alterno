# Alterno — Technical Specification

**Date**: 2026-10-16  
**Version**: 1.0  
**License**: The Unlicense  
**Repository**: https://github.com/Bizonrelax/Alterno

## 1. Overview

### 1.1. Purpose
Alterno is a desktop application for visual document comparison using the **blink comparator** technique. It rapidly alternates between documents displayed in the same screen position, making differences immediately visible through visual flickering.

### 1.2. Inspiration
The technique is borrowed from astronomy, where blink comparators have been used since 1904 to detect moving objects (asteroids, comets, variable stars) by rapidly switching between photographic plates. Clyde Tombaugh discovered Pluto in 1930 using this method.

### 1.3. Core Principle
Human vision is exceptionally sensitive to motion and flickering. When two nearly identical images or documents are displayed in rapid alternation at the same position, any differences "blink" while identical content remains stable. This allows instant detection of changes without line-by-line scanning.

## 2. Technical Stack

- **Language**: Java 21+ (Virtual Threads permitted)
- **Build System**: Maven
- **UI Framework**: JavaFX or Swing (decision pending)
- **Video Export**: FFmpeg (via JavaCV or ProcessBuilder) or Xuggler
- **Platforms**: Windows 10+, Linux (Ubuntu 20.04+), macOS 11+
- **License**: The Unlicense (public domain)

## 3. Features

### 3.1. Text Blink Mode
Compare text documents by rendering them with identical parameters and rapidly alternating display.

**Supported formats**:
- Plain text: `.txt`, `.md`
- Code: `.java`, `.js`, `.py`, `.cpp`, `.c`, `.h`, `.cs`, `.go`, `.rs`
- Data: `.json`, `.xml`, `.csv`, `.yaml`, `.yml`
- Documents: `.pdf` (via Apache PDFBox)

**Rendering requirements**:
- Identical font (Monospaced, user-configurable size)
- Identical background color
- Identical text position (pixel-perfect alignment)
- Identical line height and character spacing

### 3.2. Image Blink Mode
Compare images by displaying them at the same scale and position.

**Supported formats**:
- `.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.tiff`

**Requirements**:
- Both images scaled to identical dimensions
- Pixel-perfect alignment
- Optional: diff overlay (highlight changed pixels in red)

### 3.3. Press Mode (Video Export)
**Problem**: Real-time blink comparison may suffer from rendering lag, causing misalignment and visual artifacts.

**Solution**: Export the comparison as a video file where each frame is a document. Video players are optimized for smooth frame playback, eliminating lag issues.

**Workflow**:
1. User loads documents (A, B, optionally C, D...)
2. User configures: FPS (frames per second), pause duration, cycle order
3. Application renders each document as video frames
4. Frames are encoded into video file (`.avi`, `.mkv`, `.mp4`)
5. User plays video in any standard video player (VLC, MPV, etc.)
6. Video loops continuously for inspection

**Video specifications**:
- **Frame rate**: User-configurable (1-30 FPS, default 3 FPS)
- **Pause frames**: Optional pause on each document (0-2 seconds, default 0.5s)
- **Resolution**: Match document render size (or user-specified)
- **Codec**: H.264 (widely compatible) or VP9 (open source)
- **Container**: `.mp4` (H.264), `.mkv` (VP9), `.avi` (legacy)
- **Audio**: Optional silent track or user-provided audio file

**Advantages**:
- Zero lag during playback
- Frame-perfect synchronization
- Can pause, rewind, slow down in video player
- Shareable: send video file to colleagues
- Archival: save comparison for later review

### 3.4. Multi-Document Comparison (Future)
Compare more than two documents in a cycle.

**Use cases**:
- Compare multiple versions of a document (v1, v2, v3, v4)
- Compare translations (English, Spanish, French, German)
- Compare design iterations (draft1, draft2, draft3)

**Cycle modes**:
- **Sequential**: A → B → C → D → A → B...
- **Ping-pong**: A → B → C → D → C → B → A → B...
- **Custom**: User-defined order

## 4. Architecture

### 4.1. Layer Separation

Alterno/
├── src/main/java/com/alterno/
│ ├── App.java (entry point)
│ ├── ui/
│ │ ├── MainWindow.java (main window)
│ │ ├── BlinkController.java (blink engine)
│ │ ├── DocumentView.java (document rendering)
│ │ └── VideoExporter.java (Press mode)
│ ├── core/
│ │ ├── DocumentLoader.java (file loading)
│ │ ├── TextRenderer.java (text rendering)
│ │ ├── ImageRenderer.java (image rendering)
│ │ └── VideoEncoder.java (video encoding)
│ └── util/
│ └── FileFilter.java
└── src/main/resources/
└── styles/
└── main.css



### 4.2. Blink Engine
The blink engine manages document alternation:
- **Real-time mode**: Uses `Timer` or `AnimationTimer` to switch documents at specified FPS
- **Press mode**: Renders each document as sequential frames, encodes to video

**Timing precision**:
- Frame duration: `1000ms / FPS`
- Accuracy: ±1ms (using `System.nanoTime()`)
- Synchronization: Both documents rendered at identical position

### 4.3. Document Rendering
Documents are rendered to off-screen buffers (JavaFX `Canvas` or Swing `BufferedImage`), then displayed in the main view.

**Text rendering**:
- Use `TextFlow` (JavaFX) or `JTextArea` (Swing) with Monospaced font
- Render to `WritableImage` or `BufferedImage`
- Cache rendered images for performance

**Image rendering**:
- Load with `ImageIO` or JavaFX `Image`
- Scale to target dimensions
- Cache scaled images

## 5. User Interface

### 5.1. Main Window Layout


```
┌─────────────────────────────────────────────────────┐
│ [Load A] [Load B] [Load C] [Load D] │
├─────────────────────────────────────────────────────┤
│ │
│ │
│ [Document Display Area] │
│ (documents alternate here) │
│ │
│ │
├─────────────────────────────────────────────────────┤
│ [▶ Start/Stop] Speed: [===●====] 3 FPS │
│ Pause: [==●=====] 0.5s │
│ Mode: ( ) Text ( ) Image ( ) Press │
├─────────────────────────────────────────────────────┤
│ Current: Document A | Size: 1024x768 | FPS: 3 │
└─────────────────────────────────────────────────────┘
```



### 5.2. Controls
- **Load buttons**: Load documents into slots A, B, C, D (drag & drop supported)
- **Start/Stop**: Toggle blink alternation (hotkey: `Space`)
- **Speed slider**: 1-30 FPS (hotkeys: `↑`/`↓`)
- **Pause slider**: 0-2 seconds pause on each document (hotkeys: `←`/`→`)
- **Mode selector**: Text Blink / Image Blink / Press
- **Status bar**: Current document, dimensions, FPS

### 5.3. Press Mode Dialog
When user selects Press mode and clicks Start:


```
┌─────────────────────────────────────────┐
│ Press Mode Configuration │
├─────────────────────────────────────────┤
│ Output file: [_____________] [Browse] │
│ Format: [MP4 ▼] │
│ Frame rate: [3] FPS │
│ Pause duration: [0.5] seconds │
│ Cycle count: [∞] (infinite loop) │
│ Audio: [ ] Add silent track │
│ │
│ [Cancel] [Export Video] │
└─────────────────────────────────────────┘
```




### 5.4. Hotkeys
- `Space`: Start/Stop blink
- `↑`/`↓`: Increase/decrease FPS
- `←`/`→`: Increase/decrease pause
- `1`/`2`/`3`/`4`: Show only document A/B/C/D
- `Ctrl+O`: Open file
- `Ctrl+E`: Export video (Press mode)
- `Esc`: Exit

## 6. Technical Requirements

### 6.1. Performance
- **Real-time blink**: <16ms frame rendering time (60 FPS capable)
- **Document loading**: <2 seconds for files up to 10 MB
- **Memory usage**: <512 MB for typical documents
- **Video encoding**: Real-time or faster (1 minute video encoded in <1 minute)

### 6.2. Synchronization
- **Pixel-perfect alignment**: Documents rendered at identical position (≤1 pixel deviation)
- **Timing accuracy**: Frame duration accuracy ±1ms
- **No visual artifacts**: No flickering, tearing, or misalignment

### 6.3. Cross-Platform
- Windows 10+ (64-bit)
- Linux: Ubuntu 20.04+, Fedora 34+
- macOS 11+ (Big Sur)
- No platform-specific code in core logic

## 7. Video Export (Press Mode)

### 7.1. Encoding Pipeline
1. **Render frames**: Each document rendered as sequence of frames
2. **Add pause frames**: Insert duplicate frames for pause duration
3. **Encode**: Use FFmpeg (via JavaCV or ProcessBuilder) to encode frames
4. **Mux**: Combine video (and optional audio) into container
5. **Output**: Write to user-specified file

### 7.2. Frame Sequence Example
For 2 documents (A, B) at 3 FPS with 0.5s pause:

Frame 1: Document A Frame 2: Document A (pause) Frame 3: Document B Frame 4: Document B (pause) Frame 5: Document A Frame 6: Document A (pause) ...


### 7.3. Video Player Compatibility
Exported videos should play in:
- VLC Media Player
- MPV
- Windows Media Player
- QuickTime (macOS)
- Web browsers (HTML5 video)

## 8. Future Enhancements

### 8.1. Diff Overlay Mode
Highlight changed regions in red overlay while blinking.

### 8.2. PDF Support
Use Apache PDFBox to render PDF pages as images for blink comparison.

### 8.3. Batch Comparison
Compare entire directories of files, generating video for each pair.

### 8.4. Audio Support
- Add silent audio track (for video players that require audio)
- Allow user to attach audio file (e.g., narration)

### 8.5. Annotation Mode
Allow user to annotate differences directly on the display.

## 9. Acceptance Criteria

### 9.1. MVP (Minimum Viable Product)
- [ ] Maven project structure
- [ ] Main window with document slots (A, B)
- [ ] Text Blink mode with adjustable FPS
- [ ] Image Blink mode
- [ ] Hotkeys (Space, arrows)
- [ ] Cross-platform builds (Windows, Linux, macOS)

### 9.2. Press Mode
- [ ] Video export to MP4 (H.264)
- [ ] Configurable FPS and pause
- [ ] Infinite loop in exported video
- [ ] Plays in VLC without issues

### 9.3. Multi-Document (Future)
- [ ] Support for 3+ document slots
- [ ] Sequential and ping-pong cycle modes
- [ ] Custom cycle order

## 10. Risks and Mitigations

### 10.1. Rendering Lag
**Risk**: Real-time blink may lag on slow machines  
**Mitigation**: Press mode (video export) eliminates lag

### 10.2. Font Rendering Differences
**Risk**: Different platforms render fonts slightly differently  
**Mitigation**: Use identical font file bundled with application

### 10.3. Video Codec Compatibility
**Risk**: Some video players may not support H.264  
**Mitigation**: Offer multiple formats (MP4, MKV, AVI)

### 10.4. Large Documents
**Risk**: Very large documents may exceed memory  
**Mitigation**: Paginate documents, compare page by page

## 11. License

The Unlicense — public domain. No copyright, no restrictions, no attribution required.

See [LICENSE](LICENSE) file for full text.

---

**End of Specification**