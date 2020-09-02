<script>
  import TodoItem from "./TodoItem.svelte"
  import { v4 as uuidv4 } from 'uuid';

  let newTodoTitle = '';
  let currentFilter = 'all';

  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
  $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'completed'
    ? todos.filter(todo => todo.completed)
    : todos.filter(todo => !todo.completed)

  function addTodo(event) {
    if (event.key === 'Enter') {
      todos = [...todos, {
        id: uuidv4(),
        completed: false,
        title: newTodoTitle
      }];
      newTodoTitle = '';
    }
  }

  function deleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id)
  }

  function toggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id)
    todos = todos.splice(todoIndex, 1, { ...todos[todoIndex], completed: !todos[todoIndex].completed })
  }

  function checkAllTodos(event) {
    todos = todos.map(todo => ({ ...todo, completed: event.target.checked }));
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed)
  }

  let todos = [
    {
      id: 1,
      title: "One todo",
      completed: false
    },
    {
      id: 2,
      title: "Two todo",
      completed: false
    },
    {
      id: 3,
      title: "Three todo",
      completed: false
    }
  ]
</script>

<div class="container">
  <h1 class="title">✅ Svelte Todo App ✅</h1>
  <input bind:value={newTodoTitle} type="text" name="todo-input" id="todo-input" placeholder="Enter a new todo item..."
    class="todo-input" on:keydown={addTodo} />

  {#each filteredTodos as todo}
      <TodoItem {...todo} on:deleteTodo={deleteTodo} on:togglecomplete={toggleComplete}/>
  {/each}

  <div class="inner-container">
      <div><label><input class="inner-container-input" type="checkbox" on:change={checkAllTodos}>Check All</label></div>
      <div>{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
        <div>
            <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
            <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
            <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
        </div>
        <dir>
            <button on:click={clearCompleted}>Clear Completed</button>
        </dir>
    </div>
</div>

<style>
  .title {
    text-align: center;
  }
  .container {
      max-width: 800px;
      margin: 10px auto;
  }
  .logo {
      display: block;
      margin: 20px auto;
      width: 50%;
  }
  .todo-input {
      width: 100%;
      padding: 10px, 20px;
      font-size: 18px;
      margin-bottom: 20px;
  }
  .inner-container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 16px;
      border-top: 1px solid lightgrey;
      padding-top: 15px;
      margin-bottom: 13px;
  }
  .inner-container-input {
      margin-right: 12px;
  }
  button {
      font-size: 14px;
      background-color: white;
      appearance: none;
  }
  button:hover {
      background: lightseagreen;
  }
  button:focus {
      outline: none;
  }
  .active {
      background: lightseagreen;
  }
</style>
 