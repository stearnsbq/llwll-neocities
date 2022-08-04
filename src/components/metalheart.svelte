<script lang="ts">
  import * as THREE from "three";
  import { onMount } from "svelte";
  import { NURBSCurve } from "three/examples/jsm/curves/NURBSCurve";
  import { FlakesTexture } from "three/examples/jsm/textures/FlakesTexture";
  import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader";
  import { ParametricGeometry } from "three/examples/jsm/geometries/ParametricGeometry";
  import { BloomEffect, EffectComposer, EffectPass, RenderPass } from "postprocessing";


  //import {OrbitControls} from 'three/examples/jsm/'

  let canvas;

  onMount(() => {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      2000
    );
    camera.position.set(0, 350, 150);

    scene.add(camera);

    const loader = new THREE.TextureLoader();

    const bgTexture = loader.load("fractals/fractal.png", (texture) => {
      console.log(texture);
    });

    scene.background = bgTexture;

    // scene.add(new THREE.AmbientLight(0xffff00));

    const pointLight = new THREE.PointLight(0xffff00, 1);
    pointLight.position.set(0, 0, 0);

    scene.add(pointLight);

    const nurbsControlPoints = [];
    const nurbsKnots = [];
    const nurbsDegree = 1000;

    for (let i = 0; i <= nurbsDegree; i++) {
      nurbsKnots.push(0);
    }

    for (let i = 0, j = 3500; i < j; i++) {
      nurbsControlPoints.push(
        new THREE.Vector4(
          Math.random() * 800 - 400,
          Math.random() * 700,
          50,
          0.005 // weight of control point: higher means stronger attraction
        )
      );

      const knot = (i + 1) / (j - nurbsDegree);
      nurbsKnots.push(THREE.MathUtils.clamp(knot, 0, 1));
    }



    const nurbsCurve = new NURBSCurve(
      nurbsDegree,
      nurbsKnots,
      nurbsControlPoints,
      0,
      0
    );

    const nurbsGeometry = new THREE.TubeGeometry(undefined, 64, 15, 10);

    const renderer = new THREE.WebGLRenderer({ 	powerPreference: "high-performance",
	antialias: false,
	stencil: false,
	depth: false, canvas });

    // renderer.outputEncoding = THREE.sRGBEncoding;
    // renderer.toneMapping = THREE.ACESFilmicToneMapping;
    // renderer.toneMappingExposure = 1.25;

    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    const mapLoader = new THREE.PMREMGenerator(renderer);

    new RGBELoader().setPath("fractals/").load("fractal.hdr", (hdrmap) => {
      const envMap = mapLoader.fromCubemap(hdrmap);
      const texture = new THREE.CanvasTexture(new FlakesTexture());

    //   texture.wrapS = THREE.RepeatWrapping;
    //   texture.wrapT = THREE.RepeatWrapping;
    //   texture.repeat.x = 2;
    //   texture.repeat.y = 2;

      nurbsGeometry.setFromPoints(nurbsCurve.getPoints(1500));

      const material = new THREE.MeshPhysicalMaterial({
        clearcoat: 5.0,
        clearcoatRoughness: 0.1,
        metalness: 0.9,
        roughness: 0.6,
        color: 0xffffff,
       // normalMap: texture,
        normalScale: new THREE.Vector2(0.15, 0.15),
        envMap,
      });

      const object = new THREE.Mesh(nurbsGeometry, material);

      scene.add(object);
    });

    const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));
composer.addPass(new EffectPass(camera, new BloomEffect()));

    function animate() {


       requestAnimationFrame(animate);
    //   renderer.render(scene, camera);

        composer.render()
    }

    animate();
  });
</script>

<canvas bind:this={canvas} />
