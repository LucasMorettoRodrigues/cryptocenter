<script setup>

const props = defineProps({
    name: String
})

</script>

<template>
    <div class="flex">
        <label :for="name">{{ name }}</label>
        <input :id="name" :type="type" @change="$emit('customChange', $event.target.value)">
    </div>
</template>

<style scoped>
div {
    flex-direction: column;
}

input {
    padding: 10px;
    border: 1px solid rgb(216, 216, 216);
    outline-color: lightblue;
    margin-bottom: 10px;
}
</style>