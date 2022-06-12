<template>
    <div class="wrapper">
        <div class="flex align-center space-between header">
            <h3>{{ title }}</h3>
            <RouterLink to="/">More</RouterLink>
        </div>
        <div>
            <ul>
                <li v-for="(item, index) in list">
                    <div>{{ index + 1 }}</div>
                    <div class="name">{{ item.name }}</div>
                    <div :class="item.change < 0 ? 'red' : 'green'">{{ item.change }} %</div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { RouterLink } from 'vue-router'

export default {
    props: ['title', 'list']
}
</script>

<style scoped>
.wrapper {
    background-color: rgb(97, 97, 255);
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
    color: rgb(97, 97, 255);
    font-weight: 500;
    border-radius: 10px;
    font-size: 12px;
    text-decoration: none;
}

ul {
    padding: 0;
}

ul li {
    display: flex;
    margin-bottom: 4px;
}

ul li:last-child {
    margin-bottom: 0;
}

ul li .name {
    flex: 1;
    margin-left: 15px;
}

.red,
.green {
    background-color: white;
    border-radius: 5px;
    padding: 0 5px;
}

.green {
    color: green;
}

.red {
    color: red;
}
</style>