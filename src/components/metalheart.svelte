<script lang="ts">
    import * as THREE from 'three'
    import { onMount } from 'svelte';
    import {NURBSCurve} from 'three/examples/jsm/curves/NURBSCurve'
    import {ParametricGeometry} from 'three/examples/jsm/geometries/ParametricGeometry'
    //import {OrbitControls} from 'three/examples/jsm/'

    let canvas;




    onMount(() => {





        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 2000);
        camera.position.set( 0, 350, 150 );

        scene.add(camera)

        scene.background = new THREE.Color( 0xff );

        scene.add( new THREE.AmbientLight( 0x808080 ) );

        const nurbsControlPoints = [];
		const nurbsKnots = [];
        const nurbsDegree = 800;

        for ( let i = 0; i <= nurbsDegree; i ++ ) {

            nurbsKnots.push( 0 );

        }

        for ( let i = 0, j = 2000; i < j; i ++ ) {

            nurbsControlPoints.push(
                new THREE.Vector4(
                    Math.random() * 800 - 400,
                    Math.random() * 700 ,
                    Math.random() * 800 - 400,
                    1 // weight of control point: higher means stronger attraction
                )
            );

            const knot = ( i + 1 ) / ( j - nurbsDegree );
            nurbsKnots.push( THREE.MathUtils.clamp( knot, 0, 10 ) );

        }

            const nurbsCurve = new NURBSCurve( nurbsDegree, nurbsKnots, nurbsControlPoints, 0, 0 );

            const nurbsGeometry = new THREE.TubeGeometry(undefined, 64, 3);


			nurbsGeometry.setFromPoints( nurbsCurve.getPoints( 2000 ) );

            const nurbsMaterial = new THREE.LineBasicMaterial( { color: 0xfffffff, opacity: 0.25, transparent: true } );

            var material = new THREE.MeshStandardMaterial( {
    metalness: 1,   // between 0 and 1
    roughness: 0.5, // between 0 and 1
                envMap: renderTarget.texture
} );

            const object = new THREE.Mesh( nurbsGeometry, material );


            scene.add(object)

            const renderer = new THREE.WebGLRenderer( { antialias: true, canvas } );

            renderer.setPixelRatio( window.devicePixelRatio );
            renderer.setSize( window.innerWidth, window.innerHeight );




        function animate() {

            requestAnimationFrame( animate );
            renderer.render( scene, camera );
        }

        animate()
    
        

    })

</script>


<canvas bind:this={canvas}></canvas>