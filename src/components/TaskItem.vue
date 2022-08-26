<template>
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : ' ' , 'task']">
        <h3>{{ task.text }}
            <b @click="onDelete(task.id)">x</b>
        </h3>
        <h5>reminder : {{ task.reminder }}</h5>
    </div>
</template>

<script>
export default {
    name: "TaskItem",
    props: {
        task: Object
    },
    methods: {
        onDelete(id){
            this.$emit("delete-task", id)
        }
    },
    
}
</script>

<style scoped>
    .task{
        padding: 2% 4%;
        margin-bottom: 3%;
        background: #f3f3f3;
        cursor: pointer;
        transition: .4s all ease-in-out;
    }
    .task:active{
        transform: scale(0.98);
    }
    .task:hover{
        background: #d8d5d5;
    }
    .reminder{
        border-left: 5px solid #1a3af5;
    }
    h3{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    h3 > b{
        color: red;
        cursor: pointer;
    }
</style>