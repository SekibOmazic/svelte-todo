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
</style>

<script>
  import { createEventDispatcher } from "svelte";
  import { checkAll } from "./actionTypes";

  const dispatch = createEventDispatcher();

  export let todosRemaining;
</script>

<div class="extra-container">
    <div>
        <label>
            <input type="checkbox" on:change={() => dispatch(checkAll)} />
            Check All
        </label>
    </div>
    <div>{todosRemaining} items left</div>
</div>