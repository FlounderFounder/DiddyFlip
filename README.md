# DiddyFlip

A pixel-art style coin flip simulator with physics, set in a supermarket environment with baby oil bottles.

## Quick Start
Simply open `index.html` in a web browser to play!

---

## Customization Guide

---

## Customization Options

### 1. Environment Background

**Location in code:** Around line ~720 (search for `Create supermarket skybox background`)

#### Option A: Solid Color Background
Replace the skybox canvas creation with a solid color:

```javascript
// Find this section and modify:
const skyboxMat = new THREE.MeshBasicMaterial({
    color: 0xYOURCOLOR,  // e.g., 0x87CEEB for sky blue
    side: THREE.BackSide
});
```

#### Option B: Custom Image Background
Add this code AFTER the skybox mesh is created (after `window.skyboxMesh = skybox;`):

```javascript
// Load custom environment image
const envLoader = new THREE.TextureLoader();
envLoader.load('YOUR_IMAGE_URL_OR_PATH', function(texture) {
    texture.wrapS = THREE.RepeatWrapping;
    texture.wrapT = THREE.RepeatWrapping;
    
    // Calculate panoramic tiling (preserves aspect ratio)
    const img = texture.image;
    const cylinderAspect = (2 * Math.PI * 180) / 100; // ~11.3
    const imageAspect = img.width / img.height;
    const repeatX = Math.ceil(cylinderAspect / imageAspect);
    
    texture.repeat.set(repeatX, 1);
    
    window.skyboxMesh.material.map = texture;
    window.skyboxMesh.material.color.set(0xffffff);
    window.skyboxMesh.material.needsUpdate = true;
});
```

---

### 2. Coin Face Images

**Location in code:** Around line ~350 (search for `createCoinTexture`)

#### Default Behavior
The coin uses procedurally generated "H" and "T" textures.

#### Custom Images for Coin Faces
Replace the texture creation with image loading:

```javascript
// Replace headsTexture and tailsTexture creation with:
const textureLoader = new THREE.TextureLoader();

// Heads side
const headsTexture = textureLoader.load('YOUR_HEADS_IMAGE_URL');
headsTexture.minFilter = THREE.NearestFilter;
headsTexture.magFilter = THREE.NearestFilter;

// Tails side  
const tailsTexture = textureLoader.load('YOUR_TAILS_IMAGE_URL');
tailsTexture.minFilter = THREE.NearestFilter;
tailsTexture.magFilter = THREE.NearestFilter;
```

**Recommended:** Square images (512x512) work best.

---

### 3. Floor Texture

**Location in code:** Around line ~485 (search for `Warmer beige/cream tiles`)

Modify the canvas drawing code to change tile colors, size, or pattern:

```javascript
// Tile color (RGB values)
const r = 240 - variation;  // Red
const g = 230 - variation;  // Green  
const b = 210 - variation;  // Blue

// Tile size
const tileSize = 64;  // Change for larger/smaller tiles

// Grout color
ctx.strokeStyle = '#9ca3af';
```

---

### 4. Scene Colors & Fog

**Location in code:** Around line ~880 (search for `Update lighting and fog`)

```javascript
scene.background = new THREE.Color(0xebe5d9);  // Background color
scene.fog = new THREE.Fog(0xebe5d9, 40, 250);  // Fog color, near, far
```

---

### 5. Physics Settings

**Location in code:** Around line ~430 (search for `Cannon.js World`)

```javascript
world.gravity.set(0, -9.82, 0);     // Gravity (x, y, z)
world.solver.iterations = 15;        // Collision accuracy
CONFIG.flipForce = 25;               // Default flip power
CONFIG.flipTorque = 10;              // Spin amount
```

---

### 6. Coin Size

**Location in code:** Around line ~215 (search for `CONFIG`)

```javascript
CONFIG.coinRadius = 1.5;      // Coin radius
CONFIG.coinThickness = 0.2;   // Coin thickness
```

---

### 7. Bottle Spawning

**Location in code:** Around line ~830 (search for `COIN_SPAWN_CLEARANCE`)

```javascript
const COIN_SPAWN_CLEARANCE = 25;    // Clear zone around coin spawn
const BOTTLE_MIN_DISTANCE = 20;      // Minimum distance between bottles

// Number of bottles:
for (let i = 0; i < 8; i++)  // Laying bottles (change 8)
for (let i = 0; i < 6; i++)  // Standing bottles (change 6)
```

---

### 8. Camera Settings

**Location in code:** Around line ~395 (search for `PerspectiveCamera`)

```javascript
camera = new THREE.PerspectiveCamera(60, ...);  // FOV (60 degrees)
camera.position.set(0, 2, 8);                    // Initial position

// Camera floor limit (line ~1175)
const MIN_CAMERA_HEIGHT = 1.5;  // Minimum height above floor
```

---

## Quick Reference: Color Codes

| Element | Default Color | Location |
|---------|--------------|----------|
| Background | `0xebe5d9` | scene.background |
| Floor tiles | RGB(240,230,210) | Floor canvas |
| Coin gold | `0xfcd34d` | createCoinTexture |
| Coin edge | `0xb59e5f` | CONFIG.colors.coinSide |
| Shelf metal | `0x64748b` | createSupermarketShelf |
| Bottle cap | `0x3b82f6` | createFullSizeBottle |

---

## File Structure

```
pixel-coin-flip.html    # Main application (single file)
pixel-coin-flip-README.md   # This documentation
```

All code is contained in a single HTML file for easy deployment.

