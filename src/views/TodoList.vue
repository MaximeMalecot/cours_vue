<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import TodoItem from "../components/TodoItem.vue";
const newTask = ref('');
const tasks = ref([]);
const showAll = ref(false);
const handleCreate = async () => {
    if(newTask.value){
        tasks.value.push({
            id: tasks.value.length + 1,
            title: newTask.value,
            completed: false,
        });
        await fetch(
            'http://localhost:3000/tasks',
            {
                method: 'POST',
                body: JSON.stringify({
                    title: newTask.value,
                    completed: false,
                }),
                headers: {
                    'Content-type': 'application/json; charset=UTF-8',
                },
            }
        );
        newTask.value = '';
    }
}

onMounted(async ()=>{
    const apiTasks = await (await fetch("http://localhost:3000/tasks")).json();
    tasks.value = apiTasks;
})

const handleDelete = async (task) => {
    const index = tasks.value.indexOf(task);
    await fetch(
            `http://localhost:3000/tasks/${task.id}`,
            {
                method: 'DELETE',
            }
        );
    tasks.value.splice(index, 1);
}

const handleUpdate = async (task) => {
    const res = await fetch(
        `http://localhost:3000/tasks/${task.id}`,
        {
            method: 'PUT',
            body: JSON.stringify(task),
            headers: {
                'Content-type': 'application/json; charset=UTF-8',
            },
        }
    );
    if(res.status === 200){
        const index = tasks.value.findIndex(t => t.id === task.id);
        tasks.value[index] = task;
    }
}
</script>

<template>
    <div>
        <h1>Todo List</h1>
        <button @click.prevent="() => {showAll = !showAll}">Show all</button>
        <form @submit.prevent="handleCreate">
            <input type="text" name="task" v-model="newTask" />
            <button type="submit">Add</button>
        </form>
        <ul>
            <template v-for="task in tasks" >
                <TodoItem v-show="showAll || !task.completed" bind:key="task.id" :task="task" :handleUpdate="handleUpdate" :handleDelete="handleDelete"/>
            </template>
        </ul>
    </div>
</template>