<script>
  import { draggable } from "@neodrag/svelte";

  let data = [
    {
      id: 1,
      name: "Pedro",
      value: 7,
    },
    {
      id: 2,
      name: "Antonio",
      value: 20,
    },
    {
      id: 3,
      name: "Mario",
      value: 5,
    },
  ];

  let elementsInDropZone = ["Form"];
  let newName = ""; // To hold the new name input
  let newValue = ""; // To hold the new value input

  function addToDropZone(component) {
    elementsInDropZone = [...elementsInDropZone, component];
  }

  function handleKeyPress(event, component) {
    if (event.key === "Enter" || event.key === " ") {
      addToDropZone(component);
    }
  }

  function initialInterface() {
    console.log(data);
  }

  function addElement() {
    const newId =
      data.length > 0 ? Math.max(...data.map((item) => item.id)) + 1 : 1;
    data = [
      ...data,
      { id: newId, name: newName, value: parseInt(newValue, 10) },
    ];
    newName = ""; // Reset input fields after adding
    newValue = "";
  }

  function clearDropZone() {
    elementsInDropZone = [];
  }

  initialInterface();
</script>

<main>
  <div class="drop-zone">
    <div class="data-table-container">
      <!-- Container for the dynamic table -->
      {#if data.length > 0}
        <table>
          <thead>
            <tr><th>ID</th><th>Name</th><th>Value</th></tr>
          </thead>
          <tbody>
            {#each data as item}
              <tr>
                <td>{item.id}</td>
                <td>{item.name}</td>
                <td>{item.value}</td>
              </tr>
            {/each}
          </tbody>
        </table>
      {/if}
    </div>
    {#each elementsInDropZone as element, index}
      {#if element === "Form"}
        <form class="form-container" use:draggable={{ bounds: "parent" }}>
          <input bind:value={newName} placeholder="Name" />
          <input bind:value={newValue} type="number" placeholder="Value" />
          <button type="button" on:click={addElement}>Add</button>
        </form>
      {:else if element === "TextField"}
        <input
          class="dropped-element"
          use:draggable={{ bounds: "parent" }}
          type="text"
        />
      {:else if element === "Button"}
        <button class="dropped-element" use:draggable={{ bounds: "parent" }}>
          Click
        </button>
      {:else if element === "Checkbox"}
        <input
          class="dropped-element"
          use:draggable={{ bounds: "parent" }}
          type="checkbox"
        />
      {/if}
    {/each}
  </div>
  <div class="top-bar">
    {#each ["TextField", "Button", "Checkbox"] as element}
      <div
        class="draggable-element"
        on:click={() => addToDropZone(element)}
        on:keypress={(event) => handleKeyPress(event, element)}
        tabindex="0"
        role="button"
        aria-label={"Add " + element}
      >
        {element}
      </div>
    {/each}
    <div
      class="trash"
      on:click={clearDropZone}
      on:keypress={(event) => {
        if (event.key === "Enter" || event.key === " ") {
          clearDropZone();
        }
      }}
      tabindex="0"
      role="button"
      aria-label="Delete all elements"
    >
      Delete all elements
    </div>
  </div>
</main>

<style>
  .data-table-container {
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: white;
    padding: 10px;
    border: 1px solid #ccc;
    z-index: 10;
  }

  table {
    border-collapse: collapse;
    width: auto;
  }

  table,
  th,
  td {
    border: 1px solid black;
  }

  th,
  td {
    padding: 5px;
    text-align: left;
  }

  .drop-zone {
    position: absolute;
    top: 60px;
    left: 0;
    right: 0;
    bottom: 0;
    overflow: auto;
    display: flex;
    align-items: start;
    flex-wrap: wrap;
    gap: 10px;
    border: 2px dashed #ccc;
    padding: 10px;
    min-height: 500px;
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
    height: 60px;
    background-color: #4a4a4a;
    color: white;
    display: flex;
    align-items: center;
    padding: 0 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    z-index: 100;
  }

  .draggable-element {
    margin-right: 15px;
    cursor: pointer;
    background-color: #000000;
    border: 1px solid #ddd;
    padding: 5px 10px;
    border-radius: 4px;
    transition: background-color 0.3s;
    outline: none;
  }

  .draggable-element:hover,
  .draggable-element:focus {
    background-color: #e7e7e7;
  }

  .form-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 10px;
    background-color: #f0f0f0;
    border: 2px solid #ddd;
    border-radius: 5px;
    width: 200px; /* Set a fixed width for the form */
  }

  .form-container input,
  .form-container button {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin: 0; /* Remove margins that could affect the layout */
  }
</style>
