<script>
	//import { Canvas, InteractiveObject, OrbitControls, T } from '@threlte/core'
	import { spring } from 'svelte/motion'
	import { degToRad } from 'three/src/math/MathUtils.js'
  import Header from './Header.svelte'
  import Chat from './Chat.svelte'
  import {T, Canvas, PerspectiveCamera, AmbientLight, OrbitControls, InteractiveObject} from '@threlte/core';
  import Hexglobe from "./lib/Hexglobe.svelte";



  let hexData;
  
  fetch('./countries.geo.json')
    .then((response) => response.json())
    .then((data) => {
      hexData = data.features;
    })
    .catch((error) => {
      console.log(error);
      return [];
    });
    
	const scale = spring(1)
</script>

<div class="split left">
	<div class="centered">
  <Header/>
  <Chat/>
  </div>
  </div>

<div class="split right">
	<Canvas>
		<PerspectiveCamera fov={90} position={{ x: 0, y:0, z: -12 }}>
			<OrbitControls  autoRotate autoRotateSpeed={0.8} />
		  </PerspectiveCamera>
		  <AmbientLight intensity={0.9}  />
		  <T.Group>
			<T.Mesh position.y={0.5} castShadow let:ref>
				<!-- Add interaction -->
				<InteractiveObject
					object={ref}
					interactive
					on:pointerenter={() => ($scale = 2)}
					on:pointerleave={() => ($scale = 1)}
				/>
				<T.SphereGeometry />
				<T.MeshStandardMaterial color="white" />
			</T.Mesh>
			</T.Group>
		  {#if hexData}
			<Hexglobe hexdata={hexData}/>
		  {/if}
	</Canvas>
</div>
<style>    
 /* Split the screen in half */
 .split {
  height: 100%;
  width: 70%;
  position: fixed;
  z-index: 1;
  top: 0;
  overflow-x: hidden;
  padding-top: 20px;
}

/* Control the left side */
.left {
  left: 0;
  /* background-color: #111; */
}

/* Control the right side */
.right {
  right: 0;
  /* background-color: red; */
}

/* If you want the content centered horizontally and vertically */
.centered {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

</style>
