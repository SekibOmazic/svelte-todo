<style lang="scss">
  .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }
</style>

<script>
  import { createEventDispatcher } from "svelte";
  import { addTodo } from "./actionTypes";

  let title = "";

  const dispatch = createEventDispatcher();

  const addNewTodo = event => {
    if (event.key === "Enter") {
      if (title.trim() !== "") {
        dispatch(addTodo, { title });
        title = "";
      }
    }
  };
</script>

<input type="text" class="todo-input" placeholder="What needs to be done"
  bind:value={title} 
  on:keydown={addNewTodo} />