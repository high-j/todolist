<template>
    <h1>ToDo List</h1>
    <form @submit.prevent="addTodo()">
        <label>New Event : </label>
        <input v-model="newTodo" name="newTodo" autocomplete="off">&ensp;
        <button class="add">Add ToDo</button>
    </form><br>
    <hr />
    <div>
        <div class="row">
            <div>
                <h2>Current List</h2>
                <ul>
                    <li v-for="(todo, index) in todos" :key="index">
                        <span>
                            {{ todo.content }}
                        </span>&ensp;
                        <button @click="removeTodo(index)" class="del">Remove</button>
                    </li>
                </ul>
                <h4 v-if="todos.length === 0">Empty list.</h4>
            </div>
            <div>
                <h2> Already Finished</h2>
                <ul>
                    <li v-for="(todo, index) in todos" :key="index">
                        <span :class="{ done: todo.done }" @click="doneTodo(todo)" class="ok">
                            {{ todo.content }}
                        </span>&ensp;
                    </li>
                </ul>
                <h4 v-if="todos.length === 0">Empty list.</h4>
            </div>

        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
export default {
    name: 'Host',
    setup() {
        const newTodo = ref('');
        const list = ref([]);
        const defaultData = [{
            done: false,
            content: 'Write a blog post'
        }]
        const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
        //console.log(todosData)
        const todos = ref(todosData);
        function addTodo() {
            if (newTodo.value) {
                todos.value.push({
                    done: false,
                    content: newTodo.value
                });
                newTodo.value = '';
            }
            saveData();
        }

        function doneTodo(todo) {
            todo.done = !todo.done
            saveData();
        }

        function removeTodo(index) {
            let finish = todos.value.splice(index, 1); //start at index and delete 1 item
            todos.value.splice(index, 1); //start at index and delete 1 item
            saveData();
            list.value.push(finish)
            //console.log(finish)
            
        }

        function saveData() {
            const storageData = JSON.stringify(todos.value);
            localStorage.setItem('todos', storageData);
            //console.log(todos)
        }

        return {
            todos,
            newTodo,
            addTodo,
            doneTodo,
            removeTodo,
            saveData
        }
    },

}
</script>

<style>
h1 {
    font-size: 50px;
}
.row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    vertical-align: middle;
    margin: 2em;
}

body {
    background-color: #b49771;
    color: aliceblue;
    text-align: center;
}

h2 {
    font-size: 30px;
}

label {
    font-size: 22px;
    font-weight: bold;
}

ul {
    border-style: dotted;
    border-radius: 10px;
    width: 80%;
    position: relative;
    left: 5%;
}

span {
    text-align: center;
    font-size: 30px;
    color: #342E2D;
}

.add {
    background-color: rgb(103, 150, 193);
    border-radius: 10px;
    font-size: 15px;
}

.del {
    background-color: rgb(208, 65, 94);
    border-radius: 10px;
    font-size: 15px;
}

.don {
    background-color: rgba(65, 208, 179, 0.966);
    border-radius: 10px;
    font-size: 15px;
}

input {
    width: 200px;
    height: 25px;
}

.ok {
    color: rgb(124, 122, 122);
}
</style>