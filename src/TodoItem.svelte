<style lang="scss">
  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    &:hover {
      color: black;
    }
  }
  .todo-item-left {
    // later
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit {
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    &:focus {
      outline: none;
    }
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>

<script>
	import { createEventDispatcher } from "svelte";
	import { fade } from "svelte/transition";
	import { fly } from "svelte/transition";
	import { editTodo, doneEditing, deleteTodo, toggle } from "./actionTypes";

	const dispatch = createEventDispatcher();

	export let todo;

	let beforeEditCache = "";

	const edit = todo => {
	  beforeEditCache = todo.title;
	  todo.editing = true;
	  dispatch(editTodo);
	};

	const doneEdit = todo => {
	  if (todo.title.trim() === "") {
	    todo.title = beforeEditCache;
	  }
	  todo.editing = false;
	  dispatch(doneEditing);
	};

	const doneEditKeydown = (todo, event) => {
	  if (event.key === "Enter") {
	    doneEdit(todo);
	  }
	  if (event.key === "Escape") {
	    todo.title = beforeEditCache;
	    todo.editing = false;
	    dispatch(doneEditing);
	  }
	};
</script>

<div class="todo-item">
    <div class="todo-item-left" transition:fly="{{ y: 20, duration: 300 }}">
      <input type="checkbox" bind:checked={todo.completed} on:change={() => dispatch(toggle, { todo })} />
      {#if !todo.editing}
      <div class="todo-item-label" class:completed={todo.completed} on:dblclick={() => edit(todo)}>{todo.title}</div>
      {:else}
      <input class="todo-item-edit" type="text" bind:value={todo.title} on:blur={() => doneEdit(todo)} on:keydown={() => doneEditKeydown(todo, event)} autofocus />
      {/if}
    </div>
    <div class="remove-item" on:click={() => dispatch(deleteTodo, { todo }) }>
      &times;
    </div>
</div>
