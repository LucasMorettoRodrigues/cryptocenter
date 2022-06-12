<template>
    <div class="wrapper">
        <div class="flex align-center space-between header">
            <h3>Top Gainers</h3>
            <RouterLink to="/">More</RouterLink>
        </div>
        <div>
            <ul>
                <li>
                    <div>1</div>
                    <div class="name">Name</div>
                    <div>15%</div>
                </li>
                <li>
                    <div>2</div>
                    <div class="name">Name</div>
                    <div>15%</div>
                </li>
                <li>
                    <div>3</div>
                    <div class="name">Name</div>
                    <div>15%</div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { RouterLink } from 'vue-router'
</script>

<style scoped>
.wrapper {
    background-color: rgb(78, 78, 255);
    padding: 20px;
    border-radius: 10px;
    color: white;
    width: 100%;
}

.header {
    margin-bottom: 10px;
}

.header h3 {
    font-weight: bold;
}

a {
    padding: 4px 8px;
    background-color: white;
    border-radius: 10px;
    font-size: 12px;
    text-decoration: none;
}

ul {
    padding: 0;
}

ul li {
    display: flex;
}

ul li .name {
    flex: 1;
    margin-left: 15px;
}
</style>