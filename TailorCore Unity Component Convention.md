## Runtime and Editor Script Separation

TailorCore's Unity implementation maintains strict separation between runtime and editor scripts:

1. **Runtime Scripts**:
    
    - Located in `src/unity/Core/` and subfolders
    - Execute during normal application operation
    - Focus on core functionality and rendering
    - Must operate without editor dependencies
    - Named with clear functionality indicators (e.g., `MeasurementSet.cs`, `GarmentLoader.cs`)
2. **Editor Scripts**:
    
    - Located in `src/unity/UI/Editors/`
    - Only execute within Unity Editor environment
    - Focus on authoring tools and workflow enhancement
    - Named with `Editor` suffix or contained in `Editor` namespace
    - Example: `MeasurementToolsWindow.cs`, `GarmentEditorWindow.cs`

This separation ensures that runtime functionality remains performant and clean, while editor-specific functionality doesn't bloat the runtime build.

## MonoBehaviour and ScriptableObject Usage

TailorCore follows specific guidelines for component structure:

1. **MonoBehaviour Usage**:
    
    - For components that require lifecycle hooks (Update, Start, etc.)
    - For components attached to scene GameObjects
    - For components managing visual representation
    - Should have minimal dependencies and focused responsibility
    - Example: `MeasurementMarker.cs`, `GarmentScaler.cs`
2. **ScriptableObject Usage**:
    
    - For shared configuration data
    - For data that persists between scenes
    - For template-based data definition
    - Named with clear type indicators (e.g., `MeasurementConfig`, `GarmentCategory`)
    - Example: `MeasurementRules.cs`, `GarmentTemplate.cs`
3. **Core Principles**:
    
    - Components should have a single responsibility
    - Prefer composition over inheritance
    - Limit direct GameObject references
    - Use events for cross-component communication
    - Keep MonoBehaviours thin with logic in non-MonoBehaviour classes

## Unity Folder Structure Conventions

```
src/unity/
├── Core/                           # Runtime components
│   ├── MeasurementSystem/          # Measurement-related components
│   │   ├── MeasurementSet.cs       # Container for measurement data
│   │   ├── MeasurementMarker.cs    # Visual representation of measurement point
│   │   └── MeasurementValidator.cs # Validation logic
│   ├── GarmentSystem/              # Garment-related components
│   │   ├── GarmentLoader.cs        # GLB loading and processing
│   │   ├── GarmentScaler.cs        # Scaling based on measurements
│   │   └── AttachmentPointController.cs # Attachment point management
│   ├── AvatarSystem/               # Avatar-related components
│   │   ├── AvatarLoader.cs         # Avatar loading and setup
│   │   └── AvatarController.cs     # Avatar control and animation
│   └── API/                        # API communication
│       ├── ApiConnector.cs         # Base API communication
│       └── MeasurementEndpoints.cs # Measurement-specific API calls
├── UI/                             # UI components
│   ├── Editors/                    # Editor-only UI
│   │   ├── MeasurementToolsWindow.cs # Measurement editing tools
│   │   └── GarmentEditorWindow.cs  # Garment editing tools
│   └── Components/                 # Runtime UI components
│       ├── FilePickerComponent.cs  # File selection UI
│       └── MeasurementDisplayComponent.cs # Measurement visualization
└── Utils/                          # Utilities
    ├── GlbUtils.cs                 # GLB file utilities
    ├── MeasurementUtils.cs         # Measurement calculation utilities
    └── DebugGizmoDrawer.cs         # Debug visualization
```

This structure ensures clear separation of concerns while maintaining discoverability. Each component resides in a location that reflects its function in the overall system.

## Component-Based Architecture Rules

TailorCore's Unity implementation adheres to these component architecture rules:

1. **Component Responsibilities**:
    
    - Each component must have a clearly defined, single responsibility
    - Components should be composable and reusable
    - State should be encapsulated within components
    - Public APIs should be minimal and well-documented
2. **Dependency Management**:
    
    - Components should minimize direct dependencies on other components
    - Use dependency injection or service locator for required dependencies
    - Prefer interfaces over concrete implementations for dependencies
    - Use events for loose coupling between components
3. **Data Flow**:
    
    - Unidirectional data flow preferred (parent to child)
    - Changes should propagate through well-defined channels
    - Avoid circular dependencies between components
    - Use Command pattern for complex operations
4. **Lifetime Management**:
    
    - Clear ownership hierarchies for components
    - Explicit initialization and cleanup methods
    - Proper handling of destroyed objects
    - Avoidance of static state except where absolutely necessary

## Measurement Renderer and Prefab Usage

The measurement visualization system uses:

1. **MeasurementMarker Component**:
    
    - Attached to GameObjects representing measurement points
    - Stores reference to measurement ID and position
    - Handles visual representation in scene
    - Supports selection and manipulation
2. **DebugLandmarkGizmo Component**:
    
    - Provides editor-time visualization
    - Renders spheres, labels, and connection lines
    - Color-coding based on measurement categories
    - Supports visibility toggles for different measurement sets
3. **Prefab Structure**:
    
    - Measurement marker prefabs for consistency
    - Prefab variants for different measurement types
    - Runtime instantiation for dynamic measurement creation
    - Example: `MeasurementMarker.prefab`, `PrimaryMarker.prefab`, `SecondaryMarker.prefab`
4. **Visualization Guidelines**:
    
    - Use consistent visual language across all measurement points
    - Scale visualization based on viewport distance
    - Provide clear indication of selected points
    - Use color to distinguish different measurement categories
    - Enable toggling of visualization layers

This component-based approach ensures flexible, maintainable code while adhering to Unity best practices and TailorCore's architecture principles.