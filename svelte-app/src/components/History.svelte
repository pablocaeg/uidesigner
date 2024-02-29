<div id="history-btn-container">
  <button id="undo" class="history-btn">↩</button>
  <button id="redo" class="history-btn">↪</button>
  <p id="history-dropdown">Show history</p>
</div>

<style>
  #history-btn-container {
    margin: 15px 0px 15px 20%;
    display: flex;
  }
  .history-btn {
    background: none;
    color: inherit;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;

    font-size: 17px;
    margin: 0px 2px;
    padding: 5px;
    padding-bottom: 2px;
    border-radius: 5px;
    font-weight: 300;
  }
  .history-btn:hover {
    background-color: rgba(223, 223, 223, 0.383);
  }
  .history-btn:active,
  .history-btn:focus {
    /* remove previous button style */
    background-color: transparent;
    outline: none;
    box-shadow: none;
  }
  #history-dropdown {
    margin-left: 5px;
    font-weight: 300;
    font-size: 16px;
    padding: 7px;
    border-radius: 5px;
    overflow: auto;
  }
  #history-dropdown:hover {
    background-color: rgba(223, 223, 223, 0.383);
  }
</style>
