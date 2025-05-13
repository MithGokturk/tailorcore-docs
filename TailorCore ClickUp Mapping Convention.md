## Task ID Usage in Code

TailorCore uses a consistent task identification system within code to maintain traceability between implementation and ClickUp tasks:

1. **Task ID Format**:
    
    - Prefix: `MVP-` (indicating Minimum Viable Product phase)
    - Number: Three-digit sequential identifier (e.g., `101`, `102`)
    - Complete format: `MVP-101`
2. **Comment Placement**:
    
    - Task IDs must appear in file header comments
    - Task IDs should be included in method-level comments for significant functionality
    - Complex implementations should reference task IDs at key decision points
3. **Standard Format**:
    
    ```csharp
    // MVP-101: Implement measurement validation logic
    public class MeasurementValidator
    {
        // MVP-101.1: Core validation method for measurement completeness
        public ValidationResult Validate(MeasurementSet measurements)
        {
            // Implementation...
        }
    }
    ```
    
4. **Subtask References**:
    
    - Use decimal notation for subtasks: `MVP-101.1`, `MVP-101.2`
    - Include in comments when implementing specific subtask requirements
    - Helpful for complex tasks broken down into multiple implementation steps

## Code-to-Task Traceability Strategy

TailorCore maintains bidirectional traceability between code and tasks:

1. **Code-to-Task Mapping**:
    
    - Each significant code file includes references to originating tasks
    - Task IDs in comments provide direct links to requirements
    - Implementation follows task acceptance criteria
    - Task checklists map to implementation components
2. **Task-to-Code Mapping**:
    
    - ClickUp tasks include links to relevant GitHub commits
    - Task comments reference specific files or components
    - Pull request descriptions list implemented tasks
    - Code review checklists verify task implementation
3. **Documentation Integration**:
    
    - Architecture documentation references related task IDs
    - Technical design decisions link to requirements tasks
    - Test cases map to feature tasks
    - API documentation includes origin task references
4. **Quality Assurance**:
    
    - Test scripts reference associated feature tasks
    - Bug reports link to original feature tasks
    - Bug fix commits reference both bug and original feature tasks
    - Regression test coverage mapped to task IDs

## End-to-End Task Implementation Sequence

TailorCore tasks follow a consistent implementation sequence:

1. **Domain Layer First**:
    
    - Begin with domain entities and business logic
    - Implement core models and validation rules
    - Establish domain services and interfaces
    - Ensure strong encapsulation of business rules
2. **Application Layer Second**:
    
    - Implement commands and queries
    - Create handlers for business operations
    - Define validators and application services
    - Establish cross-cutting concerns
3. **Infrastructure Layer Third**:
    
    - Implement repositories and data access
    - Create service implementations
    - Build adapters for external systems
    - Set up file handling and persistence
4. **API Layer Fourth**:
    
    - Create controllers and endpoints
    - Implement request/response mapping
    - Set up routing and configuration
    - Add middleware components
5. **UI Layer Last**:
    
    - Implement Babylon.js visualization
    - Create Unity editor components
    - Add user interface elements
    - Connect to API endpoints

This sequence ensures that core business logic is established before implementation details, following Clean Architecture principles.

## Commit Message Convention

Commit messages in TailorCore must adhere to a consistent format that includes task references:

1. **Basic Format**:
    
    ```
    [MVP-101] Add measurement validation logic
    
    Implement core validation rules for measurement points including:
    - Completeness validation
    - Range validation
    - Symmetry validation
    
    Part of the measurement system implementation.
    ```
    
2. **Key Components**:
    
    - Task ID in square brackets: `[MVP-101]`
    - Short, imperative summary (under 50 characters)
    - Detailed description with bullet points as needed
    - Context or relation to larger features
    - Co-authored-by tags for pair programming
3. **Multiple Tasks**:
    
    ```
    [MVP-101][MVP-102] Implement measurement system
    ```
    
4. **Special Indicators**:
    
    - `[FIX]` for bug fixes
    - `[REFACTOR]` for code refactoring
    - `[DOCS]` for documentation updates
    - `[TEST]` for test additions
5. **Branch Naming**:
    
    - Feature branches: `feature/MVP-101-measurement-validation`
    - Bug fix branches: `fix/MVP-101-measurement-validation-fix`
    - Release branches: `release/v0.1.0`

Consistent task reference in both code comments and commit messages ensures clear traceability throughout the development lifecycle, facilitating code reviews, bug tracking, and feature auditing.