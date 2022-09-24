<template>
    <h4>Vue 3 Composite API (Todo App)</h4>
    <form action="" @submit.prevent="addTodo">
        <label for="newTodo">New Todo</label><br>
        <input v-model="newTodo" type="text" name="newTodo" placeholder="Add a new todo"><br>
        <button class="submit-btn">Add New Todo</button>
    </form>
    <button class="remove-btn" v-if="todos.length > 0" @click="removeAll">Remove All</button>
    <button class="mark-btn" v-if="todos.length > 0" @click="markAllDone">Mark All Done</button>
    <ul v-if="todos.length > 0" class="todos">
        <li class="todo" v-for="(todo, idx) in todos" :key="todo.id">
            <h3 class="todoText" :class="{done: todo.done }" @click="toggleDone(todo)">{{todo.content}}</h3>
            <button class="todoRemove" @click="removeTodo(idx)">Remove</button>
        </li>

    </ul>

</template>

<script>
import { ref, onBeforeMount } from 'vue';

export default {
    setup() {
        const newTodo = ref("");
        const todos = ref([]);

        const addTodo = () => {
            todos.value.push({
                done: false,
                id: Date.now(),
                content: newTodo.value
            });
            newTodo.value = "";
            localStorage.setItem("data", JSON.stringify([...todos.value]));
        };

        const toggleDone = (todo) => {
            todo.done = !todo.done;
        }

        const removeTodo = (idx) => {
            todos.value.splice(idx, 1)
        }

        const markAllDone = () => {
            todos.value.forEach((todo) => {
                todo.done = true;
            })
        }

        const removeAll = () => {
            todos.value = [];
        }
        
        onBeforeMount(()=>{
            todos.value = JSON.parse(localStorage.getItem("data"))
        })


        return {
            removeAll,
            markAllDone,
            toggleDone,
            removeTodo,
            todos,
            newTodo,
            addTodo,
        }
    }

}
</script>

<style>
input {
    padding: 0.5rem;
    border: none;
    box-shadow: 0px 0px 9px rgb(178, 178, 178);
    border-radius: 10px;
    margin: 0.5rem auto 2rem auto;
}

.submit-btn {
    padding: 0.8rem;
    background: blue;
    color: white;
    border: none;
    border-radius: 8px;
    margin-bottom: 0.5rem;
}

.remove-btn,
.mark-btn {
    padding: 0.4rem;
    background: rgba(204, 115, 255, 0.472);
    color: rgb(0, 0, 0);
    border: none;
    border-radius: 4px;
    margin: 0.5rem;
    margin-bottom: 1rem;
}

.todoRemove {
    padding: 0.4rem;
    background: rgba(255, 115, 115, 0.472);
    color: rgb(0, 0, 0);
    border: none;
    border-radius: 4px;
    margin: 0.5rem;
}

.todos {
    border-radius: 10px;
    box-shadow: 0px 0px 10px -2px rgb(195, 195, 195);
    padding: 0.4rem;
    margin: auto;
    width: 50%;
}

@media only screen and (max-width: 777px) {
    .todos {
        width: 95%;
    }
}

.todo {
    display: flex;
    align-items: center;
    margin: auto;
    width: 300px;
    margin: 0.5rem auto 0.5rem auto;
    padding-right: 0.3rem;
    padding-left: 0.6rem;
    border-radius: 7px;
    background: linear-gradient(45deg, #7f7fff33, #ff868633, #ffff8c33);
}

.todo b {
    margin-right: 0.5rem;
}

.todo h3 {
    color: purple;
    width: 250px;
    text-align: left;
    overflow: hidden;
    overflow-x: auto;
}

.done {
    text-decoration: line-through;
}

* {
    list-style-type: none;
    padding: 0;
}
</style>