# Engineering Notes: Hierarchical Scene Graph Transformations & Dirty-Bit Invalidation

**Date:** November 24, 2026  
**Category:** UI Systems Architecture / 2D Graphics Engines  

## Overview
Techniques and mathematical data structures for managing deep scene graph hierarchies in interactive 2D graphics systems, focusing on local-to-world affine matrix propagation, lazy evaluation with dirty-bit invalidation, and localized subtree bounding-box recalculation.

### Key Learnings & Implementations
1. **Local-to-World Affine Matrix Concatenation**:
   - Representing hierarchical spatial relationships using 3x3 2D affine transformation matrices (`[a, b, c, d, tx, ty]`).
   - Composing child transform matrices relative to parents via matrix multiplication (`worldMatrix = parentWorldMatrix * localMatrix`), ensuring localized rotation, translation, and scale cascades naturally down the component tree.
2. **Lazy Evaluation & Dirty-Bit Propagation**:
   - Avoiding costly full-tree matrix recalculations every frame by establishing a reactive dirty-flag propagation system.
   - When a node's local transform changes, marking its dirty bit and propagating the flag down to descendant subtrees while leaving sibling branches untouched; computing world transforms lazily on-demand during the render pass or hit-test query.
3. **Subtree Bounding Box & Frustum Culling**:
   - Deriving hierarchical Axis-Aligned Bounding Boxes (AABB) enclosing all transformed child geometries for rapid spatial pruning.
   - Leveraging cached parent bounds to cull entire off-screen subtrees during the paint traversal before individual primitives are evaluated, maintaining steady frame budgets even with thousands of nested scene nodes.
