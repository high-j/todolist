<template>
    <h1>ToDo List</h1>
    <form @submit.prevent="addTodo()">
        <label>New Event : </label>
        <input v-model="newTodo" name="newTodo" autocomplete="off">&ensp;
        <button class="add">ADD TODO</button>
    </form><br>
    <hr />
    <div>
        <div class="row">
            <div>
                <h2>Undo</h2>
                <ul>
                    <li v-for="(todo, index) in todos" :key="index">
                        <span>
                            {{ todo.content }}
                        </span>&ensp;
                        <button @click="finishTodo(index)" class="finish">Done</button>&ensp;
                        <button @click="removeTodo(index)" class="del">Remove</button>
                    </li>
                </ul>
                <h4 v-if="todos.length === 0">It's Empty !</h4>
            </div>
            <div>
                <h2>Finished</h2>
                <ul>
                    <li v-for="(todo, index) in list" :key="index">
                        <span @click="finishTodo(index)" class="ok">
                            {{ todo[0].content }}
                        </span>&ensp;
                    </li>
                </ul>
                <h4 v-if="list.length === 0">Empty list.</h4>
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
        const todos = ref(todosData);
        //新增todo
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
        //delete todo item
        function removeTodo(index) {
            //splice : start at index and delete 1 item
            todos.value.splice(index, 1); 
            saveData();
            
        }
    
        function saveData() {
            const storageData = JSON.stringify(todos.value);
            localStorage.setItem('todos', storageData);
        }
        
        // 已完成(Done)事項
        function finishTodo(index) {
            let done = todos.value.splice(index, 1);
            list.value.push(done);
            saveData();
            finishData();
            
        }

        function finishData() {
            const storageData = JSON.stringify(list.value);
            localStorage.setItem('list', storageData);
        }

        return {
            todos,
            list,
            newTodo,
            addTodo,
            doneTodo,
            removeTodo,
            saveData,
            finishData,
            finishTodo
        }
    },

}
</script>

<style>
@import "./host.css";
</style>