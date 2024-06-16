<template>
  <div id="panorama-viewer" style="width: 20%;"></div>
</template>

<script>
	import * as THREE from 'three'
	import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
	import dat from "dat.gui"
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		mounted() {
		  this.initScene()
		  this.guiUi()
		},
		methods: {
		  guiUi(){
		    const controlData = {
		        "图一":()=>{this.loadPanorama("bg.jpg")},
		        "图二":()=>{this.loadPanorama("bg2.jpg")}
		    }
		
		    //创建实例 ff
		    const gui = new dat.GUI();
		    const f = gui.addFolder("配置");
		    f.add(controlData,"图一")
		    f.add(controlData,"图二")
		    //创建id
		    f.domElement.id = "gui"
		    f.open()
		  },
		  initScene() {
		    // 创建场景
		    this.scene = new THREE.Scene()
		
		    // 创建相机
		    this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
		    this.camera.position.z = 1
		
		    // 创建渲染器
		    this.renderer = new THREE.WebGLRenderer()
		    this.renderer.setSize(window.innerWidth, window.innerHeight)
		    this.$el.appendChild(this.renderer.domElement)
		
		    // 添加轨道控制器
		    this.controls = new OrbitControls(this.camera, this.renderer.domElement)
		    
		    // 加载全景图
		    this.loadPanorama('bg.jpg')
		  },
		  loadPanorama(name) {
		    // 加载全景图纹理
		    const texture = new THREE.TextureLoader().load(`static/${name}`)
		
		    // 创建球体几何体
		    const geometry = new THREE.SphereGeometry(500, 60, 40)
		    geometry.scale(-1, 1, 1) // 反转球体内外
		
		    // 创建材质并应用纹理
		    const material = new THREE.MeshBasicMaterial({ map: texture })
		    const mesh = new THREE.Mesh(geometry, material)
		    this.scene.add(mesh)
		
		    // 开始渲染
		    this.animate()
		  },
		  animate() {
		    // 在每一帧中进行渲染
		    requestAnimationFrame(this.animate)
		    this.renderer.render(this.scene, this.camera)
		    this.controls.update()
		  }
		}
	}
</script>

<style>
	
</style>
