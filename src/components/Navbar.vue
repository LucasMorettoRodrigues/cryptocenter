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
                        <RouterLink to="/">Market</RouterLink>
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
    margin: 0 10px;
    font-size: 16px;
    color: white;
    border-bottom: 1px solid transparent;
    opacity: 0.9;
    transition: all ease-in-out .3s
}

a:hover,
a.router-link-active {
    border-color: white;
    opacity: 1;
}

li {
    margin-left: 10px;
}

.logo a {
    color: white;
    font-size: 30px;
    border-bottom: none;
    opacity: 1;
}
</style>
