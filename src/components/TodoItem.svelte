<script>
  import { createEventDispatcher } from "svelte";
  import { fade } from 'svelte/transition';
  
  const dispatch = createEventDispatcher();
  
  export let todo;
</script>

<li transition:fade>
    <input 
        type="checkbox"
        checked={todo.done}
        on:change={() => dispatch('toggleDone')}
    />
    <span class:done={todo.done}>{todo.text}</span>
    <button on:click={() => dispatch('delete')}>Delete</button>
</li>

<style>
    .done {
        color: gray;
        text-decoration: line-through;
    }
    li {
        margin-bottom: 1rem;
    }
    button {
        margin-left: 1rem;
    }
</style>