<script lang="ts">
 import { open } from '@tauri-apps/api/dialog';

  import {onMount} from 'svelte';
  
  let image: string | ArrayBuffer = "./coordsystem.jpg";
  let text = "h"
  let dat = {
    x: 0,
    y: 0,
    ex: 0,
    ey: 0,
  }

 function op(e: any) {
  console.log(e)
 }

 async function openDialog (e: any) {
    console.log(await open())
  }

  function handle(e) {
    const file = e.target.files[0]
    const reader = new FileReader()
    reader.onload = event => {
      image = event.target.result
      console.log(887, image)
    }
    reader.readAsDataURL(file)
    console.log(image)
  }

  function clicked(e: MouseEvent) {
    function equi(x, y, width, height) {
      const longitude = (x / width) * 360 - 180;
      const latitude = ((y / height) * 180 - 90);
      return [longitude, latitude];
    }
    
    const [x, y] = [e.offsetX, e.offsetY]
    const [ex, ey] = equi(e.offsetX, e.offsetY, 800, 400)
    
    dat = {
      x, y, ex, ey 
    }

    // text = `${e.offsetX} ${e.offsetY} ${equi(e.offsetX, e.offsetY, 800, 400)}`
  }

</script>

<div>
  <header>
    <!-- <button on:click={openDialog}>Click to open dialog</button> -->
    
    <div>
      <input type="file" on:change={handle} />
    </div>
    {#if image}
      <div>
        <img src={image} alt="Selected Imadge" on:click={clicked} style="width: 800px; height:400px">
      </div>
    {/if}

    {`${dat.x}`} __ {`${dat.y}`} <br>
    {`${dat.ex.toFixed(2)}`} __ {`${dat.ey.toFixed()}`}
  </header>
</div>