<template>
<div>
    <span> {{ todo.text }}</span>
    <button @click="$emit('deleteTodo', todo.id)">Sil</button>
</div>
</template>

<script>
export default {
    props: ['todo']
}
</script>
