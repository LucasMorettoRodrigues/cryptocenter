<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
    <header>
        <div class="main-container flex align-center space-between">
            <div>
                <h1 class="logo">
                    <RouterLink to="/">CryptoCenter</RouterLink>
                </h1>
            </div>
            <nav>
                <ul class="flex align-items-center">
                    <li>
                        <RouterLink to="/">Home</RouterLink>
                    </li>
                    <li>
                        <RouterLink to="/wallet">Wallet</RouterLink>
                    </li>
                </ul>
            </nav>
        </div>
    </header>
</template>

<style scoped>
header {
    background-color: black;
    padding: 10px;
}

a {
    padding: 10px;
    color: white;
}

li {
    margin-left: 10px;
}

.logo {
    color: white;
}
</style>
