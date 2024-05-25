<script lang="ts">
  import TodoItem from './components/TodoItem.svelte'
  import Svelte from './assets/svelte.svg'
  import type { Todo } from "./types";

  let todos: Todo[] = [];
  let todoText = ''
  let lastId = 0

  $: uncompletedCount = todos.map((todo) => !todo.done).length;
  $: status = `${uncompletedCount} of ${todos.length} remaining`;

  const createTodo = (text: string, done: boolean = false): Todo => ({id: ++lastId, text, done})
  
  const addTodo = () => {
    todos.push(createTodo(todoText))
    todos = todos; // triggers reactivity
    todoText = '';
  }
  const archiveCompleted = () => todos = todos.filter((todo) => !todo.done)
  const deleteTodo = (todoId: number) => (todos = todos.filter((todo) => todo.id !== todoId))
  const toggleDone = (todoId: number) => (todos = todos.map(todo => (todo.id === todoId ? {...todo, done: !todo.done} : todo)))
</script>

<main>
  <img class="logo" src={Svelte} alt="Svelte">
  <h1>Todo List</h1>

  <div>
    {status}
    <button on:click={archiveCompleted}>Archive completed</button>
  </div>
  
    <form on:submit|preventDefault={addTodo}>
      <input 
        type="text"
        size="30"
        placeholder="Enter new todo..."
        bind:value={todoText}
      />
      <button disabled={!todoText}>Add</button>
    </form>
    <ul>
      {#each todos as todo}
      <TodoItem {todo}
        on:delete={() => deleteTodo(todo.id)}
        on:toggleDone={() => toggleDone(todo.id)}
      />
      {/each}
    </ul>
</main>

<style>
  .logo {
    width: 90px;
  }
  h1 {
    margin-block: 1rem;
  }
  form {
    margin-block: 1rem;
  }
  button {
    margin-left: .5rem;
  }
  ul {
    list-style: none;
    margin-left: 0;
    padding-left: 0;
  }
</style>
