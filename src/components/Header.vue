<template>
    <header>
        <h1>{{title}}</h1>
        <Button @btn-click="$emit('show-Add-Task')" :title="toggleAddTask?'close':'Add Task' " :color="toggleAddTask?'red':'#1a3af5'"/>
    </header>
</template>

<script>
import Button from './Button.vue'
export default {
    name:"Header",
    props:{
        title: String,
        toggleAddTask: Boolean
    },
    components:{
        Button
    }

}
</script>

<style>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>