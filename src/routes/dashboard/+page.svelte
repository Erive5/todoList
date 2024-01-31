<script>
    let todoList = ["Hacer muchas listas"];
    let currTodo = "";
    let error = false;

    function addTodo() {
        error = false;
        if (!currTodo) {
            error = true;
        }
        todoList = [...todoList, currTodo];
        currTodo = "";
    }

    function editTodo(index){
        let newTodoList = todoList.filter((val,i) => {
            return i !== index;
        });

        currTodo = todoList[index]
        todoList = newTodoList
    }

    function removeTodo(index){
        let newTodoList = todoList.filter((val,i) => {
            return i !== index;
        });
        todoList = newTodoList
    }

</script>

<div class="mainContainer">
    <div class="headerContainer">
        <h1>Tareas</h1>
        <div class="headerButtons">
            <button>
                <i class="fa-regular fa-floppy-disk"></i>
                <p>Guardar</p>
            </button>
            <button>
                <i class="fa-solid fa-right-from-bracket"></i>
                <p>Salir</p>
            </button>
        </div>
        
    </div>
    <main>
        {#if todoList.length === 0 }
        <p>
            ¡No tienes tareas que hacer!
        </p>

        {/if}
        {#each todoList as todo, index}
            <div class="todo">
                <p>
                    {index + 1}. {todo}
                </p>
                <div class="actions">
                    <!-- svelte-ignore a11y-no-static-element-interactions -->
                    <i on:click={() => {
                        editTodo(index)
                    }} on:keydown={() => {}} 
                    class="fa-regular fa-pen-to-square"></i>

                    <!-- svelte-ignore a11y-no-static-element-interactions -->
                    <i on:click={() => {
                        removeTodo(index)
                    }} on:keydown={() => {}} class="fa-regular fa-trash-can"></i>
                </div>
            </div>
        {/each}
    </main>
    <div class={"ingresarTarea " + (error ? "errorBorder" : "")}>
        <input bind:value={currTodo} type="text" placeholder="Ingresar tarea" />
        <button on:click={addTodo}>Añadir</button>
    </div>
</div>

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

    .headerButtons{
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

    .todo {
        border-left: 1px solid cyan;
        padding: 8px 14px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .actions {
        display: flex;
        align-items: center;
        gap: 14px;
        font-size: 1.3rem;
    }

    .actions i:hover {
       cursor: pointer;
    }

    .actions i:hover {
       color: coral;
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
