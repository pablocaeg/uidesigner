<script>
  import { onMount } from "svelte";

  let currentElement = null;
  let dropZone;
  let elements = []; // Almacena los elementos añadidos a la drop zone
  let draggingExistingElement = false;
  let draggedElementIndex = -1;
  let nextElementId = 1; // Contador para IDs de elementos

  let selectedElementIndex = null;

function selectElementForRepositioning(element, index) {
  selectedElementIndex = index;
  // Agregar lógica para escuchar eventos de teclado
  window.addEventListener('keydown', handleKeyPress);
}

function clearSelection() {
  selectedElementIndex = null;
  window.removeEventListener('keydown', handleKeyPress);
}

function handleKeyPress(event) {
  if (selectedElementIndex == null) return;
  let moved = false;
  const moveAmount = 5; // Cantidad de píxeles para mover el elemento

  switch (event.key) {
    case 'ArrowRight':
      elements[selectedElementIndex].x += moveAmount / dropZone.getBoundingClientRect().width;
      moved = true;
      break;
    case 'ArrowLeft':
      elements[selectedElementIndex].x -= moveAmount / dropZone.getBoundingClientRect().width;
      moved = true;
      break;
    case 'ArrowUp':
      elements[selectedElementIndex].y -= moveAmount / dropZone.getBoundingClientRect().height;
      moved = true;
      break;
    case 'ArrowDown':
      elements[selectedElementIndex].y += moveAmount / dropZone.getBoundingClientRect().height;
      moved = true;
      break;
  }

  if (moved) {
    elements = elements.slice(); // Actualizar la reactividad en Svelte
  }
}

  onMount(() => {
    // Inicializar o recuperar elementos si es necesario
  });

  function handleDragOver(event) {
    event.preventDefault(); // Necesario para permitir soltar
  }

  function handleDrop(event) {
      clearSelection()

    event.preventDefault();
    const rect = dropZone.getBoundingClientRect();
    const x = event.clientX - rect.left;
    const y = event.clientY - rect.top;
    const relativeX = x / rect.width;
    const relativeY = y / rect.height;

    if (draggingExistingElement) {
      elements[draggedElementIndex] = {
        ...elements[draggedElementIndex],
        x: relativeX,
        y: relativeY,
      };
      elements = elements.slice(); // Reactivar en Svelte
      draggingExistingElement = false;
    } else {
      // Aquí manejar la adición de un nuevo elemento
      const elementType = event.dataTransfer.getData("text/plain");
      currentElement = {
        id: nextElementId++,
        type: elementType,
        x: relativeX,
        y: relativeY,
      };
      elements = [...elements, { ...currentElement }];
    }
  }

  function handleTrashDragOver(event) {
    event.preventDefault(); // Necessary to allow dropping
  }

  function handleTrashDrop(event) {
    event.preventDefault();
    if (draggingExistingElement) {
      elements = elements.filter((_, index) => index !== draggedElementIndex);
      draggingExistingElement = false;
    }
  }

  function handleElementDragStart(event, index) {
    draggingExistingElement = true;
    draggedElementIndex = index;
  }

  function handleDragEnd() {
    draggingExistingElement = false;
  }

  function createElement(element) {
    const disabledInteractionsStyle = 'pointer-events: none;';

    switch (element.type) {
      case "Button":
      return `<button style="${disabledInteractionsStyle}">Test Button</button>`;
            case "TextField":
      return `<input type="text" style="${disabledInteractionsStyle}" placeholder="Campo de texto"/>`;
            case "Checkbox":
      return `<input type="checkbox" style="${disabledInteractionsStyle}"/>`;
            // Aquí puedes añadir más casos para diferentes tipos de elementos
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
    {#each elements as element, index (element.id)}
      <div
        class="dropped-element"
        draggable="true"
        on:dblclick={() => selectElementForRepositioning(element, index)}
        on:dragstart={(event) => handleElementDragStart(event, index)}
        on:dragend={handleDragEnd}
        style="position: absolute; left: {element.x * 100}%; top: {element.y *
          100}%;"
      >
        {@html createElement(element)}
      </div>
    {/each}
  </div>

  <!-- List of draggable elements -->
  <div class="top-bar">
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
    <div
      class="trash"
      on:dragover={handleTrashDragOver}
      on:drop={handleTrashDrop}
    >
      Delete element
    </div>
  </div>
</main>

<style>
  .drop-zone {
    position: absolute;
    top: 60px; /* Height of the top bar */
    left: 0;
    right: 0;
    bottom: 0;
    overflow: auto; /* In case content overflows */
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(50px, 1fr));
    gap: 10px;
    border: 2px dashed #ccc;
    padding: 10px;
    min-height: 500px; /* Set a minimum height for visibility when empty */
  }
  
  .trash {
    margin-left: 40%;
    cursor: pointer;
    background-color: #ef4848;
    border: 1px solid #ddd;
    padding: 5px 10px;
    border-radius: 4px;
    transition: background-color 0.3s;
  }

  .top-bar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 60px; /* Adjust the height as needed */
    background-color: #4a4a4a; /* A dark shade for the background */
    color: white;
    display: flex;
    align-items: center;
    padding: 0 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    z-index: 100;
  }

  .draggable-element {
    margin-right: 15px; /* Spacing between elements */
    cursor: pointer;
    background-color: #000000;
    border: 1px solid #ddd;
    padding: 5px 10px;
    border-radius: 4px;
    transition: background-color 0.3s;
  }

  .draggable-element:hover {
    background-color: #e7e7e7;
  }
</style>
