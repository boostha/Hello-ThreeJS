# Hello Three.JS

### 1. Download three.js starter pack
    - https://github.com/designcourse/threejs-webpack-starter
    
### 2. Open in VS Code and NPM i

### 3. npm run dev

### 4. Script.JS
    - canvas = output to index.html
    
    - Needs 3 things to work
        - Scene
        - Camera
        - Renderer
        
    - Animation
        - Clock (elapsedTime) = Timer for animations so it is the same for every user
        - window.requestAnimationFrame(tick) = calls the next frame
        
### 5. Objects + Materials
    - Objects needs 3 things
        - Geometry (Shape of object)
            - Lots of geometry, main ones are box, sphere, cylinder, torus, plane
            - https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene
            
        - Material (Skin of object)
            - Basic, Normal, Standard, Toon
            
        - Mesh (Glue)
        
### 6. Delete Geometry, Materials and Mesh

### 7. Make new geometry
    - const sphereGeometry = new THREE.SphereGeometry(15, 32, 16)
    - (radius, widthSegments, heightSegments)
    
### 8. Make Material
    - const sphereMaterial = new THREE.MeshBasicMaterial( { color: 0xffffff } );
    
### 9. Make Mesh
    - const sphere = new THREE.Mesh( sphereGeometry, sphereMaterial );
    
### 10. Add to Scene
    - scene.add(sphere)

    - change sphereGeometry radius from 15 to 2
    - change camera.position.z to 5

    - input (wireframe: true) in material to visualise width and height segments

    - comment out pointlight
    - Switch THREE.MeshBasicMaterial to THREE.MeshStandardMaterial
    - comment in pointlight and add ambient light
        - const ambientLight = new THREE.AmbientLight(0xffffff, 0.1)
        - scene.add(ambientLight)
### 11. Textures
    - https://3dtextures.me/2021/11/16/tiles-048/
    
    - add textures to static
    
    - add textureloader under scene
        - const textureloader = new THREE.TextureLoader()
        
    - import textures
        - const aOTexture = textureloader.load('/tiles2/ambientOcclusion.jpg')
        - const baseTexture = textureloader.load('/tiles2/basecolor.jpg')
        - const heightTexture = textureloader.load('/tiles2/height.png')
        - const metallicTexture = textureloader.load('/tiles2/metallic.jpg')
        - const normalTexture = textureloader.load('/tiles2/normal.jpg')
        - const roughnessTexture = textureloader.load('/tiles2/roughness.jpg')
        
### 12. Add Textures to material
    - map: baseTexture
        - base texture
        
    - normalMap: normalTexture
        - adds texture so it doesnt look 2D
        - The texture to create a normal map. The RGB values affect the surface normal for each pixel fragment and change the way the color is lit. Normal maps do not change the actual shape of the surface, only the lighting. In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness. 
        
    - aoMap: aOTexture
        - grayscale image that will fake shadows (add shadows to where its dark)
        
        - Nothing happens we need to add another set of UVs
        - UV = the coordinates that help position the textures on the geometries
        
        - add under mesh
        - sphere.geometry.setAttribute('uv2', new THREE.BufferAttribute(sphere.geometry.attributes.uv.array, 2))
        
        - Did it work? increase intensity on aO map
        - aoMapIntensity: 1 / 2 / 5 / 10 go back to 2
        
    - displacementMap: heightTexture
        - Looks ugly because sphere does not have enough vertices (widthSegments and heightSegments)
        - (2, 64, 64) / (2, 128, 128)
        
        - Looks alittle bit better but we have to turn down the scale
        - displacementScale: 1 / 0.1 / 0.2
        
    - metalnessMap: metallicTexture
        - The blue channel of this texture is used to alter the metalness of the material.
        
    - metalness: 0
        - Non-metallic materials such as wood or stone use 0.0, metallic use 1.0, with nothing (usually) in between
        - 0 / 0.3 / 0.7 / 1
        - change point light to 1.2 and ambientlight to 0.2
        - looks okay but we're missing one more thing
        
    - roughnessMap: roughnessTexture
        - The green channel of this texture is used to alter the roughness of the material
        - lights reflection
        
    - roughness : 0 / 1 / 2 / 3
        - How rough the material appears. 0.0 means a smooth mirror reflection, 1.0 means fully diffuse. Default is 1.0.
        
### Animate Sphere
    - sphere.rotation.y = .5 * elapsedTime
    - sphere.rotation.x = .5 * elapsedTime