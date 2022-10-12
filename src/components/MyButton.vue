<script setup>
import { computed } from "vue";

const props = defineProps({
    onClick: {
        type: Function,
        required: true,
    },
    title: {
        type: String,
        required: true,
    },
    color: {
        type: String,
        default: 'blue',
        validator: (value) => {
            return ['blue', 'red', 'green'].includes(value);
        },
    },
    size: {
        type: String,
        default: 'medium',
    },
    rounded: {
        type: Boolean,
        default: false,
    },
});

function handleClick($event){
    console.log('test' + $event);
    props.onClick();
}

const buttonStyle = computed(() => {
    return {
        backgroundColor: props.color,
        borderRadius: props.rounded ? '50%' : '0',
    }
});

</script>

<template>
    <button :style="buttonStyle" @click="handleClick">
        {{ title }}
    </button>
</template>