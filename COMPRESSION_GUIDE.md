# Model Compression Guide

## Quick Fix: Compress Your GLB File

Your model is 25.6MB which is too large for mobile AR. Follow these steps:

### Method 1: Online Tool (Easiest)
1. Visit: https://gltf.report/
2. Upload `model_1.glb`
3. Click "Compress" with these settings:
   - ✅ Draco compression
   - ✅ Texture compression (WebP or KTX2)
   - Resolution: 1024x1024 or 512x512
4. Download optimized file
5. Replace your original model

### Method 2: Desktop Tool
1. Download glTF Sample Viewer: https://github.com/KhronosGroup/glTF-Sample-Viewer
2. Import your model
3. Export with compression enabled

### Method 3: Command Line (Advanced)
```bash
# Install gltf-transform
npm install -g @gltf-transform/cli

# Compress model
gltf-transform optimize model_1.glb model_1_compressed.glb --compress

# With texture resizing
gltf-transform resize model_1.glb model_1_optimized.glb --width 1024 --height 1024
```

## Target Sizes
- ✅ Excellent: < 5MB
- ⚠️ Acceptable: 5-10MB
- ❌ Too Large: > 10MB

## What Gets Compressed
- Meshes (Draco compression)
- Textures (WebP/JPEG instead of PNG)
- Remove unused data
- Simplify geometry
