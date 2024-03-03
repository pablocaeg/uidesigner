<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  let dropzoneElement;

  // Dispatch the dropzoneElement when it's bound
  $: if (dropzoneElement) {
    dispatch("dropzoneBound", { dropzoneElement });
  }
</script>

<div bind:this={dropzoneElement} id="dropzone"></div>

<style>
  #dropzone {
    margin: 20px 24px;
    overflow: auto;
    display: flex;
    align-items: start;
    flex-wrap: wrap;
    gap: 10px;
    border: 2px dashed #ccc;
    padding: 10px;
    min-height: 600px;
    position: relative;
  }
</style>
