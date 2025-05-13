## Purpose and Scope

TailorCore is a comprehensive platform that enables the creation, measurement, and visualization of digital garments on 3D avatars. The MVP delivers a local-first solution that allows users to create, edit, and visualize garments with accurate measurements across multiple rendering environments.

Key objectives of the TailorCore MVP include:

- Creating a seamless workflow from garment design to virtual try-on
- Supporting precise measurement anchoring for accurate fit visualization
- Enabling consistent 3D visualization across different environments
- Operating entirely locally without requiring internet connectivity
- Standardizing the asset pipeline to ensure visual consistency

## Primary Modules

The TailorCore system consists of four primary modules:

1. **Backend Module**: Core business logic implementing domain entities, application services, and infrastructure components following Clean Architecture principles.
    
2. **Unity Module**: Firm-side authoring environment for garment creation, editing, and measurement anchoring.
    
3. **Babylon.js Module**: Web-based visualization environment for garment try-on and fit assessment.
    
4. **API Module**: Local REST API serving as the communication layer between Unity, Babylon.js, and the core backend services.
    

## Clean Architecture Implementation

TailorCore follows a strict Clean Architecture pattern with four distinct layers:

1. **Domain Layer**: Contains business entities, value objects, and domain services that express core business rules with no external dependencies.
    
    - Located in `src/backend/domain/`
    - Organized by bounded contexts (measurement, garment, avatar, pipeline)
2. **Application Layer**: Houses application-specific business rules, commands, queries, and interfaces.
    
    - Located in `src/backend/application/`
    - Implements CQRS pattern with MediatR
3. **Infrastructure Layer**: Provides concrete implementations of interfaces defined in the application layer.
    
    - Located in `src/backend/infrastructure/`
    - Includes persistence, file storage, and rendering services
4. **API Layer**: Delivers REST APIs for communication between modules.
    
    - Located in `src/backend/api/`
    - Uses ASP.NET Core Minimal APIs

## Folder Layout and Naming Conventions

```
src/
├── backend/
│   ├── domain/              # Domain entities and business logic
│   │   ├── common/          # Shared domain components
│   │   ├── measurement/     # Measurement-related domain
│   │   ├── garment/         # Garment-related domain
│   │   ├── avatar/          # Avatar-related domain
│   │   └── pipeline/        # Processing pipeline domain
│   ├── application/         # Application services and use cases
│   ├── infrastructure/      # Implementation details
│   └── api/                 # API endpoints
├── frontend/                # Web client implementation
│   ├── core/                # Core services and utilities
│   ├── modules/             # Feature modules
│   ├── babylon/             # Babylon.js integration
│   └── ui/                  # User interface components
├── unity/                   # Unity implementation
│   ├── Core/                # Core components and services
│   ├── UI/                  # Editor and runtime UI
│   └── Utils/               # Utility scripts
├── shared/                  # Shared code between platforms
│   ├── cs/                  # C# shared code
│   └── ts/                  # TypeScript shared code
└── tests/                   # Test implementations
```

Key naming conventions:

- **Files**: PascalCase for classes, camelCase for modules
- **Folders**: lowercase for backend structure, PascalCase for Unity folders
- **Namespaces**: TailorCore.[Module].[Context]
- **Classes**: Descriptive, role-based names (e.g., MeasurementValidator, GarmentScaler)

## Technology Stack

TailorCore MVP utilizes the following technologies:

- **Backend**:
    
    - ASP.NET Core 8.0 LTS
    - MediatR for CQRS implementation
    - Local file-based storage (JSON)
    - In-process messaging
- **Frontend**:
    
    - TypeScript for web client
    - Babylon.js 6.0+ for WebGL rendering
    - Progressive Web App capabilities
- **Unity**:
    
    - Unity 2022.3 LTS
    - Custom editor extensions
    - GLB import/export pipeline
- **Shared**:
    
    - GLB (glTF 2.0 binary) as the primary asset format
    - JSON schema for measurement data exchange
    - Standardized coordinate system (Y-up, cm units)

All components operate locally, without dependencies on cloud services or external APIs, enabling a completely offline user experience.