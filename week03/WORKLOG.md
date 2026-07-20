# Detect walls and rooms from floor plan based on line detection only.

Input Image -> Convert to Gray -> Choose Detection Parameters -> Optional Morphological Filtering (For wifi and thick_only) -> Pass 1 : Multi Threshold Detection -> Pass 2 : Adaptive Threshold Detection -> Pass 3 : LSD Detection -> Pass 4 : Glass Wall Detection -> Pass 5 : Skeleton Detection -> Wall Classification -> Merge Duplicate Walls -> Return Final Walls

## Core Function: ``` detect_walls(image, sensitivity='high') ```
- It takes a floor plan image as input and returns a list of detected walls along with their properties such as start point, end point, orientation, length, material type, and thickness.
- Parameters
    - image: input floor plan image in BGR format
    - sensitivity: ```low, medium, high, wifi and thick_only``` - Determines how aggressively walls should be detected
- Procedure:
    - Convert Image to Grayscale
    - Morphological Filtering for wifi and thick_only
    - Multi-Pass Wall Detection
        - Multi-Threshold Detection
        - Adaptive Thresholding
        - Line Segment Detector (LSD)
        - Glass Wall Detection
        - Skeleton-Based Detection
    - Duplicate Removal (using set data structure provided bu python)
    - Wall Classification (using ``` _estimate_wall_thickness(gray, x1, y1, x2, y2) ``` function)
    - Merge Nearby Walls (using ``` _merge_nearby_walls(walls, distance_threshold=15) ``` function)
- Returns a list of dictionaries in form:
    - ```
        [
            {
                "start": (100,120),
                "end": (400,120),
                "length":300,
                "type":"horizontal",
                "material":"solid_wall",
                "thickness":5
            }
        ]
        ```

### Helper Function: ```_create_dense_area_mask(lines, width, height, grid_size=50, density_threshold=8)```
- Used to identify regions of the floor plan that contain a high concentration of detected line segments. The assumption is that these dense regions are usually text, furniture, legends, dimensions, or annotations, therefore are to be removed (later).

### Helper Function: ``` _estimate_wall_thickness(gray, x1, y1, x2, y2) ```
- used to estimate wall thickness by sampling perpendicular to the line.

### Helper Function: ``` _merge_nearby_walls(walls, distance_threshold=15) ```
- Used to combine multiple detections that actually represent the same physical wall.

### Function: ``` detect_rooms(image) ```
- Used to identify enclosed room regions in the floor plan.

### Function ``` generate_svg(walls, rooms, image_path, output_path) ```
- Used to generate a SVG with detected walls and rooms overlaid on the image.