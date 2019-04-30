---
title: three.js绘制土星视频
date: 2018-12-21 09:00:00
tags: [three.js]
categories: three.js
---

### 项目步骤

#### 创建一个渲染器

```
<html>
  <head>
    <meta charset=utf-8>
    <title>My first three.js app</title>
    <style>
      body { margin: 0; }
      canvas { width: 100%; height: 100% }
    </style>
  </head>
  <body>
    <script src="js/three.js"></script>
    <script>
      var renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);
      // 多个场景渲染时，不清除上个场景渲染的内容
      renderer.autoClear = false
    </script>
  </body>
</html>
```

####  加载最大背景

```
var backgroundScene = new THREE.Scene()
// 正视投影相机
var backgroundCamera = new THREE.OrthographicCamera(-2, 2, -1, 1, 0.1, 1000000)
backgroundCamera.position.z = 10
var backgroundGeometry = new THREE.PlaneGeometry(4, 1, 0)
var backgroundTexture = new THREE.TextureLoader().load('/assets/background.png')
var backgroundMaterial = new THREE.MeshBasicMaterial({
  map: backgroundTexture,
  color: 0xffffff,
  transparent: true,
  side: THREE.DoubleSide,
  opacity: options.backgroud.opacity
})
var backgroundPlane = new THREE.Mesh(backgroundGeometry, backgroundMaterial)
backgroundScene.add(backgroundPlane)
backgroundMaterial.depthWrite = false
```

#### 加载  石头背景

```
var scene = new THREE.Scene()
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000000)
camera.position.z = 10

var backMeteorsGeometry = new THREE.PlaneGeometry(4, 1, 0)
var backMeteorsTexture = new THREE.TextureLoader().load('/assets/backMeteors.png')
var backMeteorsMaterial = new THREE.MeshBasicMaterial({
	map: backMeteorsTexture,
	color: 0xffffff,
	transparent: true,
	side: THREE.DoubleSide,
	opacity: options.backMeteors.opacity,
	blending: THREE.AdditiveBlending
})
var backMeteorsPlane = new THREE.Mesh(backMeteorsGeometry, backMeteorsMaterial)
scene.add(backMeteorsPlane)
backMeteorsMaterial.depthWrite = false
```

#### 石头效果

```
var rockGroups = []
var rockContainers = new THREE.Group()
for (var m = 0; m < 1000; m++) {
  var rockGroup = new THREE.Group()
  var rockGeometry = new THREE.OctahedronGeometry(30)
  var rockLineMaterial = new THREE.LineBasicMaterial({
    color: 0xffffff
  })
  var rockMaterial = new THREE.MeshBasicMaterial({
    shading: THREE.SmoothShading,
    color: options.rockGroups.color
  })

  // 给石头添加变形
  for (var b = 0; b < rockGeometry.vertices.length; b++) {
    var vertice = rockGeometry.vertices[b]
    vertice.x = (Math.random() - 0.5) * 100 + vertice.x
    vertice.y = (Math.random() - 0.5) * 100 + vertice.y
    vertice.z = (Math.random() - 0.5) * 100 + vertice.z
  }

  rockGroup.add(new THREE.LineSegments(new THREE.WireframeGeometry(rockGeometry), rockLineMaterial))
  rockGroup.add(new THREE.Mesh(rockGeometry, rockMaterial))

  rockGroup.position.x = (Math.random() - 0.5) * 10000 - 60
  rockGroup.position.y = (Math.random() - 0.5) * 10000 - 60
  rockGroup.position.z = (Math.random() - 0.5) * 100 - 60

  rockContainers.add(rockGroup)
  rockGroups.push(rockGroup)
}
scene.add(rockContainers)
```

#### 土星流动效果

```
var vertShader = document.getElementById('vertexShader').textContent
var fragShader = document.getElementById('fragmentShader').textContent
var planetGeometry = new THREE.SphereGeometry(200, 100, 100)
var planetTexture = new THREE.TextureLoader().load('/assets/planet.png')
var shaderUniforms = {
  planetTexture: { type: 't', value: planetTexture },
  time: { type: 'f', value: 0 },
  flowWaveRadius: { type: 'f', value: 0.1 },
  flowIndex: { type: 'f', value: 0 }
}
var planetMaterial = new THREE.ShaderMaterial({
  uniforms: shaderUniforms,
  vertexShader: vertShader,
  fragmentShader: fragShader
})
var planetPlane = new THREE.Mesh(planetGeometry, planetMaterial)
scene.add(planetPlane)
```

#### 添加光照

```
// 环境光
var ambient = new THREE.AmbientLight(0x555555)
scene.add(ambient)

// 点光源
var light = new THREE.DirectionalLight(0xffffff)
light.position = camera.position
scene.add(light)
```

#### 添加插件

```
// 初始化性能插件
var stats
function initStats() {
	stats = new Stats()
	document.body.appendChild(stats.dom)
}

// 生成gui设置配置项
var gui
function initGui() {
  //声明一个保存需求修改的相关数据的对象
  gui = {
    backgroundOpacity: 0.5,
    backMeteorsSpeed: 1,
    backMeteorsOpacity: 0.5,
    rockGroupsSpeed: 1,
    rockGroupsRotationXSpeed: 0.01,
    rockGroupsRotationYSpeed: 0.01,
    rockGroupsRotationZSpeed: 0.01,
    rockGroupsLineOpacity: 0.5,
    rockGroupsOpacity: 0.5,
    rockGroupsColor: '#5e63b9',
    planetTimeValue: 0.005,
    planetOpacity: 1,
    redraw() {
      options.backgroundOpacity = gui.backgroundOpacity
      options.backMeteors.speed = gui.backMeteorsSpeed
      options.backMeteors.backMeteorsOpacity = gui.backMeteorsOpacity
      options.rockGroups.speed = gui.rockGroupsSpeed
      options.rockGroups.rotationXSpeed = gui.rockGroupsRotationXSpeed
      options.rockGroups.rotationYSpeed = gui.rockGroupsRotationYSpeed
      options.rockGroups.rotationZSpeed = gui.rockGroupsRotationZSpeed
      options.rockGroups.lineOpacity = gui.lineOpacity
      options.rockGroups.opacity = gui.opacity
      options.rockGroups.color = gui.rockGroupsColor
      options.planet.timeValue = gui.planetTimeValue
      options.planet.opacity = gui.planetOpacity
    }
  }
  var datGui = new dat.GUI()
  // 将设置属性添加到gui当中，gui.add(对象，属性，最小值，最大值）
  gui.add(controls, 'size', 0, 10).onChange(controls.redraw);
  datGui.add(gui, 'backgroundOpacity', 0, 1).step(0.1).onChange(gui.redraw)
  datGui.add(gui, 'backMeteorsSpeed', 0, 4).step(1).onChange(gui.redraw)
  datGui.add(gui, 'backMeteorsOpacity', 0, 1).step(0.1).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsSpeed', 0, 4).step(1).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsRotationXSpeed', 0.001, 0.04).step(0.005).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsRotationYSpeed', 0.001, 0.04).step(0.005).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsRotationZSpeed', 0.001, 0.04).step(0.005).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsLineOpacity', 0, 1).step(0.1).onChange(gui.redraw)
  datGui.add(gui, 'rockGroupsOpacity', 0, 1).step(0.1).onChange(gui.redraw)
  datGui.addColor(gui, 'rockGroupsColor').onChange(gui.redraw)
  datGui.add(gui, 'planetTimeValue', 0.001, 0.04).step(0.005).onChange(gui.redraw)
  datGui.add(gui, 'planetOpacity', 0, 1).step(0.1).onChange(gui.redraw)

  gui.redraw()
}
```

#### 着色器

着色  器变量，attribute、varying、uniform
顶点着色器：attribute、uniform
片段着色器：uniform
varying 是顶点着色器与片段着色器的传输纽带

```
// 顶点着色器
<script id="vertexShader" type="x-shader/x-vertex">
  varying vec2 vUv;

  void main()
  {
    vUv = uv;
    gl_Position = projectionMatrix *
                  modelViewMatrix *
                  vec4(position,1.0);
  }
</script>

// 片段着色器
<script id="fragmentShader" type="x-shader/x-fragment">
  varying vec2 vUv;
  uniform sampler2D planetTexture;
  uniform float time;
  uniform float flowWaveRadius;
  uniform float flowIndex;
  uniform float opacity;

  void main()
  {
    float pi = 3.1415926535897932384626433832795;
    float easeBase = ( 1.0 - vUv.y * 0.4);
    float ease = pow( easeBase , 6.0 );
    float offsetY = ( ease + (sin( (vUv.x + flowIndex) * pi * 4.0 ) * vUv.x * vUv.y + 1.0 )  * flowWaveRadius - time * 0.1);
    float vUvY = mod( offsetY * 1000.0,1000.0) / 1000.0;
    vec2 offsetUv = vec2(vUv.x, vUvY);
    vec4 textureColor = texture2D(planetTexture, offsetUv);
    gl_FragColor = vec4(0, 0, 0, 0.5);
    gl_FragColor = gl_FragColor + textureColor;
  }
</script>
```

#### 让画面动起来

```
function animate() {
  stats.update()
  requestAnimationFrame(animate)

  // 背景
  backMeteorsPlane.position.x += options.backMeteors.speed
  if (backMeteorsPlane.position.x > scale / 10) {
    backMeteorsPlane.position.x = 0
  }

  // 石头
  for (var i = 0; i < rockGroups.length; i++) {
    var meteor = rockGroups[i]
    meteor.position.x -= options.rockGroups.speed

    meteor.rotation.x -= options.rockGroups.rotationXSpeed
    meteor.rotation.y -= options.rockGroups.rotationYSpeed
    meteor.rotation.z -= options.rockGroups.rotationZSpeed

    meteor.children[1].material.color.set(options.rockGroups.color)
  }

  // 背景流动
  shaderUniforms.time.value += options.planet.timeValue
  shaderUniforms.flowIndex.value = 0.5 + Math.sin(shaderUniforms.time.value * 0.2) * 0.5

  renderer.render(backgroundScene, backgroundCamera)
  renderer.render(scene, camera)
}
```

#### 窗口变化时的更新

对于透视投影相机，需要更新 aspect、同时更新投影矩阵，场景中的其它物体的坐标会自动更新。

```
function onWindowResize() {
	camera.aspect = window.innerWidth / window.innerHeight
	camera.updateProjectionMatrix()

	renderer.setSize(window.innerWidth, window.innerHeight)
}
```

### 问题及解决方案

1. 将图片平铺时，采用正视相机
2. 图像循环平移，但是重置位置时会发生闪变的效果，左右两个图片，实现了循环播放的效果
3. transparent、opacity，小石头有透明的效果。背景需要设置透明的效果，透明物体与不透明物体相遇时的效果，可以通过设置透明物体材质的 blending 属性
4. 相机位置对物体位置造成的影响，如何查看到物体的具体位置，可视化图像的位置？

### 深度渲染  的参数

1. 开启深度渲染，depthTest
2. 材质是否添加到深度缓冲区，depthWrite
3. 透明物体与不透明物体的叠加效果，blending

### 学习资料

1. [Three.js 官方文档](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)
2. [Three.js 现学现卖](https://aotu.io/notes/2017/08/28/getting-started-with-threejs/index.html)
3. [Three.js 开发指南](https://book.douban.com/subject/26349497/)
4. [Three.js 基础理论](https://medium.com/@necsoft/three-js-101-hello-world-part-1-443207b1ebe1)
5. [Three.js 教程](https://teakki.com/p/58a3ef1bf0d40775548c908f)
6. [Three.js 官方示例](https://threejs.org/examples/#webgl_animation_cloth)
7. [Three.js 绘制土星视频(原创英文版)](https://medium.freecodecamp.org/how-i-recreated-the-music-video-gorillazs-andromeda-with-webgl-f9b0fe55fb17)

### 图片集锦

![Three.js基础概念](https://teakki.com/file/image/58a55615f0d40775548cbaed)
![Three.js核心对象结构](https://misc.aotu.io/JChehe/2017-08-28-getting-started-with-threejs/three_render.jpg)
