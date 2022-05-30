<script setup>

const props = defineProps({
    title: String
})

</script>

<template>
    <div class="modal-background flex align-center">
        <div class="modal">
            <div class="top flex align-center">
                <div @click="$emit('close')" class="close-btn-container flex align-center justify-center">
                    <span class="close-btn">X</span>
                </div>
            </div>
            <h1 class="title text-align-center">{{ title }}</h1>
            <div class="wrapper">
                <slot />
            </div>
        </div>
    </div>
</template>

<style scoped>
.modal-background {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 100;
    background-color: rgba(0, 0, 0, 0.705);
    justify-content: center;
}

.modal {
    position: relative;
    width: 400px;
    background-color: white;
}

.top {
    justify-content: flex-end;
}

.wrapper {
    padding: 20px;
    padding-top: 0;
}

.close-btn {
    color: rgb(190, 190, 190);
    cursor: pointer;
    font-size: 18px;
    margin-top: 5px;
    margin-right: 10px;
}

.close-btn:hover {
    color: black;
}

.title {
    margin-bottom: 10px;
}
</style>