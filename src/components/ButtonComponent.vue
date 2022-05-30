<script setup>

const props = defineProps({
    text: String
})

</script>

<template>
    <div class="flex align-center">
        <input type="submit" :value="text">
    </div>
</template>

<style scoped>
div {
    justify-content: flex-end;
}

input {
    padding: 10px 20px;
    background-color: rgb(46, 167, 46);
    border: none;
    text-align: right;
    font-weight: bold;
    color: white;
    cursor: pointer;
    border-radius: 5px;
    opacity: 0.9;
}

input:hover {
    opacity: 1;
}
</style>