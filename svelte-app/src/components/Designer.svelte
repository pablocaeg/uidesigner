<script>
  import ElementPropertiesPopup from "./ElementPropertiesPopup.svelte";
  import { onMount } from "svelte";

  let popupPosition = { x: 0, y: 0 };
  let showPopup = false; // Declare showPopup
  let currentElement = null;
  let dropZone;
  let elements = []; // This will store the elements added to the drop zone

  onMount(() => {
    // Initialize or fetch elements if necessary
  });

  function handleDragOver(event) {
    event.preventDefault(); // Necessary for allowing drop
  }

  function handleDrop(event) {
    event.preventDefault();
    const elementType = event.dataTransfer.getData("text/plain");

    // Calculate relative position
    const rect = dropZone.getBoundingClientRect();
    const x = event.clientX - rect.left;
    const y = event.clientY - rect.top;
    const relativeX = x / rect.width;
    const relativeY = y / rect.height;

    popupPosition.x = event.clientX;
    popupPosition.y = event.clientY;
    currentElement = { type: elementType, x: relativeX, y: relativeY };
    showPopup = true;
  }

  function addElement(elementProps) {
    elements = [...elements, { ...currentElement, ...elementProps }];
    console.log(elements);
    showPopup = false;
  }

  function cancelAddElement() {
    showPopup = false;
  }

  function createElement(element) {
    switch (element.type) {
      case "Button":
      return `<button style="font-size: ${element.size}px; background-color: ${element.color};">${element.text}</button>`;      // Aquí puedes añadir más casos para diferentes tipos de elementos
      default:
        return `<div>Elemento sin añadir</div>`;
    }
  }
</script>

<main>
  <div
    class="drop-zone"
    bind:this={dropZone}
    on:dragover={handleDragOver}
    on:drop={handleDrop}
  >
    <ElementPropertiesPopup
      x={popupPosition.x}
      y={popupPosition.y}
      show={showPopup}
      onSubmit={addElement}
      onCancel={cancelAddElement}
    />

    {#each elements as element}
      <div
        class="dropped-element"
        style="position: absolute; left: {element.x * 100}%; top: {element.y *
          100}%;"
      >
        {@html createElement(element)}
      </div>
    {/each}
  </div>

  <!-- List of draggable elements -->
  <div class="elements-list">
    {#each ["TextField", "Button", "Checkbox"] as element}
      <div
        class="draggable-element"
        draggable="true"
        on:dragstart={(event) =>
          event.dataTransfer.setData("text/plain", element)}
      >
        {element}
      </div>
    {/each}
  </div>
</main>

<style>
  .drop-zone {
    position: relative;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    gap: 10px;
    border: 2px dashed #ccc;
    padding: 10px;
    position: relative;
    min-height: 500px; /* Set a minimum height for visibility when empty */
  }

  .draggable-element {
    cursor: pointer;
    /* Additional styling */
  }
</style>
