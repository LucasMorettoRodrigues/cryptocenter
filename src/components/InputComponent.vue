<script setup>

const props = defineProps({
    label: String,
    type: String,
    value: String,
})

</script>

<template>
    <div class="flex">
        <label :for="label">{{ label }}</label>
        <input step="any" :id="label" :type="type" @change="$emit('customChange', $event.target.value)" :value="value">
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