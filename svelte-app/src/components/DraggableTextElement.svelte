<script>
  import { draggable } from "@neodrag/svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let textDefault;
  export let element_type;
  export let container;

  let activeElement;

  let initialX = 0;
  let initialY = 0;

  function rgbToHex(rgb) {
    let [r, g, b] = rgb.match(/\d+/g).map(Number); // Extract the RGB components from the input string

    // Convert each RGB component to a two-digit hexadecimal number and concatenate them
    let hex =
      "#" +
      [r, g, b]
        .map((x) => x.toString(16).padStart(2, "0").toUpperCase())
        .join("");

    // Check if the hex color is #000000, if so, change it to #FFFFFF
    if (hex === "#000000") {
      return "#FFFFFF";
    } else {
      return hex;
    }
  }

  function enableEdit(event) {
    activeElement = event.target;

    dispatch("activeElementSelected", { activeElement });

    // Get computed styles
    const computedStyle = window.getComputedStyle(activeElement);

    activeElement = event.target;

    // Dispatch computed style properties instead of the element's direct style properties
    dispatch("activeElementChange", {
      color: rgbToHex(computedStyle.color),
      fontSize: parseInt(computedStyle.fontSize, 10), // Convert px to an integer value
      background: rgbToHex(computedStyle.backgroundColor),
      isSelected: true,
    });
  }

  function getOriginalElementListeners(element) {
    draggable(element, { axis: "both" });
    element.addEventListener("neodrag:start", handleDragStart);
    element.addEventListener("neodrag:end", handleDragEnd);
  }

  function handleDragStart(event) {
    const originalElement = event.target;
    const rect = originalElement.getBoundingClientRect();

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
      placeholder.style.color = textDefault;
      placeholder.contentEditable = true;
      placeholder.addEventListener("focus", enableEdit);

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
    z-index: 999;
    margin: 0px 7px;
    padding: 4px 12px;
    font-size: 15px;
    background-color: rgb(255, 255, 255);
    border: 2px solid black;
  }
</style>
