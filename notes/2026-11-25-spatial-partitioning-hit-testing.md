# Engineering Notes: Spatial Partitioning & Accelerated Hit-Testing in 2D Scene Graphs

**Date:** November 25, 2026  
**Category:** UI Systems Architecture / 2D Graphics Engines  

## Overview
Architectural strategies and spatial data structures for accelerating interactive hit-testing, ray-casting, and pointer dispatching across dense 2D scene graphs, emphasizing dynamic quadtree partitioning, inverse affine transformation caching, and composite event propagation.

### Key Learnings & Implementations
1. **Dynamic Quadtree Spatial Indexing**:
   - Constructing loose adaptive quadtrees to partition 2D world-space coordinates, enabling logarithmic $O(\log N)$ query times rather than linear scans across thousands of interactive scene graph nodes.
   - Implementing loose boundary thresholds (expanding cell bounds by 15-20%) to mitigate thrashing when animated entities or draggable UI components oscillate across quadrant boundaries.
2. **Hierarchical Inverse Coordinate Mapping**:
   - Inverting cached 3x3 world affine matrices (`worldToLocal = worldMatrix^-1`) to transform screen-space pointer coordinates directly into the node's untransformed local space.
   - Performing point-in-polygon and oriented bounding box (OBB) hit tests against native, axis-aligned local geometries, completely avoiding complex transformed polygon intersection math.
3. **Capture, Target, and Bubble Event Dispatch Pipeline**:
   - Structuring an event dispatch pipeline mirroring the DOM specification: traversing from root to target during the capture phase, validating geometry hits, and bubbling interaction events back upward.
   - Providing mechanisms for early event cancellation (`stopPropagation()`) at intermediate containers to prevent redundant event processing on obscured or underlying elements.
