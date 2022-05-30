<script setup>

const props = defineProps({
    label: String,
    modelValue: {
        type: [String, Number],
        default: ''
    },
})

</script>

<template>
    <div class="flex">
        <label :for="label">{{ label }}</label>
        <input v-bind="$attrs" :id="label" :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)" />
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