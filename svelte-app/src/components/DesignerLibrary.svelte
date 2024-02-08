<script>
    import { draggable } from '@neodrag/svelte';

    let elementsInDropZone = [];

    function addToDropZone(component) {
        elementsInDropZone = [...elementsInDropZone, component];
    }

    function handleKeyPress(event, component) {
        if (event.key === 'Enter' || event.key === ' ') {
            addToDropZone(component);
        }
    }

    function clearDropZone() {
        elementsInDropZone = [];
    }
</script>

<main>
    <div class="drop-zone">
        {#each elementsInDropZone as element, index}
                {#if element}
                    {#if element === 'TextField'}
                        <input class="dropped-element" use:draggable={{ bounds:"parent"}} type="text" />
                    {:else if element === 'Button'}
                        <button class="dropped-element"  use:draggable={{ bounds:"parent"}}> Click </button>
                    {:else if element === 'Checkbox'}
                        <input class="dropped-element " use:draggable={{ bounds:"parent"}} type="checkbox" />
                    {/if}
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
                if (event.key === 'Enter' || event.key === ' ') {
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
    .dropped-element {
        position: absolute;
        width: auto;
        height: 20%;
    }
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
        outline: none; /* Remove default focus outline */
    }

    .draggable-element:hover,
    .draggable-element:focus {
        background-color: #e7e7e7;
    }
</style>
