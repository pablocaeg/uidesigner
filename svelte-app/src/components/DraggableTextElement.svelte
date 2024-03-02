<script>
  import { draggable } from "@neodrag/svelte";

  export let element_type;
  export let container;

  let initialX = 0;
  let initialY = 0;

  function getOriginalElementListeners(element) {
    draggable(element, { axis: "both" });
    element.addEventListener("neodrag:start", handleDragStart);
    element.addEventListener("neodrag:end", handleDragEnd);
  }

  function handleDragStart(event) {
    const originalElement = event.target;
    const rect = originalElement.getBoundingClientRect();

    console.log(rect);

    initialX = rect.left;
    initialY = rect.top;

    const clone = originalElement.cloneNode(true);

    getOriginalElementListeners(clone);

    originalElement.replaceWith(clone);

    originalElement.style.position = "absolute";
    originalElement.style.left = `${rect.left + window.scrollX}px`;
    originalElement.style.top = `${rect.top + window.scrollY}px`;
    originalElement.style.margin = "0"; //

    container.appendChild(originalElement);
  }

  function handleDragEnd(event) {
    let dropZone = document.getElementById("dropzone");
    let dropRect = dropZone.getBoundingClientRect();

    // Calculate final drop position using initial position and offset
    let finalDropX = initialX + event.detail.offsetX;
    let finalDropY = initialY + event.detail.offsetY;

    // Check if the drag ended inside the drop zone
    if (
      finalDropX >= dropRect.left &&
      finalDropX <= dropRect.right &&
      finalDropY >= dropRect.top &&
      finalDropY <= dropRect.bottom
    ) {
      const placeholder = document.createElement(element_type);

      placeholder.textContent = "Modify this element with your text";
      placeholder.style.position = "absolute";
      placeholder.style.left = `${finalDropX - dropRect.left}px`;
      placeholder.style.top = `${finalDropY - dropRect.top}px`;
      placeholder.contentEditable = true;

      draggable(placeholder, { axis: "both", bounds: dropZone });

      dropZone.appendChild(placeholder);
    }

    event.target.remove();
  }
</script>

<div
  class="draggable-element"
  use:draggable={{ axis: "both" }}
  on:neodrag:start={handleDragStart}
  on:neodrag:end={handleDragEnd}
>
  <slot />
</div>

<style>
  .draggable-element {
    margin: 0px 15px;
    padding: 5px 15px;
    font-size: 17px;
    background-color: rgb(255, 255, 255);
    border: 2px solid black;
  }
</style>
