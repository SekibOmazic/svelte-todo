<style lang="scss">
  .container {
    max-width: 600px;
    margin: 0 auto;
  }
  .logo {
    display: block;
    margin: 20px auto;
    height: 75px;
  }

  .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }
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
    border: 1px solid #ccc; //override defaults
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    &:focus {
      outline: none;
    }
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
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
	import { onMount } from "svelte";

	let newTodo = "";
	let tempId = 4;
	let currentFilter = "all";
	let beforeEditCache = "";

	let todos = [
	  {
	    id: 1,
	    title: "Learn Svelte",
	    completed: false,
	    editing: false
	  },
	  {
	    id: 2,
	    title: "Learn GraphQl",
	    completed: false,
	    editing: false
	  },
	  {
	    id: 3,
	    title: "Learn Flutter",
	    completed: false,
	    editing: false
	  }
	];

	const addTodo = event => {
	  if (event.key === "Enter") {
	    const todo = {
	      id: tempId,
	      title: event.target.value,
	      completed: false,
	      editing: false
	    };
	    todos = [...todos, todo];
	    newTodo = "";
	    tempId = tempId + 1;
	  }
	};

	const deleteTodo = id => (todos = todos.filter(todo => todo.id !== id));

	const checkAllTodos = event => {
	  todos = todos.map(todo => ({ ...todo, completed: event.target.checked }));
	};

	const clearCompleted = () => {
	  todos = todos.filter(todo => !todo.completed);
	};

	const updateFilter = filter => (currentFilter = filter);

	const editTodo = todo => {
	  beforeEditCache = todo.title;
	  todo.editing = true;
	  // reasign causes new render
	  todos = todos;
	};

	const doneEdit = todo => {
	  if (todo.title.trim() === "") {
	    todo.title = beforeEditCache;
	  }
	  todo.editing = false;
	  // reasign causes new render
	  todos = todos;
	};

	const doneEditKeydown = (todo, event) => {
	  if (event.key === "Enter") {
	    doneEdit(todo);
	  }
	  if (event.key === "Escape") {
	    todo.title = beforeEditCache;
	    todo.editing = false;
	    todos = todos;
	  }
	};

	onMount(async () => {
	  const res = await fetch("https://api.chucknorris.io/jokes/random");
	  const response = await res.json();
	  console.log(response.value);
	});

	$: todosRemaining = todos.filter(todo => !todo.completed).length;

	$: filteredTodos =
	  currentFilter === "all"
	    ? todos
	    : currentFilter === "completed"
	    ? todos.filter(todo => todo.completed)
	    : todos.filter(todo => !todo.completed);
</script>

<div class="container">
  <img src={'/img/svelte-logo-horizontal.svg'} alt="svelte logo" class="logo">

  <input type="text" class="todo-input" placeholder="What needs to be done"
  bind:value={newTodo} on:keydown={addTodo}>

{#each filteredTodos as todo}
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" bind:checked={todo.completed} />
      {#if !todo.editing}
      <div class="todo-item-label" class:completed={todo.completed} on:dblclick={() => editTodo(todo)}>{todo.title}</div>
      {:else}
      <input class="todo-item-edit" type="text" bind:value={todo.title} on:blur={() => doneEdit(todo)} on:keydown={() => doneEditKeydown(todo, event)} autofocus />
      {/if}
    </div>
    <div class="remove-item" on:click={() => deleteTodo(todo.id)}>
      &times;
    </div>
  </div>
  {/each}


  <div class="extra-container">
    <div><label><input type="checkbox" on:change={checkAllTodos}>Check All</label></div>
    <div>{todosRemaining} items left</div>
  </div>

  <div class="extra-container">
    <div>
      <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
      <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
      <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
    </div>

    <div>
      <button on:click={clearCompleted}>Clear Completed</button>
    </div>
  </div>
</div>