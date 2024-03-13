<script>
  import { onMount } from 'svelte';

  let todos = [];
  let newTodo = '';

  const addTodo = () => {
    if (newTodo.trim().length > 0) {
      todos = [...todos, { text: newTodo.trim(), completed: false }];
      newTodo = '';
      updateLocalStorage();
    }
  };

  const removeTodo = (index) => {
    todos = todos.filter((_, i) => i !== index);
    updateLocalStorage();
  };

  const realizado = (index) => {
    todos[index].completed = !todos[index].completed;
    updateLocalStorage();
  };

  const updateLocalStorage = () => {
    localStorage.setItem('todos', JSON.stringify(todos));
  }

  onMount(() => {
    // Cargar tareas almacenadas en el localStorage al cargar la página
    const storedTodos = localStorage.getItem('todos');
    if (storedTodos) {
      todos = JSON.parse(storedTodos);
    }
  });
</script>

<main>
  <h1>Simple To-Do List</h1>
  <input bind:value={newTodo} placeholder="Agregar nueva tarea..." />
  <button class="add" on:click={addTodo}>Agregar</button>
  <ul>
    {#each todos as { text, completed }, index (index)}
      <li>
        <div class="todo">
          <span class:completed={completed}>{text}</span>
          <button class="real" on:click={() => realizado(index)}>Realizado</button>
          <button class="deleted" on:click={() => removeTodo(index)}>Eliminar</button>
        </div>
      </li>
    {/each}
  </ul>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 32px;
  }

  input {
    padding: 8px;
    font-size: 15px;
    width: 350px;
  }

  .add {
    padding: 8px 16px;
    font-size: 16px;
    background-color: #144b48;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-top: 16px;
    font-size: 16px;
    text-align: left;
  }

  li {
    display: flex;
    justify-content: center;
  }

  .todo {
    display: flex;
    align-items: center;
    background-color: #f2f2f2;
    padding: 8px 20px 8px 20px;
    border-radius: 4px;
    position: relative;
    max-width: 625px;
  }

  .completed {
    text-decoration: none;
  }

  .completed:before {
    content: "";
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    height: 2px;
    background-color: rgb(176, 206, 8);
  }

  .deleted {
    padding: 8px 16px;
    font-size: 16px;
    background-color: #144b48;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .real {
    padding: 8px 16px;
    font-size: 16px;
    background-color: #144b48;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 380px;
    margin-right: 6px;
  }
</style>
