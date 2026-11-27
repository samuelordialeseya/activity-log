# Engineering Notes: Offscreen Surface Caching & Layer Compositing Strategies

**Date:** November 27, 2026  
**Category:** UI Systems Architecture / 2D Graphics Engines  

## Overview
Techniques and memory retention policies for accelerating rendering pipelines through offscreen framebuffer caching, retained layer compositing, and dirty region boundary clipping during continuous viewport panning, scaling, and interactive UI updates.

### Key Learnings & Implementations
1. **Retained Subtree Framebuffer Caching**:
   - Isolating complex, static vector hierarchies or heavyweight SVG/canvas subtrees into dedicated offscreen textures (Frame Buffer Objects / render targets).
   - Bypassing full recursive subtree traversals during layout pans, pinches, or scroll interactions by drawing the pre-rasterized texture quad with a single blit operation.
2. **Granular Dirty Region Isolation & Invalidation Flags**:
   - Classifying property mutations into transform changes (which only update the composited layer matrix) versus structural/geometry modifications (which flag the raster cache as dirty).
   - Propagating dirty rect bounds up the layer tree to clip offscreen redraws strictly to modified bounding areas rather than repainting entire subsurfaces.
3. **GPU VRAM Budgeting & LRU Cache Eviction**:
   - Enforcing memory ceilings on composite texture allocations using a weighted Least-Recently-Used (LRU) cache policy based on surface dimension and time since last render pass.
   - Falling back to direct inline rendering or lower-resolution mip proxies when VRAM allocation thresholds are approached to avoid frame stalls or device memory pressure.
