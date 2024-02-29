<script>
  import { draggable } from "@neodrag/svelte";
  let activeTab = "modify-general"; // tab selected by default

  let tempMouseMoveHandler;

  let modifyGeneralContainer;

  let initialX = 0;
  let initialY = 0;

  function setActiveTab(tabName) {
    activeTab = tabName; // updates the active tab based on users selection
  }

  function makeElementDraggable(element) {
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

    makeElementDraggable(clone);

    originalElement.replaceWith(clone);

    originalElement.style.position = "absolute";
    originalElement.style.left = `${rect.left}px`;
    originalElement.style.top = `${rect.top}px`;
    originalElement.style.margin = "0"; //

    modifyGeneralContainer.appendChild(originalElement);

    tempMouseMoveHandler = (e) => {
      console.log("Mouse X:", e.clientX, "Mouse Y:", e.clientY);
    };
    document.addEventListener("mousemove", tempMouseMoveHandler);
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
      const placeholder = document.createElement("h1");
      placeholder.textContent = "Your title would be here :)";
      placeholder.style.position = "absolute";
      console.log("Text position in X: " + (finalDropX - dropRect.left) + "& Text Position in Y: " + (finalDropY - dropRect.top));
      placeholder.style.left = `${finalDropX - dropRect.left}px`;
      placeholder.style.top = `${finalDropY - dropRect.top}px`;

      draggable(placeholder, { axis: "both" });

      dropZone.appendChild(placeholder);
    }

    event.target.remove(); // Remove the dragged element
    document.removeEventListener("mousemove", tempMouseMoveHandler);
    console.log("finalDropX:" +finalDropX + "  finalDropY:" + finalDropY)
  }
</script>

<div>
  <div id="upper-bar-container">
    <div id="upper-bar">
      <p id="project-title">
        lowsheets | A low-code programming system based on spreadsheets
      </p>
    </div>
  </div>
  <div id="tabs">
    <button
      class="tab-btn"
      class:active={activeTab === "default"}
      on:click={() => setActiveTab("default")}
      >Default interfaces based on created tables</button
    >
    <button
      class="tab-btn"
      class:active={activeTab === "modify-table"}
      on:click={() => setActiveTab("modify-table")}
      >Modify interface of the selected table</button
    >
    <button
      class="tab-btn"
      class:active={activeTab === "modify-general"}
      on:click={() => setActiveTab("modify-general")}
      >Modify general interface of the application</button
    >
  </div>
  <div id="top-bar-container">
    <div id="top-bar">
      {#if activeTab === "default"}
        <p>Default content will go here</p>
      {:else if activeTab === "modify-table"}
        <p>Modify table content will go here</p>
      {:else if activeTab === "modify-general"}
        <div bind:this={modifyGeneralContainer} id="modify-general-container">
          <div
            class="draggable-element"
            use:draggable={{ axis: "both" }}
            on:neodrag:start={handleDragStart}
            on:neodrag:end={handleDragEnd}
          >
            Title
          </div>
          <div class="draggable-element" use:draggable={{ axis: "both" }}>
            Subtitle
          </div>
          <div class="draggable-element" use:draggable={{ axis: "both" }}>
            Text
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  #upper-bar {
    margin: 20px 0px 0px 15px;
    display: flex;
  }
  #project-title {
    font-size: 16px;
    font-weight: 300;
    margin-bottom: 17px;
  }
  #top-bar-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  #top-bar {
    margin: 0px;
    height: 100px;
    width: 96.5%;
    display: flex;
    background-color: rgb(255, 255, 255);
    border: 3px solid white;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 2px 3px 0px;
    border-radius: 10px;
  }
  #tabs {
    display: flex;
    margin: 0px 25px;
  }
  .tab-btn {
    /* remove previous button style */
    background: none;
    color: inherit;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;
    /* my style */
    margin: 15px 12px;
    font-size: 14px;
    font-weight: 300;
  }
  .tab-btn:active,
  .tab-btn:focus {
    /* remove previous button style */
    background-color: transparent;
    outline: none;
    box-shadow: none;
  }
  .active {
    text-decoration: underline 2px rgb(86, 86, 86);
    text-underline-offset: 7px;
    font-weight: 550;
  }
  .tab-btn:hover {
    text-decoration: underline 2px gainsboro;
    text-underline-offset: 7px;
    cursor: pointer;
  }

  #modify-general-container {
    height: 100%;
    margin: 0px 15px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .draggable-element {
    margin: 0px 15px;
    padding: 5px 15px;
    font-size: 17px;
    background-color: rgb(255, 255, 255);
    border: 2px solid black;
  }
</style>
