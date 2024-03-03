<script>
  import Dropzone from "./Dropzone.svelte";
  import Menu from "./Menu.svelte";

  let dropzoneElement;

  function handleDropzoneBound(event) {
    dropzoneElement = event.detail.dropzoneElement;
  }
</script>

<div id="interface-container">
  <Menu {dropzoneElement}></Menu>
  <Dropzone on:dropzoneBound={handleDropzoneBound}></Dropzone>
</div>

<style>
  #interface-container {
    position: absolute;
    width: 100%;
    height: 100%;
    margin: 0;
    min-height: 100vh;
  }
</style>
