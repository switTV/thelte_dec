<script lang="ts">
    import { T, useTask} from "@threlte/core"
    import { OrbitControls, interactivity, Sky, RoundedBoxGeometry} from '@threlte/extras'
    import { spring } from "svelte/motion";
    import {DoubleSide, MeshStandardMaterial, SphereGeometry} from "three"

    interactivity()
    const scale = spring(1)

    let rotation = 0

    useTask((delta) => {
      rotation += delta
      
    })
</script>

<svelte:head>
  <title>3d SvelteKit</title>
</svelte:head>

<Sky elevation={0.5}/>

<T.PerspectiveCamera
  makeDefault
  position={[10, 10, 10]}
  on:create={({ ref }) => {
    ref.lookAt(0, 1, 0)
  }}
  fov={50}
>
  <OrbitControls enableDamping/>
</T.PerspectiveCamera>

<T.DirectionalLight 
  position={[0, 10, 10]}
  castShadow
/>

<T.Mesh 
  rotation.y={rotation}
  position.y={1.2}
  scale={$scale}
  on:pointerenter={() => scale.set(1.5)}
  on:pointerleave={() => scale.set(1)}
  castShadow
>

    <RoundedBoxGeometry />
    <T.MeshPhongMaterial color="skyblue"/>
</T.Mesh>


<T.Mesh
  rotation.x={-Math.PI / 2}
  receiveShadow
  material={new MeshStandardMaterial({side: DoubleSide, color: "white"})}
>
  <T.BoxGeometry args={[40, 40]}/>
</T.Mesh>