<script>
export default {
    props: ['title', 'open']
}
</script>

<template>
    <transition name="back">
        <div v-if="open" class="modal-background" @click="$emit('close')"></div>
    </transition>
    <transition name="modal">
        <div v-if="open" class="modal">
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
    </transition>
</template>

<style scoped>
.modal-background {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 10;
    background-color: rgba(0, 0, 0, 0.705);
    justify-content: center;
}

.modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
    width: 400px;
    background-color: white;
    z-index: 10;
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

.back-enter-active {
    animation: back .5s ease-out;
}

.back-leave-active {
    animation: back .5s ease-in reverse;
}

@keyframes back {
    0% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}

.modal-enter-active {
    animation: modal .5s ease-in-out;
}

.modal-leave-active {
    animation: modal .5s ease-in-out reverse;
}

@keyframes modal {
    0% {
        transform: translateX(150%) translateY(-50%) scale(0.5);
        opacity: 0;
    }

    100% {
        transform: translateX(-50%) translateY(-50%) scale(1);
        opacity: 1;
    }
}
</style>