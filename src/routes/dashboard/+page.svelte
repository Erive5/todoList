<script>
  import { db } from "../../lib/firebase/firebase";
  import { authHandlers, authStore } from "../../store/store";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import TodoItem from "../../components/TodoItem.svelte";

  let todoList = [""];
  let currTodo = "";
  let error = false;

  authStore.subscribe((curr) => {
    todoList = curr.data.todos;
  });

  function addTodo() {
    error = false;
    if (!currTodo) {
      error = true;
    }
    todoList = [...todoList, currTodo];
    currTodo = "";
  }

  function editTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
      return i !== index;
    });

    currTodo = todoList[index];
    todoList = newTodoList;
  }

  function removeTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
      return i !== index;
    });
    todoList = newTodoList;
  }

  async function saveTodos() {
    try {
      const userRef = doc(db, "users", $authStore.user.uid);
      await setDoc(
        userRef,
        {
          todos: todoList,
        },
        { merge: true }
      );
    } catch (err) {
        console.log("Hubo un error al intentar guardar tu información");
    }
  }
</script>

{#if !$authStore.loading}
  <div class="mainContainer">
    <div class="headerContainer">
      <h1>Tareas</h1>
      <div class="headerButtons">
        <button on:click={saveTodos}>
          <i class="fa-regular fa-floppy-disk"></i>
          <p>Guardar</p>
        </button>
        <button on:click={authHandlers.logout}>
          <i class="fa-solid fa-right-from-bracket"></i>
          <p>Salir</p>
        </button>
      </div>
    </div>
    <main>
      {#if todoList.length === 0}
        <p>¡No tienes tareas que hacer!</p>
      {/if}
      {#each todoList as todo, index}
      <TodoItem todo = {todo} index={index} removeTodo={removeTodo} editTodo={editTodo}/>

      {/each}
    </main>
    <div class={"ingresarTarea " + (error ? "errorBorder" : "")}>
      <input bind:value={currTodo} type="text" placeholder="Ingresar tarea" />
      <button on:click={addTodo}>Añadir</button>
    </div>
  </div>
{/if}

<style>
  .mainContainer {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    gap: 24px;
    padding: 24px;
    width: 100%;
    max-width: 1000px;
    margin: 0 auto;
  }

  .headerContainer {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .headerButtons {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .headerContainer button {
    background: #003c5b;
    color: white;
    padding: 10px 18px;
    border: none;
    border-radius: 4px;
    font-weight: 700;
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
  }

  .headerContainer button i {
    font-size: 1.1rem;
  }

  .headerContainer button:hover {
    opacity: 0.7;
  }

  main {
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex: 1;
  }

  .ingresarTarea {
    display: flex;
    align-items: stretch;
    border: 1px solid #0891b2;
    border-radius: 5px;
    overflow: hidden;
  }

  .errorBorder {
    border-color: coral !important;
  }

  .ingresarTarea input {
    background: transparent;
    border: none;
    padding: 14px;
    color: white;
    flex: 1;
  }

  .ingresarTarea input:focus {
    outline: none;
  }

  .ingresarTarea button {
    padding: 0 14px;
    background: #003c5b;
    border: none;
    color: cyan;
    font-weight: 600;
    cursor: pointer;
  }

  .ingresarTarea button:hover {
    background: transparent;
  }
</style>
