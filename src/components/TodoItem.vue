<script setup>
import { ref, inject } from 'vue';
const props = defineProps({
    task: {
        type: Object,
        required: true,
    },
    handleDelete: {
        type: Function,
        required: true,
    },
    handleUpdate: {
        type: Function,
        required: true,
    },
});
const title = ref(props.task.title);
</script>

<template>
    <li>
        <input class="deleteBtn" type="button" value="X" @click="handleDelete(task)"/>
        <input type="text" v-model="title" @keypress.enter="handleUpdate({...task, title})"/>
        <input type="checkbox" :checked="task.completed" @click="handleUpdate({...task, completed: !task.completed})"/>
    </li>
</template>

<style>
    .deleteBtn{
        background-color: red;
        color: black;
        border-radius: 10px;
    }
</style>