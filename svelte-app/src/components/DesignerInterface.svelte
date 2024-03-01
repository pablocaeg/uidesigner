<script>
  import Dropzone from "./Dropzone.svelte";
  import Menu from "./Menu.svelte";
</script>

<div id="interface-container">
  <Menu></Menu>
  <Dropzone></Dropzone>
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
