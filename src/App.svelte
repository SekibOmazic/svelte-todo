<script>
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
</script>

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

<div class="container">
  <img src={'/img/svelte-logo-horizontal.svg'} alt="svelte logo" class="logo">

  <input type="text" class="todo-input" placeholder="What needs to be done">

{#each todos as todo}
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox">
      <div class="todo-item-label">{todo.title}</div>
    </div>
    <div class="remove-item">
      &times;
    </div>
  </div>
  {/each}


  <div class="extra-container">
    <div><label><input type="checkbox">Check All</label></div>
    <div>3 items left</div>
  </div>

  <div class="extra-container">
    <div>
      <button>All</button>
      <button>Active</button>
      <button>Completed</button>
    </div>

    <div>
      <button>Clear Completed</button>
    </div>
  </div>
</div>