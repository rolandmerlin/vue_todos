<template>
    <main>
        <h1>TODOS VueJs</h1>
        <div class="todos">
            <span>#</span>
            <span>
                <input type="text" name="add_todos" v-model="todoname">
            </span>
            <span>
                <input type="text" name="add_todos_description" v-model="tododesc"/>
            </span>
            <span>
                <button id="add_todos_submit" v-on:click="AddTodos()">Ajouter</button>
            </span>
        </div>
        <div id="todos">
            <template v-for="(todo,k) in todos" :key="'todo_'+k">
                <span>{{todo.id}}</span>
                <span>{{todo.name}}</span>
                <span>{{todo.desc}}</span>
                <span><button v-on:click="DeleteTodo(todo.id)">Supprimer</button></span>
            </template>
        </div>
        <div style="text-align:center">
            <br>
            <button type="button" v-on:click="ClearTodo()">Vider toutes les tâches</button>
        </div>
    </main>
</template>
<script setup>
    import {ref} from 'vue'
    const todos = ref(JSON.parse(localStorage.getItem('todos') || "[]"));
    const todoname = ref('');
    const tododesc = ref('');
    const currentId = ref(1*Math.max(...todos.value.map(t => t.id),0)+1);

    function UpdateTodos(data_todos){
        todos.value = data_todos;
        localStorage.setItem('todos',JSON.stringify(todos.value));
    }

    function AddTodos(){
        if (todoname.value.length<1){
            alert('Veuillez spécifier une tache à faire');
            return;
        }
        if (tododesc.value.length==0){
            alert('Veuillez spécifier une description pour la tache à faire');
            return;
        }
        UpdateTodos([...todos.value,{
            'id':currentId.value,
            'name':todoname.value,
            'desc':tododesc.value
        }]);
        currentId.value++
    }
    function ClearTodo(){
        UpdateTodos([]);
        currentId.value = 1;
    }

    function DeleteTodo(id){
        UpdateTodos(todos.value.filter(t => t.id!==id));
    }
</script>
<style>
    .todos, #todos {
        display:grid;
        grid-template-columns: 50px 1fr 1fr 100px;
        gap:4px;
        width:600px;
        margin:0 auto;
        line-height: 35px;    
    }
    .todos span, #todos span {
        text-align: center;
        padding:0;
    }
    .todos span {
        padding:4px;
    }
    button {
        padding:4px 10px;
    }
    input {
        padding:4px 10px;
    }
</style>