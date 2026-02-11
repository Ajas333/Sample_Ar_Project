# AR Session Not Starting - Troubleshooting

## Your Current Issue

**Symptoms:**
- AR is supported (device has AR capabilities)
- AR button appears and is clickable
- Status shows "not-presenting" 
- AR session doesn't actually launch

**This means:** The AR viewer tries to start but fails immediately.

---

## Solutions to Try (In Order)

### Solution 1: Test with Lightweight Model First ✅

**File:** `test-lightweight.html`

1. Deploy `test-lightweight.html` to your HTTPS domain
2. Open it on your mobile device
3. Tap the AR button
4. **Does it work?**
   - ✅ **YES** → Your `mode_2.glb` file has issues (see Solution 3)
   - ❌ **NO** → Browser/device compatibility issue (see Solution 4)

---

### Solution 2: Try Simplified Version

**File:** `test-simple.html` (just created)

This has:
- Custom AR button (not using shadow DOM)
- Different AR mode priority (scene-viewer first)
- Simplified configuration

Test this on your mobile device.

---

### Solution 3: Model File Issues 🔧

Even at 8MB, your `mode_2.glb` might have problems:

#### A. Check Model Validity
Visit: https://gltf.report/
1. Upload your `mode_2.glb`
2. Look for errors/warnings
3. Click "Compress" to re-export a clean version

#### B. Common Model Problems:
- ❌ **Too many polygons** (millions of triangles)
- ❌ **Complex materials** (PBR with many textures)
- ❌ **Large textures** (4K+ resolution)
- ❌ **Animations** (can cause issues on some devices)
- ❌ **Multiple meshes** (consider merging)

#### C. Model Requirements for Mobile AR:
- ✅ Under 10MB file size
- ✅ Less than 100k triangles
- ✅ Textures: 1024x1024 or smaller
- ✅ Simple materials
- ✅ No lights (AR uses real-world lighting)

---

### Solution 4: Browser/Device Issues 📱

#### Android Issues:
1. **Try Chrome** (not Firefox, Edge, etc.)
2. **Update Google Play Services for AR** (ARCore)
3. **Check ARCore support:** https://developers.google.com/ar/devices
4. **Grant camera permission** when prompted

#### iOS Issues:
1. **Use Safari** (Chrome doesn't support AR on iOS)
2. **iOS 12+ required**
3. **Check device:** iPhone 6S or newer, iPad 5th gen or newer

#### Check Browser:
```javascript
// Open browser console and type:
navigator.xr.isSessionSupported('immersive-ar')
```

---

### Solution 5: HTTPS & CORS Issues 🔒

1. **Verify HTTPS** - AR requires secure connection
2. **Check file access:**
   - `mode_2.glb` must be on same domain, OR
   - Server must send CORS headers

3. **Test file directly:**
   - Open: `https://yourdomain.com/mode_2.glb`
   - Should download/show file, not 404

---

### Solution 6: Try Different AR Modes Order

Edit your HTML, change this line:
```html
<!-- Current -->
ar-modes="webxr scene-viewer quick-look"

<!-- Try this instead -->
ar-modes="scene-viewer quick-look webxr"
```

**Why:** Android devices prefer `scene-viewer`, iOS prefers `quick-look`.

---

## Quick Diagnostic Commands

### Test 1: File Accessibility
Open browser console on mobile (Remote Debugging):
```javascript
fetch('mode_2.glb').then(r => console.log('File OK:', r.ok))
```

### Test 2: AR Support
```javascript
const mv = document.querySelector('model-viewer');
console.log('Can activate AR:', mv.canActivateAR);
```

### Test 3: Force AR Launch
```javascript
document.querySelector('model-viewer').activateAR();
```

---

## Most Likely Solution for You

Based on "not-presenting" error with 8MB model:

**Your model probably has:**
- Too many polygons (geometry too complex)
- Large textures (compress them)
- Incompatible materials

**Action:**
1. Upload `mode_2.glb` to https://gltf.report/
2. Click "Compress" with Draco compression
3. Reduce texture size to 512x512 or 1024x1024
4. Download the optimized version
5. Test again

---

## Alternative: Use Different Model Format

If problems persist, try converting your model:

1. **Blender** (free):
   - Import your current model
   - Export as GLB with these settings:
     - ✅ Draco compression
     - ✅ Limit texture size: 1024
     - ✅ Merge meshes
     - ❌ No animations (if not needed)

2. **Online tools:**
   - https://products.aspose.app/3d/conversion
   - Convert to GLB format

---

## Next Steps

1. ✅ Test `test-simple.html` (just created)
2. ✅ Test `test-lightweight.html` (known working model)
3. 📊 Upload `mode_2.glb` to gltf.report and check for issues
4. 🔧 Optimize/compress your model
5. 🚀 Test again

Let me know what happens with the lightweight test - that will tell us if it's your model or the device!
