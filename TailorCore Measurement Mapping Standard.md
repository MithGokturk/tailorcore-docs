## Measurement Classification

TailorCore's measurement system classifies anatomical landmarks into two primary categories:

1. **Primary Landmarks**: Essential reference points that define the core structure of the avatar's measurements. These points are mandatory for all garment types and serve as anchors for the fitting system.
    
2. **Secondary Landmarks**: Supplementary points that provide additional precision for specific garment types. These are organized into subcategories:
    
    - Secondary.Chest: Additional points for upper body garments
    - Secondary.Waist: Additional points for mid-body measurements
    - Secondary.Hip: Additional points for lower body garments
    - Secondary.Limb: Additional points for arms and legs

Each landmark category has specific validation rules and usage contexts within the fitting algorithm.

## Coordinate System

TailorCore uses a standardized coordinate system with the following properties:

- **Origin**: Located at the floor between the feet of a standing avatar in T-pose
- **Units**: All measurements stored in centimeters (cm)
- **Axis Orientation**:
    - Y-axis: Vertical dimension (height)
    - X-axis: Horizontal dimension (width, left-to-right)
    - Z-axis: Depth dimension (front-to-back)
- **Right-handed Coordinate System**: Consistent across Unity and Babylon.js

Coordinate values must adhere to anatomically plausible ranges:

|Body Region|X Min/Max (cm)|Y Min/Max (cm)|Z Min/Max (cm)|
|---|---|---|---|
|Torso|-25.0 / 25.0|70.0 / 170.0|-20.0 / 20.0|
|Arms|-60.0 / 60.0|70.0 / 170.0|-30.0 / 30.0|
|Legs|-25.0 / 25.0|0.0 / 80.0|-25.0 / 25.0|
|Head|-15.0 / 15.0|150.0 / 190.0|-15.0 / 15.0|

## Naming Convention Rules

Measurement point identifiers follow these strict naming conventions:

1. All lowercase with underscores as separators
2. No spaces or special characters
3. Anatomical location first, followed by qualifiers
4. Directional indicators as suffixes:
    - Left/right: `_l` or `_r` suffix
    - Front/back: `_front` or `_back` suffix
    - Center/middle: `_center` or `_mid` suffix

Examples of properly formatted measurement point names:

- `neck_base_front`: Front base of the neck
- `shoulder_tip_r`: Tip of the right shoulder
- `waist_center`: Center point of the waist
- `hip_l`: Left side hip point
- `knee_cap_r`: Right knee cap
- `thigh_mid_l`: Middle point of left thigh

Points that are symmetrical across the sagittal plane follow a pairing convention with `_l` and `_r` suffixes (e.g., `shoulder_tip_l` and `shoulder_tip_r`).

## Measurement Data Validation

TailorCore implements a multi-layer validation process for measurement data:

1. **Schema Validation**: Ensures the measurement data structure conforms to the defined JSON schema:
    
    ```json
    {
      "version": "string",
      "gender": "string (enum: female, male)",
      "points": [
        {
          "id": "string",
          "x": "number",
          "y": "number",
          "z": "number"
        }
      ],
      "metadata": {
        "source": "string",
        "timestamp": "string (ISO 8601)"
      }
    }
    ```
    
2. **Completeness Validation**: Verifies that all required points for the specific gender and garment category are present.
    
3. **Range Validation**: Ensures coordinates fall within anatomically plausible ranges.
    
4. **Symmetry Validation**: For paired points (left/right), verifies appropriate symmetry:
    
    - X-coordinate: Must be approximately mirrored (within 0.5 cm tolerance)
    - Y-coordinate: Must be within 1.0 cm
    - Z-coordinate: Must be within 1.0 cm
5. **Distance Validation**: Checks that adjacent points maintain appropriate distances based on anatomical constraints.
    

Validation is performed at key stages:

- During measurement data creation in Unity
- During API transfer
- Before garment fitting in both Unity and Babylon.js

## Integration with Garment Fitting Logic

The measurement system drives garment fitting through these mechanisms:

1. **Backend**:
    
    - Validates measurement completeness against garment requirements
    - Calculates derived measurements (circumferences, lengths)
    - Stores measurement-to-garment relationships
2. **Unity**:
    
    - Places measurement markers in 3D space
    - Links measurement points to garment attachment points
    - Provides visualization tools for measurement debugging
    - Scales garment meshes based on measurement relationships
3. **Babylon.js**:
    
    - Loads measurement data from GLB metadata or API
    - Positions garments based on avatar measurement points
    - Provides real-time visualization of measurement points
    - Implements fallback strategies for missing measurements

Measurement-driven garment transformations follow these steps:

1. Calculate scaling factors based on key measurements
2. Apply primary positioning based on anchor points
3. Apply secondary adjustments for specific garment regions
4. Validate final positioning against collision constraints

This standardized measurement system ensures consistent garment fit across both Unity and Babylon.js environments.
