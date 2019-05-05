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
</style>

<script>
	import { onMount } from "svelte";
	import { fade } from "svelte/transition";
	import { fly } from "svelte/transition";

	import AddTodo from "./AddTodo.svelte";
	import Footer from "./Footer.svelte";
	import Remaining from "./Remaining.svelte";
	import TodoItem from "./TodoItem.svelte";

	let tempId = 4;
	let currentFilter = "all";

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
	  const { title } = event.detail;
	  const todo = {
	    id: tempId,
	    title,
	    completed: false,
	    editing: false
	  };
	  todos = [...todos, todo];
	  tempId = tempId + 1;
	};

	// just to re-render
	const toggle = todo => (todos = todos);

	const deleteTodo = event => {
	  const {
	    todo: { id }
	  } = event.detail;
	  console.log("deleteTodo", id);
	  todos = todos.filter(todo => todo.id !== id);
	};

	const checkAllTodos = event => {
	  todos = todos.map(todo => ({ ...todo, completed: event.target.checked }));
	};

	const clearCompleted = () => {
	  todos = todos.filter(todo => !todo.completed);
	};

	const updateFilter = filter => (currentFilter = filter);

	const updateTodoList = () => (todos = todos);

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
  <img src={'img/svelte-logo-horizontal.svg'} alt="svelte logo" class="logo">

  <AddTodo on:addTodo={addTodo} />

{#each filteredTodos as todo}
  <TodoItem 
    todo={todo}
    on:deleteTodo={deleteTodo} 
    on:toggle={updateTodoList}
    on:editTodo={() => updateTodoList()}
    on:doneEditing={() => updateTodoList()}
  />
{/each}

  <Remaining todosRemaining={todosRemaining} on:checkAll={()=> checkAllTodos(event)}/>

  <Footer
    currentFilter={currentFilter}
    on:showAll={() => updateFilter('all')}
    on:showActive={() => updateFilter('active')}
    on:showCompleted={() => updateFilter('completed')}
    on:clearCompleted={clearCompleted}
  />

</div>