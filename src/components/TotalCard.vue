<script setup>

const props = defineProps({
    wallet: Object,
    data: Object
})

const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
})

</script>

<template>
    <div class="total-container">
        <div class="total-card">
            <div>
                <h2>Number of Assets:</h2>
                <h1>{{ wallet.length }}</h1>
            </div>
            <div class="bottom-container">
                <h2>Total in Assets:</h2>
                <h1>{{ formatter.format(this.wallet.reduce((s, i) => s + (i.balance * this.data.find(j => j.name
                        === i.name).quote.USD.price), 0))
                }}</h1>
            </div>

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
    font-size: 26px;
    color: #555;
}

.total-card h1 {
    font-size: 40px;
    color: #111;
}

.total-card h1:first-child {
    margin-bottom: 20px;
}

.bottom-container {
    margin-top: 20px;
}
</style>