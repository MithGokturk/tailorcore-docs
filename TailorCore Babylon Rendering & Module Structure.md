
## Babylon.js Folder Structure

The Babylon.js implementation follows a modular organization for clear separation of concerns:

```
src/frontend/babylon/
├── engine/                  # Core engine initialization and setup
│   ├── BabylonEngine.ts     # Engine instance management
│   ├── SceneManager.ts      # Scene creation and lifecycle
│   └── CameraController.ts  # Camera positioning and control
├── loaders/                 # Asset loading components
│   ├── GlbLoader.ts         # GLB file loading and processing
│   ├── MeasurementLoader.ts # Measurement data integration
│   └── MaterialLoader.ts    # Material loading and fallback handling
├── renderers/               # Visualization components
│   ├── AvatarRenderer.ts    # Avatar rendering and positioning
│   ├── GarmentRenderer.ts   # Garment visualization
│   └── MeasurementRenderer.ts # Measurement point visualization
└── utils/                   # Utility functions
    ├── MeshUtils.ts         # Mesh manipulation helpers
    ├── MaterialUtils.ts     # Material processing utilities
    └── RenderingUtils.ts    # Rendering helpers
```

This structure isolates core rendering concerns from business logic, allowing clean integration with the TailorCore backend while maintaining platform-specific rendering optimizations.

## Scene Structure and Naming Conventions

Babylon.js scenes in TailorCore follow consistent structural patterns:

1. **Root Container**:
    
    - Named `scene.rootNode`
    - Serves as the parent for all scene elements
    - Positioned at world origin (0,0,0)
2. **Avatar Container**:
    
    - Named `avatar_root`
    - Contains all avatar-related meshes and groups
    - Maintains reference to measurement data
3. **Garment Container**:
    
    - Named `garment_root`
    - Contains all garment meshes organized by category
    - Follows naming convention: `[garmentType]_[identifier]`
4. **Measurement Visualization**:
    
    - Named `measurement_container`
    - Contains visualization elements for measurement points
    - Toggleable visibility for debugging
5. **Camera Setup**:
    
    - Named `mainCamera`
    - ArcRotateCamera positioned for optimal viewing
    - Configured with constrained rotation limits

For consistent identification, all meshes follow these naming conventions:

- Avatar components: `avatar_[bodyPart]_mesh`
- Garment components: `[garmentType]_[component]_mesh`
- Measurement markers: `marker_[pointId]`
- Attachment points: `attachment_[pointId]`

## Observable and Event-Driven Architecture

TailorCore's Babylon.js implementation uses an event-driven approach through the Observable pattern:

1. **Core Observables**:
    
    - `onMeasurementsLoaded`: Triggered when measurement data is available
    - `onGarmentLoaded`: Triggered when a garment is loaded into the scene
    - `onFittingCompleted`: Triggered when garment fitting is complete
    - `onRenderingModeChanged`: Triggered when visualization mode changes
2. **Observable Registration**:
    
    ```typescript
    // Example observable registration
    scene.onMeasurementsLoaded = new BABYLON.Observable();
    scene.onMeasurementsLoaded.add((data) => {
      // Handle measurement data
    });
    ```
    
3. **State Management**:
    
    - Scene metadata contains current state
    - Components observe state changes rather than direct coupling
    - Central SceneManager coordinates state transitions
4. **Error Handling**:
    
    - Observable error events for critical failures
    - Fallback mechanisms for missing or invalid data
    - Graceful degradation when optimal rendering not possible

## Measurement Point Integration

Measurement data is integrated into the Babylon.js environment through:

1. **MeasurementSet Class**:
    
    ```typescript
    class MeasurementSet {
      public version: string;
      public gender: string;
      public points: MeasurementPoint[];
      
      public getPointById(id: string): MeasurementPoint {
        // Implementation
      }
      
      public getPositionById(id: string): BABYLON.Vector3 {
        // Implementation
      }
    }
    ```
    
2. **Loading Process**:
    
    - Primary loading from GLB metadata (embedded in avatar)
    - Secondary loading from API when needed
    - Validation before use in fitting calculations
3. **Visualization**:
    
    - Optional rendering of measurement points as spheres
    - Color-coding by measurement category
    - Size variation based on point importance
    - Text labels for debugging
4. **Fitting Integration**:
    
    - Attachment points aligned with measurement points
    - Scaling factors derived from measurement relationships
    - Real-time updates when measurements change

## Avatar Interaction and Real-Time Feedback

The Babylon.js implementation supports interactive feedback through:

1. **Interaction Modes**:
    
    - Rotation: 360° view of avatar and garments
    - Zoom: Close-up inspection of fit details
    - Measurement: Interactive display of measurements
    - Fitting: Real-time garment adjustment
2. **Real-Time Updates**:
    
    - Immediate visualization of measurement changes
    - Dynamic garment transformations
    - Visual indicators for fit quality
3. **Performance Considerations**:
    
    - LOD (Level of Detail) implementation for complex garments
    - Deferred updates for performance-intensive operations
    - Progressive loading for complex scenes
4. **Feedback Visualization**:
    
    - Color gradients to indicate fit tension
    - Numeric display of key measurements
    - Comparison to ideal fit metrics
    - Highlighting of problematic areas

The Babylon.js implementation prioritizes visual fidelity and consistent rendering across devices while maintaining the performance standards required for interactive garment visualization.