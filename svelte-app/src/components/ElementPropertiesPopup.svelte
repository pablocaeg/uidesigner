<!-- ElementPropertiesPopup.svelte -->
<script>
  export let show = false;
  export let onSubmit;
  export let onCancel;

  let elementProps = {
    color: "#FFFFFF", // Default color
    size: 16,
    text: "",
  };

  function handleSubmit() {
    onSubmit(elementProps);
  }

  function handleCancel() {
    onCancel();
  }
</script>

{#if show}
  <div class="popup">
    <div>
      <label>Color: <input type="color" bind:value={elementProps.color} /></label
      >
    </div>
    <div>
        <label>Size: 
          <input type="range" min="12" max="24" bind:value={elementProps.size}>
          {elementProps.size}px
        </label>
      </div>
    <div>
      <label>Text: <input type="text" bind:value={elementProps.text} /></label>
    </div>
    <button on:click={handleSubmit}>Add Element</button>
    <button on:click={handleCancel}>Cancel</button>
  </div>
{/if}

<style>
  .popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 10px;
    border: 1px solid #ccc;
    z-index: 10;
  }
</style>
