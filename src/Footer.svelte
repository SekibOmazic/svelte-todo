<style lang="scss">
  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;

    input {
      margin-right: 8px;
    }
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    &:hover {
      background: salmon;
    }
    &:focus {
      outline: none;
    }
  }

  .active {
    background: salmon;
  }
</style>


<script>
  import { createEventDispatcher } from "svelte";
  import {
    showAll,
    showActive,
    showCompleted,
    clearCompleted
  } from "./actionTypes";
  const dispatch = createEventDispatcher();

  export let currentFilter;
</script>

<div class="extra-container">
    <div>
      <button on:click={() => dispatch(showAll)} class:active="{currentFilter === 'all'}">All</button>
      <button on:click={() => dispatch(showActive)} class:active="{currentFilter === 'active'}">Active</button>
      <button on:click={() => dispatch(showCompleted)} class:active="{currentFilter === 'completed'}">Completed</button>
    </div>

    <div>
      <button on:click={() => dispatch(clearCompleted)}>Clear Completed</button>
    </div>
</div>