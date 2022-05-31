<script setup>

const props = defineProps({
    text: String,
})

</script>

<template>
    <div class="total-container">
        <div class="total-card">
            <h2>Total in Assets:</h2>
            <h1>{{ text }}</h1>
        </div>
    </div>
</template>

<style scoped>
.total-container {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
}

.total-card {
    background-color: rgb(233, 233, 233);
    padding: 50px 80px;
    border-radius: 5px;
}

.total-card h2 {
    margin-bottom: 20px;
    font-size: 26px;
    color: #555;
}

.total-card h1 {
    font-size: 40px;
    color: #111;
}
</style>