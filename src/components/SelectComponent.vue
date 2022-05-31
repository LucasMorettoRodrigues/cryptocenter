<script setup>

const props = defineProps({
    label: String,
    modelValue: {
        type: [String, Number],
        default: ''
    },
    options: Array
})

</script>

<template>
    <div class="flex">
        <label :for="label">{{ label }}</label>
        <select v-bind="$attrs" :id="label" :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)">
            <option v-for="(item, index) in options" :key="index">{{ item }}</option>
        </select>
    </div>
</template>

<style scoped>
div {
    flex-direction: column;
}

select {
    padding: 10px;
    background-color: white;
    border: 1px solid rgb(216, 216, 216);
    outline-color: lightblue;
    margin-bottom: 10px;
}
</style>